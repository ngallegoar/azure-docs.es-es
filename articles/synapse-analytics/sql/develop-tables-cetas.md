---
title: CETAS en Synapse SQL
description: Uso de CETAS con Synapse SQL
services: synapse-analytics
author: filippopovic
ms.service: synapse-analytics
ms.topic: overview
ms.subservice: sql
ms.date: 04/15/2020
ms.author: fipopovi
ms.reviewer: jrasnick
ms.openlocfilehash: 18f472da30b34fcacd70bba9ea7371b56f1a7abf
ms.sourcegitcommit: 3fc3457b5a6d5773323237f6a06ccfb6955bfb2d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/11/2020
ms.locfileid: "90032918"
---
# <a name="cetas-with-synapse-sql"></a>CETAS con Synapse SQL

En el grupo de SQL o en SQL a petición (versión preliminar), puede usar CREATE EXTERNAL TABLE AS SELECT (CETAS) para completar las siguientes tareas:  

- Crear una tabla externa
- Exportar, en paralelo, los resultados de una instrucción SELECT de Transact-SQL a

  - Hadoop
  - Blob de Azure Storage
  - Azure Data Lake Storage Gen2

## <a name="cetas-in-sql-pool"></a>CETAS en un grupo de SQL

Para más información sobre el grupo de SQL, el uso y la sintaxis de CETAS, consulte el artículo [CREATE EXTERNAL TABLE AS SELECT](/sql/t-sql/statements/create-external-table-as-select-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest). Además, para obtener instrucciones sobre CTAS con el grupo de SQL, consulte el artículo [CREATE TABLE AS SELECT](/sql/t-sql/statements/create-table-as-select-azure-sql-data-warehouse?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest).

## <a name="cetas-in-sql-on-demand"></a>CETAS en SQL a petición

Cuando se utiliza el recurso de SQL a petición, CETAS se usa para crear una tabla externa y exportar los resultados de la consulta a Azure Storage Blob o Azure Data Lake Storage Gen2.

## <a name="syntax"></a>Sintaxis

```syntaxsql
CREATE EXTERNAL TABLE [ [database_name  . [ schema_name ] . ] | schema_name . ] table_name
    WITH (
        LOCATION = 'path_to_folder',  
        DATA_SOURCE = external_data_source_name,  
        FILE_FORMAT = external_file_format_name  
)
    AS <select_statement>  
[;]

<select_statement> ::=  
    [ WITH <common_table_expression> [ ,...n ] ]  
    SELECT <select_criteria>
```

## <a name="arguments"></a>Argumentos

*[ [ *database_name* . [ *schema_name* ] . ] | *schema_name* . ] *table_name**

Nombre de entre una y tres partes de la tabla que se va a crear. En el caso de una tabla externa, SQL a petición almacena solo los metadatos de la tabla. En SQL a petición no se mueven ni se almacenan datos reales.

LOCATION = *'path_to_folder'*

Especifica dónde se deben escribir los resultados de la instrucción SELECT en el origen de datos externo. La carpeta raíz es la ubicación de datos especificada en el origen de datos externo. La ubicación debe apuntar a una carpeta y tener un carácter "/" final. Ejemplo: aggregated_data/

DATA_SOURCE = *external_data_source_name*

Especifica el nombre del objeto de origen de datos externo que contiene la ubicación en donde se almacenarán los datos externos. Para crear un origen de datos externo, use [CREATE EXTERNAL DATA SOURCE (Transact-SQL)](develop-tables-external-tables.md#create-external-data-source).

FILE_FORMAT = *external_file_format_name*

Especifica el nombre del objeto de formato de archivo externo que contiene el formato del archivo de datos externos. Para crear un formato de archivo externo, use [CREATE EXTERNAL FILE FORMAT (Transact-SQL)](develop-tables-external-tables.md#create-external-file-format). Actualmente solo se admiten formatos de archivo externos con FORMAT="PARQUET".

WITH *<common_table_expression>*

Especifica un conjunto de resultados temporal con nombre, conocido como expresión de tabla común (CTE). Para más información, consulte [WITH common_table_expression (Transact-SQL)](/sql/t-sql/queries/with-common-table-expression-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest).

SELECT <select_criteria>

Rellena la nueva tabla con los resultados de una instrucción SELECT. *select_criteria* es el cuerpo de la instrucción SELECT que determina qué datos se copian en la nueva tabla. Para más información sobre las instrucciones SELECT, consulte [SELECT (Transact-SQL)](/sql/t-sql/queries/select-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest).

> [!NOTE]
> La cláusula ORDER BY de la parte SELECT de CETAS no se admite.

## <a name="permissions"></a>Permisos

Debe tener permisos para mostrar el contenido de la carpeta y escribir en la carpeta LOCATION de CETAS para que funcione.

## <a name="examples"></a>Ejemplos

En estos ejemplos se usa CETAS para guardar la población total agregada por año y estado a una carpeta aggregated_data que se encuentra en el origen de datos population_ds.

Este ejemplo se basa en las credenciales, el origen de datos y el formato de archivo externo que se crearon anteriormente. Consulte el documento [tablas externas](develop-tables-external-tables.md). Para guardar los resultados de la consulta en una carpeta diferente del mismo origen de datos, cambie el argumento LOCATION. 

Para guardar los resultados en una cuenta de almacenamiento distinta, cree y use un origen de datos diferente para el argumento DATA_SOURCE.

> [!NOTE]
> Los ejemplos siguientes usan una cuenta de almacenamiento pública de datos abiertos de Azure. Es de solo lectura. Para ejecutar estas consultas, debe proporcionar el origen de datos para el que tenga permisos de escritura.

```sql
-- use CETAS to export select statement with OPENROWSET result to  storage
CREATE EXTERNAL TABLE population_by_year_state
WITH (
    LOCATION = 'aggregated_data/',
    DATA_SOURCE = population_ds,  
    FILE_FORMAT = census_file_format
)  
AS
SELECT decennialTime, stateName, SUM(population) AS population
FROM
    OPENROWSET(BULK 'https://azureopendatastorage.blob.core.windows.net/censusdatacontainer/release/us_population_county/year=*/*.parquet',
    FORMAT='PARQUET') AS [r]
GROUP BY decennialTime, stateName
GO

-- you can query created external table
SELECT * FROM population_by_year_state
```

En el ejemplo siguiente se usa una tabla externa como origen de CETAS. Esta se basa en las credenciales, el origen de datos, el formato de archivo externo y la tabla externa que se crearon anteriormente. Consulte el documento [tablas externas](develop-tables-external-tables.md).

```sql
-- use CETAS with select from external table
CREATE EXTERNAL TABLE population_by_year_state
WITH (
    LOCATION = 'aggregated_data/',
    DATA_SOURCE = population_ds,  
    FILE_FORMAT = census_file_format
)  
AS
SELECT decennialTime, stateName, SUM(population) AS population
FROM census_external_table
GROUP BY decennialTime, stateName
GO

-- you can query created external table
SELECT * FROM population_by_year_state
```

## <a name="supported-data-types"></a>Tipos de datos admitidos

CETAS se puede usar para almacenar conjuntos de resultados con los siguientes tipos de datos de SQL:

- binary
- varbinary
- char
- varchar
- date
- time
- datetime2
- Decimal
- NUMERIC
- FLOAT
- real
- bigint
- int
- SMALLINT
- TINYINT
- bit

> [!NOTE]
> Las aplicaciones de línea de negocio no se pueden utilizar con CETAS.

Los siguientes tipos de datos no se pueden usar en la parte SELECT de CETAS:

- NCHAR
- NVARCHAR
- datetime
- smalldatetime
- datetimeoffset
- money
- SMALLMONEY
- UNIQUEIDENTIFIER

## <a name="next-steps"></a>Pasos siguientes

Puede probar a consultar [Tablas externas de Apache Spark para Azure Synapse](develop-storage-files-spark-tables.md).
