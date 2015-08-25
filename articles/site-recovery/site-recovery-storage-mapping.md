<properties
	pageTitle="Asignación del almacenamiento de Site Recovery"
	description="Azure Site Recovery coordina la replicación, la conmutación por error y la recuperación de máquinas virtuales ubicadas localmente en Azure o en un sitio local secundario."
	services="site-recovery"
	documentationCenter=""
	authors="rayne-wiselman"
	manager="jwhit"
	editor=""/>

<tags
	ms.service="site-recovery"
	ms.devlang="na"
	ms.topic="get-started-article"
	ms.tgt_pltfrm="na"
	ms.workload="storage-backup-recovery"
	ms.date="05/08/2015"
	ms.author="raynew"/>


# Asignación del almacenamiento de Site Recovery


Azure Site Recovery contribuye a su estrategia de continuidad de negocio y recuperación ante desastres (BCDR) mediante la coordinación de la replicación, la conmutación por error y la recuperación de máquinas virtuales y servidores virtuales. Obtenga información acerca de los escenarios de implementación posibles en [Información general sobre Site Recovery](site-recovery-overview.md).


## Información acerca de este artículo

La asignación del almacenamiento es un elemento importante de la implementación de Site Recovery. Garantiza que haga un uso óptimo del almacenamiento. Este artículo describe la asignación de almacenamiento y proporciona un par de ejemplos que le ayudarán a entender cómo funciona la asignación de almacenamiento.


Publique cualquier pregunta en el [Foro de servicios de recuperación de Azure](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).

## Información general

La manera en que se establece la asignación del almacenamiento depende de su escenario de implementación de Site Recovery.



- **De local a local (replicación con la réplica de Hyper-V)**: asigna clasificaciones de almacenamiento en servidores VMM de origen y de destino para hacer lo siguiente:

	- **Identificación del almacenamiento de destino para máquinas virtuales de réplica**: las máquinas virtuales se replicará a un destino de almacenamiento (recurso compartido de SMB o volúmenes compartidos de clúster (CSV)) que elija.
	- **Colocación de máquinas virtuales de réplica**: la asignación de almacenamiento se usa para colocar máquinas virtuales de réplica en los servidores host de Hyper-V de forma óptima. Las máquinas virtuales de réplica se colocarán en hosts que pueden tener acceso a las redes de almacenamiento asignadas.
	- **Sin asignación de redes**: si no configura la asignación de almacenamiento, las máquinas virtuales se replicarán en la ubicación de almacenamiento predeterminada especificada en el servidor host de Hyper-V asociado a la máquina virtual de réplica.

- **De local a local (replicación con SAN)**: asigna grupos de matrices de almacenamiento en un servidor VMM de origen y de destino para hacer lo siguiente:
	- **Identificación de los grupos de almacenamiento de destino**: la asignación de almacenamiento garantiza que las LUN de un grupo de replicación se replican en el bloque de almacenamiento de destino asignado.



## Clasificaciones de almacenamiento

Asigne entre las clasificaciones de almacenamiento en servidores VMM de origen y destino, o bien en un único servidor VMM si el mismo servidor VMM administra dos sitios. Cuando se configura correctamente la asignación y la replicación está habilitada, un disco duro virtual de una máquina virtual en la ubicación principal se replicará el en almacenamiento de la ubicación de destino asignada. Observe lo siguiente:

- Las clasificaciones de almacenamiento deben estar disponibles para los grupos host ubicados en nubes de origen y de destino.
- - Las clasificaciones no necesitan tener el mismo tipo de almacenamiento. Por ejemplo, puede asignar una clasificación de origen que contenga recursos compartidos de SMB a una clasificación de destino que contenga CSV
- Obtenga más información en [Creación de clasificaciones de almacenamiento en VMM](https://technet.microsoft.com/library/gg610685.aspx).

## Ejemplo

Si las clasificaciones están configuradas correctamente en VMM, al seleccionar el servidor VMM de origen y de destino durante la asignación de almacenamiento, se mostrarán las clasificaciones de origen y de destino. Este es un ejemplo de recursos compartidos de archivos de almacenamiento y clasificaciones para una organización con dos ubicaciones en Nueva York y Chicago.

**Ubicación** | **Servidor VMM** | **Recurso compartido de archivos (origen)** | **Clasificación (origen)** | **Asignado a** | **Recurso compartido de archivos (destino)**
---|---|--- |---|---|---
Nueva York | VMM\_Source| SourceShare1 | GOLD | GOLD\_TARGET | TargetShare1
 | | SourceShare2 | SILVER | SILVER\_TARGET | TargetShare2
 | | SourceShare3 | BRONZE | BRONZE\_TARGET | TargetShare3
Chicago | VMM\_Target | | GOLD\_TARGET | No asignado |
| | | SILVER\_TARGET | No asignado |
 | | | BRONZE\_TARGET | No asignado

Se configuran en la pestaña **Almacenamiento del servidor** en la página **Recursos** del portal de Site Recovery.

![Configurar la asignación de almacenamiento](./media/site-recovery-storage-mapping/StorageMapping1.png)

En este ejemplo: - si una máquina virtual de réplica se crea para cualquier máquina virtual en almacenamiento GOLD (SourceShare1), se replicará en un almacenamiento de GOLD\_TARGET (TargetShare1). - Cuando se crea una máquina virtual de réplica para cualquier máquina virtual en almacenamiento SILVER (SourceShare2), se replicarán en un almacenamiento SILVER\_TARGET (TargetShare2) y así sucesivamente.

Los recursos compartidos de archivo reales y sus clasificaciones asignadas en VMM serían como se muestra a continuación.

![Clasificaciones de almacenamiento en VMM](./media/site-recovery-storage-mapping/StorageMapping2.png)

## Múltiples cuentas de almacenamiento

Si la clasificación de destino se asigna a varios recursos compartidos de SMB o CSV, se selecciona automáticamente la ubicación de almacenamiento óptima cuando la máquina virtual está protegida. Si no hay disponible ningún almacenamiento de destino adecuado con la clasificación especificada, la ubicación de almacenamiento predeterminada especificada en el host de Hyper-V se utiliza para colocar los discos duros virtuales de réplica.

En la tabla siguiente se muestra cómo están configurados la clasificación de almacenamiento y los volúmenes compartidos de clúster en nuestro ejemplo.

**Ubicación** | **Clasificación** | **Almacenamiento asociada**
---|---|---
Nueva York | GOLD | <p>C:\\ClusterStorage\\SourceVolume1</p><p>\\FileServer\\SourceShare1</p>
 | SILVER | <p>C:\\ClusterStorage\\SourceVolume2</p><p>\\FileServer\\SourceShare2</p>
Chicago | GOLD\_TARGET | <p>C:\\ClusterStorage\\TargetVolume1</p><p>\\FileServer\\TargetShare1</p>
 | SILVER\_TARGET| <p>C:\\ClusterStorage\\TargetVolume2</p><p>\\FileServer\\TargetShare2</p>

Esta tabla resume el comportamiento cuando se habilita la protección para las máquinas virtuales (VM1 - VM5) en este entorno de ejemplo.

**Máquina virtual** | **Almacenamiento de origen** | **Clasificación de origen** | **Almacenamiento de destino asignado**
---|---|---|---
VM1 | C:\\ClusterStorage\\SourceVolume1 | GOLD | <p>C:\\ClusterStorage\\SourceVolume1</p><p>\\\\FileServer\\SourceShare1</p><p>Both GOLD\_TARGET</p>
VM2 | \\FileServer\\SourceShare1 | GOLD | <p>C:\\ClusterStorage\\SourceVolume1</p><p>\\FileServer\\SourceShare1</p> <p>Both GOLD\_TARGET</p>
VM3 | C:\\ClusterStorage\\SourceVolume2 | SILVER | <p>C:\\ClusterStorage\\SourceVolume2</p><p>\\FileServer\\SourceShare2</p>
VM4 | \\FileServer\\SourceShare2 | SILVER |<p>C:\\ClusterStorage\\SourceVolume2</p><p>\\FileServer\\SourceShare2</p><p>Los dos SILVER\_TARGET</p>
VM5 | C:\\ClusterStorage\\SourceVolume3 | N/D | No hay ninguna asignación, por lo que se utiliza la ubicación de almacenamiento predeterminada del host de Hyper-V

## Pasos siguientes

Ahora que tiene una mejor comprensión de la asignación del almacenamiento, lea las [prácticas recomendadas](site-recovery-best-practices.md) para preparar la implementación.
 

<!---HONumber=August15_HO6-->