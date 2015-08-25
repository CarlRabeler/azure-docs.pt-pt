<properties 
   pageTitle="Cambiar los modos del dispositivo StorSimple"
   description="Obtenga información sobre los distintos modos del dispositivo StorSimple y cómo cambiarlos."
   services="storsimple"
   documentationCenter=""
   authors="alkohli"
   manager="carolz"
   editor="tysonn" />
<tags 
   ms.service="storsimple"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="07/30/2015"
   ms.author="alkohli" />

# Modos del dispositivo StorSimple

En este artículo se proporciona una breve descripción de los distintos modos en que puede funcionar el dispositivo StorSimple. El dispositivo StorSimple puede funcionar en tres modos: normal, de mantenimiento y de recuperación.

Después de leer este artículo, sabrá acerca de:

- los modos del dispositivo StorSimple.
- cómo determinar el modo en que está establecido el dispositivo StorSimple.
- cómo cambiar del modo normal al modo de mantenimiento y *viceversa*.


Las tareas de administración mencionadas solo pueden realizarse a través de la interfaz de Windows PowerShell del dispositivo StorSimple.

## Acerca de los modos del dispositivo StorSimple.

El dispositivo StorSimple puede funcionar en modo normal, de mantenimiento o de recuperación. A continuación se describe brevemente cada uno de estos modos.

### Modo Normal

Está definido como modo de funcionamiento normal de un dispositivo StorSimple totalmente configurado. De manera predeterminada, su dispositivo debería estar en modo normal.

### Modo Mantenimiento

A veces puede ser necesario colocar el dispositivo StorSimple en el modo de mantenimiento. Este modo le permite realizar tareas de mantenimiento en el dispositivo e instalar actualizaciones potencialmente problemáticas, como las relacionadas con el firmware del disco.

Solo puede poner el sistema en modo de mantenimiento por medio de Windows PowerShell para StorSimple. En este modo, todas las solicitudes de E/S se interrumpen. También se detienen servicios como la memoria de acceso aleatorio no volátil (NVRAM) o el servicio de Cluster Server. Ambos controladores se reinician al entrar o salir de este modo. Al salir del modo de mantenimiento, todos los servicios se reanudan y deben funcionar correctamente. Esta operación puede tardar unos minutos.

>[AZURE.NOTE]**El modo Mantenimiento solo se admite en un dispositivo que funcione correctamente. No se admite en un dispositivo en el que uno o ambos controladores no funcionan.**</br>

### Modo Recuperación

El modo Recuperación puede describirse como el "Modo seguro de Windows con compatibilidad de red". El modo Recuperación avisa al equipo de soporte técnico de Microsoft y le permite realizar diagnósticos en el sistema. El objetivo principal del modo de recuperación es recuperar los registros del sistema.

Si el sistema entra en modo de recuperación, debe ponerse en contacto con el soporte técnico de Microsoft para obtener asistencia. Para obtener más información, vaya a [Contactar al soporte técnico de Microsoft](storsimple-contact-microsoft-support.md).

>[AZURE.NOTE]**No puede poner el dispositivo en modo de recuperación. Si el dispositivo está en mal estado, el modo de recuperación intenta llevar al dispositivo a un estado en el que el personal de soporte técnico de Microsoft pueda examinarlo.**

## Determinar el modo del dispositivo StorSimple

Para determinar el modo del dispositivo, lleve a cabo los siguientes pasos:

1. Inicie sesión en la consola serie del dispositivo siguiendo los pasos detallados en [Uso de PuTTY para conectarse a la consola de serie del dispositivo](https://msdn.microsoft.com/library/azure/dn757808.aspx).
2. Mire el mensaje de pancarta del menú de la consola serie del dispositivo. Este mensaje indica de forma explícita si el dispositivo está en modo de mantenimiento o de recuperación. Si el mensaje no contiene información específica relativa al modo del sistema, el dispositivo está en modo normal.

## Cambiar el modo del dispositivo StorSimple 

Puede colocar el dispositivo StorSimple en modo de mantenimiento (desde el modo normal) para realizar tareas de mantenimiento o instalar las actualizaciones del modo de mantenimiento. Lleve a cabo los siguientes procedimientos para entrar o salir del modo de mantenimiento.

> [AZURE.IMPORTANT]Antes de entrar en modo de mantenimiento, verifique que ambos controladores del dispositivo funcionen correctamente; para ello, vaya a **Estado de hardware** en la página **Mantenimiento** del Portal de administración. Si el controlador no es correcto, póngase en contacto con el servicio de soporte técnico de Microsoft para conocer los pasos siguientes. Para obtener más información, vaya a [Contacto con el soporte técnico de Microsoft](storsimple-contact-microsoft-support.md).

#### Para acceder al modo de mantenimiento

1. Inicie sesión en la consola serie del dispositivo siguiendo los pasos detallados en [Uso de PuTTY para conectarse a la consola de serie del dispositivo](https://msdn.microsoft.com/library/azure/dn757808.aspx).

1. En el menú de la consola serie, seleccione la opción 1, **Iniciar sesión con acceso completo**. Cuando se le solicite, proporcione la **contraseña de administrador de dispositivo**. La contraseña predeterminada es: `Password1`.

1. En el símbolo del sistema, escriba

	`Enter-HcsMaintenanceMode`

1. Verá un mensaje de advertencia que indica que el modo de mantenimiento interrumpe todas las solicitudes de E/S y detiene la conexión al Portal de administración. También se le solicitará la confirmación. Escriba **Y** para acceder al modo de mantenimiento.

1. Ambos controladores se reiniciarán. Una vez completado el reinicio, aparecerá otro mensaje que indica que el dispositivo está en modo de mantenimiento.


#### Para salir del modo de mantenimiento

1. Inicie sesión en la consola serie del dispositivo. En el mensaje de pancarta, verifique que el dispositivo esté en el modo Mantenimiento.

2. En el símbolo del sistema, escriba:

	`Exit-HcsMaintenanceMode`

1. Aparecerán un mensaje de advertencia y un mensaje de confirmación. Escriba **Y** para salir del modo de mantenimiento.

1. Ambos controladores se reiniciarán. Una vez completado el reinicio, aparecerá otro mensaje que indica que el dispositivo está en modo normal.


## Pasos siguientes

Aprenda a [aplicar las actualizaciones del modo normal y del de mantenimiento](storsimple-update-device.md) en el dispositivo StorSimple.

<!---HONumber=August15_HO6-->