---
title: Uso de esquemas definidos por el usuario
description: Sugerencias para el uso de esquemas definidos por el usuario T-SQL para el desarrollo de soluciones en el grupo de SQL de Synapse.
services: synapse-analytics
author: XiaoyuMSFT
manager: craigg
ms.service: synapse-analytics
ms.topic: conceptual
ms.subservice: sql-dw
ms.date: 04/17/2018
ms.author: xiaoyul
ms.reviewer: igorstan
ms.custom: seo-lt-2019
ms.openlocfilehash: fc5e035215e7cabd02861c6ee2498cadd1ef0534
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "85213370"
---
# <a name="user-defined-schemas-in-synapse-sql-pool"></a>Esquemas definidos por el usuario en el grupo de SQL de Synapse
Este artículo se centra en proporcionar varias sugerencias para el uso de esquemas definidos por el usuario T-SQL para desarrollar soluciones en el grupo de SQL de Synapse.

## <a name="schemas-for-application-boundaries"></a>Esquemas para los límites de la aplicación

Los almacenamientos de datos tradicionales suelen utilizar bases de datos independientes para crear los límites de la aplicación en función de la carga de trabajo, el dominio o la seguridad. 

Por ejemplo, un almacenamiento de datos de SQL Server tradicional podría incluir una base de datos provisional, una base de datos de almacenamiento de datos y algunas bases de datos data mart. En esta topología, cada base de datos funciona como una carga de trabajo y límite de seguridad de la arquitectura.

Por el contrario, el grupo de SQL ejecuta la carga de trabajo completa del almacenamiento de datos dentro de una base de datos. No se permiten las combinaciones entre bases de datos. El grupo de SQL espera que todas las tablas que el almacenamiento utiliza se almacenen en una sola base de datos.

> [!NOTE]
> El grupo de SQL no admite ningún tipo de consultas entre bases de datos. En consecuencia, deben revisarse las implementaciones de almacenamiento de datos que utilizan este patrón.
> 
> 

## <a name="recommendations"></a>Recomendaciones
A continuación se ofrecen recomendaciones para consolidar los límites de cargas de trabajo, seguridad, dominio y funcionales con esquemas definidos por el usuario:

- Use una base de datos del grupo de SQL para ejecutar la carga de trabajo completa del almacenamiento de datos.
- Consolide el entorno de almacenamiento de datos existente para utilizar una base de datos del grupo de SQL.
- Utilice **esquemas definidos por el usuario** para proporcionar el límite implementado anteriormente con las bases de datos.

Si los esquemas definidos por el usuario no se han utilizado anteriormente, comenzará desde cero. Utilice el nombre de la base de datos anterior como base para los esquemas definidos por el usuario en la base de datos del grupo de SQL.

Si ya se han utilizado los esquemas, tiene algunas opciones:

- Quitar los nombres de esquemas heredados y empezar desde cero.
- Conservar los nombres de esquemas heredados anteponiendo el nombre de esquema heredado al nombre de tabla.
- Conservar los nombres de esquemas heredados mediante la implementación de vistas sobre la tabla en un esquema adicional para volver a crear la estructura del esquema anterior.

> [!NOTE]
> En la primera inspección, la opción 3 puede resultar la opción más atractiva. No obstante, el problema radica en los detalles. Las vistas son de solo lectura en el grupo de SQL. Cualquier modificación de datos o tablas tendría que realizarse según la tabla de base. La opción 3 también introduce una capa de vistas en el sistema. Desea incorporar algunas más a pesar de que ya utiliza vistas en la arquitectura.
> 
> 

### <a name="examples"></a>Ejemplos:
Implementar los esquemas definidos por el usuario en función de los nombres de base de datos:

```sql
CREATE SCHEMA [stg]; -- stg previously database name for staging database
GO
CREATE SCHEMA [edw]; -- edw previously database name for the data warehouse
GO
CREATE TABLE [stg].[customer] -- create staging tables in the stg schema
(       CustKey BIGINT NOT NULL
,       ...
);
GO
CREATE TABLE [edw].[customer] -- create data warehouse tables in the edw schema
(       CustKey BIGINT NOT NULL
,       ...
);
```

Mantener los nombres de esquemas heredados anteponiendo estos al nombre de tabla. Utilizar esquemas para el límite de carga de trabajo:

```sql
CREATE SCHEMA [stg]; -- stg defines the staging boundary
GO
CREATE SCHEMA [edw]; -- edw defines the data warehouse boundary
GO
CREATE TABLE [stg].[dim_customer] --pre-pend the old schema name to the table and create in the staging boundary
(       CustKey BIGINT NOT NULL
,       ...
);
GO
CREATE TABLE [edw].[dim_customer] --pre-pend the old schema name to the table and create in the data warehouse boundary
(       CustKey BIGINT NOT NULL
,       ...
);
```

Conservar los nombres de esquemas heredados con vistas:

```sql
CREATE SCHEMA [stg]; -- stg defines the staging boundary
GO
CREATE SCHEMA [edw]; -- stg defines the data warehouse boundary
GO
CREATE SCHEMA [dim]; -- edw defines the legacy schema name boundary
GO
CREATE TABLE [stg].[customer] -- create the base staging tables in the staging boundary
(       CustKey    BIGINT NOT NULL
,       ...
)
GO
CREATE TABLE [edw].[customer] -- create the base data warehouse tables in the data warehouse boundary
(       CustKey    BIGINT NOT NULL
,       ...
)
GO
CREATE VIEW [dim].[customer] -- create a view in the legacy schema name boundary for presentation consistency purposes only
AS
SELECT  CustKey
,       ...
FROM    [edw].customer
;
```

> [!NOTE]
> Cualquier cambio en la estrategia de esquema necesita una revisión del modelo de seguridad de la base de datos. En muchos casos puede simplificar el modelo de seguridad mediante la asignación de permisos a nivel de esquema. Si se requieren permisos más específicos, puede utilizar funciones de base de datos.
> 
> 

## <a name="next-steps"></a>Pasos siguientes
Para obtener más sugerencias sobre desarrollo, vea la [información general sobre desarrollo](sql-data-warehouse-overview-develop.md).

