---
title: Más información sobre Azure Application Insights | Microsoft Docs
description: Aquí se presentan algunas características que puede explorar una vez que sepa utilizar Application Insights.
ms.topic: conceptual
ms.date: 02/03/2017
ms.openlocfilehash: 6d30833840b62412ae03c761baacf54c939c9d33
ms.sourcegitcommit: 3543d3b4f6c6f496d22ea5f97d8cd2700ac9a481
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/20/2020
ms.locfileid: "86540033"
---
# <a name="more-telemetry-from-application-insights"></a>Más telemetría desde Application Insights
Una vez que haya [agregado Application Insights a su código de ASP.NET](../../azure-monitor/app/asp-net.md), puede realizar varias acciones para obtener más datos de telemetría. 

| Acción | Lo que obtiene|
|---|---|
|(Servidores IIS) [Instale Monitor de estado](https://go.microsoft.com/fwlink/?LinkId=506648) en cada máquina del servidor.<br/>(Aplicaciones web de Azure) En el panel de control de Azure de la aplicación web, abra la hoja Application Insights.| [**Contadores de rendimiento**](../../azure-monitor/app/performance-counters.md).<br/>[**Excepciones**](asp-net-exceptions.md): seguimiento de la pila detallado.<br/>[**Dependencias**](../../azure-monitor/app/asp-net-dependencies.md).|
|[Agregue el fragmento de código de JavaScript en las páginas web](../../azure-monitor/app/javascript.md).|[Rendimiento de página](../../azure-monitor/app/usage-overview.md), excepciones de explorador y rendimiento de AJAX. Telemetría del lado cliente personalizada.|
|[Cree pruebas web de disponibilidad](../../azure-monitor/app/monitor-web-app-availability.md).|Obtención de alertas si el sitio deja de estar disponible.|
|[Asegúrese de que se genera buildinfo.config](/visualstudio/debugger/diagnose-problems-after-deployment?view=vs-2015) mediante MSBuild.|[Generación de anotaciones en gráficos de métricas](./annotations.md)
|[Cree eventos y métricas personalizados](../../azure-monitor/app/api-custom-events-metrics.md).|Recuento de métricas y eventos empresariales, seguimiento de uso detallado y mucho más.|
|[Genere un perfil del sitio activo.](https://aka.ms/AIProfilerPreview)|Intervalos de función detallados de la aplicación web activa.|
