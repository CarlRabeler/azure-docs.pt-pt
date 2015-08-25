<properties 
	pageTitle="Introducción en cinco minutos a Almacenamiento de Microsoft Azure" 
	description="Mejore sus habilidades rápidamente en blobs, tablas y colas de Azure de Microsoft con las guías de inicio rápido de Azure, Visual Studio y con Emulador de almacenamiento de Azure. Ejecute su primera aplicación de Almacenamiento de Azure en cinco minutos." 
	services="storage" 
	documentationCenter=".net" 
	authors="tamram" 
	manager="adinah" 
	editor=""/>

<tags 
	ms.service="storage" 
	ms.workload="storage" 
	ms.tgt_pltfrm="na" 
	ms.devlang="dotnet" 
	ms.topic="article" 
	ms.date="05/28/2015" 
	ms.author="tamram;selcint"/>

# Introducción en cinco minutos a Almacenamiento de Azure 

Es fácil empezar a realizar tareas de desarrollo con Almacenamiento de Azure. Este tutorial muestra cómo ejecutar rápidamente una aplicación de Almacenamiento de Azure. A continuación se muestra dos escenarios para empezar fácilmente con Almacenamiento de Azure:

- [Ejecución de la primera aplicación de Almacenamiento de Azure de forma local en el Emulador de almacenamiento de Azure](#run-your-first-azure-storage-application-locally-against-the-azure-storage-emulator)
- [Ejecución de la primera aplicación de Almacenamiento de Azure en Almacenamiento de Azure en la nube](#run-your-first-azure-storage-application-against-azure-storage-in-the-cloud)

Si desea obtener más información sobre Almacenamiento de Azure antes de entrar en el código, consulte [Pasos siguientes](#next-steps).

## Requisitos previos

Asegúrese de que cumple los siguientes requisitos previos antes de empezar:

1. Para compilar y crear la aplicación, es necesario tener [Visual Studio](https://www.visualstudio.com/) instalado en el equipo. 
2. Instale la versión más reciente de [SDK de Azure para .NET](http://azure.microsoft.com/downloads/). SDK incluye los proyectos de ejemplo de la guía rápida de Azure, Emulador de almacenamiento de Azure y el [Biblioteca de cliente de Almacenamiento de Azure para .NET](https://msdn.microsoft.com/library/azure/wa_storage_30_reference_home.aspx).
3. Asegúrese de que tiene [.NET Framework 4.5](http://www.microsoft.com/download/details.aspx?id=30653) instalado en su equipo ya que es necesario para los proyectos de ejemplo de la guía rápida de Azure que usaremos en este tutorial. Si no está seguro de qué versión de .NET Framework está instalada en su equipo, consulte [Cómo saber qué versiones de .NET Framework tengo instaladas](https://msdn.microsoft.com/vstudio/hh925568.aspx). O pulse el botón **Inicio** o la tecla de Windows y escriba **Panel de Control**. A continuación, haga clic en **Programas** > **Programas y características**, y compruebe si .NET Framework 4.5 aparece en la lista de los programas instalados.

La versión más reciente de los archivos binarios de la biblioteca de cliente de Almacenamiento de Azure está disponible en [NuGet](https://www.nuget.org/packages/WindowsAzure.Storage/).


## Ejecución de la primera aplicación de Almacenamiento de Azure de forma local en el Emulador de almacenamiento de Azure

Al desarrollar una aplicación que utiliza el almacenamiento de Azure, se pueden ejecutar en el [Emulador de almacenamiento de Azure](storage-use-emulator.md). El emulador de almacenamiento proporciona un entorno local que emula los servicios de Blob, Cola y Tabla de Azure con fines de desarrollo. Puede usar el emulador de almacenamiento para probar la aplicación de almacenamiento de forma local, sin necesidad de crear una suscripción de Azure o una cuenta de almacenamiento y sin incurrir en ningún gasto.

Para probarlo, crearemos una sencilla aplicación de Almacenamiento de Azure con uno de los proyectos de ejemplo de la guía rápida de Azure en Visual Studio. Este tutorial se centra en los proyectos de ejemplo de **Almacenamiento de blobs de Azure**, **Almacenamiento de tablas de Azure** y **Almacenamiento de colas de Azure**:

1. Inicie Visual Studio.
2. En el menú **Archivo**, haga clic en **Nuevo proyecto**.
3. En el cuadro de diálogo **Nuevo proyecto**, haga clic en **Instalados** > **Plantillas** > **Visual C#**> **Nube** > **Inicio rápido** > **Servicios de datos**.
	- 3\.a. Seleccione una de las siguientes plantillas: Almacenamiento de blobs de Azure, Almacenamiento de tablas de Azure o Almacenamiento de colas de Azure. 
	- 3\.b. Asegúrese de que **.NET Framework 4.5** sea el marco de trabajo de destino seleccionado.	
	- 3\.c. Especifique un nombre para el proyecto y cree la nueva solución de Visual Studio, como se muestra aquí:
	
	![Guías rápidas de Azure][Image1]

Es aconsejable revisar el código fuente antes de ejecutar la aplicación. Para revisar el código, seleccione **Explorador de soluciones** en el menú **Vista** en Visual Studio. A continuación, haga doble clic en el archivo Program.cs.

A continuación, ejecute la aplicación de ejemplo en Emulador de almacenamiento de Azure:

1.	Pulse el botón **Inicio** o la tecla de Windows, busque *Emulador de Almacenamiento de Azure* e inicie la aplicación.
2.	En Visual Studio, haga clic en **Crear solución** en el menú **Crear**. 
3.	En el menú **Depurar**, pulse **F11** para ejecutar la solución paso a paso o pulse **F5** para ejecutar la solución de principio a fin.

## Ejecución de la primera aplicación de Almacenamiento de Azure en Almacenamiento de Azure en la nube

Para ejecutar en Almacenamiento de Azure en la nube, necesitará una suscripción de Azure y una cuenta de almacenamiento, si aún no tiene una:

- Para obtener una suscripción de Azure, consulte [Prueba gratuita](http://azure.microsoft.com/pricing/free-trial/), [Opciones de compra](http://azure.microsoft.com/pricing/purchase-options/) y [Ofertas para miembros](http://azure.microsoft.com/pricing/member-offers/) (para miembros de MSDN, Microsoft Partner Network, BizSpark y otros programas de Microsoft).
- Para crear una cuenta de almacenamiento en Azure, consulte [Cómo crear, administrar o eliminar una cuenta de almacenamiento](storage-create-storage-account.md).

Una vez que tenga la cuenta, cree una sencilla aplicación de Almacenamiento de Azure con uno de los proyectos de ejemplo de la guía rápida de Azure en Visual Studio. Este tutorial se centra en los proyectos de ejemplo de **Almacenamiento de blobs de Azure**, **Almacenamiento de tablas de Azure** y **Almacenamiento de colas de Azure**:

1. Inicie Visual Studio.
2. En el menú **Archivo**, haga clic en **Nuevo proyecto**.
3. En el cuadro de diálogo **Nuevo proyecto**, haga clic en **Instalados** > **Plantillas** > **Visual C#**> **Nube** > **Inicio rápido** > **Servicios de datos**.
	- 3\.a. Seleccione una de las siguientes plantillas: Almacenamiento de blobs de Azure, Almacenamiento de tablas de Azure o Almacenamiento de colas de Azure.
	- 3\.b. Asegúrese de que **.NET Framework 4.5** sea el marco de trabajo de destino seleccionado.
	- 3\.c. Especifique un nombre para el proyecto y cree la nueva solución de Visual Studio. 

Es aconsejable revisar el código fuente antes de ejecutar la aplicación. Para revisar el código, seleccione **Explorador de soluciones** en el menú **Vista** en Visual Studio. A continuación, haga doble clic en el archivo Program.cs.

A continuación, ejecute la aplicación de ejemplo:

1.	En Visual Studio, seleccione **Explorador de soluciones** en el menú **Vista**. Haga doble clic en el archivo App.config y marque como comentario la cadena de conexión del Emulador de almacenamiento del SDK de Azure:

	`<!--<add key="StorageConnectionString" value = "UseDevelopmentStorage=true;"/>-->`

2.	Quite el comentario de la cadena de conexión del Servicio de almacenamiento de Azure y proporcione la clave de acceso y el nombre de la cuenta de almacenamiento en el archivo App.config:`<add key="StorageConnectionString" value="DefaultEndpointsProtocol=https;AccountName=[AccountName];AccountKey=[AccountKey]"`

	Para buscar la clave de acceso y el nombre de la cuenta de almacenamiento, consulte [¿Qué es una cuenta de almacenamiento?](storage-whatis-account.md).

3.	Una vez haya proporcionado el nombre de la cuenta de almacenamiento y la clave de acceso en el archivo App.config, en el menú **Archivo** haga clic en **Guardar todo** para guardar todos los archivos del proyecto.
4.	En el menú **Crear**, haga clic en **Crear solución**.
5.	En el menú **Depurar**, pulse **F11** para ejecutar la solución paso a paso o pulse **F5** para ejecutar la solución.


## Pasos siguientes

Para obtener más información sobre Almacenamiento de Azure, consulte los siguientes recursos:

* [Introducción a Almacenamiento de Microsoft Azure](storage-introduction.md)
* [Uso del almacenamiento de blobs en .NET](storage-dotnet-how-to-use-blobs.md)
* [Uso del almacenamiento de tablas en .NET](storage-dotnet-how-to-use-tables.md)
* [Uso del almacenamiento en cola en .NET](storage-dotnet-how-to-use-queues.md)
* [Documentación de Almacenamiento de Azure](http://azure.microsoft.com/documentation/services/storage/)
* [Biblioteca de cliente de almacenamiento de Azure](https://msdn.microsoft.com/library/azure/wa_storage_30_reference_home.aspx)
* [API de REST de almacenamiento de Azure](https://msdn.microsoft.com/library/azure/dd179355.aspx)

[Image1]: ./media/storage-getting-started-guide/QuickStart.png
 

<!---HONumber=August15_HO6-->