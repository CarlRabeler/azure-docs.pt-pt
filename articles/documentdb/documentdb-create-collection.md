<properties 
	pageTitle="Crear una colección de base de datos de DocumentDB | Microsoft Azure" 
	description="Aprenda a crear colecciones mediante el portal de servicios en línea para DocumentDB de Azure, una base de datos de documentos NoSQL administrada para JSON. Obtenga una versión de evaluación gratuita hoy mismo." 
	services="documentdb" 
	authors="mimig1" 
	manager="jhubbard" 
	editor="monicar" 
	documentationCenter=""/>

<tags 
	ms.service="documentdb" 
	ms.workload="data-services" 
	ms.tgt_pltfrm="na" 
	ms.devlang="na" 
	ms.topic="article" 
	ms.date="07/08/2015" 
	ms.author="mimig"/>

# Creación de una colección de DocumentDB en el portal de vista previa de Azure

Para usar Microsoft Azure DocumentDB, debe tener una [cuenta de DocumentDB](documentdb-create-account.md), una [base de datos](documentdb-create-database.md), una colección y documentos. En este tema se describe cómo crear una colección de DocumentDB en el portal de vista previa de Azure.

No es necesario que las colecciones se creen en el portal de vista previa, también se pueden crear mediante los [SDK de DocumentDB](https://msdn.microsoft.com/library/azure/dn781482.aspx). Para un ejemplo de código de C# en el que se muestra cómo crear una colección mediante el SDK de .NET de DocumentDB, vea el archivo [Program.cs](https://github.com/Azure/azure-documentdb-net/blob/master/samples/code-samples/CollectionManagement/Program.cs) en el proyecto CollectionManagement, disponible en el repositorio [azure-documentdb-net](https://github.com/Azure/azure-documentdb-net) en [GitHub.com](https://github.com).

1.  En el [Portal de vista previa de Azure](https://portal.azure.com/), haga clic en **Examinar todo**.

2.  En la hoja **Examinar**, haga clic en **Cuentas de DocumentDB**.

3.  En la hoja **Cuentas de DocumentDB**, seleccione la cuenta que contiene la base de datos a la que va a agregar una colección. Si no se muestra ninguna cuenta, deberá [crear una cuenta de DocumentDB](documentdb-create-account.md).
    
    ![Screen shot highlighting the Browse button, DocumentDB Accounts on the Browse blade, and a DocumentDB account on the DocumentDB Accounts blade](./media/documentdb-create-collection/docdb-database-creation-1-3.png)

4. En la hoja **Base de datos**, haga clic en **Agregar colecciones**.

5. En la hoja **Agregar colección**, escriba el id. de la nueva colección. Cuando se valida el nombre, aparece una marca de verificación verde en el cuadro Id.

6. Seleccione un nivel de precios para la nueva colección. Cada colección creada es una entidad facturable. Para obtener más información sobre los niveles de rendimiento disponibles, consulte [Niveles de rendimiento en DocumentDB](documentdb-performance-levels.md).

7. Seleccione una de las siguientes **directivas de indización**.

	- **Predeterminada**. Esta directiva es mejor cuando se realizan consultas de igualdad en cadenas y se usan consultas ORDER BY, de rango y de igualdad para números. Esta directiva tiene una sobrecarga de almacenamiento de índices menor que la de **intervalo**.
	- **Hash**. Esta directiva es mejor cuando realizamos consultas de igualdad para números y cadenas. Esta directiva tiene la menor sobrecarga de almacenamiento de índice.
	- **Intervalo**. Esta directiva es mejor si está usando consultas de ORDER BY, intervalo e igualdad en números y cadenas. Esta directiva tiene una mayor sobrecarga de almacenamiento de índice que la **Predeterminada** o **Hash**.

	Para obtener más información acerca de las directivas de indización, consulte [Directivas de indización de DocumentDB](documentdb-indexing-policies.md).

8. Haga clic en **Aceptar** en la parte inferior de la pantalla para crear la nueva colección.

	![Screen shot highlighting the Add Collection button on the Database blade, the ID box on the Add Collection blade, and the OK button](./media/documentdb-create-collection/docdb-collection-creation-4-7.png)

9. La nueva colección aparece ahora en el modo **Colecciones** en la hoja **Base de datos**.
 
	![Screen shot of the new database in the DocumentDB Account blade](./media/documentdb-create-collection/docdb-collection-creation-8.png)

## Pasos siguientes

Ahora que tiene una colección, el paso siguiente es agregar o importar documentos a la colección. Cuando se trata de agregar documentos a una colección, tiene varias posibilidades:

- Puede [agregar documentos](../documentdb-view-json-document-explorer.md) mediante el Explorador de documentos en el portal de vista previa.
- También puede [importar documentos y datos](documentdb-import-data.md) mediante la Herramienta de migración de datos de DocumentDB, que le permite importar archivos CSV y JSON, así como datos de SQL Server, MongoDB, almacenamiento de tablas de Azure y otras colecciones de DocumentDB. 
- También puede agregar documentos con uno de los [SDK de DocumentDB](https://msdn.microsoft.com/library/azure/dn781482.aspx). DocumentDB tiene .NET, Java, Python, Node.js y SDK de la API de JavaScript. El archivo [Program.cs](https://github.com/Azure/azure-documentdb-net/blob/master/samples/code-samples/DocumentManagement/Program.cs) del proyecto DocumentManagement, disponible en el repositorio [azure-documentdb-net](https://github.com/Azure/azure-documentdb-net) de [GitHub.com](https://github.com), muestra las operaciones de CRUD en documentos mediante el SDK de .NET de DocumentDB.

Cuando tenga documentos en una colección, puede usar [SQL de DocumentDB](documentdb-sql-query.md) para [ejecutar consultas](documentdb-sql-query.md#executing-queries) frente a sus documentos usando el [Explorador de consultas](documentdb-query-collections-query-explorer.md) en el portal de vista previa, la [API de REST](https://msdn.microsoft.com/library/azure/dn781481.aspx) o uno de los [SDK](https://msdn.microsoft.com/library/azure/dn781482.aspx).

<!---HONumber=August15_HO6-->