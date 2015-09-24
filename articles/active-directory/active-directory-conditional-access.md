<properties
	pageTitle="Administración de riesgos con el acceso condicional"
	description="Tema en el que se explica cómo permitir el acceso desde cualquier lugar a recursos específicos desde dispositivos conocidos compatibles con las directivas. También se explica cómo denegar el acceso desde dispositivos perdidos, robados o no compatibles."
	services="active-directory, virtual-network"
	documentationCenter=""
	authors="femila"
	manager="stevepo"
	editor=""/>

<tags
	ms.service="active-directory"
	ms.devlang="na"
	ms.topic="article"
  ms.tgt_pltfrm="na"
  ms.workload="identity" 
	ms.date="07/31/2015"
	ms.author="femila"/>


# Administración de riesgos con el acceso condicional

Las tendencias actuales relacionadas con el método de trabajo de los empleados, su forma de ser productivos y los medios con los que cuentan para hacer su trabajo están cambiando rápidamente. Los empleados llevan sus dispositivos personales al trabajo. En estos dispositivos personales, tienen aplicaciones que utilizan en su vida personal digital. Y están acostumbrados a la libertad y las funcionalidades que estos les proporcionan. Desean usar estas mismas aplicaciones en el trabajo, y quieren que su vida laboral tenga la misma flexibilidad de la que gozan en su vida personal digital. Los empleados corporativos de hoy en día esperan poder trabajar desde cualquier ubicación en los dispositivos que deseen, así como conectarse y acceder sin problemas a las aplicaciones empresariales.

Esta flexibilidad de permitir a los usuarios trabajar dónde, cuándo y cómo deseen conlleva un riesgo mayor. No en vano, son multitud los datos que demuestran que estos dispositivos se pueden robar, traspapelar o perder. En muchos de estos smartphones y tabletas hay una cantidad increíble de contenido corporativo y de clientes de carácter confidencial Este es el equilibrio perfecto que los arquitectos de TI, los expertos en seguridad y los administradores de TI intentan mantener. Es el equilibrio entre permitir a los usuarios ser productivos en los dispositivos que les gusta usar y, al mismo tiempo, ofrecer el nivel adecuado de protección y seguridad para el contenido corporativo que reside en ellos.

Con las diversas funcionalidades de acceso que se ofrecen a través de Azure Active Directory, Office 365 y Microsoft Intune, los administradores de TI pueden solucionar cuestiones como las siguientes:

- Permitir a los empleados el acceso en cualquier parte, sin abrir la puerta a nadie en Internet.
- Permitir el acceso desde cualquier parte a recursos específicos con dispositivos conocidos que sean compatibles con las directivas.
- Denegar el acceso desde dispositivos perdidos, robados o no compatibles.

![][1]

## Pasos siguientes

En los temas siguientes, se describen los distintos mecanismos que existen para el establecimiento de directivas de acceso condicional en su organización.

- [Descripción general sobre el registro de dispositivos de Azure Active Directory](https://msdn.microsoft.com/library/azure/dn903763.aspx)
- [Configuración del acceso condicional local mediante el registro de dispositivos de Azure Active Directory](https://msdn.microsoft.com/library/azure/dn788908.aspx)
- [Directivas de dispositivo de acceso condicional para servicios de Office 365](https://msdn.microsoft.com/library/azure/dn903766.aspx)
- [Acceso condicional de Azure en versión de vista previa para aplicaciones SaaS](https://msdn.microsoft.com/library/azure/dn906877.aspx)


<!--Image references-->
[1]: ./media/active-directory-conditional-access/condaccoverviewvsdx1.png

<!---HONumber=August15_HO6-->