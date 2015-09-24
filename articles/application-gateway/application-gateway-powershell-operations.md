<properties
   pageTitle="Creación, inicio o eliminación de una puerta de enlace de aplicaciones | Microsoft Azure"
   description="Creación, configuración, inicio y eliminación de una puerta de enlace de aplicaciones de Azure"
   documentationCenter="na"
   services="application-gateway"
   authors="joaoma"
   manager="jdial"
   editor="tysonn"/>
<tags
   ms.service="application-gateway"
   ms.devlang="na"
   ms.topic="hero-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="06/30/2015"
   ms.author="joaoma"/>

# Creación, inicio o eliminación de una puerta de enlace de aplicaciones

En esta versión, puede crear una puerta de enlace de aplicaciones mediante Azure PowerShell o llamadas a la API de REST. En una próxima versión se incluirá soporte para el Portal de Azure y CLI. Este artículo le guiará por los pasos necesarios para crear, configurar, iniciar y eliminar una puerta de enlace de aplicaciones.

## Antes de empezar

1. Instale la versión más reciente de los cmdlets de Azure PowerShell mediante el Instalador de plataforma web. Puede descargar e instalar la versión más reciente desde la sección **Windows PowerShell** de la [Página de descarga](http://azure.microsoft.com/downloads/).
2. Compruebe que tiene una red virtual de trabajo con una subred válida.
3. Compruebe que dispone de servidores back-end, ya sea en la red virtual o con una dirección IP virtual o dirección IP pública asignada.


Para crear una nueva puerta de enlace de aplicaciones, realice los pasos siguientes en el orden mostrado.

1. [Creación de una Puerta de enlace de aplicaciones](#create-a-new-application-gateway)
2. [Configuración de la puerta de enlace](#configure-the-gateway)
3. [Establecimiento de la configuración de la puerta de enlace](#set-the-gateway-configuration)
4. [Inicio de la puerta de enlace](#start-the-gateway)
4. [Comprobación del estado de la puerta de enlace](#verify-the-gateway-status)

Si desea eliminar una puerta de enlace de aplicaciones, consulte [Eliminación de una puerta de enlace de aplicaciones](#delete-an-application-gateway).

## Creación de una Puerta de enlace de aplicaciones

Para crear la puerta de enlace, use el cmdlet `New-AzureApplicationGateway` y reemplace los valores por los suyos. Tenga en cuenta que la facturación de la puerta de enlace no se inicia en este momento. La facturación comienza en un paso posterior, cuando la puerta de enlace se ha iniciado correctamente.

El siguiente ejemplo muestra el cmdlet en la primera línea, seguido de la salida.

	PS C:\> New-AzureApplicationGateway -Name AppGwTest -VnetName testvnet1 -Subnets @("Subnet-1")

	VERBOSE: 4:31:35 PM - Begin Operation: New-AzureApplicationGateway
	VERBOSE: 4:32:37 PM - Completed Operation: New-AzureApplicationGateway
	Name       HTTP Status Code     Operation ID                             Error
	----       ----------------     ------------                             ----
	Successful OK                   55ef0460-825d-2981-ad20-b9a8af41b399

Para validar la creación de la puerta de enlace, puede usar el cmdlet `Get-AzureApplicationGateway`.

En el ejemplo, *Description*, *InstanceCount* y *GatewaySize* son parámetros opcionales. El valor predeterminado de *InstanceCount* es 2, con un valor máximo de 10. El valor predeterminado de *GatewaySize* es Medium. Small y Large son otros valores disponibles. *Vip* y *DnsName* se muestran en blanco porque todavía no se ha iniciado la puerta de enlace. Se crearán una vez que la puerta de enlace esté en estado de ejecución.




	PS C:\> Get-AzureApplicationGateway AppGwTest
	Name          : AppGwTest
	Description   :
	VnetName      : testvnet1
	Subnets       : {Subnet-1}
	InstanceCount : 2
	GatewaySize   : Medium
	State         : Stopped
	VirtualIPs    : {}
	DnsName       :


## Configuración de la puerta de enlace

Una configuración de puerta de enlace de aplicaciones consta de varios valores. Los valores pueden ir juntos para construir la configuración.

Los valores son:

- **Grupo de servidores de back-end**: la lista de direcciones IP de los servidores back-end. Las direcciones IP mostradas deben pertenecer a la subred de red virtual o deben ser una dirección IP virtual o dirección IP pública.
- **Configuración del grupo de servidores back-end**: cada grupo tiene una configuración como el puerto, el protocolo y la afinidad basada en cookies. Estos valores están vinculados a un grupo y se aplican a todos los servidores del grupo.
- **Puerto front-end**: este puerto es el puerto público abierto en la puerta de enlace de aplicaciones. El tráfico llega a este puerto y, a continuación, se redirige a uno de los servidores backend.
- **Agente de escucha:** la escucha tiene un puerto front-end, un protocolo (Http o Https, que distinguen mayúsculas de minúsculas) y el nombre del certificado SSL (si se configura la descarga de SSL).
- **Regla**: enlaza el agente de escucha y el grupo de servidores back-end, y define a qué grupo de servidores back-end se redirigirá el tráfico cuando se seleccione un agente de escucha concreto. Actualmente, solo se admite la regla *básica*. La regla *básica* es la distribución de carga round robin.

Puede llevar a cabo la configuración mediante la creación de un objeto de configuración o usando un archivo XML de configuración. Para llevar a cabo la configuración usando un archivo XML de configuración, use el siguiente ejemplo.

 **Ejemplo XML de configuración**

	<?xml version="1.0" encoding="utf-8"?>
	<ApplicationGatewayConfiguration xmlns:i="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://schemas.microsoft.com/windowsazure">
	    <FrontendPorts>
	        <FrontendPort>
	            <Name>FrontendPort1</Name>
	            <Port>80</Port>
	        </FrontendPort>
	    </FrontendPorts>
	    <BackendAddressPools>
	        <BackendAddressPool>
	            <Name>BackendPool1</Name>
	            <IPAddresses>
	                <IPAddress>10.0.0.1</IPAddress>
	                <IPAddress>10.0.0.2</IPAddress>
	            </IPAddresses>
	        </BackendAddressPool>
	    </BackendAddressPools>
	    <BackendHttpSettingsList>
	        <BackendHttpSettings>
	            <Name>BackendSetting1</Name>
	            <Port>80</Port>
	            <Protocol>Http</Protocol>
	            <CookieBasedAffinity>Enabled</CookieBasedAffinity>
	        </BackendHttpSettings>
	    </BackendHttpSettingsList>
	    <HttpListeners>
	        <HttpListener>
	            <Name>HTTPListener1</Name>
	            <FrontendPort>FrontendPort1</FrontendPort>
	            <Protocol>Http</Protocol>
	        </HttpListener>
	    </HttpListeners>
	    <HttpLoadBalancingRules>
	        <HttpLoadBalancingRule>
	            <Name>HttpLBRule1</Name>
	            <Type>basic</Type>
	            <BackendHttpSettings>BackendSetting1</BackendHttpSettings>
	            <Listener>HTTPListener1</Listener>
	            <BackendAddressPool>BackendPool1</BackendAddressPool>
	        </HttpLoadBalancingRule>
	    </HttpLoadBalancingRules>
	</ApplicationGatewayConfiguration>

## Establecimiento de la configuración de la puerta de enlace

Después, establecerá la puerta de enlace de aplicaciones. Puede usar el cmdlet `Set-AzureApplicationGatewayConfig` con un objeto de configuración o con un archivo XML de configuración.


	PS C:\> Set-AzureApplicationGatewayConfig -Name AppGwTest -ConfigFile D:\config.xml

	VERBOSE: 7:54:59 PM - Begin Operation: Set-AzureApplicationGatewayConfig
	VERBOSE: 7:55:32 PM - Completed Operation: Set-AzureApplicationGatewayConfig
	Name       HTTP Status Code     Operation ID                             Error
	----       ----------------     ------------                             ----
	Successful OK                   9b995a09-66fe-2944-8b67-9bb04fcccb9d

## Inicio de la puerta de enlace

Una vez configurada la puerta de enlace, use el cmdlet `Start-AzureApplicationGateway` para iniciar la puerta de enlace. La facturación de una puerta de enlace de aplicaciones comienza después de que se inicie correctamente.


> [AZURE.NOTE]El cmdlet `Start-AzureApplicationGateway` puede tardar hasta 15-20 minutos en completarse.



	PS C:\> Start-AzureApplicationGateway AppGwTest

	VERBOSE: 7:59:16 PM - Begin Operation: Start-AzureApplicationGateway
	VERBOSE: 8:05:52 PM - Completed Operation: Start-AzureApplicationGateway
	Name       HTTP Status Code     Operation ID                             Error
	----       ----------------     ------------                             ----
	Successful OK                   fc592db8-4c58-2c8e-9a1d-1c97880f0b9b

## Comprobación del estado de la puerta de enlace

Use el cmdlet `Get-AzureApplicationGateway` para comprobar el estado de la puerta de enlace. Si *Start-AzureApplicationGateway* se ha completado correctamente en el paso anterior, el estado debería ser *En ejecución * y los valores VIP y DnsName deben tener entradas válidas.

En el siguiente ejemplo se muestra una puerta de enlace de aplicaciones activa y que está lista para recibir el tráfico destinado a `http://<generated-dns-name>.cloudapp.net`.

	PS C:\> Get-AzureApplicationGateway AppGwTest

	VERBOSE: 8:09:28 PM - Begin Operation: Get-AzureApplicationGateway
	VERBOSE: 8:09:30 PM - Completed Operation: Get-AzureApplicationGateway
	Name          : AppGwTest
	Description   :
	VnetName      : testvnet1
	Subnets       : {Subnet-1}
	InstanceCount : 2
	GatewaySize   : Medium
	State         : Running
	Vip           : 138.91.170.26
	DnsName       : appgw-1b8402e8-3e0d-428d-b661-289c16c82101.cloudapp.net


## Eliminación de una Puerta de enlace de aplicaciones

Para eliminar una puerta de enlace de aplicaciones:

1. Use el cmdlet `Stop-AzureApplicationGateway` para detener la puerta de enlace.
2. Use el cmdlet `Remove-AzureApplicationGateway` para quitar la puerta de enlace.
3. Compruebe que se ha quitado la puerta de enlace mediante el uso del cmdlet `Get-AzureApplicationGateway`.

En el siguiente ejemplo se muestra el cmdlet `Stop-AzureApplicationGateway` en la primera línea, seguido de la salida.

	PS C:\> Stop-AzureApplicationGateway AppGwTest

	VERBOSE: 9:49:34 PM - Begin Operation: Stop-AzureApplicationGateway
	VERBOSE: 10:10:06 PM - Completed Operation: Stop-AzureApplicationGateway
	Name       HTTP Status Code     Operation ID                             Error
	----       ----------------     ------------                             ----
	Successful OK                   ce6c6c95-77b4-2118-9d65-e29defadffb8

Cuando la puerta de enlace de aplicaciones tenga el estado detenido, use el cmdlet `Remove-AzureApplicationGateway` para quitar el servicio.


	PS C:\> Remove-AzureApplicationGateway AppGwTest

	VERBOSE: 10:49:34 PM - Begin Operation: Remove-AzureApplicationGateway
	VERBOSE: 10:50:36 PM - Completed Operation: Remove-AzureApplicationGateway
	Name       HTTP Status Code     Operation ID                             Error
	----       ----------------     ------------                             ----
	Successful OK                   055f3a96-8681-2094-a304-8d9a11ad8301

Para comprobar que se ha quitado el servicio, puede usar el cmdlet `Get-AzureApplicationGateway`. Este paso no es necesario.


	PS C:\> Get-AzureApplicationGateway AppGwTest

	VERBOSE: 10:52:46 PM - Begin Operation: Get-AzureApplicationGateway

	Get-AzureApplicationGateway : ResourceNotFound: The gateway does not exist.
	.....

## Pasos siguientes

Si desea configurar la descarga SSL, consulte [Configuración de una puerta de enlace de aplicaciones para descarga SSL](application-gateway-ssl.md).

Si desea configurar una puerta de enlace de aplicaciones para usarla con ILB, consulte [Creación de una puerta de enlace de aplicaciones con un equilibrador de carga interno (ILB)](application-gateway-ilb.md).

Si desea obtener más información acerca de opciones de equilibrio de carga en general, vea:

- [Equilibrador de carga de Azure](https://azure.microsoft.com/documentation/services/load-balancer/)
- [Administrador de tráfico de Azure](https://azure.microsoft.com/documentation/services/traffic-manager/)

<!---HONumber=August15_HO8-->