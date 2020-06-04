---
title: 'Supervisión de aplicaciones en Azure Kubernetes Service (AKS) con Application Insights: Azure Monitor | Microsoft Docs'
description: Azure Monitor se integra sin problemas con las aplicaciones que se ejecutan en Kubernetes y permite detectar problemas con las aplicaciones al instante.
ms.topic: conceptual
author: MS-jgol
ms.author: jgol
ms.date: 05/13/2020
ms.openlocfilehash: 76f9f922697ef6be6c959ea7f9bafd0872dba6ff
ms.sourcegitcommit: 318d1bafa70510ea6cdcfa1c3d698b843385c0f6
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/21/2020
ms.locfileid: "83773771"
---
# <a name="zero-instrumentation-application-monitoring-for-kubernetes---azure-monitor-application-insights"></a>Supervisión de aplicaciones sin instrumentación para Kubernetes: Application Insights de Azure Monitor

> [!IMPORTANT]
>  Actualmente, puede habilitar la supervisión de las aplicaciones Java que se ejecutan en Kubernetes sin instrumentar el código: use el [agente independiente de Java](https://docs.microsoft.com/azure/azure-monitor/app/java-in-process-agent). Aunque la solución para habilitar sin problemas la supervisión de aplicaciones está en desarrollo para otros lenguajes, puede usar los SDK para supervisar las aplicaciones que se ejecutan en AKS: [ASP.NET Core](https://docs.microsoft.com/azure/azure-monitor/app/asp-net-core), [ASP.NET](https://docs.microsoft.com/azure/azure-monitor/app/asp-net), [Node.js](https://docs.microsoft.com/azure/azure-monitor/app/nodejs), [JavaScript](https://docs.microsoft.com/azure/azure-monitor/app/javascript) y [Python](https://docs.microsoft.com/azure/azure-monitor/app/opencensus-python).

## <a name="application-monitoring-without-instrumenting-the-code"></a>Supervisión de aplicaciones sin instrumentar el código
Actualmente, solo Java permite habilitar la supervisión de aplicaciones sin instrumentar el código. Para supervisar aplicaciones en otros lenguajes, use los SDK. 

## <a name="java"></a>Java
Una vez habilitado, el agente de Java recopilará automáticamente una gran cantidad de solicitudes, dependencias, registros y métricas de las bibliotecas y los marcos de trabajo más usados.

Siga [las instrucciones detalladas](https://docs.microsoft.com/azure/azure-monitor/app/java-in-process-agent) para supervisar las aplicaciones Java que se ejecutan en aplicaciones de Kubernetes, así como otros entornos. 

## <a name="other-languages"></a>Otros idiomas

En el caso de aplicaciones en otros lenguajes, actualmente se recomienda usar los SDK:
* [ASP.NET Core](https://docs.microsoft.com/azure/azure-monitor/app/asp-net-core)
* [ASP.NET](https://docs.microsoft.com/azure/azure-monitor/app/asp-net)
* [Node.js](https://docs.microsoft.com/azure/azure-monitor/app/nodejs) 
* [JavaScript](https://docs.microsoft.com/azure/azure-monitor/app/javascript)
* [Python](https://docs.microsoft.com/azure/azure-monitor/app/opencensus-python)

## <a name="next-steps"></a>Pasos siguientes

* Más información sobre [Azure Monitor](https://docs.microsoft.com/azure/azure-monitor/overview) y [Application Insights](https://docs.microsoft.com/azure/azure-monitor/app/app-insights-overview).
* Consulte una introducción al [seguimiento distribuido](https://docs.microsoft.com/azure/azure-monitor/app/distributed-tracing) y descubra lo que el [Mapa de aplicación](https://docs.microsoft.com/azure/azure-monitor/app/app-map?tabs=net) puede hacer por su empresa.