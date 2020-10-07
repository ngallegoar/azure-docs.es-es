---
title: 'Tutorial: Introducción a los análisis de datos con SQL sin servidor'
description: En este tutorial, aprenderá a analizar datos con SQL a petición mediante los datos ubicados en bases de datos de Spark.
services: synapse-analytics
author: saveenr
ms.author: saveenr
manager: julieMSFT
ms.reviewer: jrasnick
ms.service: synapse-analytics
ms.topic: tutorial
ms.date: 07/20/2020
ms.openlocfilehash: 8d26a03a8b61850dc17bc4efff5f8ca12dfca191
ms.sourcegitcommit: 32c521a2ef396d121e71ba682e098092ac673b30
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/25/2020
ms.locfileid: "91300231"
---
# <a name="analyze-data-with-sql-on-demand"></a>Análisis de datos con SQL a petición

En este tutorial, aprenderá a analizar datos con SQL sin servidor mediante un grupo de SQL a petición con los datos ubicados en bases de datos de Spark. 

## <a name="analyze-nyc-taxi-data-in-blob-storage-using-sql-on-demand-pool"></a>Análisis de los datos de NYC Taxi en Blob Storage con un grupo de SQL a petición

1. En el centro de conectividad **Data** (Datos) de **Linked** (Vinculados), haga clic con el botón derecho en **Azure Blob Storage > Sample Datasets > nyc_tlc_yellow** (Azure Blob Storage > Conjuntos de datos de ejemplo > nyc_tlc_yellow) y seleccione **SELECT New notebook** (SELECCIONAR 100 PRIMERAS filas).
1. Así se creará un script de SQL con el código siguiente:

    ```
    SELECT
        TOP 100 *
    FROM
        OPENROWSET(
            BULK     'https://azureopendatastorage.blob.core.windows.net/nyctlc/yellow/puYear=*/puMonth=*/*.parquet',
            FORMAT = 'parquet'
        ) AS [result];
    ```
1. Haga clic en **Ejecutar**

## <a name="analyze-nyc-taxi-data-in-spark-databases-using-sql-on-demand"></a>Análisis de los datos de taxis de Nueva York en las bases de datos de Spark mediante SQL On-Demand

SQL On-Demand ve y puede consultar automáticamente las tablas de las bases de datos de Spark.

1. En Synapse Studio, vaya al centro **Develop** (Desarrollo) y cree un script de SQL.
1. En **Conectarse a** seleccione **SQL On-Demand**.
1. Pegue el siguiente texto en el script y ejecute el script.

    ```sql
    SELECT *
    FROM nyctaxi.dbo.passengercountstats
    ```

    > [!NOTE]
    > La primera vez que se ejecuta una consulta que usa SQL On-Demand, se tardarán unos 10 segundos en recopilar los recursos de SQL necesarios para ejecutar las consultas. Las consultas posteriores serán mucho más rápidas.
  


## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Análisis mediante Spark](get-started-analyze-spark.md)
