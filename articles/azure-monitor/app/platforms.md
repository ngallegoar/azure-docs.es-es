---
title: 'Application Insights: lenguajes, plataformas e integraciones| Microsoft Docs'
description: Lenguajes, plataformas e integraciones disponibles para Application Insights
ms.topic: conceptual
ms.date: 07/18/2019
ms.reviewer: olegan
ms.openlocfilehash: 153d4ad3d95c182dcc4f2aa3bad857d7e1984cc2
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/02/2020
ms.locfileid: "82891107"
---
# <a name="supported-languages"></a>Idiomas compatibles

* [C#|VB (.NET)](../../azure-monitor/app/asp-net.md)
* [Java](../../azure-monitor/app/java-get-started.md)
* [JavaScript](../../azure-monitor/app/javascript.md)
* [Node.JS](../../azure-monitor/app/nodejs.md)
* [Python](../../azure-monitor/app/opencensus-python.md)

## <a name="supported-platforms-and-frameworks"></a>Plataformas y marcos compatibles

### <a name="instrumentation-for-already-deployed-applications-codeless-agent-based"></a>Instrumentación de aplicaciones ya implementadas (sin código y basadas en agentes)
* [Conjuntos de escalado de máquinas virtuales de Azure y Azure VM](../../azure-monitor/app/azure-vm-vmss-apps.md)
* [Azure App Service](../../azure-monitor/app/azure-web-apps.md)
* [ASP.NET: para aplicaciones que ya están activas](../../azure-monitor/app/monitor-performance-live-website-now.md)
* [Azure Cloud Services](../../azure-monitor/app/cloudservices.md), incluidos los roles web y de trabajo
* [Funciones de Azure](https://docs.microsoft.com/azure/azure-functions/functions-monitoring)
### <a name="instrumentation-through-code-sdks"></a>Instrumentación a través del código (SDK)
* [ASP.NET](../../azure-monitor/app/asp-net.md)
* [ASP.NET Core](../../azure-monitor/app/asp-net-core.md)
* [Android](../../azure-monitor/learn/mobile-center-quickstart.md) (App Center)
* [iOS](../../azure-monitor/learn/mobile-center-quickstart.md) (App Center)
* [Java EE](../../azure-monitor/app/java-get-started.md)
* [Node.JS](https://www.npmjs.com/package/applicationinsights)
* [Python](../../azure-monitor/app/opencensus-python.md)
* [Aplicación Windows universal](../../azure-monitor/learn/mobile-center-quickstart.md) (App Center)
* [Aplicaciones, servicios y roles de trabajo del escritorio de Windows](../../azure-monitor/app/windows-desktop.md)

## <a name="logging-frameworks"></a>Marcos de registro
* [ILogger](https://docs.microsoft.com/azure/azure-monitor/app/ilogger)
* [Log4Net, NLog o System.Diagnostics.Trace](../../azure-monitor/app/asp-net-trace-logs.md)
* [Java, Log4J o Logback](../../azure-monitor/app/java-trace-logs.md)
* [Complemento LogStash](https://github.com/Azure/azure-diagnostics-tools/tree/master/Logstash/logstash-output-applicationinsights)
* [Azure Monitor](https://blogs.technet.microsoft.com/msoms/2016/09/26/application-insights-connector-in-oms/)

## <a name="export-and-data-analysis"></a>Exportación y análisis de datos
* [Power BI](https://powerbi.microsoft.com/blog/explore-your-application-insights-data-with-power-bi/)
* [Stream Analytics](../../azure-monitor/app/export-power-bi.md)

## <a name="unsupported-sdks"></a>SDK no compatibles
Somos conscientes de que existen otros SDK con soporte de la comunidad. Sin embargo, Azure Monitor solo proporciona compatibilidad cuando se usan los SDK admitidos que se enumeran en esta página. Estamos evaluando constantemente oportunidades para ampliar nuestra compatibilidad con otros lenguajes, por lo que debe seguir nuestra página de [anuncios de GitHub](https://github.com/microsoft/ApplicationInsights-Announcements/issues) para recibir las noticias más recientes sobre los SDK. 
