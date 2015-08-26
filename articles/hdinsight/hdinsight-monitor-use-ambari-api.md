<properties
	pageTitle="Supervisión de clústeres de Hadoop en HDInsight con la API de Ambari | Microsoft Azure"
	description="Use las API de Apache Ambari para el aprovisionamiento, la administración y la supervisión de clústeres de Hadoop. Las API y herramientas de operador intuitivas ocultan la complejidad de Hadoop."
	services="hdinsight"
	documentationCenter=""
	authors="mumian"
	editor="cgronlun"
	manager="paulettm"/>

<tags
	ms.service="hdinsight"
	ms.workload="big-data"
	ms.tgt_pltfrm="na"
	ms.devlang="na"
	ms.topic="article"
	ms.date="06/08/2015"
	ms.author="jgao"/>

# Supervisión de clústeres de Hadoop en HDInsight con la API de Ambari

Aprenda a supervisar los clústeres de HDInsight de las versiones 3.1 y 2.1 con las API de Ambari.

> [AZURE.NOTE]La información de este artículo es principalmente para los clústeres de HDInsight basados en Windows, que proporcionan una versión de solo lectura de la API de REST de Ambari. Para los clústeres basados en Linux, vea [Administrar clústeres de Hadoop mediante Ambari](hdinsight-hadoop-manage-ambari.md).

## <a id="whatisambari"></a> ¿Qué es Ambari?

[Apache Ambari][ambari-home] sirve para el aprovisionamiento, la administración y la supervisión de clústeres de Hadoop de Apache. Incluye una recopilación intuitiva de herramientas de operador y un conjunto sólido de API que ocultan la complejidad de Hadoop y simplifican la operación de clústeres. Para obtener más información sobre las API, consulte [Referencia de API de Ambari][ambari-api-reference].


HDInsight actualmente solo es compatible con la característica de supervisión de Ambari. La API de Ambari v1.1.0 es compatible con los clústeres de las versiones 3.0 y 2.1 de HDInsight. Este artículo abarca el acceso a las API de Ambari en clústeres de las versiones 3.1 y 2.1 de HDInsight. La principal diferencia entre las dos es que algunos componentes han cambiado con la incorporación de nuevas funciones (como el servidor de historial de trabajos).


##<a id="prerequisites"></a>Requisitos previos

Antes de empezar este tutorial, debe contar con lo siguiente:

- **Una estación de trabajo con Azure PowerShell**. Vea [Instalar y usar Azure PowerShell](http://azure.microsoft.com/documentation/videos/install-and-use-azure-powershell/).


- (Opcional) [cURL][curl]. Para instalarlo, consulte [Descargas y versiones de cURL][curl-download].

	>[AZURE.NOTE]Cuando utilice el comando cURL en Windows, utilice comillas dobles en lugar de comillas simples para los valores de opción.

- **Un clúster de HDInsight de Azure**. Para obtener instrucciones acerca del aprovisionamiento del clúster, consulte [Introducción al uso de HDInsight][hdinsight-get-started] o [Aprovisionamiento de clústeres de HDInsight][hdinsight-provision]. Para completar el tutorial, necesitará los datos siguientes:

	<table border="1">
	<tr><th>Propiedad del clúster</th><th>Nombre de variable de Azure PowerShell</th><th>Valor</th><th>Descripción</th></tr>
	<tr><td>Nombre del clúster de HDInsight</td><td>$clusterName</td><td></td><td>El nombre del clúster de HDInsight.</td></tr>
	<tr><td>Nombre de usuario del clúster</td><td>$clusterUsername</td><td></td><td>Nombre de usuario del clúster especificado en el aprovisionamiento.</td></tr>
	<tr><td>Contraseña de clúster</td><td>$clusterPassword</td><td></td><td>Contraseña de usuario de clúster</td></tr>
	</table>

	> [AZURE.NOTE] Rellene los valores de la tabla. Esto le resultará útil para completar el tutorial.



##<a id="jumpstart"></a>Inicio de la aplicación

Existen varias maneras de usar Ambari para supervisar clústeres de HDInsight.

**Uso de Azure PowerShell**

El siguiente es un script de Azure PowerShell para obtener la información del seguimiento de trabajos de MapReduce *en un clúster de HDInsight 3.1.* La principal diferencia aquí es que extraemos estos detalles del servicio YARN (en lugar de MapReduce).

	$clusterName = "<HDInsightClusterName>"
	$clusterUsername = "<HDInsightClusterUsername>"
	$clusterPassword = "<HDInsightClusterPassword>"

	$ambariUri = "https://$clusterName.azurehdinsight.net:443/ambari"
	$uriJobTracker = "$ambariUri/api/v1/clusters/$clusterName.azurehdinsight.net/services/yarn/components/resourcemanager"

	$passwd = ConvertTo-SecureString $clusterPassword -AsPlainText -Force
	$creds = New-Object System.Management.Automation.PSCredential ($clusterUsername, $passwd)

	$response = Invoke-RestMethod -Method Get -Uri $uriJobTracker -Credential $creds -OutVariable $OozieServerStatus

	$response.metrics.'yarn.queueMetrics'

El siguiente es un script de Azure PowerShell para obtener la información del seguimiento de trabajos de MapReduce *en un clúster de HDInsight 2.1*:

	$clusterName = "<HDInsightClusterName>"
	$clusterUsername = "<HDInsightClusterUsername>"
	$clusterPassword = "<HDInsightClusterPassword>"

	$ambariUri = "https://$clusterName.azurehdinsight.net:443/ambari"
	$uriJobTracker = "$ambariUri/api/v1/clusters/$clusterName.azurehdinsight.net/services/mapreduce/components/jobtracker"

	$passwd = ConvertTo-SecureString $clusterPassword -AsPlainText -Force
	$creds = New-Object System.Management.Automation.PSCredential ($clusterUsername, $passwd)

	$response = Invoke-RestMethod -Method Get -Uri $uriJobTracker -Credential $creds -OutVariable $OozieServerStatus

	$response.metrics.'mapred.JobTracker'

La salida es la siguiente:

![Salida de seguimiento de trabajo][img-jobtracker-output]

**Uso de cURL**

El siguiente es un ejemplo de cómo obtener información de clústeres con cURL:

	curl -u <username>:<password> -k https://<ClusterName>.azurehdinsight.net:443/ambari/api/v1/clusters/<ClusterName>.azurehdinsight.net

La salida es la siguiente:

	{"href":"https://hdi0211v2.azurehdinsight.net/ambari/api/v1/clusters/hdi0211v2.azurehdinsight.net/",
	 "Clusters":{"cluster_name":"hdi0211v2.azurehdinsight.net","version":"2.1.3.0.432823"},
	 "services"[
	   {"href":"https://hdi0211v2.azurehdinsight.net/ambari/api/v1/clusters/hdi0211v2.azurehdinsight.net/services/hdfs",
	    "ServiceInfo":{"cluster_name":"hdi0211v2.azurehdinsight.net","service_name":"hdfs"}},
	   {"href":"https://hdi0211v2.azurehdinsight.net/ambari/api/v1/clusters/hdi0211v2.azurehdinsight.net/services/mapreduce",
	    "ServiceInfo":{"cluster_name":"hdi0211v2.azurehdinsight.net","service_name":"mapreduce"}}],
	 "hosts":[
	   {"href":"https://hdi0211v2.azurehdinsight.net/ambari/api/v1/clusters/hdi0211v2.azurehdinsight.net/hosts/headnode0",
	    "Hosts":{"cluster_name":"hdi0211v2.azurehdinsight.net",
	             "host_name":"headnode0"}},
	   {"href":"https://hdi0211v2.azurehdinsight.net/ambari/api/v1/clusters/hdi0211v2.azurehdinsight.net/hosts/workernode0",
	    "Hosts":{"cluster_name":"hdi0211v2.azurehdinsight.net",
	             "host_name":"headnode0.{ClusterDNS}.azurehdinsight.net"}}]}

**Para la versión del 10/8/2014**:

Cuando se usa el extremo Ambari, "https://{clusterDns}.azurehdinsight.net/ambari/api/v1/clusters/{clusterDns}.azurehdinsight.net/services/{servicename}/components/{componentname}", el campo *host_name* devuelve ahora el nombre de dominio completo (FQDN) del nodo en lugar de solo el nombre del host. Antes de la versión del 10/8/2014, este ejemplo devolvía simplemente "**headnode0**". Después de la versión 10/8/2014, se obtiene el FQDN "**headnode0.{ClusterDNS}.azurehdinsight.net**", como se muestra en el ejemplo anterior. Este cambio se solicitó para facilitar escenarios donde se pueden implementar varios tipos de clúster (como HBase y Hadoop) en una misma red virtual (VNET). Esto ocurre, por ejemplo, cuando se usa HBase como plataforma de back-end para Hadoop.

##<a id="monitor"></a>API de supervisión de Ambari

En la siguiente lista se enumeran algunas de las llamadas a API desde la supervisión de Ambari. Para obtener más información sobre la API, consulte [Referencia de API de Ambari][ambari-api-reference].

<table border="1">
<tr><th>Supervisión de la llamada a la API</th><th>URI</th><th>Descripción</th></tr>
<tr><td>Obtener clústeres</td><td><tt>/api/v1/clusters</tt></td><td></td></tr>
<tr><td>Obtener información de clúster.</td><td><tt>/api/v1/clusters/&lt;ClusterName>.azurehdinsight.net</tt></td><td>clústeres, servicios, hosts</td></tr>
<tr><td>Obtener servicios</td><td><tt>/api/v1/clusters/&lt;ClusterName>.azurehdinsight.net/services</tt></td><td>Los servicios incluyen: hdfs, mapreduce</td></tr>
<tr><td>Obtener información sobre los servicios.</td><td><tt>/api/v1/clusters/&lt;ClusterName>.azurehdinsight.net/services/&lt;ServiceName></tt></td><td></td></tr>
<tr><td>Obtener componentes de servicio</td><td><tt>/api/v1/clusters/&lt;ClusterName>.azurehdinsight.net/services/&lt;ServiceName>/components</tt></td><td>HDFS: namenode, datanode<br/>MapReduce: jobtracker; tasktracker</td></tr>
<tr><td>Obtener información de componente.</td><td><tt>/api/v1/clusters/&lt;ClusterName>.azurehdinsight.net/services/&lt;ServiceName>/components/&lt;ComponentName></tt></td><td>ServiceComponentInfo, componentes de host, métricas</td></tr>
<tr><td>Obtener hosts</td><td><tt>/api/v1/clusters/&lt;ClusterName>.azurehdinsight.net/hosts</tt></td><td>headnode0, workernode0</td></tr>
<tr><td>Obtener información sobre host.</td><td><tt>/api/v1/clusters/&lt;ClusterName>.azurehdinsight.net/hosts/&lt;HostName>
</td><td></td></tr>
<tr><td>Obtener componentes de host</td><td><tt>/api/v1/clusters/&lt;ClusterName>.azurehdinsight.net/hosts/&lt;HostName>/host_components </tt></td><td>namenode, resourcemanager</td></tr>
<tr><td>Obtener información de componente de host</td><td><tt>/api/v1/clusters/&lt;ClusterName>.azurehdinsight.net/hosts/&lt;HostName>/host_components/&lt;ComponentName> </tt></td><td>HostRoles, componente, host, métricas</td></tr>
<tr><td>Obtener configuraciones</td><td><tt>/api/v1/clusters/&lt;ClusterName>.azurehdinsight.net/configurations </tt></td><td>Tipos de configuración: core-site, hdfs-site, mapred-site, hive-site</td></tr>
<tr><td>Obtener información de configuración</td><td><tt>/api/v1/clusters/&lt;ClusterName>.azurehdinsight.net/configurations?type=&lt;ConfigType>&amp;tag=&lt;VersionName> </tt></td><td>Tipos de configuración: core-site, hdfs-site, mapred-site, hive-site</td></tr>
</table>


##<a id="nextsteps"></a>Pasos siguientes

Ahora sabe cómo usar las llamadas de API de supervisión de Ambari. Para obtener más información, consulte:

- [Administrar clústeres de HDInsight con el Portal de administración][hdinsight-admin-portal]
- [Administrar clústeres de HDInsight con Azure PowerShell][hdinsight-admin-powershell]
- [Administrar clústeres de HDInsight con la interfaz de línea de comandos][hdinsight-admin-cli]
- [Documentación de HDInsight][hdinsight-documentation]
- [Introducción a HDInsight][hdinsight-get-started]



[ambari-home]: http://ambari.apache.org/
[ambari-api-reference]: https://github.com/apache/ambari/blob/trunk/ambari-server/docs/api/v1/index.md

[curl]: http://curl.haxx.se
[curl-download]: http://curl.haxx.se/download.html

[microsoft-hadoop-SDK]: http://hadoopsdk.codeplex.com/wikipage?title=Ambari%20Monitoring%20Client

[powershell-install]: ../install-configure-powershell.md
[powershell-script]: http://technet.microsoft.com/library/ee176949.aspx

[hdinsight-admin-powershell]: hdinsight-administer-use-powershell.md
[hdinsight-admin-portal]: hdinsight-administer-use-management-portal.md
[hdinsight-admin-cli]: hdinsight-administer-use-command-line.md
[hdinsight-documentation]: /documentation/services/hdinsight/
[hdinsight-get-started]: ../hdinsight-get-started.md
[hdinsight-provision]: hdinsight-provision-clusters.md

[img-jobtracker-output]: ./media/hdinsight-monitor-use-ambari-api/hdi.ambari.monitor.jobtracker.output.png
 

<!---HONumber=August15_HO6-->