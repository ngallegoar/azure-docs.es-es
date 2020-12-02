---
title: archivo de inclusión
description: archivo de inclusión
services: azure-monitor
author: rboucher
tags: azure-service-management
ms.topic: include
ms.date: 07/22/2019
ms.author: bwren
ms.custom: include file
ms.openlocfilehash: ff5d04a2923f16c763e1529ecb365f60d6275ca2
ms.sourcegitcommit: a43a59e44c14d349d597c3d2fd2bc779989c71d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/25/2020
ms.locfileid: "96026617"
---
### <a name="general-query-limits"></a>Límites generales de las consultas

| Límite | Descripción |
|:---|:---|
| Lenguaje de consulta | Azure Monitor usa el mismo [lenguaje de consulta Kusto](/azure/kusto/query/) que Azure Data Explorer. Consulte las [diferencias del lenguaje de consulta de registros de Azure Monitor](/azure/data-explorer/kusto/query/) para los elementos del lenguaje KQL que no se admiten en Azure Monitor. |
| Regiones de Azure | Las consultas de registro pueden experimentar una sobrecarga excesiva cuando los datos abarcan áreas de trabajo Log Analytics de varias regiones de Azure. Para más información, consulte [Límites de consulta](../articles/azure-monitor/log-query/scope.md#query-scope-limits). |
| Consultas entre recursos | El número máximo de recursos de Application Insights y de áreas de trabajo de Log Analytics en una sola consulta se limita a 100.<br>No se admite la consulta entre recursos en el Diseñador de vistas.<br>La consulta entre recursos en las alertas de registro se admite en la nueva API scheduledQueryRules.<br>Consulte [Límites de la consulta entre recursos](../articles/azure-monitor/log-query/cross-workspace-query.md#cross-resource-query-limits) para obtener más información. |

### <a name="user-query-throttling"></a>Limitación de las consultas de usuario
Azure Monitor tiene varios límites para protegerse frente a los usuarios que envían un número excesivo de consultas. Este comportamiento puede sobrecargar potencialmente los recursos de back-end del sistema y poner en peligro la capacidad de respuesta del servicio. Los límites siguientes están diseñados para proteger a los clientes frente a interrupciones y garantizar un nivel de servicio coherente. Los límites y la limitación de usuarios están diseñados para afectar solo a un escenario de uso extremo y no deben ser de importancia para un uso normal.


| Measure | Límite por usuario | Descripción |
|:---|:---|:---|
| Consultas simultáneas | 5 | Si ya hay 5 consultas en ejecución para el usuario, las consultas nuevas se colocan en una cola de simultaneidad por usuario. Cuando finaliza una de las consultas en ejecución, la siguiente consulta se extrae de la cola y se inicia. No se incluyen las consultas de reglas de alertas.
| Tiempo en la cola de simultaneidad | 3 minutos | Si una consulta se encuentra en la cola durante más de 3 minutos sin iniciarse, se terminará con una respuesta de error HTTP con el código 429. |
| Total de consultas en la cola de simultaneidad | 200 | Una vez que el número de consultas en la cola alcanza las 200, las consultas adicionales se rechazarán con un código de error HTTP 429. Este número es adicional a las 5 consultas que se pueden ejecutar simultáneamente. |
| Velocidad de consulta | 200 consultas cada 30 segundos | Esta es la velocidad global con la que un solo usuario puede enviar consultas a todas las áreas de trabajo.  Este límite se aplica a consultas mediante programación o a consultas iniciadas por elementos de visualización, como los paneles de Azure y la página de resumen del área de trabajo de Log Analytics. |

- Optimice las consultas como se describe en [Optimización de las consultas de registro en Azure Monitor](../articles/azure-monitor/log-query/query-optimization.md).
- Los paneles y los libros pueden contener varias consultas en una sola vista que generan una ráfaga de consultas cada vez que se cargan o actualizan. Considere la posibilidad de dividirlos en varias vistas que se cargan a petición. 
- En Power BI, considere la posibilidad de extraer solo los resultados agregados en lugar de los registros sin procesar.