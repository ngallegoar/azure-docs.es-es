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
ms.openlocfilehash: f2092753ab054a639e208aab4a6b7317c1bd5edb
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "74795830"
---
| Límite | Descripción |
|:---|:---|
| Lenguaje de consulta | Azure Monitor usa el mismo [lenguaje de consulta Kusto](/azure/kusto/query/) que Azure Data Explorer. Consulte las [diferencias del lenguaje de consulta de registros de Azure Monitor](../articles/azure-monitor/log-query/data-explorer-difference.md) para los elementos del lenguaje KQL que no se admiten en Azure Monitor. |
| Regiones de Azure | Las consultas de registro pueden experimentar una sobrecarga excesiva cuando los datos abarcan áreas de trabajo Log Analytics de varias regiones de Azure. Para más información, consulte [Límites de consulta](../articles/azure-monitor/log-query/scope.md#query-limits). |
| Consultas entre recursos | El número máximo de recursos de Application Insights y de áreas de trabajo de Log Analytics en una sola consulta se limita a 100.<br>No se admite la consulta entre recursos en el Diseñador de vistas.<br>La consulta entre recursos en las alertas de registro se admite en la nueva API scheduledQueryRules.<br>Consulte [Límites de la consulta entre recursos](../articles/azure-monitor/log-query/cross-workspace-query.md#cross-resource-query-limits) para obtener más información. |
| Limitación de consultas | Un usuario está limitado a 200 consultas cada 30 segundos en cualquier número de áreas de trabajo. Este límite se aplica a consultas mediante programación o a consultas iniciadas por elementos de visualización, como los paneles de Azure y la página de resumen del área de trabajo de Log Analytics. |
