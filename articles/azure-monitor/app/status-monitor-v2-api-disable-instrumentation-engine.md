---
title: Referencia de la API de Azure Application Insights Agent
description: Referencia de la API de Application Insights Agent. Disable-InstrumentationEngine. Supervise el rendimiento de los sitios web sin volver a implementarlos. Funciona con las aplicaciones web de ASP.NET hospedadas en local, en las máquinas virtuales o en Azure.
ms.topic: conceptual
author: TimothyMothra
ms.author: tilee
ms.date: 04/23/2019
ms.openlocfilehash: dafa12db95a10df467bf0d042cfd9395720648b2
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "77671382"
---
# <a name="application-insights-agent-api-disable-instrumentationengine"></a>API de Application Insights Agent: Disable-InstrumentationEngine

Este artículo describe un cmdlet que forma parte del [módulo Az.ApplicationMonitor de PowerShell](https://www.powershellgallery.com/packages/Az.ApplicationMonitor/).

## <a name="description"></a>Descripción
Deshabilita el motor de instrumentación mediante la eliminación de algunas claves del Registro.
Reinicie IIS para aplicar los cambios.

> [!IMPORTANT] 
> Este cmdlet requiere una sesión de PowerShell con permisos de administrador.

## <a name="examples"></a>Ejemplos

```powershell
PS C:\> Disable-InstrumentationEngine
```

## <a name="parameters"></a>Parámetros 

### <a name="-verbose"></a>-Verbose
**Parámetro común.** Use este modificador para generar registros detallados.

## <a name="output"></a>Output


#### <a name="example-output-from-successfully-disabling-the-instrumentation-engine"></a>Salida de ejemplo al deshabilitar correctamente el motor de instrumentación

```
Configuring IIS Environment for instrumentation engine...
Registry: removing 'HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\IISADMIN[Environment]'
Registry: removing 'HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\W3SVC[Environment]'
Registry: removing 'HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\WAS[Environment]'
Configuring registry for instrumentation engine...
```


## <a name="next-steps"></a>Pasos siguientes

 Haga mucho más con Application Insights Agent:
 - Use nuestra guía para [solucionar problemas](status-monitor-v2-troubleshoot.md) de Application Insights Agent.
