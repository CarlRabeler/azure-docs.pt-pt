<properties
	pageTitle="Captura de una imagen de una máquina virtual que ejecuta Linux"
	description="Aprenda a capturar una imagen de una máquina virtual de Azure que ejecuta Linux."
	services="virtual-machines"
	documentationCenter=""
	authors="dsk-2015"
	manager="timlt"
	editor="tysonn"
	tags="azure-service-management"/>

<tags
	ms.service="virtual-machines"
	ms.workload="infrastructure-services"
	ms.tgt_pltfrm="vm-linux"
	ms.devlang="na"
	ms.topic="article"
	ms.date="07/16/2015"
	ms.author="dkshir"/>


# Captura de una máquina virtual de Linux para usar como plantilla

En este artículo se muestra cómo puede capturar una máquina virtual de Azure con Linux para usarla como plantilla en la creación de otras máquinas virtuales. Esta plantilla incluye el disco del sistema operativo y cualquier otro disco de datos conectado a la máquina virtual. No incluye la configuración de red, por lo que deberá configurarla usted mismo cuando cree las otras máquinas virtuales que utilicen la plantilla.

Azure trata esta plantilla como una imagen y la almacena en **Imágenes**. Aquí también están almacenadas todas las imágenes que ha cargado. Para obtener más información sobre las imágenes, consulte [Acerca de las imágenes de máquina virtual en Azure][].

## Antes de empezar

En estos pasos se supone que ya ha creado un máquina virtual de Azure con el modelo de implementación clásica y ha configurado el sistema operativo, incluido adjuntar cualquier disco de datos. Si no ha realizado esto todavía, siga estas instrucciones:

- [Creación de una máquina virtual que ejecuta Linux][]


## Capture la máquina virtual

1. Conéctese a la máquina virtual mediante un cliente SSH de su elección. Para obtener los detalles, consulte [Inicio de sesión en una máquina virtual con Linux][].

2. En la ventana SSH, escriba el siguiente comando. Tenga en cuenta que el resultado de `waagent` puede variar ligeramente según la versión de esta utilidad:

	`sudo waagent -deprovision`

	Este comando intentará limpiar el sistema y dejarlo adecuado para un reaprovisionamiento. Esta operación ejecuta las siguientes tareas:

	- Quita las claves de host de SSH (si Provisioning.RegenerateSshHostKeyPair es "y" en el archivo de configuración)
	- Borra la configuración de Nameserver en /etc/resolv.conf
	- Quita la contraseña del usuario `root` desde /etc/shadow (si Provisioning.DeleteRootPassword es "y" en el archivo de configuración)
	- Quita concesiones del cliente de DHCP en caché
	- Restablece el nombre de host a localhost.localdomain
	- Elimina la última cuenta de usuario aprovisionada (obtenida desde /var/lib/waagent) **y los datos asociados**.

	>[AZURE.NOTE]El desaprovisionando elimina los archivos y datos en un esfuerzo por "generalizar" la imagen. Solo puede ejecutar este comando en máquinas virtuales que desee capturar como nueva plantilla de imagen. No garantiza que se haya borrado toda información sensible de la imagen o que sea adecuada para su redistribución a terceros.


3. Escriba **y** para continuar. Puede agregar el parámetro `-force` para evitar este paso de confirmación.

4. Escriba **Salir** para cerrar el cliente SSH.


	>[AZURE.NOTE]En los siguientes pasos se supone que ya ha [instalado la CLI de Azure](../xplat-cli-install.md) en el equipo cliente. Los pasos siguientes también pueden realizarse en el [Portal de administración][].

5. Desde el equipo cliente, abra la CLI de Azure e inicie sesión en su suscripción de Azure. Para obtener más información, consulte [Conexión a una suscripción de Azure desde la CLI de Azure](../xplat-cli-connect.md).

6. Asegúrese de que está en modo de administración de servicios:

	`azure config mode asm`

7. Apague la máquina virtual que ya está desaprovisionada en los pasos anteriores mediante:

	`azure vm shutdown <your-virtual-machine-name>`

	>[AZURE.NOTE]Puede encontrar todas las máquinas virtuales creadas en su mediante `azure vm list`

8. Cuando se detenga la máquina virtual, capture la imagen con el comando:

	`azure vm capture -t <your-virtual-machine-name> <new-image-name>`

	Escriba el nombre de la imagen que desee en lugar de _nuevo-nombre-imagen_. Este comando crea una imagen de sistema operativo generalizada. El subcomando `-t` elimina la máquina virtual original.

9.	La nueva imagen ahora está disponible en la lista de imágenes que pueden usarse para configurar nuevas máquinas virtuales. Puede visualizarla mediante el comando:

	`azure vm image list`

	En el [Portal de administración][], aparecerá en la lista **IMÁGENES**.

	![Captura correcta de la imagen](./media/virtual-machines-linux-capture-image/VMCapturedImageAvailable.png)


## Pasos siguientes
La imagen está lista para ser utilizada como plantilla para crear máquinas virtuales. Puede usar el comando de la CLI de Azure `azure vm create` y proporcionar el nombre de la imagen que acaba de crear. Consulte [Uso de la CLI de Azure con la API de administración de servicios](virtual-machines-command-line-tools.md) para obtener más información acerca del comando. Como alternativa, puede usar el [Portal de administración][] para crear una máquina virtual personalizada mediante el método **Desde la galería** y seleccionar la imagen que acaba de crear. Consulte [Creación de una máquina virtual personalizada][] para obtener más detalles.

**Consulte también:**[Guía de usuario del Agente de Linux de Azure](virtual-machines-linux-agent-user-guide.md)

[Portal de administración]: http://manage.windowsazure.com
[Inicio de sesión en una máquina virtual con Linux]: virtual-machines-linux-how-to-log-on.md
[Acerca de las imágenes de máquina virtual en Azure]: http://msdn.microsoft.com/library/azure/dn790290.aspx
[Creación de una máquina virtual personalizada]: virtual-machines-create-custom.md
[How to Attach a Data Disk to a Virtual Machine]: storage-windows-attach-disk.md
[Creación de una máquina virtual que ejecuta Linux]: virtual-machines-linux-tutorial.md

<!---HONumber=August15_HO8-->