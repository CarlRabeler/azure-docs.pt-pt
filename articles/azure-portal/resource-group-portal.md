<properties 
	pageTitle="Uso del Portal de vista previa de Azure para administrar los recursos de Azure" 
	description="Agrupe varios recursos como grupo lógico que se convierte en el límite del ciclo de vida de los recursos que contiene." 
	services="azure-portal" 
	documentationCenter="" 
	authors="" 
	writer="tfitzmac" 
	manager="wpickett" 
	editor=""/>

<tags 
	ms.service="azure-portal" 
	ms.workload="multiple" 
	ms.tgt_pltfrm="na" 
	ms.devlang="na" 
	ms.topic="article" 
	ms.date="06/22/2015" 
	ms.author="tomfitz"/>


# Uso del Portal de vista previa de Azure para administrar los recursos de Azure

## Introducción

Históricamente, la administración de un recurso (una entidad administrada por un usuario, como un servidor de base de datos, base de datos o sitio web) en Microsoft Azure precisaba realizar operaciones en un recurso al mismo tiempo Si contaba con una aplicación compleja compuesta de varios recursos, la administración de esta aplicación se convertía en una tarea compleja. En el Portal de vista previa de Azure puede crear grupos de recursos para administrar juntos todos los recursos en una aplicación. El grupo de recursos es un nuevo concepto en Azure que sirve como límite del ciclo de vida para cada recurso contenido en él.

Los grupos de recursos le permiten administrar juntos todos los recursos en una aplicación. La nueva funcionalidad de administración, Resource Manager de Azure, permite el uso de grupos de recursos. Resource Manager de Azure permite agrupar varios recursos como grupo lógico que sirve como límite del ciclo de vida para cada recurso contenido en él. Normalmente, un grupo contenderá recursos relacionados con una aplicación específica. Por ejemplo, un grupo puede contener un recurso de sitio web que hospeda el sitio web público, una Base de datos SQL que almacena datos relacionales que usa el sitio y una cuenta de almacenamiento que conserva recursos no relacionales.

Aquí tiene una introducción concisa de cómo usar grupos de recursos en el Portal de Azure.

## Creación de grupos de recursos

Cuando se crea un recurso en el portal, siempre se crea dentro de un grupo de recursos. Puede elegir crear un nuevo grupo de recursos o usar un grupo de recursos existente en el flujo de creación.<br><br />

![crear grupo de recursos](./media/resource-group-portal/1_createWebsite.png)

Cuando se crea una aplicación que consta de algunos recursos que funcionan juntos (por ejemplo, sitio web + base de datos), siempre se crea en su propio grupo de recursos, por lo que puede administrar el ciclo de vida de todos los recursos relacionados utilizando dicho grupo de recursos. Puede agregar o quitar recursos adicionales del grupo de recursos a medida que evoluciona la aplicación.

![crear grupo de recursos](./media/resource-group-portal/2_createWSandDB.png)

## Exploración de grupos de recursos

Puede explorar todos los grupos de recursos haciendo clic en la barra de salto a la izquierda de la pantalla. Un grupo de recursos cuenta con un cuadro que proporciona toda la información sobre un grupo de recursos determinado. El cuadro del grupo de recursos también proporcionará una vista unificada de la información de supervisión y facturación para todos los recursos del grupo.

La sección de resumen muestra una asignación visual de todos los recursos del grupo en cuestión. También muestra los recursos de otros grupos que están vinculados a este grupo de recursos. La asignación de recursos también muestra el estado de cada uno de ellos.![resumen del grupo de recursos](./media/resource-group-portal/3_1BrowseRGs.png)

La parte de asignación de recursos puede personalizarse para mostrar un tamaño mayor en el que aparecerán todos los recursos contenidos en el grupo y los recursos de otros grupos que se encuentran vinculados. Esta parte puede anclarse al panel de inicio, que copiará la parte del panel de inicio.

![pin](./media/resource-group-portal/3_2BrowseRGs.png)

Si hace clic en la asignación de recursos, se inicia la vista de lista de todos los recursos en la asignación de recursos. Esta vista mostrará todos los recursos dentro de un grupo de recursos o que se encuentren vinculados a él. Si hace clic en estos recursos, se iniciarán los cuadros.

![ver recursos](./media/resource-group-portal/3_3BrowseRGs.png)

## Incorporación de recursos a grupos de recursos

Puede agregar recursos a un grupo de recursos con el comando **Add** en el cuadro de grupo de recursos. Siga los pasos del flujo para agregar otros recursos al grupo.

![agregar recurso](./media/resource-group-portal/4_AddResource.png)

Nota: no es recomendable poner el proyecto de equipo en el mismo grupo de recursos que otros recursos de Azure. Si crea un proyecto de equipo en una nueva cuenta y grupo, cree un sitio web. El grupo de sitio se establecerá de forma predeterminada en el último grupo usado (grupo VSO) y finalizará los recursos de desarrollador/tiempo de ejecución en el mismo grupo.

## Eliminación de grupos de recursos

Puesto que los grupos de recursos le permiten administrar el ciclo de vida de todos los recursos contenidos, la eliminación de un grupo de recursos provocará que se eliminen todos los recursos que alberga. También puede eliminar recursos individuales de un grupo de recursos. Debe prestar atención cuando elimine un grupo de recursos, ya que puede haber otros recursos vinculados a él. Puede ver los recursos vinculados en la asignación de recursos y seguir los pasos necesarios para evitar consecuencias no deseadas cuando elimine los grupos de recursos.

## Pasos siguientes
Introducción

- Para ver una introducción a los conceptos del Administrador de recursos, consulte [Información general del Administrador de recursos de Azure](../resource-group-overview.md).  
- Para obtener información sobre cómo usar Azure PowerShell cuando se implementan recursos, consulte [Uso de Azure PowerShell con el Administrador de recursos de Azure](../powershell-azure-resource-manager.md).
- Para obtener información sobre cómo usar la interfaz de la línea de comandos de Azure cuando se implementan recursos, consulte [Uso de la línea de comandos de Azure para Mac, Linux y Windows con el Administrador de recursos de Azure](../xplat-cli-azure-resource-manager.md). 
- Para saber cómo organizar de manera lógica los recursos, consulte [Uso de etiquetas para organizar los recursos de Azure](../resource-group-using-tags.md).
  


 

<!---HONumber=August15_HO6-->