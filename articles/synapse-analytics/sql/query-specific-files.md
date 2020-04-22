---
title: Uso de metadatos de archivo en consultas
description: La función OPENROWSET proporciona información sobre archivos y rutas de acceso de todos los archivos que se usan en la consulta para filtrar o analizar datos según el nombre de archivo o la ruta de acceso de la carpeta.
services: synapse-analytics
author: azaricstefan
ms.service: synapse-analytics
ms.topic: how-to
ms.subservice: ''
ms.date: 04/15/2020
ms.author: v-stazar
ms.reviewer: jrasnick, carlrab
ms.openlocfilehash: 40a8e2c153ec3d8e7b4007340b9433a38f9ccc89
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/16/2020
ms.locfileid: "81427548"
---
# <a name="using-file-metadata-in-queries"></a>Uso de metadatos de archivo en consultas

El servicio de consultas de SQL a petición puede tratar varios archivos y carpetas, tal y como se describe en el artículo [Consulta de carpetas y varios archivos](query-folders-multiple-csv-files.md). En este artículo, aprenderá a usar la información de los metadatos sobre los nombres de archivos y carpetas en las consultas.

A veces, puede que necesite saber qué origen de archivo o carpeta se correlaciona con una fila específica en el conjunto de resultados.

Puede usar las funciones `filepath` y `filename` para devolver nombres de archivo o la ruta de acceso en el conjunto de resultados. O bien puede usarlas para filtrar los datos según el nombre de archivo o la ruta de acceso de la carpeta. Estas funciones se describen en las secciones [Función filename](develop-storage-files-overview.md#filename-function) y [Función filepath](develop-storage-files-overview.md#filepath-function) de la sintaxis. A continuación encontrará breves descripciones, junto con ejemplos.

## <a name="prerequisites"></a>Prerrequisitos

Antes de leer el resto de este artículo, consulte los siguientes requisitos previos:

- [Primera configuración](query-data-storage.md#first-time-setup)
- [Requisitos previos](query-data-storage.md#prerequisites)

## <a name="functions"></a>Functions

### <a name="filename"></a>Nombre de archivo

Esta función devuelve el nombre de archivo del que se origina la fila.

En el ejemplo siguiente se leen los archivos de datos de NYC Yellow Taxi de los últimos tres meses de 2017 y se devuelve el número de carreras por archivo. La parte de OPENROWSET de la consulta especifica qué archivos se leerán.

```sql
SELECT
    r.filename() AS [filename]
    ,COUNT_BIG(*) AS [rows]
FROM OPENROWSET(
        BULK 'https://sqlondemandstorage.blob.core.windows.net/parquet/taxi/year=2017/month=9/*.parquet',
        FORMAT='PARQUET') AS [r]
GROUP BY
    r.filename()
ORDER BY
    [filename];
```

En el ejemplo siguiente se muestra cómo se puede usar *filename()* en la cláusula WHERE para filtrar los archivos que se van a leer. Accede a toda la carpeta en la parte de OPENROWSET de la consulta y filtra los archivos en la cláusula WHERE.

Los resultados serán los mismos que en el ejemplo anterior.

```sql
SELECT
    r.filename() AS [filename]
    ,COUNT_BIG(*) AS [rows]
FROM OPENROWSET(
    BULK 'https://sqlondemandstorage.blob.core.windows.net/parquet/taxi/year=2017/month=9/*.parquet',
    FORMAT='PARQUET') AS [r]
WHERE
    r.filename() IN ('yellow_tripdata_2017-10.parquet', 'yellow_tripdata_2017-11.parquet', 'yellow_tripdata_2017-12.parquet')
GROUP BY
    r.filename()
ORDER BY
    [filename];
```

### <a name="filepath"></a>Filepath

La función filepath devuelve una ruta de acceso completa o parcial:

- Cuando se la llama sin ningún parámetro, devuelve la ruta de acceso completa al archivo del que se origina la fila.
- Cuando se la llama con un parámetro, devuelve la parte de la ruta de acceso que coincide con el carácter comodín en la posición especificada del parámetro. Por ejemplo, el valor 1 del parámetro devolvería la parte de la ruta de acceso que coincide con el primer carácter comodín.

En el ejemplo siguiente se leen archivos de datos de NYC Yellow Taxi para los últimos tres meses de 2017. Devuelve el número de carreras por ruta de acceso de archivo. La parte de OPENROWSET de la consulta especifica qué archivos se leerán.

```sql
SELECT
    r.filepath() AS filepath
    ,COUNT_BIG(*) AS [rows]
FROM OPENROWSET(
        BULK 'https://sqlondemandstorage.blob.core.windows.net/csv/taxi/yellow_tripdata_2017-1*.csv',
        FORMAT = 'CSV',
        FIRSTROW = 2
    )
    WITH (
        vendor_id INT,
        pickup_datetime DATETIME2,
        dropoff_datetime DATETIME2,
        passenger_count SMALLINT,
        trip_distance FLOAT,
        rate_code SMALLINT,
        store_and_fwd_flag SMALLINT,
        pickup_location_id INT,
        dropoff_location_id INT,
        payment_type SMALLINT,
        fare_amount FLOAT,
        extra FLOAT,
        mta_tax FLOAT,
        tip_amount FLOAT,
        tolls_amount FLOAT,
        improvement_surcharge FLOAT,
        total_amount FLOAT
    ) AS [r]
GROUP BY
    r.filepath()
ORDER BY
    filepath;
```

En el ejemplo siguiente se muestra cómo se puede usar *filepath()* en la cláusula WHERE para filtrar los archivos que se van a leer.

Puede usar los caracteres comodín en la parte de OPENROWSET de la consulta y filtrar los archivos en la cláusula WHERE. Los resultados serán los mismos que en el ejemplo anterior.

```sql
SELECT
    r.filepath() AS filepath
    ,r.filepath(1) AS [year]
    ,r.filepath(2) AS [month]
    ,COUNT_BIG(*) AS [rows]
FROM OPENROWSET(
        BULK 'https://sqlondemandstorage.blob.core.windows.net/csv/taxi/yellow_tripdata_*-*.csv',
        FORMAT = 'CSV',
        FIRSTROW = 2
    )
WITH (
    vendor_id INT,
    pickup_datetime DATETIME2,
    dropoff_datetime DATETIME2,
    passenger_count SMALLINT,
    trip_distance FLOAT,
    rate_code SMALLINT,
    store_and_fwd_flag SMALLINT,
    pickup_location_id INT,
    dropoff_location_id INT,
    payment_type SMALLINT,
    fare_amount FLOAT,
    extra FLOAT,
    mta_tax FLOAT,
    tip_amount FLOAT,
    tolls_amount FLOAT,
    improvement_surcharge FLOAT,
    total_amount FLOAT
) AS [r]
WHERE
    r.filepath(1) IN ('2017')
    AND r.filepath(2) IN ('10', '11', '12')
GROUP BY
    r.filepath()
    ,r.filepath(1)
    ,r.filepath(2)
ORDER BY
    filepath;
```

## <a name="next-steps"></a>Pasos siguientes

En el artículo siguiente, aprenderá a [consultar archivos de Parquet](query-parquet-files.md).
