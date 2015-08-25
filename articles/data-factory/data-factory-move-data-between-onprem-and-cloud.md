<properties 
	pageTitle="Movimiento de datos entre una ubicación local y la nube mediante Factoría de datos de Azure" 
	description="Obtenga información acerca de cómo mover datos entre local y la nube mediante Data Management Gateway y Factoría de datos de Azure." 
	services="data-factory" 
	documentationCenter="" 
	authors="spelluru" 
	manager="jhubbard" 
	editor="monicar"/>

<tags 
	ms.service="data-factory" 
	ms.workload="data-services" 
	ms.tgt_pltfrm="na" 
	ms.devlang="na" 
	ms.topic="article" 
	ms.date="07/29/2015" 
	ms.author="spelluru"/>

# Movimiento de datos entre orígenes locales y la nube con Data Management Gateway
Uno de los desafíos de la integración de datos moderna es mover datos sin problemas entre ubicación la local y la nube. Factoría de datos hace que esta integración con la puerta de enlace de administración de datos sea perfecta. La puerta de enlace de administración de la factoría de datos es un agente que puede instalar de forma local para permitir que las canalizaciones híbridas.

Este artículo proporciona información general de la integración de los almacenes de datos locales y los almacenes de datos en la nube mediante la factoría de datos. Este artículo se basa en el artículo [Actividades de movimiento de datos](data-factory-data-movement-activities.md) y otros artículos de conceptos básicos de Factoría de datos. La información general siguiente supone que está familiarizado con conceptos de la factoría de datos como canalizaciones, actividades, conjuntos de datos y actividad de copia.

La puerta de enlace de datos proporciona las siguientes capacidades:

1.	Modelo de orígenes de datos local y orígenes de datos en la nube en la misma factoría de datos y movimiento de datos.
2.	Tiene un solo panel de vidrio para la supervisión y administración con visibilidad del estado de la puerta de enlace mediante el panel basado en la nube de Factoría de datos.
3.	Administrar el acceso a orígenes de datos locales de forma segura.
	1. No es necesario ningún cambio en el firewall corporativo. La puerta de enlace solo hace conexiones salientes basadas en HTTP para abrir Internet.
	2. Cifrar las credenciales de los almacenes de datos locales con su certificado.
4.	Mover datos de manera eficaz: los datos se transfieren en paralelo, con resistencia a problemas de red intermitentes mediante una lógica automática de reintentos.

## Consideraciones para utilizar Data Management Gateway
1.	Una sola instancia de Data Management Gateway se puede usar para varios orígenes de datos locales, pero tenga en cuenta que **una sola puerta de enlace está asociada a una sola Factoría de datos de Azure** y no puede compartirse con otra factoría de datos.
2.	**Solo puede haber una instancia de Data Management Gateway** instalada en un solo equipo. Supongamos que tiene dos factorías de datos que necesitan tener acceso a orígenes de datos locales. Debe instalar puertas de enlace en dos equipos locales donde cada puerta de enlace esté asociada a una factoría de datos diferente.
3.	La **puerta de enlace no necesita estar en el mismo equipo que el origen de datos** pero, si está cerca del origen de datos, reduce el tiempo de conexión de la puerta de enlace al origen de datos. Le recomendamos que instale la puerta de enlace en un equipo diferente del que hospeda el origen de datos local para que la puerta de enlace no compita por los recursos con el origen de datos.
4.	Puede tener **varias puertas de enlace en diferentes equipos conectados al mismo origen de datos local**. Por ejemplo, puede que tenga dos puertas de enlace que atienden a dos factorías de datos pero el mismo origen de datos local está registrado con las dos factorías de datos.
5.	Si ya tiene una puerta de enlace instalada en el equipo que atiende a un escenario de **Power BI** , instale una **puerta de enlace independiente para Factoría de datos de Azure** en otro equipo.
6.	Debe **usar la puerta de enlace, incluso cuando use ExpressRoute**. 
7.	Debe tratar el origen de datos como un origen de datos local (que está detrás de un firewall) incluso si usa **ExpressRoute** y **usar la puerta de enlace** para establecer la conectividad entre el servicio y el origen de datos. 

## Instalación de la puerta de enlace: requisitos previos
1.	Las versiones de **sistema operativo** compatibles son Windows 7, Windows 8/8.1, Windows Server 2008 R2 y Windows Server 2012.
2.	La **configuración** recomendada del equipo de la puerta de enlace es de al menos 2 GHz, 4 núcleos, 8 GB de RAM y disco de 80 GB.
3.	Si el equipo host está en hibernación, la puerta de enlace no podrá responder a las solicitudes de datos. Por tanto, configure un **plan de energía** adecuado en el equipo antes de instalar la puerta de enlace. La instalación de la puerta de enlace emite un mensaje si el equipo está configurado para la hibernación.

Puesto que las ejecuciones de la actividad de copia suceden con una frecuencia determinada, el uso de recursos (CPU, memoria) en el equipo también sigue el mismo patrón con las horas pico y los tiempos de inactividad. El uso de recursos también depende en gran medida de la cantidad de datos que se mueven. Cuando hay varios trabajos de copia en curso, observará que el uso de los recursos aumenta durante las horas pico. Aunque arriba se muestra la configuración mínima, siempre es mejor tener una configuración con más recursos que la configuración mínima descrita, en función de la carga específica para el movimiento de datos.

## Instalación
Data Management Gateway puede instalarse mediante la descarga de un paquete de instalación MSI del Centro de descarga de Microsoft. El MSI también puede usarse para actualizar el Data Management Gateway existente a la versión más reciente, con toda la configuración conservada. Para encontrar el vínculo al paquete MSI en el Portal de Azure, siga el tutorial paso a paso que aparece a continuación.

### Procedimientos recomendados de instalación:
1.	Configurar el plan de energía en el equipo host para la puerta de enlace, de forma que el equipo no hiberne. Si el equipo host está en hibernación, la puerta de enlace no podrá responder a las solicitudes de datos.
2.	Se debe hacer una copia de seguridad del certificado asociado a la puerta de enlace.

### Solución de problemas de instalación
Si su compañía usa un firewall o un servidor proxy, pueden ser necesarios pasos adicionales en caso de que Data Management Gateway no pueda conectarse a los servicios en la nube de Microsoft.

#### Consulta de los registros de la puerta de enlace con el Visor de eventos:

La aplicación de administrador de configuración de Gateway muestra el estado de la puerta de enlace como "Desconectado" o "Conectando".

Para obtener más información puede consultar los registros de la puerta de enlace en los registros de eventos de Windows. Puede encontrarlos mediante el **Visor de eventos** de Windows en **Registros de aplicaciones y servicios** > **Data Management Gateway**. Cuando soluciones problemas relacionados con la puerta de enlace consulte los eventos de error en el Visor de eventos.


#### Posibles síntomas de problemas relacionados con el firewall:

1. Al intentar registrar la puerta de enlace, recibirá el siguiente error: "Error al registrar la clave de la puerta de enlace. Antes de volver a intentar registrar la clave de la puerta de enlace, confirme que Data Management Gateway está en estado conectado y el servicio host de Data Management Gateway se ha iniciado."
2. Al abrir el Administrador de configuración, verá el estado como "Desconectado" o "Conectando". Al ver los registros de eventos de Windows, bajo "Visor de eventos" > "Registros de aplicaciones y servicios" > "Data Management Gateway" aparecen mensajes de error como " No es posible conectar con el servidor remoto " o "Un componente de Data Management Gateway ha dejado de responder y se reiniciará automáticamente. Nombre del componente: puerta de enlace."

Esto se debe a una configuración incorrecta del servidor proxy o el firewall, que impide a Data Management Gateway conectarse a servicios en la nube para autenticarse.

Los dos firewalls de los que posiblemente se trate son: firewall corporativo que se ejecuta en el enrutador central de la organización y Firewall de Windows configurado como un demonio en el equipo local donde está instalada la puerta de enlace. Estas son algunas consideraciones:

- No es necesario cambiar la directiva de entrada para el firewall corporativo.
- El firewall corporativo y Firewall de Windows deben habilitar la regla de salida para los puertos TCP: 80, 440 y de 9305 a 9354. Bus de servicio de Microsoft Azure los usa para establecer la conexión entre los servicios en la nube y Data Management Gateway.

El programa de instalación MSI configurará automáticamente las reglas de Firewall de Windows para los puertos de entrada del equipo de puerta de enlace (consulte los puertos y la sección de consideraciones de seguridad anterior).

Pero el programa de instalación considera que los puertos de salida mencionados anteriormente se permiten de forma predeterminada en el equipo local y el firewall corporativo. De no ser así, necesitará habilitar estos puertos de salida. Si el Firewall de Windows se ha reemplazado por un firewall de otros fabricantes, podría ser necesario abrir manualmente estos puertos.

Si su compañía usa un servidor proxy, necesitará agregar Microsoft Azure a la lista blanca. Puede descargar una lista de direcciones IP de Microsoft Azure válidas del [ Centro de descarga de Microsoft](http://msdn.microsoft.com/library/windowsazure/dn175718.aspx).

## Mediante la puerta de enlace de datos: tutorial paso a paso
En este tutorial se crea una factoría de datos con una canalización que mueve los datos de una base de datos de SQL Server local a un blob de Azure.

### Paso 1: Creación de una factoría de datos de Azure
En este paso, use el Portal de administración de Azure para crear una instancia de Factoría de datos de Azure denominada **ADFTutorialOnPremDF**. También puede crear una factoría de datos mediante cmdlets de la Factoría de datos de Azure.

1.	Tras iniciar sesión en el [Portal de vista previa de Azure](https://portal.azure.com), haga clic en **NUEVO** en la esquina inferior izquierda, seleccione **Análisis de datos** en la hoja **Crear** y haga clic en **Factoría de datos** en la hoja **Análisis de datos**.

	![New->DataFactory](./media/data-factory-move-data-between-onprem-and-cloud/NewDataFactoryMenu.png)
  
6. En la hoja **Nueva factoría de datos**:
	1. Escriba **ADFTutorialOnPremDF** en el **nombre**.
	2. Haga clic en **NOMBRE DEL GRUPO DE RECURSOS** y seleccione **ADFTutorialResourceGroup**. Puede seleccionar un grupo de recursos existente o crear uno nuevo. Para crear un nuevo grupo de recursos:
		1. Haga clic en **Crear un nuevo grupo de recursos**.
		2. En la hoja **Crear grupo de recursos**, escriba un **nombre** para el grupo de recursos y haga clic en **Aceptar**.

7. Observe que está seleccionada la opción **Agregar al Panel de inicio** en la hoja **Nueva factoría de datos**.

	![Agregar al Panel de inicio](./media/data-factory-move-data-between-onprem-and-cloud/OnPremNewDataFactoryAddToStartboard.png)

8. En la hoja **Nueva factoría de datos**, haga clic en **Crear**.

	El nombre del generador de datos de Azure debe ser único global. Si recibe el error: **El nombre de la factoría de datos "ADFTutorialOnPremDF" no está disponible**, cambie el nombre de la factoría de datos (por ejemplo, yournameADFTutorialDataFactory) e intente crearla de nuevo. Use este nombre en lugar de ADFTutorialFactory al realizar los restantes pasos de este tutorial.

9. Busque las notificaciones del proceso de creación en el centro de **NOTIFICACIONES** situado a la izquierda. Haga clic en **X** para cerrar la hoja **NOTIFICACIONES** si está abierta.

	![Centro de NOTIFICACIONES](./media/data-factory-move-data-between-onprem-and-cloud/OnPremNotificationsHub.png)

11. Una vez completada la creación, verá la hoja **Factoría de datos** como se muestra a continuación:

	![Página principal de Factoría de datos](./media/data-factory-move-data-between-onprem-and-cloud/OnPremDataFactoryHomePage.png)

### Paso 2: Creación de una puerta de enlace de administración de datos
5.	En la hoja **Factoría de datos** de **ADFTutorialOnPremDF**, haga clic en **Servicios vinculados**. 

	![Página principal de Factoría de datos](./media/data-factory-move-data-between-onprem-and-cloud/DataFactoryHomePage.png)

2.	En la hoja **Servicios vinculados**, haga clic en **+ Puerta de enlace de datos**.

	![Servicios vinculados: botón Agregar una puerta de enlace](./media/data-factory-move-data-between-onprem-and-cloud/OnPremLinkedServicesAddGaewayButton.png)

2. En la hoja **Crear**, escriba **adftutorialgateway** para el **nombre** y haga clic en **Aceptar**.

	![Hoja Crear puerta de enlace](./media/data-factory-move-data-between-onprem-and-cloud/OnPremCreateGatewayBlade.png)

3. En la hoja **Configurar**, haga clic en **Instalar directamente en este equipo**. Esto descarga el paquete de instalación de la puerta de enlace, instala, configura y registra la puerta de enlace en el equipo.

	> [AZURE.NOTE]Utilice Internet Explorer o un explorador web compatible con Microsoft ClickOnce.

	![Puerta de enlace: hoja Configurar](./media/data-factory-move-data-between-onprem-and-cloud/OnPremGatewayConfigureBlade.png)

	Esta es la forma más sencilla (un clic) para descargar, instalar, configurar y registrar la puerta de enlace en un solo paso. Puede ver que la aplicación **Administrador de configuración de Microsoft Data Management Gateway** está instalada en el equipo. También puede encontrar el archivo ejecutable **ConfigManager.exe** en la carpeta: **C:\\Program Files\\Microsoft Data Management Gateway\\1.0\\Shared**.

	También puede descargar e instalar la puerta de enlace manualmente con los vínculos de esta hoja y registrarla usando la clave que se muestra en el cuadro de texto **REGISTRAR CON LA CLAVE**.
	
	Consulte la sección [Data Management Gateway](#DMG) para obtener información detallada de la puerta de enlace, incluidos procedimientos recomendados y algunas consideraciones importantes.

	>[AZURE.NOTE]Debe ser administrador del equipo local para instalar y configurar correctamente Data Management Gateway. Puede agregar usuarios adicionales al grupo local de Windows Usuarios de usuarios de Data Management Gateway. Los miembros de este grupo podrán usar la herramienta Administrador de configuración de Data Management Gateway para configurar la puerta de enlace.

4. Haga clic en el centro de **NOTIFICACIONES** en la parte izquierda. Espere hasta ver el mensaje **La configuración rápida de ''adftutorialgateway'' se realizó correctamente** en la hoja **Notificaciones**.

	![La configuración rápida se realizó correctamente](./media/data-factory-move-data-between-onprem-and-cloud/express-setup-succeeded.png)
6. Haga clic en **Aceptar** en la hoja **Crear** y, a continuación, en la hoja **Nueva puerta de enlace de datos**.
6. Cierre la hoja **Servicios vinculados** (presionando el botón **X** situado en la esquina superior derecha) y vuelva a abrir la hoja **Servicios vinculados** para ver el estado más reciente de la puerta de enlace. 
7. Confirme que el **estado** de la puerta de enlace es **En línea**. 

	![Estado de la puerta de enlace](./media/data-factory-move-data-between-onprem-and-cloud/gateway-status.png)

5. Inicie la aplicación **Administración de configuración de Microsoft Data Management Gateway** en el equipo.

	![Administrador de configuración de puertas de enlace](./media/data-factory-move-data-between-onprem-and-cloud/OnPremDMGConfigurationManager.png)

6. Espere hasta que los valores se hayan establecido como se indica a continuación:
	1. Si el **estado** del servicio no es **Iniciado**, haga clic en **Iniciar servicio** para iniciarlo y espere un minuto para que se actualicen los demás campos.
	2. **Nombre de la puerta de enlace** está establecido en **adftutorialgateway**.
	3. **Nombre de instancia** está establecido en **adftutorialgateway**.
	4. **Estado de la clave de la puerta de enlace** está establecido en **Registrada**.
	5. La barra de estado de la parte inferior muestra **Conectado al servicio en la nube Data Management Gateway** junto con una **marca de verificación verde**.
	
7. Cambie a **Certificados**. El certificado especificado en esta pestaña se usa para cifrar y descifrar las credenciales para el almacén de datos local que especifique en el portal. Haga clic en **Cambiar** usar su propio certificado. De forma predeterminada, la puerta de enlace usa el certificado generado automáticamente por el servicio Factoría de datos.

	![Configuración del certificado de puerta de enlace](./media/data-factory-move-data-between-onprem-and-cloud/gateway-certificate.png)

8. En el portal, en la hoja **Servicios vinculados**, confirme que el **estado** de la puerta de enlace es **Bueno**.


### Paso 2: Creación de servicios vinculados 
En este paso, creará dos servicios vinculados: **StorageLinkedService** y **SqlServerLinkedService**. El servicio **SqlServerLinkedService** vincula una base de datos de SQL Server local y el servicio vinculado **StorageLinkedService** vincula un almacén de blobs de Azure a **ADFTutorialDataFactory**. Más adelante en este tutorial creará una canalización que copia datos de la base de datos de SQL Server local al almacén de blobs de Azure.

#### Adición de un servicio vinculado a una base de datos de SQL Server local
1.	En la hoja **Servicios vinculados**, haga clic en **Nuevo almacén de datos** en la barra de comandos.
2.	Escriba **SqlServerLinkedService** para el **Nombre**. 
2.	Haga clic en flecha junto al **Tipo** y seleccione **SQL Server**.

	![Creación de un nuevo almacén de datos](./media/data-factory-move-data-between-onprem-and-cloud/new-data-store.png)
3.	Debe definir más opciones bajo **Tipo**.
4.	Para la configuración **Puerta de enlace de datos**, seleccione la puerta de enlace que acaba de crear. 

	![Configuración de SQL Server](./media/data-factory-move-data-between-onprem-and-cloud/sql-server-settings.png)
4.	Escriba el nombre de su servidor de base de datos para la configuración **Servidor**.
5.	Escriba el nombre de la base de datos para la configuración **Base de datos**.
6.	Haga clic en flecha junto a **Credenciales**.

	![Hoja Credenciales](./media/data-factory-move-data-between-onprem-and-cloud/credentials-dialog.png)
7.	En la hoja **Credenciales**, haga clic en la opción **Haga clic aquí para establecer las credenciales**.
8.	En el cuadro de diálogo **Establecer credenciales**, realice lo siguiente:

	![Cuadro de diálogo Establecer credenciales](./media/data-factory-move-data-between-onprem-and-cloud/setting-credentials-dialog.png) 1. Seleccione la **autenticación** que desea que use el servicio Factoría de datos para conectarse a la base de datos. 2. Escriba el nombre del usuario que tiene acceso a la base de datos para la configuración **NOMBRE DE USUARIO**. 3. Escriba la contraseña para el usuario para la configuración **CONTRASEÑA**. 4. Haga clic en **Aceptar** para cerrar el cuadro de diálogo. 
4. Haga clic en **Aceptar** para cerrar la hoja **Credenciales**. 
5. Haga clic en **Aceptar** en la hoja **Nuevo almacén de datos**. 	
6. Confirme que el estado de **SqlServerLinkedService** está establecido en En línea en la hoja Servicios vinculados.![Estado del servicio vinculado de SQL Server](./media/data-factory-move-data-between-onprem-and-cloud/sql-server-linked-service-status.png)

Si tiene acceso al portal desde un equipo diferente del equipo de la puerta de enlace, debe asegurarse de que la aplicación del Administrador de credenciales puede conectarse al equipo de la puerta de enlace. Si la aplicación no puede ponerse en contacto con el equipo de la puerta de enlace, no podrá establecer las credenciales del origen de datos ni probar la conexión al origen de datos.

##### Configuración de credenciales y seguridad
Cuando usa el método descrito anteriormente con la aplicación "Establecer credenciales" iniciada desde el Portal de Azure para establecer credenciales para un origen de datos local, el portal cifra las credenciales con el certificado especificado en la pestaña Certificado del Administrador de configuración de Data Management Gateway en el equipo de puerta de enlace.

Si desea un enfoque basado en API para cifrar las credenciales, puede usar el cmdlet [New-AzureDataFactoryEncryptValue](https://msdn.microsoft.com/library/azure/dn834940.aspx) de PowerShell para cifrar las credenciales. El cmdlet usa el certificado cuyo uso tiene configurado esa puerta de enlace para cifrar las credenciales. Puede usar las credenciales cifradas devueltas por este cmdlet y agregarlas al elemento EncryptedCredential de connectionString en el archivo JSON que se va a emplear con el cmdlet [New-AzureDataFactoryLinkedService](https://msdn.microsoft.com/library/azure/dn820246.aspx) o en el fragmento JSON en el Editor de la Factoría de datos en el portal.

	"connectionString": "Data Source=<servername>;Initial Catalog=<databasename>;Integrated Security=True;EncryptedCredential=<encrypted credential>",

**Nota:** Si usa la aplicación "Establecer credenciales", establece automáticamente las credenciales cifradas en el servicio vinculado, como se mostró anteriormente.

No hay otro enfoque más para establecer credenciales mediante el Editor de la Factoría de datos. Si crea un servicio vinculado de SQL Server mediante el editor y escribe las credenciales en texto sin formato, las credenciales se cifran mediante un certificado que posee el servicio Factoría de datos, NO el certificado que la puerta de enlace está configurada para usar. Aunque este enfoque puede resultar un poco más rápido, en algunos casos es menos seguro. Por lo tanto, se recomienda seguir este enfoque solo para fines de desarrollo y pruebas.

#### Adición de un servicio vinculado para una cuenta de almacenamiento de Azure
 
1. En la hoja **Servicios vinculados**, haga clic en **Nuevo almacén de datos** en la barra de herramientas. 
2. Escriba **StorageLinkedService** en el campo **Nombre**. 
3. Haga clic en flecha junto a **Tipo** y seleccione **Almacenamiento de Azure**.
4. Debería ver unos campos nuevos: **Nombre de cuenta** y **Clave de cuenta** en la configuración **Tipo**. 
3. Escriba el nombre de la cuenta de almacenamiento de Azure en **Nombre de cuenta**.
4. Escriba la clave de su cuenta de almacenamiento de Azure en **Clave de cuenta**. 
5. Haga clic en **Aceptar** para cerrar el cuadro de diálogo. 

 
### Paso 3: Creación de conjuntos de datos de entrada y salida
En este paso, creará conjuntos de datos de entrada y de salida que representan datos de entrada y de salida para la operación de copia (base de datos de SQL Server local => almacenamiento de blobs de Azure). Antes de crear conjuntos de datos o tablas (conjuntos de datos rectangulares), debe hacer lo siguiente (después de la lista, se detallan los pasos):

- Cree una tabla con el nombre **emp** en la base de datos de SQL Server que agregó como servicio vinculado a la factoría de datos e inserte un par de entradas de ejemplo en la tabla.
- Cree un contenedor de blob denominado **adftutorial** en la cuenta de almacenamiento de blobs de Azure que agregó como un servicio vinculado a la factoría de datos.

### Preparación de la instancia local de SQL Server para el tutorial

1. En la base de datos que especificó para el servicio vinculado de SQL Server local (**SqlServerLinkedService**), use el siguiente script de SQL para crear la tabla **emp** en la base de datos.


        CREATE TABLE dbo.emp
		(
			ID int IDENTITY(1,1) NOT NULL, 
			FirstName varchar(50),
			LastName varchar(50),
    		CONSTRAINT PK_emp PRIMARY KEY (ID)
		)
		GO
 

2. Inserte algún ejemplo en la tabla:


        INSERT INTO emp VALUES ('John', 'Doe')
		INSERT INTO emp VALUES ('Jane', 'Doe')



### Creación de la tabla de entrada

1.	En la hoja **FACTORÍA DE DATOS**, haga clic en la ventana **Crear e implementar** para iniciar el **Editor** para la factoría de datos.

	![Mosaico Crear e implementar](./media/data-factory-move-data-between-onprem-and-cloud/author-deploy-tile.png) 
1. En el **Editor de la Factoría de datos**, haga clic en **Nuevo conjunto de datos** en la barra de comandos y seleccione **SQL local**. 
2.	Reemplace el script JSON del panel derecho por el texto siguiente:    

		{
		  "name": "EmpOnPremSQLTable",
		  "properties": {
		    "type": "SqlServerTable",
		    "linkedServiceName": "SqlServerLinkedService",
		    "typeProperties": {
		      "tableName": "emp"
		    },
		    "external": true,
		    "availability": {
		      "frequency": "Hour",
		      "interval": 1
		    },
		    "policy": {
		      "externalData": {
		        "retryInterval": "00:01:00",
		        "retryTimeout": "00:10:00",
		        "maximumRetry": 3
		      }
		    }
		  }
		}

	Tenga en cuenta lo siguiente:
	
	- **type** está establecido en **SqlServerTable**.
	- **tableName** está establecido en **emp**.
	- **linkedServiceName** está establecido en **SqlServerLinkedService** (creó este servicio vinculado en el paso 2).
	- Para una tabla de entrada no generada por otra canalización en Factoría de datos de Azure, debe especificar la propiedad **external** en **true**. Indica que los datos de entrada se han producido fuera del servicio Factoría de datos de Azure. Opcionalmente, puede especificar las directivas de datos externos mediante el elemento **externalData** en la sección **Policy**.    

	Consulte la [documentación de referencia de scripting con JSON][json-script-reference] para obtener información detallada acerca de las propiedades JSON.

2. Haga clic en **Implementar** en la barra de comandos para implementar el conjunto de datos (la tabla es un conjunto de datos rectangular). Confirme que aparece un mensaje en la barra de título que indica **LA TABLA SE IMPLEMENTÓ CORRECTAMENTE**.


### Creación de la tabla de salida

1.	En el **Editor de la Factoría de datos**, haga clic en **Nuevo conjunto de datos** en la barra de comandos y seleccione **Almacenamiento de blobs de Azure**.
2.	Reemplace el script JSON del panel derecho por el texto siguiente: 

		{
		  "name": "OutputBlobTable",
		  "properties": {
		    "type": "AzureBlob",
		    "linkedServiceName": "StorageLinkedService",
		    "typeProperties": {
		      "folderPath": "adftutorial/outfromonpremdf",
		      "format": {
		        "type": "TextFormat",
		        "columnDelimiter": ","
		      }
		    },
		    "availability": {
		      "frequency": "Hour",
		      "interval": 1
		    }
		  }
		}
  
	Tenga en cuenta lo siguiente:
	
	- **type** está establecido en **AzureBlob**.
	- **linkedServiceName** está establecido en **StorageLinkedService** (creó este servicio vinculado en el paso 2).
	- **folderPath** está establecido en **adftutorial/outfromonpremdf**, donde outfromonpremdf es la carpeta del contenedor adftutorial. Solo tiene que crear el contenedor **adftutorial**.
	- El elemento **availability** está establecido en **hourly** (**frequency** está establecido en **hour** e **interval**, en **1**). El servicio Factoría de datos generará un segmento de datos de salida cada hora en la tabla **emp** de la base de datos SQL de Azure. 

	Si no especifica **fileName** para una **tabla de entrada**, todos los archivos o blobs de la carpeta de entrada (**folderPath**) se consideran entradas. Si especifica un nombre de archivo en JSON, solo el archivo o blob especificado se consideran una entrada. Puede ver algunos archivos de ejemplo en [tutorial][adf-tutorial].
 
	Si no especifica un valor **fileName** para una tabla de salida, los archivos generados en la **ruta de la carpeta** se denominan con el siguiente formato: Data.<Guid>.txt (por ejemplo: Data.0a405f8a-93ff-4c6f-b3be-f69616f1df7a.txt.).

	Para establecer **folderPath** y **fileName** de forma dinámica según la hora de **SliceStart**, use la propiedad partitionedBy. En el ejemplo siguiente, folderPath usa Year, Month y Day de SliceStart (hora de inicio del segmento que se va a procesar) y fileName usa Hour de SliceStart. Por ejemplo, si se está produciendo una división de 2014-10-20T08:00:00, el nombre de carpeta se establece en wikidatagateway/wikisampledataout/2014/10/20 y el nombre de archivo se establece en 08.csv.

	  	"folderPath": "wikidatagateway/wikisampledataout/{Year}/{Month}/{Day}",
        "fileName": "{Hour}.csv",
        "partitionedBy": 
        [
        	{ "name": "Year", "value": { "type": "DateTime", "date": "SliceStart", "format": "yyyy" } },
            { "name": "Month", "value": { "type": "DateTime", "date": "SliceStart", "format": "MM" } }, 
            { "name": "Day", "value": { "type": "DateTime", "date": "SliceStart", "format": "dd" } }, 
            { "name": "Hour", "value": { "type": "DateTime", "date": "SliceStart", "format": "hh" } } 
        ],

 

	Consulte la [documentación de referencia de scripting con JSON][json-script-reference] para obtener información detallada acerca de las propiedades JSON.

2.	Haga clic en **Implementar** en la barra de comandos para implementar el conjunto de datos (la tabla es un conjunto de datos rectangular). Confirme que aparece un mensaje en la barra de título que indica **LA TABLA SE IMPLEMENTÓ CORRECTAMENTE**.
  

### Paso 4: Creación y ejecución de una canalización
En este paso, va a crear una **canalización** con una **actividad de copia** que usa **EmpOnPremSQLTable** como entrada y **OutputBlobTable** como salida.

1.	En la hoja **FACTORÍA DE DATOS**, haga clic en la ventana **Crear e implementar** para iniciar el **Editor** para la factoría de datos.

	![Mosaico Crear e implementar](./media/data-factory-move-data-between-onprem-and-cloud/author-deploy-tile.png) 
2.	Haga clic en **Nueva canalización** en la barra de comandos. Si no ve el botón, haga clic en **... (puntos suspensivos)** para expandir la barra de comandos.
2.	Reemplace el script JSON del panel derecho por el texto siguiente:   


		{
		  "name": "ADFTutorialPipelineOnPrem",
		  "properties": {
		    "description": "This pipeline has one Copy activity that copies data from an on-prem SQL to Azure blob",
		    "activities": [
		      {
		        "name": "CopyFromSQLtoBlob",
		        "description": "Copy data from on-prem SQL server to blob",
		        "type": "Copy",
		        "inputs": [
		          {
		            "name": "EmpOnPremSQLTable"
		          }
		        ],
		        "outputs": [
		          {
		            "name": "OutputBlobTable"
		          }
		        ],
		        "typeProperties": {
		          "source": {
		            "type": "SqlSource",
		            "sqlReaderQuery": "select * from emp"
		          },
		          "sink": {
		            "type": "BlobSink"
		          }
		        },
		        "Policy": {
		          "concurrency": 1,
		          "executionPriorityOrder": "NewestFirst",
		          "style": "StartOfInterval",
		          "retry": 0,
		          "timeout": "01:00:00"
		        }
		      }
		    ],
		    "start": "2015-02-13T00:00:00Z",
		    "end": "2015-02-14T00:00:00Z",
		    "isPaused": false
		  }
		}

	Tenga en cuenta lo siguiente:
 
	- En la sección de actividades, solo hay una actividad cuyo **type** está establecido en **Copy**.
	- La **entrada** de la actividad está establecida en **EmpOnPremSQLTable** y la **salida** de la actividad está establecida en **OutputBlobTable**.
	- En la sección **transformation** , **SqlSource** está especificado como el **tipo de origen** y **BlobSink ** está especificado como el **tipo de receptor**.
- La consulta SQL **select * from emp** está especificada para la propiedad **sqlReaderQuery** de **SqlSource**.

	Reemplace el valor de la propiedad **start** por el día actual y el valor **end** por el próximo día. Las fechas y horas de inicio y de finalización deben estar en [formato ISO](http://en.wikipedia.org/wiki/ISO_8601). Por ejemplo: 2014-10-14T16:32:41Z. La hora de **end** es opcional, pero se utilizará en este tutorial.
	
	Si no especifica ningún valor para la propiedad **end**, se calcula como "**start + 48 horas**". Para ejecutar la canalización indefinidamente, especifique **9/9/9999** como valor de la propiedad **end**.
	
	Está definiendo la duración del procesamiento de los segmentos de datos según las propiedades de **disponibilidad** que se definieron para cada tabla de la Factoría de datos de Azure.
	
	En el ejemplo anterior, habrá 24 segmentos de datos, porque cada segmento de datos se produce cada hora.
	
2. Haga clic en **Implementar** en la barra de comandos para implementar el conjunto de datos (la tabla es un conjunto de datos rectangular). Confirme que aparece un mensaje en la barra de título que indica **LA CANALIZACIÓN SE IMPLEMENTÓ CORRECTAMENTE**.
5. Ahora, cierre la hoja **Editor** haciendo clic en **X**. Vuelva a hacer clic en **X** para cerrar la hoja ADFTutorialDataFactory con la barra de herramientas y la vista de árbol. Si ve el mensaje que indica que **se descartarán las modificaciones no guardadas**, haga clic en **Aceptar**.
6. Debe volver a la hoja **FACTORÍA DE DATOS** para **ADFTutorialDataFactory**.

**¡Enhorabuena!** Ha creado correctamente una factoría de datos de Azure, servicios vinculados, tablas y una canalización, y ha programado la canalización.

#### Visualización de la factoría de datos en una vista de diagrama 
1. En el **Portal de vista previa de Azure**, haga clic en el icono **Diagrama** en la página principal de Factoría de datos **ADFTutorialOnPremDF**.

	![Vínculo de diagrama](./media/data-factory-move-data-between-onprem-and-cloud/OnPremDiagramLink.png)

2. Debería ver un diagrama similar al siguiente:

	![Vista de diagrama](./media/data-factory-move-data-between-onprem-and-cloud/OnPremDiagramView.png)

	Puede acercar, alejar, acercar al 100%, ampliar para ajustar, colocar automáticamente canalizaciones y tablas, y mostrar información de linaje (resalta elementos ascendentes y descendentes de los elementos seleccionados). Puede hacer doble clic en un objeto (tabla o canalización de entrada o salida) para ver sus propiedades.

### Paso 5: Supervisión de las canalizaciones y los conjuntos de datos
En este paso, usará el Portal de Azure para supervisar lo que está ocurriendo en una factoría de datos de Azure. También puede usar los cmdlets de PowerShell para supervisar los conjuntos de datos y las canalizaciones. Para obtener más información acerca de la supervisión, consulte [Supervisión y administración de canalizaciones](monitor-manage-pipelines.md).

1. Vaya al **Portal de vista previa de Azure** (si lo ha cerrado).
2. Si la hoja de **ADFTutorialOnPremDF** no está abierta, ábrala haciendo clic en **ADFTutorialOnPremDF** en el **Panel de inicio**.
3. Deben mostrarse el **recuento** y los **nombres** de las tablas y la canalización que creó en esta hoja.

	![Página principal de Factoría de datos](./media/data-factory-move-data-between-onprem-and-cloud/OnPremDiagramView.png)
4. Ahora haga clic en el icono **Conjuntos de datos**.
5. En la hoja **Conjuntos de datos**, haga clic en **EmpOnPremSQLTable**.

	![Segmentos EmpOnPremSQLTable](./media/data-factory-move-data-between-onprem-and-cloud/OnPremSQLTableSlicesBlade.png)

6. Tenga en cuenta que ya se han producido los segmentos de datos hasta el momento actual y están **listos**. Esto ocurre porque ha insertado los datos en la base de datos de SQL Server y están ahí todo el tiempo. Confirme que no aparecen segmentos en la sección de **segmentos problemáticos** de la parte inferior.


	Las listas **Segmentos actualizados recientemente** y **Segmentos erróneos recientes** se ordenan por la **HORA DE LA ÚLTIMA ACTUALIZACIÓN**. En las situaciones siguientes, se cambia la hora de actualización de un segmento.
    

	-  El estado del segmento se actualiza manualmente, por ejemplo, con **Set-AzureDataFactorySliceStatus** (o) al hacer clic en **EJECUTAR** en la hoja **SEGMENTO** para el segmento.
	-  El segmento cambia de estado debido a una ejecución (por ejemplo, una ejecución se inició, una ejecución finalizó y produjo un error, una ejecución finalizó correctamente, etc.).
 
	Haga clic en el título de las listas o **... (puntos suspensivos)** para ver la lista más amplia de segmentos. Haga clic en **Filtro** en la barra de herramientas para filtrar los segmentos.
	
	Para ver los intervalos de datos ordenados por las horas de inicio y finalización, haga clic en el icono **Segmentos de datos (por hora del segmento)**.

7. A continuación, en la hoja **Conjuntos de datos**, haga clic en **OutputBlobTable**.

	![OputputBlobTable slices][image-data-factory-output-blobtable-slices]
8. Confirme que se han producido los segmentos hasta el momento actual y que están **listos**. Espere hasta que los estados de los segmentos hasta el momento actual estén establecidos en **Listo**.
9. Haga clic en cualquier segmento de datos de la lista. Debe aparecer la hoja **SEGMENTO DE DATOS**.

	![Hoja Segmento de datos](./media/data-factory-move-data-between-onprem-and-cloud/DataSlice.png)

	Si el segmento no está en el estado **Listo**, puede ver los segmentos ascendentes que no están en estado Listo y bloquean la ejecución del segmento actual en la lista **Segmentos ascendentes que no están listos**.

10. Haga clic en la **ejecución de la actividad** de la lista en la parte inferior para ver los **detalles de la ejecución de la actividad**.

	![Activity Run Details blade][image-data-factory-activity-run-details]

11. Haga clic en **X** para cerrar todas las hojas hasta que
12. regrese a la hoja principal para **ADFTutorialOnPremDF**.
14. (Opcional) Haga clic en **Canalizaciones**, elija **ADFTutorialOnPremDF** y obtenga detalles de las tablas de entrada (**Consumido**) o las tablas de salida (**Producido**).
15. Use herramientas como el **Explorador de almacenamiento de Azure** para comprobar la salida.

	![Explorador de almacenamiento de Azure](./media/data-factory-move-data-between-onprem-and-cloud/OnPremAzureStorageExplorer.png)


## Creación y registro de una puerta de enlace con Azure PowerShell 
En esta sección se explica cómo crear y registrar una puerta de enlace usando cmdlets de Azure PowerShell.

1. Inicie **Azure PowerShell** en modo de administrador. 
2. Los cmdlets de Factoría de datos de Azure están disponibles en el modo **AzureResourceManager**. Ejecute el siguiente comando para cambiar al modo **AzureResourceManager**.     

        switch-azuremode AzureResourceManager


2. Use el cmdlet **New-AzureDataFactoryGateway** para crear una puerta de enlace lógica como se indica a continuación:

		New-AzureDataFactoryGateway -Name <gatewayName> -DataFactoryName <dataFactoryName> -ResourceGroupName ADF –Description <desc>

	**Ejemplo de comando y salida**:


		PS C:\> New-AzureDataFactoryGateway -Name MyGateway -DataFactoryName $df -ResourceGroupName ADF –Description “gateway for walkthrough”

		Name              : MyGateway
		Description       : gateway for walkthrough
		Version           :
		Status            : NeedRegistration
		VersionStatus     : None
		CreateTime        : 9/28/2014 10:58:22
		RegisterTime      :
		LastConnectTime   :
		ExpiryTime        :
		ProvisioningState : Succeeded


3. Use el cmdlet **New-AzureDataFactoryGatewayKey** para generar una clave de registro para la puerta de enlace recién creada y almacene la clave en una variable local **$Key**:

		New-AzureDataFactoryGatewayKey -GatewayName <gatewayname> -ResourceGroupName ADF -DataFactoryName <dataFactoryName>

	
	**Ejemplo de comando y salida:**


		PS C:\> $Key = New-AzureDataFactoryGatewayKey -GatewayName MyGateway -ResourceGroupName ADF -DataFactoryName $df 

	
4. En Azure PowerShell, cambie a la carpeta **C:\\\\Archivos de programa\\Microsoft Data Management Gateway\\1.0\\PowerShellScript** y ejecute el script **RegisterGateway.ps1** asociado con la variable local **$Key** como se muestra en el siguiente comando para registrar el agente cliente instalado en su equipo en la puerta de enlace lógica que creó antes.

		PS C:\> .\RegisterGateway.ps1 $Key.GatewayKey
		
		Agent registration is successful!

5. Use el cmdlet **Get-AzureDataFactoryGateway** para obtener la lista de puertas de enlace de su factoría de datos. Cuando el **Estado** es **En línea**, significa que la puerta de enlace está lista para usarla.

		Get-AzureDataFactoryGateway -DataFactoryName <dataFactoryName> -ResourceGroupName ADF

Puede quitar una puerta de enlace con el cmdlet **Remove-AzureDataFactoryGateway** y actualizar la descripción de una puerta de enlace con los cmdlets **Set-AzureDataFactoryGateway**. Para ver la sintaxis y otros detalles de estos cmdlets, consulte la documentación de referencia de los cmdlets de Factoría de datos.


## Flujo de datos de copia mediante Data Management Gateway
Al usar una actividad de copia en una canalización de datos para introducir datos locales en la nube para su posterior procesamiento, o bien exportar los datos de resultados en la nube de nuevo a un almacén de datos local, la actividad de copia usa internamente una puerta de enlace para transferir los datos de origen de un origen de datos local a la nube y viceversa.

A continuación se muestra el flujo de datos de alto nivel y el resumen de los pasos para copiar con una puerta de enlace de datos:![Flujo de datos mediante la puerta de enlace](./media/data-factory-move-data-between-onprem-and-cloud/data-flow-using-gateway.png)

1.	El desarrollador de datos crea una nueva puerta de enlace para una Factoría de datos de Azure mediante el [Portal de Azure](http://portal.azure.com) o un [cmdlet de PowerShell](https://msdn.microsoft.com/library/dn820234.aspx). 
2.	El desarrollador de datos usa el panel "Servicios vinculado" para definir un nuevo servicio vinculado para un almacén de datos local con la puerta de enlace. Como parte de la configuración de los datos de servicios vinculados el desarrollador usa la aplicación Establecer credenciales, como se muestra en el tutorial paso a paso para especificar las credenciales y los tipos de autenticación. El cuadro de diálogo de la aplicación Establecer credenciales se comunicará con el almacén de datos para probar la conexión y la puerta de enlace para guardar las credenciales.
3.	La puerta de enlace cifrará las credenciales con el certificado asociado a la puerta de enlace (suministrado por el desarrollador de datos), antes de guardar las credenciales en la nube.
4.	El servicio de movimiento de la factoría de datos se comunica con la puerta de enlace para la programación y administración de trabajos a través de un canal de control que usa una cola de bus compartida del servicio de Azure. Cuando es necesario un trabajo de actividad de copia, la factoría de datos pone en cola la solicitud junto con la información de credenciales. La puerta de enlace inicia el trabajo después de sondear la cola.
5.	La puerta de enlace descifra las credenciales con el mismo certificado y, a continuación, se conecta al almacén de datos local con el tipo de autenticación adecuado.
6.	La puerta de enlace copia datos desde el almacén local a un almacenamiento en la nube o desde un almacenamiento en la nube a un almacén de datos local según cómo esté configurada la actividad de copia en la canalización de datos. Nota: Para este paso, la puerta de enlace se comunica directamente con el servicio de almacenamiento basado en la nube (Blob de Azure, SQL de Azure, etc.) a través del canal seguro (HTTPS).

### Consideraciones de puertos y seguridad

1. Como se mencionó anteriormente en el tutorial paso a paso, hay varias maneras de establecer credenciales para almacenes de datos locales con la factoría de datos. Las consideraciones de puerto varían para estas opciones.	

	- Mediante la aplicación **Establecer credenciales**: de forma predeterminada, el programa de instalación de Data Management Gateway abre los puertos **8050** y **8051** en el Firewall de Windows local para el equipo de puerta de enlace. La aplicación Establecer credenciales usa estos puertos para retransmitir las credenciales a la puerta de enlace. Estos puertos se abren solo para el equipo en el Firewall de Windows local. Estos puertos no se pueden alcanzar desde Internet y no es necesario que estén abiertos en el firewall corporativo.
	2.	Mediante commandlet de powershell [New-AzureDataFactoryEncryptValue](https://msdn.microsoft.com/library/dn834940.aspx): a. Si está usando el comando de Powershell para cifrar las credenciales y como resultado no desea que la instalación de puerta de enlace abra los puertos de entrada en el equipo de puerta de enlace en Firewall de Windows, puede hacerlo mediante el comando siguiente durante la instalación:
	
			msiexec /q /i DataManagementGateway.msi NOFIREWALL=1
3.	Si usa la aplicación **Configuración de credenciales**, debe iniciarla en un equipo que pueda conectarse a Data Management Gateway para poder establecer credenciales para el origen de datos y probar la conexión al origen de datos.
4.	Al copiar datos desde y hacia una base de datos de SQL Server local hacia o desde una base de datos SQL a Azure, asegúrese de lo siguiente:	
	- 	El firewall del equipo de la puerta de enlace permite la salida de la comunicación TCP en el puerto **TCP** **1433**.
	- 	Establezca la [configuración de firewall de SQL Azure](https://msdn.microsoft.com/library/azure/jj553530.aspx) para agregar la **dirección IP del equipo de la puerta de enlace** a las **direcciones IP permitidas**.
5.	Si copia datos desde y hacia SQL Server local a cualquier destino y los equipos de la puerta de enlace y de SQL Server son diferentes, haga lo siguiente: [configure Firewall de Windows](https://msdn.microsoft.com/library/ms175043.aspx) en el equipo de SQL Server para que la puerta de enlace pueda tener acceso a la base de datos a través de puertos en los que escucha la instancia de SQL Server. En la instancia predeterminada es el puerto 1433.

<!---HONumber=August15_HO6-->