---
title: Uso de vistas T-SQL
description: Sugerencias para usar las vistas T-SQL y desarrollar soluciones en el grupo de SQL de Synapse.
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
ms.openlocfilehash: 26eb3a495fd1c896416265687d92da66dfc3599b
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/02/2020
ms.locfileid: "85212299"
---
# <a name="views-in-synapse-sql-pool"></a>Vistas en el grupo de SQL de Synapse

Las vista se pueden usar de formas diferentes para mejorar la calidad de la solución.

El grupo de SQL admite las vistas estándar y materializadas. Ambas son tablas virtuales creadas con expresiones SELECT y se presentan a las consultas como tablas lógicas.

Las vistas encapsulan la complejidad del cálculo común de datos y agregan una capa de abstracción a los cambios en el cálculo, por lo que no es necesario volver a escribir las consultas.

## <a name="standard-view"></a>Vista estándar

Una vista estándar calcula sus datos cada vez que se utiliza la vista. No hay datos almacenados en disco. Normalmente, los usuarios usan las vistas estándar como una herramienta que ayuda a organizar los objetos lógicos y las consultas de una base de datos.

Para utilizar una vista estándar, una consulta debe hacer referencia directa a ella. Para más información, consulte la documentación de [CREATE VIEW](/sql/t-sql/statements/create-view-transact-sql?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest).

Las vistas en el grupo de SQL se almacenan solo como metadatos. Por lo tanto, las siguientes opciones no están disponibles:

* No hay ninguna opción de enlace de esquema.
* No se puede actualizar las tablas base mediante la vista.
* No se pueden crear vistas en tablas temporales.
* No hay compatibilidad con las sugerencias EXPAND y NOEXPAND
* No hay ninguna vista indexada en el grupo de SQL.

Las vistas estándar se pueden usar para aplicar combinaciones de rendimiento optimizado entre tablas. Por ejemplo, una vista puede incorporar una clave de distribución redundante como parte de los criterios de combinación para minimizar el movimiento de datos.

Otra ventaja de una vista podría ser forzar una consulta específica o una sugerencia de combinación. La utilización de vistas de esta manera garantiza que las combinaciones siempre se realizarán de manera óptima, evitando la necesidad de los usuarios de recordar la construcción correcta de sus combinaciones.

## <a name="materialized-view"></a>Vista materializada

Una vista materializada calcula previamente, almacena y mantiene sus datos en el grupo de SQL como una tabla. No es necesario volver a calcular cada vez que se utiliza una vista materializada.

A medida que los datos se cargan en las tablas base, el grupo de SQL actualiza de forma sincrónica las vistas materializadas.  El optimizador de consultas usa automáticamente las vistas materializadas implementadas para mejorar el rendimiento de las consultas, incluso si no se hace referencia a las vistas en la consulta.  

Las consultas que se benefician más de las vistas materializadas son las consultas complejas (normalmente con combinaciones y agregaciones) en tablas grandes que producen un conjunto de resultados pequeño.  

Para más información sobre la sintaxis de la vista materializada y otros requisitos, consulte [CREATE MATERIALIZED VIEW AS SELECT](/sql/t-sql/statements/create-materialized-view-as-select-transact-sql?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest).  

Para obtener una guía sobre cómo optimizar las consultas, consulte [Optimización del rendimiento con vistas materializadas](performance-tuning-materialized-views.md).

## <a name="example"></a>Ejemplo

Se trata de un patrón de aplicación común para volver a crear tablas con la característica CREATE TABLE AS SELECT (CTAS) seguida de un patrón de cambio de nombre de objetos mientras se cargan los datos.  

En el ejemplo siguiente se agregan registros de fecha nuevos a una dimensión de fecha. Observe cómo una nueva tabla, DimDate_New, se crea por primera vez y luego cambia de nombre para reemplazar la versión original de la tabla.

```sql
CREATE TABLE dbo.DimDate_New
WITH (DISTRIBUTION = ROUND_ROBIN
, CLUSTERED INDEX (DateKey ASC)
)
AS
SELECT *
FROM   dbo.DimDate  AS prod
UNION ALL
SELECT *
FROM   dbo.DimDate_stg AS stg;

RENAME OBJECT DimDate TO DimDate_Old;
RENAME OBJECT DimDate_New TO DimDate;

```

Sin embargo, este enfoque puede provocar que las tablas aparezcan y desaparezcan de la vista del usuario junto con la emisión de mensajes de error del tipo "la tabla no existe".

Las vistas pueden utilizarse para proporcionar a los usuarios una capa de presentación coherente mientras se cambia el nombre de los objetos subyacentes. Al proporcionar acceso a los datos mediante vistas, los usuarios no necesitan visibilidad para las tablas subyacentes.

Esta capa proporciona una experiencia de usuario coherente asegurándose de que los diseñadores del almacenamiento de datos puedan hacer evolucionar el modelo de datos. Hacer evolucionar las tablas subyacentes significa que los diseñadores pueden usar CTAS para maximizar el rendimiento durante el proceso de carga de datos.

## <a name="next-steps"></a>Pasos siguientes

Para obtener más sugerencias sobre desarrollo, vea la [información general sobre desarrollo de grupos de SQL](sql-data-warehouse-overview-develop.md).
