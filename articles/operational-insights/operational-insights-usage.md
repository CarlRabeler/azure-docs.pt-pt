<properties
   pageTitle="Administración de servidores y uso de datos"
   description="Obtenga información sobre la cantidad de datos que se envían al servicio de Operational Insights desde sus servidores"
   services="operational-insights"
   documentationCenter=""
   authors="bandersmsft"
   manager="jwhit"
   editor="tysonn" />
<tags
   ms.service="operational-insights"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/05/2015"
   ms.author="banders" />

# Administración de servidores y uso de datos

[AZURE.INCLUDE [operational-insights-note-moms](../../includes/operational-insights-note-moms.md)]

Operational Insights recopila datos y los envía al servicio de Operational Insights regularmente. Puede emplear el panel **Uso** para ver la cantidad de datos que se envían al servicio Visión operativa. El panel **Uso** muestra también el número de datos que envían diariamente las soluciones y la frecuencia con que los grupos de administración envían datos.

>[AZURE.NOTE]Si tiene una cuenta gratuita, dispone de un límite de 500 MB de datos diarios para el servicio de Operational Insights. Si alcanza este límite diario, el análisis de los datos se detendrá y se reanudará al inicio del día siguiente.

Puede ver el uso con el mosaico **Servidores y uso** en el panel **Información general** de Visión operativa.

![imagen del mosaico Servidores y uso](./media/operational-insights-usage/overview-servers-usage.png)

Si ha superado su límite de uso diario, o está cerca del límite, tiene la posibilidad de quitar una solución para reducir la cantidad de datos que envía al servicio Visión operativa. Para obtener más información sobre como quitar soluciones, consulte [Uso de la Galería de soluciones para agregar o quitar soluciones](operational-insights-setup-workspace.md).

Si un grupo de administración de Operations Manager tiene problemas para enviar datos al servicio de Operational Insights, puede solucionar el problema o quitar el grupo de Operational Insights, si es necesario.

![imagen del panel Uso](./media/operational-insights-usage/usage-dash.png)

El panel **Uso** muestra la siguiente información:

- Uso medio por día

- Uso de datos para cada solución durante el día de hoy

- La frecuencia con que los servidores de cada grupo de administración envían datos al servicio de Operational Insights

## Para trabajar con datos de uso, siga estos pasos:

1. En la página **Información general**, haga clic en el mosaico **Servidores y uso**.

2. En el panel **Uso**, vea las categorías de uso que muestran las áreas objeto de su interés.

3. Si existen datos sobre una solución que usa innecesariamente gran cantidad de la cuota asignada, podría plantearse la posibilidad de quitar esa solución.

## Para solucionar el problema o quitar grupos de administración, siga estos pasos:

1. En la página **Información general**, haga clic en el mosaico **Servidores y uso**.

2. En el panel **Uso**, vea información sobre los grupos de administración que envían datos.

3. Si hay un grupo de administración que no envía datos, puede hacer clic en **Solucionar problemas** para obtener información detallada de solución de problemas. Si ya no quiere mantener un grupo de administración y todos los agentes que se comunican con él, haga clic en **Quitar**.

[AZURE.INCLUDE [operational-insights-export](../../includes/operational-insights-export.md)]

<!---HONumber=August15_HO6-->