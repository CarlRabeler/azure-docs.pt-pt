<properties 
   pageTitle="Instalar el dispositivo StorSimple 8100"
   description="Aquí se describe cómo desempaquetar, montar en bastidor y colocar los cables del dispositivo StorSimple 8100."
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
   ms.date="07/20/2015"
   ms.author="alkohli" />

# Instalar el dispositivo StorSimple 8100

## Información general

Su Microsoft Azure StorSimple 8100 es un dispositivo de montaje en bastidor de una sola carcasa.

En este tutorial se explica cómo desempaquetar, montar en bastidor y conectar los cables del hardware del dispositivo StorSimple 8100 antes de configurar el software de StorSimple.

## Desempaquete el dispositivo StorSimple 8100

En los pasos siguientes se proporcionan instrucciones claras y detalladas sobre cómo desempaquetar el dispositivo de almacenamiento StorSimple 8100. Este dispositivo se suministra en una única caja.

### Preparación para desempaquetar el dispositivo

Antes de desempaquetar el dispositivo, revise la información siguiente.

>[AZURE.WARNING]![icono de peso elevado](./media/storsimple-8100-hardware-installation/HCS_HeavyWeight_Icon.png)
>
> 1. Asegúrese de que haya dos personas disponibles para administrar el peso del dispositivo si lo está manejando de forma manual. Un dispositivo totalmente configurado puede pesar hasta 32 kg.
>
> 2. Coloque la caja en una superficie plana y nivelada.

A continuación, complete los pasos siguientes para desempaquetar el dispositivo.

#### Para desempaquetar el dispositivo

1. Compruebe si la caja y la espuma del embalaje presentan golpes, cortes, daños por agua o cualquier otro daño evidente. Si la caja o el embalaje están muy dañados, no abra la caja. Póngase en contacto con el soporte técnico de Microsoft para ayudarle a determinar si el dispositivo está en buen estado. 

2. Desempaquete la caja. En la siguiente imagen se muestra el dispositivo StorSimple de Azure desempaquetado.

     ![Desempaquetar el dispositivo de almacenamiento](./media/storsimple-8100-hardware-installation/HCSUnpackyour2Udevice.png)

    **Figura 1: Vista del dispositivo de almacenamiento desempaquetado**

     Etiqueta | Descripción 
     ----- | -------------
     1 | Caja de embalaje
     2 | Espuma inferior
     3 | Dispositivo
     4 | Espuma superior
     5 | Caja de accesorios

3. Después de desempaquetar la caja, asegúrese de que dispone de:

   - 1 único dispositivo contenedor
   - 2 cables de alimentación
   - 1 cable Ethernet cruzado
   - 2 cables de consola serie
   - 1 convertidor serie a USB para el acceso en serie
   - 1 destornillador T10 seguro
   - 4 adaptadores de QSFP a SFP+ únicos
   - 1 kit de montaje en bastidor (2 rieles laterales con hardware de montaje)
   - Documentación de introducción

    Si no recibió alguno de los elementos enumerados anteriormente, póngase en contacto con el soporte técnico de Microsoft.

El paso siguiente es el montaje en bastidor del dispositivo.

## Montaje en bastidor del dispositivo StorSimple 8100

Siga los pasos siguientes para instalar el dispositivo de almacenamiento StorSimple 8100 en un bastidor de 19 pulgadas estándar con postes delanteros y traseros. El dispositivo StorSimple 8100 tiene un único alojamiento principal.

La instalación consta de varios pasos, cada uno de los cuales se explica en los procedimientos siguientes.

### Preparación de la ubicación

El dispositivo debe instalarse en un bastidor estándar de 19 pulgadas con postes delanteros y traseros. Utilice el procedimiento siguiente para preparar la instalación en bastidor.

#### Para preparar el sitio para la instalación en bastidor

1. Asegúrese de colocar el dispositivo de forma segura sobre una superficie de trabajo plana, estable y nivelada (o similar).

2. Compruebe que la ubicación en la que desea efectuar la instalación dispone de alimentación de CA estándar de una fuente independiente o una unidad de distribución de energía (PDU) en bastidor con una fuente de alimentación ininterrumpida (UPS).

3. Asegúrese de que haya disponible una ranura 2U en el bastidor en el que desea montar el dispositivo.

>[AZURE.WARNING]![icono de peso elevado](./media/storsimple-8100-hardware-installation/HCS_HeavyWeight_Icon.png)
> 
> Asegúrese de que haya dos personas disponibles para administrar el peso del dispositivo si está instalando el dispositivo de forma manual. Un receptáculo totalmente montado puede pesar hasta 32 kg.

### Requisitos previos del bastidor

El receptáculo 8100 está diseñado para instalarse en un armario de bastidor de 19 pulgadas estándar con:

- Una profundidad mínima de 27,84 pulgadas de poste a poste del bastidor.
- Peso máximo de 32 kg para el dispositivo
- Contrapresión máxima de 5 pascales (medidor de agua de 0,5 mm).

### Kit de guías de montaje en bastidor

Se proporciona un conjunto de guías de montaje para utilizar con el armario de bastidor de 19 pulgadas. Las guías se han probado para soportar el peso máximo del receptáculo. Estas guías también permitirán la instalación de varios receptáculos sin pérdida de espacio en el bastidor.

#### Para instalar el dispositivo en las guías

1. Con el receptáculo en la superficie de trabajo, quite los topes de las bridas delantera derecha e izquierda extrayendo los topes. Los topes de las bridas simplemente encajan en las bridas.

2. Normalmente, las guías vienen instaladas de fábrica. En caso contrario, instale las guías del lado izquierdo y derecho en los laterales del chasis del receptáculo. Estas se instalan mediante seis tornillos métricos en cada lado. Para ayudarle con la orientación, las guías disponen de las marcas **LH – Front** y **RH – Front**, y el extremo que se fija en la parte trasera del receptáculo tiene un extremo ahusado.<br/>

    ![Fijación de las guías al chasis del receptáculo](./media/storsimple-8100-hardware-installation/HCSAttachingRailSlidestoEnclosureChassis.png)

   **Figura 2: Fijación de guías a los laterales del receptáculo**

    Label | Description
    ----- | -----------
    1     | M 3x4 button-head screws
    2     | Chassis slides

3. Instale los ensamblajes de las guías del lado izquierdo y derecho en los miembros verticales del armario del bastidor. Los soportes presentan las marcas **LH**, **RH** y **This side up** para guiarle a la hora de efectuar la orientación correcta.

4. Busque los pasadores de la guía en la parte frontal y trasera del soporte de la guía. Extienda la guía para ajustarla entre los postes del bastidor e inserte los pasadores en los orificios del miembro vertical del poste del bastidor delantero y trasero. Asegúrese de que el soporte de la guía esté nivelado.

5. Use dos de los tornillos métricos suministrados métricos para fijar el soporte de la guía en los miembros verticales del bastidor. Utilice un tornillo en la parte frontal y otro en la parte posterior.

6. Repita estos pasos con el otro soporte de la guía.<br/>

     ![Fijación de las guías al armario del chasis](./media/storsimple-8100-hardware-installation/HCSAttachingRailSlidestoRackCabinet.png)

    **Figura 3: Fijación de soportes de guía en el bastidor**

     Etiqueta | Descripción
     ----- | -----------
     1 | Tornillo de fijación
     2 | Tornillo del poste del bastidor delantero de orificio cuadrado
     3 | Pasadores de ubicación de la guía delantera izquierda
     4 | Tornillo de fijación
     5 | Pasadores de ubicación de la guía delantera derecha

### Montaje del dispositivo en el bastidor

Con las guías de bastidor que acaba de instalar, realice los pasos siguientes para montar el dispositivo en el bastidor.

#### Para montar el dispositivo

1. Con la ayuda de otra persona, levante el receptáculo y alinéelo con las guías del bastidor. 

2. Inserte cuidadosamente el dispositivo en las guías y, a continuación, empuje el dispositivo completamente en el armario del bastidor.<br/>

    ![Inserción del dispositivo en el bastidor](./media/storsimple-8100-hardware-installation/HCSInsertingDeviceintheRack.png)

    **Figura 4: Montaje del dispositivo en el bastidor**

3. Fije el receptáculo en el bastidor mediante la instalación de un tornillo de cabeza compatible con un tornillo de cabeza Phillips proporcionado a través de cada brida, izquierda y derecha.

4. Presione los topes de las bridas en su posición para ajustarlos.<br/>

     ![Instalación de los topes de las bridas](./media/storsimple-8100-hardware-installation/HCSInstallingFlangeCaps.png)
 
    **Figura 5: Instalación de los topes de las bridas**

     Etiqueta | Descripción
     ----- | -----------
     1 | Tornillo de fijación del receptáculo

El siguiente paso es la colocación de los cables de alimentación, red y acceso de serie del dispositivo.

## Instalación de cables del dispositivo StorSimple 8100

En los siguientes procedimientos se explica cómo pasar los cables del dispositivo StorSimple 8100 de alimentación, red y conexiones en serie.

### Requisitos previos

Antes de comenzar a pasar los cables del dispositivo, necesitará:

- Desempaquetar por completo el dispositivo de almacenamiento

- 2 cables de alimentación suministrados con el dispositivo

- Acceso a 2 unidades de distribución de energía (recomendado).

- Cables de red

- Cables serie suministrados

- Convertidor USB serie con el controlador apropiado instalado en su equipo (si es necesario)

- Los adaptadores QSFP a SFP + para su uso con interfaces de red de 10 GbE

- [Transceptores, cables y conmutadores de interfaces de red 10 GbE compatibles](https://msdn.microsoft.com/library/azure/dn891474.aspx)


### Cables de alimentación

El dispositivo incluye módulos de alimentación y de refrigeración (PCM) redundantes. Ambos PCM deben estar instalados y conectados a diferentes fuentes de alimentación para garantizar una alta disponibilidad.

Realice los pasos siguientes para pasar los cables de alimentación del dispositivo.

#### Instalación de los cables de alimentación

1. Asegúrese de que el interruptor de alimentación de cada uno de los PCM se encuentran en la posición OFF.

2. Conecte los cables de alimentación en ambos PCM en el receptáculo principal.

3. Conecte los cables de alimentación a las unidades de distribución de energía (PDU) del bastidor tal y como se muestra en la siguiente ilustración. Asegúrese de que los dos PCM utilicen fuentes de alimentación independientes.

4. Gire el interruptor de alimentación de ambos PCM a la posición ON para encender el sistema.

    >[AZURE.NOTE]Para garantizar una alta disponibilidad del sistema, debe cumplir estrictamente el esquema de cableado de potencia que se muestra en el siguiente diagrama de cableado.

    ![Colocación del cable de alimentación del dispositivo 2U](./media/storsimple-8100-hardware-installation/HCSCableYour2UDeviceforPower.png)

    **Figura 6: Cables de alimentación del dispositivo**

     Etiqueta | Descripción
     ----- | -----------
     1 | PCM 0
     2 | Controlador 1
     3 | Controlador 0
     4 | PCM 1
     5 | PDU

### Cables de red

El dispositivo es una configuración de dispositivo activo/en espera: en un momento dado, un módulo de controlador está activo y procesando todas las operaciones de disco y red mientras el otro módulo del controlador está en espera. Si se produce un error en un controlador, el controlador en modo de espera se activa inmediatamente y continúa todas las operaciones de discos y de red.

Para admitir esta conmutación por error de controlador redundante, necesitará pasar los cables de red del dispositivo del modo descrito en los pasos siguientes.

#### Colocación de cables de conexión de red

1. El dispositivo tiene seis interfaces de red en cada controlador: cuatro puertos Ethernet de 1 Gbps y dos de 10 Gbps. Identifique los distintos puertos de datos de la placa posterior del dispositivo.

    ![Panel posterior del dispositivo 8100](./media/storsimple-8100-hardware-installation/HCSBackplaneof2UDevicewithPortsLabeled.jpg)

    **Figura 7: Parte posterior de dispositivo en la que se muestran los puertos de datos**
 
     Etiqueta | Descripción
     ------- | -----------
     0,1,4,5 | Interfaces de red de 1 GbE
     2,3 | Interfaces de red de 10 GbE
     6 | Puertos serie

2. Para lograr una alta disponibilidad, el dispositivo requiere un mínimo de dos conexiones para cada controlador.
    1. El puerto DATA 0 se habilita y configura automáticamente mediante la consola serie del dispositivo. Además del puerto DATA 0, también es necesario configurar otro puerto de datos a través del Portal de administración. 
    2. Identifique interfaces de red idénticas en cada controlador. Por ejemplo, si decide conectar los puertos DATA 0 y DATA 3 para uno de los controladores, necesitará conectar los puertos DATA 0 y DATA 3 correspondientes en el otro controlador. 

3. Para lograr una alta disponibilidad, asegúrese de conectar:
    1. Interfaces idénticas en cada controlador de la red relevante para garantizar la disponibilidad si se produce un error en el controlador.
    2. Interfaces de cada controlador a al menos dos conmutadores diferentes para garantizar la disponibilidad si se produce un error de conmutador.
    3. El puerto DATA 0 a la LAN principal (red con acceso a Internet). Los demás puertos de datos pueden conectarse al segmento de la LAN SAN/iSCSI (VLAN) de la red, dependiendo del rol deseado.

    Como mínimo, configure una interfaz de red para el acceso a la nube y otro para iSCSI. Para obtener una alta disponibilidad y rendimiento, configure dos pares de interfaces de red en cada controlador. Consulte el siguiente diagrama de cableado de red. (La configuración de red mínima se muestra mediante líneas azules continuas. Para obtener un alto rendimiento y disponibilidad, la configuración adicional requerida se muestra mediante líneas de puntos).

    ![Colocación del cable de red del dispositivo 2U](./media/storsimple-8100-hardware-installation/HCSCableYour2UDeviceforNetwork.png)

    **Figura 8: Cables de red del dispositivo**

    Etiqueta | Descripción
    ----- | -----------
     Encontrará | LAN con acceso a Internet
     B | Controlador 0
     C | PCM 0
     D | Controlador 1
     E | PCM 1
     F, G | Hosts
     0-5 | Interfaces de red
   
### Cableado del puerto serie

Realice los pasos siguientes para pasar el cable del puerto serie.

#### Colocación de los cables de conexión en serie

1. El dispositivo tiene un puerto serie en cada controlador que se identifica mediante un icono de una llave inglesa. Consulte la figura 7 para buscar los puertos serie en la placa posterior del dispositivo. 

2. Identifique el controlador activo en la placa posterior del dispositivo. Un LED que parpadeará en azul indica que el controlador está activo.

3. Utilice los cables serie proporcionados (si es necesario, el convertidor de USB a serie de su equipo portátil) y conecte la consola o el equipo (con la emulación de terminales en el dispositivo) al puerto serie del controlador activo.

4. Instale los controladores de serie a USB (incluidos con el dispositivo) en el equipo.

5. Configure la conexión serie del modo indicado a continuación: 115,200 baudios, 8 bits de datos, 1 bit de parada, sin paridad y el control del flujo establecido en Ninguno.

6. Presione INTRO en la consola para comprobar que la conexión funciona. Debería aparecer un menú de consola serie.

>[AZURE.NOTE]**Administración de Lights-Out**: cuando el dispositivo está instalado en un centro de datos remoto o en una sala de equipos con acceso limitado, asegúrese de que las conexiones serie a ambos controladores estén siempre conectadas a un conmutador de consola serie o un equipo similar. Esto permite el control remoto de fuera de banda y las operaciones de soporte si hay interrupciones de red o errores inesperados.

Ahora su dispositivo dispondrá de los cables de alimentación, de acceso a la red y de conectividad serie. El siguiente paso es configurar el software en el dispositivo.

## Pasos siguientes

Ahora está listo para [implementar y configurar el dispositivo StorSimple local](storsimple-deployment-walkthrough.md)
 

<!---HONumber=August15_HO6-->