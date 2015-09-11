<properties 
	pageTitle="¿Qué son las aplicaciones lógicas?" 
	description="Obtenga más información acerca de las Aplicaciones lógicas del Servicio de aplicaciones" 
	authors="kevinlam1" 
	manager="dwrede" 
	editor="" 
	services="app-service\logic" 
	documentationCenter=""/>

<tags
	ms.service="app-service-logic"
	ms.workload="integration"
	ms.tgt_pltfrm="na"
	ms.devlang="na"
	ms.topic="article"
	ms.date="07/08/2015"
	ms.author="klam"/>

#¿Qué son las aplicaciones lógicas?

| Referencia rápida |
| --------------- |
| [Lenguaje de definición de aplicaciones lógicas](https://msdn.microsoft.com/library/azure/dn948512.aspx?f=255&MSPPError=-2147217396) |
| [Documentación del conector de aplicaciones lógicas](https://azure.microsoft.com/es-es/documentation/articles/app-service-logic-connectors-list/) |
| [Foro de aplicaciones lógicas](https://social.msdn.microsoft.com/Forums/es-es/home?forum=azurelogicapps) |

Servicio de aplicaciones de Azure cuenta con una administración PaaS (plataforma como servicio) completa para los desarrolladores que hace que sea más fácil crear aplicaciones web, móviles y de integración. Las aplicaciones lógicas forman parte de este conjunto y permiten a cualquier usuario técnico o el desarrollador automatizar la ejecución de procesos de negocio y flujo de trabajo a través de un diseñador visual fácil de usar.

Lo mejor de todo, las aplicaciones lógicas pueden combinarse con [conectores][connectors] de nuestro Marketplace para ayudar a resolver incluso escenarios de integración complicados de forma fácil.

![Diseñador de aplicación de flujo](./media/app-service-logic-what-are-logic-apps/Designer.png)

Si desea replicar automáticamente nuevos registros en la Base de datos de SQL y enviarlos correo a la recepción, o encontrar tweets negativos y enviarlos a un canal de inactividad

##¿Por qué aplicaciones lógicas?

Las aplicaciones lógicas permiten a los desarrolladores diseñar flujos de trabajo que comienzan por desencadenar y luego ejecutar una serie de pasos, que invocan a una aplicación de API del Servicio de aplicaciones, al tiempo que se ocupan de forma segura de la autenticación y los procedimientos recomendados, como ejecución duradera.

Si desea automatizar cualquier proceso empresarial (por ejemplo, encontrar tweets negativos y publicar en el canal de inactividad interno, o replicar nuevos registros de clientes de SQL, como lleguen, en el sistema CRM), las aplicaciones lógicas realizan la integración de orígenes de datos dispares de la nube al entorno local de forma sencilla. Consulte nuestros [conectores][connectors] para obtener más ideas y [comenzar a][create] ahora para ver qué puede hacer.

Además, con nuestras aplicaciones de [API de BizTalk][biztalk], puede realizar la escalación a escenarios de integración maduros con la eficacia de un [motor de reglas][rules], la [administración de socios comerciales][tpm] y mucho más.

- **Herramientas de diseño fáciles de usar**: las aplicaciones lógicas pueden diseñarse de principio a fin en el explorador. Inicio con un desencadenador: desde una simple programación hasta siempre vez que aparezcas un tweet acerca de su compañía. A continuación, se coordina cualquier número de acciones mediante la sofisticada galería de conectores.

- **Crear fácilmente SaaS**: incluso las tareas de composición que son fáciles de describir son difíciles de implementar en el código. Las aplicaciones lógicas hacen muy fácil de conectar sistemas dispares. ¿Desea crear una tarea en el software CRM que se basa en la actividad de las cuentas de Facebook o Twitter? ¿Desea conectar su solución de marketing en la nube a su sistema de facturación local? Las aplicaciones lógicas son la forma más rápida y más confiable para ofrecer soluciones a estos problemas.

- **Introducción rápida desde plantillas**: para ayudarle a empezar, hemos dispuesto una [galería de plantillas][templates] que permiten crear rápidamente algunas soluciones comunes. Desde soluciones avanzadas de BizTalk a la conectividad de SaaS simple e incluso algunos que sean solo por diversión: la galería es la forma más rápida de comprender la eficacia de las aplicaciones lógicas.

- **Extensibilidad incorporada**: ¿no ve el conector que necesita? Las aplicaciones lógicas forman parte del conjunto de aplicaciones Servicio de aplicaciones y están diseñadas para trabajar con Aplicaciones de API; puede crear fácilmente su propia aplicación de API que se usará como un conector. Cree una nueva aplicación solo para usted, o bien compártala y rentabilícela en el marketplace.

- **Eficacia de integración real**: empiece por algo sencillo y crezca a medida que sea necesario. Las aplicaciones lógicas pueden aprovechar con facilidad la eficacia de BizTalk, la solución de integración líder del sector de Microsoft para permitir a los profesionales de integración crear las soluciones que necesitan. Obtenga más información sobre las [capacidades de BizTalk proporcionadas con Servicios de aplicaciones][biztalk].

## Conceptos de aplicaciones lógicas

Las siguientes son algunas de las principales partes que componen la experiencia de aplicaciones lógicas.

- **Flujo de trabajo**: las aplicaciones lógicas ofrecen una forma gráfica para modelar los procesos de negocio como una serie de pasos o un flujo de trabajo.
- **Conectores**: las aplicaciones lógicas necesitan tener acceso a datos y servicios. Un conector es un tipo especial de aplicación de API. Se crea específicamente para ayudarle cuando se conecta y trabaja con datos. Vea la lista de conectores disponibles ahora en [Uso de conectores][connectors].
- **Desencadenadores**: algunos conectores también pueden actuar como un desencadenador. Un desencadenador inicia una nueva instancia de un flujo de trabajo de acuerdo con un evento específico, como la llegada de un correo electrónico o un cambio en la cuenta de almacenamiento de Azure.
-  **Acciones**: cada paso después de la llamada a la acción de un desencadenador en un flujo de trabajo. Cada acción normalmente se asigna a una operación en el conector o aplicaciones de API personalizadas.
- **BizTalk**: para escenarios más avanzados de integración, Servicios de aplicaciones de Azure incluye capacidades de BizTalk. BizTalk es la plataforma de integración líder del sector de Microsoft. Las Aplicaciones de API de BizTalk permiten incluir fácilmente la validación, transformación, reglas y mucho más en sus flujos de trabajo de aplicaciones lógicas. Obtenga más información en [Qué son las Aplicaciones de API de BizTalk][biztalk].

## Introducción

Para comenzar con las aplicaciones lógicas, siga el tutorial [Creación de una aplicación lógica][create].

Para obtener más información sobre la plataforma de Servicio de aplicaciones de Azure, consulte [Servicio de aplicaciones de Azure][appservice].

[biztalk]: app-service-logic-what-are-biztalk-api-apps.md
[appservice]: ../app-service/app-service-value-prop-what-is.md
[create]: app-service-logic-create-a-logic-app.md
[connectors]: app-service-logic-connectors-list.md
[tpm]: app-service-logic-create-a-trading-partner-agreement.md
[rules]: app-service-logic-use-biztalk-rules.md
[templates]: app-service-logic-use-logic-app-templates.md
 

<!---HONumber=August15_HO8-->