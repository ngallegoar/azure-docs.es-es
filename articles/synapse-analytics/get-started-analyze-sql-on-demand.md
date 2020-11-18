---
title: 'Tutorial: Introducción al análisis de datos con un grupo de SQL sin servidor'
description: En este tutorial, aprenderá a analizar datos con un grupo de SQL sin servidor mediante los datos ubicados en bases de datos de Spark.
services: synapse-analytics
author: saveenr
ms.author: saveenr
manager: julieMSFT
ms.reviewer: jrasnick
ms.service: synapse-analytics
ms.subservice: sql
ms.topic: tutorial
ms.date: 07/20/2020
ms.openlocfilehash: 4ca9ababbeb7843f1a014a4bd51a5e24a74acbae
ms.sourcegitcommit: 96918333d87f4029d4d6af7ac44635c833abb3da
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/04/2020
ms.locfileid: "93322945"
---
# <a name="analyze-data-with-serverless-sql-pool-in-azure-synapse-analytics"></a>Análisis de datos con un grupo de SQL sin servidor en Azure Synapse Analytics

En este tutorial, aprenderá a analizar datos con un grupo de SQL sin servidor mediante los datos ubicados en bases de datos de Spark. 

## <a name="analyze-nyc-taxi-data-in-blob-storage-using-serverless-sql-pool"></a>Análisis de los datos de taxis de Nueva York en Blob Storage mediante un grupo de SQL sin servidor

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

## <a name="analyze-nyc-taxi-data-in-spark-databases-using-serverless-sql-pool"></a>Análisis de los datos de taxis de Nueva York en las bases de datos de Spark mediante un grupo de SQL sin servidor

Las tablas de las bases de datos de Spark se ven automáticamente y se pueden consultar mediante un grupo de SQL sin servidor.

1. En Synapse Studio, vaya al centro **Develop** (Desarrollo) y cree un script de SQL.
1. Establezca **Conectarse a** en **grupo de SQL sin servidor**.
1. Pegue el siguiente texto en el script y ejecute el script.

    ```sql
    SELECT *
    FROM nyctaxi.dbo.passengercountstats
    ```

    > [!NOTE]
    > La primera vez que se ejecuta una consulta que usa un grupo de SQL sin servidor dicho grupo tarda alrededor de 10 segundos en recopilar los recursos de SQL necesarios para ejecutar las consultas. Las consultas posteriores serán mucho más rápidas.
  


## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Análisis de datos en Storage](get-started-analyze-storage.md)
