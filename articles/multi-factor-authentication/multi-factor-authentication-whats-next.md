<properties 
	pageTitle="Pasos siguientes para Azure Multi-Factor Authentication" 
	description="Esta es la página de Azure Multi-Factor Authentication que describe los pasos siguientes a realizar con MFA. Esto incluye informes, alertas de fraude, omisión por única vez, mensajes de voz personalizados, almacenamiento en caché, direcciones IP de confianza y contraseñas de aplicación." 
	services="multi-factor-authentication" 
	documentationCenter="" 
	authors="billmath" 
	manager="swadhwa" 
	editor="curtand"/>

<tags 
	ms.service="multi-factor-authentication" 
	ms.workload="identity" 
	ms.tgt_pltfrm="na" 
	ms.devlang="na" 
	ms.topic="article" 
	ms.date="07/02/2015" 
	ms.author="billmath"/>

# Configuración de Azure Multi-Factor Authentication

El artículo siguiente le ayudará a administrar Azure Multi-Factor Authentication ahora que ya está preparado y con todo funcionando. Abarca una variedad de temas que le permitirá aprovechar al máximo Azure Multi-Factor Authentication. Tenga en cuenta que no todas estas características están disponibles en todas las versiones de Azure Multi-Factor Authentication.

Característica| Descripción| ¿Qué se va a tratar?
:------------- | :------------- | :------------- | 
[Alerta de fraude](#fraud-alert)|Se puede instalar y configurar la alerta de fraude para que los usuarios puedan informar sobre intentos fraudulentos de obtener acceso a sus recursos.|Cómo realizar la instalación y configuración y cómo informar sobre fraudes
[Omisión por única vez](#one-time-bypass) |Una omisión por única vez permite a un usuario autenticarse una sola vez omitiendo la autenticación multifactor.|Cómo instalar y configurar una omisión por única vez
[Mensajes de voz personalizados](#custom-voice-messages) |Los mensajes de voz personalizados permiten utilizar sus propias grabaciones o saludos con la autenticación multifactor. |Cómo instalar y configurar los mensajes y saludos personalizados
[Almacenamiento en caché](#caching)|El almacenamiento en caché permite establecer un período de tiempo específico para que los intentos de autenticación siguientes se realicen correctamente de forma automática. |Cómo instalar y configurar el almacenamiento en caché de la autenticación.
[Direcciones IP de confianza](#trusted-ips)|IP de confianza es una característica de la autenticación multifactor que permite a los administradores de un inquilino administrado o federado la capacidad de omitir la autenticación multifactor para los usuarios que inician sesión desde la intranet local de la empresa.|Configurar e instalar las direcciones IP que están exentas para la autenticación multifactor	
[Contraseñas de aplicación](#app-passwords)|Las contraseñas de aplicación permiten omitir la autenticación multifactor a una aplicación que no sea compatible con la misma y poder seguir trabajando.|Información acerca de las contraseñas de aplicación.
[Suspensión de Multi-Factor Authentication para exploradores y dispositivos recordados (vista previa pública)](#suspend-multi-factor-authentication-for-remembered-devices-and-browsers-public-preview)|Permite suspender la autenticación multifactor durante un número determinado de días después de que un usuario inicie sesión correctamente utilizando la autenticación multifactor.|Información sobre cómo habilitar esta característica y configurar el número de días.




## Alerta de fraude
Se puede instalar y configurar la alerta de fraude para que los usuarios puedan informar sobre intentos fraudulentos de obtener acceso a sus recursos. Los usuarios pueden informar del fraude con la aplicación móvil o a través de su teléfono.

### Instalar y configurar la alerta de fraude


1. Inicie sesión en [http://azure.microsoft.com](http://azure.microsoft.com)
2. En la parte izquierda, seleccione Active Directory.
3. En la parte superior, seleccione los proveedores de Multi-Factor Authentication. Aparecerá una lista de los proveedores de Multi-Factor Authentication.
4. Si tiene más de un proveedor de Multi-Factor Authentication, seleccione aquel en el que desea habilitar la alerta de fraude y haga clic en Administrar en la parte inferior de la página. Si sólo tiene uno, haga clic en Administrar. Se abrirá el Portal de administración de Azure Multi-Factor Authentication.
5. En el Portal de administración de Azure Multi-Factor Authentication, a la izquierda, haga clic en Configuración.
6. En la sección de Alerta de fraude, coloque una comprobación en Permitir a los usuarios enviar alertas de fraude.
7. Si desea que los usuarios se bloquean cuando se informe de un fraude, coloque una comprobación en la casilla correspondiente a Bloquear usuario al notificarse fraudes.
8. En el cuadro situado bajo Código para notificar fraudes durante el saludo inicial, escriba un código de números que se pueda usar durante la comprobación de llamada. Si un usuario introduce este código, en lugar del signo #, se notificará una alerta de fraude.
9. Haga clic en Guardar en la parte inferior.

<center>![Cloud](./media/multi-factor-authentication-whats-next/fraud.png)</center>

### Para informar sobre un fraude
Una alerta de fraude se puede notificar de dos maneras. Ya sea a través de la aplicación móvil o del teléfono.

### Para notificar de una alerta de fraude con la aplicación móvil
<ol>
<li>Cuando se envía una comprobación a su teléfono, haga clic en ella y se iniciará la aplicación Multi-Factor Authentication.</li>
<li>Para informar sobre un fraude, haga clic en Cancelar y notificar fraude. Aparecerá un cuadro que dice que se informará al</li> personal de asistencia de TI de su organización. Haga clic en Notificar fraude.
<li>En la aplicación, haga clic en Cerrar.</li></ol>

<center>![Cloud](./media/multi-factor-authentication-whats-next/report1.png)</center>

### Para notificar de una alerta de fraude con el teléfono
<ol>
<li>Cuando una llamada de comprobación llega a su teléfono, respóndala.</li>
<li>Para informar de fraude, escriba el código que se ha configurado para informar de fraude a través del teléfono y, a continuación, el signo #. Se le notificará que se ha enviado una alerta de fraude.</li>
<li>Finalice la llamada.</li></ol>

### Para ver el informe de fraude

1. Inicie sesión en [http://azure.microsoft.com](http://azure.microsoft.com)
2. En la parte izquierda, seleccione Active Directory.
3. En la parte superior, seleccione los proveedores de Multi-Factor Authentication. Aparecerá una lista de los proveedores de Multi-Factor Authentication.
4. Si tiene más de un proveedor de Multi-Factor Authentication, seleccione aquel en el que desea ver el informe de la alerta de fraude y haga clic en Administrar en la parte inferior de la página. Si sólo tiene uno, haga clic en Administrar. Se abrirá el Portal de administración de Azure Multi-Factor Authentication.
5. En el Portal de administración de Azure Multi-Factor Authentication, a la izquierda, en Ver un informe haga clic en Alerta de fraude.
6. Especifique el intervalo de fechas que desea ver en el informe. También puede especificar cualquier nombre de usuario específico, números de teléfono y el estado de los usuarios.
7. Haga clic en Ejecutar. Aparecerá un informe similar al siguiente. También puede hacer clic en Exportar a CSV si desea exportar el informe.

## Omisión por única vez

Una omisión por única vez permite a un usuario autenticarse una sola vez omitiendo la autenticación multifactor. La omisión es temporal y expira una vez que ha pasado el número especificado de segundos. Por ello en situaciones donde la aplicación móvil o el teléfono no están recibiendo una notificación o una llamada de teléfono, puede habilitar una omisión única, para que el usuario puede tener acceso al recurso deseado.

### Para crear una omisión por única vez

<ol>
<li>Inicie sesión en [http://azure.microsoft.com](http://azure.microsoft.com)</li>
<li>En la parte izquierda, seleccione Active Directory.</li>
<li>En la parte superior, seleccione los proveedores de Multi-Factor Authentication. Aparecerá una lista de los proveedores de Multi-Factor Authentication.</li>
<li>Si tiene más de un proveedor de Multi-Factor Authentication, seleccione aquel que esté asociado con el directorio para el usuario para el que desea crear una omisión por única vez y haga clic en Administrar en la parte inferior de la página. Si sólo tiene uno, haga clic en Administrar. Se abrirá el Portal de administración de Azure Multi-Factor Authentication.</li>
<li>En el Portal de administración de Azure Multi-Factor Authentication, a la izquierda, en Administración de usuarios haga clic en Configuración.</li>

<center>![Cloud](./media/multi-factor-authentication-whats-next/create1.png)</center>

<li>En la página omisión por única vez, haga clic en Nueva omisión por única vez.</li>
<li>Escriba el nombre de usuario, el número de segundos que durará la omisión, el motivo para la misma y haga clic en Omitir.</li>

<center>![Cloud](./media/multi-factor-authentication-whats-next/create2.png)</center>

<li>Ahora, el usuario tiene que iniciar sesión antes de que expire la omisión por única vez.</li>

### Para ver el informe de omisión por única vez

1. Inicie sesión en [http://azure.microsoft.com](http://azure.microsoft.com)
2. En la parte izquierda, seleccione Active Directory.
3. En la parte superior, seleccione los proveedores de Multi-Factor Authentication. Aparecerá una lista de los proveedores de Multi-Factor Authentication.
4. Si tiene más de un proveedor de Multi-Factor Authentication, seleccione aquel en el que desea ver el informe de la alerta de fraude y haga clic en Administrar en la parte inferior de la página. Si sólo tiene uno, haga clic en Administrar. Se abrirá el Portal de administración de Azure Multi-Factor Authentication.
5. En el Portal de administración de Azure Multi-Factor Authentication, a la izquierda, en Ver un informe haga clic en Omisión por única vez.
6. Especifique el intervalo de fechas que desea ver en el informe. También puede especificar cualquier nombre de usuario específico, números de teléfono y el estado de los usuarios.
7. Haga clic en Ejecutar. Aparecerá un informe similar al siguiente. También puede hacer clic en Exportar a CSV si desea exportar el informe.

<center>![Cloud](./media/multi-factor-authentication-whats-next/report.png)</center>

## Mensajes de voz personalizados

Los mensajes de voz personalizados permiten utilizar sus propias grabaciones o saludos con la autenticación multifactor. Pueden utilizarse además o reemplazar a las grabaciones de Microsoft.

Antes de comenzar tenga en cuenta lo siguiente:

- Los formatos de archivo compatibles en la actualidad son .wav y. mp3.
- El límite de tamaño de archivo es 5MB.
- Se recomienda que los mensajes de autenticación no tengan más de 20 segundos. Cualquier valor superior a este podría provocar un error de comprobación porque el usuario puede no responder antes de que finalice el mensaje y se agote el tiempo de espera de la comprobación.



### Para configurar mensajes de voz personalizados en Azure Multi-Factor Authentication
<ol>
<li>Cree un mensaje de voz personalizado mediante uno de los formatos de archivo compatibles. Consulte las recomendaciones de mensaje de voz personalizados a continuación.</li>
<li>Inicie sesión en [http://azure.microsoft.com](http://azure.microsoft.com)</li>
<li>En la parte izquierda, seleccione Active Directory.</li>
<li>En la parte superior, seleccione los proveedores de Multi-Factor Authentication. Aparecerá una lista de los proveedores de Multi-Factor Authentication.</li>
<li>Si tiene más de un proveedor de Multi-Factor Authentication, seleccione aquel en el que desea configurar el mensaje de voz personalizado y haga clic en Administrar en la parte inferior de la página. Si sólo tiene uno, haga clic en Administrar. Se abrirá el Portal de administración de Azure Multi-Factor Authentication.</li>
<li>En el Portal de administración de Azure Multi-Factor Authentication, a la izquierda, haga clic en Mensajes de voz.</li>

<center>![Cloud](./media/multi-factor-authentication-whats-next/custom1.png)</center>

<li>En la sección Mensajes de voz, haga clic en Nuevo mensaje de voz.</li>

<center>![Cloud](./media/multi-factor-authentication-whats-next/custom2.png)</center>

<li>En la página Configurar: Nuevo mensaje de voz, haga clic en Administrar archivos de sonido.</li>

<center>![Cloud](./media/multi-factor-authentication-whats-next/custom3.png)</center>

<li>En la página Configurar: Archivos de sonido, haga clic en Cargar archivo de sonido.</li>

<center>![Cloud](./media/multi-factor-authentication-whats-next/custom4.png)</center>

<li>En la página Configurar: Cargar archivo de sonido, haga clic en Examinar y navegue hasta el mensaje de voz, haga clic en Abrir.</li>
<li>Agregue una descripción y haga clic en Cargar.</li>
<li>Cuando se complete, verá un mensaje de que se ha cargado correctamente el archivo.</li>
<li>A la izquierda, haga clic en Mensajes de voz.</li>
<li>En la sección Mensajes de voz, haga clic en Nuevo mensaje de voz.</li>
<li>En la lista desplegable Idioma, seleccione un idioma.</li>
<li>Si este mensaje es para una aplicación específica, especifíquela en el cuadro aplicación.</li>
<li>En Tipo de mensaje, seleccione el tipo de mensaje que se va a reemplazar con el nuevo mensaje personalizado.</li>
<li>En la lista desplegable Archivo de sonido, seleccione el archivo.</li>
<li>Haga clic en Crear. Verá un mensaje que indica que ha creado correctamente un mensaje de voz.</li>

<center>![Cloud](./media/multi-factor-authentication-whats-next/custom5.png)</center>


## Almacenamiento en caché en Azure Multi-Factor Authentication

El almacenamiento en caché permite establecer un período de tiempo específico para que los intentos de autenticación siguientes se realicen correctamente de forma automática. Esto evita a los usuarios tener que esperar por llamadas telefónicas o textos si se autentican durante el período de tiempo especificado.



### Para configurar el almacenamiento en caché en Azure Multi-Factor Authentication
<ol>

1. Inicie sesión en [http://azure.microsoft.com](http://azure.microsoft.com)
2. En la parte izquierda, seleccione Active Directory.
3. En la parte superior, seleccione los proveedores de Multi-Factor Authentication. Aparecerá una lista de los proveedores de Multi-Factor Authentication.
4. Si tiene más de un proveedor de Multi-Factor Authentication, seleccione aquel en el que desea habilitar la alerta de fraude y haga clic en Administrar en la parte inferior de la página. Si sólo tiene uno, haga clic en Administrar. Se abrirá el Portal de administración de Azure Multi-Factor Authentication.
5. En el Portal de administración de Azure Multi-Factor Authentication, a la izquierda, haga clic en Almacenamiento en caché.
6. En la página Configurar almacenamiento en caché, haga clic en Nueva caché
7. Seleccione el tipo de caché y los segundos en caché. Haga clic en Crear.


<center>![Cloud](./media/multi-factor-authentication-whats-next/cache.png)</center>

## IP de confianza

IP de confianza es una característica de la autenticación multifactor que permite a los administradores de un inquilino administrado o federado la capacidad de omitir la autenticación multifactor para los usuarios que inician sesión desde la intranet local de la empresa. Las características están disponibles para los inquilinos de Azure AD que tienen licencias de Azure AD Premium, Enterprise Mobility Suite o Azure Multi-Factor Authentication.

 
Tipo de un inquilino de Azure AD.| Opciones de IP de confianza disponibles
:------------- | :------------- | 
Administrada|Intervalos de direcciones IP específicos. Los administradores pueden especificar un intervalo de direcciones IP que puede omitir la autenticación multifactor para los usuarios que inician sesión desde la intranet de la empresa.
Federada|<li>Todos los usuarios federados. Todos los usuarios federados que inician sesión desde dentro de la organización omitirán la autenticación multifactor usando una notificación emitida por AD FS.</li><li>Intervalos de direcciones IP específicos. Los administradores pueden especificar un intervalo de direcciones IP que pueden omitir la autenticación multifactor para los usuarios que inician sesión desde la intranet corporativa.

Esta omisión solo funciona desde dentro de la intranet de una empresa. Por ejemplo, si solo ha seleccionado todos los usuarios federados y un usuario inicia sesión desde fuera de la intranet corporativa, ese usuario tendrá que autenticarse mediante la autenticación multifactor, incluso si presenta una notificación de AD FS. La siguiente tabla describe cuando son necesarias las contraseñas de autenticación multifactor y de aplicación dentro y fuera de la red corporativa cuando está habilitada la opción de IP de confianza.


|IP de confianza está activado| IP de confianza está desactivado
:------------- | :------------- | :------------- | 
Dentro de la red corporativa|Para los flujos de explorador NO es necesaria la autenticación multifactor.|Para los flujos de explorador es necesaria la autenticación multifactor
|Para las aplicaciones cliente enriquecidas, las contraseñas regulares funcionarán si el usuario no ha creado ninguna contraseña de aplicación. Una vez que se cree una contraseña de aplicación, se requerirán contraseñas de aplicación.|Para las aplicaciones cliente enriquecidas, se requerirán contraseñas de aplicación.
Fuera de la red corporativa|Para los flujos de explorador es necesaria la autenticación multifactor.|Para los flujos de explorador es necesaria la autenticación multifactor.
|Para las aplicaciones cliente enriquecidas, se requerirán contraseñas de aplicación.|Para las aplicaciones cliente enriquecidas, se requerirán contraseñas de aplicación.

### Para habilitar IP de confianza

 
<ol>
<li>Inicie sesión en el Portal de administración de Azure.</li>
<li>En la parte izquierda, haga clic en Active Directory.</li>
<li>En Directorio, haga clic en el directorio en el que desea configurar IP de confianza.</li>
<li>En el directorio que ha seleccionado, haga clic en Configurar.</li>
<li>En la sección de la autenticación multifactor, haga clic en Administrar configuración del servicio.</li>
<li>En la página Configuración del servicio, en IP de confianza, seleccione: <ul> <li>Para solicitudes de usuarios federados cuyo origen esté en mi intranet: todos los usuarios federados que inician sesión desde la red corporativa omitirán la autenticación multifactor a través del uso de una notificación emitida por AD FS.

<li>Para solicitudes de un intervalo de IP públicas específico: escriba las direcciones IP en los cuadros proporcionados usando la notación CIDR. Por ejemplo: xxx.xxx.xxx.0/24 para direcciones IP en el intervalo de xxx.xxx.xxx.1 – xxx.xxx.xxx.254, o xxx.xxx.xxx.xxx/32 para una única dirección IP. Puede introducir hasta 12 intervalos de direcciones IP.</li></ul>


<center>![Cloud](./media/multi-factor-authentication-whats-next/trustedips.png)</center>


<li>Haga clic en Guardar.</li>
<li>Una vez que se han aplicado las actualizaciones, haga clic en Cerrar.</li>
## Contraseñas de aplicación

En algunas aplicaciones, como Office 2010 o anteriores y Apple Mail no se puede utilizar la autenticación multifactor. Para usar estas aplicaciones, tendrá que utilizar "contraseñas de aplicación" en lugar de la contraseña tradicional. Las contraseñas de aplicación permiten a la aplicación omitir la autenticación multifactor y poder seguir trabajando.

>[AZURE.NOTE]Autenticación moderna para los clientes de Office 2013
>
> Los clientes de Office 2013 (incluye Outlook) ahora admiten nuevos protocolos de autenticación que pueden habilitarse para admitir Multi-Factor Authentication. Esto significa que una vez habilitada, las contraseñas de aplicación no son necesarias para los clientes de Office 2013. Para obtener más información, consulte [Anuncio de la vista previa pública de la autenticación moderna de Office 2013](https://blogs.office.com/2015/03/23/office-2013-modern-authentication-public-preview-announced/).

### Aspectos importantes acerca de las contraseñas de aplicación

La siguiente es una lista importante de cosas que debe saber acerca de las contraseñas de aplicación.

Experiencia de autenticación|Para las aplicaciones basadas en explorador|Para las aplicaciones no basadas en explorador
:------------- | :------------- | :------------- 
|<ul><li>El primer factor de autenticación se lleva a cabo localmente</li><li>el segundo factor es un método basado en el teléfono efectuado por Identidad de nube.</li>|<ul><li>Los administradores y usuarios pueden usar contraseñas de aplicación para iniciar sesión.

- Los usuarios pueden tener varias contraseñas de aplicación lo que aumenta el área expuesta al robo. Como son difíciles de recordar, las contraseñas de aplicación se suelen anotar. Esto no es recomendable y no debe hacerse porque para iniciar sesión con la contraseña de aplicación es necesario solo un factor.
- Aplicaciones que almacenar en caché las contraseñas y usan este método en escenarios locales, pueden fallar ya que la contraseña de aplicación no se reconocerá fuera del identificador de organización. Un ejemplo es el de los mensajes de correo electrónico de Exchange que son locales pero que se archivan en la nube. La misma contraseña no funcionará.
- La contraseña real se genera automáticamente y no la proporciona el usuario. Esto es así porque una contraseña generada automáticamente es más difícil de adivinar y por tanto es más segura.
- Actualmente hay un límite de 40 contraseñas por usuario. Para crear una nueva contraseña de aplicación se le pedirá que elimine una de las existentes.


### Guía de nomenclatura para las contraseñas de aplicación
Se recomienda que los nombres de contraseña de aplicación reflejen el dispositivo en el que se van a usar. Por ejemplo, si tiene un equipo portátil que tiene aplicaciones sin explorador como Outlook, Word y Excel, sólo necesita crear una contraseña de aplicación denominada Portátil y utilizar esa contraseña de aplicación en todas estas aplicaciones. Aunque puede crear contraseñas independientes para cada una de estas aplicaciones, no se recomienda. El modo recomendado es usar una contraseña de aplicación por dispositivo.


<center>![Cloud](./media/multi-factor-authentication-whats-next/naming.png)</center>


### Contraseñas de aplicación federada (SSO)
Azure AD admite la federación con los Servicios de dominio de Active Directory (AD DS) de Windows Server. Si su organización está federada (SSO) con Azure AD y va a usar Azure Multi-Factor Authentication, a continuación tiene información importante que debe tener en cuenta al utilizar las contraseñas de aplicación. Esto se aplica solo a los clientes federados (SSO).

- La contraseña de aplicación se comprueba mediante Azure AD y, por tanto, omite la federación. La federación solo se usa activamente al configurar la contraseña de aplicación.
- Para los usuarios federados (SSO), nunca se pasará al proveedor de identidades (IdP) a diferencia del flujo pasivo. Las contraseñas se almacenarán en el identificador de la organización. Si el usuario abandona la empresa, esta información tiene que fluir al identificador de la organización usando DirSync en tiempo real. La deshabilitación o eliminación de la cuenta puede tardar hasta 3 horas en sincronizarse, retrasando la deshabilitación o eliminación de contraseña de aplicación en Azure AD.
- La configuración del control de acceso de cliente local no se cumple con la contraseña de aplicación
- No hay ningún registro o capacidad de auditoría en la autenticación local que esté disponible para la contraseña de aplicación
- Es necesaria una mayor formación del usuario final para el cliente de Microsoft Lync 2013. Para los pasos necesarios, vea cómo cambiar la contraseña en su correo electrónico a la contraseña de aplicación.
- Algunos diseños de arquitectura avanzada pueden requerir el uso de una combinación de nombre de usuario y contraseñas de la organización y de contraseñas de aplicación cuando se usa la autenticación multifactor con clientes, en función de dónde se autentiquen. Para los clientes que se autentican en una infraestructura local, utilizaría un nombre de usuario y una contraseña de la organización. Para los clientes que se autentican en Azure AD, usaría la contraseña de aplicación.

Por ejemplo, suponga que tiene una arquitectura que consta de lo siguiente:

- Federa su instancia local de Active Directory con Azure AD
- Usa Exchange Online
- Usa Lync que es específicamente local
- Usa Azure Multi-Factor Authentication


<center>![Proofup](./media/multi-factor-authentication-whats-next/federated.png)</center>

 En estos casos, debe hacer lo siguiente:

- Cuando inicie sesión en Lync, utilice el nombre de usuario y la contraseña de su organización.
- Al intentar obtener acceso a la libreta de direcciones a través de un cliente de Outlook que se conecta a Exchange online, utilice una contraseña de aplicación.

### Permiso para la creación de contraseñas de aplicación
De forma predeterminada, los usuarios no pueden crear contraseñas de aplicación. Esta característica debe habilitarse. Para permitir que los usuarios tengan la capacidad de crear contraseñas de aplicación utilice el procedimiento siguiente.

#### Para permitir que los usuarios creen contraseñas de aplicación



1. Inicie sesión en el Portal de administración de Azure.
2. En la parte izquierda, haga clic en Active Directory.
3. En Directorio, haga clic en el directorio para el usuario que desea activar.
4. En la parte superior, haga clic en Usuarios.
5. En la parte inferior de la página, haga clic en Admin. Multi-Factor Auth. Con esto se abrirá la página de Multi-Factor Authentication.
6. En la parte superior de la página de autenticación multifactor, haga clic en Configuración del servicio.
7. Asegúrese de que está seleccionado el botón de opción de Permitir a los usuarios crear contraseñas de aplicación para iniciar sesión en aplicaciones que no son de explorador.

<center>![Cloud](./media/multi-factor-authentication-whats-next/trustedips.png)</center>

### Creación de contraseñas de aplicación
Los usuarios pueden crear contraseñas de aplicación durante el registro inicial. Se les da una opción al final del proceso de registro que les permite hacerlo.

Además los usuarios también pueden crear contraseñas de aplicación más adelante cambiando su configuración en el Portal de Azure, el portal de Office 365 o

### Creación de contraseñas de aplicación en el portal de Office 365
--------------------------------------------------------------------------------


1. Inicie sesión en el portal de Office 365
2. En la esquina superior derecha seleccione el widget de configuración
3. A la izquierda seleccione Comprobación de seguridad adicional
4. A la derecha, seleccione **Actualizar los números de teléfono usados para la seguridad de cuenta**
5. En la página de proofup, en la parte superior, seleccione las contraseñas de aplicación
6. Haga clic en **Crear**
7. Escriba un nombre para la contraseña de aplicación y haga clic en **Siguiente**
8. Copie la contraseña de aplicación en el Portapapeles y péguela en la aplicación.

<center>![Cloud](./media/multi-factor-authentication-whats-next/security.png)</center>


### Creación de contraseñas de aplicación en el Portal de Azure
--------------------------------------------------------------------------------
1. Inicie sesión en el Portal de administración de Azure
3. En la parte superior, haga clic con el botón derecho en su nombre de usuario y seleccione Comprobación de seguridad adicional.
5. En la página de proofup, en la parte superior, seleccione las contraseñas de aplicación
6. Haga clic en **Crear**
7. Escriba un nombre para la contraseña de aplicación y haga clic en **Siguiente**
8. Copie la contraseña de aplicación en el Portapapeles y péguela en la aplicación.


<center>![Cloud](./media/multi-factor-authentication-whats-next/app2.png)</center>

### Para crear contraseñas de aplicación si no tiene una suscripción a Office 365 o Azure
--------------------------------------------------------------------------------
1. Inicie sesión en [https://myapps.microsoft.com](https://myapps.microsoft.com)	
2. En la parte superior, seleccione el perfil.
3. Haga clic en su nombre de usuario y seleccione Comprobación de seguridad adicional.
5. En la página de proofup, en la parte superior, seleccione las contraseñas de aplicación
6. Haga clic en **Crear**
7. Escriba un nombre para la contraseña de aplicación y haga clic en **Siguiente**
8. Copie la contraseña de aplicación en el Portapapeles y péguela en la aplicación.

<center>![Cloud](./media/multi-factor-authentication-whats-next/myapp.png)</center>

## Suspensión de Multi-Factor Authentication para exploradores y dispositivos recordados (vista previa pública)

Suspender Multi-Factor Authentication para exploradores y dispositivos recordados es una característica que le permite proporcionar a los usuarios la opción de suspender MFA durante un número definido de días después de realizar un inicio de sesión correcto con MFA. Se trata de una característica disponible para todos los usuarios MFA que mejora su facilidad de uso. Sin embargo, puesto que los usuarios pueden suspender MFA, esta característica puede reducir la seguridad de la cuenta.

Para asegurarse de que se protegen las cuentas de usuario, debe restaurar Multi-Factor Authentication para sus dispositivos si se presenta cualquiera de los siguientes escenarios:

- Si su cuenta corporativa se ha visto comprometida
- Si un dispositivo recordado se ha perdido o ha sido robado

> [AZURE.NOTE]Esta característica se implementa como una caché de cookies del explorador. No funcionará si no están habilitadas las cookies del explorador.

### Activación y desactivación de la suspensión de MFA para los dispositivos recordados

<ol>
<li>Inicie sesión en el Portal de administración de Azure.</li>
<li>En la parte izquierda, haga clic en Active Directory.</li>
<li>En Active Directory, haga clic en el directorio en el que desea configurar Suspensión de Multi-Factor Authentication para dispositivos recordados.</li>
<li>En el directorio que ha seleccionado, haga clic en Configurar.</li>
<li>En la sección de la autenticación multifactor, haga clic en Administrar configuración del servicio.</li>
<li>En la página Configuración del servicio, en administrar la configuración de dispositivo de usuario, seleccione o anule la selección de la opción para **permitir a los usuarios suspender la autenticación multifactor haciendo que se recuerde el dispositivo **.</li>
![Suspender dispositivos](./media/multi-factor-authentication-manage-users-and-devices/suspend.png) <li>Establezca el número de días que desea permitir la suspensión. El valor predeterminado es 14 días</li>. <li>Haga clic en Guardar.</li> <li>Haga clic en Cerrar.</li>

<!---HONumber=August15_HO6-->