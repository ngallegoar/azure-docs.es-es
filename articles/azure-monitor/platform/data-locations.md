---
title: Ubicaciones de datos de supervisión en Azure Monitor | Microsoft Docs
description: Se describen las diferentes ubicaciones de Azure donde se almacenan datos de supervisión, incluida la plataforma de datos de Azure Monitor.
ms.subservice: ''
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 05/21/2019
ms.openlocfilehash: 7d4459867081d920fefb9471b1a682d21040da9d
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/02/2020
ms.locfileid: "77666622"
---
# <a name="monitoring-data-locations-in-azure-monitor"></a>Ubicaciones de datos de supervisión en Azure Monitor

Azure Monitor se basa en una [plataforma de datos](data-platform.md) de [Registros](data-platform-logs.md) y [Métricas](data-platform-metrics.md), tal como se describe en la [plataforma de datos de Azure Monitor](data-platform.md). Sin embargo, los datos de supervisión de recursos de Azure se pueden escribirse en otras ubicaciones, ya sea antes de copiarlos en Azure Monitor o para admitir escenarios adicionales. 

## <a name="monitoring-data-locations"></a>Ubicaciones de datos de supervisión

En la siguiente tabla se identifican las diferentes ubicaciones de Azure a las que se envían datos de supervisión y los distintos métodos para tener acceso a estos.

| Location | Descripción | Métodos de acceso |
|:---|:---|:---|:--|
| Métricas de Azure Monitor | Base de datos de serie temporal que está optimizada para el análisis de los datos con marca de tiempo. | [Explorador de métricas](metrics-getting-started.md)<br>[API de métricas de Azure Monitor ](/rest/api/monitor/metrics) |
| Registros de Azure Monitor    | Área de trabajo de Log Analytics basada en Azure Data Explorer que proporciona un motor de análisis eficaz y un lenguaje de consulta completo. | [Log Analytics](../log-query/portals.md)<br>[API de Log Analytics](https://dev.loganalytics.io/)<br>[API de Application Insights](https://dev.applicationinsights.io/reference/get-query) |
| Registro de actividades | Los datos del registro de actividades son muy útiles cuando se envían a registros de Azure Monitor para analizarlos con otros datos, pero también se recopilan por separado de manera que se pueden ver directamente en Azure Portal. | [Azure Portal](activity-log-view.md#azure-portal)<br>[API de eventos de Azure Monitor](/rest/api/monitor/eventcategories) |
| Azure Storage | Algunos orígenes de datos escribirán directamente en Azure Storage y requieren una configuración para mover datos a registros. También puede enviar datos a Azure Storage para archivado e integración con sistemas externos.  | [Storage Analytics](/rest/api/storageservices/storage-analytics)<br>[Explorador de servidores](/visualstudio/azure/vs-azure-tools-storage-resources-server-explorer-browse-manage)<br>[Explorador de Storage](/azure/vs-azure-tools-storage-manage-with-storage-explorer?tabs=windows) |
| Event Hubs | Envíe datos a Azure Event Hubs para transmitirlos a otras ubicaciones. | [Captura en almacenamiento](../../event-hubs/event-hubs-capture-overview.md)  |
| Azure Monitor para máquinas virtuales | Azure Monitor para VM almacena datos de estado de carga de trabajo en una ubicación personalizada que su experiencia de supervisión usa en Azure Portal. | [Azure Portal](../insights/vminsights-overview.md)<br>[API REST de monitor de carga de trabajo](https://docs.microsoft.com/rest/api/monitor/microsoft.workloadmonitor/components)<br>[API REST de Azure Resource Health](https://docs.microsoft.com/rest/api/resourcehealth/)  |
| Alertas | Alertas creadas por Azure Monitor. | [Azure Portal](alerts-managing-alert-instances.md)<br>[API REST de administración de alertas](https://docs.microsoft.com/rest/api/monitor/alertsmanagement/alerts) |



## <a name="next-steps"></a>Pasos siguientes

- Vea las diferentes fuentes de [datos de supervisión que recopila Azure Monitor](data-sources.md).
- Más información sobre la [tipos de datos de supervisión recopilados por Azure Monitor](data-platform.md) y cómo ver y analizar estos datos.
