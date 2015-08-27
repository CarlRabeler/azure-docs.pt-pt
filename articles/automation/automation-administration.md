<properties 
   pageTitle="Administración de Automatización de Azure"
   description="Este artículo contiene varios temas para administrar un entorno de Automatización de Azure. Actualmente incluye la retención de datos y las copias de seguridad de Automatización de Azure."
   services="automation"
   documentationCenter=""
   authors="bwren"
   manager="stevenka"
   editor="tysonn" />
<tags 
   ms.service="automation"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="07/06/2015"
   ms.author="bwren" />

# Administración de Automatización de Azure

Este artículo contiene varios temas para administrar un entorno de Automatización de Azure.

## Retención de datos

Cuando elimina un recurso en Automatización de Azure, se conserva durante 90 días para fines de auditoría antes de quitarlo de manera permanente. Durante este tiempo, no es posible ver ni usar el recurso. Esta directiva también se aplica a recursos que pertenecen a una cuenta de automatización eliminada.

Automatización de Azure elimina automáticamente y quita de manera permanente los trabajos que tengan más de 90 días.

La tabla siguiente resume la directiva de retención para distintos recursos.

|Datos|Directiva|
|:---|:---|
|Cuentas|Se quitan de manera permanente 90 días después de que un usuario elimina la cuenta.|
|Recursos|Se quitan de manera permanente 90 días después de que un usuario elimina el recurso o 90 días después de que un usuario elimina la cuenta que contiene el recurso.|
|Módulos|Se quitan de manera permanente 90 días después de que un usuario elimina el módulo o 90 días después de que un usuario elimina la cuenta que contiene el módulo.|
|Runbooks|Se quitan de manera permanente 90 días después de que un usuario elimina el recurso o 90 días después de que un usuario elimina la cuenta que contiene el recurso.|
|Trabajos|Se eliminan y quitan de manera permanente 90 días después de la última modificación. Esto puede ser después de completar, detener o suspender el trabajo.|

La directiva de retención se aplica a todos los usuarios y, por el momento, no se puede personalizar.

## Copia de seguridad de Automatización de Azure

Cuando se elimina una cuenta de automatización en Microsoft Azure, se eliminan todos los objetos de la cuenta, incluidos runbooks, módulos, configuraciones, trabajos y recursos. No es posible recuperar los objetos una vez eliminada la cuenta. Puede usar la información siguiente para crear una copia de seguridad de los contenidos de la cuenta de automatización antes de eliminarla.

### Runbooks

Puede exportar los runbooks a archivos de script con el Portal de administración de Azure o el cmdlet [Get-AzureAutomationRunbookDefinition](https://msdn.microsoft.com/library/dn690269.aspx) en Windows PowerShell. Es posible importar estos archivos de script a otra cuenta de automatización, tal como se describe en [Creación o importación de un runbook](https://msdn.microsoft.com/library/dn643637.aspx).


### Módulos de integración

No es posible exportar módulos de integración desde Automatización de Azure. Debe asegurarse de que se encuentren disponibles fuera de la cuenta de automatización.

### Recursos

No es posible exportar [recursos](https://msdn.microsoft.com/library/dn939988.aspx) desde Automatización de Azure. Si utiliza el Portal de administración de Azure, debe anotar los detalles de las variables, las credenciales, los certificados, las conexiones y las programaciones. Luego debe crear manualmente todos los recursos utilizados por los runbooks que importa a otra automatización.

Puede usar [cmdlets de Azure](https://msdn.microsoft.com/library/dn690262.aspx) para recuperar los detalles de recursos no cifrados y guardarlos para una referencia futura o crear recursos equivalentes en otra cuenta de automatización.

No es posible recuperar el valor de variables cifradas o del campo de contraseña de las credenciales mediante el uso de cmdlets. Si no conoce estos valores, puede recuperarlos desde un runbook con las actividades [Get-AutomationVariable](https://msdn.microsoft.com/library/dn940012.aspx) y [Get-AutomationPSCredential](https://msdn.microsoft.com/library/dn940015.aspx).

No es posible exportar certificados desde Automatización de Azure. Debe asegurarse de que todos los certificados estén disponibles fuera de Azure.

<!---HONumber=August15_HO6-->