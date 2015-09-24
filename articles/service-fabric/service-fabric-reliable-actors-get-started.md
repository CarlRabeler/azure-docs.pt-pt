<properties
   pageTitle="Introducción a los actores confiables | Microsoft Azure"
   description="Este tutorial le guiará a través de los pasos para crear, depurar e implementar un servicio HelloWorld canónico con los actores confiables de Service Fabric."
   services="service-fabric"
   documentationCenter=".net"
   authors="jessebenson"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="08/05/2015"
   ms.author="claudioc"/>

# Actores confiables: el escenario de tutorial de HelloWorld canónico
En este artículo se explican los conceptos básicos de los actores confiables de Service Fabric, además de ofrecer orientación sobre cómo completar los pasos para crear, depurar e implementar una aplicación HelloWorld sencilla en Visual Studio.

## Instalación y configuración
Antes de comenzar, asegúrese de que tiene la configuración del entorno de desarrollo de Service Fabric en el equipo. Encontrará instrucciones detalladas sobre cómo configurar el entorno de desarrollo [aquí](service-fabric-get-started.md).

## Conceptos básicos
Para empezar a trabajar con actores confiables, solo es necesario comprender cuatro conceptos básicos:

* **Servicio de actor**. Los actores confiables se empaquetan en los servicios que se pueden implementar en la infraestructura de Service Fabric. Un servicio puede hospedar uno o varios actores. Se profundizará en los detalles sobre las ventajas y desventajas de un actor frente a varios por servicio más adelante. Por ahora supongamos que debemos implementar solo un actor.
* **Interfaz de actor**. La interfaz de actor se usar para definir la interfaz pública de un actor. En la terminología del modelo actor, se define el tipo de mensajes que el actor es capaz de entender y procesar. Otros actores o aplicaciones de cliente usan la interfaz de actor para "enviar" (asincrónicamente) mensajes al actor. Los actores confiables pueden implementar varias interfaces; como se verá, un actor HelloWorld puede implementar la interfaz IHelloWorld, pero también una interfaz ILogging que define diferentes mensajes y funcionalidades.
* **Registro de actor**. En el servicio de actor, el tipo de actor debe registrarse para que Service Fabric sea consciente del nuevo tipo y puede usarlo para crear nuevos actores.
* **Clase ActorProxy**. La clase ActorProxy se usa para enlazar a un actor e invocar los métodos expuestos a través de sus interfaces. La clase ActorProxy ofrece dos funciones importantes:
	* Resolución de nombres: es capaz de ubicar el actor en el clúster (encontrar el nodo en que se hospeda el clúster).
	* Control de errores: puede volver a intentar las invocaciones de métodos y volver a determinar la ubicación del actor, por ejemplo, tras un error que requiere que el actor se reubique en otro nodo del clúster.

## Creación de un proyecto en Visual Studio
Después de instalar las herramientas de Service Fabric para Visual Studio, puede crear un nuevo tipo de proyecto. Los nuevos tipos de proyecto están en la categoría "Nube" del cuadro de diálogo Nuevo proyecto


![Herramientas de Service Fabric para VS: nuevo proyecto][1]

En el siguiente cuadro de diálogo puede elegir el tipo de proyecto que desea crear.

![Plantillas de proyecto de Service Fabric][5]

Para el proyecto HelloWorld, se usará el servicio de actor de Service Fabric.

Una vez creada la solución, verá la siguiente estructura:

![Estructura de proyecto de Service Fabric][2]

## Bloques de creación básicos de actores de confianza

Una solución típica de actores confiables se compone de tres proyectos:

* El proyecto de aplicación (HelloWorldApplication). Este es el proyecto que empaqueta todos los servicios juntos para la implementación. Contiene los scripts de PowerShell y ApplicationManifest.xml para administrar la aplicación.

* El proyecto de interfaz (HelloWorld.Interfaces). Este es el proyecto que contiene la definición de la interfaz del actor. En el proyecto de interfaces puede definir las interfaces que usarán los actores de la solución.

```csharp

using System;
using System.Collections.Generic;
using System.Linq;
using System.Threading;
using System.Threading.Tasks;
using Microsoft.ServiceFabric.Actors;

namespace HelloWorld.Interfaces
{
    public interface IHelloWorld : IActor
    {
        Task<string> SayHello(string greeting);
    }
}

```

* El proyecto de servicio (HelloWorld). Este es el proyecto que se usa para definir el servicio de Service Fabric que hospedará al actor. Contiene código reutilizable que no deben editarse en la mayoría de casos (ServiceHost.cs) y la implementación del actor. La implementación del actor implica implementar una clase que derive de un tipo base (Actor) e implemente las interfaces definidas en el proyecto .Interfaces.

```csharp

using System;
using System.Collections.Generic;
using System.Linq;
using System.Threading;
using System.Threading.Tasks;
using HelloWorld.Interfaces;
using Microsoft.ServiceFabric;
using Microsoft.ServiceFabric.Actors;

namespace HelloWorld
{
    public class HelloWorld : Actor, IHelloWorld
    {
        public Task<string> SayHello(string greeting)
        {
            return Task.FromResult("You said: '" + greeting + "', I say: Hello Actors!");
        }
    }
}

```

El proyecto de servicio de actor contiene el código para crear un servicio de Service Fabric, en la definición del servicio, los tipos de actor se registran para que se puedan usar para crear una instancia nuevos actores.

```csharp

public class Program
{
    public static void Main(string[] args)
    {
        try
        {
            using (FabricRuntime fabricRuntime = FabricRuntime.Create())
            {
                fabricRuntime.RegisterActor(typeof(HelloWorld));

                Thread.Sleep(Timeout.Infinite);
            }
        }
        catch (Exception e)
        {
            ActorEventSource.Current.ActorHostInitializationFailed(e);
            throw;
        }
    }
}  

```

Si comienza a partir de un proyecto nuevo en Visual Studio y tiene una única definición de actor, el registro se incluye de forma predeterminada en el código que genera Visual Studio. Si define otros actores en el servicio, deberá agregar el registro de actor mediante:

```csharp

fabricRuntime.RegisterActor(typeof(MyNewActor));


```

## Depuración

Las herramientas de Service Fabric para Visual Studio admiten la depuración en el equipo local. Puede iniciar una sesión de depuración presionando F5. Visual Studio genera (si es necesario) paquetes, implementa la aplicación en el clúster de Service Fabric local y asocia al depurador. La experiencia es similar a la depuración de una aplicación ASP.NET. Durante el proceso de implementación, puede ver el progreso en la Ventana de salida

![Ventana de salida de depuración de Service Fabric][3]


## Pasos siguientes

[Introducción a Actores confiables de Service Fabric](service-fabric-reliable-actors-introduction.md) [Documentación de referencia de las API de actores](https://msdn.microsoft.com/library/azure/dn971626.aspx) [Código de ejemplo](https://github.com/Azure/servicefabric-samples)


<!--Image references-->
[1]: ./media/service-fabric-reliable-actors-get-started/reliable-actors-newproject.PNG
[2]: ./media/service-fabric-reliable-actors-get-started/reliable-actors-projectstructure.PNG
[3]: ./media/service-fabric-reliable-actors-get-started/debugging-output.PNG
[4]: ./media/service-fabric-reliable-actors-get-started/vs-context-menu.png
[5]: ./media/service-fabric-reliable-actors-get-started/reliable-actors-newproject1.PNG

<!---HONumber=August15_HO6-->