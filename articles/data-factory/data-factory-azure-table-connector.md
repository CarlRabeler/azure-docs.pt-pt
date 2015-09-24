<properties 
	pageTitle="Conector de tablas de Azure: movimiento de datos a y desde tablas de Azure" 
	description="Obtenga información acerca del conector de tablas de Azure para el servicio Factoría de datos que le permite mover datos a y desde el almacenamiento de tablas de Azure." 
	services="data-factory" 
	documentationCenter="" 
	authors="spelluru" 
	manager="jhubbard" 
	editor="monicar"/>

<tags 
	ms.service="data-factory" 
	ms.workload="data-services" 
	ms.tgt_pltfrm="na" 
	ms.devlang="na" 
	ms.topic="article" 
	ms.date="07/29/2015" 
	ms.author="spelluru"/>

# Conector de tablas de Azure: movimiento de datos a y desde tablas de Azure

En este artículo se describe cómo puede usar la actividad de copia en la Factoría de datos de Azure para mover datos a tablas de Azure desde otro almacén de datos y viceversa. Este artículo se basa en el artículo sobre [actividades de movimiento de datos](data-factory-data-movement-activities.md) que presenta una introducción general del movimiento de datos con la actividad de copia y las combinaciones del almacén de datos admitidas.

## Ejemplo: copia de datos de una tabla de Azure a un blob de Azure

El ejemplo siguiente muestra:

1.	Un servicio vinculado de tipo AzureStorage (usado para tabla y blob).
2.	Un conjunto de datos de entrada de tipo AzureTable.
3.	Un conjunto de datos de salida de tipo AzureBlob. 
3.	Una canalización con la actividad de copia que usa AzureTableSource y BlobSink. 

El ejemplo copia los datos que pertenecen a la partición predeterminada de una tabla de Azure a un blob cada hora. Las propiedades JSON usadas en estos ejemplos se describen en las secciones que aparecen después de los ejemplos.

**Servicio vinculado de Almacenamiento de Azure:**

	{
	  "name": "StorageLinkedService",
	  "properties": {
	    "type": "AzureStorage",
	    "typeProperties": {
	      "connectionString": "DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>"
	    }
	  }
	}

**Conjunto de datos de entrada de tabla de Azure:**

El ejemplo se supone que ha creado una tabla "MyTable" en la tabla de Azure.
 
Si se establece "external": "true" y se especifica la directiva externalData, se indica a la factoría de datos que la tabla es externa a la factoría de datos y que no se produce por ninguna actividad de la factoría de datos.

	{
	  "name": "AzureTableInput",
	  "properties": {
	    "type": "AzureTable",
	    "linkedServiceName": "StorageLinkedService",
	    "typeProperties": {
	      "tableName": "MyTable"
	    },
	    "external": true,
	    "availability": {
	      "frequency": "Hour",
	      "interval": 1
	    },
	    "policy": {
	      "externalData": {
	        "retryInterval": "00:01:00",
	        "retryTimeout": "00:10:00",
	        "maximumRetry": 3
	      }
	    }
	  }
	}

**Conjunto de datos de salida de blob de Azure:**

Los datos se escriben en un nuevo blob cada hora (frecuencia: hora, intervalo: 1). La ruta de acceso de la carpeta para el blob se evalúa dinámicamente según la hora de inicio del segmento que se está procesando. La ruta de acceso de la carpeta usa las partes year, month, day y hours de la hora de inicio.

	{
	  "name": "AzureBlobOutput",
	  "properties": {
	    "type": "AzureBlob",
	    "linkedServiceName": "StorageLinkedService",
	    "typeProperties": {
	      "folderPath": "mycontainer/myfolder/yearno={Year}/monthno={Month}/dayno={Day}/hourno={Hour}",
	      "partitionedBy": [
	        {
	          "name": "Year",
	          "value": {
	            "type": "DateTime",
	            "date": "SliceStart",
	            "format": "yyyy"
	          }
	        },
	        {
	          "name": "Month",
	          "value": {
	            "type": "DateTime",
	            "date": "SliceStart",
	            "format": "%M"
	          }
	        },
	        {
	          "name": "Day",
	          "value": {
	            "type": "DateTime",
	            "date": "SliceStart",
	            "format": "%d"
	          }
	        },
	        {
	          "name": "Hour",
	          "value": {
	            "type": "DateTime",
	            "date": "SliceStart",
	            "format": "%H"
	          }
	        }
	      ],
	      "format": {
	        "type": "TextFormat",
	        "columnDelimiter": "\t",
	        "rowDelimiter": "\n"
	      }
	    },
	    "availability": {
	      "frequency": "Hour",
	      "interval": 1
	    }
	  }
	}

**Canalización con actividad de copia:**

La canalización contiene una actividad de copia que está configurada para usar los conjuntos de datos de entrada y de salida y está programada para ejecutarse cada hora. En la definición de la canalización JSON, el tipo **source** se establece en **AzureTableSource** y el tipo **sink**, en **BlobSink**. La consulta SQL especificada con la propiedad **AzureTableSourceQuery** selecciona los datos de la partición predeterminada cada hora, que se van a copiar.

	{  
	    "name":"SamplePipeline",
	    "properties":{  
	    	"start":"2014-06-01T18:00:00",
		    "end":"2014-06-01T19:00:00",
	    	"description":"pipeline for copy activity",
	    	"activities":[  
				{
	        		"name": "AzureTabletoBlob",
	        		"description": "copy activity",
	        		"type": "Copy",
	        		"inputs": [
	          			{
		            		"name": "AzureTableInput"
		        		}
	        		],
	        		"outputs": [
	          			{
	          	  			"name": "AzureBlobOutput"
		          		}
		        	],
		        	"typeProperties": {
		          		"source": {
		            		"type": "AzureTableSource",
				            "AzureTableSourceQuery": "PartitionKey eq 'DefaultPartitionKey'"
		          		},
		          		"sink": {
		            		"type": "BlobSink"
		          		}
    	    		},
		        	"scheduler": {
		          		"frequency": "Hour",
		          		"interval": 1
		        	},				
		        	"policy": {
		          		"concurrency": 1,
		          		"executionPriorityOrder": "OldestFirst",
		          		"retry": 0,
		          		"timeout": "01:00:00"
		        	}
				}
		     ]	
		}
	}

## Ejemplo: copia de datos de un blob de Azure a una tabla de Azure

El ejemplo siguiente muestra:

1.	Un servicio vinculado de tipo AzureStorage (usado para tabla y blob)
3.	Un conjunto de datos de entrada de tipo AzureBlob.
4.	Un conjunto de datos de salida de tipo AzureTable. 
4.	Una canalización con la actividad de copia que usa BlobSource y AzureTableSink. 

El ejemplo copia los datos que pertenecen a una serie temporal desde un blob de Azure a una tabla de una base de datos de una tabla de Azure cada hora. Las propiedades JSON usadas en estos ejemplos se describen en las secciones que aparecen después de los ejemplos.

**Servicio vinculado de Almacenamiento de Azure (para tabla y blob de Azure):**

	{
	  "name": "StorageLinkedService",
	  "properties": {
	    "type": "AzureStorage",
	    "typeProperties": {
	      "connectionString": "DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>"
	    }
	  }
	}

**Conjunto de datos de entrada de blob de Azure:**

Los datos se seleccionan de un nuevo blob cada hora (frecuencia: hora, intervalo: 1). La ruta de acceso de la carpeta y el nombre de archivo para el blob se evalúan dinámicamente según la hora de inicio del segmento que se está procesando. La ruta de acceso de la carpeta usa la parte year, month y day de la hora de inicio y el nombre de archivo, la parte hour. El valor "external": "true" informa al servicio Factoría de datos que esta tabla es externa a la factoría y no la produce una actividad de la factoría de datos.
	
	{
	  "name": "AzureBlobInput",
	  "properties": {
	    "type": "AzureBlob",
	    "linkedServiceName": "StorageLinkedService",
	    "typeProperties": {
	      "folderPath": "mycontainer/myfolder/yearno={Year}/monthno={Month}/dayno={Day}",
	      "fileName": "{Hour}.csv",
	      "partitionedBy": [
	        {
	          "name": "Year",
	          "value": {
	            "type": "DateTime",
	            "date": "SliceStart",
	            "format": "yyyy"
	          }
	        },
	        {
	          "name": "Month",
	          "value": {
	            "type": "DateTime",
	            "date": "SliceStart",
	            "format": "%M"
	          }
	        },
	        {
	          "name": "Day",
	          "value": {
	            "type": "DateTime",
	            "date": "SliceStart",
	            "format": "%d"
	          }
	        },
	        {
	          "name": "Hour",
	          "value": {
	            "type": "DateTime",
	            "date": "SliceStart",
	            "format": "%H"
	          }
	        }
	      ],
	      "format": {
	        "type": "TextFormat",
	        "columnDelimiter": ",",
	        "rowDelimiter": "\n"
	      }
	    },
	    "external": true,
	    "availability": {
	      "frequency": "Hour",
	      "interval": 1
	    },
	    "policy": {
	      "externalData": {
	        "retryInterval": "00:01:00",
	        "retryTimeout": "00:10:00",
	        "maximumRetry": 3
	      }
	    }
	  }
	}

**Conjunto de datos de salida de tabla de Azure:**

El ejemplo copia los datos a una tabla denominada "MyTable" en la tabla de Azure. Debe crear la tabla en la tabla de Azure con el mismo número de columnas que espera que contenga el archivo CSV de blob. Se agregan nuevas filas a la tabla cada hora.

	{
	  "name": "AzureTableOutput",
	  "properties": {
	    "type": "AzureTable",
	    "linkedServiceName": "StorageLinkedService",
	    "typeProperties": {
	      "tableName": "MyOutputTable"
	    },
	    "availability": {
	      "frequency": "Hour",
	      "interval": 1
	    }
	  }
	}

**Canalización con actividad de copia:**

La canalización contiene una actividad de copia que está configurada para usar los conjuntos de datos de entrada y de salida y está programada para ejecutarse cada hora. En la definición de la canalización JSON, el tipo **source** se establece en **BlobSource** y el tipo **sink**, en **AzureTableSink**.


	{  
	    "name":"SamplePipeline",
	    "properties":{  
	    "start":"2014-06-01T18:00:00",
	    "end":"2014-06-01T19:00:00",
	    "description":"pipeline with copy activity",
	    "activities":[  
	      {
	        "name": "AzureBlobtoTable",
	        "description": "Copy Activity",
	        "type": "Copy",
	        "inputs": [
	          {
	            "name": "AzureBlobInput"
	          }
	        ],
	        "outputs": [
	          {
	            "name": "AzureTableOutput"
	          }
	        ],
	        "typeProperties": {
	          "source": {
	            "type": "BlobSource",
	            "blobColumnSeparators": ","
	          },
	          "sink": {
	            "type": "AzureTableSink",
	            "writeBatchSize": 100,
	            "writeBatchTimeout": "01:00:00"
	          }
	        },
	        "scheduler": {
	          "frequency": "Hour",
	          "interval": 1
	        },						
	        "policy": {
	          "concurrency": 1,
	          "executionPriorityOrder": "OldestFirst",
	          "retry": 0,
	          "timeout": "01:00:00"
	        }
	      }
	      ]
	   }
	}

## Propiedades del servicio vinculado de Almacenamiento de Azure

Puede vincular una cuenta de Almacenamiento de Azure a una Factoría de datos de Azure mediante un servicio vinculado de Almacenamiento de Azure. En la tabla siguiente se proporcionan descripciones de los elementos JSON específicos del servicio vinculado de Almacenamiento de Azure.

| Propiedad | Descripción | Obligatorio |
| -------- | ----------- | -------- |
| type | La propiedad type debe establecerse en: AzureStorage | Sí |
| connectionString | Especifique la información necesaria para conectarse a Almacenamiento de Azure para la propiedad connectionString. Puede obtener connectionString para Almacenamiento de Azure desde el Portal de Azure. | Sí |

## Propiedades de tipo de conjunto de datos de tabla de Azure

Para obtener una lista completa de las secciones y propiedades disponibles para definir conjuntos de datos, consulte el artículo [Creación de conjuntos de datos](data-factory-create-datasets.md). Las secciones como structure, availability y policy de un conjunto de datos JSON son similares en todos los tipos de conjunto de datos (SQL Azure, blob de Azure, tabla de Azure, etc.).

La sección typeProperties es diferente en cada tipo de conjunto de datos y proporciona información acerca de la ubicación de los datos en el almacén de datos. La sección **typeProperties** del conjunto de datos de tipo **AzureTable** tiene las propiedades siguientes.

| Propiedad | Descripción | Obligatorio |
| -------- | ----------- | -------- |
| tableName | Nombre de la tabla en la instancia de Base de datos de tablas de Azure a la que hace referencia el servicio vinculado. | Sí

## Propiedades de tipo de actividad de copia de tabla de Azure

Para obtener una lista completa de las secciones y propiedades disponibles para definir actividades, consulte el artículo [Creación de canalizaciones](data-factory-create-pipelines.md). Propiedades como nombre, descripción, tablas de entrada y salida, varias directivas, etc. están disponibles para todos los tipos de actividades.

Por otro lado, las propiedades disponibles en la sección typeProperties de la actividad varían con cada tipo de actividad y, en caso de la actividad de copia, varían en función de los tipos de orígenes y receptores.

**AzureTableSource** admite las siguientes propiedades en la sección typeProperties:

Propiedad | Descripción | Valores permitidos | Obligatorio
-------- | ----------- | -------------- | -------- 
azureTableSourceQuery | Utilice la consulta personalizada para leer los datos. | Cadena de consulta de tabla de Azure. Ejemplo: **ColumnA eq ValueA** | No
azureTableSourceIgnoreTableNotFound | Indica si se omite la excepción de la tabla inexistente. | TRUE<br/>FALSE | No |

**AzureTableSink** admite las siguientes propiedades en la sección typeProperties:


Propiedad | Descripción | Valores permitidos | Obligatorio  
-------- | ----------- | -------------- | -------- 
azureTableDefaultPartitionKeyValue | Valor predeterminado de la clave de la partición que puede usar el receptor. | Valor de cadena. | No 
azureTablePartitionKeyName | Nombre de columna especificado por el usuario, cuyos valores de columna se utilizan como clave de la partición. Si no se especifica, se utiliza AzureTableDefaultPartitionKeyValue como clave de la partición. | Un nombre de columna. | No |
azureTableRowKeyName | Nombre de columna especificado por el usuario, cuyos valores de columna se usan como clave de fila. Si no se especifica, use un GUID para cada fila. | Un nombre de columna. | No  
azureTableInsertType | Modo de insertar datos en la tabla de Azure. | merge<br/>replace | No 
writeBatchSize | Inserta datos en la tabla de Azure cuando se alcanza el valor de writeBatchSize o writeBatchTimeout. | Entero de 1 a 100 (unidad = recuento de filas) | No (predeterminado = 100) 
writeBatchTimeout | Inserta datos en la tabla de Azure cuando se alcanza el valor de writeBatchSize o writeBatchTimeout. | (Unidad = intervalo de tiempo)Ejemplo: “00:20:00” (20 minutos) | No (el valor predeterminado de intervalo de tiempo del cliente de almacenamiento es 90 segundos)

[AZURE.INCLUDE [data-factory-structure-for-rectangualr-datasets](../../includes/data-factory-structure-for-rectangualr-datasets.md)]

### Asignación de tipos para tabla de Azure

Como se mencionó en el artículo sobre [actividades de movimiento de datos](data-factory-data-movement-activities.md), la actividad de copia realiza conversiones automáticas de tipos de los tipos de origen a los tipos de receptor con el siguiente enfoque de dos pasos:

1. Conversión de tipos de origen nativos al tipo .NET
2. Conversión de tipo .NET al tipo del receptor nativo

Al mover datos a y desde la tabla de Azure, se usarán las siguientes [asignaciones definidas por el servicio Tabla de Azure](https://msdn.microsoft.com/library/azure/dd179338.aspx) desde tipos OData de la tabla de Azure al tipo .NET y viceversa.

| Tipo de datos OData | Tipo .NET | Detalles |
| --------------- | --------- | ------- |
| Edm.Binary | byte | Matriz de bytes de hasta 64 KB de tamaño. |
| Edm.Boolean | booleano | Valor booleano. |
| Edm.DateTime | DateTime | Valor de 64 bits expresado como hora universal coordinada (UTC). El intervalo admitido de DateTime comienza a las 12:00 de la noche del 1 de enero de 1601 D.C. (E.C.), UTC. El intervalo finaliza el 31 de diciembre de 9999. |
| Edm.Double | double | Valor de punto flotante de 64 bits. |
| Edm.Guid | Guid | Identificador único global de 128 bits. |
| Edm.Int32 | Int32 o bien int | Entero de 32 bits. |
| Edm.Int64 | Int64 o bien long | Entero de 64 bits. |
| Edm.String | String | Valor codificado mediante UTF-16. Los valores de cadena pueden tener un tamaño de hasta 64 KB. |

### Ejemplo de conversión de tipo

El siguiente es un ejemplo de la copia de datos desde un blob de Azure a una tabla de Azure con conversiones de tipo.

Supongamos que el conjunto de datos Blob está en formato CSV y contiene tres columnas. Una de ellas es una columna de fecha y hora con un formato de fecha y hora personalizado mediante los nombres abreviados de los días de la semana en francés.

Va a definir el conjunto de datos de origen de Blob como se indica a continuación junto con las definiciones de tipo para las columnas.
	
	{
	    "name": " AzureBlobInput",
	    "properties":
	    {
	         "structure": 
	          [
	                { "name": "userid", "type": "Int64"},
	                { "name": "name", "type": "String"},
	                { "name": "lastlogindate", "type": "Datetime", "culture": "fr-fr", "format": "ddd-MM-YYYY"}
	          ],
	        "type": "AzureBlob",
	        "linkedServiceName": "StorageLinkedService",
	        "typeProperties": {
	            "folderPath": "mycontainer/myfolder",
	            "fileName":"myfile.csv",
	            "format":
	            {
	                "type": "TextFormat",
	                "columnDelimiter": ","
	            }
	        },
	        "external": true,
	        "availability":
	        {
	            "frequency": "Hour",
	            "interval": 1,
	        },
			"policy": {
	            "externalData": {
	                "retryInterval": "00:01:00",
	                "retryTimeout": "00:10:00",
	                "maximumRetry": 3
	            }
			}
	    }
	}

Teniendo en cuenta la asignación de tipo desde el tipo OData de una tabla de Azure al tipo .NET anterior, tendrá que definir la tabla de la tabla de Azure con el siguiente esquema.

**Esquema de tabla de Azure:**

Nombre de la columna | Tipo
----------- | --------
userid | Edm.Int64
name | Edm.String 
lastlogindate | Edm.DateTime

A continuación definirá el conjunto de datos de tabla de Azure como sigue. No es necesario especificar la sección "structure" con la información de tipo porque ya se especificó en el almacén de datos subyacente.

	{
	  "name": "AzureTableOutput",
	  "properties": {
	    "type": "AzureTable",
	    "linkedServiceName": "StorageLinkedService",
	    "typeProperties": {
	      "tableName": "MyOutputTable"
	    },
	    "availability": {
	      "frequency": "Hour",
	      "interval": 1
	    }
	  }
	}

En este caso, la Factoría de datos realizará automáticamente las conversiones de tipos, incluido el campo Datetime con el formato personalizado de fecha y hora siguiendo la referencia cultural fr-fr, al mover datos de un blob a una tabla de Azure.



[AZURE.INCLUDE [data-factory-column-mapping](../../includes/data-factory-column-mapping.md)]

<!---HONumber=August15_HO6-->