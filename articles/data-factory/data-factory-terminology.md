<properties 
	pageTitle="Factoría de datos de Azure: terminología" 
	description="En este artículo se presenta la terminología que se usa al crear factorías de datos con el servicio Factoría de datos de Azure." 
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
	ms.date="07/07/2015" 
	ms.author="spelluru"/>

#Factoría de datos de Azure: terminología

## Factoría de datos
Una **factoría de datos de Azure** tiene una o más canalizaciones que procesan los datos en almacenes de datos vinculados (Almacenamiento de Azure, Base de datos SQL de Azure, SQL Server local, etc.) mediante servicios de proceso vinculados como HDInsight de Azure. Una factoría de datos de Azure no contiene datos en ella misma; en su lugar, los datos se almacenan en los almacenes de datos mencionados anteriormente.

## Servicios vinculados
Un **servicio vinculado** es un servicio que está vinculado a una factoría de datos de Azure. Un servicio vinculado puede ser uno de los siguientes:

- Un servicio de **almacenamiento de datos**, como Almacenamiento de Azure, Base de datos SQL de Azure o una base de datos de SQL Server local. Un almacén de datos es un contenedor de conjuntos de datos de entrada y salida.    
- Un servicio de **proceso**, como HDInsight de Azure, Aprendizaje automático de Azure Batch. Un servicio de proceso procesa los datos de entrada y genera los datos de salida.  

## Conjunto de datos
Un **conjunto de datos** es una vista de datos con nombre. Los datos que se describen pueden variar desde bytes simples, pasando por datos semiestructurados, como archivos CSV, hasta tablas relacionales o incluso modelos. Una **tabla** de Factoría de datos es un conjunto de datos que tiene un esquema y es rectangular. Después de crear un servicio vinculado en un almacén de datos que hace referencia a un almacén de datos, puede definir conjuntos de datos que representan datos de entrada y salida que se almacenan en el almacén de datos.


##Canalización
Una **canalización** en una factoría de datos de Azure procesa los datos de los servicios de almacenamiento vinculados mediante servicios de proceso vinculados. Contiene una secuencia de actividades donde cada actividad realiza una operación de procesamiento específica. Por ejemplo, una **actividad de copia** copia los datos de un almacenamiento de origen en un almacenamiento de destino y **una actividad de HDInsight** usa un clúster de HDInsight de Azure para procesar los datos mediante consultas de Hive o scripts de Pig. Una factoría de datos puede tener una o más canalizaciones.

Los pasos típicos para crear una instancia de la Factoría de datos de Azure son:

1. Crear una **factoría de datos**.
2. Crear un **servicio vinculado** para cada almacén de datos o servicio de proceso.
3. Crear **conjuntos de datos** de entrada y salida.
4. Crear una **canalización**. 

##Actividad
Un paso del procesamiento de datos en una canalización que toma uno o más conjuntos de datos de entrada y produce uno o más conjuntos de datos de salida. Las actividades se ejecutan en un entorno de ejecución (por ejemplo, un clúster de HDInsight de Azure) y leen o escriben datos en un almacén de datos asociado o vinculado a la factoría de datos de Azure.

El servicio de Factoría de datos de Azure admite las siguientes actividades en una canalización:

- La **actividad de copia** copia los datos de un almacén de datos en otro almacén de datos. Consulte [Copia de datos con la Factoría de datos de Azure][copy-data-with-adf] para obtener información sobre cuáles son los almacenes de datos que admite la actividad de copia. 
- La **actividad de HDInsight** procesa datos ejecutando scripts de Hive o Pig o programas de MapReduce en un clúster de HDInsight. Consulte [Uso de Pig y Hive con Factoría de datos][use-pig-hive] e [Invocación de programas de MapReduce desde Factoría de datos][run-map-reduce] para obtener más información. 
- La **actividad de puntuación por lotes de Aprendizaje automático de Azure** invoca la API de puntuación por lotes de Aprendizaje automático de Azure. Consulte [Creación de canalizaciones predictivas mediante la Factoría de datos de Azure y el Aprendizaje automático de Azure][azure-ml-adf] para obtener más información. 
- La **actividad de procedimiento almacenado** invoca un procedimiento almacenado en una base de datos SQL de Azure. Consulte la [actividad de procedimiento almacenado][msdn-stored-procedure-activity] en MSDN Library para obtener información detallada.   

##Segmento
Una tabla de una factoría de datos de Azure se compone de segmentos. El ancho de un segmento se determina por la programación: cada hora o diariamente. Cuando la programación es "cada hora", se produce un segmento cada hora con la hora de inicio y la hora de finalización de una canalización. Por ejemplo, si la fecha y hora de inicio de la canalización es 03/03/2015 06:00:00 (6 a.m.) y la fecha y hora de finalización es 03/03/2015 09:00:00 (9 a.m. del mismo día), se generan tres segmentos de datos, uno para cada intervalo de 1 hora: 6-7 a.m., 7-8 a.m. y 8-9 a.m.

Los segmentos ofrecen la posibilidad de trabajar con un subconjunto de datos globales durante un período de tiempo específico (por ejemplo, el segmento que se genera para la duración (hora): de las 13:00 a las 14:00 h).

## Ejecución de la actividad en un segmento
La **ejecución** o la ejecución de una actividad es una unidad de procesamiento de un segmento. Podría haber una o varias ejecuciones de un segmento en caso de reintentos o si vuelve a ejecutar el segmento en caso de error. Un segmento se identifica mediante su hora de inicio.

##Data Management Gateway
**Data Management Gateway** de Microsoft es un software que conecta orígenes de datos locales a servicios en la nube para su consumo. Debe tener al menos una puerta de enlace instalada en su entorno corporativo y registrarla con el portal de la Factoría de datos de Azure antes de agregar orígenes de datos locales como servicios vinculados.
 
##Centro de datos
Un **centro de datos** es un contenedor de servicios de almacenamiento y proceso de datos. Por ejemplo, un clúster de Hadoop con HDFS como almacenamiento y Hive o Pig como proceso (procesamiento) es un centro de datos. De forma similar, un almacenamiento de datos empresariales (EDW) se puede modelar como un centro de datos (base de datos como almacenamiento, procedimientos almacenados o herramienta ETL como servicios de proceso). Las canalizaciones utilizan almacenes de datos y se ejecutan en recursos de proceso en un centro de datos. En este momento solo se admite el centro de HDInsight.

El centro de datos permite la división de una factoría de datos en agrupaciones específicas lógicas o de dominio, como el "centro de Azure del oeste de EE. UU." que administra todos los servicios vinculados (almacenes de datos y proceso) y las canalizaciones centradas en el centro de datos del oeste de EE. UU., o el "centro de EDW de ventas" que administra todos los servicios vinculados y las canalizaciones relacionadas con el llenado y el procesamiento de datos para el almacén de datos empresariales de ventas.

Una característica importante de los centros es que una canalización se ejecuta en un único centro. Esto significa que al definir una canalización, todos los servicios vinculados a los que hacen referencia las tablas o las actividades dentro de esa canalización deben tener el mismo nombre de centro que la propia canalización. Si no se especifica la propiedad HubName para un servicio vinculado, el servicio vinculado se coloca en el centro "predeterminado".

## Otras referencias

1. [Introducción a la Factoría de datos de Azure][adf-intro]. Este artículo proporciona información general sobre el servicio Factoría de datos de Azure, el valor que proporciona y los escenarios que admite.
2. [Introducción a la Factoría de datos][datafactory-getstarted]. Este artículo ofrece un completo tutorial que le muestra cómo crear una factoría de datos de Azure de ejemplo que copia datos desde un blob de Azure hasta una base de datos SQL de Azure.


[Power-Query-Azure-Table]: http://office.microsoft.com/en-001/excel-help/connect-to-microsoft-azuretable-storage-HA104122607.aspx
[Power-Query-Azure-Blob]: http://office.microsoft.com/en-001/excel-help/connect-to-microsoft-azure-blob-storage-HA104113447.aspx
[Power-Query-Azure-SQL]: http://office.microsoft.com/en-001/excel-help/connect-to-a-microsoft-azure-sql-database-HA104019809.aspx
[Power-Query-OnPrem-SQL]: http://office.microsoft.com/en-001/excel-help/connect-to-a-sql-server-database-HA104019808.aspx

[adf-faq]: data-factory-faq.md
[adf-intro]: data-factory-introduction.md
[copy-data-with-adf]: data-factory-copy-activity.md
[use-pig-hive]: data-factory-pig-hive-activities.md
[run-map-reduce]: data-factory-map-reduce.md
[azure-ml-adf]: data-factory-create-predictive-pipelines.md
[adf-common-scenarios]: data-factory-common-scenarios.md
[create-factory-using-dotnet-sdk]: data-factory-create-data-factories-programmatically.md
[data-factory-editor]: data-factory-editor.md
[create-data-factory-using-powershell]: data-factory-monitor-manage-using-powershell.md

[adf-powershell-reference]: https://msdn.microsoft.com/library/dn820234.aspx


[msdn-stored-procedure-activity]: https://msdn.microsoft.com/library/dn912649.aspx
[msdn-class-library-reference]: https://msdn.microsoft.com/library/dn883654.aspx
[msdn-rest-api-reference]: https://msdn.microsoft.com/library/dn906738.aspx

[adf-tutorial]: data-factory-tutorial.md
[datafactory-getstarted]: data-factory-get-started.md

[image-data-factory-introduction-traditional-ETL]: ./media/data-factory-introduction/TraditionalETL.PNG

[image-data-factory-introduction-todays-diverse-processing-landspace]: ./media/data-factory-introduction/TodaysDiverseDataProcessingLandscape.PNG

[image-data-factory-application-model]: ./media/data-factory-introduction/DataFactoryApplicationModel.png

[image-data-factory-data-flow]: ./media/data-factory-introduction/DataFactoryDataFlow.png



 

<!---HONumber=August15_HO6-->