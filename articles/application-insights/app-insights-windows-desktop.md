<properties 
	pageTitle="Application Insights para servicios y aplicaciones de escritorio de Windows" 
	description="Analice el uso y el rendimiento de la aplicación de escritorio de Windows con Application Insights." 
	services="application-insights" 
    documentationCenter="windows"
	authors="alancameronwills" 
	manager="douge"/>

<tags 
	ms.service="application-insights" 
	ms.workload="tbd" 
	ms.tgt_pltfrm="ibiza" 
	ms.devlang="na" 
	ms.topic="article" 
	ms.date="08/19/2015" 
	ms.author="awills"/>

# Application Insights en servicios, aplicaciones y roles de trabajo de escritorio de Windows

*Application Insights se encuentra en su versión de vista previa.*

[AZURE.INCLUDE [app-insights-selector-get-started](../../includes/app-insights-selector-get-started.md)]

Application Insights permite supervisar el uso y rendimiento de la aplicación implementada.

Todas las aplicaciones de Windows (incluidas las aplicaciones de escritorio, los servicios en segundo plano y los roles de trabajo) pueden usar el SDK principal de Application Insights para enviar telemetría a Application Insights. El SDK principal solo proporciona una API: a diferencia de los SDK web y de dispositivos, no incluye módulos que recopilen datos automáticamente, por lo que tiene que escribir un código para enviar su propia telemetría.


## <a name="add"></a> Creación de recursos en Application Insights


1.  En el [portal de Azure][portal], cree un nuevo recurso de Application Insights. Para el tipo de aplicación, elija Aplicación de la Tienda Windows. 

    ![Haga clic en Nuevo, Application Insights.](./media/app-insights-windows-desktop/01-new.png)

    (Su elección del tipo de aplicación establece el contenido de la hoja de información general así como de las propiedades disponibles en el [explorador de métricas][metrics]).

2.  Realice una copia de la clave de instrumentación.

    ![Haga clic en Propiedades, seleccione la clave y presione ctrl + C.](./media/app-insights-windows-desktop/02-props.png)

## <a name="sdk"></a>Instalación del SDK en la aplicación


1. En Visual Studio, edite los paquetes de NuGet de su proyecto de aplicación de escritorio.

    ![Haga clic con el botón secundario en el proyecto y seleccione Administrar paquetes de Nuget.](./media/app-insights-windows-desktop/03-nuget.png)

2. Instale el paquete de la API central de Application Insights.

    ![Busque "Application Insights"](./media/app-insights-windows-desktop/04-core-nuget.png)

    Puede instalar otros paquetes, como el contador de rendimiento o paquetes de captura de registros, si desea usar su funcionalidad.

3. Establezca InstrumentationKey en el código; por ejemplo, en main().

    `TelemetryConfiguration.Active.InstrumentationKey = "your key";`

*¿Por qué falta ApplicationInsights.config?*

* El paquete de la API central no instala el archivo .config, que solo se usa para configurar los recolectores de telemetría. Por tanto, escriba su propio código para establecer la clave de instrumentación y enviar la telemetría.
* Si ha instalado uno de los otros paquetes, tendrá un archivo .config. Puede insertar la clave de instrumentación allí en lugar codificarla.

*¿Podría usar un paquete de NuGet diferente?*

* Sí, puede usar el paquete de servidor web (Microsoft.ApplicationInsights.Web), que instala recopiladores para una variedad de módulos de recopilación, como los contadores de rendimiento. Instalará un archivo .config, donde puede colocar su clave de instrumentación. Use [ApplicationInsights.config para deshabilitar los módulos que no desea](app-insights-configuration-with-applicationinsights-config.md), por ejemplo, el recopilador de solicitud HTTP. 
* Si desea usar los [paquetes de recopiladores de registros o de seguimiento](app-insights-asp-net-trace-logs.md), comience con el paquete de servidor web. 

## <a name="telemetry"></a>Inserción de llamadas de telemetría

Cree una instancia `TelemetryClient` y después [utilícela para enviar telemetría][api].


Por ejemplo, en una aplicación de Windows Forms, podría escribir:

```C#

    public partial class Form1 : Form
    {
        private TelemetryClient tc = new TelemetryClient();
        ...
        private void Form1_Load(object sender, EventArgs e)
        {
            // Alternative to setting ikey in config file:
            tc.InstrumentationKey = "key copied from portal";

            // Set session data:
            tc.Context.User.Id = Environment.GetUserName();
            tc.Context.Session.Id = Guid.NewGuid().ToString();
            tc.Context.Device.OperatingSystem = Environment.OSVersion.ToString();

            // Log a page view:
            tc.TrackPageView("Form1");
            ...
        }

        protected override void OnClosing(CancelEventArgs e)
        {
            stop = true;
            if (tc != null)
            {
                tc.Flush(); // only for desktop apps
            }
            base.OnClosing(e);
        }

```

Utilice cualquiera de las [API de Application Insights][api] para enviar telemetría. En las aplicaciones de escritorio de Windows, no se envía automáticamente ninguna telemetría. Normalmente se usaría:

* TrackPageView(pageName) acerca de cómo cambiar formularios, páginas o pestañas
* TrackEvent(eventName) para las demás acciones de usuario
* TrackMetric(name, value) en una tarea en segundo plano para enviar informes periódicos de métricas no asociados a eventos específicos.
* TrackTrace(logEvent) para el [registro de diagnóstico][diagnostic]
* TrackException(exception) en cláusulas catch


Para asegurarse de que toda la telemetría se envía antes de cerrar la aplicación, use `TelemetryClient.Flush()`. Normalmente, la telemetría se envía por lotes en intervalos regulares. (El vaciado se recomienda solo si está usando únicamente la API principal. Los SDK web y de dispositivos implementan este comportamiento de forma automática).


#### Inicializadores de contexto

Para ver los recuentos de usuarios y sesiones, puede establecer los valores en cada instancia `TelemetryClient`. Como alternativa, puede utilizar un inicializador de contexto para realizar esta adición para todos los clientes:

```C#

    class UserSessionInitializer: IContextInitializer
    {
        public void Initialize(TelemetryContext context)
        {
            context.User.Id = Environment.UserName;
            context.Session.Id = Guid.NewGuid().ToString();
        }
    }

    static class Program
    {
        ...
        static void Main()
        {
            TelemetryConfiguration.Active.ContextInitializers.Add(
                new UserSessionInitializer());
            ...

```



## <a name="run"></a>Ejecución del proyecto

[Ejecute la aplicación con F5](http://msdn.microsoft.com/library/windows/apps/bg161304.aspx) y úsela para generar telemetría.

En Visual Studio, aparecerá un recuento de los eventos que se han enviado.

![](./media/app-insights-windows-desktop/appinsights-09eventcount.png)



## <a name="monitor"></a>Consulta de los datos del monitor

Vuelva a la hoja de la aplicación en el portal de Azure.

Los primeros eventos aparecerán en Búsqueda de diagnóstico.

Si espera más datos, haga clic en Actualizar después de unos segundos.

Si ha utilizado TrackMetric o el parámetro de mediciones de TrackEvent, abra el [explorador de métricas][metrics] y abra la hoja Filtros, donde podrá ver las métricas.



## <a name="usage"></a>Pasos siguientes

[Seguimiento del uso de la aplicación][knowUsers]

[Captura y búsqueda de registros de diagnóstico][diagnostic]

[Solución de problemas][qna]




<!--Link references-->

[diagnostic]: app-insights-diagnostic-search.md
[metrics]: app-insights-metrics-explorer.md
[portal]: http://portal.azure.com/
[qna]: app-insights-troubleshoot-faq.md
[knowUsers]: app-insights-overview-usage.md
[api]: app-insights-api-custom-events-metrics.md
[CoreNuGet]: https://www.nuget.org/packages/Microsoft.ApplicationInsights
 

<!---HONumber=August15_HO8-->