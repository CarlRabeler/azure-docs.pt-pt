<properties
   pageTitle="Guía del desarrollador de Azure Active Directory | Microsoft Azure"
   description="Este artículo ofrece una guía completa sobre recursos orientados al desarrollador para Azure Active Directory."
   services="active-directory"
   documentationCenter="dev-center-name"
   authors="msmbaldwin"
   manager="mbaldwin"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.devlang="na"
   ms.topic="hero-article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="08/12/2015"
   ms.author="mbaldwin"/>


# Guía del desarrollador de Azure Active Directory

## Información general
Al ser una plataforma de administración de identidades como servicio (IDMaaS), Azure Active Directory proporciona a los desarrolladores una manera eficaz de integrar la administración de identidades en sus aplicaciones. Los siguientes artículos ofrecen información general sobre la implementación y las características clave de Azure Active Directory. Se recomienda leerlos por orden o, si desea profundizar en el tema, ir directamente a [Introducción](#getting-started).


1. [Integración con Azure Active Directory](active-directory-how-to-integrate.md): descubra por qué la integración con Azure Active Directory ofrece la mejor solución para lograr una autorización y un inicio de sesión seguros.

1. [Escenarios de autenticación de Active Directory](active-directory-authentication-scenarios.md): aproveche la autenticación simplificada de Azure Active Directory para proporcionar inicio de sesión en la aplicación.

1. [API Graph de Azure Active Directory](https://msdn.microsoft.com/library/azure/hh974476.aspx): use la API Graph de Azure Active Directory para tener acceso mediante programación a Azure Active Directory a través de extremos de la API de REST.

1. [Integración de aplicaciones en Azure Active Directory](active-directory-integrating-applications.md): obtenga información sobre el registro de la aplicación y las directrices de personalización de marca para aplicaciones multiempresa.

1. [Bibliotecas de autenticación de Azure Active Directory](active-directory-authentication-libraries.md): autentique usuarios fácilmente para obtener tokens de acceso con las bibliotecas de autenticación de Azure.

Para ver información general de Azure Active Directory presentada en la conferencia Build 2015, consulte la sección [Vídeos](#videos) a continuación.


## Introducción

Estos tutoriales están adaptados para varias plataformas y pueden ayudarle a empezar a desarrollar rápidamente con Azure Active Directory. Como requisito previo, debe [obtener un inquilino de Azure Active Directory](active-directory-howto-tenant.md).

### Guías de inicio rápido de aplicaciones para móvil o PC

|[![iOS](./media/active-directory-developers-guide/ios.png)](active-directory-devquickstarts-ios.md)|[![Android](./media/active-directory-developers-guide/android.png)](active-directory-devquickstarts-android.md)|[![.NET](./media/active-directory-developers-guide/net.png)](active-directory-devquickstarts-dotnet.md)| [![Windows Phone](./media/active-directory-developers-guide/windows.png)](active-directory-devquickstarts-windowsphone.md)|[![Tienda Windows](./media/active-directory-developers-guide/windows.png)](active-directory-devquickstarts-windowsstore.md)|[![Xamarin](./media/active-directory-developers-guide/xamarin.png)](active-directory-devquickstarts-xamarin.md)|[![Cordova](./media/active-directory-developers-guide/cordova.png)](active-directory-devquickstarts-cordova.md)
|:--:|:--:|:--:|:--:|:--:|:--:|:--:
|[iOS](active-directory-devquickstarts-ios.md)|[Android](active-directory-devquickstarts-android.md)|[.NET](active-directory-devquickstarts-dotnet.md)|[Windows Phone](active-directory-devquickstarts-windowsphone.md)|[Tienda Windows](active-directory-devquickstarts-windowsstore.md)|[Xamarin](active-directory-devquickstarts-xamarin.md)|[Cordova](active-directory-devquickstarts-cordova.md)


### Guías de inicio rápido de API web o aplicación web

|[![.NET](./media/active-directory-developers-guide/net.png)](active-directory-devquickstarts-webapp-dotnet.md)|[![.NET](./media/active-directory-developers-guide/net.png)](active-directory-devquickstarts-webapi-dotnet.md)|[![Javascript](./media/active-directory-developers-guide/javascript.png)](active-directory-devquickstarts-angular.md)|[![Node.js](./media/active-directory-developers-guide/nodejs.png)](active-directory-devquickstarts-webapi-nodejs.md)
|:--:|:--:|:--:|:--:
|[Aplicación web .NET](active-directory-devquickstarts-webapp-dotnet.md)|[API web .NET](active-directory-devquickstarts-webapi-dotnet.md)|[Javascript](active-directory-devquickstarts-angular.md)|[Node.js](active-directory-devquickstarts-webapi-nodejs.md)


## Procedimientos

En estos artículos se describe cómo realizar tareas específicas con Azure Active Directory:

- [Obtención de un inquilino de Azure Active Directory](active-directory-howto-tenant.md)
- [Anuncio de la aplicación en la galería de aplicaciones de Azure Active Directory](active-directory-app-gallery-listing.md)
- [Creación de una aplicación con las API de Office 365](https://msdn.microsoft.com/office/office365/howto/getting-started-Office-365-APIs)
- [Envío de aplicaciones web para Office 365 al panel de vendedores](https://msdn.microsoft.com/office/office365/howto/submit-web-apps-seller-dashboard)
- [Vista previa: cómo compilar aplicaciones que inician sesión para los usuarios tanto con cuentas personales como profesionales o educativas](active-directory-appmodel-v2-overview.md)


## Referencia

Estos artículos proporcionan información básica sobre las API de REST y de la biblioteca de autenticación, los protocolos, los errores, los ejemplos de código y los extremos.

###  Soporte técnico
- [Preguntas con etiquetas](http://stackoverflow.com/questions/tagged/azure-active-directory): encuentre soluciones de Azure Active Directory en Stackoverflow buscando las etiquetas [azure-active-directory](http://stackoverflow.com/questions/tagged/azure-active-directory) y [adal](http://stackoverflow.com/questions/tagged/adal).

### Código

- [Bibliotecas de código abierto de Azure Active Directory](http://github.com/AzureAD): la manera más fácil de encontrar el código fuente de una biblioteca es usar la [lista de la biblioteca](active-directory-authentication-libraries.md).

- [Ejemplos de Azure Active Directory](http://github.com/AzureADSamples): la forma más sencilla de navegar por la lista de ejemplos es usar el [índice de ejemplos de código](active-directory-code-samples.md).


### API Graph

- [Referencia de API Graph](https://msdn.microsoft.com/library/azure/hh974476.aspx): referencia de REST para la API Graph de Azure Active Directory. [Vea la experiencia interactiva con la referencia de API Graph](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/api-catalog).

- [Ámbitos de los permisos de API Graph](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/graph-api-permission-scopes): ámbitos de los permisos de OAuth 2.0 que sirven para controlar el acceso que una aplicación tiene a los datos de directorio en un inquilino.


### Protocolos de autenticación

- [Referencia del protocolo SAML 2.0](https://msdn.microsoft.com/library/azure/dn195591.aspx): el protocolo SAML 2.0 permite que las aplicaciones ofrezcan una experiencia de inicio de sesión único a sus usuarios.


- [Referencia del protocolo OAuth 2.0](https://msdn.microsoft.com/library/azure/dn645545.aspx): puede usar el protocolo OAuth 2.0 para autorizar el acceso a las aplicaciones web y las API web en su inquilino de Azure Active Directory.


- [Referencia del protocolo OpenID Connect 1.0](https://msdn.microsoft.com/library/azure/dn645541.aspx): el protocolo OpenID Connect 1.0 amplía OAuth 2.0 para su uso como protocolo de autenticación.


- [Referencia del protocolo WS-Federation 1.2](https://msdn.microsoft.com/library/azure/dn903702.aspx): el protocolo WS-Federation 1.2 se describe en la especificación Web Services Federation, versión 1.2.

- [Tipos de token y notificación admitidos](active-directory-token-and-claims.md): puede usar esta guía para entender y evaluar las notificaciones en los tokens web JSON (JWT) y SAML 2.0.

## Vídeos

### Build 2015

Los ponentes de estas presentaciones de información general sobre el desarrollo de aplicaciones con características de Azure Active Directory trabajan directamente en el equipo de ingeniería. Las presentaciones tratan temas fundamentales, entre los que se incluyen IDMaaS, autenticación, federación de identidades e inicio de sesión único.

- [Azure Active Directory: Administración de identidades como servicio para aplicaciones modernas](http://azure.microsoft.com/documentation/videos/build-2015-azure-active-directory-identity-management-as-a-service-for-modern-applications)
- [Desarrollo de aplicaciones web modernas con Azure Active Directory](http://azure.microsoft.com/documentation/videos/build-2015-develop-modern-web-applications-with-azure-active-directory)
- [Desarrollo de aplicaciones nativas modernas con Azure Active Directory](http://azure.microsoft.com/documentation/videos/build-2015-develop-modern-native-applications-with-azure-active-directory)

### Azure Friday
[Azure Friday](http://azure.microsoft.com/documentation/videos/azure-friday/) es una serie periódica de vídeos de 1:1 que se publica los viernes y ofrece entrevistas breves (de 10 a 15 minutos) a expertos en diversos temas de Azure. Utilice la característica de filtro de servicios de la página para ver todos los vídeos de Azure Active Directory.

- [Identidad de Azure 101](http://azure.microsoft.com/documentation/videos/azure-identity-basics/)
- [Identidad de Azure 102](http://azure.microsoft.com/documentation/videos/azure-identity-creating-active-directory/)
- [Identidad de Azure 103](http://azure.microsoft.com/documentation/videos/azure-identity-application-to-authenticate/)

## Redes sociales

- [Blog del equipo de Active Directory](http://blogs.technet.com/b/ad/): manténgase al tanto de los últimos avances en el mundo de Azure Active Directory.

- [Blog del equipo de Azure Active Directory Graph](http://blogs.msdn.com/b/aadgraphteam): información de Azure Active Directory específica de la API Graph.

- [Identidad en la nube](http://www.cloudidentity.net): reflexiones de un PM de Azure Active Directory sobre la administración de identidades como servicio.

- [Azure Active Directory en Twitter](https://twitter.com/azuread): anuncios de Azure Active Directory en 140 caracteres o menos.

<!---HONumber=August15_HO8-->