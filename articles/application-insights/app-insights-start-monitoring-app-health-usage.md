<properties 
	pageTitle="Incorporación del SDK de Application Insights para supervisar la aplicación de ASP.NET" 
	description="Analice el uso, la disponibilidad y el rendimiento de su aplicación web de Microsoft Azure o local con Application Insights." 
	services="application-insights" 
    documentationCenter=".net"
	authors="alancameronwills" 
	manager="douge"/>

<tags 
	ms.service="application-insights" 
	ms.workload="tbd" 
	ms.tgt_pltfrm="ibiza" 
	ms.devlang="na" 
	ms.topic="get-started-article" 
	ms.date="08/05/2015" 
	ms.author="awills"/>


# Incorporación del SDK de Application Insights para supervisar la aplicación de ASP.NET

*Application Insights se encuentra en su versión de vista previa.*

[AZURE.INCLUDE [app-insights-selector-get-started](../../includes/app-insights-selector-get-started.md)]


Application Insights de Visual Studio supervisa su aplicación activa para ayudarle a [detectar y diagnosticar problemas y excepciones de rendimiento][detect], y [descubrir cómo se usa la aplicación][knowUsers]. Se puede usar con una amplia variedad de tipos de aplicación. Funciona con las aplicaciones hospedadas en máquinas virtuales de Azure o en sus servidores IIS locales propios, así como con las aplicaciones web de Azure.



![Gráficos de supervisión de rendimiento de ejemplo](./media/app-insights-start-monitoring-app-health-usage/10-perf.png)

*Consulte también:*

* [ASP.NET 5](app-insights-asp-net-five.md)
* [Aplicaciones para dispositivos y servidores Java][platforms]

#### Antes de comenzar

En muchos tipos de aplicaciones, [Visual Studio puede agregar Application Insights a la aplicación en cuestión](#ide) casi sin que usted se dé cuenta. Sin embargo, puesto que está leyendo esto para entender mejor el proceso, le guiaremos a través de los pasos manualmente.


Necesita:

* Una suscripción a [Microsoft Azure](http://azure.com). Si su equipo u organización tiene una suscripción a Azure, el propietario puede agregarle a esta con su [cuenta Microsoft](http://live.com).
* Visual Studio 2013 o posterior.

## <a name="add"></a> 1. Creación de recursos en Application Insights

Inicie sesión en el [Portal de Azure][portal] y cree un nuevo recurso de Application Insights. Elija ASP.NET como tipo de aplicación.

![Haga clic en Nuevo, Application Insights.](./media/app-insights-start-monitoring-app-health-usage/01-new-asp.png)

Un [recurso][roles] de Azure es una instancia de un servicio. Este recurso es donde se le presentará telemetría de su aplicación analizada.

La elección del tipo de aplicación establece el contenido predeterminado de las hojas de recursos y las propiedades que estarán visibles en el [Explorador de métricas][metrics].

####  Realice una copia de la clave de instrumentación.

La clave identifica al recurso y se instalará pronto en el SDK para dirigir los datos al recurso.

![Haga clic en Propiedades, seleccione la clave y presione ctrl + C.](./media/app-insights-start-monitoring-app-health-usage/02-props-asp.png)

Los pasos que acaba de realizar para crear un recurso nuevo son una excelente manera de comenzar a supervisar cualquier aplicación. Ahora puede enviarle datos.

## <a name="sdk"></a> 2. Instale el SDK en su aplicación.

Instalar y configurar el SDK de Application Insights varía en función de la plataforma en la que trabaja. En el caso de las aplicaciones de ASP.NET, es fácil.

1. En Visual Studio, edite los paquetes de NuGet de su proyecto de aplicación de escritorio.

    ![Haga clic con el botón secundario en el proyecto y seleccione Administrar paquetes de Nuget.](./media/app-insights-start-monitoring-app-health-usage/03-nuget.png)

2. Instale el SDK de Application Insights para aplicaciones web.

    ![Busque "Application Insights"](./media/app-insights-start-monitoring-app-health-usage/04-ai-nuget.png)


3. Edite ApplicationInsights.config (que la instalación de NuGet ha agregado). Inserte esto justo antes de la etiqueta de cierre:

    `<InstrumentationKey>` *la clave de instrumentación que copió* `</InstrumentationKey>`

    (También puede [establecer la clave escribiendo código][apikey] en su aplicación.)

#### Para actualizar a futuras versiones del SDK

De vez en cuando, lanzamos una versión nueva del SDK.

Para actualizar a una [nueva versión del SDK](app-insights-release-notes-dotnet.md), vuelva a abrir el Administrador de paquetes de NuGet y filtre los paquetes instalados. Seleccione Microsoft.ApplicationInsights.Web y elija Actualizar.

Si ha realizado personalizaciones en ApplicationInsights.config, guarde una copia del mismo antes de actualizar y después combine los cambios en la nueva versión.


## <a name="run"></a> 3. Ejecución del proyecto

Ejecute la aplicación con F5 y pruébela. Abra varias páginas para generar telemetría.

En Visual Studio, aparecerá un recuento de los eventos que se han enviado.

![](./media/app-insights-start-monitoring-app-health-usage/appinsights-09eventcount.png)

## <a name="monitor"></a> 4. Visualización de la telemetría

Vuelva al [portal de Azure][portal] y busque el recurso de Application Insights.


Busque los datos en los gráficos de Información general. Al principio, solo aparecerán uno o dos puntos. Por ejemplo:

![Haga clic en las distintas opciones para obtener más datos](./media/app-insights-start-monitoring-app-health-usage/12-first-perf.png)

Haga clic en cualquier gráfico para ver métricas más detalladas. [Más información acerca de las métricas.][perf]

Implemente ahora la aplicación y observe cómo se acumulan los datos.


Si se trabaja en modo de depuración, la telemetría se agiliza a través de la canalización y los datos aparecen en cuestión de segundos. Cuando se implementa una aplicación, los datos se acumulan a menor velocidad.

#### ¿No hay datos?

* Abra el icono [Buscar][diagnostic] para ver los eventos individuales.
* Use la aplicación y abra varias páginas para generar telemetría.
* Espere unos segundos y haga clic en Actualizar.
* Vea [Solución de problemas][qna].

#### ¿Tiene problemas el servidor de compilación?

Consulte [este apartado de la solución de problemas](app-insights-troubleshoot-faq.md#NuGetBuild).

## Adición del seguimiento de dependencia

El SDK necesita un poco de ayuda para obtener acceso a algunos datos. En concreto, este paso adicional será necesario para medir automáticamente las llamadas de la aplicación a las bases de datos, las API de REST y otros componentes externos. Estas métricas de dependencia pueden ser inestimables para ayudar a diagnosticar problemas de rendimiento.

#### Si la aplicación se ejecuta en su servidor IIS

Inicie sesión en el servidor con derechos de administrador e instale [Monitor de estado de Application Insights](http://go.microsoft.com/fwlink/?LinkId=506648).

(También puede usar el Monitor de estado para [instrumentar una aplicación que ya está en ejecución](app-insights-monitor-performance-live-website-now.md), incluso si no se ha compilado con el SDK).

#### Si la aplicación es una aplicación web de Azure

En el panel de control de la aplicación web de Azure, agregue la extensión Application Insights.

![En la aplicación web, Herramientas Supervisión de rendimiento, Agregar, Application Insights](./media/app-insights-start-monitoring-app-health-usage/05-extend.png)

(La extensión solo ayuda a una aplicación que se ha compilado con el SDK. A diferencia del Monitor de estado, no puede instrumentar una aplicación existente).

## Adición de la supervisión del lado cliente

Instaló el SDK que envía datos de telemetría desde el servidor (back-end) de la aplicación. Ahora puede agregar la supervisión del lado cliente. Esto proporciona datos de los usuarios, sesiones, vistas de página y excepciones o bloqueos que se producen en el cliente.

También podrá escribir su propio código para realizar un seguimiento de cómo los usuarios trabajan con la aplicación, incluidos el nivel detallado de clics y las pulsaciones de teclas.

#### Si los clientes son exploradores web

Si la aplicación muestra páginas web, agregar un fragmento de código de JavaScript en cada página. Obtenga el código del recurso de Application Insights:

![En la aplicación web, abra Inicio rápido y haga clic en 'Obtener código para supervisar mis páginas web'](./media/app-insights-start-monitoring-app-health-usage/02-monitor-web-page.png)

Observe que el código contiene la clave de instrumentación que identifica al recurso de la aplicación.

[Obtenga más información sobre el seguimiento de páginas web.](app-insights-web-track-usage.md)

#### Si los clientes son aplicaciones de dispositivos

Si la aplicación atiende a clientes como teléfonos u otros dispositivos, agregue el [SDK adecuado](app-insights-platforms.md) a la aplicación del dispositivo.

Si configura el SDK del cliente con la misma clave de instrumentación que el SDK del servidor, las dos secuencias se integrarán de modo que podrá verlas juntas.


## Finalización de la instalación.

Para obtener la vista completa de 360 grados de la aplicación, debe realizar algunas tareas adicionales:

* [Configure las pruebas web][availability] para comprobar que la aplicación efectivamente está activa y responde adecuadamente.
* [Captura del seguimientos de registros][netlogs] de su marco de registro de favoritos
* [Realice el seguimiento de métricas y eventos personalizado][api] en el cliente, en el servidor o en ambos para obtener más información acerca de cómo se usa la aplicación.

## <a name="ide"></a> La forma automatizada

Al comienzo del presente artículo, indicamos que le mostraríamos la forma manual de crear un recurso de Application Insights y luego instalar el SDK. Creemos que es recomendable comprender las dos partes de ese procedimiento. Sin embargo, para las aplicaciones de ASP.NET (y muchas otras), existen una manera automatizada incluso más rápida.

Necesitará [Visual Studio](http://go.microsoft.com/fwlink/?linkid=397827&clcid=0x409) (2013 Update 3 o posterior) y una cuenta en [Microsoft Azure](http://azure.com).

#### Si se trata de un proyecto nuevo:

Cuando cree un proyecto nuevo en Visual Studio, asegúrese de seleccionar Application Insights.


![Crear un proyecto ASP.NET](./media/app-insights-start-monitoring-app-health-usage/appinsights-01-vsnewp1.png)

Visual Studio crea un recurso en Application Insights, agrega el SDK al proyecto y coloca la clave en el `.config` archivo.

Si el proyecto tiene páginas web, también agrega el [SDK de JavaScript][client] a la página web principal.

#### ....o si se trata de un proyecto existente

Haga clic con el botón secundario en el Explorador de soluciones y seleccione Agregar Application Insights.

![Elija Agregar Application Insights](./media/app-insights-start-monitoring-app-health-usage/appinsights-03-addExisting.png)

Visual Studio crea un recurso en Application Insights, agrega el SDK al proyecto y coloca la clave en el `.config` archivo.

En este caso, no se agrega el [SDK de JavaScript][client] a las páginas web: le recomendamos que lo haga en el paso siguiente.

#### Opciones de configuración

La primera vez deberá iniciar sesión o registrarse en Microsoft Azure en vista previa. (Al margen de la cuenta de Visual Studio Online).

Si esta aplicación forma parte de una aplicación mayor, es posible que quiera usar **Configurar valor** para colocarla en el mismo grupo de recursos que los demás componentes.

*¿No encuentra la opción Application Insights? Compruebe que está usando Visual Studio 2013 Update 3 o posterior, que las herramientas de Application Insights se encuentran habilitadas en Extensiones y actualizaciones.*

#### Abra Application Insights desde un proyecto.

![Haga clic con el botón secundario en el proyecto y abra el Portal de Azure](./media/app-insights-start-monitoring-app-health-usage/appinsights-04-openPortal.png)




## <a name="video"></a>Vídeo

> [AZURE.VIDEO getting-started-with-application-insights]


<!--Link references-->

[api]: app-insights-api-custom-events-metrics.md
[apikey]: app-insights-api-custom-events-metrics.md#ikey
[availability]: app-insights-monitor-web-app-availability.md
[azure]: ../insights-perf-analytics.md
[client]: app-insights-javascript.md
[detect]: app-insights-detect-triage-diagnose.md
[diagnostic]: app-insights-diagnostic-search.md
[knowUsers]: app-insights-overview-usage.md
[metrics]: app-insights-metrics-explorer.md
[netlogs]: app-insights-asp-net-trace-logs.md
[perf]: app-insights-web-monitor-performance.md
[platforms]: app-insights-platforms.md
[portal]: http://portal.azure.com/
[qna]: app-insights-troubleshoot-faq.md
[redfield]: app-insights-monitor-performance-live-website-now.md
[roles]: app-insights-resources-roles-access-control.md
[start]: app-insights-get-started.md

 

<!---HONumber=August15_HO6-->