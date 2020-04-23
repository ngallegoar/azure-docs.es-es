---
title: Referencia de la API de Azure Application Insights Agent
description: Referencia de la API de Application Insights Agent. Enable-InstrumentationEngine. Supervise el rendimiento de los sitios web sin volver a implementarlos. Funciona con las aplicaciones web de ASP.NET hospedadas en local, en las máquinas virtuales o en Azure.
ms.topic: conceptual
author: TimothyMothra
ms.author: tilee
ms.date: 04/23/2019
ms.openlocfilehash: 70102395b94c89d4ce0edfddeda1df0be0e2f216
ms.sourcegitcommit: 31ef5e4d21aa889756fa72b857ca173db727f2c3
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/16/2020
ms.locfileid: "81536767"
---
# <a name="application-insights-agent-api-enable-instrumentationengine"></a>API de Application Insights Agent: Enable-InstrumentationEngine

Este artículo describe un cmdlet que forma parte del [módulo Az.ApplicationMonitor de PowerShell](https://www.powershellgallery.com/packages/Az.ApplicationMonitor/).

## <a name="description"></a>Descripción

Habilita el motor de instrumentación mediante la definición de algunas claves del Registro.
Reinicie IIS para aplicar los cambios.

El motor de instrumentación puede complementar los datos recopilados por los SDK de .NET.
Recopila los eventos y mensajes que describen la ejecución de un proceso administrado. Estos eventos y mensajes incluyen códigos de resultado de la dependencia, verbos HTTP y [texto de comando SQL](asp-net-dependencies.md#advanced-sql-tracking-to-get-full-sql-query).

Habilite el motor de instrumentación si:
- Ya ha habilitado la supervisión con el cmdlet Enable pero no ha habilitado el motor de instrumentación.
- Ha instrumentado la aplicación manualmente con los SDK de .NET y desea recopilar datos de telemetría adicionales.

> [!IMPORTANT] 
> Este cmdlet requiere una sesión de PowerShell con permisos de administrador.

> [!NOTE] 
> - Este cmdlet requiere que revise y acepte la licencia, y la declaración de privacidad.
> - El motor de instrumentación produce más sobrecarga y está desactivado de forma predeterminada.

## <a name="examples"></a>Ejemplos

```powershell
PS C:\> Enable-InstrumentationEngine
```

## <a name="parameters"></a>Parámetros

### <a name="-acceptlicense"></a>-AcceptLicense
**Opcional.** Use este modificador para aceptar la licencia y la declaración de privacidad en las instalaciones sin periféricos.

### <a name="-verbose"></a>-Verbose
**Parámetro común.** Use este modificador para generar registros detallados.

## <a name="output"></a>Output


#### <a name="example-output-from-successfully-enabling-the-instrumentation-engine"></a>Salida de ejemplo al habilitar correctamente el motor de instrumentación

```
Configuring IIS Environment for instrumentation engine...
Configuring registry for instrumentation engine...
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
 - [Obtenga la configuración](status-monitor-v2-api-get-config.md) para confirmar que sus opciones se registraron correctamente.
 - [Obtenga el estado](status-monitor-v2-api-get-status.md) para inspeccionar la supervisión.
