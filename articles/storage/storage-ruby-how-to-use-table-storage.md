<properties 
	pageTitle="Uso del almacenamiento de tablas de Ruby | Microsoft Azure" 
	description="Aprenda a usar el servicio de almacenamiento de tablas en Azure. Los ejemplos de código están escritos usando la API Ruby." 
	services="storage" 
	documentationCenter="ruby" 
	authors="tfitzmac" 
	manager="wpickett" 
	editor=""/>

<tags 
	ms.service="storage" 
	ms.workload="storage" 
	ms.tgt_pltfrm="na" 
	ms.devlang="ruby" 
	ms.topic="article" 
	ms.date="07/29/2015" 
	ms.author="tomfitz"/>


# Uso del almacenamiento de tablas de Ruby

[AZURE.INCLUDE [storage-selector-table-include](../../includes/storage-selector-table-include.md)]

## Información general

Esta guía le indicará cómo actuar en situaciones habituales usando el servicio Tabla de Microsoft Azure. Los ejemplos se han escrito usando la API Ruby. Entre los escenarios descritos se incluyen la **creación y eliminación de una tabla, la inserción y la consulta de entidades en una tabla**.

[AZURE.INCLUDE [storage-table-concepts-include](../../includes/storage-table-concepts-include.md)]

[AZURE.INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

## Creación de una aplicación de Ruby

Cree una aplicación de Ruby. Para obtener instrucciones, consulte [Creación de una aplicación de Ruby en Azure](/develop/ruby/tutorials/web-app-with-linux-vm/).

## Configuración de la aplicación para obtener acceso al almacenamiento

Para usar el almacenamiento de Azure tendrá que descargar y usar el paquete Ruby azure, que incluye un conjunto de útiles bibliotecas que se comunican con los servicios REST de almacenamiento.

### Uso de RubyGems para obtener el paquete

1. Use una interfaz de línea de comandos como **PowerShell** (Windows), **Terminal** (Mac) o **Bash** (Unix).

2. Escriba **gem install azure** en la ventana de comandos para instalar la gema y las dependencias.

### Importación del paquete

Con el editor de texto que prefiera, agregue lo siguiente al principio del archivo de Ruby en el que pretenda utilizar el almacenamiento:

	require "azure"

## Configuración de una conexión de almacenamiento de Azure

El módulo azure leerá las variables de entorno **AZURE\_STORAGE\_ACCOUNT** y **AZURE\_STORAGE\_ACCESS\_KEY** para obtener información necesaria para conectarse a su cuenta de almacenamiento de Azure. Si no se establecen estas variables de entorno, debe especificar la información de la cuenta antes de usar **Azure::TableService** con el siguiente código:

	Azure.config.storage_account_name = "<your azure storage account>"
	Azure.config.storage_access_key = "<your azure storage access key>"

Para obtener estos valores:

1. Inicie sesión en el [Portal de administración de Azure](https://manage.windowsazure.com/).

2. Vaya a la cuenta de almacenamiento que desea utilizar.

3. Haga clic en **ADMINISTRAR CLAVES** en la parte inferior del panel de navegación.

4. En el cuadro de diálogo emergente, verá el nombre de cuenta de almacenamiento, la clave de acceso principal y la clave de acceso secundaria. Para la clave de acceso, puede elegir la principal o la secundaria.

## Creación de una tabla

El objeto **Azure::TableService** permite trabajar con tablas y entidades. Para crear una tabla, use el método **create\_table()**. En el siguiente ejemplo se crea una tabla o se imprime el error, si hay alguno.

	azure_table_service = Azure::TableService.new
	begin
	  azure_table_service.create_table("testtable")
	rescue
	  puts $!
	end

## Incorporación de una entidad a una tabla

Para agregar una entidad, primero cree un objeto hash que defina las propiedades de la entidad. Tenga en cuenta que para cada entidad debe especificar valores en **PartitionKey** y **RowKey**. Estos son los identificadores exclusivos de sus entidades y son valores que se pueden consultar más rápidamente que las demás propiedades. El servicio de almacenamiento de Azure usa **PartitionKey** para distribuir automáticamente las entidades de la tabla entre varios nodos de almacenamiento. Las entidades con la misma **PartitionKey** se almacenan en el mismo nodo. **RowKey** es el identificador exclusivo de la entidad de la partición a la que pertenece.

	entity = { "content" => "test entity", 
	  :PartitionKey => "test-partition-key", :RowKey => "1" }
	azure_table_service.insert_entity("testtable", entity)

## Actualización de una entidad

Hay varios métodos para actualizar una entidad existente:

* **update\_entity():** actualiza una entidad existente reemplazándola.
* **merge\_entity():**: actualiza una entidad que ya existe combinando los valores de las nuevas propiedades con la entidad existente.
* **insert\_or\_merge\_entity():** actualiza una entidad existente reemplazándola. Si no hay entidades, se insertará una nueva.
* **insert\_or\_replace\_entity():** actualiza una entidad que ya existe combinando los valores de las nuevas propiedades con la entidad existente. Si no hay entidades, se insertará una nueva.

El ejemplo siguiente demuestra cómo actualizar una entidad usando **update\_entity()**:

	entity = { "content" => "test entity with updated content", 
	  :PartitionKey => "test-partition-key", :RowKey => "1" }
	azure_table_service.update_entity("testtable", entity)

Con **update\_entity()** y **merge\_entity()**, si la entidad que se está actualizando no existe, se producirá un error en la operación de actualización. Por lo tanto, si desea almacenar una entidad independientemente de si ya existe, debe usar **insert\_or\_replace\_entity()** o **insert\_or\_merge\_entity()**.

## Trabajo con grupos de entidades

A veces resulta útil enviar varias operaciones juntas en un lote a fin de garantizar el procesamiento atómico por parte del servidor. Para ello, cree primero un objeto **Batch** y, a continuación, utilice el método **execute\_batch()** en **TableService**. El siguiente ejemplo muestra el envío de dos entidades con RowKey 2 y 3 en un lote. Tenga en cuenta que solo funciona para entidades con el mismo valor de PartitionKey.

	azure_table_service = Azure::TableService.new
	batch = Azure::Storage::Table::Batch.new("testtable", 
	  "test-partition-key") do
	  insert "2", { "content" => "new content 2" }
	  insert "3", { "content" => "new content 3" }
	end
	results = azure_table_service.execute_batch(batch)

## Consulta de una entidad

Para consultar la entidad de una tabla, use el método **get\_entity()**, pasando el nombre de tabla, **PartitionKey** y **RowKey**.

	result = azure_table_service.get_entity("testtable", "test-partition-key", 
	  "1")

## Consulta de un conjunto de entidades

Para realizar una consulta de un conjunto de entidades en una tabla, cree un objeto hash de consulta y use el método **query\_entities()**. El siguiente ejemplo muestra la obtención de todas las entidades con el mismo valor de **PartitionKey**:

	query = { :filter => "PartitionKey eq 'test-partition-key'" }
	result, token = azure_table_service.query_entities("testtable", query)

**Tenga en cuenta** que si el resultado establecido es demasiado largo para que lo devuelva una única consulta, se devolverá un token de continuación que puede usar para recuperar las páginas siguientes.

## Consulta de un subconjunto de propiedades de las entidades

Una consulta de tabla puede recuperar solo algunas propiedades de una entidad. Esta técnica, denominada "proyección", reduce el ancho de banda y puede mejorar el rendimiento de las consultas, en especial en el caso de entidades de gran tamaño. Utilice la cláusula select y pase los nombres de las propiedades que quiera que lleguen al cliente.

	query = { :filter => "PartitionKey eq 'test-partition-key'", 
	  :select => ["content"] }
	result, token = azure_table_service.query_entities("testtable", query)

## Eliminación de una entidad

Para eliminar una entidad, use el método **delete\_entity()**. Tiene que pasar el nombre de la tabla que contiene la entidad, la PartitionKey y la RowKey de la entidad.

		azure_table_service.delete_entity("testtable", "test-partition-key", "1")

## Cómo eliminar una tabla

Para eliminar una tabla, use el método **delete\_table()** para pasar el nombre de la tabla que desee eliminar.

		azure_table_service.delete_table("testtable")

## Pasos siguientes

Ahora que está familiarizado con los aspectos básicos del almacenamiento de tablas, utilice estos vínculos para obtener más información acerca de tareas de almacenamiento más complejas.

- Vea la Referencia MSDN: [Almacenamiento de Azure](http://msdn.microsoft.com/library/azure/gg433040.aspx)
- Visite el [Blog del equipo de almacenamiento de Azure](http://blogs.msdn.com/b/windowsazurestorage/)
- Visite el repositorio de [SDK de Azure para Ruby](http://github.com/WindowsAzure/azure-sdk-for-ruby) en GitHub
 

<!---HONumber=August15_HO6-->