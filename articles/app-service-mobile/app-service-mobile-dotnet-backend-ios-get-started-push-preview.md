<properties
	pageTitle="Incorporación de notificaciones push a la aplicación iOS con las Aplicaciones móviles de Azure"
	description="Obtenga información acerca de cómo usar las Aplicaciones móviles de Azure para enviar notificaciones push a su aplicación iOS."
	services="app-service\mobile"
	documentationCenter="ios"
	manager="dwrede"
	editor=""
	authors="krisragh"/>

<tags
	ms.service="app-service-mobile"
	ms.workload="mobile"
	ms.tgt_pltfrm="mobile-ios"
	ms.devlang="objective-c"
	ms.topic="article"
	ms.date="07/29/2015"
	ms.author="krisragh"/>


# Incorporación de notificaciones push a la aplicación iOS

[AZURE.INCLUDE [app-service-mobile-selector-get-started-push-preview](../../includes/app-service-mobile-selector-get-started-push-preview.md)]&nbsp;[AZURE.INCLUDE [app-service-mobile-note-mobile-services-preview](../../includes/app-service-mobile-note-mobile-services-preview.md)]

En este tutorial, agregará notificaciones push al proyecto de [inicio rápido de iOS] para que cada vez que se inserte un registro, se envíe una notificación push. Este tutorial está basado en el tutorial de [inicio rápido de iOS], que debe completar primero. El [simulador de iOS no admite notificaciones push](https://developer.apple.com/library/ios/documentation/IDEs/Conceptual/iOS_Simulator_Guide/TestingontheiOSSimulator.html), por lo que, para este tutorial, necesita un dispositivo iOS físico y una [suscripción al programa para desarrolladores de Apple](https://developer.apple.com/programs/ios/).

## <a id="register"></a>Registro de aplicaciones para notificaciones push

[AZURE.INCLUDE [Habilitación de notificaciones de inserción de Apple](../../includes/enable-apple-push-notifications.md)]

## Configuración de Azure para enviar notificaciones push

[AZURE.INCLUDE [app-service-mobile-apns-configure-push-preview](../../includes/app-service-mobile-apns-configure-push-preview.md)]

##<a name="review"></a>Revisión de la configuración del proyecto de servidor (opcional)

[AZURE.INCLUDE [app-service-mobile-dotnet-backend-enable-push-preview](../../includes/app-service-mobile-dotnet-backend-enable-push-preview.md)]

##<a id="update-server"></a>Actualización del proyecto de servidor para enviar notificaciones push

[AZURE.INCLUDE [app-service-mobile-dotnet-backend-configure-push-apns](../../includes/app-service-mobile-dotnet-backend-configure-push-apns.md)]

## <a name="publish-the-service"></a>Implementación del proyecto de servidor en Azure

[AZURE.INCLUDE [app-service-mobile-dotnet-backend-publish-service-preview](../../includes/app-service-mobile-dotnet-backend-publish-service-preview.md)]

## <a id="add-push"></a>Incorporación de notificaciones push a la aplicación

[AZURE.INCLUDE [Agregar notificaciones push a la aplicación](../../includes/app-service-add-push-notifications-to-app.md)]

## <a id="test"></a>Prueba de las notificaciones push en la aplicación

[AZURE.INCLUDE [Prueba de las notificaciones push en la aplicación](../../includes/test-push-notifications-in-app.md)]

<!-- Anchors.  -->
[Generate iOS certificate signing request]: #certificates
[Register your app and enable push notifications]: #register
[Create a provisioning profile for the app]: #profile
[Add push notifications to the app]: #add-push
[Configure your mobile backend to send push requests]: #configure
[Update the server to send push notifications]: #update-server
[Publish the mobile backend to Azure]: #publish-mobile-service
[Test your app]: #test-the-service

<!-- Images. -->
[5]: ./media/mobile-services-ios-get-started-push/mobile-services-ios-push-step5.png
[6]: ./media/mobile-services-ios-get-started-push/mobile-services-ios-push-step6.png
[7]: ./media/mobile-services-ios-get-started-push/mobile-services-ios-push-step7.png

[9]: ./media/mobile-services-ios-get-started-push/mobile-services-ios-push-step9.png
[10]: ./media/mobile-services-ios-get-started-push/mobile-services-ios-push-step10.png
[17]: ./media/mobile-services-ios-get-started-push/mobile-services-ios-push-step17.png
[18]: ./media/mobile-services-ios-get-started-push/mobile-services-selection.png
[19]: ./media/mobile-services-ios-get-started-push/mobile-push-tab-ios.png
[20]: ./media/mobile-services-ios-get-started-push/mobile-push-tab-ios-upload.png
[21]: ./media/mobile-services-ios-get-started-push/mobile-portal-data-tables.png
[22]: ./media/mobile-services-ios-get-started-push/mobile-insert-script-push2.png
[23]: ./media/mobile-services-ios-get-started-push/mobile-quickstart-push1-ios.png
[24]: ./media/mobile-services-ios-get-started-push/mobile-quickstart-push2-ios.png
[25]: ./media/mobile-services-ios-get-started-push/mobile-quickstart-push3-ios.png
[26]: ./media/mobile-services-ios-get-started-push/mobile-quickstart-push4-ios.png
[28]: ./media/mobile-services-ios-get-started-push/mobile-services-ios-push-step18.png

[101]: ./media/mobile-services-ios-get-started-push/mobile-services-ios-push-01.png
[102]: ./media/mobile-services-ios-get-started-push/mobile-services-ios-push-02.png
[103]: ./media/mobile-services-ios-get-started-push/mobile-services-ios-push-03.png
[104]: ./media/mobile-services-ios-get-started-push/mobile-services-ios-push-04.png
[105]: ./media/mobile-services-ios-get-started-push/mobile-services-ios-push-05.png
[106]: ./media/mobile-services-ios-get-started-push/mobile-services-ios-push-06.png
[107]: ./media/mobile-services-ios-get-started-push/mobile-services-ios-push-07.png
[108]: ./media/mobile-services-ios-get-started-push/mobile-services-ios-push-08.png

[110]: ./media/mobile-services-ios-get-started-push/mobile-services-ios-push-10.png
[111]: ./media/mobile-services-ios-get-started-push/mobile-services-ios-push-11.png
[112]: ./media/mobile-services-ios-get-started-push/mobile-services-ios-push-12.png
[113]: ./media/mobile-services-ios-get-started-push/mobile-services-ios-push-13.png
[114]: ./media/mobile-services-ios-get-started-push/mobile-services-ios-push-14.png
[115]: ./media/mobile-services-ios-get-started-push/mobile-services-ios-push-15.png
[116]: ./media/mobile-services-ios-get-started-push/mobile-services-ios-push-16.png
[117]: ./media/mobile-services-ios-get-started-push/mobile-services-ios-push-17.png

<!-- URLs. -->
[inicio rápido de iOS]: app-service-mobile-dotnet-backend-ios-get-started-preview.md
[Install Xcode]: https://go.microsoft.com/fwLink/p/?LinkID=266532
[iOS Provisioning Portal]: http://go.microsoft.com/fwlink/p/?LinkId=272456
[Azure Mobile App iOS SDK]: https://go.microsoft.com/fwLink/?LinkID=529823
[Azure Notification Hubs Nuget]: https://www.nuget.org/packages/WindowsAzure.ServiceBus/
[Apple Push Notification Service]: http://go.microsoft.com/fwlink/p/?LinkId=272584
[Get started with Mobile Services]: ../mobile-services-dotnet-backend-ios-get-started.md
[Get Started with Mobile Apps]: app-service-mobile-dotnet-backend-ios-get-started-preview.md
[Azure Management Portal]: https://manage.windowsazure.com/
[apns object]: http://go.microsoft.com/fwlink/p/?LinkId=272333
 

<!---HONumber=August15_HO8-->