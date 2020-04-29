---
title: Actividad de búsqueda en Azure Data Factory
description: Más información sobre cómo usar la actividad de búsqueda para buscar un valor desde un origen externo. Además, las actividades posteriores pueden hacer referencia a esta salida.
services: data-factory
documentationcenter: ''
author: djpmsft
ms.author: daperlov
manager: jroth
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.date: 06/15/2018
ms.openlocfilehash: 02abdaf46ca2af6c96d3b5e8d4ce5876831bd415
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/28/2020
ms.locfileid: "81418008"
---
# <a name="lookup-activity-in-azure-data-factory"></a>Actividad de búsqueda en Azure Data Factory
[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-asa-md.md)]

La actividad de búsqueda puede recuperar un conjunto de datos de cualquiera de los orígenes de datos compatible con Azure Data Factory. Utilícela en el siguiente escenario:
- Determinar dinámicamente en qué objetos operar en una actividad posterior, en lugar de codificar de forma rígida el nombre del objeto. Algunos ejemplos de objeto son los archivos y las tablas.

La actividad de búsqueda lee y devuelve el contenido de una tabla o un archivo de configuración. También devuelve el resultado de ejecutar una consulta o un procedimiento almacenado. El resultado de la actividad de búsqueda se puede usar en una actividad de transformación o copia posterior si es un valor singleton. La salida se puede usar en una actividad ForEach si es una matriz de atributos.

## <a name="supported-capabilities"></a>Funcionalidades admitidas

Se admiten los siguientes orígenes de datos para la actividad de búsqueda. El número más grande de filas que puede devolver la actividad de búsqueda es 5000, con un tamaño máximo de 2 MB. Actualmente, la duración máxima de la actividad de búsqueda antes del tiempo de expiración es una hora.

[!INCLUDE [data-factory-v2-supported-data-stores](../../includes/data-factory-v2-supported-data-stores-for-lookup-activity.md)]

## <a name="syntax"></a>Sintaxis

```json
{
    "name": "LookupActivity",
    "type": "Lookup",
    "typeProperties": {
        "source": {
            "type": "<source type>"
            <additional source specific properties (optional)>
        },
        "dataset": { 
            "referenceName": "<source dataset name>",
            "type": "DatasetReference"
        },
        "firstRowOnly": false
    }
}
```

## <a name="type-properties"></a>Propiedades de tipo

Nombre | Descripción | Tipo | ¿Necesario?
---- | ----------- | ---- | --------
dataset | Proporciona la referencia de conjunto de datos para la búsqueda. Obtenga los detalles de la sección **Propiedades del conjunto de datos** de cada artículo del conector correspondiente. | Par clave-valor | Sí
source | Contiene propiedades de origen específicas para el conjunto de datos, al igual que el origen de la actividad de copia. Obtenga los detalles de la sección **Copiar propiedades de la actividad** de cada artículo del conector correspondiente. | Par clave-valor | Sí
firstRowOnly | Indica si se deben devolver todas las filas o solo la primera. | Boolean | No. El valor predeterminado es `true`.

> [!NOTE]
> 
> * No se admiten las columnas de origen con el tipo **ByteArray**.
> * La **estructura** no se admite en las definiciones del conjunto de datos. En el caso de los archivos de formato de texto, utilice la fila de encabezado para proporcionar el nombre de columna.
> * Si el origen de la búsqueda es un archivo JSON, la configuración `jsonPathDefinition` para cambiar la forma del objeto JSON no se admite. Se recuperarán los objetos completos.

## <a name="use-the-lookup-activity-result-in-a-subsequent-activity"></a>Uso del resultado de la actividad de búsqueda en una actividad posterior

El resultado de la búsqueda se devuelve en la sección `output` del resultado de ejecución de la actividad.

* **Cuando `firstRowOnly` está establecido en `true` (valor predeterminado)** , el formato de salida es como se muestra en el código siguiente. El resultado de búsqueda está en una clave `firstRow` fija. Para utilizar el resultado en una actividad posterior, utilice el patrón de `@{activity('MyLookupActivity').output.firstRow.TableName}`.

    ```json
    {
        "firstRow":
        {
            "Id": "1",
            "TableName" : "Table1"
        }
    }
    ```

* **Cuando `firstRowOnly` está establecido en `false`** , el formato de salida es como se muestra en el código siguiente. Un campo `count` indica cuántos registros se devuelven. Se muestran los valores detallados bajo una matriz `value` fija. En tal caso, la actividad de búsqueda va seguida de una [actividad ForEach](control-flow-for-each-activity.md). Pase la matriz `value` al campo `items` de la actividad ForEach mediante el patrón de `@activity('MyLookupActivity').output.value`. Para acceder a elementos en la matriz `value`, use la siguiente sintaxis: `@{activity('lookupActivity').output.value[zero based index].propertyname}`. Un ejemplo es `@{activity('lookupActivity').output.value[0].tablename}`.

    ```json
    {
        "count": "2",
        "value": [
            {
                "Id": "1",
                "TableName" : "Table1"
            },
            {
                "Id": "2",
                "TableName" : "Table2"
            }
        ]
    } 
    ```

### <a name="copy-activity-example"></a>Ejemplo de actividad de copia
En este ejemplo, la actividad copia los datos de una tabla SQL de su base de datos de Azure SQL a Azure Blob Storage. El nombre de la tabla SQL se almacena en un archivo JSON en Blob Storage. La actividad de búsqueda busca el nombre de la tabla en entorno de tiempo de ejecución. JSON se modifica de forma dinámica con este enfoque. No es necesario volver a implementar canalizaciones ni conjuntos de datos. 

En este ejemplo se muestra solo la búsqueda de la primera fila. Para la búsqueda de todas las filas y encadenar los resultados con la actividad ForEach, consulte los ejemplos en [Copia de varias tablas en bloque mediante Azure Data Factory](tutorial-bulk-copy.md).

### <a name="pipeline"></a>Canalización
Esta canalización contiene dos actividades: búsqueda y copia. 

- La actividad de búsqueda está configurada para usar **LookupDataset**, que hace referencia a una ubicación en una instancia de Azure Blob Storage. La actividad de búsqueda lee el nombre de la tabla SQL desde un archivo JSON en esta ubicación. 
- La actividad de copia usa la salida de la actividad de búsqueda, que es el nombre de la tabla SQL. La propiedad **tableName** en **SourceDataset** se configura para usar la salida de la actividad de búsqueda. Con la actividad de copia se copian datos de la tabla SQL en una ubicación de Azure Blob Storage. La ubicación se especifica mediante la propiedad **SinkDataset**. 

```json
{
    "name": "LookupPipelineDemo",
    "properties": {
        "activities": [
            {
                "name": "LookupActivity",
                "type": "Lookup",
                "typeProperties": {
                    "source": {
                        "type": "BlobSource"
                    },
                    "dataset": { 
                        "referenceName": "LookupDataset", 
                        "type": "DatasetReference" 
                    }
                }
            },
            {
                "name": "CopyActivity",
                "type": "Copy",
                "typeProperties": {
                    "source": { 
                        "type": "SqlSource", 
                        "sqlReaderQuery": "select * from @{activity('LookupActivity').output.firstRow.tableName}" 
                    },
                    "sink": { 
                        "type": "BlobSink" 
                    }
                },                
                "dependsOn": [ 
                    { 
                        "activity": "LookupActivity", 
                        "dependencyConditions": [ "Succeeded" ] 
                    }
                 ],
                "inputs": [ 
                    { 
                        "referenceName": "SourceDataset", 
                        "type": "DatasetReference" 
                    } 
                ],
                "outputs": [ 
                    { 
                        "referenceName": "SinkDataset", 
                        "type": "DatasetReference" 
                    } 
                ]
            }
        ]
    }
}
```

### <a name="lookup-dataset"></a>Conjunto de datos de búsqueda
El conjunto de datos **lookup** es el archivo **sourcetable.json** de la carpeta de búsqueda de Azure Storage que el tipo **AzureStorageLinkedService** especifica. 

```json
{
    "name": "LookupDataset",
    "properties": {
        "type": "AzureBlob",
        "typeProperties": {
            "folderPath": "lookup",
            "fileName": "sourcetable.json",
            "format": {
                "type": "JsonFormat",
                "filePattern": "SetOfObjects"
            }
        },
        "linkedServiceName": {
            "referenceName": "AzureStorageLinkedService",
            "type": "LinkedServiceReference"
        }
    }
}
```

### <a name="source-dataset-for-copy-activity"></a>Conjunto de datos **source** para la actividad de copia
El conjunto de datos **source** usa la salida de la actividad de búsqueda, que es el nombre de la tabla SQL. Con la actividad de copia se copian datos de esta tabla SQL en una ubicación de Azure Blob Storage. La ubicación se especifica mediante el conjunto de datos **sink**. 

```json
{
    "name": "SourceDataset",
    "properties": {
        "type": "AzureSqlTable",
        "typeProperties":{
            "tableName": "@{activity('LookupActivity').output.firstRow.tableName}"
        },
        "linkedServiceName": {
            "referenceName": "AzureSqlLinkedService",
            "type": "LinkedServiceReference"
        }
    }
}
```

### <a name="sink-dataset-for-copy-activity"></a>Conjunto de datos **sink** para la actividad de copia
Con la actividad de copia se copian datos de la tabla SQL en el archivo **filebylookup.csv** de la carpeta **csv** en Azure Storage. La propiedad **AzureStorageLinkedService** especifica el archivo. 

```json
{
    "name": "SinkDataset",
    "properties": {
        "type": "AzureBlob",
        "typeProperties": {
            "folderPath": "csv",
            "fileName": "filebylookup.csv",
            "format": {
                "type": "TextFormat"                                                                    
            }
        },
        "linkedServiceName": {
            "referenceName": "AzureStorageLinkedService",
            "type": "LinkedServiceReference"
        }
    }
}
```

### <a name="azure-storage-linked-service"></a>Servicio vinculado de Azure Storage
Esta cuenta de almacenamiento contiene el archivo JSON con los nombres de las tablas SQL. 

```json
{
    "properties": {
        "type": "AzureStorage",
        "typeProperties": {
            "connectionString": "DefaultEndpointsProtocol=https;AccountName=<StorageAccountName>;AccountKey=<StorageAccountKey>"
        }
    },
        "name": "AzureStorageLinkedService"
}
```

### <a name="azure-sql-database-linked-service"></a>Servicio vinculado de Azure SQL Database
Esta instancia de Azure SQL Database contiene los datos que se copiarán en Blob Storage. 

```json
{
    "name": "AzureSqlLinkedService",
    "properties": {
        "type": "AzureSqlDatabase",
        "description": "",
        "typeProperties": {
            "connectionString": "Server=<server>;Initial Catalog=<database>;User ID=<user>;Password=<password>;"
        }
    }
}
```

### <a name="sourcetablejson"></a>sourcetable.json

#### <a name="set-of-objects"></a>Conjunto de objetos

```json
{
  "Id": "1",
  "tableName": "Table1"
}
{
   "Id": "2",
  "tableName": "Table2"
}
```

#### <a name="array-of-objects"></a>Matriz de objetos

```json
[ 
    {
        "Id": "1",
        "tableName": "Table1"
    },
    {
        "Id": "2",
        "tableName": "Table2"
    }
]
```

## <a name="limitations-and-workarounds"></a>Limitaciones y soluciones alternativas

A continuación se indican algunas de las limitaciones de la actividad de búsqueda y las soluciones alternativas sugeridas.

| Limitación | Solución alternativa |
|---|---|
| La actividad de búsqueda tiene un máximo de 5.000 filas y un tamaño máximo de 2 MB. | Diseñe una canalización de dos niveles donde la canalización exterior recorra en iteración una canalización interna, que recupera datos que no superan el tamaño o el número máximo de filas. |
| | |

## <a name="next-steps"></a>Pasos siguientes
Consulte otras actividades de flujo de control compatibles con Data Factory: 

- [Actividad de ejecución de canalización](control-flow-execute-pipeline-activity.md)
- [Actividad ForEach](control-flow-for-each-activity.md)
- [Actividad GetMetadata](control-flow-get-metadata-activity.md)
- [Actividad web](control-flow-web-activity.md)
