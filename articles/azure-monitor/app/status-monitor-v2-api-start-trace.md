---
title: Referencia de la API de Azure Application Insights Agent
description: Referencia de la API de Application Insights Agent. Start-Trace. Recopile registros de ETW desde el Monitor de estado y el SDK de Application Insights.
ms.topic: conceptual
author: TimothyMothra
ms.author: tilee
ms.date: 04/23/2019
ms.openlocfilehash: b9680101f1a22dd6d9c1617c8afc13a10ad1c594
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "77671229"
---
# <a name="application-insights-agent-api-start-applicationinsightsmonitoringtrace"></a>API de Application Insights Agent: Start-ApplicationInsightsMonitoringTrace

Este artículo describe un cmdlet que forma parte del [módulo Az.ApplicationMonitor de PowerShell](https://www.powershellgallery.com/packages/Az.ApplicationMonitor/).

## <a name="description"></a>Descripción

Recopila [eventos de ETW](https://docs.microsoft.com/windows/desktop/etw/event-tracing-portal) desde el runtime para adjuntar sin código. Este cmdlet es una alternativa a la ejecución de [PerfView](https://github.com/microsoft/perfview).

Los eventos recopilados se imprimirán en la consola en tiempo real y se guardarán en un archivo ETL. [PerfView](https://github.com/microsoft/perfview) puede abrir el archivo ETL de salida para llevar a cabo una investigación más extensa.

Este cmdlet se ejecutará hasta alcanzar que se agote el tiempo de espera (con un valor predeterminado de 5 minutos) o hasta que se lo detiene manualmente (`Ctrl + C`).

> [!IMPORTANT] 
> Este cmdlet requiere una sesión de PowerShell con permisos de administrador.

## <a name="examples"></a>Ejemplos

### <a name="how-to-collect-events"></a>Recopilación de eventos

Por lo general, le pediríamos que recopile eventos para investigar por qué no se está instrumentando la aplicación.

El runtime para adjuntar sin código emitirá eventos de ETW cuando se inician IIA y la aplicación.

Para recopilar estos eventos:
1. En una consola cmd con privilegio de administración, ejecute `iisreset /stop` para desactivar IIS y todas las aplicaciones web.
2. Ejecute este cmdlet.
3. En una consola cmd con privilegio de administración ejecute `iisreset /start` para iniciar IIS.
4. Intente ir a la aplicación.
5. Cuando la aplicación termina de cargarse, puede detenerla de manera manual (`Ctrl + C`) o esperar a que se agote el tiempo de espera.

### <a name="what-events-to-collect"></a>Qué eventos se recopilan

Existen tres opciones al recopilar los eventos:
1. Use el modificador `-CollectSdkEvents` para recopilar los eventos emitidos desde el SDK de Application Insights.
2. Use el modificador `-CollectRedfieldEvents` para recopilar los eventos que emite el Monitor de estado y el runtime de Redfield. Estos registros son útiles al diagnosticar el inicio de IIS y de las aplicaciones.
3. Use ambos modificadores para recopilar ambos tipos de evento.
4. De manera predeterminada, si no se especifica ningún modificador, se recopilarán ambos tipos de evento.


## <a name="parameters"></a>Parámetros

### <a name="-maxdurationinminutes"></a>-MaxDurationInMinutes
**Opcional.** Use este parámetro para establecer durante cuánto tiempo este script debe recopilar eventos. El valor predeterminado es 5 minutos.

### <a name="-logdirectory"></a>-LogDirectory
**Opcional.** Use este conmutador para establecer el directorio de salida del archivo ETL. De manera predeterminada, este archivo se creará en el directorio de módulos de PowerShell. Se mostrará la ruta de acceso completa durante la ejecución de un script.


### <a name="-collectsdkevents"></a>-CollectSdkEvents
**Opcional.** Use este modificador para recopilar eventos del SDK de Application Insights.

### <a name="-collectredfieldevents"></a>-CollectRedfieldEvents
**Opcional.** Use este modificador para recopilar eventos desde el Monitor de estado y el runtime de Redfield.

### <a name="-verbose"></a>-Verbose
**Parámetro común.** Use este modificador para generar registros detallados.



## <a name="output"></a>Output


### <a name="example-of-application-startup-logs"></a>Ejemplo de registros de inicio de aplicación
```
PS C:\Windows\system32> Start-ApplicationInsightsMonitoringTrace -ColectRedfieldEvents
Starting...
Log File: C:\Program Files\WindowsPowerShell\Modules\Az.ApplicationMonitor\content\logs\20190627_144217_ApplicationInsights_ETW_Trace.etl
Tracing enabled, waiting for events.
Tracing will timeout in 5 minutes. Press CTRL+C to cancel.

2:42:31 PM EVENT: Microsoft-ApplicationInsights-IIS-ManagedHttpModuleHelper Trace Resolved variables to: MicrosoftAppInsights_ManagedHttpModulePath='C:\Program Files\WindowsPowerShell\Modules\Az.ApplicationMonitor\content\Runtime\Microsoft.ApplicationInsights.RedfieldIISModule.dll', MicrosoftAppInsights_ManagedHttpModuleType='Microsoft.ApplicationInsights.RedfieldIISModule.RedfieldIISModule'
2:42:31 PM EVENT: Microsoft-ApplicationInsights-IIS-ManagedHttpModuleHelper Trace Resolved variables to: MicrosoftDiagnosticServices_ManagedHttpModulePath2='', MicrosoftDiagnosticServices_ManagedHttpModuleType2=''
2:42:31 PM EVENT: Microsoft-ApplicationInsights-IIS-ManagedHttpModuleHelper Trace Environment variable 'MicrosoftDiagnosticServices_ManagedHttpModulePath2' or 'MicrosoftDiagnosticServices_ManagedHttpModuleType2' is null, skipping managed dll loading
2:42:31 PM EVENT: Microsoft-ApplicationInsights-IIS-ManagedHttpModuleHelper Trace MulticastHttpModule.constructor, success, 70 ms
2:42:31 PM EVENT: Microsoft-ApplicationInsights-RedfieldIISModule Trace Current assembly 'Microsoft.ApplicationInsights.RedfieldIISModule, Version=2.8.18.27202, Culture=neutral, PublicKeyToken=f23a46de0be5d6f3' location 'C:\Program Files\WindowsPowerShell\Modules\Az.ApplicationMonitor\content\Runtime\Microsoft.ApplicationInsights.RedfieldIISModule.dll'
2:42:31 PM EVENT: Microsoft-ApplicationInsights-RedfieldIISModule Trace Matched filter '.*'~'STATUSMONITORTE', '.*'~'DemoWithSql'
2:42:31 PM EVENT: Microsoft-ApplicationInsights-RedfieldIISModule Trace Lightup assembly calculated path: 'C:\Program Files\WindowsPowerShell\Modules\Az.ApplicationMonitor\content\Runtime\Microsoft.ApplicationInsights.Redfield.Lightup.dll'
2:42:31 PM EVENT: Microsoft-ApplicationInsights-FrameworkLightup Trace Loaded applicationInsights.config from assembly's resource Microsoft.ApplicationInsights.Redfield.Lightup, Version=2.8.18.27202, Culture=neutral, PublicKeyToken=f23a46de0be5d6f3/Microsoft.ApplicationInsights.Redfield.Lightup.ApplicationInsights-recommended.config
2:42:34 PM EVENT: Microsoft-ApplicationInsights-FrameworkLightup Trace Successfully attached ApplicationInsights SDK
2:42:34 PM EVENT: Microsoft-ApplicationInsights-RedfieldIISModule Trace RedfieldIISModule.LoadLightupAssemblyAndGetLightupHttpModuleClass, success, 2687 ms
2:42:34 PM EVENT: Microsoft-ApplicationInsights-RedfieldIISModule Trace RedfieldIISModule.CreateAndInitializeApplicationInsightsHttpModules(lightupHttpModuleClass), success
2:42:34 PM EVENT: Microsoft-ApplicationInsights-IIS-ManagedHttpModuleHelper Trace ManagedHttpModuleHelper, multicastHttpModule.Init() success, 3288 ms
2:42:35 PM EVENT: Microsoft-ApplicationInsights-IIS-ManagedHttpModuleHelper Trace Resolved variables to: MicrosoftAppInsights_ManagedHttpModulePath='C:\Program Files\WindowsPowerShell\Modules\Az.ApplicationMonitor\content\Runtime\Microsoft.ApplicationInsights.RedfieldIISModule.dll', MicrosoftAppInsights_ManagedHttpModuleType='Microsoft.ApplicationInsights.RedfieldIISModule.RedfieldIISModule'
2:42:35 PM EVENT: Microsoft-ApplicationInsights-IIS-ManagedHttpModuleHelper Trace Resolved variables to: MicrosoftDiagnosticServices_ManagedHttpModulePath2='', MicrosoftDiagnosticServices_ManagedHttpModuleType2=''
2:42:35 PM EVENT: Microsoft-ApplicationInsights-IIS-ManagedHttpModuleHelper Trace Environment variable 'MicrosoftDiagnosticServices_ManagedHttpModulePath2' or 'MicrosoftDiagnosticServices_ManagedHttpModuleType2' is null, skipping managed dll loading
2:42:35 PM EVENT: Microsoft-ApplicationInsights-IIS-ManagedHttpModuleHelper Trace MulticastHttpModule.constructor, success, 0 ms
2:42:35 PM EVENT: Microsoft-ApplicationInsights-RedfieldIISModule Trace RedfieldIISModule.CreateAndInitializeApplicationInsightsHttpModules(lightupHttpModuleClass), success
2:42:35 PM EVENT: Microsoft-ApplicationInsights-IIS-ManagedHttpModuleHelper Trace ManagedHttpModuleHelper, multicastHttpModule.Init() success, 0 ms
Timeout Reached. Stopping...
```


## <a name="next-steps"></a>Pasos siguientes

Más soluciones de problemas:

- Revise aquí los pasos para más soluciones de problemas: https://docs.microsoft.com/azure/azure-monitor/app/status-monitor-v2-troubleshoot
- Revise la [referencia de API](status-monitor-v2-overview.md#powershell-api-reference) para obtener información sobre los parámetros que podría haber pasado por alto.
- Si necesita ayuda adicional, puede ponerse en contacto con nosotros en [GitHub](https://github.com/Microsoft/ApplicationInsights-Home/issues).



 Haga mucho más con Application Insights Agent:
 - Use nuestra guía para [solucionar problemas](status-monitor-v2-troubleshoot.md) de Application Insights Agent.
 - [Obtenga la configuración](status-monitor-v2-api-get-config.md) para confirmar que sus opciones se registraron correctamente.
 - [Obtenga el estado](status-monitor-v2-api-get-status.md) para inspeccionar la supervisión.
