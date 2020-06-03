---
title: Consulta de archivos JSON con SQL a petición (versión preliminar)
description: En esta sección se explica cómo leer archivos JSON mediante SQL a petición en Azure Synapse Analytics.
services: synapse-analytics
author: azaricstefan
ms.service: synapse-analytics
ms.topic: how-to
ms.subservice: ''
ms.date: 05/20/2020
ms.author: v-stazar
ms.reviewer: jrasnick, carlrab
ms.openlocfilehash: 7a8c9083ecbadbf63cf0ac65dc1803b478e939fe
ms.sourcegitcommit: 64fc70f6c145e14d605db0c2a0f407b72401f5eb
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/27/2020
ms.locfileid: "83873401"
---
# <a name="query-json-files-using-sql-on-demand-preview-in-azure-synapse-analytics"></a>Consulta de archivos JSON con SQL a petición (versión preliminar) en Azure Synapse Analytics

En este artículo, aprenderá a escribir una consulta con SQL a petición (versión preliminar) en Azure Synapse Analytics. El objetivo de la consulta es leer archivos JSON. Los formatos admitidos se enumeran en [OPENROWSET](develop-openrowset.md).

## <a name="prerequisites"></a>Prerrequisitos

El primer paso es **crear una base de datos** en la que se ejecutarán las consultas. Luego, se inicializan los objetos, para lo que hay que ejecutar un [script de instalación](https://github.com/Azure-Samples/Synapse/blob/master/SQL/Samples/LdwSample/SampleDB.sql) en esa base de datos. Este script de instalación creará los orígenes de datos, las credenciales con ámbito de base de datos y los formatos de archivo externos que se usan en estos ejemplos.

## <a name="sample-json-files"></a>Archivos JSON de ejemplo

La sección siguiente contiene scripts de ejemplo para leer archivos JSON. Los archivos se almacenan en un contenedor *json*, carpeta *books*, y contienen una entrada con un único libro con la siguiente estructura:

```json
{
   "_id":"ahokw88",
   "type":"Book",
   "title":"The AWK Programming Language",
   "year":"1988",
   "publisher":"Addison-Wesley",
   "authors":[
      "Alfred V. Aho",
      "Brian W. Kernighan",
      "Peter J. Weinberger"
   ],
   "source":"DBLP"
}
```

## <a name="read-json-files"></a>Lectura de archivos JSON

Para procesar archivos JSON mediante JSON_VALUE y [JSON_QUERY](/sql/t-sql/functions/json-query-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest), debe leer el archivo JSON desde el almacenamiento como una única columna. El siguiente script lee el archivo *book1.json* como una única columna:

```sql
SELECT
    *
FROM
    OPENROWSET(
        BULK 'json/books/book1.json',
        DATA_SOURCE = 'SqlOnDemandDemo',
        FORMAT='CSV',
        FIELDTERMINATOR ='0x0b',
        FIELDQUOTE = '0x0b',
        ROWTERMINATOR = '0x0b'
    )
    WITH (
        jsonContent varchar(8000)
    ) AS [r];
```

> [!NOTE]
> Está leyendo todo el archivo JSON como una única fila o columna. Por lo tanto, FIELDTERMINATOR, FIELDQUOTE y ROWTERMINATOR se establecen en 0x0b.

## <a name="query-json-files-using-json_value"></a>Consulta de archivos JSON mediante JSON_VALUE

En la consulta siguiente se muestra cómo usar [JSON_VALUE](/sql/t-sql/functions/json-value-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest) para recuperar valores escalares (título, editor) de un libro titulado *Probabilistic and Statistical Methods in Cryptology, An Introduction by Selected Topics*:

```sql
SELECT
    JSON_VALUE(jsonContent, '$.title') AS title,
    JSON_VALUE(jsonContent, '$.publisher') as publisher,
    jsonContent
FROM
    OPENROWSET(
        BULK 'json/books/*.json',
        DATA_SOURCE = 'SqlOnDemandDemo',
        FORMAT='CSV',
        FIELDTERMINATOR ='0x0b',
        FIELDQUOTE = '0x0b',
        ROWTERMINATOR = '0x0b'
    )
    WITH (
        jsonContent varchar(8000)
    ) AS [r]
WHERE
    JSON_VALUE(jsonContent, '$.title') = 'Probabilistic and Statistical Methods in Cryptology, An Introduction by Selected Topics';
```

## <a name="query-json-files-using-json_query"></a>Consulta de archivos JSON mediante JSON_QUERY

En la consulta siguiente se muestra cómo usar [JSON_QUERY](/sql/t-sql/functions/json-query-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest) para recuperar objetos y matrices (autores) de un libro titulado *Probabilistic and Statistical Methods in Cryptology, An Introduction by Selected Topics*:

```sql
SELECT
    JSON_QUERY(jsonContent, '$.authors') AS authors,
    jsonContent
FROM
    OPENROWSET(
        BULK 'json/books/*.json',
        DATA_SOURCE = 'SqlOnDemandDemo',
        FORMAT='CSV',
        FIELDTERMINATOR ='0x0b',
        FIELDQUOTE = '0x0b',
        ROWTERMINATOR = '0x0b'
    )
    WITH (
        jsonContent varchar(8000)
    ) AS [r]
WHERE
    JSON_VALUE(jsonContent, '$.title') = 'Probabilistic and Statistical Methods in Cryptology, An Introduction by Selected Topics';
```

## <a name="query-json-files-using-openjson"></a>Consulta de archivos JSON mediante OPENJSON

En la siguiente consulta se usa [OPENJSON](/sql/t-sql/functions/openjson-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest). Recuperará objetos y propiedades dentro de un libro titulado *Probabilistic and Statistical Methods in Cryptology, An Introduction by Selected Topics*:

```sql
SELECT
    j.*
FROM
    OPENROWSET(
        BULK 'json/books/*.json',
        DATA_SOURCE = 'SqlOnDemandDemo',
        FORMAT='CSV',
        FIELDTERMINATOR ='0x0b',
        FIELDQUOTE = '0x0b',
        ROWTERMINATOR = '0x0b'
    )
    WITH (
        jsonContent NVARCHAR(max) -- Use appropriate length. Make sure JSON fits. 
    ) AS [r]
CROSS APPLY OPENJSON(jsonContent) AS j
WHERE
    JSON_VALUE(jsonContent, '$.title') = 'Probabilistic and Statistical Methods in Cryptology, An Introduction by Selected Topics';
```

## <a name="next-steps"></a>Pasos siguientes

En los siguientes artículos de esta serie se muestra:

- [Consulta de carpetas y varios archivos](query-folders-multiple-csv-files.md)
- [Creación y uso de vistas](create-use-views.md)
