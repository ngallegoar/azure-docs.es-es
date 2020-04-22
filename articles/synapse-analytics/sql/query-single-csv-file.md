---
title: Consulta de archivos CSV con SQL a petición (versión preliminar)
description: En este artículo, aprenderá a consultar archivos CSV únicos con distintos formatos de archivo mediante SQL a petición (versión preliminar).
services: synapse analytics
author: azaricstefan
ms.service: synapse-analytics
ms.topic: how-to
ms.subservice: ''
ms.date: 04/15/2020
ms.author: v-stazar
ms.reviewer: jrasnick, carlrab
ms.openlocfilehash: 3d09692c06bcdffbb070f545950092592e417838
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/16/2020
ms.locfileid: "81427560"
---
# <a name="query-csv-files"></a>Consulta de archivo CSV

En este artículo, aprenderá a consultar un único archivo CSV con SQL a petición (versión preliminar) en Azure Synapse Analytics. Los archivos CSV pueden tener distintos formatos: 

- Con y sin una fila de encabezado
- Valores delimitados por comas y tabulaciones
- Finales de línea al estilo de Windows y Unix
- Valores entre comillas y sin comillas, y caracteres de escape

A continuación se abordan todas las variaciones anteriores.

## <a name="prerequisites"></a>Prerrequisitos

Antes de leer el resto de este artículo, consulte los siguientes artículos:

- [Primera configuración](query-data-storage.md#first-time-setup)
- [Requisitos previos](query-data-storage.md#prerequisites)

## <a name="windows-style-new-line"></a>Nueva línea al estilo de Windows

La consulta siguiente muestra cómo leer un archivo CSV sin una fila de encabezado, con una nueva línea al estilo de Windows y columnas delimitadas por comas.

Vista previa del archivo:

![Primeras 10 filas del archivo CSV sin encabezado, nueva línea al estilo de Windows.](./media/query-single-csv-file/population.png)

```sql
SELECT *
FROM OPENROWSET(
        BULK 'https://sqlondemandstorage.blob.core.windows.net/csv/population/population.csv',
         FORMAT = 'CSV',
        FIELDTERMINATOR =',',
        ROWTERMINATOR = '\n'
    )
WITH (
    [country_code] VARCHAR (5) COLLATE Latin1_General_BIN2,
    [country_name] VARCHAR (100) COLLATE Latin1_General_BIN2,
    [year] smallint,
    [population] bigint
) AS [r]
WHERE
    country_name = 'Luxembourg'
    AND year = 2017;
```

## <a name="unix-style-new-line"></a>Nueva línea al estilo de Unix

La consulta siguiente muestra cómo leer un archivo sin una fila de encabezado, con una nueva línea al estilo de Unix y columnas delimitadas por comas. Tenga en cuenta la ubicación diferente del archivo en comparación con los demás ejemplos.

Vista previa del archivo:

![Primeras 10 filas del archivo CSV sin fila de encabezado y con nueva línea al estilo de Unix.](./media/query-single-csv-file/population-unix.png)

```sql
SELECT *
FROM OPENROWSET(
        BULK 'https://sqlondemandstorage.blob.core.windows.net/csv/population-unix/population.csv',
        FORMAT = 'CSV',
        FIELDTERMINATOR =',',
        ROWTERMINATOR = '0x0a'
    )
WITH (
    [country_code] VARCHAR (5) COLLATE Latin1_General_BIN2,
    [country_name] VARCHAR (100) COLLATE Latin1_General_BIN2,
    [year] smallint,
    [population] bigint
) AS [r]
WHERE
    country_name = 'Luxembourg'
    AND year = 2017;
```

## <a name="header-row"></a>Fila de encabezado

La consulta siguiente muestra cómo leer un archivo con una fila de encabezado, con una nueva línea al estilo de Unix y columnas delimitadas por comas. Tenga en cuenta la ubicación diferente del archivo en comparación con los demás ejemplos.

Vista previa del archivo:

![Primeras 10 filas del archivo CSV con una fila de encabezado y con nueva línea al estilo de Unix.](./media/query-single-csv-file/population-unix-hdr.png)

```sql
SELECT *
FROM OPENROWSET(
        BULK 'https://sqlondemandstorage.blob.core.windows.net/csv/population-unix-hdr/population.csv',
        FORMAT = 'CSV',
        FIELDTERMINATOR =',',
        FIRSTROW = 2
    )
    WITH (
        [country_code] VARCHAR (5) COLLATE Latin1_General_BIN2,
        [country_name] VARCHAR (100) COLLATE Latin1_General_BIN2,
        [year] smallint,
        [population] bigint
    ) AS [r]
WHERE
    country_name = 'Luxembourg'
    AND year = 2017;
```

## <a name="custom-quote-character"></a>Carácter de comillas personalizado

La consulta siguiente muestra cómo leer un archivo con una fila de encabezado, con una nueva línea al estilo de Unix, columnas delimitadas por comas y valores entre comillas. Tenga en cuenta la ubicación diferente del archivo en comparación con los demás ejemplos.

Vista previa del archivo:

![Primeras 10 filas del archivo CSV con una fila de encabezado, con nueva línea al estilo de Unix y valores entre comillas.](./media/query-single-csv-file/population-unix-hdr-quoted.png)

```sql
SELECT *
FROM OPENROWSET(
        BULK 'https://sqlondemandstorage.blob.core.windows.net/csv/population-unix-hdr-quoted/population.csv',
        FORMAT = 'CSV',
        FIELDTERMINATOR =',',
        ROWTERMINATOR = '0x0a',
        FIRSTROW = 2,
        FIELDQUOTE = '"'
    )
    WITH (
        [country_code] VARCHAR (5) COLLATE Latin1_General_BIN2,
        [country_name] VARCHAR (100) COLLATE Latin1_General_BIN2,
        [year] smallint,
        [population] bigint
    ) AS [r]
WHERE
    country_name = 'Luxembourg'
    AND year = 2017;
```

> [!NOTE]
> Esta consulta devolvería los mismos resultados si se omitiese el parámetro FIELDQUOTE, ya que el valor predeterminado de FIELDQUOTE es una comilla doble.

## <a name="escaping-characters"></a>Caracteres de escape

La consulta siguiente muestra cómo leer un archivo con una fila de encabezado, con una nueva línea al estilo de Unix, columnas delimitadas por comas y un carácter de escape usado para el delimitador de campos (coma) sin valores. Tenga en cuenta la ubicación diferente del archivo en comparación con los demás ejemplos.

Vista previa del archivo:

![Primeras 10 filas del archivo CSV con una fila de encabezado, con nueva línea al estilo de Unix y un carácter de escape usado para el delimitador de campos.](./media/query-single-csv-file/population-unix-hdr-escape.png)

```sql
SELECT *
FROM OPENROWSET(
        BULK 'https://sqlondemandstorage.blob.core.windows.net/csv/population-unix-hdr-escape/population.csv',
        FORMAT = 'CSV',
        FIELDTERMINATOR =',',
        ROWTERMINATOR = '0x0a',
        FIRSTROW = 2,
        ESCAPECHAR = '\\'
    )
    WITH (
        [country_code] VARCHAR (5) COLLATE Latin1_General_BIN2,
        [country_name] VARCHAR (100) COLLATE Latin1_General_BIN2,
        [year] smallint,
        [population] bigint
    ) AS [r]
WHERE
    country_name = 'Slov,enia';
```

> [!NOTE]
> Esta consulta produciría un error si no se especificara ESCAPECHAR, ya que la coma en "Slov,enia" se trataría como delimitador de campos en lugar de como parte del nombre del país. "Slov,enia" se trataría como dos columnas. Por lo tanto, la fila en particular tendría una columna más que las demás filas y una columna más de la definida en la cláusula WITH.

## <a name="tab-delimited-files"></a>Archivos delimitados por tabulaciones

La consulta siguiente muestra cómo leer un archivo con una fila de encabezado, con una nueva línea al estilo de Unix y columnas delimitadas por tabulaciones. Tenga en cuenta la ubicación diferente del archivo en comparación con los demás ejemplos.

Vista previa del archivo:

![Primeras 10 filas del archivo CSV con una fila de encabezado, con nueva línea al estilo de Unix y delimitador de tabulaciones.](./media/query-single-csv-file/population-unix-hdr-tsv.png)

```sql
SELECT *
FROM OPENROWSET(
        BULK 'https://sqlondemandstorage.blob.core.windows.net/csv/population-unix-hdr-tsv/population.csv',
        FORMAT = 'CSV',
        FIELDTERMINATOR ='\t',
        ROWTERMINATOR = '0x0a',
        FIRSTROW = 2
    )
    WITH (
        [country_code] VARCHAR (5) COLLATE Latin1_General_BIN2,
        [country_name] VARCHAR (100) COLLATE Latin1_General_BIN2,
        [year] smallint,
        [population] bigint
    ) AS [r]
WHERE
    country_name = 'Luxembourg'
    AND year = 2017
```

## <a name="returning-subset-of-columns"></a>Devolución del subconjunto de columnas

Hasta ahora, ha especificado el esquema de archivos CSV mediante WITH y enumerando todas las columnas. Solo puede especificar las columnas que realmente necesita en la consulta mediante un número ordinal para cada columna necesaria. También omitirá las columnas que no sean de interés.

La consulta siguiente devuelve el número de nombres de país distintos incluidos en un archivo, especificando solo las columnas necesarias:

> [!NOTE]
> Eche un vistazo a la cláusula WITH en la consulta siguiente y observe que hay "2" (sin comillas) al final de la fila en la que se define la columna *[country_name]* . Significa que la columna *[country_name]* es la segunda columna del archivo. La consulta omitirá todas las columnas del archivo excepto la segunda.

```sql
SELECT
    COUNT(DISTINCT country_name) AS countries
FROM OPENROWSET(
        BULK 'https://sqlondemandstorage.blob.core.windows.net/csv/population/population.csv',
         FORMAT = 'CSV',
        FIELDTERMINATOR =',',
        ROWTERMINATOR = '\n'
    )
WITH (
    --[country_code] VARCHAR (5) COLLATE Latin1_General_BIN2,
    [country_name] VARCHAR (100) COLLATE Latin1_General_BIN2 2
    --[year] smallint,
    --[population] bigint
) AS [r]
```

## <a name="next-steps"></a>Pasos siguientes

En los artículos siguientes obtendrá información sobre:

- [Consulta de archivos de Parquet](query-parquet-files.md)
- [Consulta de carpetas y varios archivos](query-folders-multiple-csv-files.md)
