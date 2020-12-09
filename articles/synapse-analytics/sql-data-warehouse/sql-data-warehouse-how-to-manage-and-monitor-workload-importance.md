---
title: Administración y supervisión de la importancia de la carga de trabajo en un grupo de SQL dedicado
description: Aprenda a administrar y supervisar la importancia del nivel de solicitud en un grupo de SQL dedicado para Azure Synapse Analytics.
services: synapse-analytics
author: ronortloff
manager: craigg
ms.service: synapse-analytics
ms.subservice: sql-dw
ms.topic: conceptual
ms.date: 02/04/2020
ms.author: rortloff
ms.reviewer: jrasnick
ms.custom: azure-synapse
ms.openlocfilehash: fb0a5fbf33b48521882646bf8fb5eb3fe5dacca6
ms.sourcegitcommit: 6a350f39e2f04500ecb7235f5d88682eb4910ae8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/01/2020
ms.locfileid: "96459217"
---
# <a name="manage-and-monitor-workload-importance-in-dedicated-sql-pool-for-azure-synapse-analytics"></a>Administración y supervisión de la importancia de la carga de trabajo en un grupo de SQL dedicado para Azure Synapse Analytics

Administre y supervise la importancia del nivel de solicitud de un grupo de SQL dedicado en Azure Synapse mediante vistas de catálogo y DMV.

## <a name="monitor-importance"></a>Supervisión de la importancia

Supervise la importancia con la nueva columna de importancia de la vista de administración dinámica [sys.dm_pdw_exec_requests](/sql/relational-databases/system-dynamic-management-views/sys-dm-pdw-exec-requests-transact-sql?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest).
En la consulta de supervisión siguiente se muestran la hora de envío y la de inicio para las consultas. Revise la hora de envío y la de inicio junto con la importancia para ver cómo ha influido la importancia en la programación.

```sql
SELECT s.login_name, r.status, r.importance, r.submit_time, r.start_time
  FROM sys.dm_pdw_exec_sessions s
  JOIN sys.dm_pdw_exec_requests r ON s.session_id = r.session_id
  WHERE r.resource_class is not null
ORDER BY r.start_time
```

Para examinar más a fondo cómo se programan las consultas, use las vistas de catálogo.

## <a name="manage-importance-with-catalog-views"></a>Administración de la importancia con vistas de catálogo

La vista de catálogo sys.workload_management_workload_classifiers contiene información sobre los clasificadores. Para excluir los clasificadores definidos por el sistema que se asignan a las clases de recursos ejecute el código siguiente:

```sql
SELECT *
  FROM sys.workload_management_workload_classifiers
  WHERE classifier_id > 12
```

La vista de catálogo [sys.workload_management_workload_classifier_details](/sql/relational-databases/system-catalog-views/sys-workload-management-workload-classifier-details-transact-sql?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest) contiene información sobre los parámetros que se usan en la creación del clasificador.  En la consulta siguiente se muestra que se ha creado ExecReportsClassifier en el parámetro ```membername``` para los valores con ExecutiveReports:

```sql
SELECT c.name,cd.classifier_type, classifier_value
  FROM sys.workload_management_workload_classifiers c
  JOIN sys.workload_management_workload_classifier_details cd
    ON cd.classifier_id = c.classifier_id
  WHERE c.name = 'ExecReportsClassifier'
```

![Resultados de la consulta](./media/sql-data-warehouse-how-to-manage-and-monitor-workload-importance/wlm-query-results.png)

Para simplificar la solución de problemas de clasificaciones incorrectas, se recomienda quitar las asignaciones de rol de clase de recursos a medida que se creen los clasificadores de carga de trabajo. El código siguiente devuelve las pertenencias existentes a roles de clase de recurso. Ejecute sp_droprolemember para cada ```membername``` devuelto desde la clase de recurso correspondiente.
A continuación se muestra un ejemplo de la comprobación de existencia antes de quitar un clasificador de carga de trabajo:

```sql
IF EXISTS (SELECT 1 FROM sys.workload_management_workload_classifiers WHERE name = 'ExecReportsClassifier')
  DROP WORKLOAD CLASSIFIER ExecReportsClassifier;
GO
```

## <a name="next-steps"></a>Pasos siguientes

- Para más información sobre la clasificación, vea [Clasificación de la carga de trabajo](sql-data-warehouse-workload-classification.md).
- Para más información sobre la importancia, vea [Importancia de la carga de trabajo](sql-data-warehouse-workload-importance.md).

> [!div class="nextstepaction"]
> [Ir a Configuración de la importancia de la carga de trabajo](sql-data-warehouse-how-to-configure-workload-importance.md)
