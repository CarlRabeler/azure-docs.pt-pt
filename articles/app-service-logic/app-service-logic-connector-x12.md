<properties 
   pageTitle="Conector X12 de BizTalk" 
   description="Conector X12 de BizTalk" 
   services="app-service\logic" 
   documentationCenter=".net,nodejs,java" 
   authors="rajeshramabathiran" 
   manager="dwrede" 
   editor=""/>

<tags
   ms.service="app-service-logic"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="integration" 
   ms.date="06/14/2015"
   ms.author="rajram"/>

#Conector X12 de BizTalk
El conector X12 de Microsoft Azure le permite recibir y enviar mensajes mediante el protocolo X12 en comunicaciones negocio a negocio. X12 se conoce también comúnmente como ASC X12, o Accredited Standards Committee X12. Su uso está muy extendido en las industrias.

##Requisitos previos
- Aplicación de API TPM: antes de crear un conector X12, debe crear un [conector de administración de socios comerciales de BizTalk][1]
- Base de datos SQL de Azure: cada una de las aplicaciones de API B2B requiere su propia base de datos SQL de Azure.
- Bus de servicio de Azure: es opcional y se usa solo en el caso de procesamiento por lotes.

##Uso del conector X12 de BizTalk
Para usar el conector X12 de BizTalk, primero debe crear una instancia de la aplicación de API del conector X12 de BizTalk. Esta tarea puede realizarse en línea mediante la creación de una aplicación lógica, o bien seleccionando la aplicación de API del conector X12 de BizTalk en Azure Marketplace.

##Configuración del conector X12 de BizTalk
Los socios comerciales son las entidades implicadas en comunicaciones B2B (negocio a negocio). Cuando dos asociados establecen una relación, esto se conoce como contrato. El contrato definido se basa en la comunicación que los dos socios desean lograr y es específico del protocolo o el transporte.

Los pasos necesarios para crear un contrato de socio comercial se documentan [aquí][2]

##Uso del conector X12 en la superficie del diseñador de aplicaciones lógicas
El conector X12 puede usarse como un desencadenador o como una acción.

###Desencadenador
- Inicie el diseñador de flujo de aplicaciones lógicas de Azure.
- Haga clic en el conector X12 en el panel derecho.

	![Configuración del desencadenador][3]
- Haga clic en ->

	![Opciones del desencadenador][4]
- El conector X12 expone un solo desencadenador. Seleccione *Lote de versión*

	![Entrada de lote de versión][5]
- Este desencadenador no tiene ninguna entrada. Haga clic en ->

	![Lote de versión configurado][6]
- Como parte de la salida, el conector devuelve la carga X12, el identificador del contrato e información sobre si el mensaje se procesa por lotes o no.

###Acción
- Haga clic en el conector X12 en el panel derecho.

	![Configuración de la acción][7]
- Haga clic en ->

	![Lista de acciones][8]
- El conector X12 admite muchas acciones. Seleccione *Codificar*.

	![Codificar entrada][9]
- Proporcione las entradas para la acción y configúrela.

	![Codificar configurado][10]

Parámetro|Tipo|Descripción del parámetro
---|---|---
Contenido|cadena|Mensaje XML
Id. de contrato|cadena|Id. de contrato
Es mensaje por lotes|cadena|Es mensaje por lotes

La acción devuelve un objeto que contiene la carga X12.

## Aplicaciones adicionales del conector
Una vez creado el conector, puede agregarlo a un flujo empresarial mediante una aplicación lógica. Consulte [¿Qué son las aplicaciones lógicas?](app-service-logic-what-are-logic-apps.md)

También puede consultar las estadísticas de rendimiento y la seguridad de control para el conector. Consulte [Administración y supervisión de conectores y aplicaciones de API](../app-service-api/app-service-api-manage-in-portal.md).


<!--References -->
[1]: app-service-logic-connector-tpm.md
[2]: app-service-logic-create-a-trading-partner-agreement.md
[3]: ./media/app-service-logic-connector-x12/TriggerSettings.PNG
[4]: ./media/app-service-logic-connector-x12/ListOfTriggers.PNG
[5]: ./media/app-service-logic-connector-x12/ReleaseBatchTriggerInput.PNG
[6]: ./media/app-service-logic-connector-x12/ReleaseBatchTriggerConfigured.PNG
[7]: ./media/app-service-logic-connector-x12/ActionSettings.PNG
[8]: ./media/app-service-logic-connector-x12/ListOfActions.PNG
[9]: ./media/app-service-logic-connector-x12/EncodeInput.PNG
[10]: ./media/app-service-logic-connector-x12/EncodeConfigured.PNG
[11]: ./media/app-service-logic-connector-x12/TriggerSettings.PNG

<!---HONumber=August15_HO6-->