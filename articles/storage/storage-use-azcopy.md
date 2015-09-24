<properties 
	pageTitle="Usar AzCopy con Almacenamiento de Microsoft Azure" 
	description="Aprenda a utilizar la herramienta AzCopy para cargar, descargar y copiar contenidos de blobs y archivos." 
	services="storage" 
	documentationCenter="" 
	authors="tamram" 
	manager="adinah" 
	editor="cgronlun"/>

<tags 
	ms.service="storage" 
	ms.workload="storage" 
	ms.tgt_pltfrm="na" 
	ms.devlang="na" 
	ms.topic="article" 
	ms.date="06/22/2015" 
	ms.author="tamram"/>

# Introducción a la utilidad de línea de comandos AzCopy

## Información general

AzCopy es una utilidad de línea de comandos diseñada para realizar operaciones de alto rendimiento de carga, descarga y copia de datos a y desde Almacenamiento de blobs, archivos y tablas de Microsoft Azure. Esta guía proporciona información general para usar AzCopy.

> [AZURE.NOTE]Esta guía asume que ha instalado AzCopy 3.2.0 o posterior. AzCopy 3.x ahora se encuentra diponible a nivel general.
> 
> En esta guía también se aborda la utilización de AzCopy 4.2.0, que es una versión en vista previa de AzCopy. Asimismo, en la guía, las funciones proporcionadas solo en la versión de vista previa se designan como *vista previa*.
> 
> Tenga en cuenta que, para AzCopy 4.x, las opciones de la línea de comandos y la funcionalidad pueden cambiar en versiones futuras.

## Descarga e instalación de AzCopy

1. Descargue la [versión más reciente de AzCopy](http://aka.ms/downloadazcopy) o la [versión más reciente de vista previa](http://aka.ms/downloadazcopypr).
2. Ejecución de la instalación. De forma predeterminada, la instalación de AzCopy crea una carpeta llamada `AzCopy` bajo `%ProgramFiles(x86)%\Microsoft SDKs\Azure` (en una máquina con Windows de 64 bits) o `%ProgramFiles%\Microsoft SDKs\Azure` (en una máquina con Windows de 32 bits). Sin embargo, puede cambiar la ruta de acceso de instalación desde el asistente para la instalación.
3. Si lo desea, puede agregar la ubicación de instalación de AzCopy a la ruta de acceso del sistema.

## Introducción a la sintaxis de la línea de comandos de AzCopy

A continuación, abra una ventana de comandos y vaya al directorio de instalación de AzCopy en el equipo, donde se encuentra el ejecutable `AzCopy.exe`. La sintaxis básica del comando AzCopy es:

	AzCopy /Source:<source> /Dest:<destination> /Pattern:<filepattern> [Options]

> [AZURE.NOTE]A partir de la versión 3.0.0 de AzCopy, la sintaxis de línea de comandos de AzCopy requiere que se especifiquen todos los parámetros para incluir su nombre, *por ejemplo*, `/ParameterName:ParameterValue`.

## Escritura del primer comando de AzCopy

**Cargar un archivo desde el sistema de archivos a Almacenamiento de blobs:**
	
	AzCopy /Source:C:\myfolder /Dest:https://myaccount.blob.core.windows.net/mycontainer /DestKey:key /Pattern:abc.txt

Al copiar un único archivo, recuerde que debe especificar la opción /Pattern con el nombre de archivo. Encontrará más ejemplos en una sección posterior de este artículo.

## Introducción a los parámetros

En la tabla siguiente se describen los parámetros para AzCopy. También puede escribir uno de los siguientes comandos desde la línea de comandos para obtener ayuda en el uso de AzCopy:

- Para obtener ayuda detallada sobre la línea de comandos de AzCopy: `AzCopy /?`
- Para obtener ayuda detallada con algún parámetro de AzCopy: `AzCopy /?:SourceKey`
- Para obtener ejemplos de línea de comandos: `AzCopy /?:Samples` 

<table>
  <tr>
    <th>Nombre de opción</th>
    <th>Descripción</th>
    <th>Aplicable a Almacenamiento de blobs (S/N)</th>
    <th>Aplicable al almacenamiento de archivos (S/N) (solo versión de vista previa)</th>
    <th>Aplicable al almacenamiento de tablas (S/N) (solo versión de vista previa)</th>
  </tr>
  <tr>
    <td><b>/Source:&lt;origen></b></td>
    <td>Especifica los datos de origen desde los que copiar. El origen puede ser un directorio del sistema de archivos, un contenedor de blobs, un directorio virtual de blobs, un recurso compartido de archivos de almacenamiento, un directorio de archivos de almacenamiento o una tabla de Azure.</td>
    <td>Y</td>
    <td>Y<br /> (solo vista previa)</td>
    <td>Y<br /> (solo vista previa)</td>
  </tr>
  <tr>
    <td><b>/Dest:&lt;destino></b></td>
    <td>Especifica el destino de la copia. El destino puede ser un directorio del sistema de archivos, un contenedor de blobs, un directorio virtual de blobs, un recurso compartido de archivos de almacenamiento, un directorio de archivos de almacenamiento o una tabla de Azure.</td>
    <td>Y</td>
    <td>Y<br /> (solo vista previa)</td>
    <td>Y<br /> (solo vista previa)</td>
  </tr>
  <tr>
    <td><b>/Pattern:&lt;patrón-archivo></b></td>
      <td>
          Especifica un patrón de archivos que indica qué archivos se van a copiar. El comportamiento del parámetro /Pattern viene determinado por la ubicación de los datos de origen y la presencia de la opción del modo recursivo. El modo recursivo viene especificado por la opción /S.
          <br />
          Si el origen especificado es un directorio del sistema de archivos, se aplican los caracteres comodín estándar y el patrón de archivos proporcionado coincidirá con los archivos del directorio. Si se especifica la opción /S, AzCopy también hará coincidir el patrón especificado con todos los archivos de cualquier subcarpeta que se encuentre bajo el directorio.
          <br />
          Si el origen especificado es un contenedor de blobs o un directorio virtual, entonces los caracteres comodín no se aplican. Si se especifica la opción /S, AzCopy interpreta el patrón de archivos especificado como un prefijo de blobs. Si la opción /S no se especifica, AzCopy hará coincidir el patrón de archivos con los nombres de blob exactos.
          <br />
          Si el origen especificado es un recurso compartido de archivos de Azure, debe especificar el nombre de archivo exacto (p. ej. abc.txt) para copiar un solo archivo, o especificar la opción&#160;/S&#160;para copiar todos los archivos en el recurso compartido de forma recursiva. Si intenta especificar tanto un patrón de archivos como la opción&#160;/S&#160;conjuntamente, se producirá un error.
          <br />
          AzCopy utiliza la coincidencia entre mayúsculas y minúsculas cuando /Soure es un contenedor de blob o el directorio virtual de blob y utiliza la falta de coincidencia entre mayúsculas y minúsculas en todos los demás casos.
          <br/>
          El patrón de archivos predeterminado usado cuando no se especifica un patrón de archivos es *.* para una ubicación del sistema de archivos, o un prefijo vacío para una ubicación de Almacenamiento de Azure. No se admite la especificación de varios patrones de archivos.</td>
    <td>Y</td>
    <td>Y<br /> (solo vista previa)</td>
    <td>N</td>
  </tr>
  <tr>
    <td><b>/DestKey:&lt;clave-almacenamiento></b></td>
    <td>Especifica la clave de cuenta de almacenamiento para el recurso de destino.</td>
    <td>Y</td>
    <td>Y<br /> (solo vista previa)</td>
    <td>Y<br /> (solo vista previa)</td>
  </tr>
  <tr>
    <td class="auto-style1"><b>/DestSAS:&lt;token-sas></b></td>
    <td class="auto-style1">Especifica una firma de acceso compartido (SAS) para los permisos de LECTURA y ESCRITURA del destino (si corresponde). Encierre la SAS con comillas dobles, ya que puede contener caracteres de línea de comandos especiales.<br />
        Si el recurso de destino es un contenedor de blobs, un recurso compartido de archivos o tabla, puede especificar esta opción seguida por el token SAS, o bien puede especificar la SAS como parte del URI del contenedor de blob, recurso compartido de archivos o tabla, sin esta opción.<br />
        Si el origen y el destino son blobs en ambos casos, el blob de destino debe residir dentro de la misma cuenta de almacenamiento que el blob de origen.</td>
    <td class="auto-style1">Y</td>
    <td class="auto-style1">Y<br /> (solo vista previa)</td>
    <td class="auto-style1">Y<br /> (solo vista previa)</td>
  </tr>
  <tr>
    <td><b>/SourceKey:&lt;clave-almacenamiento></b></td>
    <td>Especifica la clave de cuenta de almacenamiento para el recurso de origen.</td>
    <td>Y</td>
    <td>Y<br /> (solo vista previa)</td>
    <td>Y<br /> (solo vista previa)</td>
  </tr>
  <tr>
    <td><b>/SourceSAS:&lt;token-sas></b></td>
    <td>Especifica una firma de acceso compartido (SAS) para los permisos de LECTURA y LISTA del origen (si corresponde). Encierre la SAS con comillas dobles, ya que puede contener caracteres de línea de comandos especiales.
        <br />
        Si el recurso de origen es un contenedor de blobs, y no se proporciona una clave ni una SAS, el contenedor de blobs se leerá a través de acceso anónimo.
        <br />
        Si el origen es un recurso compartido de archivos o una tabla, es necesario proporcionar una clave o una SAS.</td>
    <td>Y</td>
    <td>Y<br /> (solo vista previa)</td>
    <td>Y<br /> (solo vista previa)</td>
  </tr>
  <tr>
    <td><b>/S</b></td>
    <td>Especifica el modo recursivo para operaciones de copia. En el modo recursivo, AzCopy copiará todos los blobs o archivos que coincidan con el patrón de archivos especificado, incluidos los que se encuentren en las subcarpetas.</td>
    <td>Y</td>
    <td>Y<br /> (solo vista previa)</td>
    <td>N</td>
  </tr>
  <tr>
    <td><b>/BlobType:&lt;bloque | página | anexo></b></td>
    <td>Especifica si el blob de destino es un blob en bloques, un blob en páginas o un blob de anexo. Esta opción solamente es aplicable cuando se carga un blob; de lo contrario, se genera un error. Si el destino es un blob y esta opción no se especifica, AzCopy, de forma predeterminada, creará un blob en bloques.</td>
    <td>Y</td>
    <td>N</td>
    <td>N</td>
  </tr>
  <tr>
    <td><b>/CheckMD5</b></td>
    <td>Calcula un hash MD5 para datos descargados y comprueba que el hash MD5 almacenado en la propiedad Content-MD5 del blob o archivo coincide con el hash calculado. La comprobación MD5 se desactiva de forma predeterminada, por lo que debe especificar esta opción para realizar dicha comprobación cuando se descargan datos.
	<br />
    Tenga en cuenta que Almacenamiento de Azure no garantiza que el hash MD5 almacenado para el blob o archivo esté actualizado. El cliente será el responsable de actualizar el MD5 siempre que el blob o el archivo se modifique.
	<br />
    AzCopy siempre establece la propiedad Content-MD5 para un blob o archivo de Azure después de cargarlo en el servicio.</td>
    <td>Y</td>
    <td>Y<br /> (solo vista previa)</td>
    <td>N</td>
  </tr>
  <tr>
    <td><b>/Snapshot</b></td>
    <td>Indica si se desean transferir instantáneas. Esta opción solo es válida cuando el origen es un blob. 
        <br />
        El nombre de las instantáneas del blob transferidas se cambia según este formato: [nombre del blob] (tiempo de la instantánea)[extensión]. 
        <br />
        De forma predeterminada, las instantáneas no se copian.</td>
    <td>Y</td>
    <td>N</td>
    <td>N</td>
  </tr>
  <tr>
    <td><b>/V:[archivo de registro detallado]</b></td>
    <td>Envía mensajes con el estado detallado a un archivo de registro. De forma predeterminada, el archivo de registro detallado se denomina <code>AzCopyVerbose.log</code> en <code>%LocalAppData%\Microsoft\Azure\AzCopy</code>. Si especifica una ubicación existente para el archivo en esta opción, el registro detallado se anexará a dicho archivo.</td>
    <td>Y</td>
    <td>Y<br /> (solo vista previa)</td>
    <td>Y<br /> (solo vista previa)</td>
  </tr>
  <tr>
    <td><b>/Z:[carpeta de archivos de diario]</b></td>
    <td>Especifica una carpeta de archivos de diario para reanudar una operación.<br />
        AzCopy siempre admite la reanudación si una operación se ha interrumpido.<br />
        Si esta opción no se especifica, o si se especifica sin una ruta de acceso a la carpeta, AzCopy creará el archivo de diario en la ubicación predeterminada, que es <code>%LocalAppData%\Microsoft\Azure\AzCopy</code>.<br />
        Cada vez que emite un comando a AzCopy, comprueba si existe un archivo de diario en la carpeta predeterminada o si existe una carpeta que especificó a través de esta opción. Si el archivo de diario no existe en ningún lugar, AzCopy trata la operación como una nueva y genera un nuevo archivo de diario.
        <br />
		Si el archivo de diario existe, AzCopy comprobará si la línea de comandos escrita coincide con la de dicho archivo. Si las dos líneas de comandos coinciden, AzCopy reanudará la operación incompleta. Si no coinciden, se le preguntará si desea sobrescribir el archivo de diario, iniciar una nueva operación o cancelar la operación actual. 
        <br />
        El archivo de diario se elimina cuando la operación se completa correctamente.
		<br />
		Tenga en cuenta que la reanudación de una operación a partir de un archivo de diario creado por una versión anterior de AzCopy no se admite.</td>
    <td>Y</td>
    <td>Y<br /> (solo vista previa)</td>
    <td>Y<br /> (solo vista previa)</td>
  </tr>
  <tr>
    <td><b>/@:archivo-de-parámetro</b></td>
    <td>Especifica un archivo que contiene parámetros. AzCopy procesa los parámetros del archivo como si hubieran sido especificados en la línea de comandos.<br /> 
		En un archivo de respuesta, puede especificar varios parámetros en una sola línea o especificar cada parámetro en su propia línea. Tenga en cuenta que un parámetro individual no puede ocupar varias líneas. 
        <br />
		Los archivos de respuesta pueden incluir líneas de comentario que comiencen con el símbolo <code>#</code>. 
        <br />
        Puede especificar varios archivos de respuesta. Sin embargo, tenga en cuenta que AzCopy no admite archivos de respuesta anidados.</td>
    <td>Y</td>
    <td>Y<br /> (solo vista previa)</td>
    <td>Y<br /> (solo vista previa)</td>
  </tr>
  <tr>
    <td><b>/Y</b></td>
    <td>Suprime todos los mensajes de confirmación de AzCopy.</td>
    <td>Y</td>
    <td>Y<br /> (solo vista previa)</td>
    <td>Y<br /> (solo vista previa)</td>
  </tr>
  <tr>
    <td><b>/L</b></td>
    <td>Especifica solamente una operación de descripción; no se copian datos.
    <br />
    AzCopy interpretará el uso de esta opción como una simulación para ejecutar la línea de comandos sin esta opción /L y contar el número de objetos que se va a copiar, puede especificar la opción /V al mismo tiempo para comprobar qué objetos se copiarán en el registro detallado.
    <br />
    El comportamiento de esta opción también está determinado por la ubicación de los datos de origen y la presencia de la opción /S del modo recursivo y la opción /Pattern del patrón de archivos.
    <br />
    AzCopy requiere el permiso LISTA y LECTURA de esta ubicación de origen cuando se usa esta opción.</td>
    <td>Y</td>
    <td>Y<br /> (solo vista previa)</td>
    <td>N</td>
  </tr>
  <tr>
    <td><b>/MT</b></td>
    <td>Establece la hora de la última modificación del archivo descargado para que coincida con la del blob o archivo de origen.</td>
    <td>Y</td>
    <td>Y<br /> (solo vista previa)</td>
    <td>N</td>
  </tr>
  <tr>
    <td><b>/XN</b></td>
    <td>Excluye un recurso origen más nuevo. El recurso no se copiará si el origen es más nuevo que el destino.</td>
    <td>Y</td>
    <td>Y<br /> (solo vista previa)</td>
    <td>N</td>
  </tr>
  <tr>
    <td><b>/XO</b></td>
    <td>Excluye un recurso de origen más antiguo. El recurso no se copiará si el recurso de origen es más antiguo que el destino.</td>
    <td>Y</td>
    <td>Y<br /> (solo vista previa)</td>
    <td>N</td>
  </tr>
  <tr>
    <td><b>/A</b></td>
    <td>Carga solamente archivos que tienen el atributo de archivado establecido.</td>
    <td>Y</td>
    <td>Y<br /> (solo vista previa)</td>
    <td>N</td>
  </tr>
  <tr>
    <td><b>/IA:[RASHCNETOI]</b></td>
    <td>Carga solamente archivos que tienen cualquiera de los atributos especificados establecido.<br />
        Los atributos disponibles son los siguientes:  
        <br />
        R&#160;&#160;&#160;Archivos de solo lectura
        <br />
        A&#160;&#160;&#160;Archivos listos para archivarse
        <br />
        S&#160;&#160;&#160;Archivos del sistema
        <br />
        H&#160;&#160;&#160;Archivos ocultos
        <br />
        C&#160;&#160;&#160;Archivos comprimidos
        <br />
        N&#160;&#160;&#160;Archivos normales
        <br />
        E&#160;&#160;&#160;Archivos cifrados
        <br />
        T&#160;&#160;&#160;Archivos temporales
        <br />
        O&#160;&#160;&#160;Archivos fuera de línea
        <br />
        I&#160;&#160;&#160;Archivos no indizados</td>
    <td>Y</td>
    <td>Y<br /> (solo vista previa)</td>
    <td>N</td>
  </tr>
  <tr>
    <td><b>/XA:[RASHCNETOI]</b></td>
    <td>Excluye archivos que tienen cualquiera de los atributos especificados establecido.<br />
        Los atributos disponibles son los siguientes:  
        <br />
        R&#160;&#160;&#160;Archivos de solo lectura  
        <br />
        A&#160;&#160;&#160;Archivos listos para archivarse  
        <br />
        S&#160;&#160;&#160;Archivos del sistema  
        <br />
        H&#160;&#160;&#160;Archivos ocultos  
        <br />
        C&#160;&#160;&#160;Archivos comprimidos  
        <br />
        N&#160;&#160;&#160;Archivos normales  
        <br />
        E&#160;&#160;&#160;Archivos cifrados  
        <br />
        T&#160;&#160;&#160;Archivos temporales  
        <br />
        O&#160;&#160;&#160;Archivos fuera de línea  
        <br />
        I&#160;&#160;&#160;Archivos no indizados</td>
    <td>Y</td>
    <td>Y<br /> (solo vista previa)</td>
    <td>N</td>
  </tr>
  <tr>
    <td><b>/Delimiter:&lt;delimitador></b></td>
    <td>Indica el carácter delimitador usado para delimitar directorios virtuales en un nombre de blob.<br />
        De forma predeterminada, AzCopy usa / como carácter delimitador. Sin embargo, AzCopy admite el uso de cualquier carácter convencional (como @, # o %) como delimitador. Si necesita incluir uno de estos caracteres especiales en la línea de comandos, encierre el nombre de archivo entre comillas dobles. 
        <br />
        Esta opción solamente se aplica para descarga de blobs.</td>
    <td>Y</td>
    <td>N</td>
    <td>N</td>
  </tr>
  <tr>
    <td><b>/NC:&lt;número-de-simultáneos></b></td>
    <td>Especifica el número de operaciones simultáneas.
        <br />
        AzCopy inicia de manera predeterminada un número determinado de operaciones simultáneas para aumentar el rendimiento de la transferencia de datos. Tenga en cuenta que un número elevado de operaciones simultáneas en un entorno con poco ancho de banda puede saturar la conexión de red e impedir que las operaciones se completen. Reduzca el número de operaciones simultáneas basándose en el ancho de banda de red disponible real.
        <br />
		El límite máximo de operaciones simultáneas es de 512.</td>
    <td>Y</td>
    <td>Y<br /> (solo vista previa)</td>
    <td>Y<br /> (solo vista previa)</td>
  </tr>
  <tr>
    <td><b>/SourceType:Blob|Table</b></td>
    <td>Especifica que el recurso <code>source</code> es un blob disponible en el entorno de desarrollo local, que se ejecuta en el emulador de almacenamiento.</td>
    <td>Y</td>
    <td>N</td>
    <td>Y<br /> (solo vista previa)</td>
  </tr>
  <tr>
    <td><b>/DestType:Blob|Table</b></td>
    <td>Especifica que el recurso <code>destination</code> es un blob disponible en el entorno de desarrollo local, que se ejecuta en el emulador de almacenamiento.</td>
    <td>Y</td>
    <td>N</td>
    <td>Y<br /> (solo vista previa)</td>
  </tr>
  <tr>
    <td><strong>/PKRS:&lt;"key1#key2#key3#..."></strong></td>
    <td>Divide el rango de claves de partición para habilitar la exportación de datos de tablas en paralelo, lo que aumenta la velocidad de la operación de exportación.
        <br />
        Si esta opción no se especifica, AzCopy utiliza un solo subproceso para exportar las entidades de tabla. Por ejemplo, si el usuario especifica /PKRS:"aa#bb", AzCopy inicia tres operaciones simultáneas.
        <br />
        Cada operación exporta uno de los tres rangos de claves de partición, como se muestra a continuación: 
        <br />
        &#160;&#160;&#160;[&lt;primera clave de partición>, aa) 
        <br />
        &#160;&#160;&#160;[aa, bb)
        <br />
        &#160;&#160;&#160;[bb, &lt;última clave de partición>] </td>
    <td>N</td>
    <td>N</td>
    <td>Y<br /> (solo vista previa)</td>
  </tr>
  <tr>
    <td><strong>/SplitSize:</strong><file-size><strong>&lt;tamaño-archivo></strong></td>
    <td>Especifica el tamaño dividido del archivo exportado en MB; el valor mínimo permitido es 32.
        <br />
        Si esta opción no se especifica, AzCopy exportará los datos de la tabla a un único archivo.
        <br />
        Si los datos de la tabla se exportan a un blob, y el tamaño del archivo exportado alcanza el límite de 200 GB establecido para el tamaño de un blob, AzCopy dividirá el archivo exportado, incluso si no se ha especificado esta opción. </td>
    <td>N</td>
    <td>N</td>
    <td>Y<br /> (solo vista previa)</td>
  </tr>
  <tr>
    <td><b>/EntityOperation:&lt;InsertOrSkip | InsertOrMerge | InsertOrReplace> </b>
</td>
    <td>Especifica el comportamiento de la importación de los datos de la tabla.
        <br />
        InsertOrSkip: omite una entidad existente o inserta una nueva entidad si esta no existe en la tabla.
        <br />
        InsertOrMerge: combina una entidad existente o inserta una nueva entidad si esta no existe en la tabla.
        <br />
        InsertOrReplace: reemplaza una entidad existente o inserta una nueva entidad si esta no existe en la tabla. </td>
    <td>N</td>
    <td>N</td>
    <td>Y<br /> (solo vista previa)</td>
  </tr>
  <tr>
    <td><b>/Manifest:&lt;archivo-manifiesto></b></td>
    <td>Especifica el archivo de manifiesto para la operación de exportación e importación de tablas. <br />
    Esta opción es opcional durante la operación de exportación, AzCopy generará un archivo de manifiesto con nombre predefinido si no se especifica esta opción.
    <br />
    Esta opción es necesaria durante la operación de importación para localizar los archivos de datos.</td>
    <td>N</td>
    <td>N</td>
    <td>Y<br /> (solo vista previa)</td>
  </tr>
  <tr>
    <td><b>/SyncCopy</b></td>
    <td>Indica si se van a copiar archivos o blobs entre dos extremos de Almacenamiento de Azure de forma sincrónica. <br />
		De forma predeterminada, AzCopy usa la copia asincrónica del servidor. Especifique esta opción para hacer una copia sincrónica, que se descarga blobs o archivos en la memoria local y, a continuación, los carga en el Almacenamiento de Azure. Puede usar esta opción al copiar archivos dentro del almacenamiento de blobs o el almacenamiento de archivos o del almacenamiento de blobs al almacenamiento de archivos o viceversa.</td>
    <td>Y</td>
    <td>Y<br /> (solo vista previa)</td>
    <td>N</td>
  </tr>
  <tr>
    <td><b>/SetContentType:&lt;tipo-contenido></b></td>
    <td>Especifica el tipo de contenido MIME de los blobs o archivos de destino. <br />
		AzCopy establece el tipo de contenido de un blob o archivo en <code>application/octet-stream</code> de forma predeterminada. Puede establecer el tipo de contenido para todos los blobs o archivos especificando explícitamente un valor para esta opción. Si especifica esta opción sin un valor, AzCopy establecerá cada tipo de contenido de blob o archivo según su extensión de archivo.</td>
    <td>Y</td>
    <td>Y<br /> (solo vista previa)</td>
    <td>N</td>
  </tr>
    <tr>
    <td><b>/PayloadFormat:&lt;JSON | CSV></b></td>
    <td>Especifica el formato del archivo de datos exportados de tabla.<br />
    Si no se especifica esta opción, de forma predeterminada, AzCopy exporta el archivo de datos de tabla en formato JSON.</td>
    <td>N</td>
    <td>N</td>
    <td>Y<br /> (solo vista previa)</td>
  </tr>
</table>
<br/>

## Limitación de escrituras concurrentes mientras se copian datos

Cuando copie blobs o archivos con AzCopy, recuerde que otra aplicación puede estar modificando los datos mientras se copian. Si es posible, asegúrese de que los datos no se modifican durante la operación de copia. Por ejemplo, cuando copie un VHD asociado con una máquina virtual de Azure, asegúrese de que ninguna otra aplicación está escribiendo actualmente en el VHD. Alternativamente, puede crear una instantánea del VHD primero y, después, copiar la instantánea.

Si no puede impedir que otras aplicaciones escriban en blobs o archivos mientras se copian, recuerde que para cuando el trabajo finalice, los recursos copiados puede que ya no tengan una paridad total con los recursos de origen.

## Copia de blobs de Azure con AzCopy

Los ejemplos siguientes muestran diversos escenarios para copiar blobs con AzCopy.

### Copia de un solo blob

**Cargar un archivo desde el sistema de archivos a Almacenamiento de blobs:**
	
	AzCopy /Source:C:\myfolder /Dest:https://myaccount.blob.core.windows.net/mycontainer /DestKey:key /Pattern:abc.txt

**Descargar un blob desde Almacenamiento de blobs en el sistema de archivos:**

	AzCopy /Source:https://myaccount.blob.core.windows.net/mycontainer /Dest:C:\myfolder /SourceKey:key /Pattern:abc.txt

Para obtener más información sobre cómo trabajar con las claves de acceso de almacenamiento, consulte [Vista, copia y regeneración de las claves de acceso de almacenamiento](../storage-create-storage-account/#regeneratestoragekeys).

### Copia de un blob a través de la copia del lado del servidor

Cuando copia un blob dentro de una cuenta de almacenamiento o a través de cuentas de almacenamiento, se realiza una operación de copia del lado del servidor. Para obtener más información acerca de las operaciones de copia del lado del servidor, consulte [Introducción a la copia asincrónica de blobs entre cuentas](http://blogs.msdn.com/b/windowsazurestorage/archive/2012/06/12/introducing-asynchronous-cross-account-copy-blob.aspx).

**Copia de un blob dentro de una cuenta de almacenamiento:**

	AzCopy /Source:https://myaccount.blob.core.windows.net/mycontainer1 /Dest:https://myaccount.blob.core.windows.net/mycontainer2 /SourceKey:key /DestKey:key /Pattern:abc.txt 

**Copia de un blob entre cuentas de almacenamiento:**

	AzCopy /Source:https://sourceaccount.blob.core.windows.net/mycontainer1 /Dest:https://destaccount.blob.core.windows.net/mycontainer2 /SourceKey:key1 /DestKey:key2 /Pattern:abc.txt
 
### Copia de un blob desde la región secundaria 

Si la cuenta de almacenamiento tiene el almacenamiento con redundancia geográfica de acceso de lectura habilitado, puede copiar datos desde la región secundaria.

**Copia de un blob en la cuenta principal desde la secundaria:**

	AzCopy /Source:https://myaccount1-secondary.blob.core.windows.net/mynewcontainer1 /Dest:https://myaccount2.blob.core.windows.net/mynewcontainer2 /SourceKey:key1 /DestKey:key2 /Pattern:abc.txt

**Descarga de un blob de la secundaria en un archivo del sistema de archivos:**

	AzCopy /Source:https://myaccount-secondary.blob.core.windows.net/mynewcontainer /Dest:C:\myfolder /SourceKey:key /Pattern:abc.txt

### Carga de un archivo en un nuevo contenedor de blobs o directorio virtual

**Carga de un archivo en un nuevo contenedor de blobs**

	AzCopy /Source:C:\myfolder /Dest:https://myaccount.blob.core.windows.net/mynewcontainer /DestKey:key /Pattern:abc.txt

Tenga en cuenta que si el contenedor de destino especificado no existe, AzCopy lo creará y cargará el archivo en él.

**Carga de un archivo en un nuevo directorio virtual de blobs**

	AzCopy /Source:C:\myfolder /Dest:https://myaccount.blob.core.windows.net/mycontainer/vd /DestKey:key /Pattern:abc.txt

Tenga en cuenta que, si el directorio virtual especificado no existe, AzCopy cargará el archivo para incluir el directorio virtual en su nombre (*p.ej.*, `vd/abc.txt` en el ejempo anterior).

### Descarga de un blob en una nueva carpeta

	AzCopy /Source:https://myaccount.blob.core.windows.net/mycontainer /Dest:C:\myfolder /SourceKey:key /Pattern:abc.txt

Si la carpeta `C:\myfolder` todavía no existe, AzCopy la creará en el sistema de archivos y descargará `abc.txt ` en la nueva carpeta.

### Carga de archivos y subcarpetas de un directorio en un contenedor de forma recursiva

	AzCopy /Source:C:\myfolder /Dest:https://myaccount.blob.core.windows.net/mycontainer /DestKey:key /S

Al especificar la opción `/S`, se copiará el contenido del directorio especificado en Almacenamiento de blobs de forma recursiva, lo que significa que todas las subcarpetas y sus archivos también se copiarán. Por ejemplo, supongamos que los siguientes archivos residen en la carpeta `C:\myfolder`:

	C:\myfolder\abc.txt
	C:\myfolder\abc1.txt
	C:\myfolder\abc2.txt
	C:\myfolder\subfolder\a.txt
	C:\myfolder\subfolder\abcd.txt

Después de la operación de copia, el contenedor incluirá los siguientes archivos:

    abc.txt
    abc1.txt
    abc2.txt
    subfolder\a.txt
    subfolder\abcd.txt

### Carga de archivos desde un directorio a un contenedor de forma no recursiva

	AzCopy /Source:C:\myfolder /Dest:https://myaccount.blob.core.windows.net/mycontainer /DestKey:key

Si no especifica la opción `/S` en la línea de comandos, AzCopy no realizará la copia de forma recursiva. Solamente se copiarán los archivos del directorio especificado; NO se copiará ninguna subcarpeta ni sus archivos. Por ejemplo, supongamos que los siguientes archivos residen en la carpeta `C:\myfolder`:

	C:\myfolder\abc.txt
	C:\myfolder\abc1.txt
	C:\myfolder\abc2.txt
	C:\myfolder\subfolder\a.txt
	C:\myfolder\subfolder\abcd.txt

Después de la operación de copia, el contenedor incluirá los siguientes archivos:

	abc.txt
	abc1.txt
	abc2.txt

### Descarga de todos los blobs de un contenedor a un directorio del sistema de archivos de forma recursiva

	AzCopy /Source:https://myaccount.blob.core.windows.net/mycontainer /Dest:C:\myfolder /SourceKey:key /S

Supongamos que los siguientes blobs residen en el contenedor especificado:

	abc.txt
	abc1.txt
	abc2.txt
	vd1\a.txt
	vd1\abcd.txt

Después de la operación de copia, el directorio `C:\myfolder` incluirá los siguientes archivos:

	C:\myfolder\abc.txt
	C:\myfolder\abc1.txt
	C:\myfolder\abc2.txt
	C:\myfolder\vd1\a.txt
	C:\myfolder\vd1\abcd.txt

### Descarga de blobs de un directorio de blobs virtual a un directorio del sistema de archivos de forma recursiva

	AzCopy /Source:https://myaccount.blob.core.windows.net/mycontainer/vd1/ /Dest:C:\myfolder /SourceKey:key /S

Supongamos que los siguientes blobs residen en el contenedor especificado:

	abc.txt
	abc1.txt
	abc2.txt
	vd1\a.txt
	vd1\abcd.txt

Después de la operación de copia, el directorio `C:\myfolder` incluirá los siguientes archivos. Tenga en cuenta que solamente se copiarán los blobs del directorio virtual:

	C:\myfolder\a.txt
	C:\myfolder\abcd.txt

### Carga de archivos que coincidan con el patrón de archivos especificado en un contenedor de forma recursiva 

	AzCopy /Source:C:\myfolder /Dest:https://myaccount.blob.core.windows.net/mycontainer /DestKey:key /Pattern:a* /S

Supongamos que los siguientes archivos residen en la carpeta `C:\myfolder`:

	C:\myfolder\abc.txt
	C:\myfolder\abc1.txt
	C:\myfolder\abc2.txt
	C:\myfolder\xyz.txt
	C:\myfolder\subfolder\a.txt
	C:\myfolder\subfolder\abcd.txt

Después de la operación de copia, el contenedor incluirá los siguientes archivos:

	abc.txt
	abc1.txt
	abc2.txt
	subfolder\a.txt
	subfolder\abcd.txt
	
### Descarga de blobs con el prefijo especificado al sistema de archivos de forma recursiva

	AzCopy /Source:https://myaccount.blob.core.windows.net/mycontainer /Dest:C:\myfolder /SourceKey:key /Pattern:a /S

Supongamos que los siguientes blobs residen en el contenedor especificado. Se copiarán todos los blobs que comiencen con el prefijo `a`:

	abc.txt
	abc1.txt
	abc2.txt
	xyz.txt
	vd1\a.txt
	vd1\abcd.txt

Después de la operación de copia, la carpeta `C:\myfolder` incluirá los siguientes archivos:

	C:\myfolder\abc.txt
	C:\myfolder\abc1.txt
	C:\myfolder\abc2.txt

Tenga en cuenta que el prefijo se aplica al directorio virtual, que forma la primera parte del nombre del blob. En el ejemplo mostrado anteriormente, el directorio virtual no coincide con el prefijo especificado, por lo que no se copia.


### Copia de un blob y sus instantáneas en otra cuenta de almacenamiento

	AzCopy /Source:https://sourceaccount.blob.core.windows.net/mycontainer1 /Dest:https://destaccount.blob.core.windows.net/mycontainer2 /SourceKey:key1 /DestKey:key2 /Pattern:abc.txt /Snapshot

Después de la operación de copia, el contenedor de destino incluirá el blob y sus instantáneas. Suponiendo que el blob del ejemplo anterior tiene dos instantáneas, el contenedor incluirá el siguiente blob e instantáneas:

	abc.txt
	abc (2013-02-25 080757).txt
	abc (2014-02-21 150331).txt


### Uso de un archivo de respuesta para especificar parámetros de línea de comandos

	AzCopy /@:"C:\myfolder\abc.txt"

Puede incluir cualquier parámetro de la línea de comandos de AzCopy en un archivo de respuesta. AzCopy procesa los parámetros del archivo como si hubieran sido especificados en la línea de comandos, realizando una sustitución directa con el contenido del archivo.

**Especificación de uno o varios archivos de respuesta de una sola línea**

Imaginemos un archivo de respuesta llamado `source.txt` que especifica un contenedor de origen:

	/Source:http://myaccount.blob.core.windows.net/mycontainer

Y un archivo de respuesta llamado `dest.txt` que especifica una carpeta de destino en el sistema de archivos:

	/Dest:C:\myfolder

Y un archivo de respuesta llamado `options.txt` que especifica opciones para AzCopy:

	/S /Y

Para llamar a AzCopy con estos archivos de respuesta, todos los cuales residen en un directorio `C:\responsefiles`, use este comando:

	AzCopy /@:"C:\responsefiles\source.txt" /@:"C:\responsefiles\dest.txt" /SourceKey:<sourcekey> /@:"C:\responsefiles\options.txt"   

AzCopy procesa este comando como si se incluyeran todos los parámetros individuales en la línea de comandos:

	AzCopy /Source:http://myaccount.blob.core.windows.net/mycontainer /Dest:C:\myfolder /SourceKey:<sourcekey> /S /Y

**Especificación de un archivo de respuesta de varias líneas**

Imaginemos un archivo de respuesta llamado `copyoperation.txt`, que contiene las siguientes líneas. Cada parámetro de AzCopy se especifica en su propia línea:

	/Source:http://myaccount.blob.core.windows.net/mycontainer
	/Dest:C:\myfolder
	/SourceKey:<sourcekey>
	/S 
	/Y

Para llamar a AzCopy con este archivo de respuesta, use este comando:

	AzCopy /@:"C:\responsefiles\copyoperation.txt"

AzCopy procesa este comando como si se incluyeran todos los parámetros individuales en la línea de comandos:

	AzCopy /Source:http://myaccount.blob.core.windows.net/mycontainer /Dest:C:\myfolder /SourceKey:<sourcekey> /S /Y

Tenga en cuenta que cada parámetro de AzCopy se debe especificar completamente en una línea. Por ejemplo, AzCopy no se procesará si divide el parámetro en dos líneas, tal y como se muestra aquí para el parámetro `/sourcekey`:

	http://myaccount.blob.core.windows.net/mycontainer
 	C:\myfolder
	/sourcekey:
	[sourcekey]
	/S 
	/Y

### Especificación de una firma de acceso compartido (SAS)
	
**Especificación de una SAS para el contenedor de origen usando la opción /sourceSAS**

	AzCopy /Source:https://myaccount.blob.core.windows.net/mycontainer1 /DestC:\myfolder /SourceSAS:SAS /S

**Especificación de una SAS para el contenedor de origen en el URI de dicho contenedor**

	AzCopy /Source:https://myaccount.blob.core.windows.net/mycontainer1/?SourceSASToken /Dest:C:\myfolder /S

**Especificación de una SAS para el contenedor de destino usando la opción /destSAS**

	AzCopy /Source:C:\myfolder /Dest:https://myaccount.blob.core.windows.net/mycontainer1 /DestSAS:SAS /Pattern:abc.txt

**Especificación de una SAS para los contenedores de origen y destino**

	AzCopy /Source:https://myaccount.blob.core.windows.net/mycontainer1 /Dest:https://myaccount.blob.core.windows.net/mycontainer2 /SourceSAS:SAS1 /DestSAS:SAS2 /Pattern:abc.txt

### Especificación de una carpeta de archivo de diario

Cada vez que emite un comando a AzCopy, comprueba si existe un archivo de diario en la carpeta predeterminada o si existe una carpeta que especificó a través de esta opción. Si el archivo de diario no existe en ningún lugar, AzCopy trata la operación como una nueva y genera un nuevo archivo de diario.

Si el archivo de diario existe, AzCopy comprobará si la línea de comandos escrita coincide con la de dicho archivo. Si las dos líneas de comandos coinciden, AzCopy reanudará la operación incompleta. Si no coinciden, se le preguntará si desea sobrescribir el archivo de diario, iniciar una nueva operación o cancelar la operación actual.

**Uso de la ubicación predeterminada para el archivo de diario**

	AzCopy /Source:C:\myfolder /Dest:https://myaccount.blob.core.windows.net/mycontainer /DestKey:key /Z

Si omite la opción `/Z` o especifica la opción `/Z` sin la ruta de acceso a la carpeta, tal y como se muestra anteriormente, AzCopy crea el archivo de diario en la ubicación predeterminada, que es `%SystemDrive%\Users\%username%\AppData\Local\Microsoft\Azure\AzCopy`. Si el archivo de diario ya existe, AzCopy reanuda la operación basándose en dicho archivo.

**Especificación de una ubicación personalizada para el archivo de diario**

	AzCopy /Source:C:\myfolder /Dest:https://myaccount.blob.core.windows.net/mycontainer /DestKey:key /Z:C:\journalfolder\

Este ejemplo crea el archivo de diario si todavía no existe. Si no existe, AzCopy reanuda la operación basándose en el archivo de diario.

**Reanudación de una operación de AzCopy**

	AzCopy /Z:C:\journalfolder\

Este ejemplo reanuda la última operación que es posible que no terminara.


### Generación de un archivo de registro

**Escritura del archivo de registro detallado en la ubicación predeterminada**

	AzCopy /Source:C:\myfolder /Dest:https://myaccount.blob.core.windows.net/mycontainer /DestKey:key /V

Si especifica la opción `/V` sin proporcionar una ruta de acceso de archivo al registro detallado, AzCopy crea el archivo de registro en la ubicación predeterminada, que es `%SystemDrive%\Users\%username%\AppData\Local\Microsoft\Azure\AzCopy`.

**Escritura del archivo de registro detallado en una ubicación predeterminada**

	AzCopy /Source:C:\myfolder /Dest:https://myaccount.blob.core.windows.net/mycontainer /DestKey:key /V:C:\myfolder\azcopy1.log

Tenga en cuenta que si especifica una ruta de acceso relativa después de la opción `/V`, como por ejemplo `/V:test/azcopy1.log`, el registro detallado se creará en el directorio de trabajo actual dentro de una subcarpeta llamada `test`.


### Establecimiento de la hora de la última modificación de los archivos descargados para que coincida con la de los blobs de origen

	AzCopy /Source:https://myaccount.blob.core.windows.net/mycontainer /Dest:C:\myfolder /SourceKey:key /MT

### Exclusión de los blobs de la operación de copia basándose en la hora de su última modificación

Especifique la opción `/MT` para comparar la hora de la última modificación del blob de origen y el archivo de destino.

**Exclusión de blobs que son más nuevos que el archivo de destino**

	AzCopy /Source:https://myaccount.blob.core.windows.net/mycontainer /Dest:C:\myfolder /SourceKey:key /MT /XN

**Exclusión de blobs que son más antiguos que el archivo de destino**

	AzCopy /Source:https://myaccount.blob.core.windows.net/mycontainer /Dest:C:\myfolder /SourceKey:key /MT /XO

### Especificación del número de operaciones simultáneas para iniciar

La opción `/NC` especifica el número de operaciones de copia simultáneas. De forma predeterminada, AzCopy iniciará operaciones simultáneas a ocho veces el número de procesadores de núcleo que tiene. Si ejecuta AzCopy en una red con un ancho de banda bajo, puede especificar un número bajo para esta opción para evitar errores provocados por la competencia de recursos.


### 	Ejecución de AzCopy en recursos de blob en el emulador de almacenamiento

	AzCopy /Source:https://127.0.0.1:10004/myaccount/myfileshare/ /Dest:C:\myfolder /SourceKey:key /SourceType:Blob /S

### Copia sincrónica de blobs entre dos extremos de Almacenamiento de Azure

De forma predeterminada, AzCopy copia datos entre dos extremos de almacenamiento asincrónicamente. Por lo tanto, la operación de copia se ejecutará en segundo plano a través de la capacidad de ancho de banda de reserva sin SLA en términos de velocidad con que se copiará un blob y AzCopy comprobará periódicamente el estado de la copia hasta que se complete o devuelva un error.

La opción `/SyncCopy` garantiza que la operación de copia tenga una velocidad coherente. Para hacer la copia sincrónica, AzCopy descarga los blobs que se deben copiar desde el origen especificado a la memoria local y, a continuación, los carga en el destino de almacenamiento de blobs.

	AzCopy /Source:https://myaccount1.blob.core.windows.net/myContainer/ /Dest:https://myaccount2.blob.core.windows.net/myContainer/ /SourceKey:key1 /DestKey:key2 /Pattern:ab /SyncCopy

Tenga en cuenta que `/SyncCopy` podría generar un coste de salida adicional, en comparación con la copia asincrónica. Es recomendable usar esta opción en la máquina virtual de Azure, que se encuentra en la misma región que la cuenta de almacenamiento de origen, para evitar el costo de salida.

### Especificación del tipo de contenido MIME de un blob de destino

De forma predeterminada, AzCopy define el tipo de contenido de un blob de destino como `application/octet-stream`. A partir de la versión 3.1.0, puede especificar explícitamente el tipo de contenido a través de la opción `/SetContentType:[content-type]`. Esta sintaxis establece el tipo de contenido para todos los blobs de una operación de copia.

	AzCopy /Source:C:\myfolder\ /Dest:https://myaccount.blob.core.windows.net/myContainer/ /DestKey:key /Pattern:ab /SetContentType:video/mp4

Si especifica `/SetContentType` sin un valor, AzCopy establecerá cada blob o tipo de contenido del archivo según su extensión de archivo.

	AzCopy /Source:C:\myfolder\ /Dest:https://myaccount.blob.core.windows.net/myContainer/ /DestKey:key /Pattern:ab /SetContentType

## Copia de archivos en el almacenamiento de archivos de Azure con AzCopy (solo versión de vista previa)

Los ejemplos siguientes muestran diversos escenarios para copiar archivos de Azure con AzCopy.

### Descarga de un archivo desde un recurso compartido de archivos de Azure en el sistema de archivos

	AzCopy /Source:https://myaccount.file.core.windows.net/myfileshare/myfolder1/ /Dest:C:\myfolder /SourceKey:key /Pattern:abc.txt

Tenga en cuenta que si el origen especificado es un recurso compartido de archivos de Azure, entonces debe especificar el nombre de archivo exacto (*p. ej.,*`abc.txt`) para copiar un solo archivo, o especificar la opción `/S` para copiar todos los archivos en el recurso compartido de forma recursiva. Si intenta especificar tanto un patrón de archivos como la opción `/S` conjuntamente, se producirá un error.

### Descargar archivos y carpetas de un recurso compartido de archivos de Azure en el sistema de archivos de forma recursiva, especificar la firma de acceso de recurso compartido

	AzCopy /Source:https://myaccount.file.core.windows.net/myfileshare/ /Dest:C:\myfolder /SourceSAS:SAS /S

Tenga en cuenta que ninguna carpeta vacía se copiará.


### Carga de archivos y carpetas desde el sistema de archivos en un recurso compartido de archivos de Azure

	AzCopy /Source:C:\myfolder /Dest:https://myaccount.file.core.windows.net/myfileshare/ /DestKey:key /S

Tenga en cuenta que ninguna carpeta vacía se copiará.


### Carga de archivos que coincidan con el patrón de archivos especificado en un recurso compartido de Azure de forma recursiva

	AzCopy /Source:C:\myfolder /Dest:https://myaccount.file.core.windows.net/myfileshare/ /DestKey:key /Pattern:ab* /S

### Copia sincrónica de archivos en el almacenamiento de archivos de Azure

Almacenamiento de archivos de Azure es compatible con copia asincrónica del lado servidor.

Copia asincrónica de Almacenamiento de archivos a Almacenamiento de archivos:

	AzCopy /Source:https://myaccount1.file.core.windows.net/myfileshare1/ /Dest:https://myaccount2.file.core.windows.net/myfileshare2/ /SourceKey:key1 /DestKey:key2 /S

Copia asincrónica de Almacenamiento de archivos a Blob de bloques:
  
	AzCopy /Source:https://myaccount1.file.core.windows.net/myfileshare/ /Dest:https://myaccount2.blob.core.windows.net/mycontainer/ /SourceKey:key1 /DestKey:key2 /S

Copia asincrónica de Almacenamiento de blob de bloques/páginas a Almacenamiento de archivos:

	AzCopy /Source:https://myaccount1.blob.core.windows.net/mycontainer/ /Dest:https://myaccount2.file.core.windows.net/myfileshare/ /SourceKey:key1 /DestKey:key2 /S

Tenga en cuenta que no se admite la copia asincrónica desde Almacenamiento de archivos a Blob de páginas.

### Copia sincrónica de archivos en el almacenamiento de archivos de Azure

Además de la copia asincrónica, usuario también puede especificar opción `/SyncCopy` para copiar datos de Almacenamiento de archivos a Almacenamiento de archivos, de Almacenamiento de archivos al Almacenamiento de blobs y del Almacenamiento de blobs para Almacenamiento de archivos de forma sincrónica, para ello, AzCopy descarga los datos de origen en la memoria local y cargarlo de nuevo al destino.

	AzCopy /Source:https://myaccount1.file.core.windows.net/myfileshare1/ /Dest:https://myaccount2.file.core.windows.net/myfileshare2/ /SourceKey:key1 /DestKey:key2 /S /SyncCopy

	AzCopy /Source:https://myaccount1.file.core.windows.net/myfileshare/ /Dest:https://myaccount2.blob.core.windows.net/mycontainer/ /SourceKey:key1 /DestKey:key2 /S /SyncCopy
	
	AzCopy /Source:https://myaccount1.blob.core.windows.net/mycontainer/ /Dest:https://myaccount2.file.core.windows.net/myfileshare/ /SourceKey:key1 /DestKey:key2 /S /SyncCopy

Al copiar del Almacenamiento de archivos al Almacenamiento de blobs, el tipo de blob predeterminado es el blob de bloques, el usuario puede especificar la opción `/BlobType:page` para cambiar el tipo de blob de destino.

Tenga en cuenta que `/SyncCopy` podría generar un coste de salida adicional, en comparación con la copia asincrónica. Es recomendable usar esta opción en la máquina virtual de Azure, que se encuentra en la misma región que la cuenta de almacenamiento de origen, para evitar el costo de salida.


## Copia de entidades en una Tabla de Azure con AzCopy (solo versión de vista previa)

Los ejemplos siguientes muestran diversos escenarios para copiar entidades de Tabla de Azure con AzCopy.

### Exportar entidades al sistema de archivos local

	AzCopy /Source:https://myaccount.table.core.windows.net/myTable/ /Dest:C:\myfolder\ /SourceKey:key

AzCopy escribe un archivo de manifiesto para la carpeta o contenedor de blobs de destino que se haya especificado. El archivo de manifiesto se utiliza por parte del proceso de importación para ubicar los archivos de datos necesarios y realizar la validación de datos durante el proceso de importación. El archivo de manifiesto utiliza la siguiente convención de nombres de forma predeterminada:

	<account name>_<table name>_<timestamp>.manifest

El usuario también puede especificar la opción `/Manifest:<manifest file name>` para establecer el nombre de archivo de manifiesto.

	AzCopy /Source:https://myaccount.table.core.windows.net/myTable/ /Dest:C:\myfolder\ /SourceKey:key /Manifest:abc.manifest


### Entidades de exportación al formato de archivo de datos JSON y CSV

AzCopy de forma predeterminada exporta las entidades de tabla para archivos de JSON, el usuario puede especificar la opción `/PayloadFormat:JSON|CSV` para decidir el tipo de archivo de datos exportados.

	AzCopy /Source:https://myaccount.table.core.windows.net/myTable/ /Dest:C:\myfolder\ /SourceKey:key /PayloadFormat:CSV

Cuando se especifica el formato de carga CSV, además de los datos de archivos con extensión `.csv` que se encuentra en el lugar especificado por el parámetro `/Dest`, AzCopy generará el archivo de esquema con la extensión `.schema.csv` para cada archivo de datos. Tenga en cuenta que AzCopy no incluye la compatibilidad para la "importación" del archivo de datos CSV, puede utilizar el formato JSON para exportar e importar datos de la tabla.

### Exportar entidades a un blob de Azure

	AzCopy /Source:https://myaccount.table.core.windows.net/myTable/ /Dest:https://myaccount.blob.core.windows.net/mycontainer/ /SourceKey:key1 /Destkey:key2

AzCopy generará un archivo de datos JSON en la carpeta o el contenedor de blobs local con la siguiente convención de nomenclatura:

	<account name>_<table name>_<timestamp>_<volume index>_<CRC>.json

El archivo de datos JSON generado sigue el formato de carga para metadatos mínimos. Para obtener más información sobre el formato de carga, consulte [Formato de carga para las operaciones del servicio Tabla](http://msdn.microsoft.com/library/azure/dn535600.aspx).

Tenga en cuenta que, al exportar las entidades de tabla de almacenamiento a Almacenamiento ode BLOB, AzCopy exportará las entidades de tabla a archivos de datos temporales locales en primer lugar y, después, los cargará en Blob; estos archivos de datos temporales se colocan en la carpeta de archivos de diario con la ruta de acceso predeterminada "<code>%LocalAppData%\\Microsoft\\Azure\\AzCopy</code>". Puede especificar la opción /Z:[carpeta-de-archivos-de-diario] para cambiar la ubicación de la carpeta de archivos de diario y así cambiar la ubicación de los archivos de datos temporales. El tamaño de los archivos de datos temporales se decide según el tamaño de las entidades de tabla y el tamaño especificado con la opción /SplitSize, aunque el archivo de datos temporales en el disco local se eliminará inmediatamente después de que se cargue en el Blob. Asegúrese de que tiene suficiente espacio en el disco local para almacenar estos archivos de datos temporales antes de que se eliminen.

### Dividir los archivos exportados

	AzCopy /Source:https://myaccount.table.core.windows.net/mytable/ /Dest:C:\myfolder /SourceKey:key /S /SplitSize:100

AzCopy usa un *índice de volumen* en los nombres de los archivos de datos divididos para distinguir los diversos archivos. El índice de volumen consta de dos partes, un *índice de rango de clave de partición* y un *índice de archivo dividido*. Ambos índices se basan en 0.

El rango de clave de partición será 0 si un usuario no especifica la opción `/PKRS` (que se presenta en la siguiente sección).

Por ejemplo, imagine que AzCopy genera dos archivos de datos después de que el usuario especifique la opción `/SplitSize`. Los nombres de archivos de datos resultantes serían:

	myaccount_mytable_20140903T051850.8128447Z_0_0_C3040FE8.json
	myaccount_mytable_20140903T051850.8128447Z_0_1_0AB9AC20.json

Tenga en cuenta que el mínimo valor posible para la opción `/SplitSize` es 32 MB. Si el destino especificado es Almacenamiento de blobs, AzCopy dividirá el archivo de datos una vez que su tamaño alcance el límite especificado (200 GB), independientemente de si el usuario ha especificado o no la opción `/SplitSize`.

### Exportar entidades simultáneamente

	AzCopy /Source:https://myaccount.table.core.windows.net/myTable/ /Dest:C:\myfolder\ /SourceKey:key /PKRS:"aa#bb"

AzCopy iniciará operaciones simultáneas para exportar entidades cuando el usuario especifique la opción `/PKRS`. Cada operación exporta un rango de claves de partición.

Tenga en cuenta que el número de operaciones simultáneas está controlado también por la opción `/NC`. AzCopy usa el número de procesadores de núcleo como valor predeterminado de `/NC` al copiar entidades de tabla, incluso si no se especificó `/NC`. Si el usuario especifica la opción `/PKRS`, AzCopy usa el valor más pequeño de los dos (rangos de claves de partición u operaciones simultáneas especificadas de manera implícita o explícita) para determinar el número de operaciones simultáneas que van a dar inicio. Para obtener más detalles, escriba `AzCopy /?:NC` en la línea de comandos.

### Importar entidades simultáneamente

	AzCopy /Source:C:\myfolder\ /Dest:https://myaccount.table.core.windows.net/mytable1/ /DestKey:key /Manifest:"myaccount_mytable_20140103T112020.manifest" /EntityOperation:InsertOrReplace 

La opción `/EntityOperation` indica cómo insertar entidades en la tabla. Los valores posibles son:

- `InsertOrSkip` omite una entidad existente o inserta una nueva entidad si esta no existe en la tabla.
- `InsertOrMerge` combina una entidad existente o inserta una nueva entidad si esta no existe en la tabla.
- `InsertOrReplace` reemplaza una entidad existente o inserta una nueva entidad si esta no existe en la tabla.

Tenga en cuenta que no puede especificar la opción `/PKRS` en el escenario de importación. A diferencia del escenario de exportación, en el que debe especificar la opción `/PKRS` para iniciar las operaciones simultáneas, AzCopy iniciará de manera predeterminada operaciones simultáneas cuando importe las entidades. El número predeterminado de operaciones simultáneas que se inician equivale al número de procesadores de núcleo; sin embargo, puede especificar un número diferente para la simultaneidad con la opción `/NC`. Para obtener más detalles, escriba `AzCopy /?:NC` en la línea de comandos.


## Problemas conocidos y prácticas recomendadas

#### Ejecutar una instancia de AzCopy en un solo equipo.
AzCopy está diseñado para maximizar el uso de los recursos del equipo para acelerar la transferencia de datos. Se recomienda ejecutar solo una instancia de AzCopy en un equipo y especificar la opción `/NC` si necesita más operaciones simultáneas. Para obtener más información, escriba `AzCopy /?:NC` en la línea de comandos.

#### Habilitar algoritmos MD5 que cumplan con FIPS para AzCopy cuando "Use algoritmos que cumplen con FIPS para el cifrado, firma y operaciones hash".
AzCopy usa de forma predeterminada la implementación de .NET MD5 para calcular el MD5 al copiar los objetos, pero hay algunos requisitos de seguridad que necesitan AzCopy para habilitar la configuración de MD5 compatible con FIPS.

Puede crear un archivo app.config `AzCopy.exe.config` con la propiedad `AzureStorageUseV1MD5` y separarlo con AzCopy.exe.

	<?xml version="1.0" encoding="utf-8" ?>
	<configuration>
	  <appSettings>
	    <add key="AzureStorageUseV1MD5" value="false"/>
	  </appSettings>
	</configuration>

Para la propiedad "AzureStorageUseV1MD5" • True: el valor predeterminado, AzCopy utilizará la implementación de MD5. NET. • False: AzCopy utilizará el algoritmo de MD5 compatible con FIPS.

Tenga en cuenta que los algoritmos compatibles con FIPS están deshabilitados de forma predeterminada en el equipo de Windows, puede escribir secpol.msc en la ventana de ejecución y comprobar este modificador en Configuración de seguridad -> Directivas locales -> Opciones de seguridad -> Criptografía de sistema: Usar algoritmos compatibles con FIPS para cifrado, firma y operaciones hash.

## Versiones de AzCopy

| Versión | Novedades |
|---------|-----------------------------------------------------------------------------------------------------------------|
| **V4.2.0** | **Versión de vista previa actual. Incluye toda la funcionalidad de V3.2.0. También admite SAS de recurso compartido de almacenamiento de archivo, copia asincrónica de almacenamiento de archivos, las entidades de tabla de exportación a CSV y especifica el nombre de manifiesto al exportar entidades de tabla**
| **V3.2.0** | **Versión actual. Admite Blob de anexo y configuración de MD5 compatible con FIPS**
| V4.1.0 | Incluye toda la funcionalidad de V3.1.0. Admite la copia sincrónica de blobs y archivos y la especificación del tipo de contenido de los blobs y archivos de destino
| V3.1.0 | Admite la copia sincrónica de blobs y la especificación del tipo de contenido de los blobs de destino.
| V4.0.0 | Incluye toda la funcionalidad de V3.0.0. Admite también la copia de archivos a o desde el Almacenamiento de archivos de Azure, así como la copia de entidades a o desde el Almacenamiento de tablas de Azure.
| V3.0.0 | Modifica la sintaxis de línea de comandos de AzCopy para requerir nombres de parámetro, y rediseña la ayuda de la línea de comandos. Esta versión solo admite la copia a y desde el Almacenamiento de blobs de Azure.	
| V2.5.1 | Optimiza el rendimiento al utilizar las opciones /xo y /xn. Soluciona errores relacionados con caracteres especiales en nombres de archivos originales y daños en archivos de diario después de que el usuario especifica una sintaxis de línea de comandos errónea.	
| V2.5.0 | Optimiza el rendimiento de escenarios de copia a gran escala e introduce varias mejoras importantes relacionadas con la facilidad de uso.
| V2.4.1 | Admite la especificación de la carpeta de destino en el asistente para la instalación.                     			
| V2.4.0 | Admite la carga y descarga de archivos para Almacenamiento de archivos de Azure.
| V2.3.0 | Admite cuentas de almacenamiento con redundancia geográfica de acceso de lectura.|
| V2.2.2 | Actualizada para usar la versión 3.0.3 de la biblioteca del cliente de almacenamiento de Azure.
| V2.2.1 | Problema de rendimiento solucionado cuando se copian grandes cantidades de archivos dentro de la misma cuenta de almacenamiento.
| V2.2 | Admite el establecimiento del delimitador de directorios virtuales para nombres de blob. Admite la especificación de la ruta de acceso al archivo de diario.|
| V2.1 | Proporciona más de 20 opciones para admitir operaciones de carga, descarga y copia de blobs de una forma diferente.|


## Pasos siguientes

Para obtener más información acerca de Almacenamiento de Azure y AzCopy, consulte los recursos siguientes.

### Documentación de Almacenamiento de Azure:

- [Introducción a Almacenamiento de Azure](storage-introduction.md)
- [Almacenamiento de archivos en Almacenamiento de blobs](storage-dotnet-how-to-use-blobs.md)
- [Creación de un recurso compartido de archivos SMB en Azure con Almacenamiento de archivos](storage-dotnet-how-to-use-files.md)

### Publicaciones en blobs de Almacenamiento de Azure
- [AzCopy: Introducción a la copia sincrónica y el tipo de contenido personalizado](http://blogs.msdn.com/b/windowsazurestorage/archive/2015/01/13/azcopy-introducing-synchronous-copy-and-customized-content-type.aspx)
- [AzCopy: Presentación de la disponibilidad general de AzCopy 3.0 y de la versión de vista previa de AzCopy 4.0 compatible con tablas y archivos](http://blogs.msdn.com/b/windowsazurestorage/archive/2014/10/29/azcopy-announcing-general-availability-of-azcopy-3-0-plus-preview-release-of-azcopy-4-0-with-table-and-file-support.aspx)
- [AzCopy: Optimizada para escenarios de copia a gran escala](http://go.microsoft.com/fwlink/?LinkId=507682)
- [Introducing Microsoft Azure File Service](http://blogs.msdn.com/b/windowsazurestorage/archive/2014/05/12/introducing-microsoft-azure-file-service.aspx) (Introducción al servicio de archivos de Microsoft Azure)
- [AzCopy: Compatibilidad para almacenamiento con redundancia geográfica de acceso de lectura](http://blogs.msdn.com/b/windowsazurestorage/archive/2014/04/07/azcopy-support-for-read-access-geo-redundant-account.aspx)
- [AzCopy: Transferencia de datos con modo reiniciable y token SAS](http://blogs.msdn.com/b/windowsazurestorage/archive/2013/09/07/azcopy-transfer-data-with-re-startable-mode-and-sas-token.aspx)
- [AzCopy: Uso de copia de blobs entre cuentas](http://blogs.msdn.com/b/windowsazurestorage/archive/2013/04/01/azcopy-using-cross-account-copy-blob.aspx)
- [AzCopy: Carga y descarga de archivos para blobs de Azure](http://blogs.msdn.com/b/windowsazurestorage/archive/2012/12/03/azcopy-uploading-downloading-files-for-windows-azure-blobs.aspx)

 

<!---HONumber=August15_HO6-->