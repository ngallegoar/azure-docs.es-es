---
title: Contadores de eventos en Application Insights | Microsoft Docs
description: Supervise los contadores de eventos de .NET/.NET Core personalizados y del sistema en Application Insights.
ms.topic: conceptual
ms.date: 09/20/2019
ms.custom: devx-track-csharp
ms.openlocfilehash: a9af36f3c81ee52b41a8eed875c1a286b95bf838
ms.sourcegitcommit: 23aa0cf152b8f04a294c3fca56f7ae3ba562d272
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/07/2020
ms.locfileid: "91803650"
---
# <a name="eventcounters-introduction"></a>Introducción a los contadores de eventos

[`EventCounter`](/dotnet/core/diagnostics/event-counters) es el mecanismo de .NET/.NET Core para publicar y consumir contadores o estadísticas. Los contadores de eventos se admiten en todas las plataformas de sistema operativo (Windows, Linux y macOS). Puede considerarse un equivalente multiplataforma de los [contadores de rendimiento](/dotnet/api/system.diagnostics.performancecounter) que solo se admiten en los sistemas Windows.

Aunque los usuarios pueden publicar cualquier `EventCounters` personalizado que necesiten, el entorno de ejecución de .NET Core 3.0 o superior publica un conjunto de estos contadores de forma predeterminada. En este documento se recorren los pasos necesarios para recopilar y ver `EventCounters` (definidos por el sistema o por el usuario) en Azure Application Insights.

## <a name="using-application-insights-to-collect-eventcounters"></a>Uso de Application Insights para recopilar contadores de eventos

Application Insights admite la recopilación de `EventCounters` con su `EventCounterCollectionModule`, que forma parte del paquete NuGet [Microsoft.ApplicationInsights.EventCounterCollector](https://www.nuget.org/packages/Microsoft.ApplicationInsights.EventCounterCollector) lanzado recientemente. `EventCounterCollectionModule` se habilita automáticamente al usar [AspNetCore](asp-net-core.md) o [WorkerService](worker-service.md). `EventCounterCollectionModule` recopila contadores con una frecuencia de recopilación no configurable de 60 segundos. No se requieren permisos especiales para recopilar contadores de eventos.

## <a name="default-counters-collected"></a>Contadores predeterminados recopilados

A partir de la versión 2.15.0 del [SDK de AspNetCore](asp-net-core.md) o [SDK de WorkerService](worker-service.md), no se recopila ningún contador de forma predeterminada. El propio módulo está habilitado, por lo que los usuarios simplemente pueden agregar los contadores deseados para recopilarlos.

Para obtener una lista de los contadores conocidos que se hayan publicado en el Runtime de .NET, consulte el documento de los [Contadores disponibles](/dotnet/core/diagnostics/event-counters#available-counters).

## <a name="customizing-counters-to-be-collected"></a>Personalización de los contadores que se van a recopilar

El ejemplo siguiente muestra cómo agregar/quitar contadores. Esta personalización se llevaría a cabo en el método `ConfigureServices` de la aplicación después de que se haya habilitado la recopilación de telemetría de Application Insights mediante `AddApplicationInsightsTelemetry()` o `AddApplicationInsightsWorkerService()`. A continuación se muestra un ejemplo de código de una aplicación ASP.NET Core. Para otros tipos de aplicaciones, consulte [este](worker-service.md#configuring-or-removing-default-telemetrymodules) documento.

```csharp
    using Microsoft.ApplicationInsights.Extensibility.EventCounterCollector;
    using Microsoft.Extensions.DependencyInjection;

    public void ConfigureServices(IServiceCollection services)
    {
        //... other code...

        // The following code shows how to configure the module to collect
        // additional counters.
        services.ConfigureTelemetryModule<EventCounterCollectionModule>(
            (module, o) =>
            {
                // This removes all default counters, if any.
                module.Counters.Clear();

                // This adds a user defined counter "MyCounter" from EventSource named "MyEventSource"
                module.Counters.Add(new EventCounterCollectionRequest("MyEventSource", "MyCounter"));

                // This adds the system counter "gen-0-size" from "System.Runtime"
                module.Counters.Add(new EventCounterCollectionRequest("System.Runtime", "gen-0-size"));
            }
        );
    }
```

## <a name="disabling-eventcounter-collection-module"></a>Deshabilitación del módulo de recopilación EventCounter

`EventCounterCollectionModule` se puede deshabilitar mediante `ApplicationInsightsServiceOptions`. A continuación se muestra un ejemplo de cómo utilizar el SDK de ASP.NET Core.

```csharp
    using Microsoft.ApplicationInsights.AspNetCore.Extensions;
    using Microsoft.Extensions.DependencyInjection;

    public void ConfigureServices(IServiceCollection services)
    {
        //... other code...

        var applicationInsightsServiceOptions = new ApplicationInsightsServiceOptions();
        applicationInsightsServiceOptions.EnableEventCounterCollectionModule = false;
        services.AddApplicationInsightsTelemetry(applicationInsightsServiceOptions);
    }
```

También se puede usar un enfoque similar para el SDK de WorkerService, pero se debe cambiar el espacio de nombres tal y como se muestra en el ejemplo siguiente.

```csharp
    using Microsoft.ApplicationInsights.WorkerService;
    using Microsoft.Extensions.DependencyInjection;

    var applicationInsightsServiceOptions = new ApplicationInsightsServiceOptions();
    applicationInsightsServiceOptions.EnableEventCounterCollectionModule = false;
    services.AddApplicationInsightsTelemetryWorkerService(applicationInsightsServiceOptions);
```

## <a name="event-counters-in-metric-explorer"></a>Los contadores de eventos en el explorador de métricas

Para ver las métricas de EventCounter en el [explorador de métricas](../platform/metrics-charts.md), seleccione el recurso de Application Insights y elija las métricas basadas en registros como espacio de nombres de métrica. Después, las métricas de EventCounter se mostrarán en la categoría Custom.

> [!div class="mx-imgBorder"]
> ![Contadores de eventos notificados en el explorador de métricas de Application Insights](./media/event-counters/metrics-explorer-counter-list.png)

## <a name="event-counters-in-analytics"></a>Contadores de eventos en Analytics

También puede buscar y mostrar informes de los contadores de eventos en [Analytics](../log-query/log-query-overview.md), en la tabla **customMetrics**.

Por ejemplo, ejecute la siguiente consulta para ver qué contadores se recopilan y están disponibles para consulta:

```Kusto
customMetrics | summarize avg(value) by name
```

> [!div class="mx-imgBorder"]
> ![Contadores de eventos notificados en los análisis de Application Insights](./media/event-counters/analytics-event-counters.png)

Para un gráfico de un contador específico (por ejemplo, `ThreadPool Completed Work Item Count`) en el período más reciente, ejecute la consulta siguiente.

```Kusto
customMetrics 
| where name contains "System.Runtime|ThreadPool Completed Work Item Count"
| where timestamp >= ago(1h)
| summarize  avg(value) by cloud_RoleInstance, bin(timestamp, 1m)
| render timechart
```
> [!div class="mx-imgBorder"]
> ![Gráfico de un solo contador en Application Insights](./media/event-counters/analytics-completeditems-counters.png)

Al igual que otros datos de telemetría, **customMetrics** también tiene una columna `cloud_RoleInstance` que indica la identidad de la instancia del servidor host en el que se ejecuta la aplicación. La consulta anterior muestra el valor del contador por instancia y se puede usar para comparar el rendimiento de las distintas instancias del servidor.

## <a name="alerts"></a>Alertas
Al igual que otras métricas, puede [establecer una alerta](../platform/alerts-log.md) para advertirle si un contador de eventos supera un límite especificado. Abra el panel de alertas y haga clic en Agregar alerta.

## <a name="frequently-asked-questions"></a>Preguntas más frecuentes

### <a name="can-i-see-eventcounters-in-live-metrics"></a>¿Puedo ver los contadores de eventos en Live Metrics?

A día de hoy, Live Metrics no muestra los contadores de eventos. Para ver la telemetría, use el explorador de métricas o el análisis.

### <a name="i-have-enabled-application-insights-from-azure-web-app-portal-but-i-cant-see-eventcounters"></a>He habilitado Application Insights en el portal de aplicaciones web de Azure, pero no veo contadores de eventos.

 La [extensión de Application Insights](./azure-web-apps.md) para ASP.NET Core aún no admite esta característica. Este documento se actualizará cuando se admita la característica.

## <a name="next-steps"></a><a name="next"></a>Pasos siguientes

* [Seguimiento de dependencias](./asp-net-dependencies.md)

