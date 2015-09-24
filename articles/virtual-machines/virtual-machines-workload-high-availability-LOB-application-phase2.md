<properties 
	pageTitle="Fase 2 de la carga de trabajo de aplicación de línea de negocio: Configuración de controladores de dominio" 
	description="En esta segunda fase, creará y configurará los dos controladores de dominio de Active Directory." 
	documentationCenter=""
	services="virtual-machines" 
	authors="JoeDavies-MSFT" 
	manager="timlt" 
	editor=""
	tags="azure-resource-manager"/>

<tags 
	ms.service="virtual-machines" 
	ms.workload="infrastructure-services" 
	ms.tgt_pltfrm="na" 
	ms.devlang="na" 
	ms.topic="article" 
	ms.date="08/11/2015" 
	ms.author="josephd"/>

# Fase 2 de la carga de trabajo de aplicación de línea de negocio: Configuración de controladores de dominio

En esta fase de la implementación de una aplicación de línea de negocio de alta disponibilidad en servicios de infraestructura de Azure, configurará dos controladores de dominio de réplica en la red virtual de Azure para que las solicitudes web de cliente de recursos web se puedan autenticar localmente en la red virtual de Azure, en lugar de enviar ese tráfico de autenticación a través de la conexión con la red local.

Debe completar esta fase antes de pasar a la [fase 3](virtual-machines-workload-high-availability-LOB-application-phase3.md). Vea [Implementación de una aplicación de línea de negocio de alta disponibilidad en Azure](virtual-machines-workload-high-availability-LOB-application-overview.md) en todas las fases.

## Creación de máquinas virtuales de controlador de dominio en Azure

En primer lugar, se debe rellenar la columna **Nombre de máquina Virtual** de la Tabla M y modificar los tamaños de máquina virtual según sea necesario en la columna **Tamaño mínimo**.

Elemento | Nombre de la máquina virtual | Imagen de la Galería | Tamaño mínimo 
--- | --- | --- | --- 
1\. | \_\_\_\_\_\_\_\_\_\_\_\_\_\_ (primer controlador de dominio, ejemplo DC1) | Windows Server 2012 R2 Datacenter | Standard\_D1
2\. | \_\_\_\_\_\_\_\_\_\_\_\_\_\_ (segundo controlador de dominio, ejemplo DC2) | Windows Server 2012 R2 Datacenter | Standard\_D1
3\. | \_\_\_\_\_\_\_\_\_\_\_\_\_\_ (servidor de base de datos principal, ejemplo SQL1) | Microsoft SQL Server 2014 Enterprise – Windows Server 2012 R2 | 	Standard\_DS4
4\. | \_\_\_\_\_\_\_\_\_\_\_\_\_\_ (servidor de base de datos secundario, ejemplo SQL2) | Microsoft SQL Server 2014 Enterprise – Windows Server 2012 R2 | 	Standard\_DS4
5\. | \_\_\_\_\_\_\_\_\_\_\_\_\_\_ (testigo de nodos de mayoría para el clúster, ejemplo MN1) | Windows Server 2012 R2 Datacenter | Standard\_D1
6\. | \_\_\_\_\_\_\_\_\_\_\_\_\_\_ (primer servidor web, ejemplo WEB1) | Windows Server 2012 R2 Datacenter | Standard\_D3
7\. | \_\_\_\_\_\_\_\_\_\_\_\_\_\_ (segundo servidor web, ejemplo WEB2) | Windows Server 2012 R2 Datacenter | Standard\_D3

**Table M: Máquinas virtuales para la aplicación de línea de negocio de alta disponibilidad en Azure**

Para obtener una lista completa de tamaños de máquinas virtuales, consulte [Tamaños de máquinas virtuales y servicios en la nube de Azure](https://msdn.microsoft.com/library/azure/dn197896.aspx).

Use el siguiente bloque de comandos de Azure PowerShell para crear las máquinas virtuales para los dos controladores de dominio. Especifique los valores de las variables quitando los caracteres < and >. Tenga en cuenta que este conjunto de comandos de PowerShell usa los valores de las siguientes opciones:

- Tabla M, para las máquinas virtuales
- Tabla V, para la configuración de red virtual
- Tabla S, para la subred
- Tabla ST, para las cuentas de almacenamiento
- Tabla A, para los conjuntos de disponibilidad

Recuerde que definió las tablas V, S, ST y A en [Fase 1: Configuración de Azure](virtual-machines-workload-high-availability-LOB-application-phase1.md).

Cuando proporcione todos los valores adecuados, ejecute el bloque resultante en el símbolo del sistema de Azure PowerShell.

	# Set up subscription and key variables
	$subscr="<name of the Azure subscription>"
	Set-AzureSubscription -SubscriptionName $subscr
	Switch-AzureMode AzureResourceManager
	$rgName="<resource group name>"
	$locName="<Azure location of your resource group>"
	$saName="<Table ST – Item 2 – Storage account name column>"
	$vnetName="<Table V – Item 1 – Value column>"
	$avName="<Table A – Item 1 – Availability set name column>"
	
	# Create the first domain controller
	$vmName="<Table M – Item 1 - Virtual machine name column>"
	$vmSize="<Table M – Item 1 - Minimum size column>"
	$staticIP="<Table V – Item 6 - Value column>"
	$vnet=Get-AzurevirtualNetwork -Name $vnetName -ResourceGroupName $rgName
	$nic=New-AzureNetworkInterface -Name ($vmName +"-NIC") -ResourceGroupName $rgName -Location $locName -SubnetId $vnet.Subnets[1].Id -PrivateIpAddress $staticIP
	$avSet=Get-AzureAvailabilitySet –Name $avName –ResourceGroupName $rgName 
	$vm=New-AzureVMConfig -VMName $vmName -VMSize $vmSize -AvailabilitySetId $avset.Id
	
	$diskSize=<size of the extra disk for AD DS data in GB>
	$storageAcc=Get-AzureStorageAccount -ResourceGroupName $rgName -Name $saName
	$vhdURI=$storageAcc.PrimaryEndpoints.Blob.ToString() + "vhds/" + $vmName + "-ADDSDisk.vhd"
	Add-AzureVMDataDisk -VM $vm -Name "ADDSData" -DiskSizeInGB $diskSize -VhdUri $vhdURI  -CreateOption empty
	
	$cred=Get-Credential -Message "Type the name and password of the local administrator account for the first domain controller." 
	$vm=Set-AzureVMOperatingSystem -VM $vm -Windows -ComputerName $vmName -Credential $cred -ProvisionVMAgent -EnableAutoUpdate
	$vm=Set-AzureVMSourceImage -VM $vm -PublisherName MicrosoftWindowsServer -Offer WindowsServer -Skus 2012-R2-Datacenter -Version "latest"
	$vm=Add-AzureVMNetworkInterface -VM $vm -Id $nic.Id
	$storageAcc=Get-AzureStorageAccount -ResourceGroupName $rgName -Name $saName
	$osDiskUri=$storageAcc.PrimaryEndpoints.Blob.ToString() + "vhds/" + $vmName + "-OSDisk.vhd"
	$vm=Set-AzureVMOSDisk -VM $vm -Name "OSDisk" -VhdUri $osDiskUri -CreateOption fromImage
	New-AzureVM -ResourceGroupName $rgName -Location $locName -VM $vm
	
	# Create the second domain controller
	$vmName="<Table M – Item 2 - Virtual machine name column>"
	$vmSize="<Table M – Item 2 - Minimum size column>"
	$staticIP="<Table V – Item 7 - Value column>"
	$vnet=Get-AzurevirtualNetwork -Name $vnetName -ResourceGroupName $rgName
	$nic=New-AzureNetworkInterface -Name ($vmName +"-NIC") -ResourceGroupName $rgName -Location $locName -SubnetId $vnet.Subnets[1].Id -PrivateIpAddress $staticIP
	$avSet=Get-AzureAvailabilitySet –Name $avName –ResourceGroupName $rgName 
	$vm=New-AzureVMConfig -VMName $vmName -VMSize $vmSize -AvailabilitySetId $avset.Id
	
	$diskSize=<size of the extra disk for AD DS data in GB>
	$storageAcc=Get-AzureStorageAccount -ResourceGroupName $rgName -Name $saName
	$vhdURI=$storageAcc.PrimaryEndpoints.Blob.ToString() + "vhds/" + $vmName + "-ADDSDisk.vhd"
	Add-AzureVMDataDisk -VM $vm -Name "ADDSData" -DiskSizeInGB $diskSize -VhdUri $vhdURI  -CreateOption empty
	
	$cred=Get-Credential -Message "Type the name and password of the local administrator account for the second domain controller." 
	$vm=Set-AzureVMOperatingSystem -VM $vm -Windows -ComputerName $vmName -Credential $cred -ProvisionVMAgent -EnableAutoUpdate
	$vm=Set-AzureVMSourceImage -VM $vm -PublisherName MicrosoftWindowsServer -Offer WindowsServer -Skus 2012-R2-Datacenter -Version "latest"
	$vm=Add-AzureVMNetworkInterface -VM $vm -Id $nic.Id
	$storageAcc=Get-AzureStorageAccount -ResourceGroupName $rgName -Name $saName
	$osDiskUri=$storageAcc.PrimaryEndpoints.Blob.ToString() + "vhds/" + $vmName + "-OSDisk.vhd"
	$vm=Set-AzureVMOSDisk -VM $vm -Name "OSDisk" -VhdUri $osDiskUri -CreateOption fromImage
	New-AzureVM -ResourceGroupName $rgName -Location $locName -VM $vm

## Configuración del primer controlador de dominio

Use el cliente de Escritorio remoto que prefiera y cree una conexión de Escritorio remoto para la primera máquina virtual de controlador de dominio. Use el DNS de la intranet o el nombre del equipo y las credenciales de la cuenta de administrador local.

A continuación, deberá agregar el disco de datos adicionales al primer controlador de dominio.

### <a id="datadisk"></a>Para inicializar un disco vacío

1.	En el panel izquierdo del Administrador de servidores, haga clic en **Servicios de archivos y almacenamiento** y, a continuación, haga clic en **Discos**.
2.	En el panel de contenido, en el grupo **Discos**, haga clic en **disco 2** (con la **partición** establecida en **Desconocida**).
3.	Haga clic en **Tareas** y, a continuación, haga clic en **Nuevo volumen**.
4.	En la página Antes de empezar del Asistente para volumen nuevo, haga clic en **Siguiente**.
5.	En la página Selección del servidor y del disco, haga clic en **Disco 2** y, a continuación, haga clic en **Siguiente**. Cuando se le solicite, haga clic en Aceptar.
6.	En la página Especificación del tamaño de la página de volumen, haga clic en **Siguiente**.
7.	En la página Asignación a una letra de unidad o carpeta, haga clic en **Siguiente**.
8.	En la página Selección de la configuración del sistema de archivos, haga clic en **Siguiente**.
9.	En la página Confirmación de las selecciones, haga clic en **Crear**.
10.	Cuando haya terminado, haga clic en **Cerrar**.

A continuación, pruebe la conectividad del primer controlador de dominio en ubicaciones de red de su organización.

### <a id="testconn"></a>Para probar la conectividad

1.	En el escritorio, abra un símbolo del sistema de Windows PowerShell.
2.	Use el comando **ping** para hacer ping en nombres y direcciones IP de recursos en la red de la organización.

Este procedimiento garantiza que la resolución de nombres DNS funciona correctamente (que la máquina virtual está configurada correctamente con los servidores DNS locales) y que se pueden enviar paquetes a la red virtual entre locales y desde ella. Si se produce un error en esta prueba básica, póngase en contacto con el departamento de TI para solucionar los problemas de entrega de paquetes y de resolución de nombres DNS.

A continuación, desde el símbolo de Windows PowerShell en el primer controlador de dominio, ejecute los siguientes comandos:

	$domname="<DNS domain name of the domain for which this computer will be a domain controller>"
	Install-WindowsFeature AD-Domain-Services -IncludeManagementTools
	Install-ADDSDomainController -InstallDns –DomainName $domname  -DatabasePath "F:\NTDS" -SysvolPath "F:\SYSVOL" -LogPath "F:\Logs"

Se le pedirá que proporcione las credenciales de una cuenta de administrador de dominio. El equipo se reiniciará.

## Configuración del segundo controlador de dominio

Use el cliente de Escritorio remoto que prefiera y cree una conexión de Escritorio remoto para la segunda máquina virtual de controlador de dominio. Use el DNS de la intranet o el nombre del equipo y las credenciales de la cuenta de administrador local.

A continuación, deberá agregar el disco de datos adicionales al segundo controlador de dominio. Consulte el procedimiento [Para inicializar un disco vacío](#datadisk).

A continuación, desde el símbolo del sistema de Windows PowerShell en el segundo controlador de dominio, ejecute los siguientes comandos:

	$domname="<DNS domain name of the domain for which this computer will be a domain controller>"
	Install-WindowsFeature AD-Domain-Services -IncludeManagementTools
	Install-ADDSDomainController -InstallDns –DomainName $domname  -DatabasePath "F:\NTDS" -SysvolPath "F:\SYSVOL" -LogPath "F:\Logs"

Se le pedirá que proporcione las credenciales de una cuenta de administrador de dominio. El equipo se reiniciará.

Tiene que actualizar los servidores DNS de la red virtual para que Azure asigne a las máquinas virtuales las direcciones IP de los dos nuevos controladores de dominio que se usarán como sus servidores DNS. Tenga en cuenta que este procedimiento usa valores de la tabla V (para la configuración de red virtual) y de la tabla M (para las máquinas virtuales).

1.	En el panel izquierdo del [Portal de vista previa de Azure](https://portal.azure.com/), haga clic en **Examinar todo > Máquinas virtuales** y después en el nombre de la red virtual (Tabla V – Elemento 1 – Columna Valor).
2.	En el panel de la red virtual, haga clic en **Toda la configuración**.
3.	En el panel **Configuración**, haga clic en **Servidores DNS**.
4.	En el panel **Servidores DNS**, escriba lo siguiente:
	- Para **Servidor DNS principal**: Tabla V – Elemento 6 – Columna Valor
	- Para **Servidor DNS secundario**: Tabla V – Elemento 7 – Columna Valor
5.	En el panel izquierdo del Portal de vista previa de Azure, haga clic en **Examinar todo > Máquinas virtuales**.
6.	En el panel **Máquinas virtuales**, haga clic en el nombre del primer controlador de dominio (Tabla M – Elemento 1 –Columna Nombre de la máquina virtual).
7.	En el panel de la máquina virtual, haga clic en **Reiniciar**.
8.	Cuando el primer controlador de dominio se inicie, haga clic en el nombre del segundo controlador de dominio en el panel **Máquinas virtuales** (Tabla M – Elemento 2 –Columna Nombre de la máquina virtual).
9.	En el panel de la máquina virtual, haga clic en **Reiniciar**. Espere hasta que se inicie el segundo controlador de dominio.

Tenga en cuenta que reiniciamos los dos controladores de dominio, por lo que no se configuran con los servidores DNS locales como servidores DNS. Como ambos son por sí mismos servidores DNS, se configuran automáticamente con los servidores DNS locales como reenviadores DNS cuando ascienden a controladores de dominio.

A continuación, es necesario crear un sitio de replicación de Active Directory para garantizar que los servidores de la red virtual de Azure usan los controladores de dominio local. Inicie sesión en el controlador de dominio principal con una cuenta de administrador de dominio y ejecute los siguientes comandos desde un símbolo del sistema de Windows PowerShell de nivel de administrador:

	$vnet="<Table V – Item 1 – Value column>"
	$vnetSpace="<Table V – Item 5 – Value column>"
	New-ADReplicationSite -Name $vnet 
	New-ADReplicationSubnet –Name $vnetSpace –Site $vnet

Agregue una cuenta de usuario para administrar las máquinas virtuales de SQL Server.

	New-ADUser -SamAccountName sqlservice -AccountPassword (read-host "Set user password" -assecurestring) -name "sqlservice" -enabled $true -PasswordNeverExpires $true -ChangePasswordAtLogon $false

Este diagrama muestra la configuración resultante de la realización correcta de esta fase con los nombres de equipo de marcador de posición.

![](./media/virtual-machines-workload-high-availability-LOB-application-phase2/workload-lobapp-phase2.png)

## Paso siguiente

Para continuar con la configuración de esta carga de trabajo, vaya a [Fase 3: Configuración de la infraestructura de SQL Server](virtual-machines-workload-high-availability-LOB-application-phase3.md).

## Recursos adicionales

[Implementación de una aplicación de línea de negocio de alta disponibilidad en Azure](virtual-machines-workload-high-availability-LOB-application-overview.md)

[Plano de arquitectura de aplicaciones de línea de negocio](http://msdn.microsoft.com/dn630664)

[Configuración de una aplicación de LOB basada en web en una nube híbrida para pruebas](../virtual-network/virtual-networks-setup-lobapp-hybrid-cloud-testing.md)

[Instrucciones de implementación de los servicios de infraestructura de Azure](virtual-machines-infrastructure-services-implementation-guidelines.md)

[Carga de trabajo de servicios de infraestructura de Azure: granja de SharePoint Server 2013](virtual-machines-workload-intranet-sharepoint-farm.md)

<!---HONumber=August15_HO8-->