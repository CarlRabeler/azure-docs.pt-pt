   <properties
   pageTitle="Carga de datos en Almacenamiento de datos SQL | Microsoft Azure"
   description="Conozca los escenarios comunes para la carga de datos en Almacenamiento de datos SQL"
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="lodipalm"
   manager="barbkess"
   editor="jrowlandjones"/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="06/21/2015"
   ms.author="lodipalm;barbkess"/>

# Carga de datos en Almacenamiento de datos SQL
El Almacenamiento de datos SQL ofrece numerosas opciones para la carga de datos entre las que se incluyen:

- Factoría de datos de Azure
- Utilidad de línea de comandos BCP
- PolyBase
- SQL Server Integration Services (SSIS)
- Herramientas de carga de datos de terceros

Aunque todos los métodos anteriores se pueden usar con el Almacenamiento de datos SQL. Muchos de nuestros usuarios están mirando las cargas iniciales en los cientos de gigabytes hasta las decenas de terabytes. En las secciones siguientes, se ofrece orientación sobre la carga de datos inicial.

## Carga inicial en el Almacenamiento de datos SQL desde SQL Server 
Al cargar en el Almacenamiento de datos SQL desde una instancia de SQL Server local, se recomiendan los siguientes pasos:

1. **BCP** los datos de SQL Server a archivos planos 
2. Usar **AZCopy** o **Importación/Exportación** (para conjuntos de datos grandes) para mover los archivos a Azure
3. Configurar PolyBase para leer sus archivos desde la cuenta de almacenamiento
4. Crear nuevas tablas y cargar datos con **PolyBase**

En las secciones siguientes echaremos un vistazo a cada paso en profundidad y daremos ejemplos del proceso.

> [AZURE.NOTE]Antes de mover datos desde un sistema, como SQL Server, sugerimos revisar los artículos [Migrar esquema][] y [Migrar código][] de nuestra documentación.

## Exportación de archivos con BCP

Para preparar los archivos para el traslado a Azure, necesitará exportarlos a archivos planos. Esto se realiza mejor con la utilidad de la línea de comandos de BCP. Si no dispone aún de la utilidad, se puede descargar con las [Utilidades de la línea de comandos de Microsoft para SQL Server][]. Un comando BCP de ejemplo podría ser similar al siguiente:

```
bcp "<Directory><File>" -c -T -S <Server Name> -d <Database Name>
```

Este comando tomará los resultados de una consulta y los exportará a un archivo en el directorio que elija. Puede colocar el proceso en paralelo ejecutando varios comandos BCP para tablas independientes a la vez. Esto le permitirá ejecutar un proceso de BCP por núcleo de su servidor; le aconsejamos que pruebe algunas operaciones más pequeñas en configuraciones diferentes para ver qué funciona mejor en su entorno.

Además, conforme realizaremos la carga mediante PolyBase, tenga en cuenta que PolyBase no admite todavía con UTF-16, y todos los archivos deben estar en UTF-8. Esto se puede lograr con facilidad mediante la inclusión de la marca '-c' en su comando BCP o también puede convertir los archivos */*planos de UTF-16 en UTF-8 mediante el siguiente código:

```
Get-Content <input_file_name> -Encoding Unicode | Set-Content <output_file_name> -Encoding utf8
```
 
Cuando haya exportado correctamente los datos a los archivos, será el momento de moverlos a Azure. Esto se puede lograr con AZCopy o con el servicio de importación y exportación, tal como se describe en la siguiente sección.

## Carga en Azure con AZCopy o importación y exportación
Si va a mover datos en el intervalo de 5 a 10 terabytes o superior, recomendamos que use nuestro servicio de envío de disco[Importación/Exportación][] para lograr el traslado. Sin embargo, en nuestro estudios, hemos logrados mover datos en el rango de TB de dígito único cómodamente mediante Internet pública con AZCopy. Este proceso también puede acelerarse o ampliarse con ExpressRoute.

En los siguientes pasos se detalla cómo mover datos locales desde local a una cuenta de Almacenamiento de Azure mediante AZCopy. Si no dispone de una cuenta de Almacenamiento de Azure en la misma región puede crear una siguiendo la [Documentación del Almacenamiento de Azure][]. También puede cargar datos desde una cuenta de almacenamiento en una región distinta, pero el rendimiento en este caso no será óptimo.

> [AZURE.NOTE]Esta documentación supone que ha instalado la utilidad de la línea de comandos de AZCopy y que puede ejecutarla con Powershell. Si este no es el caso, siga las [instrucciones de instalación de AZCopy][].

Ahora, dado un conjunto de archivos que ha creado con BCP, AzCopy puede ejecutarse simplemente Azure Powershell o ejecutando un script Powershell. En un nivel alto, el símbolo del sistema necesario para ejecutar AZCopy adoptará la forma:

```
AZCopy /Source:<File Location> /Dest:<Storage Container Location> /destkey:<Storage Key> /Pattern:<File Name> /NC:256
```

Además de la básica, recomendamos las siguientes prácticas recomendadas para la carga con AZCopy:


+ **Conexiones simultáneas**: además de aumentar el número de operaciones de AZCopy que se ejecutan al mismo tiempo, la propia operación de AZCopy se puede poner aún más en paralelo estableciendo el parámetro/NC, que abre varias conexiones simultáneas al destino. Mientras el parámetro puede ser en 512 como máximo, descubrimos que la transferencia de datos óptima tuvo lugar en 256 y recomendamos que se prueba un número de valores para encontrar lo que es óptimo para su configuración.

+ **ExpressRoute**: como se indicó anteriormente, este proceso se puede acelerar si la ruta rápida está habilitada. Encontrará información general de ExpressRoute y pasos para configurar en la [Documentación de ExpressRoute][].

+ **Estructura de carpetas**: para facilitar la transferencia con PolyBase, asegúrese de que cada tabla se asigna a su propia carpeta. Esto minimizará y simplificará sus pasos al cargar con PolyBase más adelante. Dicho eso, no hay ningún impacto si una tabla se divide en varios archivos o incluso subdirectorios dentro de la carpeta.
	 

## Configuración de PolyBase 

Ahora que los datos se encuentran en blobs de almacenamiento de Azure, los importaremos en la instancia del Almacenamiento de datos SQL mediante PolyBase. Los pasos indicados a continuación son solo para configuración de y muchos de ellos solo tendrán que completarse una vez por cada cuenta de instancia del Almacenamiento de datos SQL, usuario o cuenta de almacenamiento. Estos pasos también se han descrito con mayor detalle en nuestra documentación [Cargar con PolyBase][].

1. **Creación de clave maestra de base de datos.** Esta operación solo tendrá que completarse una vez por cada base de datos. 

2. **Creación de una credencial con ámbito de base de datos.** Esta operación solo debe crearse si desea crear una nueva credencial/usuario; de lo contrario, se puede usar una credencial creada anteriormente.

3. **Creación de un formato de archivo externo.** Además, los formatos de archivos externos también son reutilizables; solo necesitará crear uno si está cargando un nuevo tipo de archivo.

4. **Creación de un origen de datos externo.** Cuando se señala a una cuenta de almacenamiento, se puede usar un origen de datos externo al cargar desde el mismo contenedor. Para su parámetro 'LOCATION', use una ubicación con el formato: 'wasbs://mycontainer@ test.blob.core.windows.net/path’.

```
-- Creating master key
CREATE MASTER KEY;

-- Creating a database scoped credential
CREATE DATABASE SCOPED CREDENTIAL <Credential Name> 
WITH 
    IDENTITY = '<User Name>'
,   Secret = '<Azure Storage Key>'
;

-- Creating external file format (delimited text file)
CREATE EXTERNAL FILE FORMAT text_file_format 
WITH 
(
    FORMAT_TYPE = DELIMITEDTEXT 
,   FORMAT_OPTIONS  (
                        FIELD_TERMINATOR ='|' 
                    ,   USE_TYPE_DEFAULT = TRUE
                    )
);

--Creating an external data source
CREATE EXTERNAL DATA SOURCE azure_storage 
WITH 
(
    TYPE = HADOOP 
,   LOCATION ='wasbs://<Container>@<Blob Path>'
,   CREDENTIAL = <Credential Name>
)
;
```

Ahora que su cuenta de almacenamiento está correctamente configurada, puede continuar cargando sus datos en el Almacenamiento de datos SQL.

## Carga de datos con PolyBase 
Después de configurar PolyBase, puede cargar datos directamente en el Almacenamiento de datos SQL, creando simplemente una tabla externa que apunta a los datos del almacenamiento y asignando luego esos datos a una nueva tabla en el Almacenamiento de datos SQL. Esto se puede lograr con los dos comandos sencillos a continuación.

1. Use el comando 'CREATE EXTERNAL TABLE' para definir la estructura de sus datos. Para asegurarse de que captura el estado de los datos de forma rápida y eficaz, se recomienda generar script de la tabla de SQL Server en SSMS y ajustar luego manualmente para representar las diferencias de la tabla externa. Después de crear una tabla externa en Azure continuará señalando la misma ubicación, incluso si los datos se actualizan o se agregan datos adicionales.  

```
-- Creating external table pointing to file stored in Azure Storage
CREATE EXTERNAL TABLE <External Table Name> 
(
    <Column name>, <Column type>, <NULL/NOT NULL>
)
WITH 
(   LOCATION='<Folder Path>'
,   DATA_SOURCE = <Data Source>
,   FILE_FORMAT = <File Format>      
);
```

2. Cargar datos con una instrucción 'CREATE TABLE...AS SELECT'. 

```
CREATE TABLE <Table Name> 
WITH 
(
	CLUSTERED COLUMNSTORE INDEX
)
AS 
SELECT  * 
FROM    <External Table Name>
;
```

Tenga en cuenta que también puede cargar una subsección de las filas desde una tabla mediante una instrucción SELECT más detallada. Sin embargo, como PolyBase no inserta un proceso adicional a las cuentas de almacenamiento en este momento, si carga una subsección con una instrucción SELECT no será más rápido que cargar el conjunto de datos completo.

Además de la instrucción `CREATE TABLE...AS SELECT`, también puede cargar datos de tablas externas en tablas preexistentes con una instrucción 'INSERT...INTO'.

## Pasos siguientes
Para obtener más sugerencias sobre desarrollo, consulte la [información general sobre desarrollo][].

<!--Image references-->

<!--Article references-->
[Load data with bcp]: sql-data-warehouse-load-with-bcp.md
[Cargar con PolyBase]: sql-data-warehouse-load-with-polybase.md
[solution partners]: sql-data-warehouse-solution-partners.md
[información general sobre desarrollo]: sql-data-warehouse-overview-develop.md
[Migrar esquema]: sql-data-warehouse-migrate-schema.md
[Migrar código]: sql-data-warehouse-migrate-code.md

<!--MSDN references-->
[supported source/sink]: https://msdn.microsoft.com/library/dn894007.aspx
[copy activity]: https://msdn.microsoft.com/library/dn835035.aspx
[SQL Server destination adapter]: https://msdn.microsoft.com/library/ms141237.aspx
[SSIS]: https://msdn.microsoft.com/library/ms141026.aspx

<!--Other Web references-->
[instrucciones de instalación de AZCopy]: https://azure.microsoft.com/es-es/documentation/articles/storage-use-azcopy/
[Utilidades de la línea de comandos de Microsoft para SQL Server]: http://www.microsoft.com/es-es/download/details.aspx?id=36433
[Importación/Exportación]: https://azure.microsoft.com/es-es/documentation/articles/storage-import-export-service/
[Documentación del Almacenamiento de Azure]: https://azure.microsoft.com/es-es/documentation/articles/storage-create-storage-account/
[Documentación de ExpressRoute]: http://azure.microsoft.com/documentation/services/expressroute/

<!---HONumber=August15_HO8-->