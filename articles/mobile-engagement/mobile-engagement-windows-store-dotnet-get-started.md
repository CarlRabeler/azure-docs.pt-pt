<properties
	pageTitle="Introducción a Azure Mobile Engagement para aplicaciones Windows Universal"
	description="Aprenda a usar Azure Mobile Engagement con los análisis y las notificaciones de inserción para aplicaciones Windows Universal."
	services="mobile-engagement"
	documentationCenter="mobile"
	authors="piyushjo"
	manager="dwrede"
	editor="" />

<tags
	ms.service="mobile-engagement"
	ms.workload="mobile"
	ms.tgt_pltfrm="mobile-windows-store"
	ms.devlang="dotnet"
	ms.topic="hero-article" 
	ms.date="04/30/2015"
	ms.author="piyushjo" />

# Introducción a Azure Mobile Engagement para aplicaciones Windows Universal

> [AZURE.SELECTOR]
- [Universal Windows](mobile-engagement-windows-store-dotnet-get-started.md)
- [Windows Phone Silverlight](mobile-engagement-windows-phone-get-started.md)
- [iOS | Obj C](mobile-engagement-ios-get-started.md)
- [iOS | Swift](mobile-engagement-ios-swift-get-started.md)
- [Android](mobile-engagement-android-get-started.md)
- [Cordova](mobile-engagement-cordova-get-started.md)

En este tema se muestra cómo usar Azure Mobile Engagement para comprender el uso que hace de las aplicaciones y enviar notificaciones de inserción a los usuarios segmentados de una aplicación Windows Universal. En este tutorial se demuestra el escenario de difusión sencillo con Mobile Engagement. Creará una aplicación Windows Universal vacía que recopila datos básicos de uso de aplicaciones y recibe notificaciones de inserción mediante el Servicio de notificaciones de Windows (WNS). Cuando haya terminado, podrá difundir notificaciones de inserción a todos los dispositivos o dirigirse a usuarios específicos en función de las propiedades de sus dispositivos. Asegúrese de seguir el tutorial siguiente para ver cómo usar Mobile Engagement para dirigirse a usuarios y grupos de dispositivos específicos.

Este tutorial requiere lo siguiente:

+ Visual Studio 2013
+ [SDK de Mobile Engagement Windows Universal]

> [AZURE.IMPORTANT]Completar este tutorial es un requisito previo para todos los tutoriales de Mobile Engagement para aplicaciones Windows Universal. Para completarlo, deberá tener una cuenta de Azure activa. En caso de no tener ninguna, puede crear una cuenta de evaluación gratuita en tan solo unos minutos. Para obtener más información, consulte <a href="http://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fwww.windowsazure.com%2Fes-es%2Fdevelop%2Fmobile%2Ftutorials%2Fget-started%2F" target="_blank">Evaluación gratuita de Azure</a>.

##<a id="setup-azme"></a>Configure Mobile Engagement para su aplicación Windows Universal

1. Inicie sesión en el Portal de administración de Azure y, luego, haga clic en **+NUEVO** en la parte inferior de la pantalla.

2. Haga clic en **Servicios de aplicaciones**, en **Mobile Engagement** y, luego, en **Crear**.

   	![][7]

3. En el menú emergente que aparece, escriba la siguiente información:

   	![][8]

	- **Nombre de la aplicación**: escriba el nombre de la aplicación. Puede usar cualquier carácter.
	- **Plataforma**: seleccione la plataforma de destino (**Windows Universal**) para la aplicación (si la aplicación tiene como destino varias plataformas, repita este tutorial para cada plataforma).
	- **Nombre del recurso de la aplicación**: este es el nombre por el que la aplicación será accesible a través de las API y las direcciones URL. Solo debe usar caracteres convencionales de dirección URL. El nombre que se genera automáticamente debería proporcionar una sugerencia sólida. También debe anexar el nombre de la plataforma para evitar cualquier conflicto de nombres, ya que este debe ser único.
	- **Ubicación**: seleccione el centro de datos donde se alojará esta aplicación y, más importante aún, su colección.
	- **Colección**: si ya ha creado una aplicación, seleccione una colección creada anteriormente; en caso contrario, seleccione una colección nueva.
	- **Nombre de la colección**: esto representa el grupo de aplicaciones. También garantizará que todas las aplicaciones se encuentren en un grupo que permitirá cálculos agregados de métricas. Debe usar el nombre de la empresa o departamento aquí si es aplicable.

	> [AZURE.TIP]Si su aplicación Universal va a tener como destino Windows y la plataforma Windows Phone, a continuación, se deberán crear dos aplicaciones Mobile Engagement, una para cada plataforma compatible. Esto permite garantizar que puede crear una segmentación correcta de la audiencia y que puede enviar notificaciones de destino de forma adecuada para cada plataforma.

4. Seleccione la aplicación que acaba de crear en la pestaña **Aplicaciones**:

5. Haga clic en **Información de conexión** para mostrar la configuración de la conexión que se debe incluir en la integración de SDK en su aplicación móvil.

   	![][10]

6. Copie la **Cadena de conexión**, la necesitará para identificar esta aplicación en el código de aplicación y conectar con Mobile Engagement desde la aplicación Universal.

   	![][11]

##<a id="connecting-app"></a>Conectar la aplicación al backend de Mobile Engagement

En este tutorial se presenta una "integración básica", que es el conjunto mínimo necesario para recopilar los datos y enviar una notificación de inserción. Toda la documentación de integración se encuentra en la [documentación del SDK de Mobile Engagement Windows Universal].

Crearemos una aplicación básica con Visual Studio para demostrar la integración.

###Cree un nuevo proyecto de aplicación Windows Universal

Puede omitir este paso si ya dispone de una aplicación y está familiarizado con el desarrollo de Windows Universal.

1. Inicie Visual Studio y, en la pantalla principal, seleccione **Nuevo proyecto...**.

2. En el menú emergente, seleccione `Store Apps` -> `Universal Apps` -> `Blank App (Universal Apps)`. Rellene la aplicación `Name` y `Solution name` y, a continuación, haga clic en **Aceptar**.

   	![][13]

Ahora ha creado un nuevo proyecto de Windows Universal App en el que se integrará el SDK de Azure Mobile Engagement.

###Conectar la aplicación al back-end de Mobile Engagement

1. Instale el paquete nuget del [SDK de Mobile Engagement Windows Universal] en su proyecto. Si desea usar las plataformas Windows y Windows Phone necesitará hacer esto para los dos proyectos. El mismo paquete de Nuget colocará los binarios específicos de la plataforma correcta en cada proyecto.

2. Abra `Package.appxmanifest` y agregue lo siguiente si no se agrega automáticamente:

		Internet (Client)

	![][20]

3. Ahora, pegue la cadena de conexión que copió anteriormente para la aplicación Mobile Engagement y péguela en el archivo `Resources\EngagementConfiguration.xml`, entre las etiquetas `<connectionString>` y `</connectionString>`:

	![][22]

	>[AZURE.TIP]Si su aplicación va a tener como destino Windows y las plataformas Windows Phone, a continuación, se deberán crear dos aplicaciones Mobile Engagement, una para cada plataforma compatible. Esto permite garantizar que puede crear una segmentación correcta de la audiencia y que puede enviar notificaciones de destino de forma adecuada para cada plataforma.

4. En el archivo `App.xaml.cs`:

	a. Agregue la instrucción `using`:

			using Microsoft.Azure.Engagement;

	b. Inicialice el SDK en el método `OnLaunched`:

			protected override void OnLaunched(LaunchActivatedEventArgs e)
			{
			  EngagementAgent.Instance.Init(e);

			  //... rest of the code
			}

	c. Inserte lo siguiente en el método `OnActivated` y agregue el método si aún no está presente:

			protected override void OnActivated(IActivatedEventArgs e)
			{
			  EngagementAgent.Instance.Init(e);

			  //... rest of the code
			}

##<a id="monitor"></a>Habilitar supervisión en tiempo real

Para comenzar a enviar datos y asegurarse de que los usuarios estén activos, debe enviar al menos una pantalla (Actividad) al back-end de Mobile Engagement. Para ello, se crearán subclases de `MainPage` con `EngagementPage` que proporciona el SDK de Mobile Engagement.

1. 	Agregue la instrucción `using`:

		using Microsoft.Azure.Engagement;

2. Reemplace la superclase de `MainPage`, que es anterior a `Page`, por `EngagementPage`:

	![][23]

3. En el archivo `MainPage.xml`:

	a. Agregue a las declaraciones de espacios de nombres:

			xmlns:engagement="using:Microsoft.Azure.Engagement"

	b. Reemplace `Page` en el nombre de etiqueta xml por `engagement:EngagementPage`.

###Asegúrese de que la aplicación está conectada con la supervisión en tiempo real

En esta sección se muestra cómo asegurarse de que la aplicación se conecta al back-end de Mobile Engagement mediante la característica de supervisión en tiempo real de Mobile Engagement.

1. Desplácese hasta el portal de Mobile Engagement.

	En el portal de Azure, asegúrese de que se encuentra en la aplicación que estamos utilizando para este proyecto y, a continuación, haga clic en el botón **Interactuar** en la parte inferior:

	![][26]

2. Será dirigido a la página de configuración del portal de Engagement de la aplicación. Desde allí, haga clic en la pestaña **Monitor**, tal como se muestra a continuación. ![][30]

3. El monitor está listo para mostrar cualquier dispositivo, en tiempo real, que iniciará la aplicación.

4. En Visual Studio, inicie la aplicación en el simulador o en un dispositivo conectado.

5. Si funcionó, ahora debería ver una sesión en el monitor en tiempo real. ![][33]

**¡Enhorabuena!** Ha completado correctamente el primer paso de este tutorial con una aplicación que se conecta el back-end Mobile Engagement, que ya envía datos.

##<a id="integrate-push"></a>Habilitación de las notificaciones de inserción y mensajería en la aplicación

Mobile Engagement permite interactuar y llegar a los usuarios mediante notificaciones de inserción y mensajería en la aplicación en el contexto de las campañas. Este módulo se denomina REACH en el portal de Mobile Engagement. En las secciones siguientes se instalará la aplicación para recibirlos.

###Habilitar la aplicación para recibir notificaciones de inserción de WNS

1. En el archivo `Package.appxmanifest`, en la pestaña `Application`, en `Notifications`, seleccione `Yes` para `Toast capable:`:

	![][35]

###Inicializar el SDK de REACH

1. En `App.xaml.cs`, llame a `EngagementReach.Instance.Init();` en la función `OnLaunched` justo después de la inicialización del agente:

		protected override void OnLaunched(LaunchActivatedEventArgs e)
		{
		   EngagementAgent.Instance.Init(e);
		   EngagementReach.Instance.Init(e);
		}

2. En `App.xaml.cs`, llame a `EngagementReach.Instance.Init(e);` en la función `OnActivated` justo después de la inicialización del agente:

		protected override void OnActivated(IActivatedEventArgs e)
		{
		   EngagementAgent.Instance.Init(e);
		   EngagementReach.Instance.Init(e);
		}

Estará listo para enviar una notificación del sistema. Ahora, se comprobará que ha realizado correctamente la integración básica.

###Conceder acceso a Mobile Engagement para enviar notificaciones

1. Tendrá que asociar la aplicación con una aplicación de Tienda Windows para obtener `Package security identifier (SID)` y `Secret Key` (Secreto del cliente). Puede crear una aplicación desde el [Centro de desarrollo de la Tienda Windows] y, a continuación, asegurarse de **Asociar la aplicación con la Tienda** desde Visual Studio.

2. Desplácese a la **configuración** del portal de Mobile Engagement y haga clic en la sección `Native Push` de la izquierda.

3. Haga clic en el botón `Edit` para introducir su `Package security identifier (SID)` y su `Secret Key` tal como se muestra a continuación:

	![][36]

##<a id="send"></a>Enviar una notificación a su aplicación

Ahora crearemos una campaña de notificación de inserción simple que enviará una notificación de inserción a nuestra aplicación.

1. Vaya a la pestaña **COBERTURA** en el portal de Mobile Engagement.

2. Haga clic en **Nuevo anuncio** para crear la campaña de inserción. ![][37]

3. Configure el primer campo de la campaña mediante los pasos siguientes: ![][38]

	a. Asigne un nombre a la campaña.

	b. Seleccione **Tiempo de entrega** como *Cualquier momento* para permitir que la aplicación reciba una notificación, independientemente de si se ha iniciado o no.

	c. En el texto de la notificación, escriba el **título** que aparecerá en negrita en la inserción.

	d. Luego, escriba el mensaje.

4. Desplácese hacia abajo. En la sección del contenido, elija **Solo notificación**. ![][39]

5. Ha terminado de configurar la campaña más básica posible. Ahora, desplácese hacia abajo de nuevo y haga clic en el botón **Crear** para guardar su campaña.

6. Último paso, haga clic en **Activar** para activar su campaña y enviar notificaciones de inserción. ![][41]

Ahora debería ver una notificación del sistema de su campaña en el dispositivo (la aplicación debe cerrarse para ver esta notificación). Si se estaba ejecutando la aplicación, asegúrese de que esté cerrada durante dos minutos antes de activar la campaña para poder recibir la notificación del sistema. Si desea integrar la notificación de la aplicación para que la notificación se muestre en la aplicación cuando se abre, consulte [Aplicaciones Windows Universal - Integración de superposición].

<!-- URLs. -->
[documentación del SDK de Mobile Engagement Windows Universal]: ../mobile-engagement-windows-store-integrate-engagement/
[SDK de Mobile Engagement Windows Universal]: http://go.microsoft.com/?linkid=9864592
[Centro de desarrollo de la Tienda Windows]: http://go.microsoft.com/fwlink/p/?linkid=266582&clcid=0x409
[Aplicaciones Windows Universal - Integración de superposición]: ../mobile-engagement-windows-store-integrate-engagement-reach/#overlay-integration

<!-- Images. -->
[7]: ./media/mobile-engagement-windows-store-dotnet-get-started/create-mobile-engagement-app.png
[8]: ./media/mobile-engagement-windows-store-dotnet-get-started/create-azme-popup.png
[10]: ./media/mobile-engagement-windows-store-dotnet-get-started/app-main-page-select-connection-info.png
[11]: ./media/mobile-engagement-windows-store-dotnet-get-started/app-connection-info-page.png
[13]: ./media/mobile-engagement-windows-store-dotnet-get-started/UniversalAppCreation.png
[20]: ./media/mobile-engagement-windows-store-dotnet-get-started/manifest-capabilities.png
[21]: ./media/mobile-engagement-windows-store-get-started/manifest-declarations.png
[22]: ./media/mobile-engagement-windows-store-dotnet-get-started/add-connection-info.png
[23]: ./media/mobile-engagement-windows-store-dotnet-get-started/subclass-page.png
[26]: ./media/mobile-engagement-windows-store-dotnet-get-started/engage-button.png
[27]: ./media/mobile-engagement-common/engagement-portal.png
[30]: ./media/mobile-engagement-windows-store-dotnet-get-started/clic-monitor-tab.png
[33]: ./media/mobile-engagement-windows-store-dotnet-get-started/monitor.png
[34]: ./media/mobile-engagement-windows-store-get-started/manifest-declarations-reach.png
[35]: ./media/mobile-engagement-windows-store-dotnet-get-started/manifest-toast.png
[36]: ./media/mobile-engagement-windows-store-dotnet-get-started/enter-credentials.png
[37]: ./media/mobile-engagement-windows-store-dotnet-get-started/new-announcement.png
[38]: ./media/mobile-engagement-windows-store-dotnet-get-started/campaign-first-params.png
[39]: ./media/mobile-engagement-windows-store-dotnet-get-started/campaign-content.png
[41]: ./media/mobile-engagement-windows-store-dotnet-get-started/campaign-activate.png
 

<!---HONumber=August15_HO6-->