<properties
	pageTitle="Introducción a Aplicaciones móviles de Azure para aplicaciones Xamarin.iOS"
	description="Siga este tutorial para empezar a usar Aplicaciones móviles de Azure para el desarrollo de Xamarin.iOS."
	services="app-service\mobile"
	documentationCenter="xamarin"
	authors="normesta"
	manager="dwrede"
	editor=""/>

<tags
	ms.service="app-service-mobile"
	ms.workload="mobile"
	ms.tgt_pltfrm="mobile-xamarin-ios"
	ms.devlang="dotnet"
	ms.topic="hero-article"
	ms.date="08/12/2015"
	ms.author="normesta"/>


#Creación de una aplicación Xamarin.iOS

[AZURE.INCLUDE [app-service-mobile-selector-get-started-preview](../../includes/app-service-mobile-selector-get-started-preview.md)]&nbsp;[AZURE.INCLUDE [app-service-mobile-note-mobile-services-preview](../../includes/app-service-mobile-note-mobile-services-preview.md)]

##Información general

En este tutorial se muestra cómo agregar un servicio de back-end basado en la nube a una aplicación móvil Xamarin.iOS con un back-end de Aplicaciones móviles de Azure. Creará tanto un back-end de aplicación móvil nuevo como una aplicación Xamarin.iOS simple de la _lista de tareas pendientes_ que almacene los datos de la aplicación en Azure.

Completar este tutorial es un requisito previo para todos los tutoriales de aplicaciones móviles para aplicaciones Xamarin.Android.
 
##Requisitos previos

Para completar este tutorial, necesitará lo siguiente:

* Una cuenta de Azure activa. Si no dispone de ninguna cuenta, puede registrarse para obtener una versión de evaluación de Azure y conseguir hasta 10 aplicaciones móviles gratuitas que podrá seguir usando incluso después de que finalice la evaluación. Para obtener más información, consulte [Evaluación gratuita de Azure](http://azure.microsoft.com/pricing/free-trial/).
 
* [Visual Studio Community 2013] o posterior. Si instala Visual Studio Community 2013, instale [Xamarin] por separado. Puede instalar las herramientas de Xamarin al instalar Visual Studio 2015.

* Un equipo Mac con [Xcode] v7.0 o versiones posteriores y [Xamarin Studio] instalados.
 
     >[AZURE.NOTE]Si piensa compilar la aplicación en un equipo Windows mediante Visual Studio, necesitará tener acceso a un equipo Mac en red para hacerlo.
 
>[AZURE.NOTE]Si desea empezar a trabajar con el Servicio de aplicaciones de Azure antes de registrarse para abrir una cuenta de Azure, vaya a [Probar Servicio de aplicaciones](http://go.microsoft.com/fwlink/?LinkId=523751&appServiceName=mobile), donde podrá crear inmediatamente una aplicación móvil de inicio de corta duración en el Servicio de aplicaciones. No es necesario proporcionar ninguna tarjeta de crédito ni asumir ningún compromiso.

## Creación de un nuevo back-end de Aplicaciones móviles de Azure

[AZURE.INCLUDE [app-service-mobile-dotnet-backend-create-new-service-preview](../../includes/app-service-mobile-dotnet-backend-create-new-service-preview.md)]

## Descarga del proyecto de servidor

1. En el equipo, visite el [portal de Azure]. Haga clic en **Examinar todo** > **Aplicaciones móviles** y, después, haga clic en el back-end de aplicaciones móviles que acaba de crear.

2. En la hoja de la aplicación móvil, haga clic en **Configuración** y, en **Aplicación móvil**, haga clic en **Inicio rápido** > **Xamarin.iOS**.
 
3. En **Descargar y ejecutar el proyecto de servidor**, haga clic en **Descargar**. Extraiga los archivos comprimidos del proyecto en su equipo y abra la solución en Visual Studio.
 
## Prueba del proyecto de back-end de forma local

[AZURE.INCLUDE [app-service-mobile-dotnet-backend-test-local-service-preview](../../includes/app-service-mobile-dotnet-backend-test-local-service-preview.md)]

## Publicación del proyecto de servidor en Azure

[AZURE.INCLUDE [app-service-mobile-dotnet-backend-publish-service-preview](../../includes/app-service-mobile-dotnet-backend-publish-service-preview.md)]

## Descarga y ejecución de la aplicación Xamarin.iOS

1. En el equipo Mac, abra el [Portal de Azure] en una ventana del explorador.

>[AZURE.NOTE]Es más fácil ejecutar la aplicación Xamarin.iOS en un equipo Mac. Puede ejecutar la aplicación Xamarin.iOS mediante Visual Studio en el equipo Windows si lo desea, pero es un poco más complicado porque tiene que conectarse a un equipo Mac en red. Si está interesado en hacer eso, consulte [Instalación de Xamarin.iOS en Windows].

2. En **Descargar y ejecutar el proyecto de Xamarin.iOS**, haga clic en el botón **Descargar**.

  	De este modo, se descarga un proyecto que contiene la aplicación cliente que está conectada a la aplicación móvil. Guarde el archivo comprimido del proyecto en el equipo local y anote dónde lo guardó.

3. Extraiga el proyecto descargado y, después, ábralo en Xamarin Studio (o Visual Studio).

	![][9]

	![][8]

4. Presione la tecla **F5** para compilar el proyecto e iniciar la aplicación en el emulador de iPhone.

5. En la aplicación, escriba texto significativo, como _Aprender Xamarin_, y haga clic en el botón **+**.

	![][10]

	Esta acción envía una solicitud POST al nuevo back-end de aplicación móvil hospedado en Azure. Los datos de la solicitud se insertan en la tabla TodoItem. El back-end de aplicación móvil devuelve los elementos almacenados en la tabla y los datos se muestran en la lista.

>[AZURE.NOTE]Puede revisar el código que obtiene acceso al back-end de aplicación móvil para consultar e insertar datos en el archivo de C# QSTodoService.cs.

##Pasos siguientes

* [Incorporación de autenticación a la aplicación](app-service-mobile-dotnet-backend-xamarin-ios-get-started-users-preview.md) <br/>Aprenda a autenticar a los usuarios de su aplicación con un proveedor de identidades.

* [Incorporación de notificaciones de inserción a la aplicación](app-service-mobile-dotnet-backend-xamarin-ios-get-started-push-preview.md) <br/>Aprenda a enviar una notificación push muy básica a la aplicación.

<!-- Anchors. -->
[Getting started with mobile app backends]: #getting-started
[Create a new mobile app backend]: #create-new-service
[Next Steps]: #next-steps



<!-- Images. -->
[6]: ./media/app-service-mobile-dotnet-backend-xamarin-ios-get-started-preview/xamarin-ios-quickstart.png
[8]: ./media/app-service-mobile-dotnet-backend-xamarin-ios-get-started-preview/mobile-xamarin-project-ios-vs.png
[9]: ./media/app-service-mobile-dotnet-backend-xamarin-ios-get-started-preview/mobile-xamarin-project-ios-xs.png
[10]: ./media/app-service-mobile-dotnet-backend-xamarin-ios-get-started-preview/mobile-quickstart-startup-ios.png

<!-- URLs. -->
[Get started with offline data sync]: app-service-mobile-xamarin-ios-get-started-offline-data-preview.md
[Get started with authentication]: ../app-service-mobile-dotnet-backend-xamarin-ios-get-started-preview-users.md
[Get started with push notifications]: ../app-service-mobile-dotnet-backend-xamarin-ios-get-started-preview-push.md
[Visual Studio Professional 2013]: https://go.microsoft.com/fwLink/p/?LinkID=257546
[Mobile app SDK]: http://go.microsoft.com/fwlink/?LinkId=257545
[portal de Azure]: https://portal.azure.com/
[JavaScript backend version]: ../partner-xamarin-mobile-services-ios-get-started.md
[Get started with data in app services using Visual Studio 2012]: ../app-service-mobile-windows-store-dotnet-get-started-data-vs2012-preview.md
[Troubleshoot a mobile app .NET backend]: ../app-service-mobile-dotnet-backend-how-to-troubleshoot-preview.md


[Xamarin Studio]: http://xamarin.com/download
[Xamarin]: http://xamarin.com/download
[Xcode]: https://go.microsoft.com/fwLink/?LinkID=266532
[Xamarin for Windows]: https://go.microsoft.com/fwLink/?LinkID=330242&clcid=0x409
[Instalación de Xamarin.iOS en Windows]: http://developer.xamarin.com/guides/ios/getting_started/installation/windows/

<!---HONumber=August15_HO8-->