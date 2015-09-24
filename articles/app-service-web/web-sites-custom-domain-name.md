<properties
	pageTitle="Configurar un nombre de dominio personalizado en el servicio de aplicaciones de Azure"
	description="Obtenga información sobre cómo usar un nombre de dominio personalizado con una aplicación web en el servicio de aplicaciones de Azure."
	services="app-service\web"
	documentationCenter=""
	authors="MikeWasson"
	manager="wpickett"
	editor=""/>

<tags
	ms.service="app-service-web"
	ms.workload="web"
	ms.tgt_pltfrm="na"
	ms.devlang="na"
	ms.topic="article"
	ms.date="07/13/2015"
	ms.author="mwasson"/>

# Configurar un nombre de dominio personalizado en el servicio de aplicaciones de Azure

<div class="dev-center-tutorial-selector sublanding">
  <a href="/documentation/articles/custom-dns-web-site-buydomains-web-app" title="Aplicaciones web">Comprar dominio para aplicaciones web</a> <a href="/documentation/articles/web-sites-custom-domain-name" title="Aplicaciones web" class="current">Aplicaciones web con dominios externos</a> <a href="/documentation/articles/web-sites-traffic-manager-custom-domain-name/" title="Aplicaciones web con el Administrador de tráfico">Aplicaciones web con el Administrador de tráfico</a> <a href="/documentation/articles/web-sites-godaddy-custom-domain-name" title="GoDaddy">GoDaddy</a>

</div>

[AZURE.INCLUDE [websites-cloud-services-css-guided-walkthrough](../../includes/websites-cloud-services-css-guided-walkthrough.md)]

Cuando crea una aplicación web, Azure la asigna a un subdominio de azurewebsites.net. Por ejemplo, si la aplicación web se denomina **contoso**, la dirección URL es **contoso.azurewebsites.net**. Azure también asigna una dirección IP virtual.

Para un sitio web de producción, probablemente quiera que los usuarios vean un nombre de dominio personalizado. En este artículo se explica cómo reservar o configurar un dominio personalizado con [Aplicaciones web del Servicio de aplicaciones](http://go.microsoft.com/fwlink/?LinkId=529714).

[AZURE.INCLUDE [introfooter](../../includes/custom-dns-web-site-intro-notes.md)]


## Información general

Si ya tiene un nombre de dominio o desea reservar un dominio de otros registradores de dominio, estos son los pasos generales que debe segur para importar un nombre de dominio personalizado para la aplicación web:

1. Reserve el nombre de dominio. En este artículo no se cubre ese proceso. Hay varios registradores de dominios entre los que elegir. Al registrar, el sitio le guiará por el proceso:
1. Cree registros DNS que asignen el dominio a su aplicación web de Azure.
1. Agregue el nombre de dominio en el [Portal de Azure](http://go.microsoft.com/fwlink/?LinkId=529715).

En esta descripción básica, hay algunos casos específicos que se deben tener en cuenta:

- Asignación de su dominio raíz. El dominio raíz es el dominio que reservó con el registrador de dominios. Por ejemplo, **contoso.com**.
- Asignación de un subdominio. Por ejemplo, **blogs.contoso.com**. Puede asignar distintos subdominios a aplicaciones web diferentes.
- Asignación de un comodín. Por ejemplo, ***.contoso.com**. Una entrada comodín se aplica a todos los subdominios del dominio.

[AZURE.INCLUDE [modos](../../includes/custom-dns-web-site-modes.md)]


## Tipos de registro DNS

El sistema de nombres de dominio (DNS) usa registros de datos para asignar nombres de dominio en direcciones IP. Existen varios tipos de registros DNS: Para sitios web, creará un registro *A* o *CNAME*.

- Un registro A **(Dirección)** asigna un nombre de dominio a una dirección IP.
- Un registro **CNAME (Nombre canónico)** asigna un nombre de dominio a otro nombre de dominio. DNS usa el segundo nombre para buscar la dirección. Los usuarios ven el primer nombre de dominio en su explorador. Por ejemplo, podría asignar contoso.com a *&lt;yourwebapp&gt;*.azurewebsites.net.

Si cambia la dirección IP, una entrada CNAME aún es válida, mientras que un registro A se debe actualizar. Sin embargo, algunos registradores de dominios no permiten registros CNAME para el dominio raíz o para los dominios comodín. En dicho caso, debe usar un registro A.

> [AZURE.NOTE]La dirección IP puede cambiar si elimina y vuelve a crear la aplicación web o si vuelve a cambiar el modo de la aplicación web a libre.


## Búsqueda de la dirección IP virtual

Si está creando un registro CNAME, omita este paso. Para crear un registro A, necesita la dirección IP virtual de su aplicación web. Para obtener la dirección IP:

1.	En el explorador, abra el [Portal de Azure](https://portal.azure.com).
2.	Haga clic en la opción **Examinar** del lado izquierdo de la página.
3.	Haga clic en la hoja **Aplicaciones web**.
4.	Haga clic en el nombre de la aplicación web.
5.	En la página **Essentials**, haga clic en **Toda la configuración**.
6.	Haga clic en **Dominios personalizados y SSL**. 
7.	En la hoja **Dominios personalizados y SSL**, haga clic en **Traer dominios externos**. La dirección IP se encuentra en la parte inferior de esta parte.

## Creación de registros DNS

Inicie sesión en el registrador de dominios y use su herramienta para agregar un registro a o un registro CNAME. La aplicación web de los registradores es un poco diferente, pero a continuación se proporcionan algunas pautas generales.

1.	Busque la página de administración de registros DNS. Busque los vínculos o áreas del sitio etiquetadas como **Nombre de dominio**, **DNS** o **Administración del servidor del nombres**. A menudo, el vínculo se encuentra al consultar la información de la cuenta y al buscar un vínculo como **Mis dominios**.
2.	Cuando haya encontrado la página de administración, busque un vínculo que le permita agregar o editar registros DNS. Debe aparecer como **archivo Zona**, **Registros DNS** o un vínculo de configuración a **Opciones avanzadas**.

Esta página podría indicar los registros A y los registros CNAME, o bien ofrecer una lista desplegable donde podrá seleccionar el tipo de registro. Además, es posible que use otros nombres para los tipos de registro, tal como **registros Dirección IP** en vez de registro A o **registro Alias** en lugar del registro CNAME. Normalmente, el registrador crea los registros para usted. Por lo tanto, es posible que ya existan registros para el dominio raíz o para los subdominios comunes, tal como **www**.

Al crear o editar un registro, los campos le permitirán asignar su nombre de dominio a una dirección IP (para registros A) u otro dominio (para registros CNAME). Para un registro CNAME, asignará elementos *desde* su dominio personalizado *a* su subdominio azurewebsites.net.

En muchas herramientas de los registradores, simplemente escribirá la parte de subdominio del dominio, no el nombre completo del dominio. Además, muchas herramientas usan '@' para representar el dominio raíz. Por ejemplo:

<table cellspacing="0" border="1">
  <tr>
    <th>Host</th>
    <th>Tipo de registro</th>
    <th>Dirección IP o dirección URL</th>
  </tr>
  <tr>
    <td>@</td>
    <td>A (dirección)</td>
    <td>127.0.0.1</td>
  </tr>
  <tr>
    <td>www</td>
    <td>CNAME (alias)</td>
    <td>contoso.azurewebsites.net</td>
  </tr>
</table>

Suponiendo que el nombre de dominio personalizado es 'contoso.com', esto crearía los registros siguientes:

- **contoso.com** asignado a 127.0.0.1.
- **www.contoso.com** asignado a **contoso.azurewebsites.net**.

>[AZURE.NOTE]Puede utilizar DNS de Azure para hospedar los registros de dominio necesarios para la aplicación web. Para configurar el dominio personalizado y crear los registros, en DNS de Azure, consulte [Creación de registros de DNS personalizados para una aplicación web](../dns-web-sites-custom-domain).

<a name="awverify" />
## Creación de un registro awverify (solo registros A)

Si crea un registro A, la aplicación web también requiere un registro CNAME especial, que se usa para comprobar es propietario del dominio que está intentando utilizar. Este registro CNAME debe tener el formato siguiente.

- *Si el registro A asigna el dominio raíz o un dominio con comodín: *cree un registro CNAME que se asigna desde **awverify.&lt;yourdomain&gt;** hasta **awverify.&lt;yourwebappname&gt;.azurewebsites.net**. Por ejemplo, si el registro A es para **contoso.com**, cree un registro CNAME para **awverify.contoso.com**.
- *Si el registro A asigna un subdominio específico:* cree un registro CNAME que se asigne desde **awverify.&lt;subdomain&gt;** hasta **awverify.&lt;yourwebappname&gt;.azurewebsites.net**. Por ejemplo, si el registro A es para **blogs.contoso.com**, cree un registro CNAME para **awverify.blogs.contoso.com**.

Los visitantes de su aplicación web no verán el subdominio awverify, este solo es para que Azure compruebe su dominio.

## Habilitación del nombre de dominio en su aplicación web

[AZURE.INCLUDE [modes](../../includes/custom-dns-web-site-enable-on-web-site.md)]

>[AZURE.NOTE]Si desea empezar a trabajar con el Servicio de aplicaciones de Azure antes de suscribirse para abrir una cuenta de Azure, vaya a [Prueba del Servicio de aplicaciones](http://go.microsoft.com/fwlink/?LinkId=523751), donde podrá crear inmediatamente una aplicación web de inicio de corta duración en el Servicio de aplicaciones. No es necesario proporcionar ninguna tarjeta de crédito ni asumir ningún compromiso.


## Pasos siguientes

Para obtener más información, consulte: [Introducción a DNS de Azure](../dns/dns-getstarted-create-dnszone) y [Delegación de dominios en DNS de Azure](../dns/dns-domain-delegation)

## Lo que ha cambiado
* Para obtener una guía del cambio de Sitios web a Servicio de aplicaciones, consulte: [Servicio de aplicaciones de Azure y su impacto en los servicios de Azure existentes](http://go.microsoft.com/fwlink/?LinkId=529714)
* Para obtener una guía del cambio del portal anterior al nuevo, consulte: [Referencia para navegar en el portal de vista previa](http://go.microsoft.com/fwlink/?LinkId=529715)

<!-- Anchors. -->
[Overview]: #overview
[DNS record types]: #dns-record-types
[Find the virtual IP address]: #find-the-virtual-ip-address
[Create the DNS records]: #create-the-dns-records
[Enable the domain name on your web app]: #enable-the-domain-name-on-your-web-app

<!-- Images -->
[subdomain]: media/web-sites-custom-domain-name/azurewebsites-subdomain.png
 

<!---HONumber=August15_HO6-->