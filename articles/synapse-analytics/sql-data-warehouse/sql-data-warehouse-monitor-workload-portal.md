---
title: Supervisión de la carga de trabajo - Azure Portal
description: Supervisión de SQL de Synapse mediante Azure Portal
services: synapse-analytics
author: kevinvngo
manager: craigg
ms.service: synapse-analytics
ms.topic: conceptual
ms.subservice: sql-dw
ms.date: 02/04/2020
ms.author: kevin
ms.reviewer: jrasnick
ms.openlocfilehash: 01a22aa5d2ec7ed54be62f0975b0fefbafd84cd8
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "85211568"
---
# <a name="monitor-workload---azure-portal"></a>Supervisión de la carga de trabajo - Azure Portal

En este artículo se describe cómo usar Azure Portal para supervisar la carga de trabajo. Esto incluye la configuración de registros de Azure Monitor para investigar las tendencias de carga de trabajo y ejecución de consultas mediante análisis de registros para [SQL de Synapse](https://azure.microsoft.com/blog/workload-insights-with-sql-data-warehouse-delivered-through-azure-monitor-diagnostic-logs-pass/).

## <a name="prerequisites"></a>Prerrequisitos

- Suscripción de Azure: Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/) antes de empezar.
- Grupo de SQL: se recopilarán registros para un grupo de SQL. Si no tiene un grupo de SQL aprovisionado, consulte las instrucciones de [Creación de un grupo de SQL](load-data-from-azure-blob-storage-using-polybase.md).

## <a name="create-a-log-analytics-workspace"></a>Creación de un área de trabajo de Log Analytics

Navegar a la hoja Examinar de las áreas de trabajo de Log Analytics y crear un área de trabajo

![Áreas de trabajo de Log Analytics](./media/sql-data-warehouse-monitor-workload-portal/log_analytics_workspaces.png)

![Adición de área de trabajo de Analytics](./media/sql-data-warehouse-monitor-workload-portal/add_analytics_workspace.png)

![Adición de área de trabajo de Analytics](./media/sql-data-warehouse-monitor-workload-portal/add_analytics_workspace_2.png)

Para obtener más detalles sobre las áreas de trabajo, visite la siguiente [documentación](../../azure-monitor/learn/quick-create-workspace.md?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.jsond#create-a-workspace).

## <a name="turn-on-resource-logs"></a>Activación de registros de recursos

Configure las opciones de diagnóstico para emitir registros desde el grupo de SQL. Los registros constan de vistas de telemetría equivalentes a las DMV de solución de problemas de rendimiento más utilizadas. Actualmente se admiten las siguientes vistas:

- [sys.dm_pdw_exec_requests](/sql/relational-databases/system-dynamic-management-views/sys-dm-pdw-exec-requests-transact-sql?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest)
- [sys.dm_pdw_request_steps](/sql/relational-databases/system-dynamic-management-views/sys-dm-pdw-request-steps-transact-sql?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest)
- [sys.dm_pdw_dms_workers](/sql/relational-databases/system-dynamic-management-views/sys-dm-pdw-dms-workers-transact-sql?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest)
- [sys.dm_pdw_waits](/sql/relational-databases/system-dynamic-management-views/sys-dm-pdw-waits-transact-sql?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest)
- [sys.dm_pdw_sql_requests](/sql/relational-databases/system-dynamic-management-views/sys-dm-pdw-sql-requests-transact-sql?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest)

![Habilitación de registros de recursos](./media/sql-data-warehouse-monitor-workload-portal/enable_diagnostic_logs.png)

Los registros se pueden emitir a Azure Storage, Stream Analytics o Log Analytics. Para este tutorial, seleccione Log Analytics.

![Especificar registros](./media/sql-data-warehouse-monitor-workload-portal/specify_logs.png)

## <a name="run-queries-against-log-analytics"></a>Ejecución de consultas en Log Analytics

Desplácese hasta el área de trabajo de Log Analytics, donde puede hacer lo siguiente:

- Analizar registros mediante consultas de registros y guardar las consultas para reutilizarlas
- Guardar las consultas para reutilizarlas
- Crear alertas del registro
- Anclar los resultados de la consulta a un panel

Para obtener más información sobre las capacidades de las consultas de registro, visite la siguiente [documentación](../../azure-monitor/log-query/query-language.md?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json).

![Editor del área de trabajo de Log Analytics](./media/sql-data-warehouse-monitor-workload-portal/log_analytics_workspace_editor.png)

![Consultas del área de trabajo de Log Analytics](./media/sql-data-warehouse-monitor-workload-portal/log_analytics_workspace_queries.png)

## <a name="sample-log-queries"></a>Consultas de registros de ejemplo

```Kusto
//List all queries
AzureDiagnostics
| where Category contains "ExecRequests"
| project TimeGenerated, StartTime_t, EndTime_t, Status_s, Command_s, ResourceClass_s, duration=datetime_diff('millisecond',EndTime_t, StartTime_t)
```

```Kusto
//Chart the most active resource classes
AzureDiagnostics
| where Category contains "ExecRequests"
| where Status_s == "Completed"
| summarize totalQueries = dcount(RequestId_s) by ResourceClass_s
| render barchart
```

```Kusto
//Count of all queued queries
AzureDiagnostics
| where Category contains "waits"
| where Type_s == "UserConcurrencyResourceType"
| summarize totalQueuedQueries = dcount(RequestId_s)
```

## <a name="next-steps"></a>Pasos siguientes

Ahora que ha instalado y configurado los registros de Azure Monitor, [personalice los paneles de Azure](../../azure-portal/azure-portal-dashboards.md?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json) para compartirlos con su equipo.
