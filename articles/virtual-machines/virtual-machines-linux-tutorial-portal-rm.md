<properties
	pageTitle="Creación de una máquina virtual de Azure con Linux en el Portal de Azure"
	description="Utilice el Portal de Azure para crear una máquina virtual (VM) de Azure en la que se ejecute Linux con los grupos de recursos de Azure."
	services="virtual-machines"
	documentationCenter=""
	authors="squillace"
	manager="timlt"
	editor="tysonn"
	tags="azure-resource-management"/>

<tags
	ms.service="virtual-machines"
	ms.workload="infrastructure-services"
	ms.tgt_pltfrm="vm-linux"
	ms.devlang="na"
	ms.topic="article"
	ms.date="07/13/2015"
	ms.author="rasquill"/>

# Creación de una máquina virtual con Linux en el Portal de vista previa de Azure

> [AZURE.SELECTOR]
- [Azure CLI](virtual-machines-linux-tutorial.md)
- [Azure Preview Portal](virtual-machines-linux-tutorial-portal-rm.md)

Es fácil crear una máquina virtual (VM) de Azure que ejecute Linux. En este tutorial se muestra cómo usar el Portal de vista previa de Azure para crear una de forma rápida y se usa el archivo de clave pública `~/.ssh/id_rsa.pub` para proteger una conexión **SSH** a la máquina virtual. También puede crear máquinas virtuales con Linux con [sus propias imágenes como plantillas](virtual-machines-linux-create-upload-vhd.md).

> [AZURE.NOTE]En este tutorial se crea una máquina virtual de Azure que se administra mediante la API del grupo de recursos de Azure. Para obtener más información, consulte [Información general del grupo de recursos de Azure](resource-group-overview.md).

[AZURE.INCLUDE [free-trial-note](../../includes/free-trial-note.md)]

## Selección de la imagen

Vaya a Azure Marketplace en el Portal de vista previa para buscar la imagen de la máquina virtual de Windows Server que desea.

1. Inicie sesión en el [Portal de vista previa](https://portal.azure.com).

2. En el menú Concentrador, haga clic en **Nuevo** > **Proceso** > **Servidor de Ubuntu 14.04 LTS**.

	![elegir una imagen de VM](media/virtual-machines-linux-tutorial-portal-rm/chooseubuntuvm.png)

	> [AZURE.TIP]Para buscar imágenes adicionales, haga clic en **Marketplace** y, a continuación, búsquelas o filtre las que haya disponibles.

3. En la parte inferior de la página **Servidor de Ubuntu 14.04 LTS**, seleccione **Usar la pila del Administrador de recursos** para crear la máquina virtual en el Administrador de recursos de Azure. Tenga en cuenta que para la mayoría de las cargas de trabajo nueva, se recomienda la pila del Administrador de recursos. Para obtener más información, consulte [Proveedores de procesos, redes y almacenamiento de Azure en el Administrador de recursos de Azure](virtual-machines-azurerm-versus-azuresm.md).

4. A continuación, haga clic en ![botón Crear](media/virtual-machines-linux-tutorial-portal-rm/createbutton.png).

	![cambiar a la pila de procesos de administrador de recursos](media/virtual-machines-linux-tutorial-portal-rm/changetoresourcestack.png)

## Creación de la máquina virtual

Después de seleccionar la imagen, puede usar los valores predeterminados de Azure en la mayor parte de la configuración y crear rápidamente la máquina virtual.

1. En la hoja **Crear máquina virtual**, haga clic en **Datos básicos**. Escriba el **nombre** que desee que tenga la máquina virtual e introduzca un archivo de clave pública (en formato **ssh rsa**, en este caso desde el archivo `~/.ssh/id_rsa.pub`). Si tiene más de una suscripción, especifique una de ellas para la nueva máquina virtual, así como un nuevo **grupo de recursos**, o uno existente, y la **ubicación** de un centro de datos de Azure.

	![](media/virtual-machines-linux-tutorial-portal-rm/step-1-thebasics.png)

	> [AZURE.NOTE]También puede elegir la autenticación de usuario/contraseña aquí y escribir dicha información si no desea proteger su sesión **ssh** con un intercambio de claves público y privado.

2. Haga clic en **Tamaño** y seleccione un tamaño de VM apropiado para sus necesidades. Cada tamaño especifica el número de núcleos de proceso, la memoria y otras características, como la compatibilidad con el Almacenamiento premium, lo que afectará al precio. Azure recomienda automáticamente determinados tamaños según la imagen que se elija. Cuando haya terminado, haga clic en ![botón Seleccionar](media/virtual-machines-linux-tutorial-portal-rm/selectbutton-size.png).

	>[AZURE.NOTE]El almacenamiento Premium está disponible para las máquinas virtuales de la serie DS en determinadas regiones. El almacenamiento Premium es la mejor opción de almacenamiento para cargas de trabajo intensivas de datos como una base de datos. Para obtener más información, consulte [Almacenamiento Premium: Almacenamiento de alto rendimiento para cargas de trabajo de máquina virtual de Azure](storage-premium-storage-preview-portal.md)

3. Haga clic en **Configuración** para ver la configuración de red y de almacenamiento de la nueva máquina virtual. En la primera máquina virtual, generalmente podrá aceptar la configuración predeterminada. Si ha seleccionado un tamaño de máquina virtual que lo admita, puede probar Almacenamiento premium, para lo que debe seleccionar **Premium (SSD)** en **Tipo de disco**. Cuando haya terminado, haga clic en ![botón Aceptar](media/virtual-machines-linux-tutorial-portal-rm/okbutton.png).

	![](media/virtual-machines-linux-tutorial-portal-rm/step-3-settings.png)

6. Haga clic en **Resumen** para revisar la configuración elegida. Cuando haya terminado de revisar o actualizar la configuración, haga clic en ![botón Aceptar](media/virtual-machines-linux-tutorial-portal-rm/createbutton.png).

	![resumen de creación](media/virtual-machines-linux-tutorial-portal-rm/summarybeforecreation.png)

8. Mientras Azure crea la máquina virtual, puede realizar un seguimiento del progreso en **Notificaciones**, en el menú Concentrador. Después de que Azure cree la máquina virtual, la verá en el Panel de inicio, a menos que haya desactivado **Anclar a Panel de inicio** en la hoja **Crear máquina virtual**.

	> [AZURE.NOTE]Tenga en cuenta que el resumen no contiene un nombre DNS público del modo que lo hace cuando se crea una máquina virtual dentro de un servicio en la nube con la pila del proceso de administración del servicio.

## Conexión a una máquina virtual con Linux de Azure mediante **ssh**

Ahora puede conectarse a una máquina virtual con Ubuntu mediante el uso estándar de **ssh**. Sin embargo, es preciso detectar la dirección IP asignada a la máquina virtual de Azure, para lo que debe abrir el icono de máquina virtual y sus recursos. Para ello, haga clic en **Examinar**, seleccione **Reciente** y busca la máquina virtual que creó, o bien haga clic en el icono creado en el Panel de inicio. En cualquiera de los dos casos, busque y copie el valor **Dirección IP pública**, que se muestra en el gráfico siguiente.

![resumen de la creación correcta](media/virtual-machines-linux-tutorial-portal-rm/successresultwithip.png)

Ya puede usar **ssh** en la máquina virtual de Azure.

	ssh ops@23.96.106.1 -p 22
	The authenticity of host '23.96.106.1 (23.96.106.1)' can't be established.
	ECDSA key fingerprint is bc:ee:50:2b:ca:67:b0:1a:24:2f:8a:cb:42:00:42:55.
	Are you sure you want to continue connecting (yes/no)? yes
	Warning: Permanently added '23.96.106.1' (ECDSA) to the list of known hosts.
	Welcome to Ubuntu 14.04.2 LTS (GNU/Linux 3.16.0-43-generic x86_64)

	 * Documentation:  https://help.ubuntu.com/

	  System information as of Mon Jul 13 21:36:28 UTC 2015

	  System load: 0.31              Memory usage: 5%   Processes:       208
	  Usage of /:  42.1% of 1.94GB   Swap usage:   0%   Users logged in: 0

	  Graph this data and manage this system at:
	    https://landscape.canonical.com/

	  Get cloud support with Ubuntu Advantage Cloud Guest:
	    http://www.ubuntu.com/business/services/cloud

	0 packages can be updated.
	0 updates are security updates.

	The programs included with the Ubuntu system are free software;
	the exact distribution terms for each program are described in the
	individual files in /usr/share/doc/*/copyright.

	Ubuntu comes with ABSOLUTELY NO WARRANTY, to the extent permitted by
	applicable law.

	ops@ubuntuvm:~$


## Pasos siguientes

Para obtener más información sobre Linux en Azure, consulte:

- [Informática de código abierto y Linux en Azure](virtual-machines-linux-opensource.md)

- [Uso de las herramientas de línea de comandos de Azure para Mac y Linux](virtual-machines-command-line-tools.md)

- [Implementación de una aplicación LAMP con la extensión CustomScript para Linux](virtual-machines-linux-script-lamp.md)

- [Extensión de máquina virtual Docker para Linux en Azure](virtual-machines-docker-vm-extension.md)

<!---HONumber=August15_HO6-->