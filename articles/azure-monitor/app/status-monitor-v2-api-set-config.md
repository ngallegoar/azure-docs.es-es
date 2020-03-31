---
title: Referencia de la API de Azure Application Insights Agent
description: Referencia de la API de Application Insights Agent. Set-ApplicationInsightsMonitoringConfig. Supervise el rendimiento de los sitios web sin volver a implementarlos. Funciona con las aplicaciones web de ASP.NET hospedadas en local, en las máquinas virtuales o en Azure.
ms.topic: conceptual
author: TimothyMothra
ms.author: tilee
ms.date: 04/23/2019
ms.openlocfilehash: 1226b3e10adf786ed3335844a5d3f4e530911705
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "77671246"
---
# <a name="application-insights-agent-api-set-applicationinsightsmonitoringconfig"></a>API de Application Insights Agent: Set-ApplicationInsightsMonitoringConfig

Este documento describe un cmdlet que forma parte del [módulo Az.ApplicationMonitor de PowerShell](https://www.powershellgallery.com/packages/Az.ApplicationMonitor/).

## <a name="description"></a>Descripción

Establece el archivo de configuración sin tener que realizar una reinstalación completa.
Reinicie IIS para que los cambios surtan efecto.

> [!IMPORTANT] 
> Este cmdlet requiere una sesión de PowerShell con permisos de administrador.


## <a name="examples"></a>Ejemplos

### <a name="example-with-a-single-instrumentation-key"></a>Ejemplo con una única clave de instrumentación
En este ejemplo, a todas las aplicaciones en el equipo actual se les asignará una única clave de instrumentación.

```powershell
PS C:\> Enable-ApplicationInsightsMonitoring -InstrumentationKey xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx
```

### <a name="example-with-an-instrumentation-key-map"></a>Ejemplo con un mapa de claves de instrumentación
En este ejemplo:
- `MachineFilter` busca la correspondencia con el equipo actual usando el comodín `'.*'`.
- `AppFilter='WebAppExclude'` proporciona una clave de instrumentación `null`. No se puede instrumentar la aplicación especificada.
- `AppFilter='WebAppOne'` asigna una clave de instrumentación única a la aplicación especificada.
- `AppFilter='WebAppTwo'` asigna una clave de instrumentación única a la aplicación especificada.
- Por último, `AppFilter` también usa el comodín `'.*'` para buscar la correspondencia de todas las aplicaciones web que no coincidan según las reglas anteriores y les asigna una clave de instrumentación predeterminada.
- Se agregan espacios para mejorar la legibilidad.

```powershell
Enable-ApplicationInsightsMonitoring -InstrumentationKeyMap `
       @(@{MachineFilter='.*';AppFilter='WebAppExclude'},
          @{MachineFilter='.*';AppFilter='WebAppOne';InstrumentationSettings=@{InstrumentationKey='xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxx1'}},
          @{MachineFilter='.*';AppFilter='WebAppTwo';InstrumentationSettings=@{InstrumentationKey='xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxx2'}},
          @{MachineFilter='.*';AppFilter='.*';InstrumentationSettings=@{InstrumentationKey='xxxxxxxx-xxxx-xxxx-xxxx-xxxxxdefault'}})
```

## <a name="parameters"></a>Parámetros

### <a name="-instrumentationkey"></a>-InstrumentationKey
**Requerido.** Use este parámetro para proporcionar una única clave de instrumentación para que la usen todas las aplicaciones en el equipo de destino.

### <a name="-instrumentationkeymap"></a>-InstrumentationKeyMap
**Requerido.** Use este parámetro para proporcionar varias claves de instrumentación y una asignación de las claves de instrumentación utilizadas por cada aplicación.
Puede crear un solo script de instalación para varios equipos si establece `MachineFilter`.

> [!IMPORTANT]
> Las aplicaciones se harán coincidir con las reglas en el orden en que estas se proporcionan. Por tanto, debe especificar las reglas más específicas en primer lugar y las más genéricas las últimas.

#### <a name="schema"></a>Schema
`@(@{MachineFilter='.*';AppFilter='.*';InstrumentationKey='xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx'})`

- **MachineFilter** es una expresión regular de C# requerida del nombre de la máquina virtual o del equipo.
    - ".*" coincidirá con todos
    - "ComputerName" coincidirá solo son aquellos equipos que tengan el nombre especificado.
- **AppFilter** es una expresión regular de C# requerida del nombre de la máquina virtual o del equipo.
    - ".*" coincidirá con todos
    - "ApplicationName" coincidirá solo son aquellas aplicaciones IIS que tengan el nombre especificado.
- **InstrumentationKey** se requiere para habilitar la supervisión de las aplicaciones que coincidan con los dos filtros anteriores.
    - Deje este valor como null si desea definir reglas para excluir la supervisión.


### <a name="-verbose"></a>-Verbose
**Parámetro común.** Utilice este modificador para mostrar registros detallados.


## <a name="output"></a>Output

De forma predeterminada, no se produce ninguna salida.

#### <a name="example-verbose-output-from-setting-the-config-file-via--instrumentationkey"></a>Ejemplo de salida detallada de la configuración en el archivo de configuración a través de -InstrumentationKey

```
VERBOSE: Operation: InstallWithIkey
VERBOSE: InstrumentationKeyMap parsed:
Filters:
0)InstrumentationKey: xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx AppFilter: .* MachineFilter: .*
VERBOSE: set config file
VERBOSE: Config File Path:
C:\Program Files\WindowsPowerShell\Modules\Az.ApplicationMonitor\content\applicationInsights.ikey.config
```

#### <a name="example-verbose-output-from-setting-the-config-file-via--instrumentationkeymap"></a>Ejemplo de salida detallada de la configuración en el archivo de configuración a través de -InstrumentationKeyMap

```
VERBOSE: Operation: InstallWithIkeyMap
VERBOSE: InstrumentationKeyMap parsed:
Filters:
0)InstrumentationKey:  AppFilter: WebAppExclude MachineFilter: .*
1)InstrumentationKey: xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxx2 AppFilter: WebAppTwo MachineFilter: .*
2)InstrumentationKey: xxxxxxxx-xxxx-xxxx-xxxx-xxxxxdefault AppFilter: .* MachineFilter: .*
VERBOSE: set config file
VERBOSE: Config File Path:
C:\Program Files\WindowsPowerShell\Modules\Az.ApplicationMonitor\content\applicationInsights.ikey.config
```

## <a name="next-steps"></a>Pasos siguientes

  Vea la telemetría:
 - [Explore las métricas](../../azure-monitor/app/metrics-explorer.md) para supervisar el rendimiento y el uso.
- [Busque en los eventos y los registros](../../azure-monitor/app/diagnostic-search.md) para diagnosticar problemas.
- [Use Analytics](../../azure-monitor/app/analytics.md) para consultas más avanzadas.
- [Cree paneles](../../azure-monitor/app/overview-dashboard.md).
 
 Agregue más telemetría:
 - [Cree pruebas web](monitor-web-app-availability.md) para asegurarse de que el sitio permanece activo.
- [Agregue telemetría de cliente web](../../azure-monitor/app/javascript.md) para ver las excepciones de código de la página web y para habilitar las llamadas de seguimiento.
- [Agregue el SDK de Application Insights al código](../../azure-monitor/app/asp-net.md) para que pueda insertar llamadas de seguimiento y registro.
 
 Haga mucho más con Application Insights Agent:
 - Use nuestra guía para [solucionar problemas](status-monitor-v2-troubleshoot.md) de Application Insights Agent.
 - [Obtenga la configuración](status-monitor-v2-api-get-config.md) para confirmar que sus opciones se registraron correctamente.
 - [Obtenga el estado](status-monitor-v2-api-get-status.md) para inspeccionar la supervisión.
