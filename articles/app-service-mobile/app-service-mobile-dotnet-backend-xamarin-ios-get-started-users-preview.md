<properties 
	pageTitle="Introducción a la autenticación de Aplicaciones móviles en Xamarin iOS" 
	description="Obtenga información acerca de cómo utilizar Aplicaciones móviles para autenticar usuarios de su aplicación Xamarin iOS a través de una variedad de proveedores de identidad, incluidos AAD, Google, Facebook, Twitter y Microsoft." 
	services="app-service\mobile" 
	documentationCenter="xamarin" 
	authors="mattchenderson" 
	manager="dwrede" 
	editor=""/>

<tags 
	ms.service="app-service" 
	ms.workload="mobile" 
	ms.tgt_pltfrm="mobile-xamarin-ios" 
	ms.devlang="dotnet" 
	ms.topic="article" 
	ms.date="08/03/2015" 
	ms.author="mahender"/>

# Adición de la autenticación a la aplicación Xamarin.iOS

[AZURE.INCLUDE [app-service-mobile-selector-get-started-users](../../includes/app-service-mobile-selector-get-started-users.md)]&nbsp;[AZURE.INCLUDE [app-service-mobile-note-mobile-services-preview](../../includes/app-service-mobile-note-mobile-services-preview.md)]

En este tema se muestra cómo autenticar usuarios de una Aplicación móvil del Servicio de aplicaciones desde la aplicación cliente. En este tutorial podrá agregar la autenticación al proyecto de inicio rápido mediante un proveedor de identidades compatible con Servicio de aplicaciones. Una vez que la aplicación móvil haya realizado la autenticación y autorización correctamente, se mostrará el valor de identificador de usuario.

Este tutorial se basa en el inicio rápido de aplicaciones móviles. Primero debe completar el tutorial [Creación de una aplicación Xamarin.iOS].

##<a name="review"></a>Revisión de la configuración del proyecto de servidor (opcional)

[AZURE.INCLUDE [app-service-mobile-dotnet-backend-enable-auth-preview](../../includes/app-service-mobile-dotnet-backend-enable-auth-preview.md)]

##<a name="create-gateway"></a>Creación de una puerta de enlace del Servicio de aplicaciones

[AZURE.INCLUDE [app-service-mobile-dotnet-backend-create-gateway-preview](../../includes/app-service-mobile-dotnet-backend-create-gateway-preview.md)]

##<a name="register"></a>Registro de la aplicación para la autenticación y configuración de Servicios de aplicaciones

[AZURE.INCLUDE [app-service-mobile-register-authentication](../../includes/app-service-mobile-register-authentication.md)]

##<a name="permissions"></a>Restricción de los permisos para los usuarios autenticados

[AZURE.INCLUDE [app-service-mobile-restrict-permissions-dotnet-backend](../../includes/app-service-mobile-restrict-permissions-dotnet-backend.md)]

&nbsp;&nbsp;4. En Visual Studio o Xamarin Studio, ejecute el proyecto de cliente en un dispositivo o emulador. Compruebe que se produce una excepción no controlada con el código de estado 401 (No autorizado) después de iniciarse la aplicación. El error se registra en la consola del depurador. De esta forma, en Visual Studio, debería ver el error en la ventana de salida.

&nbsp;&nbsp;Este error no autorizado ocurre porque la aplicación intenta obtener acceso al back-end de la aplicación móvil como usuario autenticado. La tabla *TodoItem* ahora requiere autenticación.

Luego, actualizará la aplicación cliente para solicitar recursos del back-end de la aplicación móvil con un usuario autenticado.

##<a name="add-authentication"></a>Incorporación de autenticación a la aplicación

En esta sección, modificará la aplicación para mostrar una pantalla de inicio de sesión antes de mostrar los datos. Cuando se inicie la aplicación, no se conectará a Servicios de aplicaciones y no mostrará datos. Después de que el usuario intente actualizar una vez, aparecerá la pantalla de inicio y, una vez que haya iniciado sesión, se mostrará la lista de tareas pendientes.

1. En el proyecto de cliente, abra el archivo **QSTodoService.cs** y agregue la siguiente instrucción Using y las siguientes declaraciones de miembros a QSTodoService:


		// Logged in user
		private MobileServiceUser user; 
		public MobileServiceUser User { get { return user; } }

2. Agregue una instrucción `using` para UIKit y un método nuevo con nombre **Authenticate** a **QSTodoService** con la siguiente definición:

	```
		using UIKit;
	```

        public async Task Authenticate(UIViewController view)
        {
            try
            {
                user = await client.LoginAsync(view, MobileServiceAuthenticationProvider.Facebook);
            }
            catch (Exception ex)
            {
                Console.Error.WriteLine (@"ERROR - AUTHENTICATION FAILED {0}", ex.Message);
            }
        }

	>[AZURE.NOTE]Si usa un proveedor de identidades que no sea una cuenta de Facebook, cambie el valor que pasó anteriormente a **LoginAsync** por uno de los siguientes: _MicrosoftAccount_, _Twitter_, _Google_ o _WindowsAzureActiveDirectory_.

3. Abra **QSTodoListViewController.cs**. Modifique la definición del método de **ViewDidLoad** para quitar la llamada a **RefreshAsync()** cerca del final:

		public override async void ViewDidLoad ()
		{
			base.ViewDidLoad ();

			todoService = QSTodoService.DefaultService;
           await todoService.InitializeStoreAsync ();

           RefreshControl.ValueChanged += async (sender, e) => {
                await RefreshAsync ();
           }

			// Comment out the call to RefreshAsync
			// await RefreshAsync ();
		}


4. Modifique el método **RefreshAsync** para autenticar y mostrar una pantalla de inicio si la propiedad **User** es null. Agregue el siguiente código al principio de la definición del método:

		// start of RefreshAsync method
		if (todoService.User == null) {
			await QSTodoService.DefaultService.Authenticate (this);
			if (todoService.User == null) {
				Console.WriteLine ("couldn't login!!");
				return;
			}
		}
		// rest of RefreshAsync method
	
5. En Visual Studio o Xamarin Studio conectado a su host de compilación de Xamarin en el equipo Mac, ejecute el proyecto de cliente destinado a un dispositivo o un emulador. Compruebe que la aplicación no muestra ningún dato.

	Despliegue la lista de tareas para actualizar la pantalla, lo que dará lugar a que aparezca la pantalla de inicio de sesión. Una vez que haya proporcionado credenciales válidas, la aplicación mostrará la lista de tareas pendientes y podrá actualizar los datos.

 
<!-- URLs. -->
[Submit an app page]: http://go.microsoft.com/fwlink/p/?LinkID=266582
[My Applications]: http://go.microsoft.com/fwlink/p/?LinkId=262039
[Creación de una aplicación Xamarin.iOS]: app-service-mobile-dotnet-backend-xamarin-ios-get-started-preview.md

[Azure Management Portal]: https://portal.azure.com
 

<!---HONumber=August15_HO8-->