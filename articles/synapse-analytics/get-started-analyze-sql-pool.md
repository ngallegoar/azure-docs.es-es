---
title: 'Tutorial: Introducción al análisis de datos con grupos de SQL dedicados'
description: En este tutorial, usará los datos de ejemplo de NYC Taxi para explorar las funcionalidades de análisis del grupo de SQL.
services: synapse-analytics
author: saveenr
ms.author: saveenr
manager: julieMSFT
ms.reviewer: jrasnick
ms.service: synapse-analytics
ms.subservice: sql
ms.topic: tutorial
ms.date: 11/17/2020
ms.openlocfilehash: ae7b15bf2e2efd72184110a105f68815516569ab
ms.sourcegitcommit: 0a9df8ec14ab332d939b49f7b72dea217c8b3e1e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/18/2020
ms.locfileid: "94843917"
---
# <a name="analyze-data-with-dedicated-sql-pools"></a>Análisis de datos con grupos de SQL dedicados

Azure Synapse Analytics le ofrece la funcionalidad de analizar datos con un grupo de SQL dedicado. En este tutorial, usará los datos de NYC Taxi para explorar las capacidades de un grupo de SQL dedicado.

## <a name="load-the-nyc-taxi-data-into-sqlpool1"></a>Carga de los datos NYC Taxi en SQLPOOL1

1. En Synapse Studio, vaya al centro de conectividad **Develop** (Desarrollar) y cree un script de SQL.
1. Seleccione el grupo "SQLPOOL1" (grupo creado en el [paso 1](https://docs.microsoft.com/azure/synapse-analytics/get-started-create-workspace#create-a-sql-pool) de este tutorial) en la sección "conectar a" del script.
1. Escriba el siguiente código:
    ```
    CREATE TABLE [dbo].[Trip]
    (
        [DateID] int NOT NULL,
        [MedallionID] int NOT NULL,
        [HackneyLicenseID] int NOT NULL,
        [PickupTimeID] int NOT NULL,
        [DropoffTimeID] int NOT NULL,
        [PickupGeographyID] int NULL,
        [DropoffGeographyID] int NULL,
        [PickupLatitude] float NULL,
        [PickupLongitude] float NULL,
        [PickupLatLong] varchar(50) COLLATE SQL_Latin1_General_CP1_CI_AS NULL,
        [DropoffLatitude] float NULL,
        [DropoffLongitude] float NULL,
        [DropoffLatLong] varchar(50) COLLATE SQL_Latin1_General_CP1_CI_AS NULL,
        [PassengerCount] int NULL,
        [TripDurationSeconds] int NULL,
        [TripDistanceMiles] float NULL,
        [PaymentType] varchar(50) COLLATE SQL_Latin1_General_CP1_CI_AS NULL,
        [FareAmount] money NULL,
        [SurchargeAmount] money NULL,
        [TaxAmount] money NULL,
        [TipAmount] money NULL,
        [TollsAmount] money NULL,
        [TotalAmount] money NULL
    )
    WITH
    (
        DISTRIBUTION = ROUND_ROBIN,
        CLUSTERED COLUMNSTORE INDEX
    );

    COPY INTO [dbo].[Trip]
    FROM 'https://nytaxiblob.blob.core.windows.net/2013/Trip2013/QID6392_20171107_05910_0.txt.gz'
    WITH
    (
        FILE_TYPE = 'CSV',
        FIELDTERMINATOR = '|',
        FIELDQUOTE = '',
        ROWTERMINATOR='0X0A',
        COMPRESSION = 'GZIP'
    )
    OPTION (LABEL = 'COPY : Load [dbo].[Trip] - Taxi dataset');
    ```
1. Este script finalizará en unos 60 segundos. Carga dos millones filas de datos de NYC Taxi en una tabla denominada **dbo.Trip**.

## <a name="explore-the-nyc-taxi-data-in-the-dedicated-sql-pool"></a>Exploración de los datos de NYC Taxi en el grupo de SQL dedicado

1. En Synapse Studio, vaya al centro **Data** (Datos).
1. Vaya a **SQLPOOL1** > **Tablas**. Verá que se han cargado varias tablas.
1. Haga clic con el botón derecho en la tabla **dbo.Trip** y seleccione **New SQL Script** > **Select TOP 100 Rows** (Nuevo script SQL > Seleccionar 100 primeras filas).
1. Espere mientras se crea un nuevo script de SQL y se ejecuta.
1. Observe que en la parte superior del script de SQL, en **Connect to** (Conectar a) está seleccionado automáticamente el grupo de SQL llamado **SQLPOOL1**.
1. Reemplace el texto del script de SQL por este código y ejecútelo.

    ```sql
    SELECT PassengerCount,
          SUM(TripDistanceMiles) as SumTripDistance,
          AVG(TripDistanceMiles) as AvgTripDistance
    FROM  dbo.Trip
    WHERE TripDistanceMiles > 0 AND PassengerCount > 0
    GROUP BY PassengerCount
    ORDER BY PassengerCount
    ```

    En esta consulta se muestra la relación entre las distancias recorridas totales y la distancia media recorrida, y el número de pasajeros.
1. En la ventana de resultados del script de SQL, cambie el valor de **View** (Vista) a **Chart** (Gráfico) para ver los resultados en un gráfico de líneas.



## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Análisis mediante Spark](get-started-analyze-spark.md)

