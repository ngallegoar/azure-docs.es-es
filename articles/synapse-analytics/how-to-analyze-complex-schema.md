---
title: Análisis del esquema con matrices y estructuras anidadas
description: Procedimiento para analizar matrices y estructuras anidadas con Apache Spark y SQL
services: synapse-analytics
author: ArnoMicrosoft
ms.service: synapse-analytics
ms.topic: how-to
ms.subservice: ''
ms.date: 06/15/2020
ms.author: acomet
ms.reviewer: jrasnick
ms.openlocfilehash: b02c3627cea5e441739c77d1882505c6b82489bc
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/02/2020
ms.locfileid: "84908030"
---
# <a name="analyze-complex-data-types-in-synapse"></a>Análisis de tipos de datos complejos en Synapse

Este artículo es adecuado para archivos y contenedores de Parquet en **Azure Synapse Link para Azure Cosmos DB**. Se explica cómo los usuarios pueden utilizar Spark o SQL para leer o transformar datos con un esquema complejo como matrices o estructuras anidadas. El ejemplo siguiente se realiza con un solo documento, pero se puede escalar con facilidad a miles de millones de documentos con Spark o SQL. En el código siguiente se usa PySpark (Python).

## <a name="use-case"></a>Caso de uso

Con los tipos de datos modernos, los tipos de datos complejos suelen ser habituales y representan un desafío para los ingenieros de datos. El análisis de esquemas y matrices anidados presenta algunos desafíos:
* Dificultad para escribir consultas SQL
* Dificultad para cambiar el nombre o convertir el tipo de datos de las columnas anidadas
* Problemas de rendimiento de visitas con objetos profundamente anidados

Los ingenieros de datos deben comprender cómo procesar de forma eficaz esos tipos de datos y hacer que sean fácilmente accesibles para todos.

En el ejemplo siguiente, se usará Synapse Spark para leer y transformar objetos a través de tramas de datos en una estructura plana. Se usa Synapse SQL sin servidor para consultar directamente estos objetos y devolver los resultados como una tabla convencional.

## <a name="what-are-arrays-and-nested-structures"></a>¿Qué son las matrices y estructuras anidadas?

El objeto siguiente procede de detalles de la aplicación. En este objeto, hay estructuras anidadas, pero también matrices que contienen estructuras anidadas.

```json
{
    "id": "66532691-ab20-11ea-8b1d-936b3ec64e54",
    "context": {
        "data": {
            "eventTime": "2020-06-10T13:43:34.553Z",
            "samplingRate": "100.0",
            "isSynthetic": "false"
        },
        "session": {
            "isFirst": "false",
            "id": "38619c14-7a23-4687-8268-95862c5326b1"
        },
        "custom": {
            "dimensions": [
                {
                    "customerInfo": {
                        "ProfileType": "ExpertUser",
                        "RoomName": "",
                        "CustomerName": "diamond",
                        "UserName": "XXXX@yahoo.com"
                    }
                },
                {
                    "customerInfo": {
                        "ProfileType": "Novice",
                        "RoomName": "",
                        "CustomerName": "topaz",
                        "UserName": "XXXX@outlook.com"
                    }
                }
            ]
        }
    }
}
```

### <a name="schema-example-of-arrays-and-nested-structures"></a>Ejemplo de esquema de matrices y estructuras anidadas
Al imprimir el esquema de la trama de datos de ese objeto (denominado **df**) con el comando **df.printschema**, se ve esta representación:

* el color amarillo representa la estructura anidada
* el color verde representa una matriz con dos elementos

[![Origen del esquema](./media/how-to-complex-schema/schema-origin.png)](./media/how-to-complex-schema/schema-origin.png#lightbox)

_rid, _ts y _etag se han agregado al sistema a medida que el documento se ingería en el almacén transaccional de Azure Cosmos DB.

En la trama de datos anterior solo hay 5 columnas y 1 fila. Después de la transformación, la trama de datos seleccionada tendrá 13 columnas y 2 filas en un formato tabular.

## <a name="flatten-nested-structures-and-explode-arrays-with-apache-spark"></a>Acoplamiento de estructuras anidadas y expansión de matrices con Apache Spark

Con Synapse Spark, resulta sencillo transformar estructuras anidadas en columnas y elementos de matriz en varias filas. Los pasos siguientes los pueden utilizar todos los usuarios para su propia implementación.

[![Pasos de transformaciones de Spark](./media/how-to-complex-schema/spark-transfo-steps.png)](./media/how-to-complex-schema/spark-transfo-steps.png#lightbox)

**Paso 1**: se define una función para acoplar el esquema anidado. Esta función se puede usar sin cambios. Cree una celda en un cuaderno de Pyspark con esa función:

```python
from pyspark.sql.functions import col

def flatten_df(nested_df):
    stack = [((), nested_df)]
    columns = []

    while len(stack) > 0:
        parents, df = stack.pop()

        flat_cols = [
            col(".".join(parents + (c[0],))).alias("_".join(parents + (c[0],)))
            for c in df.dtypes
            if c[1][:6] != "struct"
        ]

        nested_cols = [
            c[0]
            for c in df.dtypes
            if c[1][:6] == "struct"
        ]

        columns.extend(flat_cols)

        for nested_col in nested_cols:
            projected_df = df.select(nested_col + ".*")
            stack.append((parents + (nested_col,), projected_df))

    return nested_df.select(columns)
```

**Paso 2**: use la función para acoplar el esquema anidado de la trama de datos **df** en una trama de datos **df_flat** nueva:

```python
from pyspark.sql.types import StringType, StructField, StructType
df_flat = flatten_df(df)
display(df_flat.limit(10))
```

La función display debe devolver 10 columnas y 1 fila. La matriz y sus elementos anidados todavía están allí.

**Paso 3**: ahora, transforme la matriz **context_custom_dimensions** de la trama de datos **df_flat** en una trama de datos **df_flat_explode** nueva. En el código siguiente, también se define la columna que se selecciona:

```python
from pyspark.sql.functions import explode
from pyspark.sql.functions import flatten
from pyspark.sql.functions import arrays_zip
df_flat_explode = df_flat.select("_rid","_ts","id","_etag",explode(df_flat.context_custom_dimensions),"context_session_isFirst","context_session_id","context_data_eventTime","context_data_samplingRate","context_data_isSynthetic")\
.select("_rid","_ts","id","_etag","col.*","context_session_isFirst","context_session_id","context_data_eventTime","context_data_samplingRate","context_data_isSynthetic")
display(df_flat_explode.limit(10))

```

La función display debe devolver el resultado siguiente: 10 columnas y 2 filas. El siguiente paso consiste en acoplar los esquemas anidados con la función definida en el paso 1.

**Paso 4**: use la función para acoplar el esquema anidado de la trama de datos **df_flat_explode** en una trama de datos **df_flat_explode_flat** nueva:
```python
df_flat_explode_flat = flatten_df(df_flat_explode)
display(df_flat_explode_flat.limit(10))
```

La función display debe mostrar 13 columnas y 2 filas:

La función printSchema de la trama de datos df_flat_explode_flat devuelve este resultado:

[![Esquema final](./media/how-to-complex-schema/schema-final.png)](./media/how-to-complex-schema/schema-final.png#lightbox)

## <a name="read-arrays-and-nested-structures-directly-with-sql-serverless"></a>Lectura de matrices y estructuras anidadas directamente con SQL sin servidor

SQL sin servidor permite la consulta y la creación de vistas y tablas en estos objetos.

En primer lugar, en función de cómo se hayan almacenado los datos, los usuarios deben utilizar la siguiente taxonomía. Todo lo que se encuentra en MAYÚSCULAS es específico de este caso de uso:

| BULK              | FORMAT |
| -------------------- | --- |
| "https://ACCOUNTNAME.dfs.core.windows.net/FILESYSTEM/PATH/FINENAME.parquet" |"Parquet" (ADLSg2)|
| N"endpoint=https://ACCOUNTNAME.documents-staging.windows-ppe.net:443/;account= ACCOUNTNAME;database=DATABASENAME;collection=COLLECTIONNAME;region=REGIONTOQUERY, SECRET="YOURSECRET" |"CosmosDB" (Synapse Link)|



**SQL sin servidor** admitirá servicios vinculados para Azure Synapse Link para Azure Cosmos DB y autenticación de paso a través de AAD. La funcionalidad está actualmente en versión preliminar controlada para Synapse Link.

Reemplace lo siguiente:
* "YOUR BULK ABOVE" por la cadena de conexión del origen de datos al que se conecta
* "YOUR TYPE ABOVE" por el formato que usa para conectarse al origen

```sql
select *
FROM
openrowset(
BULK 'YOUR BULK ABOVE',
            FORMAT='YOUR TYPE ABOVE'
)
with (id varchar(50),
        contextdataeventTime varchar(50) '$.context.data.eventTime',
        contextdatasamplingRate varchar(50) '$.context.data.samplingRate',
        contextdataisSynthetic varchar(50) '$.context.data.isSynthetic',
        contextsessionisFirst varchar(50) '$.context.session.isFirst',
        contextsessionid varchar(50) '$.context.session.id',
        contextcustomdimensions varchar(max) '$.context.custom.dimensions'
) as q 
cross apply openjson (contextcustomdimensions) 
with ( ProfileType varchar(50) '$.customerInfo.ProfileType',
            RoomName varchar(50) '$.customerInfo.RoomName',
            CustomerName varchar(50) '$.customerInfo.CustomerName',
            UserName varchar(50) '$.customerInfo.UserName'
    )
```

Se realizan dos tipos de operaciones distintas:
* La línea de código siguiente definirá la columna denominada contextdataeventTime que hace referencia al elemento anidado: Context.Data.eventTime
```sql
contextdataeventTime varchar(50) '$.context.data.eventTime'
```

Esta línea definirá la columna denominada contextdataeventTime que hace referencia al elemento anidado: Context>Data>eventTime

* Se usa **cross apply** para crear filas para cada elemento de la matriz y, después, "with" define cada objeto anidado similar a la primera viñeta: 
```sql
cross apply openjson (contextcustomdimensions) 
with ( ProfileType varchar(50) '$.customerInfo.ProfileType', 
```

Si la matriz tuviera 5 elementos con 4 estructuras anidadas, SQL sin servidor devolvería 5 filas y 4 columnas.

SQL sin servidor puede consultar en contexto, asignar la matriz en 2 filas y, mostrar todas las estructuras anidadas en columnas.

## <a name="next-steps"></a>Pasos siguientes

* [Información sobre cómo consultar Azure Synapse Link para Azure Cosmos DB con Spark](./synapse-link/how-to-query-analytical-store-spark.md)
* [Consulta de tipos anidados de Parquet](./sql/query-parquet-nested-types.md) 