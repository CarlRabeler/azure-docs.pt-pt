<properties 
    pageTitle="Notas de la versión de la actualización 1 de la serie StorSimple 8000 - enero de 2015 | Microsoft Azure"
    description="Describe las nuevas características, problemas y soluciones alternativas de la actualización 1 de la serie StorSimple 8000."
    services="storsimple"
    documentationCenter="NA"
    authors="alkohli"
    manager="carolz"
    editor="" />
 <tags 
    ms.service="storsimple"
    ms.devlang="NA"
    ms.topic="article"
    ms.tgt_pltfrm="NA"
    ms.workload="TBD"
    ms.date="08/19/2015"
    ms.author="alkohli" />

# Notas de la versión 1 de la actualización de la serie StorSimple 8000  

## Información general

Las siguientes notas de la versión describen las características nuevas e identifican los problemas críticos abiertos de la actualización 1 de la serie StorSimple 8000. También contienen una lista de las actualizaciones de software y firmware de StorSimple incluidas en esta versión. Esta es la primera versión principal que aparece después de que la versión de lanzamiento de la serie StorSimple 8000 Series se pusiera a disposición general en julio de 2014.

Esta actualización cambia el software del dispositivo para que sea la actualización 1 de la serie StorSimple 8000. Revise la información contenida en las notas de la versión antes de implementar la actualización de la solución de StorSimple. Para obtener más información, consulte cómo instalar [la actualización 1 en un dispositivo de StorSimple](storsimple-install-update-1.md).

Revise la información contenida en las notas de la versión antes de implementar las actualizaciones de la solución de StorSimple.

>[AZURE.IMPORTANT]
> 
- El 23 de junio se lanzó una revisión de gran importancia, la actualizazión 1.1. Esta revisión soluciona un problema que surgía en el motor de copia de seguridad. Si aplicó la actualización 1 antes del 23 de junio y está utilizando la versión de software **6.3.9600.17491**, asegúrese de aplicar esta actualización crítica para evitar problemas con las copias de seguridad. Una vez instalada la actualización, la versión del software cambiará a **6.3.9600.17521**.
- Si ha creado un dispositivo virtual entre el 27 de mayo y el 10 de julio cuya versión de software sea la 6.3.9600.17481, cree un nuevo dispositivo virtual y conmute por error los volúmenes del dispositivo virtual antiguo al nuevo. (Esto se debe a que no se puede actualizar el dispositivo virtual anterior). Si no crea un nuevo dispositivo virtual, es posible que las copias de seguridad empiezan a generar errores. Para ver los procedimientos de recuperación ante desastres y conmutación por error, vaya a [Conmutación por error y recuperación ante desastres para el dispositivo StorSimple](storsimple-device-failover-disaster-recovery.md).
- Para instalar la actualización 1, use el Servicio de Administrador de StorSimple, no Windows PowerShell para StorSimple.
- Esta versión contiene también las actualizaciones del firmware del disco que solo pueden aplicarse cuando el dispositivo está en modo de mantenimiento. Estas actualizaciones son perturbadoras, por lo que provocarán un tiempo de inactividad en el dispositivo. Estas actualizaciones se pueden aplicar durante el mantenimiento planificado.
- Esta actualización tarda aproximadamente 5-10 horas en instalarse (incluidas las actualizaciones de Windows). 
- Para las nuevas versiones, no podrán ver las actualizaciones de inmediato porque hacemos una implementación por fases de las actualizaciones. Busque actualizaciones de nuevo en unos días ya que estas estarán disponible pronto.

## Novedades de la actualización 1

Esta actualización contiene las siguientes características nuevas y mejoras:

- **La migración de la serie 5000-7000 a dispositivos de la serie 8000**: esta versión presenta una nueva característica de migración que permite a los usuarios de dispositivos de la serie 5000-7000 migrar sus datos a un dispositivo físico de la serie StorSimple 8000 o a un dispositivo virtual de 1100. La característica de migración tiene dos propuestas de valor claves:                                                                  

    - **Continuidad del negocio**, mediante la habilitación de la migración de los datos existentes en los dispositivos de las series 5000-7000 a dispositivos de la serie 8000.
    - **Mejores ofertas de características de los dispositivos de la serie 8000**, como la administración centralizada eficaz de varios dispositivos a través del servicio StorSimple Manager, mejor clase de hardware y firmware actualizado, dispositivos virtuales, movilidad de datos y las características de la estrategia futura.

    Para obtener más información sobre cómo migrar un dispositivo de las series 5000-7000 de StorSimple a la serie 8000, consulte la [Guía de migración](http://www.microsoft.com/download/details.aspx?id=47322).

- **Disponibilidad en el Portal de Azure Government**: StorSimple está disponible en el Portal de Azure Government. Consulte cómo [implementar un dispositivo StorSimple en el Portal de Azure Government](storsimple-deployment-walkthrough-gov.md).

- **Compatibilidad con otros proveedores de servicios en la nube**: los restantes proveedores de servicios en la nube que se admiten son Amazon S3, Amazon S3 con RRS, HP y OpenStack (beta).

- **Actualización a las API de almacenamiento más recientes**: con esta versión, StorSimple se ha actualizado a las API de los servicios de Almacenamiento de Azure más recientes. Los dispositivos de la serie StorSimple 8000 que ejecutan GA utilizan versiones de las API del servicio de Almacenamiento de Azure anteriores al 12 de febrero de 2012. Como se indicó en el [anuncio acerca de la eliminación de versiones del Servicio de almacenamiento](http://azure.microsoft.com/blog/2014/08/04/microsoft-azure-storage-service-version-removal/), el 10 de diciembre de 2015 estas API dejarán de utilizarse. La actualización 1 de la serie StorSimple 8000 debe aplicarse antes del 9 de diciembre de 2015. Si no lo hace, los dispositivos de StorSimple dejará de funcionar correctamente.

- **Soporte para el almacenamiento con redundancia de zona (ZRS)**: con la actualización a la versión más reciente de las API de almacenamiento, la serie StorSimple 8000 será compatible con el almacenamiento con redundancia de zona (ZRS), además del almacenamiento con redundancia local (LRS) y el almacenamiento con redundancia geográfica (GRS). Para obtener información detallada sobre ZRS, consulte este [artículo sobre las opciones de redundancia de Almacenamiento de Azure](../storage/storage-redundancy.md).

- **Mejora de la experiencia de implementación inicial y actualización**: en esta versión, se han mejorado los procesos de instalación y actualización. La instalación a través del Asistente para la instalación se ha mejorado para proporcionar comentarios al usuario si la configuración de red y la configuración del firewall son incorrectas. Se han proporcionado cmdlets de diagnósticos adicionales para ayudarle a solucionar los problemas de red del dispositivo. Para obtener más información acerca de los nuevos cmdlets de diagnóstico utilizados para solucionar problemas, consulte el [artículo sobre la solución de problemas en la implementación](storsimple-troubleshoot-deployment.md).

## Problemas corregidos en la actualización 1

La tabla siguiente contiene un resumen de los problemas corregidos en esta actualización.

| N.º | Característica | Problema | Se aplica a un dispositivo físico | Se aplica a un dispositivo virtual |
|-----|---------|-------|---------------------------------|--------------------------------|
| 1 | Windows PowerShell para StorSimple | Cuando un usuario accedió de manera remota al dispositivo StorSimple mediante Windows PowerShell para StorSimple y, a continuación, inició al Asistente para la instalación, se produjo un bloqueo en cuanto se introdujo la IP de Data 0. El error se ha corregido en la actualización 1. | Sí | Sí |
| 2 | Restablecimiento de fábrica | En algunos casos, al realizar un restablecimiento de fábrica, el dispositivo StorSimple se bloqueó y apareció este mensaje: **El restablecimiento a los valores de fábrica está en curso (fase 8)**. Esto sucedía si se presionaba CTRL+C mientras el cmdlet estaba en curso. El error se ha corregido.| Sí | No |
| 3 | Restablecimiento de fábrica | Después de un error en el restablecimiento de fábrica de un controlador doble, se permitió continuar con el registro del dispositivo. Esto provocó una configuración no compatible del sistema. En la actualización 1, se muestra un mensaje de error y el registro se bloquea en los dispositivos en que se haya producido algún error al realizar un restablecimiento de fábrica. | Sí | No |
| 4 | Restablecimiento de fábrica | En algunos casos, se han generado alertas de discrepancia que son falsos positivos. Dejarán de generarse alertas de falta de coincidencia incorrectas en los dispositivos con la actualización 1. | Sí | No |
| 5 | Restablecimiento de fábrica | Si un restablecimiento de fábrica se interrumpió antes de finalizar, el dispositivo entró en modo de recuperación, lo que no permitió al usuario tener acceso a Windows PowerShell para StorSimple. El error se ha corregido. | Sí | No |
| 6 | Recuperación ante desastres | Se corrigió un error de recuperación ante desastres (DR) en el que se produciría un error de DR durante la detección de las copias de seguridad en el dispositivo de destino. | Sí | Sí |
| 7 | LED de supervisión | En determinados casos, los LED de supervisión de la parte trasera del dispositivo no indicaron que el estado es correcto. Se ha desactivado el LED azul. Los LED de DATA 0 y DATA 1 parpadeaban aun cuando las interfaces no se habían configurado. El problema se ha corregido y los LED de supervisión indican el estado correcto. | Sí | No |
| 8 | Interfaces de red | En las versiones anteriores, los dispositivos de StorSimple configurados con una puerta de enlace no enrutable podían quedarse sin conexión. En esta versión, la métrica de enrutamiento para Data 0 se ha realizado en los valores más bajos; por lo tanto, aunque otras interfaces de red están habilitadas para la nube, todo el tráfico de nube del dispositivo se enrutará a través de Data 0. | Sí | Sí | 
| 9 | Copias de seguridad | En la revisión de la actualización 1.1 (versión de software 6.3.9600.17521), se ha corregido un error que se daba en la actualización 1 (versión de software 6.3.9600.17491) y que causaba el fallo de las copias de seguridad a los 24 días. | Sí | Sí |

## Problemas conocidos de la actualización 1

En la tabla siguiente se proporciona un resumen de los problemas conocidos de esta versión.

| N.º | Característica | Problema | Comentarios/solución alternativa | Se aplica a un dispositivo físico | Se aplica a un dispositivo virtual |
|-----|---------|-------|----------------------------|----------------------------|---------------------------|
| 1 | Cuórum de disco | En raras ocasiones, si se desconecta la mayoría de los discos en el revestimiento de EBOD de un dispositivo 8600 y no se produce un cuórum de disco, el bloque de almacenamiento se desconectará. Seguirá desconectado incluso si se vuelven a conectar los discos. | Necesitará reiniciar el dispositivo. Si el problema persiste, póngase en contacto con el soporte técnico de Microsoft para conocer los pasos siguientes. | Sí | No |
| 2 | Identificador de controlador incorrecto | Cuando se realiza un reemplazo de controlador, el controlador 0 puede aparecer como controlador 1. Durante el reemplazo de controlador, cuando se carga la imagen desde el nodo del mismo nivel, el identificador de controlador puede mostrarse inicialmente como el identificador del controlador del mismo nivel. En raras ocasiones, este comportamiento también puede aparecer después del reinicio del sistema. | No se requiere ninguna acción del usuario. La situación se solucionará una vez completado el reemplazo del controlador. | Sí | No |
| 3 | Cuentas de almacenamiento | El uso del servicio de almacenamiento para eliminar la cuenta de almacenamiento es un escenario no admitido. Esto provocará una situación en la que no se pueden recuperar los datos de usuario. | Sí | Sí |
| 4 | Conmutación por error del dispositivo | No se admiten varias conmutaciones por error de un contenedor de volúmenes del mismo dispositivo de origen a diferentes dispositivos de destino. La conmutación por error de un único dispositivo inactivo a varios dispositivos hará que los contenedores de volúmenes del primer dispositivo conmutado por error pierdan la propiedad de los datos. Después de este tipo de conmutación por error, estos contenedores de volúmenes aparecerán o se comportarán de forma diferente cuando se visualizan en el Portal de administración. | | Sí | No |
| 5 | Instalación | Durante la instalación del adaptador de StorSimple para SharePoint, deberá proporcionar una dirección IP de dispositivo para que la instalación finalice correctamente. | | Sí | No |
| 6 | Proxy web | Si la configuración de proxy web tiene HTTPS como protocolo especificado, la comunicación de dispositivo a servicio se verá afectada y el dispositivo se desconectará. También se generarán paquetes de compatibilidad en el proceso, que consumen muchos recursos en el dispositivo. | Asegúrese de que la dirección URL del proxy web tiene HTTP como protocolo especificado. Para obtener más información, vaya a [Configurar el proxy web para el dispositivo](storsimple-configure-web-proxy.md). | Sí | No |
| 7 | Proxy web | Si configura y habilita el proxy web en un dispositivo registrado, será necesario reiniciar el controlador activo en el dispositivo. | | Sí | No |
| 8 | Latencia alta de la nube y alta carga de trabajo de E/S | Cuando el dispositivo StorSimple encuentra una combinación de latencias muy altas de la nube (del orden de segundos) y alta carga de trabajo de E/S, los volúmenes del dispositivo pasan a un estado degradado y las operaciones de E/S pueden fallar con el error «el dispositivo no está listo». | Necesitará reiniciar los controladores de dispositivo de forma manual o realizar una conmutación por error del dispositivo para recuperarse de esta situación. | Sí | No |
| 9 | Azure PowerShell | Cuando se usa el cmdlet de StorSimple **Get-AzureStorSimpleStorageAccountCredential | Select-Object -First 1 -Wait** para seleccionar el primer objeto y crear un nuevo objeto **VolumeContainer**, el cmdlet devuelve todos los objetos. | Escriba el cmdlet entre paréntesis, como se indica a continuación: **(Get-Azure-StorSimpleStorageAccountCredential) | Select-Object -First 1 -Wait** | Sí | Sí |
| 10| Migración | Cuando se pasan varios contenedores de volúmenes para la migración, el ETA de la copia de seguridad más reciente solo es preciso en el primer contenedor de volúmenes. Además, la migración paralela se iniciará después de que se hayan migrado las cuatro primeras copias de seguridad del primer contenedor de volúmenes. | Se recomienda migrar los contenedores de volúmenes de uno en uno. | Sí | No |
| 11| Migración | Después de la restauración, los volúmenes no se agregan a la directiva de copia de seguridad ni al grupo de discos virtuales. | Para crear copias de seguridad, será preciso agregar estos volúmenes a una directiva de copia de seguridad. | Sí | Sí |
| 12| Migración | Una vez completada la migración, el dispositivo de las series 5000/7000 no debe tener acceso a los contenedores de datos migrados. | Cuando la migración finaliza y se envía, se recomienda eliminar los contenedores de datos migrados. | Sí | No |
| 13| Clonación y recuperación ante desastres | Los dispositivos StorSimple con la actualización 1 no se pueden clonar. Tampoco se puede realizar la recuperación ante desastres en un dispositivo que ejecute el software anterior a la actualización 1. | Para que se puedan realizar estas operaciones, tendrá que implementar la actualización 1 en el dispositivo de destino | Sí | Sí |
| 14 | Migración | En los dispositivos de las series 5000-7000, se puede producir un error en la copia de seguridad de la configuración para la migración cuando haya grupos de volúmenes sin volúmenes asociados. | Elimine todos los grupos de volúmenes vacíos sin volúmenes asociados y vuelva a intentar realizar la copia de seguridad de la configuración.| Sí | No |

## Actualizaciones del dispositivo físico en la actualización 1

Cuando aplique estas actualizaciones en un dispositivo físico, la versión del software cambiará a 6.3.9600.17521.

## Controlador SCSI conectado en serie (SAS) y actualizaciones de firmware en la actualización 1

Esta versión actualiza el controlador y el firmware del controlador SAS del dispositivo físico. También actualiza el firmware de disco del dispositivo.
 
- Para obtener más información sobre la actualización del controlador SAS, consulte [Actualización 1 para los controladores SAS LSI del dispositivo Microsoft Azure StorSimple](https://support.microsoft.com/kb/3043005). 

- Para obtener más información sobre la actualización de firmware, consulte [Actualización 1 del firmware del dispositivo Microsoft Azure StorSimple](https://support.microsoft.com/kb/3063414).

- Para obtener más información sobre la actualización del firmware del disco, consulte [Actualización 1 del firmware del disco para el dispositivo de Microsoft Azure StorSimple](https://support.microsoft.com/kb/3063416).
 
## Actualizaciones del dispositivo virtual en la actualización 1

No se puede aplicar esta actualización al dispositivo virtual. Sin embargo, los dispositivos virtuales creados después del 10 de julio estarán automáticamente en esta versión.

## Pasos siguientes

- [Instalación de la actualización 1 en el dispositivo](storsimple-install-update-1.md).
 

<!---HONumber=August15_HO8-->