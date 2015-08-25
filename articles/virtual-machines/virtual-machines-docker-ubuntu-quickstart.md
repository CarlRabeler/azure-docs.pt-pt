<properties
	pageTitle="Cómo usar Docker rápidamente con una imagen de máquina virtual de Ubuntu-Docker"
	description="Se describe y se muestra cómo usar Docker en Ubuntu Server directamente desde la Galería de imágenes de Azure en minutos"
	services="virtual-machines"
	documentationCenter=""
	authors="squillace"
	manager="timlt"
	editor="tysonn"/>

<tags
	ms.service="virtual-machines"
	ms.devlang="na"
	ms.topic="article"
	ms.tgt_pltfrm="vm-linux"
	ms.workload="infrastructure"
	ms.date="05/20/2015"
	ms.author="rasquill"/>

# Cómo empezar a trabajar rápidamente con Docker en Azure Marketplace

La forma más rápida de empezar a usar [Docker] es ir a Azure Marketplace y crear una máquina virtual con la plantilla de imagen **Docker on Ubuntu Server** creada por [Canonical] junto con [MSOpenTech]. Así se crea una máquina virtual de Ubuntu Server y se instala automáticamente la [extensión de máquina virtual Docker](virtual-machines-docker-vm-extension.md) junto con el motor de Docker **más reciente** previamente instalado y que se ejecuta en Azure.

Puede conectarse inmediatamente a la máquina virtual mediante SSH y empezar a trabajar con Docker directamente sin hacer nada más.

> [AZURE.NOTE]La máquina virtual creada con la plantilla de Azure Marketplace no aloja la API remota de Docker para la administración de un cliente remoto de docker. Para habilitar el control del host de Docker en esta máquina virtual de forma remota, consulte [Docker en ejecución con https](https://docs.docker.com/articles/https/) o siga los pasos de [Uso de la extensión de máquina virtual Docker desde el Portal de Azure](virtual-machines-docker-with-portal.md) o [Uso de la extensión de la máquina virtual Docker desde la CLI de Azure](virtual-machines-docker-with-xplat-cli.md). Si quiere hacer algo más avanzado, puede generar el [cliente de Windows Docker](https://github.com/ahmetalpbalkan/Docker.DotNet) desde Github y probarlo también (o simplemente cogerlo de [nuget](https://www.nuget.org/packages/Docker.DotNet/)).

En este tema:

- [Inicio de sesión en el Portal]
- [Creación de una máquina virtual con la imagen de Docker de Canonical y MSOpenTech]
- [Conexión con SSH y a disfrutar]

## <a id='logon'>Inicio de sesión en el Portal</a>

Esta parte es fácil, a menos que no disponga de una cuenta de Azure. [Obtenga una también con facilidad.](http://azure.microsoft.com/pricing/free-trial/)

## <a id='createvm'>Creación de una máquina virtual con la imagen de Docker de Canonical y MSOpenTech</a>

1. Ahora que ya ha iniciado sesión, haga clic en **Nueva** en la esquina inferior izquierda para crear una nueva imagen de máquina virtual. Es posible que vea la imagen adecuada en el banner inmediatamente:

> ![Elección de la imagen de Ubuntu Docker en el banner](./media/virtual-machines-docker-ubuntu-quickstart/CreateNewDockerBanner.png)

2. Pero si no es así, búsquela en la Galería de imágenes:

> ![Búsqueda de la imagen en la galería de imágenes](./media/virtual-machines-docker-ubuntu-quickstart/DockerOnUbuntuServerMSOpenTech.png)

3. Proporcione el nombre de usuario y la contraseña para la instancia, o un archivo **.pem** para habilitar SSH con un certificado. (En el gráfico siguiente se muestra cómo se especifica una combinación de nombre de usuario y contraseña). A continuación, presione **Crear** en la parte inferior.

> ![Configuración de la instancia de máquina virtual](./media/virtual-machines-docker-ubuntu-quickstart/CreateVMDockerUbuntuPwd.png)

4. Y espere hasta que esté en ejecución. No debería tardar demasiado.

> ![Imagen de docker que se ejecuta en el portal](./media/virtual-machines-docker-ubuntu-quickstart/DockerUbuntuRunning.png)

## <a id='havingfun'>Conexión con SSH y a disfrutar</a>

Ahora empieza la diversión. Puede conectarse inmediatamente a la máquina virtual mediante SSH:

> ![Conexión con SSH](./media/virtual-machines-docker-ubuntu-quickstart/SSHToDockerUbuntu.png)

Y empezar a emitir comandos Docker, sin olvidar que en esta máquina virtual de Azure la configuración predeterminada requiere **`sudo`**:

> ![Extracción de imágenes](./media/virtual-machines-docker-ubuntu-quickstart/DockerPullSmallImages.png)

<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## Pasos siguientes

Va a comenzar a usar [Docker].

<!--Anchors-->
[Inicio de sesión en el Portal]: #logon
[Creación de una máquina virtual con la imagen de Docker de Canonical y MSOpenTech]: #createvm
[Conexión con SSH y a disfrutar]: #havingfun
[Next steps]: #next-steps


[Docker]: https://www.docker.com/
[BusyBox]: http://en.wikipedia.org/wiki/BusyBox
[Docker scratch image]: https://docs.docker.com/articles/baseimages/#creating-a-simple-base-image-using-scratch
[Canonical]: http://www.canonical.com/
[MSOpenTech]: http://msopentech.com/
 

<!---HONumber=August15_HO6-->