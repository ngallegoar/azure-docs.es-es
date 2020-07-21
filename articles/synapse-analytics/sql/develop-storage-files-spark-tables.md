---
title: Sincronización de Apache Spark para las definiciones de tablas externas de Azure Synapse en SQL On-Demand (versión preliminar)
description: Información general sobre cómo consultar tablas de Spark mediante SQL a petición (versión preliminar)
services: synapse-analytics
author: julieMSFT
ms.service: synapse-analytics
ms.topic: overview
ms.subservice: sql
ms.date: 04/15/2020
ms.author: jrasnick
ms.reviewer: jrasnick
ms.openlocfilehash: db6b2d95bf8f38495296885d14260b9900af1d51
ms.sourcegitcommit: dabd9eb9925308d3c2404c3957e5c921408089da
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/11/2020
ms.locfileid: "86247054"
---
# <a name="synchronize-apache-spark-for-azure-synapse-external-table-definitions-in-sql-on-demand-preview"></a>Sincronización de Apache Spark para las definiciones de tablas externas de Azure Synapse en SQL On-Demand (versión preliminar)

SQL On-Demand (versión preliminar) puede sincronizar automáticamente los metadatos de Apache Spark para grupos de Azure Synapse. Se creará una base de datos de SQL a petición para cada base de datos que exista en los grupos de Spark (versión preliminar). 

Para cada tabla externa de Spark con formato Parquet y ubicada en Azure Storage, se crea una tabla externa en la base de datos de SQL On-Demand. Gracias a ello, puede apagar los grupos de Spark y seguir consultando las tablas externas de Spark desde SQL On-Demand.

Cuando se crean particiones de una en Spark, los archivos del almacenamiento se organizan por carpetas. SQL a petición usará los metadatos de la partición y solo se dirigirá a los archivos y carpetas pertinentes para la consulta.

La sincronización de metadatos se configura automáticamente para cada grupo de Spark aprovisionado en el área de trabajo de Azure Synapse. Puede empezar a realizar consultas en las tablas externas de Spark al instante.

Cada tabla externa en formato Parquet de Spark ubicada en Azure Storage se representa con una tabla externa en un esquema dbo que corresponde a una base de datos de SQL On-Demand (versión preliminar). 

En el caso de las consultas de las tablas externas de Spark, ejecute una consulta cuyo destino sea un [spark_table] externo. Antes de ejecutar el ejemplo siguiente, asegúrese de que tiene un [acceso correcto a la cuenta de almacenamiento](develop-storage-files-storage-access-control.md) en que se encuentran los archivos.

```sql
SELECT * FROM [db].dbo.[spark_table]
```

> [!NOTE]
> Las operaciones de agregar, quitar o modificar columnas de tablas externas de Spark no se reflejarán en tablas externas de SQL a petición.

## <a name="apache-spark-data-types-to-sql-data-types-mapping"></a>Asignación de tipos de datos de Apache Spark a tipos de datos de SQL

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
