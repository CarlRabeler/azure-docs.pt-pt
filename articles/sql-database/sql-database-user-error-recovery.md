<properties 
   pageTitle="Recuperación de errores de usuario en Base de datos SQL" 
   description="Obtenga información acerca de cómo recuperarse de errores de los usuarios, datos dañados accidentalmente o una base de datos eliminada con la característica Restauración a un momento dado (PITR) de Base de datos SQL de Azure." 
   services="sql-database" 
   documentationCenter="" 
   authors="elfisher" 
   manager="jeffreyg" 
   editor="monicar"/>

<tags
   ms.service="sql-database"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-management" 
   ms.date="07/23/2015"
   ms.author="elfish"/>

# Recuperar una base de datos SQL de Azure de un error de usuario

Base de datos SQL de Azure ofrece dos capacidades básicas para recuperase de los errores de usuario o de la modificación no intencionada de los datos.

- Restauración a un momento dado 
- Restaurar la base de datos eliminada

Puede obtener más información acerca de estas capacidades en esta [publicación de blog](http://azure.microsoft.com/blog/2014/10/01/azure-sql-database-point-in-time-restore/).

Base de datos SQL de Azure siempre se restaura en una base de datos nueva. Estas capacidades de restauración se ofrecen para todas las bases de datos de los niveles Basic, Standard y Premium.
##Recuperación con la restauración a un momento dado
En caso de un error de usuario o de modificación no intencionada de los datos, se puede usar Restauración a un momento dado para restaurar la base de datos a cualquier punto dado del período de retención de la base de datos.

Las bases de datos de la versión Basic tienen 7 días de retención, las de la versión Standard disponen de 14 días de retención y las de la versión Premium tienen 35 días de retención. Para obtener más información acerca de la retención de la base de datos, lea la [información general de continuidad del negocio](sql-database-business-continuity.md).

###Portal de Azure
1. Inicie sesión en el [portal de Azure](https://portal.Azure.com).
2. En el lado izquierdo de la pantalla, seleccione **EXAMINAR** y, a continuación, seleccione **Bases de datos SQL**.
3. Navegue hasta la base de datos y selecciónela.
4. En la parte superior de la hoja de la base de datos, seleccione **Restaurar**.
5. Especifique un nombre de base de datos y un momento dado y, a continuación, haga clic en **Crear**.
6. Comenzará el proceso de restauración de la base de datos, que se puede supervisar con la opción **NOTIFICACIONES** de la izquierda de la pantalla.

Una vez finalizada la restauración, puede configurar la base de datos recuperada para su uso siguiendo la guía [Finalización de una base de datos recuperada](sql-database-recovered-finalize.md).
###PowerShell
Use PowerShell para realizar la restauración de la base de datos mediante programación.

Para restaurar una base de con Restauración a un momento dado, use el cmdlet [Start-AzureSqlDatabaseRestore](https://msdn.microsoft.com/library/dn720218.aspx?f=255&MSPPError=-2147217396). Para ver un tutorial detallado, vea el [vídeo de procedimientos](http://azure.microsoft.com/documentation/videos/restore-a-sql-database-using-point-in-time-restore-with-microsoft-azure-powershell/).

		$Database = Get-AzureSqlDatabase -ServerName "YourServerName" –DatabaseName “YourDatabaseName”
		$RestoreRequest = Start-AzureSqlDatabaseRestore -SourceDatabase $Database –TargetDatabaseName “NewDatabaseName” –PointInTime “2015-01-01 06:00:00”
		Get-AzureSqlDatabaseOperation –ServerName "YourServerName" –OperationGuid $RestoreRequest.RequestID
		 
Una vez finalizada la restauración, puede configurar la base de datos recuperada para su uso siguiendo la guía [Finalización de una base de datos recuperada](sql-database-recovered-finalize.md).

###API de REST 
Use REST para realizar la restauración de la base de datos mediante programación.

1. Obtenga la base de datos que desee restaurar mediante la operación [Obtener base de datos](http://msdn.microsoft.com/library/azure/dn505708.aspx).

2.	Cree la solicitud de restauración con la operación [Crear solicitud de restauración de base de datos](http://msdn.microsoft.com/library/azure/dn509571.aspx).
	
3.	Realice el seguimiento de la solicitud de restauración con la operación [Estado de operación de base de datos](http://msdn.microsoft.com/library/azure/dn720371.aspx).

Una vez finalizada la restauración, puede configurar la base de datos recuperada para su uso siguiendo la guía [Finalización de una base de datos recuperada](sql-database-recovered-finalize.md).

##Recuperar una base de datos eliminada
En caso de que se elimine una base de datos, Base de datos SQL de Azure le permite restaurar la base de datos eliminada en el momento en que se eliminó. Base de datos SQL de Azure almacena la copia de seguridad de la base de datos eliminada durante el período de retención de la base de datos.

El período de retención de una base de datos eliminada lo determinan el nivel de servicio de la base de datos mientras esta existe, o bien el número de días en que existe la base de datos, el menor de estos dos valores. Para obtener más información acerca de la retención de la base de datos, lea la [información general de continuidad del negocio](sql-database-business-continuity.md).

###Portal de Azure
1. Inicie sesión en el [portal de Azure](https://portal.Azure.com).
2. En el lado izquierdo de la pantalla, seleccione **EXAMINAR** y, a continuación, seleccione **Servidores SQL**.
3. Navegue hasta el servidor y selecciónelo.
4. En **Operaciones** en la hoja del servidor, seleccione **Bases de datos eliminadas**.
5. Seleccione la base de datos eliminada que desee restaurar.
6. Especifique un nombre de base de datos y haga clic en **Crear**.
7. Comenzará el proceso de restauración de la base de datos, que se puede supervisar con la opción **NOTIFICACIONES** de la izquierda de la pantalla.

Una vez finalizada la restauración, puede configurar la base de datos recuperada para su uso siguiendo la guía [Finalización de una base de datos recuperada](sql-database-recovered-finalize.md).

###PowerShell
Use PowerShell para realizar la restauración de la base de datos mediante programación.

Para restaurar una base de datos eliminada, use el cmdlet [Start-AzureSqlDatabaseRestore](https://msdn.microsoft.com/library/dn720218.aspx?f=255&MSPPError=-2147217396). Para ver un tutorial detallado, vea el [vídeo de procedimientos](http://azure.microsoft.com/documentation/videos/restore-a-deleted-sql-database-with-microsoft-azure-powershell/).

1. Busque la base de datos eliminada y la fecha de eliminación en la lista de bases de datos eliminadas.
		
		Get-AzureSqlDatabase -RestorableDropped -ServerName "YourServerName"

2. Obtenga la base de datos eliminada específica e inicie la restauración.

		$Database = Get-AzureSqlDatabase -RestorableDropped -ServerName "YourServerName" –DatabaseName “YourDatabaseName” -DeletionDate "1/01/2015 12:00:00 AM""
		$RestoreRequest = Start-AzureSqlDatabaseRestore -SourceRestorableDroppedDatabase $Database –TargetDatabaseName “NewDatabaseName”
		Get-AzureSqlDatabaseOperation –ServerName "YourServerName" –OperationGuid $RestoreRequest.RequestID
		 
Una vez finalizada la restauración, puede configurar la base de datos recuperada para su uso siguiendo la guía [Finalización de una base de datos recuperada](sql-database-recovered-finalize.md).

###API de REST 
Use REST para realizar la restauración de la base de datos mediante programación.

1.	Para enumerar todas las bases de datos eliminadas que se pueden restaurar, utilice la operación [Lista de bases de datos eliminadas que se pueden restaurar](http://msdn.microsoft.com/library/azure/dn509562.aspx).
	
2.	Para obtener los detalles de la base de datos eliminada que desea restaurar, utilice la operación [Obtener base de datos eliminada que se puede restaurar](http://msdn.microsoft.com/library/azure/dn509574.aspx).

3.	Inicie la restauración con la operación [Crear solicitud de restauración de base de datos](http://msdn.microsoft.com/library/azure/dn509571.aspx).
	
4.	Realice un seguimiento del estado de la restauración con la operación [Estado de operación de base de datos](http://msdn.microsoft.com/library/azure/dn720371.aspx).

Una vez finalizada la restauración, puede configurar la base de datos recuperada para su uso siguiendo la guía [Finalización de una base de datos recuperada](sql-database-recovered-finalize.md).
 

<!---HONumber=August15_HO6-->