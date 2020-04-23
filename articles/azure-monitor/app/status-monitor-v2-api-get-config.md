---
title: Referencia de la API de Azure Application Insights Agent
description: Referencia de la API de Application Insights Agent. Get-ApplicationInsightsMonitoringConfig. Supervise el rendimiento de los sitios web sin volver a implementarlos. Funciona con las aplicaciones web de ASP.NET hospedadas en local, en las máquinas virtuales o en Azure.
ms.topic: conceptual
author: TimothyMothra
ms.author: tilee
ms.date: 04/23/2019
ms.openlocfilehash: 0cb5991b381acc9a0ec36113d0e15a7731b5074d
ms.sourcegitcommit: 31ef5e4d21aa889756fa72b857ca173db727f2c3
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/16/2020
ms.locfileid: "81537498"
---
# <a name="application-insights-agent-api-get-applicationinsightsmonitoringconfig"></a>API de Application Insights Agent: Get-ApplicationInsightsMonitoringConfig

Este artículo describe un cmdlet que forma parte del [módulo Az.ApplicationMonitor de PowerShell](https://www.powershellgallery.com/packages/Az.ApplicationMonitor/).

## <a name="description"></a>Descripción

Obtiene el archivo de configuración e imprime los valores en la consola.

> [!IMPORTANT] 
> Este cmdlet requiere una sesión de PowerShell con permisos de administrador.

## <a name="examples"></a>Ejemplos

```powershell
PS C:\> Get-ApplicationInsightsMonitoringConfig
```

## <a name="parameters"></a>Parámetros

No se requiere ningún parámetro.

## <a name="output"></a>Output


#### <a name="example-output-from-reading-the-config-file"></a>Salida de ejemplo al leer el archivo de configuración

```
RedfieldConfiguration:
Filters:
0)InstrumentationKey:  AppFilter: WebAppExclude MachineFilter: .*
1)InstrumentationKey: xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxx2 AppFilter: WebAppTwo MachineFilter: .*
2)InstrumentationKey: xxxxxxxx-xxxx-xxxx-xxxx-xxxxxdefault AppFilter: .* MachineFilter: .*
```

## <a name="next-steps"></a>Pasos siguientes

  Vea la telemetría:
 - [Explore las métricas](../../azure-monitor/platform/metrics-charts.md) para supervisar el rendimiento y el uso.
- [Busque en los eventos y los registros](../../azure-monitor/app/diagnostic-search.md) para diagnosticar problemas.
- Use [análisis](../../azure-monitor/app/analytics.md) para consultas más avanzadas.
- [Cree paneles](../../azure-monitor/app/overview-dashboard.md).
 
 Agregue más telemetría:
 - [Cree pruebas web](monitor-web-app-availability.md) para asegurarse de que el sitio permanece activo.
- [Agregue telemetría de cliente web](../../azure-monitor/app/javascript.md) para ver las excepciones de código de la página web y para habilitar las llamadas de seguimiento.
- [Agregue el SDK de Application Insights al código](../../azure-monitor/app/asp-net.md) para que pueda insertar llamadas de seguimiento y registro.
 
 Haga mucho más con Application Insights Agent:
 - Use nuestra guía para [solucionar problemas](status-monitor-v2-troubleshoot.md) de Application Insights Agent.
 - Realice cambios en la configuración mediante el cmdlet [Set config](status-monitor-v2-api-set-config.md).
