<properties pageTitle="Tutorial: integración de Azure Active Directory con Egnyte | Microsoft Azure" description="Aprenda a usar Egnyte con Azure Active Directory para habilitar el inicio de sesión único, el aprovisionamiento automático, etc." services="active-directory" authors="MarkusVi"  documentationCenter="na" manager="stevenpo"/>
<tags ms.service="active-directory" ms.devlang="na" ms.topic="article" ms.tgt_pltfrm="na" ms.workload="identity" ms.date="08/01/2015" ms.author="markvi" />
#Tutorial: integración de Azure Active Directory con Egnyte
>[AZURE.TIP]Para enviar comentarios, haga clic [aquí](http://go.microsoft.com/fwlink/?LinkId=528188).
  
El objetivo de este tutorial es mostrar la integración de Azure y Egnyte.
En la situación descrita en este tutorial se supone que ya cuenta con los elementos siguientes:

-   Una suscripción de Azure válida
-   Una suscripción habilitada para el inicio de sesión único en Egnyte
  
Después de completar este tutorial, los usuarios de Azure AD que haya asignado a Egnyte podrán realizar un inicio de sesión único en la aplicación en el sitio de la compañía de Egnyte (inicio de sesión iniciado por el proveedor de servicios) o desde [Introducción al Panel de acceso](https://msdn.microsoft.com/library/dn308586)
  
La situación descrita en este tutorial consta de los siguientes bloques de creación:

1.  Habilitación de la integración de aplicaciones para Egnyte
2.  Configuración del inicio de sesión único
3.  Configuración del aprovisionamiento de usuario
4.  Asignación de usuarios

![Escenario](./media/active-directory-saas-egnyte-tutorial/IC787812.png "Escenario")
##Habilitación de la integración de aplicaciones para Egnyte
  
El objetivo de esta sección es describir cómo se habilita la integración de aplicaciones para Egnyte.

###Siga estos pasos para habilitar la integración de aplicaciones para Egnyte:

1.  En el panel de navegación izquierdo del Portal de administración de Azure, haga clic en **Active Directory**.

    ![Active Directory](./media/active-directory-saas-egnyte-tutorial/IC700993.png "Active Directory")

2.  En la lista **Directory**, seleccione el directorio cuya integración desee habilitar.

3.  Para abrir la vista de aplicaciones, haga clic en **Applications**, en el menú superior de la vista de directorios.

    ![Aplicaciones](./media/active-directory-saas-egnyte-tutorial/IC700994.png "Aplicaciones")

4.  Haga clic en **Agregar** en la parte inferior de la página.

    ![Agregar aplicación](./media/active-directory-saas-egnyte-tutorial/IC749321.png "Agregar aplicación")

5.  En el cuadro de diálogo **¿Qué desea hacer?**, haga clic en **Agregar una aplicación de la galería**.

    ![Agregar una aplicación de la galería](./media/active-directory-saas-egnyte-tutorial/IC749322.png "Agregar una aplicación de la galería")

6.  En el **cuadro de búsqueda**, escriba **egnyte**.

    ![Galería de aplicaciones](./media/active-directory-saas-egnyte-tutorial/IC787813.png "Galería de aplicaciones")

7.  En el panel de resultados, seleccione **Egnyte** y, a continuación, haga clic en **Completar** para agregar la aplicación.

    ![Egnyte](./media/active-directory-saas-egnyte-tutorial/IC787814.png "Egnyte")
##Configuración del inicio de sesión único
  
El objetivo de esta sección es describir cómo permitir a los usuarios autenticarse en Egnyte con su cuenta de Azure AD mediante federación basada en el protocolo SAML.
Como parte de este procedimiento, se requiere crear un archivo de certificado codificado en base 64.
Si no está familiarizado con este procedimiento, consulte [Conversión de un certificado binario en un archivo de texto](http://youtu.be/PlgrzUZ-Y1o).

###Siga estos pasos para configurar el inicio de sesión único:

1.  En el Portal de Azure AD, en la página de integración de aplicaciones de **Egnyte**, haga clic en **Configurar inicio de sesión único** para abrir el cuadro de diálogo **Configurar inicio de sesión único**.

    ![Configurar inicio de sesión único](./media/active-directory-saas-egnyte-tutorial/IC787815.png "Configurar inicio de sesión único")

2.  En la página **¿Cómo desea que los usuarios inicien sesión en Egnyte?**, seleccione **Inicio de sesión único de Microsoft Azure AD** y, a continuación, haga clic en **Siguiente**.

    ![Configurar inicio de sesión único](./media/active-directory-saas-egnyte-tutorial/IC787816.png "Configurar inicio de sesión único")

3.  En la página **Configurar dirección URL de la aplicación**, en el cuadro de texto **URL de inicio de sesión de Egnyte**, escriba su dirección URL con el siguiente patrón "**https://company.egnyte.com*" y, a continuación, haga clic en **Siguiente**.

    ![Configurar dirección URL de la aplicación](./media/active-directory-saas-egnyte-tutorial/IC787817.png "Configurar dirección URL de la aplicación")

4.  En la página **Configuración de inicio de sesión único en Egnyte**, haga clic en **Descargar certificado** y, a continuación, guarde el archivo de certificado en el equipo.

    ![Configurar inicio de sesión único](./media/active-directory-saas-egnyte-tutorial/IC787818.png "Configurar inicio de sesión único")

5.  En otra ventana del explorador web, inicie sesión en el sitio de la compañía de Egnyte como administrador.

6.  Haga clic en el icono de **configuración**.

    ![Settings](./media/active-directory-saas-egnyte-tutorial/IC787819.png "Settings")

7.  En el menú, haga clic en **Settings** (Configuración).

    ![Settings](./media/active-directory-saas-egnyte-tutorial/IC787820.png "Settings")

8.  Haga clic en la pestaña **Configuration** (Configuración) y, a continuación, haga clic en **Security** (Seguridad).

    ![Seguridad](./media/active-directory-saas-egnyte-tutorial/IC787821.png "Seguridad")

9.  En la sección **Single Sign-On Authentication** (Autenticación de inicio de sesión único), siga estos pasos:

    ![Autenticación de inicio de sesión único](./media/active-directory-saas-egnyte-tutorial/IC787822.png "Autenticación de inicio de sesión único")

    1.  En **Single sign-on authentication** (Autenticación de inicio de sesión único), seleccione**SAML 2.0**.
    2.  En **Identity provider** (Proveedor de identidades), seleccione**AzureAD**.
    3.  En el Portal de Azure, en la página de diálogo **Configurar inicio de sesión único en Egnyte**, copie el valor de **Dirección URL de inicio de sesión remoto** y péguelo en el cuadro de texto **Identity provider login URL (Dirección URL de inicio de sesión de proveedor de Id.) **.
4.  En el Portal de Azure, en la página de diálogo **Configurar inicio de sesión único en Egnyte**, copie el valor de **Id. de entidad ** y péguelo en el cuadro de texto **Identity provider entity ID** (Id. de identidad de proveedor de identidades).
    5.  Cree un archivo **codificado en base 64** a partir del certificado descargado.  

        >[AZURE.TIP]Para obtener más información, consulte [Conversión de un certificado binario en un archivo de texto](http://youtu.be/PlgrzUZ-Y1o)

    6.  Abra el certificado codificado en base 64 en el Bloc de notas, copie su contenido en el Portapapeles y, a continuación, péguelo en el cuadro de texto **Identity provider certificate** (Certificado de proveedor de identidades).
    7.  En **Asignación de usuario predeterminada**, seleccione **Dirección de correo electrónico**.
    8.  En **Usar valor de emisor específico del dominio**, seleccione**Deshabilitado**.
    9.  Haga clic en **Guardar**.

10. En el Portal de Azure AD, seleccione la confirmación de configuración de inicio de sesión único y, a continuación, haga clic en **Completar** para cerrar el cuadro de diálogo **Configurar inicio de sesión único**.

    ![Configurar inicio de sesión único](./media/active-directory-saas-egnyte-tutorial/IC787823.png "Configurar inicio de sesión único")
##Configuración del aprovisionamiento de usuario
  
Para permitir que los usuarios de Azure AD inicien sesión en Egnyte, deben aprovisionarse en Egnyte. En el caso de Egnyte, el aprovisionamiento es una tarea manual.

###Para aprovisionar cuentas de usuario, realice estos pasos:

1.  Inicie sesión en el sitio de compañía de **Egnyte** como administrador.

2.  Vaya a **Settings > Users & Groups (Configuración > Usuarios y grupos)**.

3.  Haga clic en **Add New User** (Agregar nuevo usuario) y, a continuación, seleccione el tipo de usuario que desea agregar.

    ![Usuarios](./media/active-directory-saas-egnyte-tutorial/IC787824.png "Usuarios")

4.  En la sección **New Standard User** (Nuevo usuario estándar), lleve a cabo estos pasos:

    ![Nuevo usuario estándar](./media/active-directory-saas-egnyte-tutorial/IC787825.png "Nuevo usuario estándar")

    1.  En los campos **Email** (Correo electrónico) y **Username** (Nombre de usuario), escriba la información solicitada, y agregue los restantes datos de la cuenta válida de Azure Active Directory que desee aprovisionar.
    2.  Haga clic en **Guardar**.

    >[AZURE.NOTE]El titular de la cuenta de Azure Active Directory recibirá una notificación por correo electrónico.

>[AZURE.NOTE]Puede usar cualquier otra API o herramienta de creación de cuentas de usuario de Egnyte que proporcione Egnyte para aprovisionar cuentas de usuario de AAD.

##Asignación de usuarios
  
Para probar la configuración, tiene que conceder acceso, mediante su asignación, a los usuarios de Azure AD a los que quiere permitir el uso de su aplicación.

###Para asignar usuarios a Egnyte, lleve a cabo los siguientes pasos:

1.  En el Portal de Azure AD, cree una cuenta de prueba.

2.  En la página de integración de aplicaciones de **Egnyte **, haga clic en **Asignar usuarios**.

    ![Asignar usuarios](./media/active-directory-saas-egnyte-tutorial/IC787826.png "Asignar usuarios")

3.  Seleccione su usuario de prueba, haga clic en **Asignar** y, a continuación, en **Sí** para confirmar la asignación.

    ![Sí](./media/active-directory-saas-egnyte-tutorial/IC767830.png "Sí")
  
Si desea probar la configuración de inicio de sesión único, abra el Panel de acceso. Para obtener más información sobre el Panel de acceso, consulte [Introducción al Panel de acceso](https://msdn.microsoft.com/library/dn308586).

<!----HONumber=August15_HO7-->
<!--Please, pass changes in lines 54-57-->