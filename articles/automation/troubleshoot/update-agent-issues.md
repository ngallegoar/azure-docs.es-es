---
title: 'Diagnóstico de Hybrid Runbook Worker de Windows: Azure Update Management'
description: Aprenda a solucionar problemas con Azure Automation Hybrid Runbook Worker en Windows que admite Update Management.
services: automation
author: mgoedtel
ms.author: magoedte
ms.date: 01/16/2020
ms.topic: conceptual
ms.service: automation
ms.subservice: update-management
manager: carmonm
ms.openlocfilehash: ec35d11eba59ea21947e2c3cd5286bababa4eabb
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "76153861"
---
# <a name="understand-and-resolve-windows-hybrid-runbook-worker-health-in-update-management"></a>Entienda y solucione el estado de Windows Hybrid Runbook Worker de Windows en Update Management.

Puede haber muchas razones por las que el equipo no muestre el estado **Listo** en Update Management. En Update Management, puede comprobar el estado de un agente de Hybrid Runbook Worker para determinar el problema subyacente. En este artículo se explica cómo ejecutar el solucionador de problemas para máquinas de Azure desde Azure Portal y para máquinas que no son de Azure en el [escenario sin conexión](#troubleshoot-offline).

La siguiente lista enumera los tres estados de preparación en los que puede estar una máquina:

* **Listo**: Hybrid Runbook Worker está implementado y se vio por última vez hace menos de 1 hora.
* **Desconectado**: Hybrid Runbook Worker está implementado y se vio por última vez hace más de 1 hora.
* **No configurado**: Hybrid Runbook Worker no se encuentra o no ha finalizado la incorporación.

> [!NOTE]
> Puede haber un ligero retraso entre lo que Azure Portal muestra y el estado actual de la máquina.

## <a name="start-the-troubleshooter"></a>Iniciar el solucionador de problemas

Para las máquinas de Azure, al hacer clic en el vínculo **Solucionar problemas** en la columna **Preparación de actualizaciones del agente** del portal, se abre la página **Solucionar problemas del Agente de actualización**. Para las máquinas que no son de Azure, el vínculo le lleva a este artículo. Consulte las [instrucciones sin conexión](#troubleshoot-offline) para solucionar los problemas de una máquina que no es de Azure.

![Actualizar la lista de administración de máquinas virtuales](../media/update-agent-issues/vm-list.png)

> [!NOTE]
> Para comprobar el estado de Hybrid Runbook Worker, la máquina virtual debe estar en ejecución. Si la máquina virtual no se está ejecutando, aparecerá el botón **Start the VM** (Iniciar la máquina virtual).

En la página **Solucionar problemas del Agente de actualización**, haga clic en **Ejecutar comprobaciones** para iniciar el solucionador de problemas. El solucionador de problemas usa [Ejecutar comando](../../virtual-machines/windows/run-command.md) para ejecutar un script en la máquina y verificar las dependencias. Una vez completado el proceso del solucionador de problemas, devuelve el resultado de las comprobaciones.

![Página de solución de problemas del Agente de actualización](../media/update-agent-issues/troubleshoot-page.png)

Los resultados se muestran en la página cuando haya terminado el proceso. Las secciones de las comprobaciones muestran lo que está incluido en cada una de ellas.

![Comprobaciones de la solución de problemas del Agente de actualización](../media/update-agent-issues/update-agent-checks.png)

## <a name="prerequisite-checks"></a>Comprobaciones de requisitos previos

### <a name="operating-system"></a>Sistema operativo

La comprobación del sistema operativo determina si Hybrid Runbook Worker está ejecutando alguno de los siguientes sistemas operativos:

|Sistema operativo  |Notas  |
|---------|---------|
|Windows Server 2012 y posteriores |Se requiere .NET Framework 4.6 o posterior. ([Descargar .NET Framework](/dotnet/framework/install/guide-for-developers)).<br/> Se requiere Windows PowerShell 5.1.  ([Descargar Windows Management Framework 5.1](https://www.microsoft.com/download/details.aspx?id=54616)).        |

### <a name="net-462"></a>.NET 4.6.2

La comprobación de .NET Framework verifica si el sistema tiene instalada la versión mínima de [.NET Framework 4.6.2](https://www.microsoft.com/en-us/download/details.aspx?id=53345).

### <a name="wmf-51"></a>WMF 5.1

La comprobación de WMF verifica si el sistema tiene la versión requerida de Windows Management Framework (WMF), [Windows Management Fotograma 5.1](https://www.microsoft.com/download/details.aspx?id=54616).

### <a name="tls-12"></a>TLS 1.2

Esta comprobación determina si usa TLS 1.2 para cifrar las comunicaciones. TLS 1.0 ya no es compatible con la plataforma. Se recomienda que los clientes usen TLS 1.2 para comunicarse con Update Management.

## <a name="connectivity-checks"></a>Comprobaciones de conectividad

### <a name="registration-endpoint"></a>Punto de conexión del registro

Esta comprobación determina si el agente puede comunicarse correctamente con el servicio del agente.

Las configuraciones de proxy y firewall deben permitir que el agente de Hybrid Runbook Worker se comunique con el punto de conexión de registro. Para obtener una lista de direcciones y puertos que deben abrirse, vea [Network planning for Hybrid Workers](../automation-hybrid-runbook-worker.md#network-planning) (Planificación de red para instancias de Hybrid Worker).

### <a name="operations-endpoint"></a>Punto de conexión de las operaciones

Esta comprobación determina si el agente puede comunicarse correctamente con el servicio de datos en tiempo de ejecución del trabajo.

Las configuraciones de proxy y firewall deben permitir que el agente de Hybrid Runbook Worker se comunique con el servicio de datos en tiempo de ejecución del trabajo. Para obtener una lista de direcciones y puertos que deben abrirse, vea [Network planning for Hybrid Workers](../automation-hybrid-runbook-worker.md#network-planning) (Planificación de red para instancias de Hybrid Worker).

## <a name="vm-service-health-checks"></a>Comprobaciones del estado de servicio de las máquinas virtuales

### <a name="monitoring-agent-service-status"></a>Supervisión del estado de servicio del agente

Esta comprobación determina si Microsoft Monitoring Agent (`HealthService`) se ejecuta en la máquina.

Para más información sobre cómo solucionar problemas del servicio, vea [No se está ejecutando Microsoft Monitoring Agent](hybrid-runbook-worker.md#mma-not-running).

Para volver a instalar Microsoft Monitoring Agent, consulte [Instalación y configuración de Microsoft Monitoring Agent](../../azure-monitor/learn/quick-collect-windows-computer.md#install-the-agent-for-windows).

### <a name="monitoring-agent-service-events"></a>Supervisión de eventos de servicio del agente

Esta comprobación determina si se ha producido algún evento `4502` en el registro de Azure Operations Manager de la maquina durante las veinticuatro últimas horas.

Para más información sobre este evento, vea la [guía de solución de problemas](hybrid-runbook-worker.md#event-4502) para este evento.

## <a name="access-permissions-checks"></a>Comprobaciones de permisos de acceso

### <a name="machinekeys-folder-access"></a>Acceso a la carpeta MachineKeys

La comprobación del acceso a la carpeta Crypto determina si la cuenta del sistema local tiene acceso a C:\ProgramData\Microsoft\Crypto\RSA.

## <a name="troubleshoot-offline"></a><a name="troubleshoot-offline"></a>Solución de problemas sin conexión

Puede usar el solucionador de problemas en una instancia sin conexión de Hybrid Runbook Worker si ejecuta el script de forma local. El script [Troubleshoot-WindowsUpdateAgentRegistration](https://www.powershellgallery.com/packages/Troubleshoot-WindowsUpdateAgentRegistration) puede encontrarse en la Galería de PowerShell. Debe tener WMF 4-0 o superior instalado para ejecutar el script. Para descargar la versión más reciente de PowerShell, consulte [Instalación de varias versiones de PowerShell](https://docs.microsoft.com/powershell/scripting/install/installing-powershell).

El resultado de este script tendrá un aspecto similar al del siguiente ejemplo:

```output
RuleId                      : OperatingSystemCheck
RuleGroupId                 : prerequisites
RuleName                    : Operating System
RuleGroupName               : Prerequisite Checks
RuleDescription             : The Windows Operating system must be version 6.2.9200 (Windows Server 2012) or higher
CheckResult                 : Passed
CheckResultMessage          : Operating System version is supported
CheckResultMessageId        : OperatingSystemCheck.Passed
CheckResultMessageArguments : {}

RuleId                      : DotNetFrameworkInstalledCheck
RuleGroupId                 : prerequisites
RuleName                    : .NET Framework 4.5+
RuleGroupName               : Prerequisite Checks
RuleDescription             : .NET Framework version 4.5 or higher is required
CheckResult                 : Passed
CheckResultMessage          : .NET Framework version 4.5+ is found
CheckResultMessageId        : DotNetFrameworkInstalledCheck.Passed
CheckResultMessageArguments : {}

RuleId                      : WindowsManagementFrameworkInstalledCheck
RuleGroupId                 : prerequisites
RuleName                    : WMF 5.1
RuleGroupName               : Prerequisite Checks
RuleDescription             : Windows Management Framework version 4.0 or higher is required (version 5.1 or higher is preferable)
CheckResult                 : Passed
CheckResultMessage          : Detected Windows Management Framework version: 5.1.17763.1
CheckResultMessageId        : WindowsManagementFrameworkInstalledCheck.Passed
CheckResultMessageArguments : {5.1.17763.1}

RuleId                      : AutomationAgentServiceConnectivityCheck1
RuleGroupId                 : connectivity
RuleName                    : Registration endpoint
RuleGroupName               : connectivity
RuleDescription             :
CheckResult                 : Failed
CheckResultMessage          : Unable to find Workspace registration information in registry
CheckResultMessageId        : AutomationAgentServiceConnectivityCheck1.Failed.NoRegistrationFound
CheckResultMessageArguments : {}

RuleId                      : AutomationJobRuntimeDataServiceConnectivityCheck
RuleGroupId                 : connectivity
RuleName                    : Operations endpoint
RuleGroupName               : connectivity
RuleDescription             : Proxy and firewall configuration must allow Automation Hybrid Worker agent to communicate with eus2-jobruntimedata-prod-su1.azure-automation.net
CheckResult                 : Passed
CheckResultMessage          : TCP Test for eus2-jobruntimedata-prod-su1.azure-automation.net (port 443) succeeded
CheckResultMessageId        : AutomationJobRuntimeDataServiceConnectivityCheck.Passed
CheckResultMessageArguments : {eus2-jobruntimedata-prod-su1.azure-automation.net}

RuleId                      : MonitoringAgentServiceRunningCheck
RuleGroupId                 : servicehealth
RuleName                    : Monitoring Agent service status
RuleGroupName               : VM Service Health Checks
RuleDescription             : HealthService must be running on the machine
CheckResult                 : Failed
CheckResultMessage          : Microsoft Monitoring Agent service (HealthService) is not running
CheckResultMessageId        : MonitoringAgentServiceRunningCheck.Failed
CheckResultMessageArguments : {Microsoft Monitoring Agent, HealthService}

RuleId                      : MonitoringAgentServiceEventsCheck
RuleGroupId                 : servicehealth
RuleName                    : Monitoring Agent service events
RuleGroupName               : VM Service Health Checks
RuleDescription             : Event Log must not have event 4502 logged in the past 24 hours
CheckResult                 : Failed
CheckResultMessage          : Microsoft Monitoring Agent service Event Log (Operations Manager) does not exist on the machine
CheckResultMessageId        : MonitoringAgentServiceEventsCheck.Failed.NoLog
CheckResultMessageArguments : {Microsoft Monitoring Agent, Operations Manager, 4502}

RuleId                      : CryptoRsaMachineKeysFolderAccessCheck
RuleGroupId                 : permissions
RuleName                    : Crypto RSA MachineKeys Folder Access
RuleGroupName               : Access Permission Checks
RuleDescription             : SYSTEM account must have WRITE and MODIFY access to 'C:\\ProgramData\\Microsoft\\Crypto\\RSA\\MachineKeys'
CheckResult                 : Passed
CheckResultMessage          : Have permissions to access C:\\ProgramData\\Microsoft\\Crypto\\RSA\\MachineKeys
CheckResultMessageId        : CryptoRsaMachineKeysFolderAccessCheck.Passed
CheckResultMessageArguments : {C:\\ProgramData\\Microsoft\\Crypto\\RSA\\MachineKeys}

RuleId                      : TlsVersionCheck
RuleGroupId                 : prerequisites
RuleName                    : TLS 1.2
RuleGroupName               : Prerequisite Checks
RuleDescription             : Client and Server connections must support TLS 1.2
CheckResult                 : Passed
CheckResultMessage          : TLS 1.2 is enabled by default on the Operating System.
CheckResultMessageId        : TlsVersionCheck.Passed.EnabledByDefault
CheckResultMessageArguments : {}
```

## <a name="next-steps"></a>Pasos siguientes

Para solucionar problemas adicionales con las instancias de Hybrid Runbook Worker, consulte [Solución de problemas de Hybrid Runbook Worker](hybrid-runbook-worker.md).
