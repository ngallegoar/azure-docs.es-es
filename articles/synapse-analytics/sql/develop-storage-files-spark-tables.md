---
title: Consulta de tablas de Spark mediante SQL a petición (versión preliminar)
description: Información general sobre cómo consultar tablas de Spark mediante SQL a petición (versión preliminar)
services: synapse-analytics
author: julieMSFT
ms.service: synapse-analytics
ms.topic: overview
ms.subservice: ''
ms.date: 04/15/2020
ms.author: jrasnick
ms.reviewer: jrasnick
ms.openlocfilehash: 41e31a322a3d771557474fdf5c318960822bcfe1
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/16/2020
ms.locfileid: "81420079"
---
# <a name="query-spark-tables-with-azure-synapse-analytics-using-sql-on-demand-preview"></a>Consulta de tablas de Spark con Azure Synapse Analytics mediante SQL a petición (versión preliminar)

SQL a petición (versión preliminar) puede sincronizar automáticamente los metadatos de los grupos de Spark en el área de trabajo de Synapse (versión preliminar). Se creará una base de datos de SQL a petición para cada base de datos que exista en los grupos de Spark (versión preliminar). Para cada tabla de Spark basada en Parquet o en CSV, se crea una tabla externa en la base de datos de SQL a petición. Como tal, puede apagar los grupos de Spark y seguir consultando las tablas de Spark desde SQL a petición.

Cuando se crean particiones de una en Spark, los archivos del almacenamiento se organizan por carpetas. SQL a petición usará los metadatos de la partición y solo se dirigirá a los archivos y carpetas pertinentes para la consulta.

La sincronización de metadatos se configura automáticamente para cada grupo de Spark aprovisionado en el área de trabajo de Azure Synapse. Puede iniciar la consulta de tablas de Spark al instante.

Cada tabla de Spark se representa con una tabla externa en un esquema dbo que corresponde a una base de datos de SQL a petición. En el caso de las consultas de la tabla de Spark, ejecute una consulta destinada a un [spark_table] externo. Antes de ejecutar el ejemplo siguiente, asegúrese de que tiene un [acceso correcto a la cuenta de almacenamiento](develop-storage-files-storage-access-control.md) en que se encuentran los archivos.

```sql
SELECT * FROM [db].dbo.[spark_table]
```

## <a name="spark-data-types-to-sql-data-types-mapping"></a>Asignación de tipos de datos de Spark a tipos de datos de SQL

| Tipo de datos de Spark | Tipo de datos de SQL               |
| --------------- | --------------------------- |
| ByteType        | SMALLINT                    |
| ShortType       | SMALLINT                    |
| IntegerType     | int                         |
| LongType        | bigint                      |
| FloatType       | real                        |
| DoubleType      | FLOAT                       |
| DecimalType     | Decimal                     |
| TimestampType   | datetime2                   |
| DateType        | date                        |
| StringType      | varchar(max)*               |
| BinaryType      | varbinary                   |
| BooleanType     | bit                         |
| ArrayType       | varchar(max)* (en JSON)** |
| MapType         | varchar(max)* (en JSON)** |
| StructType      | varchar(max)* (en JSON)** |

\* La intercalación usada es Latin1_General_100_BIN2_UTF8.

** ArrayType, MapType y StructType se representan como JSON.



## <a name="next-steps"></a>Pasos siguientes

Vaya al artículo [Control de acceso a almacenamiento ](develop-storage-files-storage-access-control.md) para más información sobre el control de acceso al almacenamiento.
