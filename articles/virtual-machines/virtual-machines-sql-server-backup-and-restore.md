<properties 
	pageTitle="Copias de seguridad y restauración para SQL Server en Máquinas virtuales de Azure"
	description="Describe las consideraciones de copia de seguridad y restauración de bases de datos de SQL Server que se ejecutan en Máquinas virtuales de Azure."
	services="virtual-machines"
	documentationCenter="na"
	authors="rothja"
	manager="jeffreyg"
	editor="monicar" />

<tags 
	ms.service="virtual-machines"
	ms.devlang="na"
	ms.topic="article"
	ms.tgt_pltfrm="vm-windows-sql-server"
	ms.workload="infrastructure-services"
	ms.date="08/05/2015"
	ms.author="jroth" />

# Copias de seguridad y restauración para SQL Server en Máquinas virtuales de Azure

## Información general

Realizar una copia de seguridad de los datos en bases de datos de SQL Server es una parte importante de la estrategia en la protección frente a la pérdida de datos debido a errores de usuario o de aplicaciones. Esto ocurre igualmente para SQL Server que se ejecuta en Máquinas virtuales de Azure (VM).

Para SQL Server que se ejecuta en Máquinas virtuales de Azure, puede usar técnicas de copia de seguridad y restauración nativas mediante discos conectados para el destino de los archivos de copia de seguridad. No obstante, hay un límite en el número de discos que se pueden conectar a una máquina virtual de Azure según el [tamaño de la máquina virtual](virtual-machines-size-specs.md). Existe también una sobrecarga de la administración de discos que tener en cuenta.

A partir de SQL Server 2014, puede realizar la copia de seguridad o restauración de Almacenamiento de blobs de Microsoft Azure. SQL Server 2016 también proporciona mejoras para esta opción. Además, para archivos de base de datos almacenados en Almacenamiento de blobs de Microsoft Azure, SQL Server 2016 proporciona una opción para copias de seguridad prácticamente instantáneas y para restauraciones rápidas con capturas de pantalla de Azure. Este artículo proporciona una introducción de estas opciones y puede encontrar información adicional en [Copia de seguridad y restauración de SQL Server con el servicio de Almacenamiento de blobs de Microsoft Azure](https://msdn.microsoft.com/library/jj919148(v=sql.130).aspx).

>[AZURE.NOTE]Para ver una discusión de las opciones para la copia de seguridad de bases de datos de gran tamaño, vea [Estrategias de copia de seguridad de base de datos de SQL Server de varios terabytes para Máquinas virtuales de Azure](http://blogs.msdn.com/b/igorpag/archive/2015/07/28/multi-terabyte-sql-server-database-backup-strategies-for-azure-virtual-machines.aspx).

Las secciones siguientes incluyen información específica para diferentes versiones de SQL Server compatibles con Máquina virtual de Azure.

## Consideraciones de copia de seguridad cuando los archivos de base de datos se almacenan en el servicio BLOB de Microsoft Azure

Las razones para realizar las copias de seguridad de base de datos y la propia tecnología de copia de seguridad subyacente camban cuando los archivos de base de datos se almacenan en Almacenamiento de blobs de Microsoft Azure. Para obtener más información sobre el almacenamiento de archivos de base de datos en Almacenamiento de blobs de Azure, vea [Archivos de datos de SQL Server en Azure](https://msdn.microsoft.com/library/jj919148.aspx).

- Ya no tendrá que realizar copias de seguridad de base de datos para proporcionar protección frente a errores multimedia o de hardware porque Microsoft Azure proporciona esta protección como parte del servicio de Microsoft Azure.

- Deberá realizar copias de seguridad de base de datos para proporcionar protección frente a errores de usuario o para fines de archivado, razones legales o fines administrativos.

- Puede realizar prácticamente copias de seguridad instantáneas y restauraciones rápidas con la característica Copia de seguridad de instantánea de archivos de SQL Server en Microsoft SQL Server 2016 Community Technology Preview 2 (CTP2). Para obtener más información, vea [Copias de seguridad de instantáneas de archivo para Archivos de base de datos en Azure](https://msdn.microsoft.com/library/mt169363.aspx).

## Copia de seguridad y restauración en Microsoft SQL Server 2016 Community Technology Preview 2 (CTP2)

Microsoft SQL Server 2016 Community Technology Preview 2 (CTP2) admite las características de [copia de seguridad y restauración de blobs de Azure](https://msdn.microsoft.com/library/jj919148.aspx) que se encuentran en SQL Server 2014 y se describen a continuación. Sin embargo, también incluye las siguientes mejoras:

- **Franja**: cuando realiza una copia de seguridad de Almacenamiento de blobs de Microsoft Azure, SQL Server 2016 admite la copia de seguridad de varios blobs para habilitar la copia de seguridad de bases de datos de gran tamaño, hasta un máximo de 12,8 TB.

- **Copia de seguridad de instantáneas**: mediante el uso de instantáneas de Azure, Copia de seguridad de instantáneas de archivos de SQL Server ofrece copias de seguridad prácticamente instantáneas y restauraciones rápidas de los archivos de base de datos almacenados con el servicio de Almacenamiento de blobs de Azure. Esta capacidad le permite simplificar las directivas de copia de seguridad y restauración. Copia de seguridad de instantáneas de archivos también es compatible con la restauración a un momento dado. Para obtener más información, vea Copias de seguridad de instantáneas para Archivos de base de datos en Azure](https://msdn.microsoft.com/library/mt169363(v=sql.130).aspx).

- **Programación de copia de seguridad administrada**: Copia de seguridad administrada de SQL Server para Azure es ahora compatible con programaciones personalizadas. Para obtener más información, vea [Copia de seguridad administrada de SQL Server para Microsoft Azure](https://msdn.microsoft.com/library/dn449496.aspx).

>[AZURE.NOTE]Para obtener un tutorial de las capacidades de SQL Server 2016 cuando use Almacenamiento de blobs de Azure, vea [Tutorial: Uso del servicio de Almacenamiento de blobs de Microsoft Azure con bases de datos de SQL Server 2016](https://msdn.microsoft.com/library/dn466438.aspx).

## Copia de seguridad y restauración en SQL Server 2014

SQL Server 2014 incluye la siguiente mejora:

1. **Copia de seguridad y restauración de Azure**:

 - *Copia de seguridad de SQL Server en dirección URL* es ahora compatible en SQL Server Management Studio. La opción de copia de seguridad de Azure está ahora disponible cuando se utiliza la tarea de copia de seguridad o restauración, o se usa el Asistente para planes de mantenimiento en SQL Server Management Studio. Para obtener más información, consulte [Copia de seguridad de SQL Server de dirección URL] (https://msdn.microsoft.com/library/jj919148(v=sql.120).aspx).
 - *Copia de seguridad administrada de SQL Server de Azure* dispone de una nueva funcionalidad que habilita la administración de copia de seguridad automatizada. Esto es especialmente útil para la administración de copia de seguridad automatizada para instancias de SQL Server 2014 que se ejecuta en Máquina de Azure. Para obtener más información, vea [Copia de seguridad administrada de SQL Server para Microsoft Azure](https://msdn.microsoft.com/library/dn449496(v=sql.120).aspx).
 - *Copia de seguridad automatizada* proporciona información adicional para habilitar automáticamente *Copia de seguridad administrada de SQL Server de Azure* en todas las bases de datos nuevas y existentes para una máquina virtual de SQL Server en Azure. Para obtener más información, consulte [Copia de seguridad automatizada para SQL Server en Máquinas virtuales](virtual-machines-sql-server-automated-backup.md).
 - Para obtener una introducción de todas las opciones para Copia de seguridad de SQL Server 2014 de Azure, vea [Copia de seguridad y restauración de SQL Server con el servicio de Almacenamiento de blobs de Microsoft Azure](https://msdn.microsoft.com/library/jj919148(v=sql.120).aspx).

1. **Cifrado**: SQL Server 2014 es compatible con el cifrado de datos cuando se crea una copia de seguridad. Admite varios algoritmos de cifrado y el uso de un certificado o una clave asimétrica. Para obtener más información, vea [Cifrado de copia de seguridad](https://msdn.microsoft.com/library/dn449489(v=sql.120).aspx).

## Copia de seguridad y restauración en SQL Server 2012

Para obtener información detallada sobre Copia de seguridad y restauración de SQL Server en SQL Server 2012, vea [Copia de seguridad y restauración de bases de datos de SQL Server (SQL Server 2012)](https://msdn.microsoft.com/library/ms187048(v=sql.110).aspx).

A partir de la actualización acumulativa de SQL Server 2012 SP1 2, puede realizar una copia de seguridad y restauración a partir del servicio de Almacenamiento de blobs de Azure. Esta mejora puede usarse para realizar la copia de seguridad de bases de datos de SQL Server en un servidor SQL Server que se ejecute en una máquina virtual de Azure o en una instancia local. Para obtener más información, vea Copia de seguridad y restauración de SQL Server con el servicio de almacenamiento Blob de Azure](https://msdn.microsoft.com/library/jj919148(v=sql.110).aspx).

Algunas de las ventajas de usar el servicio de Almacenamiento de blobs de Azure incluye la capacidad de omitir el límite de 16 discos para discos conectados, la facilidad de administración, la disponibilidad directa del archivo de copia de seguridad en otra instancia de la instancia de SQL Server que se ejecuta en una máquina virtual de Azure, o instancias locales para fines de recuperación ante desastres o migración. Para obtener una lista completa de las ventajas que supone usar un servicio de Almacenamiento de blobs de Azure para copias de seguridad de SQL Server, vea la sección *Ventajas* en [Copia de seguridad y restauración de SQL Server con el servicio de Almacenamiento de blobs de Azure](https://msdn.microsoft.com/library/jj919148(v=sql.110).aspx).

Para obtener recomendaciones sobre prácticas recomendadas e información sobre solución de problemas, vea [Prácticas recomendadas de copia de seguridad y restauración (servicio de Almacenamiento de blobs de Azure)](https://msdn.microsoft.com/library/jj919149(v=sql.110).aspx).

## Copia de seguridad y restauración de otras versiones de SQL Server compatibles en una máquina virtual de Azure

Para la copia de seguridad y restauración de SQL Server en SQL Server 2008 R2, vea (Copia de seguridad y restauración de bases de datos en SQL Server (SQL Server 2008 R2)](https://msdn.microsoft.com/library/ms187048(v=sql.105).aspx).

Para la copia de seguridad y restauración de SQL Server en SQL Server 2008, vea (Copia de seguridad y restauración de bases de datos en SQL Server (SQL Server 2008)](https://msdn.microsoft.com/library/ms187048(v=sql.100).aspx).

## Pasos siguientes

Si aún está planificando la implementación de SQL Server en una máquina virtual de Azure, puede encontrar directrices de aprovisionamiento en el siguiente tutorial: [Aprovisionamiento de Máquina virtual de SQL Server en Azure](virtual-machines-provision-sql-server.md).

Aunque la copia de seguridad y la restauración pueden usarse para migrar los datos, existen rutas de acceso de migración de datos posiblemente más sencillas para SQL Server en una máquina virtual de Azure. Para obtener una descripción completa de las opciones de migración y recomendaciones, vea [Migración de una base de datos a SQL Server en una máquina virtual de Azure](virtual-machines-migrate-onpremises-database.md).

Revise otros [recursos para ejecutar SQL Server en Máquinas virtuales de Azure](virtual-machines-sql-server-infrastructure-services.md).

<!---HONumber=August15_HO6-->