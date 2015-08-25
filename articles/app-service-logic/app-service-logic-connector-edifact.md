<properties 
   pageTitle="Conector Edifact de BizTalk" 
   description="Conector Edifact de BizTalk" 
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

#Conector Edifact de BizTalk
El servicio Edifact de Microsoft Azure le permite recibir y enviar mensajes mediante el protocolo Edifact en comunicaciones negocio a negocio. Edifact se conoce también comúnmente como ASC Edifact, o Accredited Standards Committee Edifact. Su uso está muy extendido en las industrias.

##Requisitos previos
- Aplicación de API TPM: antes de crear un conector Edifact, debe crear un [conector de administración de socios comerciales de BizTalk][1]
- Base de datos SQL de Azure: cada una de las aplicaciones de API B2B requiere su propia base de datos SQL de Azure.
- Bus de servicio de Azure: es opcional y se usa solo en el caso de procesamiento por lotes.

##Uso del conector Edifact de BizTalk
Para usar el conector Edifact, deberá crear primero una instancia de la aplicación de API del conector AS2. Esta tarea puede realizarse en línea mediante la creación de una aplicación lógica, o bien seleccionando la aplicación de API del conector AS2 en Azure Marketplace.

##Configuración del conector Edifact de BizTalk
Los socios comerciales son las entidades implicadas en comunicaciones B2B (negocio a negocio). Cuando dos asociados establecen una relación, esto se conoce como contrato. El contrato definido se basa en la comunicación que los dos socios desean lograr y es específico del protocolo o el transporte.

Los pasos necesarios para crear un contrato de socio comercial se documentan [aquí][2]

##Uso del conector Edifact en la superficie del diseñador de aplicaciones lógicas
El conector Edifact puede usarse como un desencadenador o como una acción.

###Desencadenador
- Inicie el diseñador de flujo de aplicaciones lógicas de Azure.
- Haga clic en el conector Edifact en el panel derecho.

	![Configuración del desencadenador][3]
- Haga clic en ->

	![Opciones del desencadenador][4]
- El conector Edifact expone un solo desencadenador. Seleccione *Lote de versión*

	![Entrada de lote de versión][5]
- Este desencadenador no tiene ninguna entrada. Haga clic en ->

	![Lote de versión configurado][6]
- Como parte de la salida, el conector devuelve la carga Edifact, el identificador del contrato e información sobre si el mensaje se procesa por lotes o no.

###Acción
- Haga clic en el conector Edifact en el panel derecho.

	![Configuración de la acción][7]
- Haga clic en ->

	![Lista de acciones][8]
- El conector Edifact admite muchas acciones. Seleccione *Codificar*.

	![Codificar entrada][9]
- Proporcione las entradas para la acción y configúrela.

	![Codificar configurado][10]

Parámetro|Tipo|Descripción del parámetro
---|---|---
Contenido|cadena|Mensaje XML
Id. de contrato|integer|Id. de contrato
Es mensaje por lotes|boolean|Es mensaje por lotes
Separador de elementos de datos|cadena|Separador de elementos de datos
Separador de componentes|cadena|Separador de componentes
Terminador de segmento|cadena|Terminador de segmento
Indicador de coma decimal|cadena|Indicador de coma decimal
Separador de repeticiones|cadena|Separador de repeticiones
Carácter de escape|cadena|Carácter de escape
Carácter de reemplazo|cadena|Carácter de reemplazo
Sufijo de terminador de segmento|cadena|Sufijo de terminador de segmento

La acción devuelve un objeto que contiene la carga EDIFACT para indicar que se ha realizado con éxito.

## Aplicaciones adicionales del conector
Una vez creado el conector, puede agregarlo a un flujo empresarial mediante una aplicación lógica. Consulte [¿Qué son las aplicaciones lógicas?](app-service-logic-what-are-logic-apps.md)

También puede consultar las estadísticas de rendimiento y la seguridad de control para el conector. Consulte [Administración y supervisión de conectores y aplicaciones de API](../app-service-api/app-service-api-manage-in-portal.md).


<!--References -->
[1]: app-service-logic-connector-tpm.md
[2]: app-service-logic-create-a-trading-partner-agreement.md
[3]: ./media/app-service-logic-connector-edifact/TriggerSettings.PNG
[4]: ./media/app-service-logic-connector-edifact/ListOfTriggers.PNG
[5]: ./media/app-service-logic-connector-edifact/ReleaseBatchTriggerInput.PNG
[6]: ./media/app-service-logic-connector-edifact/ReleaseBatchTriggerConfigured.PNG
[7]: ./media/app-service-logic-connector-edifact/ActionSettings.PNG
[8]: ./media/app-service-logic-connector-edifact/ListOfActions.PNG
[9]: ./media/app-service-logic-connector-edifact/EncodeInput.PNG
[10]: ./media/app-service-logic-connector-edifact/EncodeConfigured.PNG

<!---HONumber=August15_HO6-->