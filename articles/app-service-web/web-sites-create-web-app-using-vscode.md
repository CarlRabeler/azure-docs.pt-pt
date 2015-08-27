<properties
   pageTitle="Creación de una aplicación web ASP.NET 5 en Visual Studio Code"
   description="En este tutorial muestra cómo crear una aplicación web ASP.NET 5 con Visual Studio Code."
   services="app-service\web"
   documentationCenter=".net"
   authors="erikre"
   manager="wpickett"
   editor="jimbe"/>

<tags
	ms.service="app-service-web" 
	ms.workload="web" 
	ms.tgt_pltfrm="dotnet" 
	ms.devlang="na" 
	ms.topic="article" 
	ms.date="06/14/2015" 
	ms.author="erikre;tarcher"/>

# Creación de una aplicación web ASP.NET 5 en Visual Studio Code

## Información general

En este tutorial se muestra cómo se crea una aplicación web de ASP.NET 5 utilizando [Visual Studio Code (VS Code)](http://code.visualstudio.com//Docs/whyvscode) y cómo se implementa en el [Servicio de aplicaciones de Azure](../app-service/app-service-value-prop-what-is.md). ASP.NET 5 es un rediseño significativo de ASP.NET. ASP.NET 5 es un nuevo marco de código abierto multiplataforma diseñado para crear modernas aplicaciones web basadas en la nube con .NET. Para obtener más información, consulte [Introducción a ASP.NET 5](http://docs.asp.net/en/latest/conceptual-overview/aspnet.html). Para obtener información sobre Aplicaciones web del Servicio de aplicaciones de Azure, consulte [Introducción a aplicaciones web](app-service-web-overview.md).

[AZURE.INCLUDE [app-service-web-try-app-service.md](../../includes/app-service-web-try-app-service.md)]

## Requisitos previos  

* Instalación de [VS Code](http://code.visualstudio.com/Docs/setup).
* Instalación de [Node.js](http://nodejs.org/download/). [Node.js](http://nodejs.org/) es una plataforma para crear aplicaciones de servidor rápidas y escalables mediante JavaScript. Node es el tiempo de ejecución (Node) y [npm](http://www.npmjs.com/) es el Administrador de paquetes para los módulos de Node. En este tutorial utilizará npm para aplicar la técnica scaffolding a una aplicación web ASP.NET 5.
* Instalación de Git. Puede instalarlo desde cualquiera de estas ubicaciones: [Chocolatey](https://chocolatey.org/packages/git) o [git-scm.com](http://git-scm.com/downloads). Si no está familiarizado con Git, elija [git-scm.com](http://git-scm.com/downloads) y seleccione la opción para usar Git con GitBash desde el símbolo del sistema de Windows. Una vez que instale Git, también tendrá que establecer el nombre de usuario de Git y el correo electrónico, ya que es necesario más adelante en el tutorial (al realizar una confirmación desde VS Code).  

## Instalación de ASP.NET 5 y DNX
ASP.NET 5/DNX es una pila de .NET eficiente que sirve para crear aplicaciones web y de nube modernas que se ejecutan en OS X, Linux y Windows. Se ha desarrollado desde el principio para proporcionar un marco de desarrollo optimizado para las aplicaciones que se implementan en la nube o se ejecutan de forma local. Consta de componentes modulares con una sobrecarga mínima, para continuar disfrutando de flexibilidad al crear soluciones.

> [AZURE.NOTE]ASP.NET 5 y DNX (el entorno de ejecución .NET) en OS X y Linux están disponibles actualmente en versión de vista previa/beta preliminar.

Este tutorial está diseñado para ayudarle a comenzar a crear aplicaciones con las últimas versiones de desarrollo de ASP.NET 5 y DNX. Las instrucciones siguientes son específicas de Windows. Para obtener instrucciones de instalación más detalladas para Windows, Linux y OS X, consulte [Instalación de ASP.NET 5 y DNX](https://code.visualstudio.com/Docs/ASPnet5#_installing-aspnet-5-and-dnx).

1. Para instalar el Administrador de versión de .NET (DNVM) en Windows, abra un símbolo del sistema y ejecute el comando siguiente.

		@powershell -NoProfile -ExecutionPolicy unrestricted -Command "&{$Branch='dev';iex ((new-object net.webclient).DownloadString('https://raw.githubusercontent.com/aspnet/Home/dev/dnvminstall.ps1'))}"

	De esta forma, se descargará el script DNVM y se ubicará en el directorio del perfil de usuario.

2. Reinicie Windows para completar la instalación de DNVM.

3. Abra un símbolo del sistema y compruebe la ubicación de DNVM escribiendo lo siguiente.

		where dnvm

	En el símbolo del sistema se mostrará una ruta de acceso similar a la siguiente.

	![ubicación de dnvm](./media/web-sites-create-web-app-using-vscode/00-where-dnvm.png)

4. Ahora que ya dispone de DNVM, tendrá que utilizar este script para descargar DNX para poder ejecutar las aplicaciones. Ejecute lo siguiente en el símbolo del sistema.

		dnvm upgrade

5. Verifique el DNVM y consulte el tiempo de ejecución activo; para ello, escriba lo siguiente en el símbolo del sistema.

		dnvm list

	En el símbolo del sistema se mostrarán los detalles del tiempo de ejecución activo.

	![Ubicación de DNVM](./media/web-sites-create-web-app-using-vscode/00b-dnvm-list.png)

6. Si aparece más de un tiempo de ejecución DNX, escriba lo siguiente en el símbolo del sistema para establecer el tiempo de ejecución DNX activo con la misma versión que la que se utiliza en el generador de ASP.NET 5 al crear la aplicación web más adelante en este tutorial.

		dnvm use 1.0.0-beta4 –p

> [AZURE.NOTE]Para obtener instrucciones de instalación más detalladas para Windows, Linux y OS X, consulte [Instalación de ASP.NET 5 y DNX](https://code.visualstudio.com/Docs/ASPnet5#_installing-aspnet-5-and-dnx).

## Creación de la aplicación web 

En esta sección se explica cómo aplicar la técnica scaffolding a una nueva aplicación web ASP.NET. Use el administrador de paquetes para Node (npm) para instalar [Yeoman](http://yeoman.io/) (herramienta de scaffolding de aplicación: el equivalente de la operación de VS Code de Visual Studio **Archivo > Nuevo proyecto**), [Grunt](http://gruntjs.com/) (ejecutor de tareas de JavaScript), y [Bower](http://bower.io/) (administrador de paquetes del lado cliente).

1. Abra un símbolo del sistema con derechos de administrador y navegue hasta la ubicación donde desea crear el proyecto de ASP.NET.

2. En el símbolo del sistema para instalar Yeoman y las herramientas de soporte, escriba lo siguiente.

		npm install -g yo grunt-cli generator-aspnet bower

3. En el símbolo del sistema para crear la carpeta del proyecto y aplicar la técnica scaffolding a la aplicación, escriba lo siguiente.

		yo aspnet

4. Utilice las teclas de dirección para seleccionar el tipo de **aplicación web** en el menú del generador de ASP.NET 5 y pulse & lt;Entrar>.

	![Yeoman - Generador de ASP.NET 5](./media/web-sites-create-web-app-using-vscode/01-yo-aspnet.png)

5. Defina el nombre de la nueva aplicación web ASP.NET como **SampleWebApp**. Este nombre se usa en todo el tutorial, si selecciona otro nombre, deberá sustituir cada aparición de **SampleWebApp** por dicho nombre. Al pulsar &lt;Entrar>, Yeoman creará una carpeta nueva con el nombre **SampleWebApp**, además de los archivos necesarios para la nueva aplicación.

6. Abra VS Code escribiendo lo siguiente en el símbolo del sistema.

		code .

7. En VS Code, vaya a **Archivo > Abrir carpeta**, y seleccione la carpeta que contiene la aplicación web ASP.NET.

	![Cuadro de diálogo Seleccionar carpeta](./media/web-sites-create-web-app-using-vscode/02-open-folder.png)

	VS Code cargará el proyecto y mostrará sus archivos en la ventana **Exploración**.

	![VS Code mostrando el proyecto SampleWebApp](./media/web-sites-create-web-app-using-vscode/03-vscode-project.png)

8. Seleccione **Vista > Paleta de comandos**.

9. En la **Paleta de comandos**, escriba los comandos siguientes.

		dnx:dnu restore - (SampleWebApp)

	A medida que empiece a escribir, verá la línea de comandos completa en la lista.

	![Comando Restore](./media/web-sites-create-web-app-using-vscode/04-dnu-restore.png)

	El comando Restore instala los paquetes de NuGet necesarios para ejecutar la aplicación. En el símbolo del sistema, se mostrará el mensaje **Restauración completada** cuando el proceso haya finalizado.

## Ejecute la aplicación web localmente.

Ahora que ha creado la aplicación web y ha recuperado todos los paquetes de NuGet para la aplicación, puede ejecutar la aplicación web localmente.

1. En la **Paleta de comandos** en VS Code, escriba los comandos siguientes para ejecutar la aplicación de forma local.

		dnx: kestrel - (SampleWebApp, Microsoft.AspNet.Hosting --server Kestrel --server.urls http://localhost:5001

	En la ventana de comandos, aparecerá *Started*. Si en la ventana de comandos no aparece *Started*, compruebe la esquina inferior izquierda de VS Code para ver si hay errores en el proyecto.

5. Abra un explorador y vaya a la dirección URL siguiente.

	****http://localhost:5001**

	La página predeterminada de la aplicación web se mostrará como sigue.

	![Aplicación web local en un explorador](./media/web-sites-create-web-app-using-vscode/08-web-app.png)

## Creación de una aplicación web en el Portal de vista previa de Azure

Los pasos siguientes le guiarán a través de la creación de una aplicación web en el portal de vista previa de Azure.

1. Inicie sesión en el [portal de vista previa de Azure](https://portal.azure.com).

2. Haga clic en **NUEVO**, en la parte inferior izquierda del portal.

3. Haga clic en **Aplicaciones web > Aplicación web**.

	![Nueva aplicación web de Azure](./media/web-sites-create-web-app-using-vscode/09-azure-newwebapp.png)

4. Escriba un valor para **Nombre**, como **SampleWebAppDemo**. Tenga en cuenta que este nombre debe ser exclusivo y que el portal le obligará a ello al escribir el nombre. Por lo tanto, si selecciona un valor diferente, tendrá que sustituir ese valor en cada aparición de **SampleWebAppDemo** que aparezca en este tutorial.

5. Seleccione un **plan de Servicio de aplicaciones** ya existente o cree uno nuevo. Si crea un nuevo plan, seleccione el nivel de precios, la ubicación y otras opciones. Para obtener más información sobre los planes del Servicio de aplicaciones, consulte el artículo [Introducción detallada sobre los planes del Servicio de aplicaciones de Azure](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md).

	![Hoja de la nueva aplicación web de Azure](./media/web-sites-create-web-app-using-vscode/10-azure-newappblade.png)

6. Haga clic en **Crear**.

	![hoja de aplicación web](./media/web-sites-create-web-app-using-vscode/11-azure-webappblade.png)

## Habilitación de la publicación Git para la nueva aplicación web

Git es un sistema de control de versión distribuida que puede utilizar para implementar su aplicación web del Servicio de aplicaciones de Azure. Tendrá que almacenar el código que escriba para su aplicación web en un repositorio Git local y lo implementará en Azure insertando en un repositorio remoto.

1. Inicie sesión en el [portal de vista previa de Azure](https://portal.azure.com).

2. Haga clic en **Examinar todo**.

3. Haga clic en **Aplicaciones web** para ver una lista de las aplicaciones web asociadas con su suscripción de Azure.

4. Seleccione la aplicación web que creó en este tutorial.

5. En la hoja de la aplicación web, desplácese hacia abajo hasta la sección **Implementación** y haga clic en **Configurar implementación continua**.

	![Host de la aplicación web de Azure](./media/web-sites-create-web-app-using-vscode/14-azure-deployment.png)

6. Haga clic en **Elegir origen > Repositorio de Git local**.

7. Haga clic en **Aceptar**.

	![Repositorio local de Git de Azure](./media/web-sites-create-web-app-using-vscode/15-azure-localrepository.png)

8. Si no ha configurado previamente las credenciales de implementación para publicar una aplicación web u otra aplicación del Servicio de aplicaciones, configúrelas ahora:

	* Haga clic en **FTP** para establecer las credenciales de implementación.

	* Cree un nombre de usuario y una contraseña. Necesitará esta contraseña más tarde al configurar Git.

	* Haga clic en **Guardar**.

	![Credenciales de implementación de Azure](./media/web-sites-create-web-app-using-vscode/16-azure-credentials.png)

9. En la hoja de la aplicación web, haga clic en **Configuración > Propiedades**. La URL del repositorio Git remoto que implementará se muestra en **
10.  URL**.

10. Copie el valor **dirección URL de GIT** para su uso posterior en el tutorial.

	![Dirección URL de Git de Azure](./media/web-sites-create-web-app-using-vscode/17-azure-giturl.png)

## Publicación de la aplicación web en el Servicio de aplicaciones de Azure

En esta sección, creará un repositorio Git local e insertará desde ese repositorio a Azure para implementar la aplicación web en Azure.

1. En VS Code seleccione la opción **Git** en la barra de navegación de la izquierda.

	![Icono de Git en VS Code](./media/web-sites-create-web-app-using-vscode/git-icon.png)

2. Seleccione **Inicializar repositorio Git** para asegurarse de que el área de trabajo está bajo control del código fuente de git.

	![Inicializar Git](./media/web-sites-create-web-app-using-vscode/19-initgit.png)

3. Agregue un mensaje de confirmación y haga clic en el icono de marca **Confirmar todo**.

	![Confirmar todo de Git](./media/web-sites-create-web-app-using-vscode/20-git-commit.png)

4. Una vez que Git haya completado el procesamiento, verá que no hay ningún archivo que aparezca en la ventana de Git en **Cambios**.

	![Sin cambios de Git](./media/web-sites-create-web-app-using-vscode/no-changes.png)

5. Abra un símbolo del sistema y cambie los directorios por el directorio que contiene su aplicación web.

6. Cree una referencia remota para insertar actualizaciones en la aplicación web usando la dirección URL de Git (terminada en “.git”) que copió anteriormente.

		git remote add azure [URL for remote repository]

7. Configure Git para guardar las credenciales localmente de manera que se anexe automáticamente a los comandos de inserción generados a partir de código de VS.

		git config credential.helper store

8. Envíe los cambios a Azure escribiendo el siguiente comando. Después de esta inserción inicial en Azure, podrá realizar todos los comandos de inserción desde el código de VS.

		git push -u azure master

	Se le solicitará la contraseña que ha creado anteriormente. **Nota: la contraseña no será visible.**

	La salida de este comando finaliza con un mensaje que indica que la implementación se ha realizado correctamente.

		remote: Deployment successful.
		To https://user@testsite.scm.azurewebsites.net/testsite.git
		[new branch]      master -> master

> [AZURE.NOTE]Si realiza cambios en la aplicación, puede volver a publicar directamente en el código de VS con la funcionalidad integrada de Git seleccionando la opción **Confirmar todo** seguida de la opción **Insertar**. Encontrará la opción **Insertar** en el menú desplegable junto a los botones **Confirmar todo** y **Actualizar**.

Si necesita para colaborar en un proyecto, considere la posibilidad de insertar en GitHub entre la inserción en Azure.

## Ejecución de la aplicación en Azure
Ahora que ha implementado la aplicación web, vamos a ejecutar la aplicación mientras está hospedada en Azure.

Esto puede hacerse de dos maneras:

* Abra un explorador y escriba el nombre de la aplicación web de la manera siguiente.   

		http://SampleWebAppDemo.azurewebsites.net
 
* En el Portal de vista previa de Azure, busque la hoja de aplicación web para la aplicación web y haga clic en **Examinar** para ver la aplicación en el explorador predeterminado.

![Aplicación web de Azure](./media/web-sites-create-web-app-using-vscode/21-azurewebapp.png)

## Resumen
En este tutorial, ha aprendido a crear una aplicación web en VS Code y a implementarla en Azure. Para obtener más información sobre VS Code, consulte el artículo sobre [Visual Studio Code.](https://code.visualstudio.com/Docs/). Para obtener más información sobre Aplicaciones web del Servicio de aplicaciones de Azure, consulte [Información general de aplicaciones web](app-service-web-overview.md).

<!---HONumber=August15_HO6-->