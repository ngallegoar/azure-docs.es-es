---
title: 'Inicio rápido: carga masiva de datos mediante una única instrucción T-SQL'
description: carga masiva de datos mediante la instrucción COPY
services: synapse-analytics
author: kevinvngo
manager: craigg
ms.service: synapse-analytics
ms.topic: quickstart
ms.subservice: ''
ms.date: 04/08/2020
ms.author: kevin
ms.reviewer: jrasnick
ms.custom: azure-synapse
ms.openlocfilehash: d39b3085a802ca0ff745ab1f63f4a8fba966ea48
ms.sourcegitcommit: fb23286d4769442631079c7ed5da1ed14afdd5fc
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/10/2020
ms.locfileid: "81115000"
---
# <a name="quickstart-bulk-load-data-using-the-copy-statement"></a>Inicio rápido: carga masiva de datos mediante la instrucción COPY

En este inicio rápido, realizará una carga masiva de datos en un grupo de SQL mediante la simple y flexible [instrucción COPY](https://docs.microsoft.com/sql/t-sql/statements/copy-into-transact-sql?view=azure-sqldw-latest) para la ingesta de datos de alto rendimiento. La instrucción COPY es la utilidad de carga recomendada, ya que le permite cargar datos de forma fluida y flexible proporcionando la funcionalidad para:

- Permitir la carga de usuarios con menos privilegios sin necesidad de estrictos permisos de CONTROL en el almacenamiento de datos
- Aprovechar solo una instrucción T-SQL única sin tener que crear objetos de base de datos adicionales
- Aprovechar un modelo de permisos más preciso sin exponer las claves de la cuenta de almacenamiento mediante firmas de acceso compartido (SAS)
- Especificar una cuenta de almacenamiento diferente para la ubicación de ERRORFILE (REJECTED_ROW_LOCATION)
- Personalizar los valores predeterminados de cada columna de destino y especificar los campos de datos de origen que se van a cargar en columnas de destino concretas
- Especificar un terminador de fila personalizado para los archivos .csv
- Delimitadores de filas, campos y cadenas de escape para archivos .csv
- Aprovechar los formatos de fecha de SQL Server para los archivos .csv
- Especificar caracteres comodín y varios archivos en la ruta de acceso de la ubicación de almacenamiento

## <a name="prerequisites"></a>Prerrequisitos

En este inicio rápido se da por supuesto que ya tiene un grupo de SQL. Si no se ha creado un grupo de SQL, use el inicio rápido [Creación y conexión: portal](create-data-warehouse-portal.md).

## <a name="create-the-target-table"></a>Creación de la tabla de destino

En este ejemplo, vamos a cargar datos del conjunto de datos de taxis de Nueva York. Cargaremos una tabla denominada Trip (Viaje) que representa los viajes de taxi realizados en un solo año. Ejecute el siguiente código para crear la tabla:

```sql
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
```

## <a name="run-the-copy-statement"></a>Ejecución de la instrucción COPY

Ejecute la siguiente instrucción COPY que cargará los datos de la cuenta de Azure Blob Storage en la tabla Trip (Viaje).

```sql
COPY INTO [dbo].[Trip] FROM 'https://nytaxiblob.blob.core.windows.net/2013/Trip2013/'
WITH (
   FIELDTERMINATOR='|',
   ROWTERMINATOR='0x0A'
) OPTION (LABEL = 'COPY: dbo.trip');
```

## <a name="monitor-the-load"></a>Supervisión de la carga

Para comprobar si la carga progresa, ejecute periódicamente la siguiente consulta:

```sql
SELECT  r.[request_id]                           
,       r.[status]                               
,       r.resource_class                         
,       r.command
,       sum(bytes_processed) AS bytes_processed
,       sum(rows_processed) AS rows_processed
FROM    sys.dm_pdw_exec_requests r
              JOIN sys.dm_pdw_dms_workers w
                     ON r.[request_id] = w.request_id
WHERE [label] = 'COPY: dbo.trip' and session_id <> session_id() and type = 'WRITER'
GROUP BY r.[request_id]                           
,       r.[status]                               
,       r.resource_class                         
,       r.command;

```

## <a name="next-steps"></a>Pasos siguientes

- Para ver los procedimientos recomendados para la carga de datos, consulte [Procedimientos recomendados para la carga de datos](https://docs.microsoft.com/azure/synapse-analytics/sql-data-warehouse/guidance-for-loading-data).
- Para obtener información sobre cómo administrar los recursos para las cargas de datos, consulte el artículo acerca del [aislamiento de cargas de trabajo](https://docs.microsoft.com/azure/synapse-analytics/sql-data-warehouse/quickstart-configure-workload-isolation-tsql). 
