---
title: Procedimientos recomendados de SQL a petición (versión preliminar) en Azure Synapse Analytics
description: Recomendaciones y procedimientos recomendados que debe saber para trabajar con SQL a petición (versión preliminar).
services: synapse-analytics
author: filippopovic
manager: craigg
ms.service: synapse-analytics
ms.topic: conceptual
ms.subservice: ''
ms.date: 05/01/2020
ms.author: fipopovi
ms.reviewer: jrasnick
ms.openlocfilehash: 86678365d1510199247e8a1aaa48ec844d07de32
ms.sourcegitcommit: bb0afd0df5563cc53f76a642fd8fc709e366568b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/19/2020
ms.locfileid: "83592940"
---
# <a name="best-practices-for-sql-on-demand-preview-in-azure-synapse-analytics"></a>Procedimientos recomendados de SQL a petición (versión preliminar) en Azure Synapse Analytics

En este artículo, encontrará una colección de procedimientos recomendados para usar SQL a petición (versión preliminar). SQL a petición es un recurso adicional incluido en Azure Synapse Analytics.

## <a name="general-considerations"></a>Consideraciones generales

SQL a petición permite consultar archivos de las cuentas de almacenamiento de Azure. No tiene funcionalidades de ingesta o almacenamiento local. Por ello, todos los archivos de destino de la consulta son externos a SQL a petición. Todo lo relacionado con la lectura de archivos desde el almacenamiento puede afectar el rendimiento de las consultas.

## <a name="colocate-azure-storage-account-and-sql-on-demand"></a>Colocación de una cuenta de Azure Storage y SQL a petición

Para minimizar la latencia, coloque su cuenta de Azure Storage y el punto de conexión de SQL a petición. Las cuentas de almacenamiento y los puntos de conexión aprovisionados durante la creación del área de trabajo se encuentran en la misma región.

Para obtener un rendimiento óptimo, si tiene acceso a otras cuentas de almacenamiento con SQL a petición, asegúrese de que se encuentran en la misma región. Si no están en la misma región, aumentará la latencia de la transferencia de red de los datos entre la región remota y la del punto de conexión.

## <a name="azure-storage-throttling"></a>Limitaciones de Azure Storage

Varias aplicaciones y servicios pueden acceder a la cuenta de almacenamiento. Las limitaciones de Storage se producen cuando las IOPS o el rendimiento combinados que generan las aplicaciones, los servicios y la carga de trabajo a petición de SQL superan los límites de la cuenta de almacenamiento. Como resultado, se producirá un efecto negativo significativo en el rendimiento de las consultas.

Una vez detectada la limitación, SQL a petición tiene controles integrados para este escenario. SQL a petición realizará solicitudes al almacenamiento a un ritmo más lento hasta que la limitación se resuelva.

> [!TIP]
> Para que la ejecución de las consultas sea óptima, no debe forzar la cuenta de almacenamiento con otras cargas de trabajo durante la ejecución de consultas.

## <a name="prepare-files-for-querying"></a>Preparación de los archivos para la consulta

Si es posible, puede preparar los archivos para mejorar el rendimiento:

- Convierta los archivos CSV y JSON a Parquet: Parquet es un formato de columnas. Dado que está comprimido, sus tamaños de archivo son más pequeños que los archivos CSV y JSON con los mismos datos. SQL a petición necesitará menos tiempo y solicitudes de almacenamiento para leerlos.
- Si una consulta tiene como destino un solo archivo de gran tamaño, se beneficiará de dividirlo en varios archivos más pequeños.
- Pruebe a mantener el tamaño del archivo CSV por debajo de los 10 GB.
- Es mejor tener archivos de igual tamaño para una sola ruta de acceso OPENROWSET o una ubicación de tabla externa.
- Particione los datos al almacenar las particiones en diferentes carpetas o con nombres de archivo distintos: consulte [Uso de las funciones filename y filepath para seleccionar particiones específicas](#use-fileinfo-and-filepath-functions-to-target-specific-partitions).

## <a name="push-wildcards-to-lower-levels-in-path"></a>Inserción de caracteres comodín en los niveles inferiores de la ruta de acceso

Puede usar caracteres comodín en la ruta de acceso para [consultar varios archivos y carpetas](develop-storage-files-overview.md#query-multiple-files-or-folders). SQL a petición muestra los archivos de la cuenta de almacenamiento a partir del primer "*" mediante la API de almacenamiento y elimina los archivos que no coinciden con la ruta de acceso especificada. Al reducir la lista inicial de archivos, puede mejorar el rendimiento si hay muchos archivos que coincidan con la ruta de acceso especificada hasta el primer carácter comodín.

## <a name="use-appropriate-data-types"></a>Uso del tipo de datos adecuado

Los tipos de datos que se usan en la consulta afectan al rendimiento. Puede obtener un mejor rendimiento si: 

- Usa el tamaño de datos más pequeño que se adapte al mayor valor posible.
  - Si la longitud máxima de caracteres es de 30 caracteres, utilice el tipo de datos de caracteres de longitud 30.
  - Si todos los valores de columnas de caracteres tienen un tamaño fijo, use char o nchar. De lo contrario, use varchar o nvarchar.
  - Si el valor máximo de la columna de enteros es 500, use smallint, ya que es el tipo de datos más pequeño que puede contener este valor. Puede encontrar intervalos de tipos de datos enteros [aquí](https://docs.microsoft.com/sql/t-sql/data-types/int-bigint-smallint-and-tinyint-transact-sql?view=sql-server-ver15).
- Si es posible, use varchar y char en lugar de nvarchar y nchar.
- Utilice tipos de datos basados en enteros si es posible. Las operaciones de ordenación, combinación y agrupación se realizan más rápidamente en números enteros que en datos de caracteres.
- Si usa la inferencia de esquemas, [compruebe el tipo de datos inferido](#check-inferred-data-types).

## <a name="check-inferred-data-types"></a>Comprobación de los tipos de datos inferidos

La [inferencia de esquemas](query-parquet-files.md#automatic-schema-inference) ayuda a escribir consultas rápidamente y a explorar los datos sin conocer el esquema de archivo. Como contrapartida, los tipos de datos deducidos resultan ser más grandes de lo que realmente son. Esto sucede cuando no hay suficiente información en los archivos de código fuente para asegurarse de que se utiliza el tipo de datos adecuado. Por ejemplo, los archivos de Parquet no contienen metadatos sobre la longitud máxima de la columna de caracteres y SQL a petición los deduce como varchar(8000). 

Puede comprobar los tipos de datos resultantes de la consulta mediante [sp_describe_first_results_set](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/sp-describe-first-result-set-transact-sql?view=sql-server-ver15).

En el ejemplo siguiente se muestra cómo se pueden optimizar los tipos de datos inferidos. El procedimiento se usa para mostrar tipos de datos inferidos. 
```sql  
EXEC sp_describe_first_result_set N'
    SELECT
        vendor_id, pickup_datetime, passenger_count
    FROM 
        OPENROWSET(
            BULK ''https://sqlondemandstorage.blob.core.windows.net/parquet/taxi/*/*/*'',
            FORMAT=''PARQUET''
        ) AS nyc';
```

El conjunto de resultados es el siguiente:

|is_hidden|column_ordinal|name|system_type_name|max_length|
|----------------|---------------------|----------|--------------------|-------------------||
|0|1|vendor_id|varchar(8000)|8000|
|0|2|pickup_datetime|datetime2(7)|8|
|0|3|passenger_count|int|4|

Una vez que se conocen los tipos de datos inferidos para la consulta se pueden especificar los tipos de datos adecuados:

```sql  
SELECT
    vendor_id, pickup_datetime, passenger_count
FROM 
    OPENROWSET(
        BULK 'https://sqlondemandstorage.blob.core.windows.net/parquet/taxi/*/*/*',
        FORMAT='PARQUET'
    ) 
    WITH (
        vendor_id varchar(4), -- we used length of 4 instead of inferred 8000
        pickup_datetime datetime2,
        passenger_count int
    ) AS nyc;
```

## <a name="use-fileinfo-and-filepath-functions-to-target-specific-partitions"></a>Uso de las funciones fileinfo y filepath para seleccionar particiones específicas

A menudo, los datos se organizan en particiones. Puede indicar a SQL a petición que consulte archivos y carpetas concretos. Esta función reduce el número de archivos y la cantidad de datos que la consulta necesita leer y procesar. Como ventaja adicional, logrará un mejor rendimiento.

Para obtener más información, consulte las funciones [filename](develop-storage-files-overview.md#filename-function) y [filepath](develop-storage-files-overview.md#filepath-function), así como ejemplos sobre cómo [consultar archivos específicos](query-specific-files.md).

> [!TIP]
> Convierta siempre el resultado de las funciones filepath y fileinfo al tipo de datos adecuado. Si usa tipos de datos de caracteres, asegúrese de que se usa la longitud apropiada.

> [!NOTE]
> Las funciones usadas para la eliminación de particiones, FilePath y FileInfo, no se admiten actualmente para tablas externas que no sean las creadas automáticamente para cada tabla creada en Apache Spark para Azure Synapse Analytics.

Si los datos almacenados no tienen particiones, considere la posibilidad de crear particiones para usar estas funciones en la optimización de las consultas que están dirigidas a esos archivos. Cuando [consulte las tablas de Spark con particiones](develop-storage-files-spark-tables.md) de SQL a petición, la consulta tendrá como destino automático solo los archivos necesarios.

## <a name="use-parser_version-20-for-querying-csv-files"></a>Uso de PARSER_VERSION 2.0 para consultar archivos CSV

Puede usar el analizador optimizado para rendimiento al consultar archivos CSV. Consulte [PARSER_VERSION](develop-openrowset.md) para obtener más información.

## <a name="use-cetas-to-enhance-query-performance-and-joins"></a>Uso de CETAS para mejorar el rendimiento de las consultas y las combinaciones

[CETAS](develop-tables-cetas.md) es una de las características más importantes que están disponibles en SQL a petición. CETAS es una operación en paralelo que crea metadatos de tabla externa y exporta los resultados de la consulta SELECT a un conjunto de archivos de la cuenta de almacenamiento.

Puede usar CETAS para almacenar en un nuevo conjunto de archivos las partes más frecuentemente usadas de las consultas, como las tablas de referencia combinadas. A continuación, puede realizar combinaciones con esta tabla externa única, en lugar de repetir combinaciones comunes en varias consultas.

Cuando CETAS genera archivos Parquet, las estadísticas se crean automáticamente cuando la primera consulta selecciona como destino a esta tabla externa, lo que mejora el rendimiento.

## <a name="aad-pass-through-performance"></a>Rendimiento de paso a través de AAD

SQL a petición permite tener acceso a archivos en el almacenamiento mediante la credencial de SAS o de paso a través de AAD. Es posible que experimente un rendimiento más lento con el paso a través de AAD en comparación con SAS. 

Si necesita un mejor rendimiento, pruebe las credenciales de SAS para tener acceso al almacenamiento hasta que mejore el rendimiento de paso a través de AAD.

## <a name="next-steps"></a>Pasos siguientes

Consulte el artículo [Solución de problemas](../sql-data-warehouse/sql-data-warehouse-troubleshoot.md?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json) para conocer los problemas comunes y sus soluciones. Si va a trabajar con el grupo de SQL en lugar de SQL a petición, consulte el artículo [Procedimientos recomendados para el grupo de SQL](best-practices-sql-pool.md) para obtener instrucciones específicas.
