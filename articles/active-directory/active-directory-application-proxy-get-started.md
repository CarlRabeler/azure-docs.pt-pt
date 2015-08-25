<properties
	pageTitle="Provisión de acceso remoto seguro a aplicaciones locales"
	description="Explica cómo utilizar el proxy de la aplicación de Azure AD para proporcionar acceso remoto seguro a sus aplicaciones locales."
	services="active-directory"
	documentationCenter=""
	authors="rkarlin"
	manager="terrylan"
	editor=""/>

<tags
	ms.service="active-directory"
	ms.workload="identity"
	ms.tgt_pltfrm="na"
	ms.devlang="na"
	ms.topic="article"
	ms.date="07/07/2015"
	ms.author="rkarlin"/>

# Provisión de acceso remoto seguro a aplicaciones locales

Desea proporcionar acceso a los usuarios remotos que tienen todo tipo de dispositivos: administrados, no administrados; tabletas, smartphones y equipos portátiles. Y proporcionar un acceso seguro a un sinfín de recursos puede ser una tarea compleja. En los últimos años, los proxy inversos se convirtieron en un medio popular de proporcionar acceso remoto seguro, pero era necesario que estos se encontrasen detrás de firewalls que eran difíciles de proteger y cuya alta disposición era compleja.

## Acceso remoto seguro en la nube
En un entorno de nube moderno, llevamos el acceso remoto al siguiente nivel mediante el proxy de la aplicación en Microsoft Azure Active Directory. El proxy de la aplicación es una característica de Azure AD que se proporciona como un servicio, lo que significa que es fácil de implementar y usar. Se integra con Azure Active Directory, la misma plataforma de identidad utilizada por Office 365.

## ¿Qué es el proxy de la aplicación de Azure Active Directory?
El proxy de la aplicación proporciona un inicio de sesión único y acceso remoto seguro para las aplicaciones web hospedadas de forma local, como los sitios de SharePoint y Outlook Web Access. Ahora es posible tener acceso a las aplicaciones web locales del mismo modo que a las aplicaciones SaaS en Azure Active Directory, sin necesidad de una red privada virtual o de cambios en la infraestructura de red. El proxy de la aplicación permite publicar aplicaciones. Los empleados pueden iniciar sesión en las aplicaciones desde sus propios dispositivos y autenticarse a través de este proxy basado en la nube.

## ¿Cómo funciona?
### Habilitación del acceso
El proxy de la aplicación funciona mediante la instalación de un pequeño servicio de Windows Server denominado conector dentro de la red. El conector no requiere abrir puertos de entrada y no tiene que poner nada en la red perimetral. Si tiene mucho tráfico a sus aplicaciones, puede agregar más conectores, y el servicio se encargará de mantener el equilibrio de carga. Los conectores no tienen estado y extraen todo el contenido de la nube según sea necesario. Cuando un usuario tiene acceso a las aplicaciones de forma remota, desde cualquier dispositivo, se autentica por parte de Azure Active Directory y obtiene acceso a la aplicación.

### Inicio de sesión único
El proxy de la aplicación de Azure AD proporciona el inicio de sesión único (SSO) para las aplicaciones que usan aplicaciones para notificaciones o IWA. Si la aplicación utiliza la autenticación integrada de Windows, el proxy de la aplicación suplanta al usuario mediante la delegación limitada de Kerberos para proporcionar SSO. El SSO para aplicaciones para notificaciones es posible porque ya se ha autenticado el usuario por parte de Azure Active Directory.

## Primeros pasos
Asegúrese de que tiene una suscripción de nivel Básico o Premium de Azure AD y un directorio de Azure AD del cual es usted administrador global. También necesita licencias de nivel Básico o Premium de Azure AD para el administrador de directorios y los usuarios que tienen acceso a las aplicaciones. Eche un vistazo a continuación para obtener más información.

### Introducción a la habilitación del acceso remoto a aplicaciones locales
La configuración del proxy de la aplicación se realiza en dos pasos:

1. [Habilitación del proxy de la aplicación y configuración del conector](active-directory-application-proxy-enable.md)<br>
2. [Publicar aplicaciones](active-directory-application-proxy-publish.md). Use el asistente rápido y sencillo para que sus aplicaciones locales se publiquen y sean accesibles de manera remota.

## Pasos siguientes
Hay mucho más que puede hacer con el proxy de la aplicación:


- [Publicar aplicaciones mediante su propio nombre de dominio](https://msdn.microsoft.com/library/azure/mt210927.aspx)
- [Habilitar el inicio de sesión único](https://msdn.microsoft.com/library/azure/dn879065.aspx)
- [Trabajar con las aplicaciones para notificaciones](https://msdn.microsoft.com/library/azure/mt210926.aspx)
- [Habilitar el acceso condicional](https://msdn.microsoft.com/library/azure/dn931796.aspx)


### Obtenga más información acerca del proxy de la aplicación
- [Eche un vistazo aquí para ver nuestra ayuda en línea](https://msdn.microsoft.com/library/azure/dn768219.aspx)
- [Consulte el blog del proxy de la aplicación](http://blogs.technet.com/b/applicationproxyblog/)
- [Vea nuestros vídeos de Channel 9](http://channel9.msdn.com/events/Ignite/2015/BRK3864)

## Recursos adicionales
* [Registro en Azure como una organización](../sign-up-organization.md)
* [Identidad de Azure](../fundamentals-identity.md)

<!---HONumber=August15_HO6-->