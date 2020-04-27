---
title: Solución de problemas con Change Tracking e Inventario
description: Obtenga información acerca de cómo solucionar problemas con la solución Change Tracking e Inventario de Azure Automation.
services: automation
ms.service: automation
ms.subservice: change-inventory-management
author: mgoedtel
ms.author: magoedte
ms.date: 01/31/2019
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: 11c1fd05055922b07801c20d525d852d5360b069
ms.sourcegitcommit: acb82fc770128234f2e9222939826e3ade3a2a28
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/21/2020
ms.locfileid: "81679347"
---
# <a name="troubleshoot-change-tracking-and-inventory-issues"></a>Solución de problemas de Change Tracking e Inventario

En este artículo se describe cómo solucionar problemas de Change Tracking e Inventario.

>[!NOTE]
>Este artículo se ha actualizado para usar el nuevo módulo Az de Azure PowerShell. Aún puede usar el módulo de AzureRM que continuará recibiendo correcciones de errores hasta diciembre de 2020 como mínimo. Para más información acerca del nuevo módulo Az y la compatibilidad con AzureRM, consulte [Introducing the new Azure PowerShell Az module](https://docs.microsoft.com/powershell/azure/new-azureps-module-az?view=azps-3.5.0) (Presentación del nuevo módulo Az de Azure PowerShell). Para obtener instrucciones sobre la instalación del módulo Az en Hybrid Runbook Worker, consulte [Instalación del módulo de Azure PowerShell](https://docs.microsoft.com/powershell/azure/install-az-ps?view=azps-3.5.0). Puede actualizar los módulos de su cuenta de Automation a la versión más reciente mediante [Actualización de módulos de Azure PowerShell en Azure Automation](../automation-update-azure-modules.md).

## <a name="windows"></a>Windows

### <a name="scenario-change-tracking-and-inventory-records-arent-showing-for-windows-machines"></a><a name="records-not-showing-windows"></a>Escenario: no se muestran los registros de Change Tracking e Inventario para máquinas Windows

#### <a name="issue"></a>Problema

No aparecen los resultados de Change Tracking e Inventario para las máquinas Windows que se han incorporado.

#### <a name="cause"></a>Causa

Este error puede tener las causas siguientes:

* El agente de Log Analytics para Windows no se está ejecutando.
* Está bloqueada la comunicación a la cuenta de Automation.
* No se han descargado los módulos de administración de Change Tracking e Inventario.
* La VM que se está incorporando puede provenir de una máquina clonada que no estaba preparada con sysprep con el agente de Log Analytics instalado.

#### <a name="resolution"></a>Solución

En la máquina del agente de Log Analytics, vaya a **C:\Archivos de programa\Microsoft Monitoring Agent\Agent\Tools** y ejecute los siguientes comandos:

```cmd
net stop healthservice
StopTracing.cmd
StartTracing.cmd VER
net start healthservice
```

Si sigue necesitando ayuda, puede recopilar la información de diagnóstico y ponerse en contacto con el equipo de soporte técnico. 

> [!NOTE]
> El agente de log Analytics habilita el seguimiento de errores de forma predeterminada. Para habilitar los mensajes de error detallados como en el ejemplo anterior, use el parámetro `VER`. Para más información sobre los seguimientos, use `INF` cuando invoque `StartTracing.cmd`.

##### <a name="log-analytics-agent-for-windows-not-running"></a>El agente de Log Analytics para Windows no se está ejecutando

Compruebe que el agente de Log Analytics para Windows (**HealthService.exe**) está en ejecución en la máquina.

##### <a name="communication-to-automation-account-blocked"></a>Bloqueada la comunicación con la cuenta de Automation

Consulte el Visor de eventos en la máquina y busque cualquier evento que contenga la palabra `changetracking`.

Consulte [Automatización de los recursos en el centro de datos o la nube con Hybrid Runbook Worker](../automation-hybrid-runbook-worker.md#network-planning) para obtener información acerca de las direcciones y los puertos que se deben permitir para que Change Tracking e Inventario funcione.

##### <a name="management-packs-not-downloaded"></a>Módulos de administración no descargados

Compruebe que los siguientes módulos de administración de Change Tracking e Inventario están instalados en el entorno local:

* `Microsoft.IntelligencePacks.ChangeTrackingDirectAgent.*`
* `Microsoft.IntelligencePacks.InventoryChangeTracking.*`
* `Microsoft.IntelligencePacks.SingletonInventoryCollection.*`

##### <a name="vm-from-cloned-machine-that-has-not-been-sysprepped"></a>Máquina virtual a partir de una máquina clonada que no se ha preparado con sysprep

Si utiliza una imagen clonada, primero prepare con sysprep la imagen y, a continuación, instale el agente de Log Analytics para Windows.

## <a name="linux"></a>Linux

### <a name="scenario-no-change-tracking-and-inventory-results-on-linux-machines"></a>Escenario: No hay resultados de Change Tracking e Inventario en las máquinas Linux

#### <a name="issue"></a>Problema

No ve los resultados de Change Tracking e Inventario de las máquinas Linux que se han incorporado para la solución. 

#### <a name="cause"></a>Causa
Estas son algunas causas posibles específicas de este problema:
* El agente de Log Analytics para Linux no está en ejecución.
* El agente de Log Analytics para Linux no está configurado correctamente.
* Hay conflictos de supervisión de la integridad de los archivos (FIM).

#### <a name="resolution"></a>Solución 

##### <a name="log-analytics-agent-for-linux-not-running"></a>El agente de Log Analytics para Linux no está en ejecución

Compruebe que el demonio del agente de Log Analytics para Linux (**omsagent**) está en ejecución en la máquina. Ejecute la siguiente consulta en el área de trabajo de Log Analytics que está vinculada a la cuenta de Automation.

```loganalytics Copy
Heartbeat
| summarize by Computer, Solutions
```

Si no ve la máquina en los resultados de la consulta, no se ha registrado recientemente. Probablemente haya un problema de configuración local y debe volver a instalar el agente. Para obtener información sobre la instalación y configuración, consulte [Recopilación de datos de registro con el agente de Log Analytics](https://docs.microsoft.com/azure/azure-monitor/platform/log-analytics-agent). 

Si el equipo aparece en los resultados de la consulta, compruebe la configuración de ámbito. Consulte [Soluciones de supervisión como destino en Azure Monitor](https://docs.microsoft.com/azure/azure-monitor/insights/solution-targeting).

Para más información sobre cómo solucionar este problema, consulte [Problema: no se ve ningún dato de Linux](https://docs.microsoft.com/azure/azure-monitor/platform/agent-linux-troubleshoot#issue-you-are-not-seeing-any-linux-data).

##### <a name="log-analytics-agent-for-linux-not-configured-correctly"></a>El agente de Log Analytics para Linux no está configurado correctamente

Es posible que el agente de Log Analytics para Linux no esté correctamente configurado para el registro y la recopilación de la salida de la línea de comandos mediante la herramienta Recopilador de registros de OMS. Consulte [Seguimiento de cambios en el entorno con la solución Change Tracking e Inventario](../change-tracking.md).

##### <a name="fim-conflicts"></a>Conflictos de FIM

La característica FIM de Azure Security Center podría estar validando incorrectamente la integridad de los archivos de Linux. Compruebe que FIM está operativo y configurado correctamente para la supervisión de archivos de Linux. Consulte [Seguimiento de cambios en el entorno con la solución Change Tracking e Inventario](../change-tracking.md).

## <a name="next-steps"></a>Pasos siguientes

Si su problema no aparece arriba o no puede resolverlo, intente obtener ayuda adicional a través de uno de los siguientes canales:

* Obtenga respuestas de expertos de Azure en los [foros de Azure](https://azure.microsoft.com/support/forums/).
* Póngase en contacto con [@AzureSupport](https://twitter.com/azuresupport), la cuenta oficial de Microsoft Azure para mejorar la experiencia del cliente, que pone en contacto a la comunidad de Azure con los recursos adecuados: respuestas, soporte técnico y expertos.
* Registrar un incidente de soporte técnico de Azure. Vaya al [sitio de soporte técnico de Azure](https://azure.microsoft.com/support/options/) y seleccione **Obtener soporte**.
