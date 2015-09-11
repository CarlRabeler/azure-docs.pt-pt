<properties
	pageTitle="Consultar datos desde el almacenamiento de blobs compatible con HDFS | Microsoft Azure"
	description="HDInsight usa el almacenamiento de blobs como el almacén de Big Data para HDFS. Obtenga información acerca de cómo realizar consultas de datos desde el almacenamiento de blobs y almacene los resultados de su análisis."
	keywords="blob storage,hdfs,structured data,unstructured data"
	services="hdinsight,storage"
	documentationCenter=""
	tags="azure-portal"
	authors="mumian"
	manager="paulettm"
	editor="cgronlun"/>

<tags
	ms.service="hdinsight"
	ms.workload="big-data"
	ms.tgt_pltfrm="na"
	ms.devlang="na"
	ms.topic="article"
	ms.date="07/28/2015"
	ms.author="jgao"/>


#Uso del almacenamiento de blobs de Azure compatibles con HDFS con Hadoop en HDInsight

En este tutorial, aprenda a usar el almacenamiento de blobs de Azure de bajo costo con HDInsight, a crear una cuenta de almacenamiento de Azure y un contenedor de almacenamiento de blobs y, a continuación, a tratar los datos que se encuentran dentro.

El almacenamiento de blobs de Azure es una solución de almacenamiento sólida y de uso general, que se integra sin problemas con HDInsight. A través de una interfaz del sistema de archivos distribuidos de Hadoop (HDFS), el conjunto completo de componentes de HDInsight puede operar directamente en datos estructurados o no estructurados en el almacenamiento de blobs.

Almacenar los datos en un almacenamiento de blobs hace que elimine de forma segura los clústeres de HDInsight que se usan para los cálculos sin perder los datos del usuario.

> [AZURE.NOTE]La sintaxis **asv://* no es compatible en los clústeres de HDInsight versión 3.0. Es decir, que los trabajos enviados a un clúster de HDInsight versión 3.0 y que usen explícitamente la sintaxis **asv://* tendrán errores. Debería usarse la sintaxis **wasb://* en lugar de la anterior. Además, los trabajos enviados a cualquier clúster de HDInsight versión 3.0 que se hayan creado con una tienda de metadatos existente que contenga referencias explícitas a recursos con la sintaxis asv:// tendrán errores. Estas tiendas de metadatos tendrán que volver a crearse usando la sintaxis wasb:// para recursos de dirección.

> Actualmente HDInsight solo es compatible con blobs en bloques.

> La mayoría de los comandos HDFS (por ejemplo, <b>ls</b>, <b>copyFromLocal</b> y <b>mkdir</b>) siguen funcionando según lo previsto. Únicamente los comandos específicos de la implementación nativa de HDFS (a la que nos referiremos como DFS), como <b>fschk</b> y <b>dfsadmin</b>, mostrarán comportamientos diferentes en el almacenamiento de blobs de Azure.

Para obtener información sobre el aprovisionamiento de un clúster de HDInsight, consulte [Introducción a HDInsight][hdinsight-get-started] o [Aprovisionamiento de clústeres de HDInsight][hdinsight-provision].


##<a id="architecture"></a>Arquitectura de almacenamiento de HDInsight
El diagrama siguiente proporciona una panorámica de la arquitectura de almacenamiento de HDInsight:

![Los clústeres de Hadoop usan la API de HDFS para acceder y almacenar datos estructurados y no estructurados en el almacenamiento de blobs.](./media/hdinsight-hadoop-use-blob-storage/HDI.WASB.Arch.png "Arquitectura de almacenamiento de HDInsight")

HDInsight brinda acceso al sistema de archivos distribuidos que se adjunta localmente a los nodos de ejecución. Se puede acceder a este sistema de archivos usando el URI completo, por ejemplo:

	hdfs://<namenodehost>/<path>

Además, HDInsight ofrece la capacidad de acceder a los datos almacenados en el almacenamiento de blobs de Azure. La sintaxis es:

	wasb[s]://<containername>@<accountname>.blob.core.windows.net/<path>


Hadoop admite una noción del sistema de archivos predeterminado. El sistema de archivos predeterminado implica una autoridad y un esquema predeterminados. También se puede usar para resolver rutas de acceso relativas. Durante el proceso de aprovisionamiento de HDInsight, se designan una cuenta de almacenamiento de Azure y un contenedor de almacenamiento de blobs de Azure específico de dicha cuenta como el sistema de archivos predeterminado.

Además de esta cuenta de almacenamiento, puede agregar más cuentas de almacenamiento desde la misma suscripción de Azure o desde otras diferentes durante el proceso de aprovisionamiento. Para obtener instrucciones sobre cómo agregar más cuentas de almacenamiento, consulte [Aprovisionamiento de clústeres de HDInsight][hdinsight-provision].

- **Contenedores de las cuentas de almacenamiento que se conectan a un clúster:** dado que el nombre y la clave de la cuenta se asocian al clúster durante el aprovisionamiento, tiene acceso total a los blobs de dichos contenedores.

- **Los contenedores o blobs públicos de las cuentas de almacenamiento que NO están conectados a un clúster:** tiene permiso de solo lectura a los blobs de los contenedores.

	> [AZURE.NOTE]> Los contenedor públicos le permiten obtener una lista de todos los blobs disponibles del contenedor en cuestión y obtener sus metadatos. Los blobs públicos le permiten acceder a los blobs solo si conoce la URL exacta. Para obtener más información, consulte <a href="http://msdn.microsoft.com/library/windowsazure/dd179354.aspx">Restringir acceso a contenedores y blobs</a>.

- **Los contenedores privados de las cuentas de almacenamiento que no están conectados a un clúster:** no puede tener acceso a los blobs de los contenedores a menos que defina la cuenta de almacenamiento al enviar los trabajos de WebHCat. Esto se explica posteriormente en este artículo.


Las cuentas de almacenamiento definidas en el proceso de aprovisionamiento y sus claves se almacenan en %HADOOP\_HOME%/conf/core-site.xml del nodos de clúster. El comportamiento predeterminado de HDInsight es usar las cuentas de almacenamiento definidas en el archivo core-site.xml. No es recomendable editar el archivo core-site.xml porque se puede volver a crear una imagen del nodo principal (maestro) del clúster o dicho nodo se puede migrar en cualquier momento, por lo que se perderá cualquier cambio en los archivos.

Varios trabajos de WebHCat, incluidos Hive, MapReduce, streaming de Hadoop y Pig, pueden llevar una descripción de cuentas de almacenamiento y metadatos con ellos. (Actualmente esto funciona para Pig con cuentas de almacenamiento pero no para metadatos). En la sección [Acceso a blobs usando PowerShell](#powershell) de este artículo, hay una muestra de esta característica. Para obtener más información, consulte [Uso de un clúster de HDInsight con cuentas de almacenamiento y tiendas de metadatos alternativas](http://social.technet.microsoft.com/wiki/contents/articles/23256.using-an-hdinsight-cluster-with-alternate-storage-accounts-and-metastores.aspx).

El almacenamiento de blobs puede usarse para datos estructurados y no estructurados. Los contenedores del almacenamiento de blobs almacenan los datos como pares de clave/valor y no hay jerarquía de directorios. No obstante, el carácter de barra diagonal ( / ) se puede usar en el nombre de la clave para que parezca que el archivo está almacenado dentro de una estructura de directorios. Por ejemplo, la clave de un blob puede ser *input/log1.txt*. No hay directorios *input*, pero dada la presencia del carácter de barra diagonal en el nombre de la clave, parece la ruta de un archivo.

###<a id="benefits"></a>Ventajas del almacenamiento de blobs
El coste de rendimiento implícito por no tener ubicados juntos los recursos de almacenamiento y clústeres de proceso se ve mitigado por el modo en que los clústeres de proceso se aprovisionan cerca de los recursos de la cuenta de almacenamiento, dentro del centro de datos de Azure, donde la red de alta velocidad consigue que los nodos de proceso sean muy eficientes en el acceso a los datos del almacenamiento de blobs de Azure.

Hay varias ventajas asociadas al almacenamiento de datos en el almacenamiento de blobs de Azure en lugar de utilizar HDFS:

* **Reutilización de uso compartido de datos:** los datos de HDFS se ubican dentro del clúster de cálculo. Solamente las aplicaciones que tengan acceso al clúster de cálculo podrán usar los datos usando las API HDFS. Se puede acceder a los datos del almacenamiento de blobs de Azure a través de las API HDFS o las [API REST de almacenamiento de blobs][blob-storage-restAPI]. Por lo tanto, se puede usar un conjunto mayor de aplicaciones (incluyendo otros clústeres de HDInsight) y herramientas para producir y consumir los datos.
* **Archivo de datos:** almacenar los datos en un almacenamiento de blobs de Azure hace que los clústeres de HDInsight que se usan para los cálculos se eliminen de forma segura sin perder los datos del usuario.
* **Coste de almacenamiento de datos:** almacenar datos en DFS es más caro a largo plazo que almacenarlos en el almacenamiento de blobs de Azure, ya que el coste de un clúster de cálculo es superior al de un contenedor de almacenamiento de blobs de Azure. Además, como no hay que volver a cargar los datos para cada generación de clúster de cálculo, también se ahorra en costes de carga de datos.
* **Escalada horizontal elástica:** aunque HDFS proporciona un sistema de archivos escalable en horizontal, la escala se determina en función del número de nodos que aprovisione para su clúster. Cambiar la escala puede ser un proceso más complicado que basarse en las capacidades de escalada elástica que tiene automáticamente en el almacenamiento de blobs de Azure.
* **Replicación geográfica:** sus contenedores de almacenamiento de blobs se pueden replicar geográficamente. Aunque esto le aporta recuperación geográfica y redundancia de datos, una conmutación por error en la ubicación replicada geográficamente afecta gravemente a su rendimiento y puede incurrir en costes adicionales. Por lo tanto, nuestra recomendación es que elija la replicación geográfica de forma inteligente y únicamente si merece la pena pagar el coste adicional por el valor de los datos.

Determinados trabajos y paquetes de MapReduce podrían crear resultados intermedios que realmente no desea almacenar en el almacenamiento de blobs de Azure. En tal caso, puede optar por almacenar los datos en el HDFS local. De hecho, HDInsight usa DFS para varios de estos resultados intermedios en los trabajos de Hive y otros procesos.



##<a id="preparingblobstorage"></a>Creación de un contenedor de blobs

Para usar blobs, en primer lugar debe crear una [cuenta de almacenamiento de Azure][azure-storage-create]. Como parte de este proceso, debe especificar un centro de datos de Azure que almacenará los objetos que cree con esta cuenta. El clúster y la cuenta de almacenamiento deben ubicarse en el mismo centro de datos. La base de datos de SQL Server de la tienda de metadatos de Hive y la base de datos de SQL Server de la tienda de metadatos de Oozie también deben encontrarse en el mismo centro de datos.

Cualquiera que sea su ubicación, todos los blobs que cree pertenecerán a un contenedor de su cuenta de almacenamiento de Azure. Este contenedor puede ser un blob existente creado fuera de HDInsight, o bien un contenedor que se crea para un clúster de HDInsight.

No comparta un contenedor de almacenamiento predeterminado con varios clústeres de HDInsight. Si necesita usar un contenedor compartido para proporcionar acceso a datos para varios clústeres de HDInsight, debe agregarlo como una cuenta de almacenamiento adicional en la configuración del clúster. Para obtener más información, consulte [Aprovisionamiento de clústeres de HDInsight][hdinsight-provision]. Sin embargo, puede volver a usar un contenedor de almacenamiento predeterminado después de que se haya eliminado el clúster de HDInsight original. Para clústeres de HBase, puede conservar realmente el esquema de la tabla HBase y los datos aprovisionando un nuevo clúster de HBase mediante el contenedor de almacenamiento de blobs predeterminado que se usa por un clúster de HBase que se ha eliminado.


###Usando el portal de vista previa de Azure

Al aprovisionar un clúster de HDInsight desde el portal de vista previa, tiene las opciones para usar una cuenta de almacenamiento existente o crear una nueva cuenta de almacenamiento:

![origen de datos de aprovisionamiento de hadoop de hdinsight](./media/hdinsight-hadoop-use-blob-storage/hdinsight.provision.data.source.png)

###Uso de la CLI de Azure

Si ha [instalado y configurado la CLI Azure](../xplat-cli.md), se puede usar el comando siguiente para una cuenta de almacenamiento y contenedor.

	azure storage account create <storageaccountname> --type LRS

> [AZURE.NOTE]El parámetro `--type` indica cómo se replicará la cuenta de almacenamiento. Para obtener más información, vea[Replicación de almacenamiento de Azure](../storage-redundancy.md).

Se le pedirá que especifique la región geográfica en la que se encontrará la cuenta de almacenamiento. Debe crear la cuenta de almacenamiento en la misma región en la que planea crear el clúster de HDInsight.

Cuando se haya creado la cuenta de almacenamiento, use el siguiente comando para recuperar las claves de la cuenta de almacenamiento:

	azure storage account keys list <storageaccountname>

Para crear un contenedor, use el comando siguiente:

	azure storage container create <containername> --account-name <storageaccountname> --account-key <storageaccountkey>

###Uso de Azure PowerShell

Si ha [instalado y configurado Azure PowerShell][powershell-install], puede usar lo siguiente desde el símbolo del sistema de PowerShell de Azure para crear una cuenta de almacenamiento y contenedor:

	$subscriptionName = "<SubscriptionName>"	# Azure subscription name
	$storageAccountName = "<AzureStorageAccountName>" # The storage account that you will create
	$containerName="<BlobContainerToBeCreated>" # The Blob container name that you will create

	# Connect to your Azure account and selec the current subscription
	Add-AzureAccount # The connection will expire in 12 hours.
	Select-AzureSubscription $subscriptionName #only required if you have multiple subscriptions

	# Create a storage context object
	$storageAccountkey = get-azurestoragekey $storageAccountName | %{$_.Primary}
	$destContext = New-AzureStorageContext -StorageAccountName $storageAccountName -StorageAccountKey $storageAccountKey  

	# Create a Blob storage container
	New-AzureStorageContainer -Name $containerName -Context $destContext


##<a id="addressing"></a>Archivos de dirección en almacenamiento de blobs

El esquema URI para obtener acceso a los archivos del almacenamiento de blobs desde HDInsight es:

	wasb[s]://<BlobStorageContainerName>@<StorageAccountName>.blob.core.windows.net/<path>


> [AZURE.NOTE]La sintaxis para enviar los archivos al emulador de almacenamiento (que se ejecuta en el emulador de HDInsight) es <i>wasb://&lt;ContainerName&gt;@storageemulator</i>.



El esquema URI proporciona acceso sin cifrar (con el prefijo *wasb:*) y acceso cifrado SSL con *wasbs*). Se recomienda usar *wasbs* siempre que sea posible, incluso al acceder a los datos que se encuentran en el mismo centro de datos de Azure.

El valor de &lt;BlobStorageContainerName&gt; identifica el nombre del contenedor de almacenamiento de blobs de Azure. El valor de &lt;StorageAccountName&gt; identifica el nombre de la cuenta de almacenamiento de Azure. Se necesita el nombre completo de dominio (FQDN).

Si no se ha especificado ningún valor en &lt;BlobStorageContainerName&gt; ni en &lt;StorageAccountName&gt;, se usará el sistema de archivos predeterminado. Para los archivos del sistema de archivos predeterminado, puede usar una ruta relativa o absoluta. Por ejemplo, se puede hacer referencia al archivo *hadoop-mapreduce-examples.jar* que se incluye con los clústeres de HDInsight usando uno de los siguientes:

	wasb://mycontainer@myaccount.blob.core.windows.net/example/jars/hadoop-mapreduce-examples.jar
	wasb:///example/jars/hadoop-mapreduce-examples.jar
	/example/jars/hadoop-mapreduce-examples.jar

> [AZURE.NOTE]El nombre del archivo es <i>hadoop-examples.jar</i> en las versiones 2.1 y 1.6 de los clústeres de HDInsight.


La &lt;ruta&gt; es el nombre de la ruta HDFS del archivo o el directorio. Dado que los contenedores del almacenamiento de blobs de Azure son solamente almacenes de pares clave-valor, no hay ningún sistema de archivos jerárquico real. Un carácter de barra inclinada ( / ) dentro de la clave de blob se interpreta como separador de directorios. Por ejemplo, el nombre del blob para *hadoop-mapreduce-examples.jar* es:

	example/jars/hadoop-mapreduce-examples.jar

> [AZURE.NOTE]Al trabajar con blobs fuera de HDInsight, la mayoría de las utilidades no reconocen el formato WASB y en su lugar, espera un formato básico de ruta de acceso, como `example/jars/hadoop-mapreduce-examples.jar`.

##<a id="azurecli"></a>Acceder a blobs con CLI de Azure

Use el siguiente comando para mostrar los cmdlets relacionados con blobs:

	azure storage blob

**Ejemplo de uso de CLI de Azure para cargar un archivo**

	azure storage blob upload <sourcefilename> <containername> <blobname> --account-name <storageaccountname> --account-key <storageaccountkey>

**Ejemplo de uso de CLI de Azure para descargar un archivo**

	azure storage blob download <containername> <blobname> <destinationfilename> --account-name <storageaccountname> --account-key <storageaccountkey>

**Ejemplo de uso de CLI de Azure para eliminar un archivo**

	azure storage blob delete <containername> <blobname> --account-name <storageaccountname> --account-key <storageaccountkey>

**Ejemplo de uso de CLI de Azure para mostrar archivos**

	azure storage blob list <containername> <blobname|prefix> --account-name <storageaccountname> --account-key <storageaccountkey>

##<a id="powershell"></a>Acceder a blobs con Azure PowerShell

> [AZURE.NOTE]Los comandos de esta sección proporcionan un ejemplo básico de uso de PowerShell para acceder a los datos almacenados en blobs. Para obtener un ejemplo más completo que se personaliza para trabajar con HDInsight, vea las [Herramientas de HDInsight](https://github.com/Blackmist/hdinsight-tools).

Utilice el comando siguiente para incluir los cmdlets relacionados con el blob:

	Get-Command *blob*

![Lista de cmdlets de PowerShell relacionados con el blob.][img-hdi-powershell-blobcommands]


**Ejemplo de uso de Azure PowerShell para cargar un archivo**

Consulte [Carga de datos en HDInsight][hdinsight-upload-data].

**Ejemplo de uso de Azure PowerShell para descargar un archivo**

El script siguiente descarga un blob en bloques a la carpeta actual. Antes de ejecutar el script, cambie el directorio a una carpeta en la que tenga permisos de escritura.


	$storageAccountName = "<AzureStorageAccountName>"   # The storage account used for the default file system specified at provision.
	$containerName = "<BlobStorageContainerName>"  # The default file system container has the same name as the cluster.
	$blob = "example/data/sample.log" # The name of the blob to be downloaded.

	# Use Add-AzureAccount if you haven't connected to your Azure subscription
	#Add-AzureAccount # The connection is good for 12 hours

	# Use these two commands if you have multiple subscriptions
	#$subscriptionName = "<SubscriptionName>"
	#Select-AzureSubscription $subscriptionName

	Write-Host "Create a context object ... " -ForegroundColor Green
	$storageAccountKey = Get-AzureStorageKey $storageAccountName | %{ $_.Primary }
	$storageContext = New-AzureStorageContext -StorageAccountName $storageAccountName -StorageAccountKey $storageAccountKey  

	Write-Host "Download the blob ..." -ForegroundColor Green
	Get-AzureStorageBlobContent -Container $ContainerName -Blob $blob -Context $storageContext -Force

	Write-Host "List the downloaded file ..." -ForegroundColor Green
	cat "./$blob"

**Ejemplo de uso de Azure PowerShell para eliminar un archivo**

En el siguiente script se muestra cómo eliminar un archivo.

	$storageAccountName = "<AzureStorageAccountName>"   # The storage account used for the default file system specified at provision.
	$containerName = "<BlobStorageContainerName>"  # The default file system container has the same name as the cluster.
	$blob = "example/data/sample.log" # The name of the blob to be downloaded.

	# Use Add-AzureAccount if you haven't connected to your Azure subscription
	#Add-AzureAccount # The connection is good for 12 hours

	# Use these two commands if you have multiple subscriptions
	#$subscriptionName = "<SubscriptionName>"
	#Select-AzureSubscription $subscriptionName

	Write-Host "Create a context object ... " -ForegroundColor Green
	$storageAccountKey = Get-AzureStorageKey $storageAccountName | %{ $_.Primary }
	$storageContext = New-AzureStorageContext -StorageAccountName $storageAccountName -StorageAccountKey $storageAccountKey  

	Write-Host "Delete the blob ..." -ForegroundColor Green
	Remove-AzureStorageBlob -Container $containerName -Context $storageContext -blob $blob


**Ejemplo de uso de Azure PowerShell para enumerar archivos en una carpeta**

El siguiente script muestra cómo enumerar archivos dentro de una carpeta. (En el siguiente ejemplo puede ver cómo usar el cmdlet **Invoke-Hive** para ejecutar el comando **dfs ls** para enumerar una carpeta).

	$storageAccountName = "<AzureStorageAccountName>"   # The storage account used for the default file system specified at provision.
	$containerName = "<BlobStorageContainerName>"  # The default file system container has the same name as the cluster.
	$blobPrefix = "example/data/"

	# Use Add-AzureAccount if you haven't connected to your Azure subscription
	#Add-AzureAccount # The connection is good for 12 hours

	# Use these two commands if you have multiple subscriptions
	#$subscriptionName = "<SubscriptionName>"
	#Select-AzureSubscription $subscriptionName

	Write-Host "Create a context object ... " -ForegroundColor Green
	$storageAccountKey = Get-AzureStorageKey $storageAccountName | %{ $_.Primary }
	$storageContext = New-AzureStorageContext -StorageAccountName $storageAccountName -StorageAccountKey $storageAccountKey  

	Write-Host "List the files in $blobPrefix ..."
	Get-AzureStorageBlob -Container $containerName -Context $storageContext -prefix $blobPrefix

**Ejemplo de uso de Azure PowerShell para ejecutar una consulta de Hive con una cuenta de almacenamiento sin definir**

En este ejemplo se muestra cómo enumerar una carpeta desde la cuenta de almacenamiento no definida durante el proceso de aprovisionamiento. $clusterName = "<HDInsightClusterName>"

	$undefinedStorageAccount = "<UnboundedStorageAccountUnderTheSameSubscription>"
	$undefinedContainer = "<UnboundedBlobContainerAssociatedWithTheStorageAccount>"

	$undefinedStorageKey = Get-AzureStorageKey $undefinedStorageAccount | %{ $_.Primary }

	Use-AzureHDInsightCluster $clusterName

	$defines = @{}
	$defines.Add("fs.azure.account.key.$undefinedStorageAccount.blob.core.windows.net", $undefinedStorageKey)

	Invoke-Hive -Defines $defines -Query "dfs -ls wasb://$undefinedContainer@$undefinedStorageAccount.blob.core.windows.net/;"

##<a id="nextsteps"></a>Pasos siguientes

En este artículo, ha aprendido a usar el almacenamiento de blobs de Azure compatible con HDFS con HDInsight y que el almacenamiento de blobs de Azure es un componente fundamental de HDInsight. Esto le permite crear soluciones de adquisición de datos de archivado escalable y a largo plazo con el almacenamiento de blobs de Azure; además de usar HDInsight para desbloquear la información que hay dentro de los datos estructurados y no estructurados almacenados.

Para obtener más información, consulte los artículos siguientes:

* [Introducción a HDInsight de Azure][hdinsight-get-started]
* [Carga de datos en HDInsight][hdinsight-upload-data]
* [Uso de Hive con HDInsight][hdinsight-use-hive]
* [Uso de Pig con HDInsight][hdinsight-use-pig]

[powershell-install]: ../install-configure-powershell.md
[hdinsight-provision]: hdinsight-provision-clusters.md
[hdinsight-get-started]: hdinsight-hadoop-tutorial-get-started-windows.md
[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-use-hive]: hdinsight-use-hive.md
[hdinsight-use-pig]: hdinsight-use-pig.md

[blob-storage-restAPI]: http://msdn.microsoft.com/library/windowsazure/dd135733.aspx
[azure-storage-create]: ../storage-create-storage-account.md

[img-hdi-powershell-blobcommands]: ./media/hdinsight-hadoop-use-blob-storage/HDI.PowerShell.BlobCommands.png
[img-hdi-quick-create]: ./media/hdinsight-hadoop-use-blob-storage/HDI.QuickCreateCluster.png
[img-hdi-custom-create-storage-account]: ./media/hdinsight-hadoop-use-blob-storage/HDI.CustomCreateStorageAccount.png

<!---HONumber=August15_HO8-->