---
title: Recursos para desarrollar un grupo de SQL dedicado (anteriormente SQL DW) en Azure Synapse Analytics
description: Conceptos de desarrollo, decisiones de diseño, recomendaciones y técnicas de codificación para un grupo de SQL dedicado (anteriormente SQL DW) en Azure Synapse Analytics.
services: synapse-analytics
author: XiaoyuMSFT
manager: craigg
ms.service: synapse-analytics
ms.topic: conceptual
ms.subservice: sql-dw
ms.date: 08/29/2018
ms.author: xiaoyul
ms.reviewer: igorstan
ms.openlocfilehash: 6b34c70b453c26fe27a51e1aa802564864640cb9
ms.sourcegitcommit: 6a350f39e2f04500ecb7235f5d88682eb4910ae8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/01/2020
ms.locfileid: "96453692"
---
# <a name="design-decisions-and-coding-techniques-for-a-dedicated-sql-pool-formerly-sql-dw-in-azure-synapse-analytics"></a>Decisiones de diseño y técnicas de codificación para un grupo de SQL dedicado (anteriormente SQL DW) en Azure Synapse Analytics 

 En este artículo, encontrará recursos adicionales que le ayudarán a comprender mejor las decisiones de diseño, las recomendaciones y las técnicas de codificación clave para un grupo de SQL dedicado (anteriormente SQL DW) en Azure Synapse.

## <a name="key-design-decisions"></a>Decisiones de diseño clave

En los siguientes artículos se resaltan los conceptos y las decisiones de diseño para desarrollar un almacenamiento de datos distribuido mediante la funcionalidad del grupo de SQL dedicado (anteriormente SQL DW) en Azure Synapse:

* [connections](sql-data-warehouse-connect-overview.md)
* [concurrency](resource-classes-for-workload-management.md)
* [transacciones](sql-data-warehouse-develop-transactions.md)
* [esquemas definidos por el usuario](sql-data-warehouse-develop-user-defined-schemas.md)
* [Distribución de tablas](sql-data-warehouse-tables-distribute.md)
* [índices de tablas](sql-data-warehouse-tables-index.md)
* [particiones de tablas](sql-data-warehouse-tables-partition.md)
* [CTAS](sql-data-warehouse-develop-ctas.md)
* [estadísticas](sql-data-warehouse-tables-statistics.md)

## <a name="development-recommendations-and-coding-techniques"></a>Recomendaciones de desarrollo y técnicas de codificación

En los artículos siguientes se abordan técnicas de codificación, sugerencias y recomendaciones específicas para el desarrollo de un grupo de SQL dedicado (anteriormente SQL DW):

* [procedimientos almacenados](sql-data-warehouse-develop-stored-procedures.md)
* [etiquetas](sql-data-warehouse-develop-label.md)
* [vistas](performance-tuning-materialized-views.md)
* [tablas temporales](sql-data-warehouse-tables-temporary.md)
* [SQL dinámico](sql-data-warehouse-develop-dynamic-sql.md)
* [bucle](sql-data-warehouse-develop-loops.md)
* [opciones de agrupar por](sql-data-warehouse-develop-group-by-options.md)
* [asignación de variables](sql-data-warehouse-develop-variable-assignment.md)

## <a name="next-steps"></a>Pasos siguientes

Para obtener más información de referencia, consulte las [instrucciones de T-SQL](sql-data-warehouse-reference-tsql-statements.md).
