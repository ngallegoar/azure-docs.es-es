---
title: Creación y uso de tablas externas en SQL a petición (versión preliminar)
description: En esta sección, aprenderá a crear y usar tablas externas en SQL a petición (versión preliminar). Las tablas externas son útiles si se desea controlar el acceso a los datos externos en SQL a petición y si se desea usar herramientas, como Power BI, junto con SQL a petición.
services: synapse-analytics
author: vvasic-msft
ms.service: synapse-analytics
ms.topic: overview
ms.subservice: ''
ms.date: 04/15/2020
ms.author: vvasic
ms.reviewer: jrasnick, carlrab
ms.openlocfilehash: a708e5e713e62ab5b7ca70b61235a84830b051cd
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/16/2020
ms.locfileid: "81420799"
---
# <a name="create-and-use-external-tables-in-sql-on-demand-preview-using-azure-synapse-analytics"></a>Creación y uso de tablas externas en SQL a petición (versión preliminar) mediante Azure Synapse Analytics

En esta sección, aprenderá a crear y usar tablas externas en SQL a petición (versión preliminar). Las tablas externas son útiles si se desea controlar el acceso a los datos externos en SQL a petición y si se desea usar herramientas, como Power BI, junto con SQL a petición.

## <a name="prerequisites"></a>Prerrequisitos

El primer paso es leer los artículos siguientes y asegurarse de que cumple los requisitos previos para crear y usar tablas externas de SQL a petición:

- [Primera configuración](query-data-storage.md#first-time-setup)
- [Requisitos previos](query-data-storage.md#prerequisites)

## <a name="create-an-external-table"></a>Crear una tabla externa

Puede crear tablas externas de la misma manera que crea tablas externas de SQL Server normales. La siguiente consulta crea una tabla externa que lee el archivo *population.csv*.

> [!NOTE]
> Cambie la primera línea de la consulta, es decir, [mydbname], para usar la base de datos que ha creado. Si no ha creado ninguna base de datos, consulte [Primera configuración](query-data-storage.md#first-time-setup).

```sql
USE [mydbname];
GO

CREATE EXTERNAL DATA SOURCE [CsvDataSource] WITH (
    LOCATION = 'https://sqlondemandstorage.blob.core.windows.net/csv'
);
GO

CREATE EXTERNAL FILE FORMAT CSVFileFormat
WITH (  
    FORMAT_TYPE = DELIMITEDTEXT,
    FORMAT_OPTIONS (
        FIELD_TERMINATOR = ',',
        STRING_DELIMITER = '"',
        FIRST_ROW = 2
    )
);
GO

CREATE EXTERNAL TABLE populationExternalTable
(
    [country_code] VARCHAR (5) COLLATE Latin1_General_BIN2,
    [country_name] VARCHAR (100) COLLATE Latin1_General_BIN2,
    [year] smallint,
    [population] bigint
);
WITH (
    LOCATION = 'population/population.csv',
    DATA_SOURCE = CsvDataSource,
    FILE_FORMAT = CSVFileFormat
);
GO
```

## <a name="use-a-external-table"></a>Uso de una tabla externa

Puede usar tablas externas en las consultas de la misma manera que las utiliza en las consultas de SQL Server.

En la consulta siguiente se muestra el uso de la tabla externa de *población* que creamos en la sección [Creación de una tabla externa](#create-an-external-table). Devuelve los nombres de país con su población en 2019, en orden descendente.

> [!NOTE]
> Cambie la primera línea de la consulta, es decir, [mydbname], para usar la base de datos que ha creado. Si no ha creado ninguna base de datos, consulte [Primera configuración](query-data-storage.md#first-time-setup).

```sql
USE [mydbname];
GO

SELECT
    country_name, population
FROM populationExternalTable
WHERE
    [year] = 2019
ORDER BY
    [population] DESC;
```

## <a name="next-steps"></a>Pasos siguientes

Para obtener información sobre cómo almacenar los resultados de una consulta en el almacenamiento, consulte [Almacenamiento de los resultados de una consulta en el almacenamiento](../sql/create-external-table-as-select.md).
