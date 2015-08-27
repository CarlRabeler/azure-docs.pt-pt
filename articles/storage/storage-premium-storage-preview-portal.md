<properties
	pageTitle="Almacenamiento premium: almacenamiento de alto rendimiento para cargas de trabajo de máquina virtual de Azure | Microsoft Azure"
	description="Información sobre el uso del Almacenamiento Premium de Azure para discos Aprenda a crear una cuenta de Almacenamiento Premium."
	services="storage"
	documentationCenter=""
	authors="tamram"
	manager="carolz"
	editor="tysonn"/>

<tags
	ms.service="storage"
	ms.workload="storage"
	ms.tgt_pltfrm="na"
	ms.devlang="na"
	ms.topic="article"
	ms.date="06/30/2015"
	ms.author="tamram;selcint"/>


# Almacenamiento premium: almacenamiento de alto rendimiento para cargas de trabajo de máquina virtual de Azure

## Información general

Bienvenido a **Almacenamiento premium de Azure para discos** para tener unas máquinas virtuales más rápidas.

Con la introducción del nuevo Almacenamiento premium, Microsoft Azure ahora ofrece dos tipos de almacenamiento duradero: **Almacenamiento premium** y **Almacenamiento estándar**. Almacenamiento premium almacena datos en unidades de estado sólido (SSD) de última tecnología, mientras que el Almacenamiento estándar almacena los datos en unidades de disco duro (HDD).

Almacenamiento premium ofrece soporte de disco de alto rendimiento y latencia baja para cargas de trabajo intensivas de E/S que se ejecutan en máquinas virtuales de Azure. Puede adjuntar varios discos de Almacenamiento premium a una máquina virtual (VM). Con Almacenamiento premium, las aplicaciones pueden tener hasta 32 TB de almacenamiento por máquina virtual y logran 64.000 E/S por segundo (operaciones de entrada/salida por segundo) por máquina virtual con latencias extremadamente bajas para operaciones de lectura.

Para empezar a usar Almacenamiento premium de Azure, visite la página [Empiece de forma gratuita](http://azure.microsoft.com/pricing/free-trial/).

Este artículo proporciona información general detallada del Almacenamiento premium de Azure.

## Cosas importantes acerca del Almacenamiento premium

La siguiente es una lista de las cosas importantes que deben tenerse en cuenta antes o al usar Almacenamiento premium:

- Para usar Almacenamiento premium, debes tener una cuenta de Almacenamiento premium. Para obtener información sobre cómo crear una cuenta de Almacenamiento premium, consulte [Creación y uso de una cuenta de Almacenamiento Premium para discos](#create-and-use-a-premium-storage-account-for-a-virtual-machine-data-disk).

- Almacenamiento premium está disponible actualmente en el [Portal de vista previa de Microsoft Azure](https://portal.azure.com/), y accesible a través de las siguientes bibliotecas de SDK: [API de REST de almacenamiento](http://msdn.microsoft.com//library/azure/dd179355.aspx) versión 2014-02-14 o posterior; [API de REST de administración del servicio](http://msdn.microsoft.com/library/azure/ee460799.aspx) versión 2014-10-01 o posterior; y [Azure PowerShell](../install-configure-powershell.md) versión 0.8.10 o posterior.

- Almacenamiento premium está disponible actualmente en las siguientes regiones: Oeste de EE. UU., Este de EE. UU. - 2, Europa occidental, Este de China, Sudeste asiático, Oeste de Japón y Este de Australia.

- Almacenamiento premium admite solo blobs en páginas de Azure, que se usan para contener discos persistentes para máquinas virtuales de Azure (VM). Para obtener información acerca de los blobs en páginas de Azure, consulte [Introducción a los blobs en bloques y a los blobs en páginas](http://msdn.microsoft.com/library/azure/ee691964.aspx). Almacenamiento premium no admite Blobs en bloque de Azure, archivos de Azure, tablas de Azure ni colas de Azure.

- Una cuenta de Almacenamiento premium es almacenamiento con redundancia local (LRS) y mantiene tres copias de los datos en una única región. Para consideraciones relativas a la replicación geográfica al usar Almacenamiento premium, consulte la sección [Instantáneas y copia de blob al Almacenamiento premium](#snapshots-and-copy-blob-whes-esing-premium-storage) en este artículo.

- Si quiere usar una cuenta de Almacenamiento premium para los discos de máquinas virtuales, necesitará usar la serie DS de máquinas virtuales. Puede usar discos de Almacenamiento estándar y premium con la serie DS de máquinas virtuales. Pero no puede usar discos de Almacenamiento premium con series que no sean DS de máquinas virtuales. Para obtener información sobre los tamaños y tipos de disco de máquina virtual de Azure disponibles, consulte [Tamaños de máquinas virtuales y servicios en la nube de Azure](http://msdn.microsoft.com/library/azure/dn197896.aspx).

- El proceso para configurar los discos de Almacenamiento Premium para una máquina virtual es similar al de los discos de Almacenamiento Estándar. Deberá elegir la opción de Almacenamiento premium más adecuada para sus discos y máquina virtual de Azure. El tamaño de la máquina virtual debe ser el adecuado para su carga de trabajo en función de las características de rendimiento de las ofertas Premium. Para obtener más información, consulte [Objetivos de escalabilidad y rendimiento al usar Almacenamiento premium](#scalability-and-performance-targets-whes-esing-premium-storage).

- Una cuenta de almacenamiento premium no se puede asignar a un nombre de dominio personalizado.

- El análisis de almacenamiento no se admite actualmente para el Almacenamiento premium. Para analizar las métricas de rendimiento de máquinas virtuales con discos en cuentas de Almacenamiento premium, utilice las herramientas en función del sistema operativo, tales como [Monitor de rendimiento de Windows](https://technet.microsoft.com/library/cc749249.aspx) para máquinas virtuales Windows e [IOSTAT](http://linux.die.net/man/1/iostat) para máquinas virtuales Linux.

## Uso de Almacenamiento Premium para discos
Puede usar Almacenamiento premium para discos de una de las siguientes maneras:

- Crear una nueva cuenta de almacenamiento premium y usarla al crear la máquina virtual.
- Crear una nueva máquina virtual de serie DS. Al crear la máquina virtual, puede seleccionar una cuenta de Almacenamiento premium creada anteriormente, crear una nueva o dejar que el Portal de Azure cree una cuenta premium de manera predeterminada.

Azure usa la cuenta de almacenamiento como un contenedor para el sistema operativo (SO) y los discos de datos. En otras palabras, si crea una máquina virtual de Azure serie DS y selecciona una cuenta de Almacenamiento premium de Azure, el sistema operativo y los discos de datos se almacenan en dicha cuenta de almacenamiento.

Para aprovechar las ventajas de Almacenamiento premium, cree primero una cuenta de Almacenamiento premium mediante un tipo de cuenta *Premium\_LRS*. Para ello, puede usar el [Portal de vista previa de Microsoft Azure](https://portal.azure.com/), [Azure PowerShell](../install-configure-powershell.md) o la [API de REST de administración del servicio](http://msdn.microsoft.com/library/azure/ee460799.aspx). Para obtener instrucciones detalladas, consulte [Creación y uso de una cuenta de Almacenamiento Premium para discos](#create-and-use-a-premium-storage-account-for-a-virtual-machine-data-disk).

### Notas importantes:

- Para obtener información sobre los límites de capacidad y de ancho de banda de la vista previa de las cuentas de Almacenamiento premium, consulte la sección [Objetivos de escalabilidad y rendimiento al usar Almacenamiento premium](#scalability-and-performance-targets-whes-esing-premium-storage). Si las necesidades de su aplicación superan los objetivos de escalabilidad de una sola cuenta de almacenamiento, compile la aplicación para que use varias cuentas de almacenamiento y divida los datos entre esas cuentas de almacenamiento. Por ejemplo, si quiere conectar discos de 51 terabytes (TB) a diferentes máquinas virtuales, distribúyalos entre dos cuentas de almacenamiento ya que 35 TB es el límite de una sola cuenta de Almacenamiento premium. Asegúrese de que una sola cuenta de Almacenamiento premium no tenga nunca más de 35 TB de discos aprovisionados.
- De forma predeterminada, la directiva de almacenamiento en caché de los discos es "Solo lectura" para todos los discos de datos Premium y "Lectura y Escritura" para el disco de sistema operativo Premium conectado a la máquina virtual. Se recomienda esta opción de configuración para lograr el rendimiento óptimo de E/S de la aplicación. Para discos de datos de solo escritura o de gran cantidad de escritura (por ejemplo, archivos de registro de SQL Server), deshabilite el almacenamiento en caché de disco para lograr un mejor rendimiento de la aplicación.
- Asegúrese de que hay suficiente ancho de banda disponible en la máquina virtual para dirigir el tráfico de disco. Por ejemplo, una máquina virtual STANDARD\_DS1 tiene un ancho de banda dedicado de 32 MB por segundo disponible para el tráfico de disco de Almacenamiento premium. Esto significa que un disco de Almacenamiento premium P10 conectado a esta máquina virtual solo puede tener hasta 32 MB por segundo, pero no hasta los 100 MB por segundo que puede proporcionar el disco P10. De forma similar, una máquina virtual STANDARD\_DS13 puede llegar hasta 256 MB por segundo en todos los discos. Actualmente, la máquina virtual más grande de la serie DS es STANDARD\_DS14 y puede proporcionar hasta 512 MB por segundo en todos los discos.

	Tenga en cuenta que estos límites son para el tráfico de disco por sí solo, sin incluir los aciertos de caché y el tráfico de red. Hay un ancho de banda independiente disponible para el tráfico de red de máquina virtual, que es diferente del ancho de banda dedicado para discos de Almacenamiento premium. En la tabla siguiente se enumeran los valores actuales de rendimiento (ancho de banda) y E/S máximos por máquina virtual de la serie DS en todos los discos conectados a la máquina virtual:

	<table border="1" cellspacing="0" cellpadding="5" style="border: 1px solid #000000;">
<tbody>
<tr>
	<td><strong>Tamaño de VM</strong></td>
	<td><strong>Núcleos de CPU</strong></td>
	<td><strong>Máx. E/S</strong></td>
	<td><strong>Máx. Ancho de banda de disco</strong></td>
</tr>
<tr>
	<td><strong>STANDARD_DS1</strong></td>
	<td>1</td>
	<td>3.200</td>
	<td>32 MB por segundo</td>
</tr>
<tr>
	<td><strong>STANDARD_DS2</strong></td>
	<td>2</td>
	<td>6.400</td>
	<td>64 MB por segundo</td>
</tr>
<tr>
	<td><strong>STANDARD_DS3</strong></td>
	<td>4</td>
	<td>12.800</td>
	<td>128 MB por segundo</td>
</tr>
<tr>
	<td><strong>STANDARD_DS4</strong></td>
	<td>8</td>
	<td>25.600</td>
	<td>256 MB por segundo</td>
</tr>
<tr>
	<td><strong>STANDARD_DS11</strong></td>
	<td>2</td>
	<td>6.400</td>
	<td>64 MB por segundo</td>
</tr>
<tr>
	<td><strong>STANDARD_DS12</strong></td>
	<td>4</td>
	<td>12.800</td>
	<td>128 MB por segundo</td>
</tr>
<tr>
	<td><strong>STANDARD_DS13</strong></td>
	<td>8</td>
	<td>25.600</td>
	<td>256 MB por segundo</td>
</tr>
<tr>
	<td><strong>STANDARD_DS14</strong></td>
	<td>16</td>
	<td>50.000</td>
	<td>512 MB por segundo</td>
</tr>
</tbody>
</table>

	Si desea conseguir la información mas reciente, consulte el artículo [Tamaños de máquinas virtuales y de servicios en la nube de Azure] (http://msdn.microsoft.com/library/azure/dn197896.aspx). Para obtener más información acerca de los discos de almacenamiento Premium y de sus IOPS y límites de rendimiento, consulte la tabla que encontrará en la sección [Objetivos de escalabilidad y rendimiento al usar el almacenamiento Premium](#scalability-and-performance-targets-whes-esing-premium-storage) de este artículo.

> [AZURE.NOTE]Los aciertos de caché no están limitados por las E/S o rendimiento asignados del disco. Es decir, cuando se utiliza un disco de datos con la configuración de caché de solo lectura en una máquina virtual de la serie DS, las lecturas que se atienden desde la memoria caché no están sujetas a los límites de disco de Almacenamiento premium. Por lo tanto, podría obtener un rendimiento muy alto desde un disco si la carga de trabajo es fundamentalmente de lectura. Tenga en cuenta que la caché depende de límites de E/S por segundo o de rendimiento independientes a nivel de la máquina virtual en función del tamaño de dicha máquina. Las máquinas virtuales de la serie DS tienen aproximadamente 4000 E/S por segundo y 33 MB/seg. por núcleo de caché y E/S de SSD locales.

- Puede usar discos de Almacenamiento premium y estándar en la misma máquina virtual serie DS.
- Con el Almacenamiento Premium, puede aprovisionar una máquina virtual de serie DS y conectar varios discos de datos persistentes a una máquina virtual. Si es necesario, puede crear bandas en los discos para aumentar la capacidad y el rendimiento del volumen. Si secciona discos de datos Almacenamiento premium usando [Espacios de almacenamiento](http://technet.microsoft.com/library/hh831739.aspx), será necesario configurarlo con una columna por cada disco que use. De lo contrario, el rendimiento general del volumen seccionado puede ser inferior al esperado debido a la distribución desigual de tráfico entre los discos. De forma predeterminada, la interfaz de usuario (IU) del administrador del servidor permite configurar columnas de hasta 8 discos. Pero si tiene más de 8 discos, necesitará usar PowerShell para crear el volumen y especificar manualmente el número de columnas. De lo contrario, la IU del administrador del servidor sigue usando 8 columnas aunque tenga más discos. Por ejemplo, si tiene 32 discos en un conjunto de bandas único, debe especificar 32 columnas. Puede usar el parámetro *NumberOfColumns* del cmdlet [New-VirtualDisk](http://technet.microsoft.com/library/hh848643.aspx) de PowerShell para especificar el número de columnas que usa el disco virtual. Para obtener más información, consulte [Storage Spaces Overview](http://technet.microsoft.com/library/jj822938.aspx) (Introducción a espacios de almacenamiento) y [Storage Spaces Frequently Asked Questions](http://social.technet.microsoft.com/wiki/contents/articles/11382.storage-spaces-frequently-asked-questions-faq.aspx) (Preguntas más frecuentes sobre espacios de almacenamiento).
- Evite agregar máquinas virtuales de la serie DS a un servicio en la nube existente que incluya máquinas virtuales que no son de la serie DS. Una posible solución consiste en migrar los VHD existentes a un nuevo servicio en la nube ejecutando solo máquinas virtuales de la serie DS. Si desea conservar la misma dirección IP virtual (VIP) para el nuevo servicio en la nube que hospeda las máquinas virtuales de la serie DS, use la característica [Direcciones IP reservadas](virtual-networks-configure-vnet-to-vnet-connection.md).
- La serie DS de máquinas virtuales de Azure se puede configurar para utilizar un disco del sistema operativo (SO) hospedado en una cuenta de Almacenamiento estándar o en una cuenta de Almacenamiento premium. Si utiliza el disco del sistema operativo solo para el arranque, puede usar un disco del sistema operativo basado en el almacenamiento estándar. Esto proporciona ventajas económicas y resultados de rendimiento similares al Almacenamiento premium tras el arranque. Si realiza cualquier tarea adicional en el disco del sistema operativo que no sea de arranque, use el Almacenamiento premium ya que proporciona mejores resultados de rendimiento. Por ejemplo, si la aplicación lee y escribe desde y hacia el disco del sistema operativo, el uso del disco del sistema operativo basado en Almacenamiento premium proporciona un mejor rendimiento para la máquina virtual.
- Puede usar la [Interfaz de línea de comandos de Azure (CLI de Azure)](../xplat-cli.md) con Almacenamiento premium. Para cambiar la directiva de caché en uno de los discos mediante la CLI de Azure, ejecute el siguiente comando:

	`$ azure vm disk attach -h ReadOnly <VM-Name> <Disk-Name>`

	Tenga en cuenta que las opciones de directiva de almacenamiento en caché pueden ser ReadOnly, None o ReadWrite. Para ver más opciones, consulte la Ayuda ejecutando el comando siguiente:

	`azure vm disk attach --help`


## Objetivos de escalabilidad y rendimiento al usar Almacenamiento Premium

Cuando aprovisiona un disco con una cuenta de Almacenamiento premium, la cantidad de operaciones de E/S por segundo (IOPS) y el rendimiento (ancho de banda) que se pueden obtener depende del tamaño del disco. Actualmente, hay tres tipos de discos de Almacenamiento premium: P10, P20 y P30. Cada uno tiene límites de IOPS y de rendimiento específicos, tal como se especifica en la tabla siguiente:

<table border="1" cellspacing="0" cellpadding="5" style="border: 1px solid #000000;">
<tbody>
<tr>
	<td><strong>Tipo de disco de Almacenamiento premium</strong></td>
	<td><strong>P10</strong></td>
	<td><strong>P20</strong></td>
	<td><strong>P30</strong></td>
</tr>
<tr>
	<td><strong>Tamaño del disco</strong></td>
	<td>128 GB</td>
	<td>512 GB</td>
	<td>1024 GB (1 TB)</td>
</tr>
<tr>
	<td><strong>E/S por segundo por disco</strong></td>
	<td>500</td>
	<td>2300</td>
	<td>5.000</td>
</tr>
<tr>
	<td><strong>Rendimiento por disco</strong></td>
	<td>100 MB por segundo *</td>
	<td>150 MB por segundo *</td>
	<td>200 MB por segundo *</td>
</tr>
</tbody>
</table>

> [AZURE.NOTE]Asegúrese de que haya suficiente ancho de banda disponible en la máquina virtual para dirigir el tráfico de disco, tal como se explica en la sección [Uso de Almacenamiento Premium para discos](#using-premium-storage-for-disks) mostrada anteriormente en este artículo. De lo contrario, el rendimiento y las E/S por segundo del disco estarán obligados a reducir los valores basándose en los límites de la máquina virtual en vez de en los límites de disco que se mencionan en la tabla anterior.

Azure asigna el tamaño de disco (redondeado) a la opción de disco de Almacenamiento premium más cercana según se especifica en la tabla. Por ejemplo, un disco de 100 GB se clasifica como una opción P10, puede realizar hasta 500 unidades de E/S por segundo y dispone de un rendimiento de hasta 100 MB por segundo. De forma similar, un disco de 400 GB se clasifica como una opción P20, puede realizar hasta 2300 unidades de E/S por segundo y dispone de un rendimiento de hasta 150 MB por segundo.

El tamaño de la unidad de entrada y salida (E/S) es de 256 KB. Si el tamaño de los datos transferidos es inferior a 256 KB, se considera una sola unidad de E/S. Los tamaños de E/S más grandes se cuentan como varias operaciones de E/S con un tamaño de 256 KB. Por ejemplo, una E/S de 1100 KB se cuenta como cinco unidades de E/S.

El límite de rendimiento incluye escrituras en el disco, así como lecturas desde ese disco que no se sirven desde la memoria caché. Por ejemplo, la suma de las lecturas y de las escrituras que no son de caché debe ser inferior o igual a 100 MB por segundo para un disco P10. De forma similar, por ejemplo, un único disco P10 puede tener 100 MB por segundo de lecturas que no son de caché o 100 MB por segundo de escrituras, o 60 MB por segundo de lecturas con 40 MB de escrituras por segundo.

Al crear los discos de Azure, seleccione la oferta de disco de Almacenamiento premium más adecuada según las necesidades de su aplicación en cuanto a capacidad, rendimiento, escalabilidad y picos de carga.

> [AZURE.NOTE]Puede aumentar fácilmente el tamaño de los discos existentes. Por ejemplo, si desea aumentar el tamaño de un disco de 30 GB a 128 GB o 1 TB. O bien, si lo desea, convertir el disco P20 en disco P30 porque necesita más capacidad o más E/S por segundo y rendimiento. Puede expandir el disco mediante el commandlet "Update-AzureDisk" de PowerShell con la propiedad "-ResizedSizeInGB". Para realizar esta acción, se debe desconectar el disco de la máquina virtual o se debe detener la máquina virtual.

En la tabla siguiente se describen los objetivos de escalabilidad para las cuentas de Almacenamiento premium:

<table border="1" cellspacing="0" cellpadding="5" style="border: 1px solid #000000;">
<tbody>
<tr>
	<td><strong>Capacidad total de la cuenta</strong></td>
	<td><strong>Ancho de banda total de una cuenta de almacenamiento con redundancia local</strong></td>
</tr>
<tr>
	<td>
	<ul>
       <li type=round>Capacidad de disco: 35 TB</li>
       <li type=round>Capacidad de instantánea: 10 TB</li>
    </ul>
	</td>
	<td>Hasta 50 gigabits por segundo de entrada y salida</td>
</tr>
</tbody>
</table>

- Entrada se refiere a todos los datos (solicitudes) que se envían a una cuenta de almacenamiento.
- Salida se refiere a todos los datos (respuestas) recibidos desde una cuenta de almacenamiento.

Para obtener más información, consulte [Objetivos de escalabilidad y rendimiento de Almacenamiento de Azure](http://msdn.microsoft.com/library/azure/dn249410.aspx).

## Limitación al usar Almacenamiento Premium
Puede que se produzca una limitación si las E/S por segundo o el rendimiento de la aplicación superan los límites asignados a un disco de Almacenamiento premium o si el tráfico de disco total en todos los discos de la máquina virtual supera el límite de ancho de banda de disco disponible para la máquina virtual. Para evitar la limitación, recomendamos que limite el número de solicitudes de E/S pendientes para discos basándose en los objetivos de rendimiento y escalabilidad que ha aprovisionado y en el ancho de banda de disco disponible para la máquina virtual.

La aplicación puede lograr la menor latencia cuando se ha diseñado para evitar la limitación. Por otro lado, si el número de solicitudes de E/S pendientes del disco es demasiado pequeño, la aplicación no puede aprovechar al máximo los niveles de rendimiento y IOPS que están disponibles en el disco.

En los ejemplos siguientes se muestra cómo calcular los niveles de limitación:

### Ejemplo 1:
La aplicación hizo 495 E/S de tamaño de disco de 16 KB (que es igual a 495 unidades de E/S) en un segundo en un disco P10. Si intenta una E/S de 2 MB en el mismo segundo, el total de unidades de E/S es igual a 495 + 8. Esto es porque una E/S de 2 MB resulta en 2048 KB / 256 KB = 8 unidades de E/S cuando el tamaño de la unidad de E/S es de 256 KB. Puesto que la suma de 495 + 8 supera el límite IOPS de 500 para el disco, se produce la limitación.

### Nota:
Todos los cálculos se basan en el tamaño de una unidad de E/S de 256 KB.

### Ejemplo 2:
La aplicación hizo 400 E/S de tamaño de disco de 256 KB en un disco P10. El ancho de banda total consumido es (400 * 256) / 1024 = 100 MB/s. Observe que el límite de rendimiento de un disco P10 es de 100 MB por segundo. Si la aplicación intenta realizar más operaciones de E/S en ese segundo, se limita porque supera el límite asignado.

### Ejemplo 3:
Tiene una máquina virtual DS4 con dos discos P30 conectados. Cada disco P30 es capaz de tener un rendimiento de 200 MB por segundo. Sin embargo, una máquina virtual DS4 tiene una capacidad de ancho de banda de disco total de 256 MB por segundo. Por lo tanto, no puede dirigir los discos conectados al máximo rendimiento en esta máquina virtual DS4 al mismo tiempo. Para resolver este problema, puede mantener el tráfico de 200 MB por segundo en un disco y 56 MB por segundo en el otro disco. Si la suma del disco tráfico supera los 256 MB por segundo, el tráfico de disco queda limitado.

### Nota:
Si el tráfico de disco consta principalmente de tamaños de E/S pequeños, es muy probable que la aplicación alcance el límite de IOPS antes que el límite de rendimiento. Por otro lado, si el tráfico de disco consiste principalmente en tamaños grandes de E/S, es muy probable que la aplicación alcance el límite de rendimiento en lugar del límite de IOPS. Puede aumentar la tasa de IOPS de su aplicación y la capacidad de rendimiento mediante el uso de tamaños de E/S óptimos y también mediante la limitación del número de solicitudes de E/S pendientes de un disco.

## Instantáneas y copias de blobs al usar Almacenamiento Premium
Puede crear una instantánea para el Almacenamiento premium de la misma que crea una instantánea al usar Almacenamiento estándar. Dado que el Almacenamiento premium es redundante localmente, se recomienda crear instantáneas y, luego, copiarlas en una cuenta de almacenamiento estándar con redundancia geográfica. Para obtener más información, consulte [Opciones de redundancia de Almacenamiento de Azure](http://msdn.microsoft.com/library/azure/dn727290.aspx).

Si un disco está conectado a una máquina virtual, no se permiten determinadas operaciones de API en el blob en página que respalda el disco. Por ejemplo, no puede realizar una operación [Copy Blob](http://msdn.microsoft.com/library/azure/dd894037.aspx) en ese blob mientras el disco esté conectado a una máquina virtual. En su lugar, cree una instantánea del blob mediante el método de API de REST [Snapshot Blob](http://msdn.microsoft.com/library/azure/ee691971.aspx) y, luego, realice la operación [Copy Blob](http://msdn.microsoft.com/library/azure/dd894037.aspx) de la instantánea para copiar el disco conectado. Como alternativa, puede desasociar el disco y, luego, realizar las operaciones necesarias en el blob subyacente.

### Notas importantes:

- Si la operación de blob de copia en un Almacenamiento premium sobrescribe un blob existente en el destino, el blob sobrescrito no deberá tener instantáneas. Una copia en o entre cuentas de almacenamiento premium requiere que el blob de destino no tenga instantáneas al iniciar la copia.
- El número de instantáneas de un único blob está limitado a 100. Una instantánea puede tomarse cada 10 minutos como máximo.
- 10 TB es la capacidad máxima para las instantáneas por cuenta de Almacenamiento premium. Tenga en cuenta que la capacidad de instantánea es el único dato que existe en las instantáneas. En otras palabras, la capacidad de instantánea no incluye el tamaño del blob base.
- Para copiar una instantánea de una cuenta de Almacenamiento premium a otra cuenta, debe hacer primero un CopyBlob de la instantánea para crear un nuevo blob en la misma cuenta de Almacenamiento premium. A continuación, puede copiar el nuevo blob a otras cuentas de almacenamiento. Puede eliminar el blob intermedio una vez finalizada la copia. Siga esta procedimiento para copiar instantáneas de una cuenta de Almacenamiento premium a una cuenta de Almacenamiento estándar con redundancia geográfica mediante AzCopy o Copy Blob. Para obtener más información, consulte [Uso de AzCopy con Almacenamiento de Microsoft Azure](storage-use-azcopy.md) y [Copy Blob](http://msdn.microsoft.com/library/azure/dd894037.aspx).
- Para obtener más información acerca de cómo realizar operaciones REST en blobs en página en las cuentas de Almacenamiento premium, consulte [Uso de operaciones de servicios BLOB con Almacenamiento premium de Azure](http://go.microsoft.com/fwlink/?LinkId=521969) en MSDN library.

## Uso de máquinas virtuales Linux con Almacenamiento premium
Consulte a continuación instrucciones importantes para configurar las máquinas virtuales Linux en Almacenamiento premium:

- Para todos los discos de Almacenamiento premium con la configuración de caché como “ReadOnly” o “None”, debe deshabilitar “barreras” al montar el sistema de archivos a fin de lograr los objetivos de escalabilidad para Almacenamiento Premium. No es necesita barreras para este escenario porque las escrituras en los discos de Almacenamiento premium de copia de seguridad son duraderas para esta configuración de caché. Cuando la solicitud de escritura se completa correctamente, los datos están escritos en el almacenamiento permanente. Utilice los métodos siguientes para deshabilitar “barreras” en función del sistema de archivos:
	- Si utiliza **reiserFS**, deshabilite las barreras mediante la opción de montaje “barrier=none” (para habilitar las barreras, use “barrier=flush”)
	- Si utiliza **ext3/ext4**, deshabilite las barreras mediante la opción de montaje “barrier=0” (para habilitar las barreras, use “barrier=1”)
	- Si utiliza **XFS**, deshabilite las barreras mediante la opción de montaje “nobarrier” (para habilitar las barreras, use la opción “barrier”)

- Para discos de Almacenamiento premium con caché de configuración "ReadWrite", deben habilitarse las barreras para la durabilidad de las escrituras.

Las siguientes son las distribuciones de Linux que se validan con Almacenamiento premium. Se recomienda actualizar las máquinas virtuales al menos a una de estas versiones (o posteriores) para mejorar el rendimiento y la estabilidad con Almacenamiento premium. Además, algunas de las versiones requieren el LIS más reciente (Linux Integration Services v4.0 para Microsoft Azure). Siga el vínculo proporcionado para su descarga e instalación. Seguiremos agregando más imágenes a la lista según vayamos completando validaciones adicionales. Tenga en cuenta que nuestras validaciones demostraron que el rendimiento varía para estas imágenes y también depende de las características de carga de trabajo y la configuración de las imágenes. Se optimizan diferentes imágenes para distintos tipos de cargas de trabajo. 
<table border="1" cellspacing="0" cellpadding="5" style="border: 1px solid #000000;">
<tbody>
<tr>
	<td><strong>Distribución</strong></td>
	<td><strong>Versión</strong></td>
	<td><strong>Kernel admitido</strong></td>
	<td><strong>Imagen admitida</strong></td>
</tr>
<tr>
	<td rowspan="4"><strong>Ubuntu</strong></td>
	<td>12.04</td> <td>3.2.0-75.110</td>
	<td>Ubuntu-12_04_5-LTS-amd64-server-20150119-es-es-30GB</td>
</tr>
<tr>
	<td>14.04</td> <td>3.13.0-44.73</td> <td>Ubuntu-14_04_1-LTS-amd64-server-20150123-es-es-30GB</td>
</tr>
<tr>
	<td>14.10</td>
	<td>3.16.0-29.39</td>
	<td>Ubuntu-14_10-amd64-server-20150202-es-es-30GB</td>
</tr>
<tr>
	<td>15.04</td>
	<td>3.19.0-15</td>
	<td>Ubuntu-15_04-amd64-server-20150422-es-es-30GB</td>
</tr>
<tr>
	<td><strong>SUSE</strong></td>
	<td>SLES 12</td>
	<td>3.12.36-38.1</td>
	<td>suse-sles-12-priority-v20150213<br>suse-sles-12-v20150213</td>
</tr>
<tr>
	<td><strong>CoreOS</strong></td>
	<td>584.0.0</td>
	<td>3.18.4</td>
	<td>CoreOS 584.0.0</td>
</tr>
<tr>
	<td rowspan="2"><strong>CentOS</strong></td>
	<td>6.5, 6.6, 7.0</td>
	<td></td>
	<td><a href="http://go.microsoft.com/fwlink/?LinkID=403033&clcid=0x409"> LIS 4.0 requerido</a></td>
</tr>
<tr>
	<td>7.1</td>
	<td>3.10.0-229.1.2.el7</td>
	<td><a href="http://go.microsoft.com/fwlink/?LinkID=403033&clcid=0x409"> LIS 4.0 recomendado</a></td>
</tr>

<tr>
	<td rowspan="2"><link id="138" refid="139" url="virtual-machines-oracle-azure-virtual-machines.md">Oracle</link></td>
	<td>6.4.</td>
	<td></td>
	<td><a href="http://go.microsoft.com/fwlink/?LinkID=403033&clcid=0x409"> LIS 4.0 necesarios </a></td>
</tr>
<tr>
	<td>7.0</td>
	<td></td>
	<td>Póngase en contacto con soporte técnico para obtener más información</td>
</tr>
</tbody>
</table>

### Controladores de LIS para CentOS Openlogic

Los clientes que ejecutan las máquinas virtuales de OpenLogic CentOS tienen que ejecutar el comando siguiente para instalar a los controladores más recientes:

	sudo yum install microsoft-hyper-v

A continuación, se le pedirá un reinicio para activar los nuevos controladores.



## Precios y facturación al usar Almacenamiento Premium
Al usar Almacenamiento premium, se aplican las siguientes consideraciones de facturación:

- La facturación de un disco de Almacenamiento premium depende del tamaño aprovisionado del disco. Azure asigna el tamaño de disco (redondeado) a la opción de disco de Almacenamiento premium más cercana según se especifica en la tabla que se proporciona en la sección [Objetivos de escalabilidad y rendimiento al usar Almacenamiento premium](#scalability-and-performance-targets-whes-esing-premium-storage). La facturación de cualquier disco aprovisionado se prorratea cada hora con el precio mensual de la oferta de Almacenamiento premium. Por ejemplo, si se aprovisiona un disco P10 y se elimina después de 20 horas, se factura la oferta P10 prorrateada a 20 horas. Esto es así independientemente de la cantidad de datos que se escriba en el disco o del rendimiento/IOPS usado.
- Las instantáneas en el Almacenamiento premium se facturan por la capacidad adicional usada por las instantáneas. Para obtener información sobre las instantáneas, consulte [Crear una instantánea de un blob](http://msdn.microsoft.com/library/azure/hh488361.aspx).
- Las [transferencias de datos de salida](http://azure.microsoft.com/pricing/details/data-transfers/) (datos que salen de los centros de datos de Azure) incurren en la facturación por el uso de ancho de banda.

Para obtener información detallada sobre los precios de Almacenamiento premium y máquinas virtuales de la serie DS, consulte:

- [Precios de Almacenamiento de Azure](http://azure.microsoft.com/pricing/details/storage/)
- [Precios de máquinas virtuales](http://azure.microsoft.com/pricing/details/virtual-machines/)

## Creación y uso de una cuenta de Almacenamiento premium para un disco de datos de la máquina virtual

En esta sección se muestra cómo crear una cuenta de Almacenamiento premium mediante el Portal de vista previa de Azure, Azure PowerShell y la Interfaz de línea de comandos de Azure (CLI de Azure). Además, muestra un caso de uso de ejemplo para las cuentas de almacenamiento premium: crear una máquina virtual y adjuntar un disco de datos a una máquina virtual al usar Almacenamiento premium.

### Creación de una máquina virtual de Azure mediante Almacenamiento premium a través del Portal de vista previa de Azure

En esta sección se muestra cómo crear una cuenta de Almacenamiento premium mediante el Portal de vista previa de Azure.

1.	Inicie sesión en el [Portal de vista previa de Azure](https://portal.azure.com/). Consulte la oferta [Evaluación gratuita](http://azure.microsoft.com/pricing/free-trial/) si todavía no tiene una suscripción.


    > [AZURE.NOTE]Si inicia sesión en el Portal de administración de Azure, haga clic en el nombre de cuenta de usuario en la esquina superior derecha del portal. A continuación, haga clic en **Cambiar al nuevo portal**.


2.	En el menú Concentrador, haga clic en **Nuevo**.

3.	En **Nuevo**, haga clic en **Todo**. Seleccione **Almacenamiento, caché, + copia de seguridad**. Desde allí, haga clic en **Almacenamiento** y, luego, en **Crear**.

4.	En el cuadro Cuenta de almacenamiento, escriba un nombre para la cuenta de almacenamiento. Haga clic en **Nivel de precios**. En la hoja **Niveles de precios recomendados**, haga clic en **Examinar todos los niveles de precios**. En la hoja **Elegir nivel de precios**, elija **Redundancia local premium**. Haga clic en **Seleccionar**. Tenga en cuenta que la hoja **Cuenta de almacenamiento** muestra **GRS estándar** como **Nivel de precios** predeterminado. Tras hacer clic en **Seleccionar**, el **Nivel de precios** se muestra como **Premium\_LRS**.

	![Nivel de precios][Image1]


5.	En la hoja **Cuenta de almacenamiento**, mantenga los valores predeterminados de **Grupo de recursos**, **Suscripción**, **Ubicación** y **Diagnósticos**. Haga clic en **Crear**.

Para obtener un tutorial completo del entorno de Azure, consulte [Creación de una máquina virtual que ejecuta Windows en el Portal de vista previa de Azure](../virtual-machines-windows-tutorial-azure-preview.md).

### Creación de una máquina virtual de Azure mediante Almacenamiento premium a través de Azure PowerShell
Este ejemplo de PowerShell muestra cómo crear una nueva cuenta de Almacenamiento premium y conectar un disco de datos que use esa cuenta a una nueva máquina virtual de Azure.

1. Configure el entorno de PowerShell siguiendo los pasos que se indican en [Instalación y configuración de Azure PowerShell](../install-configure-powershell.md).
2. Inicie la consola de PowerShell, conéctese a su suscripción y ejecute el cmdlet siguiente de PowerShell en la ventana de la consola. Tal como se muestra en la instrucción de PowerShell, deberá especificar el parámetro **Type** como **Premium\_LRS** al crear una cuenta de Almacenamiento premium.

		New-AzureStorageAccount -StorageAccountName "yourpremiumaccount" -Location "West US" -Type "Premium_LRS"

3. A continuación, cree una nueva máquina virtual de la serie DS y especifique que quiere Almacenamiento premium mediante la ejecución de los siguientes cmdlets de PowerShell en la ventana de consola:

    	$storageAccount = "yourpremiumaccount"
    	$adminName = "youradmin"
    	$adminPassword = "yourpassword"
    	$vmName ="yourVM"
    	$location = "West US"
    	$imageName = "a699494373c04fc0bc8f2bb1389d6106__Windows-Server-2012-R2-201409.01-en.us-127GB.vhd"
    	$vmSize ="Standard_DS2"
    	$OSDiskPath = "https://" + $storageAccount + ".blob.core.windows.net/vhds/" + $vmName + "_OS_PIO.vhd"
    	$vm = New-AzureVMConfig -Name $vmName -ImageName $imageName -InstanceSize $vmSize -MediaLocation $OSDiskPath
    	Add-AzureProvisioningConfig -Windows -VM $vm -AdminUsername $adminName -Password $adminPassword
    	New-AzureVM -ServiceName $vmName -VMs $VM -Location $location

4. Si quiere más espacio en disco para la máquina virtual, adjunte un nuevo disco de datos a una máquina virtual de la serie DS existente después de crearlo mediante la ejecución de los siguientes cmdlets de PowerShell en la ventana de consola:

    	$storageAccount = "yourpremiumaccount"
    	$vmName ="yourVM"
    	$vm = Get-AzureVM -ServiceName $vmName -Name $vmName
    	$LunNo = 1
    	$path = "http://" + $storageAccount + ".blob.core.windows.net/vhds/" + "myDataDisk_" + $LunNo + "_PIO.vhd"
    	$label = "Disk " + $LunNo
    	Add-AzureDataDisk -CreateNew -MediaLocation $path -DiskSizeInGB 128 -DiskLabel $label -LUN $LunNo -HostCaching ReadOnly -VM $vm | Update-AzureVm

### Creación de una máquina virtual de Azure mediante Almacenamiento premium a través de la interfaz de línea de comandos de Azure

La [Interfaz de la línea de comandos de Azure](../xplat-cli.md) (CLI de Azure) proporciona un conjunto de comandos de código abierto multiplataforma para trabajar con la plataforma Azure. Los ejemplos siguientes muestran cómo utilizar la CLI de Azure (versión 0.8.14 y posterior) para crear una cuenta de Almacenamiento premium, una nueva máquina virtual y conectar un nuevo disco de datos desde una cuenta de Almacenamiento premium.

#### Crear una cuenta de Almacenamiento premium

````
azure storage account create "premiumtestaccount" -l "west us" --type PLRS
````

#### Creación de una máquina virtual de la serie DS

	azure vm create -z "Standard_DS2" -l "west us" -e 22 "premium-test-vm"
		"b39f27a8b8c64d52b05eac6a62ebad85__Ubuntu-14_10-amd64-server-20150202-es-es-30GB" -u "myusername" -p "passwd@123"

#### Visualización de información acerca de la máquina virtual

	azure vm show premium-test-vm

#### Acoplamiento de un nuevo disco de datos

	azure vm disk attach-new premium-test-vm 20 https://premiumstorageaccount.blob.core.windows.net/vhd-store/data1.vhd

#### Visualización de información acerca del nuevo disco de datos

	azure vm disk show premium-test-vm-premium-test-vm-0-201502210429470316

## Pasos siguientes

[Uso de operaciones de servicios BLOB con Almacenamiento premium de Azure](http://go.microsoft.com/fwlink/?LinkId=521969)

[Creación de una máquina virtual que ejecuta Windows](../virtual-machines-windows-tutorial-azure-preview.md)

[Tamaños de máquinas virtuales y servicios en la nube de Azure](http://msdn.microsoft.com/library/azure/dn197896.aspx)

[Documentación de almacenamiento](http://azure.microsoft.com/documentation/services/storage/)

[Referencia de MSDN](http://msdn.microsoft.com/library/azure/gg433040.aspx)

[Image1]: ./media/storage-premium-storage-preview-portal/Azure_pricing_tier.png
 

<!---HONumber=August15_HO6-->