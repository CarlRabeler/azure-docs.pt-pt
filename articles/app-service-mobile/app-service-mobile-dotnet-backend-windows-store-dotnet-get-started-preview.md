<properties
	pageTitle="Creación de una aplicación universal Windows en tiempo de ejecución 8.1 en Aplicaciones móviles de Azure"
	description="Siga este tutorial para aprender a usar back-ends de aplicación móvil de Azure para el desarrollo de la Tienda Windows en C#, VB o JavaScript."
	services="app-service\mobile"
	documentationCenter="windows"
	authors="ggailey777"
	manager="dwrede"
	editor=""/>

<tags
	ms.service="app-service-mobile"
	ms.workload="mobile"
	ms.tgt_pltfrm="mobile-windows"
	ms.devlang="dotnet"
	ms.topic="hero-article"
	ms.date="08/14/2015"
	ms.author="glenga"/>

#Creación de una aplicación para Windows

[AZURE.INCLUDE [app-service-mobile-selector-get-started-preview](../../includes/app-service-mobile-selector-get-started-preview.md)]&nbsp;[AZURE.INCLUDE [app-service-mobile-note-mobile-services-preview](../../includes/app-service-mobile-note-mobile-services-preview.md)]

##Información general

En este tutorial se muestra cómo agregar un servicio back-end basado en la nube a una aplicación universal Windows en tiempo de ejecución 8.1 con un back-end de la aplicación móvil de Azure. Las soluciones de aplicaciones universales de Windows incluyen proyectos para aplicaciones tanto de la Tienda Windows 8.1 como de la Tienda de Windows Phone 8.1 y un proyecto común compartido.

[AZURE.INCLUDE [app-service-mobile-windows-universal-get-started-preview](../../includes/app-service-mobile-windows-universal-get-started-preview.md)]

##Requisitos previos

Para completar este tutorial, necesitará lo siguiente:

* Una cuenta de Azure activa. Si no dispone de ninguna cuenta, puede registrarse para obtener una versión de evaluación de Azure y conseguir hasta 10 aplicaciones móviles gratuitas que podrá seguir usando incluso después de que finalice la evaluación. Para obtener más información, consulte [Evaluación gratuita de Azure](http://azure.microsoft.com/pricing/free-trial/).

* [Visual Studio Community 2013] o posterior.

>[AZURE.NOTE]Si desea empezar a trabajar con el Servicio de aplicaciones de Azure antes de registrarse para abrir una cuenta de Azure, vaya a [Probar Servicio de aplicaciones](http://go.microsoft.com/fwlink/?LinkId=523751&appServiceName=mobile), donde podrá crear inmediatamente una aplicación móvil de inicio de corta duración en el Servicio de aplicaciones. No es necesario proporcionar ninguna tarjeta de crédito ni asumir ningún compromiso.

##Creación de un nuevo back-end de Aplicaciones móviles de Azure

[AZURE.INCLUDE [app-service-mobile-dotnet-backend-create-new-service-preview](../../includes/app-service-mobile-dotnet-backend-create-new-service-preview.md)]

## Descarga del proyecto de servidor

1. En el [Portal de Azure], haga clic en **Examinar todo** > **Aplicaciones web** y, luego, haga clic en el back-end de la aplicación móvil que acaba de crear. 

2. En el back-end de la aplicación móvil, haga clic en **Todas las configuraciones** y, en **Aplicación móvil**, haga clic en **Inicio rápido** > **Windows (C#)**.

3. En **Descargar y ejecutar el proyecto de servidor**, dentro de **Crear una nueva aplicación**, haga clic en **Descargar**, extraiga los archivos de proyecto comprimidos en el equipo local y abra la solución en Visual Studio.

4. Compile el proyecto para restaurar los paquetes de NuGet.

##Publicación del proyecto de servidor en Azure

[AZURE.INCLUDE [app-service-mobile-dotnet-backend-publish-service-preview](../../includes/app-service-mobile-dotnet-backend-publish-service-preview.md)]

##Descarga y ejecución del proyecto de cliente

Una vez que haya creado el back-end de aplicación móvil, podrá seguir una introducción rápida en el Portal de Azure para crear una nueva aplicación o modificar una ya existente para conectarla al back-end de aplicación móvil.

En esta sección, descargará un proyecto de plantilla de aplicación universal Windows que se personaliza para conectarse al back-end de la aplicación móvil de Azure.

1. De vuelta en la hoja del back-end de la aplicación móvil, haga clic en **Todas las configuraciones** y, en **Aplicación móvil**, haga clic en **Inicio rápido** > **Windows (C#)**. 

2.  En **Descargar y ejecutar el proyecto de Windows**, dentro de **Crear una nueva aplicación**, haga clic en **Descargar** y extraiga los archivos de proyecto comprimidos en el equipo local.
  
3. (Opcional) Agregue el proyecto de aplicación universal Windows a la solución con el proyecto de servidor. Esto hace que sea más fácil depurar y probar la aplicación y el back-end en la misma solución de Visual Studio, si decide hacer esto.

4. Con la aplicación de la Tienda Windows como proyecto de inicio, presione la tecla F5 para recompilar el proyecto e inicie la aplicación de la Tienda Windows.

5. En la aplicación, escriba texto significativo, como *Realice el tutorial*, en el cuadro de texto **Insertar un TodoItem** y, a continuación, haga clic en **Guardar**.

	![](./media/app-service-mobile-dotnet-backend-windows-store-dotnet-get-started-preview/mobile-quickstart-startup.png)

	Esta acción envía una solicitud POST al nuevo back-end de aplicación móvil hospedado en Azure.

6. Detenga la depuración, haga clic con el botón derecho en el proyecto `<your app name>.WindowsPhone`, haga clic en **Establecer como proyecto de inicio** y, por último, presione F5 de nuevo.

	![](./media/app-service-mobile-dotnet-backend-windows-store-dotnet-get-started-preview/mobile-quickstart-completed-wp8.png)

	Tenga en cuenta que los datos guardados en el paso anterior se cargan desde la aplicación móvil después de que se inicie la aplicación de Windows.

##Pasos siguientes

* [Incorporación de autenticación a la aplicación](app-service-mobile-dotnet-backend-windows-store-dotnet-get-started-users-preview.md) <br/>Aprenda a autenticar a los usuarios de su aplicación con un proveedor de identidades.

* [Incorporación de notificaciones de inserción a la aplicación](app-service-mobile-dotnet-backend-windows-store-dotnet-get-started-push-preview.md) <br/>Aprenda a enviar una notificación push muy básica a la aplicación.

<!-- Anchors. -->
<!-- Images. -->
<!-- URLs. -->
[Get started with authentication]: app-service-mobile-dotnet-backend-windows-store-dotnet-get-started-users-preview.md
[Mobile App SDK]: http://go.microsoft.com/fwlink/?LinkId=257545
[Portal de Azure]: https://portal.azure.com/

[Visual Studio Community 2013]: https://go.microsoft.com/fwLink/p/?LinkID=534203
 

<!---HONumber=August15_HO8-->