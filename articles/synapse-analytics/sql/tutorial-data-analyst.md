---
title: 'Tutorial para analistas de datos: uso de SQL a petición (versión preliminar) para analizar Azure Open Datasets en Azure Synapse Studio (versión preliminar)'
description: En este tutorial, aprenderá a realizar con facilidad el análisis de datos exploratorios mediante la combinación de diferentes Azure Open Datasets con SQL a petición (versión preliminar) y a visualizar los resultados en Azure Synapse Studio.
services: synapse-analytics
author: azaricstefan
ms.service: synapse-analytics
ms.topic: tutorial
ms.subservice: ''
ms.date: 04/15/2020
ms.author: v-stazar
ms.reviewer: jrasnick, carlrab
ms.openlocfilehash: 1080001cb222f91503080914d7fb253e5ee82626
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/16/2020
ms.locfileid: "81426284"
---
# <a name="use-sql-on-demand-preview-to-analyze-azure-open-datasets-and-visualize-the-results-in-azure-synapse-studio-preview"></a>Uso de SQL a petición (versión preliminar) para analizar Azure Open Datasets y visualizar los resultados en Azure Synapse Studio (versión preliminar)

En este tutorial, aprenderá a realizar el análisis de datos exploratorios mediante la combinación de diferentes Azure Open Datasets con SQL a petición y, después, visualizando los resultados en Azure Synapse Studio.

En concreto, se analiza el [conjunto de datos de taxi de Nueva York](https://azure.microsoft.com/services/open-datasets/catalog/nyc-taxi-limousine-commission-yellow-taxi-trip-records/) que incluye fechas y horas de recogida y llegada, las ubicaciones de recogida y llegada, las distancias del viaje, las tarifas detalladas, los tipos de tarifas, los tipos de pago y el número de pasajeros comunicados por el conductor.

El objetivo del análisis es encontrar tendencias en las variaciones de números de viajes de taxi a lo largo del tiempo. Puede analizar otros dos Azure Open Datasets (los de [días festivos públicos](https://azure.microsoft.com/services/open-datasets/catalog/public-holidays/) y [datos meteorológicos](https://azure.microsoft.com/services/open-datasets/catalog/noaa-integrated-surface-data/)) para comprender los valores atípicos en el número de viajes de taxi.

## <a name="create-credentials"></a>Crear credenciales

```sql
-- There is no secret. We are using public storage account which doesn't need a secret.
CREATE CREDENTIAL [https://azureopendatastorage.blob.core.windows.net/nyctlc]
WITH IDENTITY='SHARED ACCESS SIGNATURE',
SECRET = ''
GO

CREATE CREDENTIAL [https://azureopendatastorage.blob.core.windows.net/holidaydatacontainer]
WITH IDENTITY='SHARED ACCESS SIGNATURE',
SECRET = ''
GO

CREATE CREDENTIAL [https://azureopendatastorage.blob.core.windows.net/isdweatherdatacontainer]
WITH IDENTITY='SHARED ACCESS SIGNATURE',
SECRET = ''
GO
```

## <a name="automatic-schema-inference"></a>Inferencia automática del esquema

Puesto que los datos se almacenan en formato de archivo Parquet, la inferencia de esquemas automática está disponible, de modo que se pueden consultar fácilmente los datos sin necesidad de enumerar los tipos de datos de todas las columnas de los archivos. Además, puede utilizarse el mecanismo de la columna virtual y la función FilePath para filtrar un determinado subconjunto de archivos.

En primer lugar, vamos a familiarizarnos con los datos de los taxis de Nueva York mediante la ejecución de la siguiente consulta:

```sql
SELECT TOP 100 * FROM
    OPENROWSET(
        BULK 'https://azureopendatastorage.blob.core.windows.net/nyctlc/yellow/puYear=*/puMonth=*/*.parquet',
        FORMAT='PARQUET'
    ) AS [nyc]
```

A continuación se muestra el fragmento de código de resultado para los datos de los taxis de Nueva York:

![fragmento de código de resultado](./media/tutorial-data-analyst/1.png)

Del mismo modo, se puede consultar el conjunto de datos de los días festivos públicos con la siguiente consulta:

```sql
SELECT TOP 100 * FROM
    OPENROWSET(
        BULK 'https://azureopendatastorage.blob.core.windows.net/holidaydatacontainer/Processed/*.parquet',
        FORMAT='PARQUET'
    ) AS [holidays]
```

A continuación se muestra el fragmento de código de resultado para el conjunto de datos de los días festivos públicos:

![fragmento de código de resultado 2](./media/tutorial-data-analyst/2.png)

Por último, también se puede consultar el conjunto de datos meteorológicos mediante la siguiente consulta:

```sql
SELECT
    TOP 100 *
FROM  
    OPENROWSET(
        BULK 'https://azureopendatastorage.blob.core.windows.net/isdweatherdatacontainer/ISDWeather/year=*/month=*/*.parquet',
        FORMAT='PARQUET'
    ) AS [weather]
```

A continuación, se muestra el fragmento de código resultado para el conjunto de datos meteorológicos:

![fragmento de código de resultado 3](./media/tutorial-data-analyst/3.png)

Puede obtener más información sobre el significado de las columnas individuales en las descripciones de los conjuntos de datos de [taxis de Nueva York](https://azure.microsoft.com/services/open-datasets/catalog/nyc-taxi-limousine-commission-yellow-taxi-trip-records/), [días festivos](https://azure.microsoft.com/services/open-datasets/catalog/public-holidays/) y [datos meteorológicos](https://azure.microsoft.com/services/open-datasets/catalog/noaa-integrated-surface-data/).

## <a name="time-series-seasonality-and-outlier-analysis"></a>Análisis de series temporales, estacionalidad y valores atípicos

Puede resumir fácilmente el número anual de viajes de taxi con la siguiente consulta:

```sql
SELECT
    YEAR(tpepPickupDateTime) AS current_year,
    COUNT(*) AS rides_per_year
FROM
    OPENROWSET(
        BULK 'https://azureopendatastorage.blob.core.windows.net/nyctlc/yellow/puYear=*/puMonth=*/*.parquet',
        FORMAT='PARQUET'
    ) AS [nyc]
WHERE nyc.filepath(1) >= '2009' AND nyc.filepath(1) <= '2019'
GROUP BY YEAR(tpepPickupDateTime)
ORDER BY 1 ASC
```

A continuación se muestra el fragmento de código de resultado para el número anual de viajes de taxi:

![fragmento de código de resultado 4](./media/tutorial-data-analyst/4.png)

Los datos se pueden visualizar en Synapse Studio si cambia de la vista de tabla a la de gráfico. Puede elegir entre diferentes tipos de gráficos (de área, de barras, de columnas, de líneas, circulares y de dispersión). En este caso, vamos a trazar el gráfico de columnas con la columna de categoría establecida en "current_year":

![visualización del resultado 5](./media/tutorial-data-analyst/5.png)

En esta visualización, se puede ver claramente una tendencia decreciente en el número de viajes a lo largo de los años, probablemente debida a la reciente popularidad en aumento de las compañías que permiten compartir el viaje.

> [!NOTE]
> En el momento de redactar este tutorial, los datos de 2019 están incompletos, por lo que hay una gran caída en el número de viajes de ese año.

A continuación, vamos a centrar el análisis en un solo año, por ejemplo, 2016. La siguiente consulta devuelve el número diario de viajes durante ese año:

```sql
SELECT
    CAST([tpepPickupDateTime] AS DATE) AS [current_day],
    COUNT(*) as rides_per_day
FROM
    OPENROWSET(
        BULK 'https://azureopendatastorage.blob.core.windows.net/nyctlc/yellow/puYear=*/puMonth=*/*.parquet',
        FORMAT='PARQUET'
    ) AS [nyc]
WHERE nyc.filepath(1) = '2016'
GROUP BY CAST([tpepPickupDateTime] AS DATE)
ORDER BY 1 ASC
```

A continuación se muestra el fragmento de código de resultado para esta consulta:

![fragmento de código de resultado 6](./media/tutorial-data-analyst/6.png)

Una vez más, se pueden visualizar datos fácilmente si se traza el gráfico de columnas con la columna de categoría "current_day" y la columna de leyenda (series) "rides_per_day".

![visualización del resultado 7](./media/tutorial-data-analyst/7.png)

En el trazado, se puede observar que hay un patrón semanal, con el pico del sábado. Durante los meses de verano, hay menos viajes de taxi debido al período de vacaciones. Sin embargo, también hay algunas reducciones significativas en el número de viajes de taxi sin un patrón claro de cuándo y por qué se producen.

A continuación, vamos a ver si esas reducciones pueden estar correlacionadas con las festividades locales. Para ello, uniremos los viajes de los taxis de Nueva York con el conjunto de datos de días festivos públicos:

```sql
WITH taxi_rides AS
(
    SELECT
        CAST([tpepPickupDateTime] AS DATE) AS [current_day],
        COUNT(*) as rides_per_day
    FROM  
        OPENROWSET(
            BULK 'https://azureopendatastorage.blob.core.windows.net/nyctlc/yellow/puYear=*/puMonth=*/*.parquet',
            FORMAT='PARQUET'
        ) AS [nyc]
    WHERE nyc.filepath(1) = '2016'
    GROUP BY CAST([tpepPickupDateTime] AS DATE)
),
public_holidays AS
(
    SELECT
        holidayname as holiday,
        date
    FROM
        OPENROWSET(
            BULK 'https://azureopendatastorage.blob.core.windows.net/holidaydatacontainer/Processed/*.parquet',
            FORMAT='PARQUET'
        ) AS [holidays]
    WHERE countryorregion = 'United States' AND YEAR(date) = 2016
)
SELECT
*
FROM taxi_rides t
LEFT OUTER JOIN public_holidays p on t.current_day = p.date
ORDER BY current_day ASC
```

![visualización del resultado 8](./media/tutorial-data-analyst/8.png)

Esta vez, queremos resaltar el número de viajes de taxi durante los días festivos públicos. Para ello, elegiremos "none" en la columna de categoría y "rides_per_day" y "holiday" como columnas de leyenda (series).

![visualización del resultado 9](./media/tutorial-data-analyst/9.png)

En el trazado, se puede ver claramente que, durante los días festivos públicos, la cifra de viajes de taxi es menor. Sin embargo, todavía hay una gran reducción inexplicable el 23 de enero. Vamos a comprobar el tiempo en Nueva York en ese día consultando el conjunto de datos meteorológicos:

```sql
SELECT
    AVG(windspeed) AS avg_windspeed,
    MIN(windspeed) AS min_windspeed,
    MAX(windspeed) AS max_windspeed,
    AVG(temperature) AS avg_temperature,
    MIN(temperature) AS min_temperature,
    MAX(temperature) AS max_temperature,
    AVG(sealvlpressure) AS avg_sealvlpressure,
    MIN(sealvlpressure) AS min_sealvlpressure,
    MAX(sealvlpressure) AS max_sealvlpressure,
    AVG(precipdepth) AS avg_precipdepth,
    MIN(precipdepth) AS min_precipdepth,
    MAX(precipdepth) AS max_precipdepth,
    AVG(snowdepth) AS avg_snowdepth,
    MIN(snowdepth) AS min_snowdepth,
    MAX(snowdepth) AS max_snowdepth
FROM
    OPENROWSET(
        BULK 'https://azureopendatastorage.blob.core.windows.net/isdweatherdatacontainer/ISDWeather/year=*/month=*/*.parquet',
        FORMAT='PARQUET'
    ) AS [weather]
WHERE countryorregion = 'US' AND CAST([datetime] AS DATE) = '2016-01-23' AND stationname = 'JOHN F KENNEDY INTERNATIONAL AIRPORT'
```

![visualización del resultado 10](./media/tutorial-data-analyst/10.png)

Los resultados de la consulta indican que la reducción del número de viajes de taxi se debió a:

- Una tormenta de nieve ese día en Nueva York, ya que había mucha nieve (~30 cm)
- Hacía frío (una temperatura inferior a cero grados Celsius)
- Hizo viento (~10m/s)

En este tutorial se ha mostrado cómo el analista de datos puede realizar rápidamente análisis de datos exploratorios, combinar de forma sencilla diferentes conjuntos de datos mediante SQL a petición y visualizar los resultados con Azure Synapse Studio.

## <a name="next-steps"></a>Pasos siguientes

Consulte el artículo [Conexión de SQL a petición a Power BI Desktop y creación de un informe](tutorial-connect-power-bi-desktop.md) para obtener información sobre cómo conectar SQL a petición a Power BI Desktop y crear informes.
 
