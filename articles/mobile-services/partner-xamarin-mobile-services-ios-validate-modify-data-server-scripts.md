<properties
	pageTitle="Uso de scripts de servidor para validar y modificar los datos (Xamarin iOS) | Centro de desarrollo móvil"
	description="Obtenga información acerca de cómo validar y modificar los datos enviados mediante scripts de servidor desde su aplicación Xamarin iOS."
	services="mobile-services"
	documentationCenter="xamarin"
	authors="ggailey777"
	manager="dwrede"
	editor=""/>

<tags
	ms.service="mobile-services"
	ms.workload="mobile"
	ms.tgt_pltfrm="mobile-xamarin-ios"
	ms.devlang="dotnet"
	ms.topic="article"
	ms.date="04/24/2015"
	ms.author="ggailey777"/>

# Validación y modificación de datos en los Servicios móviles mediante los scripts del servidor

[AZURE.INCLUDE [mobile-services-selector-validate-modify-data](../../includes/mobile-services-selector-validate-modify-data.md)]

En este tema se muestra cómo aprovechar los scripts del servidor en Servicios móviles de Azure. Dichos scripts se registran en un servicio móvil y pueden usarse para realizar una gran variedad de operaciones en los datos que se han insertado y actualizado, incluidas la modificación y validación de los datos. En este tutorial, definirá y registrará scripts de servidor que sirven para validar y modificar datos. Dado que el comportamiento de los scripts del servidor suele afectar al cliente, también actualizará la aplicación iOS para que se beneficie de estos nuevos comportamientos. El código terminado está disponible en el ejemplo de la [aplicación ValidateModifyData][GitHub].

Este tutorial le guiará a través de estos pasos básicos:

1. [Incorporación de la validación de longitud de cadena]
2. [Actualización del cliente para admitir la validación]
3. [Incorporación de una marca de tiempo al insertar]
4. [Actualización del cliente para mostrar la marca de tiempo]

Este tutorial se basa en los pasos y en la aplicación de ejemplo del tutorial anterior [Introducción a los datos]. Antes de comenzar este tutorial, primero debe completar [Introducción a los datos].

## <a name="string-length-validation"></a>Incorporación de la validación

Siempre es conveniente validar la longitud de los datos enviados por los usuarios. En primer lugar, registre un script que valide la longitud de los datos de cadena enviados al servicio móvil y que rechace las cadenas demasiado largas, en este caso con más de 10 caracteres.

1. Inicie sesión en el [Portal de administración de Azure], haga clic en **Servicios móviles** y, a continuación, en su aplicación.

	![][0]

2. Haga clic en la pestaña **Datos** y luego en la tabla **TodoItem**.

   	![][1]

3. Haga clic en **Script** y, a continuación, seleccione la operación **Insert**.

   	![][2]

4. Sustituya el script existente por la siguiente función y luego haga clic en **Guardar**.

				function insert(item, user, request) {
        	if (item.text.length > 10) {
            request.respond(statusCodes.BAD_REQUEST, 'Text length must be 10 characters or less.');
          } else {
            request.execute();
          }
        }

    Este script comprueba la longitud de la propiedad **text** y envía una respuesta de error cuando esta sobrepasa los 10 caracteres. En caso de no sobrepasarlos, se llama al método **execute** para completar la inserción.

    > [AZURE.NOTE]Puede quitar un script registrado en la pestaña **Script** haciendo clic en **Borrar** y, a continuación, en **Guardar**.

## <a name="update-client-validation"></a>Actualización del cliente

Ahora que el servicio móvil puede validar los datos y enviar respuestas de error, debe actualizar la aplicación para que pueda identificar los errores de la validación.

1. En Xamarin Studio, abra el proyecto que ha modificado al completar el tutorial [Introducción a los datos].

2. Presione el botón **Run** para generar el proyecto e iniciar la aplicación y, a continuación, escriba el texto con más de 10 caracteres en el cuadro de texto y haga clic en el icono con el signo más (**+**).

	Observe que la aplicación produce un error no controlado como resultado de la respuesta 400 (solicitud incorrecta) devuelta por el servicio móvil.

3. En el archivo TodoService.cs, busque el control actual de excepciones <code>try/catch</code> en el método **InsertTodoItemAsync** y sustituya <code>catch</code> por:

    catch (Exception ex) { var exDetail = (ex.InnerException.InnerException as MobileServiceInvalidOperationException); Console.WriteLine(exDetail.Message);

        UIAlertView alert = new UIAlertView() {
            	Title = "Error",
            	Message = exDetail.Message
        } ;
        alert.AddButton("Ok");
        alert.Show();

        return -1;
		}

	De esta forma, aparece una ventana emergente que muestra el error al usuario.

4. Busque el método **OnAdd** en **TodoListViewController.cs**. Actualice el método para asegurarse de que el <code>índice</code> devuelto no es <code>-1</code> como se devuelve en el control de excepciones en **InsertTodoItemAsync**. En este caso, no queremos agregar una nueva fila a <code>TableView</code>.

    if (index != -1) { TableView.InsertRows(new { NSIndexPath.FromItemSection(index, 0) }, UITableViewRowAnimation.Top); itemText.Text = ""; }


5. Recompile e inicie la aplicación.

	![][4]

	Observe que el error está controlado y que el mensaje de error se muestra al usuario.


## <a name="next-steps"> </a>Pasos siguientes

Ahora que ha completado este tutorial, considere la posibilidad de continuar con el tutorial final de la serie de datos [Limitación de consultas con paginación].

Los scripts de servidor también se usan al autorizar usuarios y para enviar notificaciones de inserción. Para obtener más información, consulte los siguientes tutoriales:

* [Autorización de usuarios con scripts] <br/>Aprenda a filtrar datos basándose en el identificador de un usuario autenticado.

* [Introducción a las notificaciones de inserción] <br/>Aprenda a enviar una notificación de inserción muy básica a la aplicación.

* [Referencia del script del servidor de Servicios móviles] <br/>Obtenga más información acerca del registro y uso de scripts de servidor.

<!-- Anchors. -->
[Incorporación de la validación de longitud de cadena]: #string-length-validation
[Actualización del cliente para admitir la validación]: #update-client-validation
[Incorporación de una marca de tiempo al insertar]: #add-timestamp
[Actualización del cliente para mostrar la marca de tiempo]: #update-client-timestamp
[Next Steps]: #next-steps

<!-- Images. -->
[0]: ./media/partner-xamarin-mobile-services-ios-validate-modify-data-server-scripts/mobile-services-selection.png
[1]: ./media/partner-xamarin-mobile-services-ios-validate-modify-data-server-scripts/mobile-portal-data-tables.png
[2]: ./media/partner-xamarin-mobile-services-ios-validate-modify-data-server-scripts/mobile-insert-script-users.png

[4]: ./media/partner-xamarin-mobile-services-ios-validate-modify-data-server-scripts/mobile-quickstart-data-error-ios.png

<!-- URLs. -->
[Referencia del script del servidor de Servicios móviles]: http://go.microsoft.com/fwlink/?LinkId=262293
[Get started with Mobile Services]: /develop/mobile/tutorials/get-started-xamarin-ios
[Autorización de usuarios con scripts]: /develop/mobile/tutorials/authorize-users-in-scripts-xamarin-ios
[Limitación de consultas con paginación]: /develop/mobile/tutorials/add-paging-to-data-xamarin-ios
[Introducción a los datos]: /develop/mobile/tutorials/get-started-with-data-xamarin-ios
[Get started with authentication]: /develop/mobile/tutorials/get-started-with-users-xamarin-ios
[Introducción a las notificaciones de inserción]: /develop/mobile/tutorials/get-started-with-push-xamarin-ios

[Management Portal]: https://manage.windowsazure.com/
[Portal de administración de Azure]: https://manage.windowsazure.com/
[GitHub]: http://go.microsoft.com/fwlink/p/?LinkId=331330
 

<!---HONumber=August15_HO6-->