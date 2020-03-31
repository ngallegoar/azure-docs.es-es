---
title: Solución de problemas con la configuración de Azure Change Tracking
description: Obtenga información acerca de cómo solucionar problemas con la característica Change Tracking e Inventario de Azure Automation.
services: automation
ms.service: automation
ms.subservice: change-inventory-management
author: mgoedtel
ms.author: magoedte
ms.date: 01/31/2019
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: 6cadaea1a20743071acbe8860df02ca7bbdde954
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "77198537"
---
# <a name="troubleshoot-change-tracking-and-inventory"></a>Solución de problemas de Change Tracking e Inventario

## <a name="windows"></a>Windows

### <a name="scenario-change-tracking-records-arent-showing-for-windows-machines"></a><a name="records-not-showing-windows"></a>Escenario: no se muestran los registros de Change Tracking para máquinas Windows

#### <a name="issue"></a>Problema

No aparecen los resultados de Inventario ni de Change Tracking para máquinas Windows que se han incorporado para Change Tracking.

#### <a name="cause"></a>Causa

Este error puede tener las causas siguientes:

* Microsoft Monitoring Agent no está en ejecución.
* Está bloqueada la comunicación a la cuenta de Automation.
* No se han descargado los módulos de administración de Change Tracking.
* La máquina virtual que se incorpora puede provenir de una máquina clonada que no estaba preparada con sysprep con Microsoft Monitoring Agent instalado.

#### <a name="resolution"></a>Solución

Las soluciones que se describen a continuación pueden ayudar a resolver el problema. Si sigue necesitando ayuda, puede recopilar la información de diagnóstico y ponerse en contacto con el equipo de soporte técnico. En el equipo del agente, vaya a C:\Program Files\Microsoft Monitoring Agent\Agent\Tools y ejecute los siguientes comandos:

```cmd
net stop healthservice
StopTracing.cmd
StartTracing.cmd VER
net start healthservice
```

> [!NOTE]
> De forma predeterminada, el seguimiento de errores está habilitado. Para habilitar los mensajes de error detallados como en el ejemplo anterior, use el parámetro *VER*. Para los seguimientos de información, use *INF* al invocar a **StartTracing.cmd**.

##### <a name="microsoft-monitoring-agent-not-running"></a>Microsoft Monitoring Agent no se está ejecutando

Compruebe que Microsoft Monitoring Agent (HealthService.exe) está en ejecución en la máquina.

##### <a name="communication-to-automation-account-blocked"></a>Bloqueada la comunicación con la cuenta de Automation

Consulte el Visor de eventos de la máquina y busque cualquier evento que contenga la palabra "changetracking".

Consulte [Automatización de los recursos en el centro de datos o la nube con Hybrid Runbook Worker](../automation-hybrid-runbook-worker.md#network-planning) para obtener información acerca de las direcciones y los puertos que se deben permitir para que Change Tracking funcione.

##### <a name="management-packs-not-downloaded"></a>Módulos de administración no descargados

Compruebe que los siguientes módulos de administración de Change Tracking e Inventario están instalados en el entorno local:

* Microsoft.IntelligencePacks.ChangeTrackingDirectAgent.*
* Microsoft.IntelligencePacks.InventoryChangeTracking.*
* Microsoft.IntelligencePacks.SingletonInventoryCollection.*

##### <a name="vm-from-cloned-machine-that-has-not-been-sysprepped"></a>Máquina virtual a partir de una máquina clonada que no se ha preparado con sysprep

Si utiliza una imagen clonada, primero prepare con sysprep la imagen e instale después Microsoft Monitoring Agent.

## <a name="linux"></a>Linux

### <a name="scenario-no-change-tracking-or-inventory-results-on-linux-machines"></a>Escenario: No hay resultados de Change Tracking ni Inventario en máquinas Linux

#### <a name="issue"></a>Problema

No ve los resultados de Inventario ni Change Tracking de las máquinas Linux que se han incorporado en Change Tracking. 

#### <a name="cause"></a>Causa
Estas son algunas causas posibles específicas de este problema:
* El agente de Log Analytics para Linux no está en ejecución.
* El agente de Log Analytics para Linux no está configurado correctamente.
* Hay conflictos de supervisión de la integridad de los archivos (FIM).

#### <a name="resolution"></a>Solución 

##### <a name="log-analytics-agent-for-linux-not-running"></a>El agente de Log Analytics para Linux no está en ejecución

Compruebe que el demonio del agente de Log Analytics para Linux (omsagent) está en ejecución en la máquina. Ejecute la siguiente consulta en el área de trabajo de Log Analytics que está vinculada a la cuenta de Automation.

```loganalytics Copy
Heartbeat
| summarize by Computer, Solutions
```

Si no ve la máquina en los resultados de la consulta, no se ha registrado recientemente. Probablemente haya un problema de configuración local y debe volver a instalar el agente. Para obtener información sobre la instalación y configuración, consulte [Recopilación de datos de registro con el agente de Log Analytics](https://docs.microsoft.com/azure/azure-monitor/platform/log-analytics-agent). 

Si el equipo aparece en los resultados de la consulta, compruebe la configuración de ámbito. Consulte [Soluciones de supervisión como destino en Azure Monitor](https://docs.microsoft.com/azure/azure-monitor/insights/solution-targeting).

Para más información sobre cómo solucionar este problema, consulte [Problema: no se ve ningún dato de Linux](https://docs.microsoft.com/azure/azure-monitor/platform/agent-linux-troubleshoot#issue-you-are-not-seeing-any-linux-data).

##### <a name="log-analytics-agent-for-linux-not-configured-correctly"></a>El agente de Log Analytics para Linux no está configurado correctamente

Es posible que el agente de Log Analytics para Linux no esté correctamente configurado para el registro y la recopilación de la salida de la línea de comandos mediante la herramienta Recopilador de registros de OMS. Consulte [Seguimiento de cambios en el entorno con la solución Change Tracking](../change-tracking.md).

##### <a name="fim-conflicts"></a>Conflictos de FIM

La característica FIM de Azure Security Center podría estar validando incorrectamente la integridad de los archivos de Linux. Compruebe que FIM está operativo y configurado correctamente para la supervisión de archivos de Linux. Consulte [Seguimiento de cambios en el entorno con la solución Change Tracking](../change-tracking.md).

## <a name="next-steps"></a>Pasos siguientes

Si su problema no aparece o no puede resolverlo, utilice uno de los canales siguientes para obtener ayuda adicional.

* Obtenga respuestas de expertos de Azure en los [foros de Azure](https://azure.microsoft.com/support/forums/).
* Póngase en contacto con [@AzureSupport](https://twitter.com/azuresupport): la cuenta de Microsoft Azure oficial para mejorar la experiencia del cliente, que pone en contacto a la comunidad de Azure con los recursos adecuados: respuestas, soporte técnico y expertos.
* Si necesita más ayuda, puede registrar un incidente de soporte técnico de Azure. Vaya al [sitio de soporte técnico de Azure](https://azure.microsoft.com/support/options/) y seleccione **Obtener soporte**.
