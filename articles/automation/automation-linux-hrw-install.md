---
title: Implementación de una instancia de Hybrid Runbook Worker en Azure Automation
description: En este artículo se describe cómo instalar Hybrid Runbook Worker de Azure Automation para ejecutar runbooks en equipos Linux en su centro de datos local o en su entorno de nube.
services: automation
ms.subservice: process-automation
ms.date: 03/02/2020
ms.topic: conceptual
ms.openlocfilehash: a6cf348142d694a03da24f32793fc72325701931
ms.sourcegitcommit: 0b80a5802343ea769a91f91a8cdbdf1b67a932d3
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/25/2020
ms.locfileid: "83835230"
---
# <a name="deploy-a-linux-hybrid-runbook-worker"></a>Implementación de Hybrid Runbook Worker en Linux

La característica Hybrid Runbook Worker de Azure Automation permite ejecutar runbooks directamente en el equipo que hospeda el rol y en los recursos del entorno para administrar dichos recursos locales. Hybrid Runbook Worker en Linux ejecuta runbooks como un usuario especial que puede tener permisos elevados para ejecutar comandos que necesitan permisos elevados. Los runbooks se almacenan y administran en Azure Automation y después se entregan a uno o más equipos designados.

En este artículo se describe cómo instalar la instancia de Hybrid Runbook Worker en una máquina Linux, cómo eliminar el trabajo y cómo eliminar un grupo de Hybrid Runbook Worker.

## <a name="supported-linux-operating-systems"></a>Sistemas operativos Linux compatibles

La característica Hybrid Runbook Worker admite las siguientes distribuciones:

* Amazon Linux 2012.09 a 2015.09 (x86/x64)
* CentOS Linux 5, 6 y 7 (x86/x64)
* Oracle Linux 5, 6 y 7 (x86/x64)
* Red Hat Enterprise Linux Server 5, 6 y 7 (x86/x64)
* Debian GNU/Linux 6, 7 y 8 (x86/x64)
* Ubuntu 12.04 LTS, 14.04 LTS, 16.04 LTS y 18.04 (x86/x64)
* SUSE Linux Enterprise Server 11 y 12 (x86/x64)

## <a name="supported-runbook-types"></a>Tipos de runbook admitidos

Las instancias de Hybrid Runbook Worker en Linux no admiten el conjunto completo de tipos de runbook de Azure Automation.

Los siguientes tipos de runbook funcionan en instancias de Hybrid Worker Linux:

* Python 2
* PowerShell

  > [!NOTE]
  > Los runbooks de PowerShell requieren que PowerShell Core esté instalado en el equipo Linux. Consulte [Instalación de PowerShell Core en Linux](/powershell/scripting/install/installing-powershell-core-on-linux) para obtener información sobre cómo instalarlo.

Los siguientes tipos de runbook no funcionan en instancias de Hybrid Worker en Linux:

* Flujo de trabajo de PowerShell
* Gráfico
* Flujo de trabajo gráfico de PowerShell

## <a name="deployment-requirements"></a>Requisitos de implementación

Estos son los requisitos mínimos de Hybrid Runbook Worker en Linux:

* Dos núcleos
* 4 GB de RAM
* Puerto 443 (saliente)

### <a name="package-requirements"></a>Requisitos de paquete

| **Paquetes necesarios** | **Descripción** | **Versión mínima**|
|--------------------- | --------------------- | -------------------|
|Glibc |Biblioteca GNU C| 2.5-12 |
|Openssl| Bibliotecas OpenSSL | 1.0 (se admiten TLS 1.1 y TLS 1.2)|
|Curl | Cliente web de cURL | 7.15.5|
|Python ctypes | Python 2.x es obligatorio |
|PAM | Módulos de autenticación conectables|
| **Paquete opcional** | **Descripción** | **Versión mínima**|
| PowerShell Core | Para ejecutar runbooks de PowerShell, PowerShell debe estar instalado; consulte [Instalación de PowerShell Core en Linux](/powershell/scripting/install/installing-powershell-core-on-linux) para más información sobre cómo instalarlo.  | 6.0.0 |

## <a name="install-a-linux-hybrid-runbook-worker"></a>Instalación de Hybrid Runbook Worker en Linux

Para instalar y configurar una instancia de Hybrid Runbook Worker en un equipo Linux, siga un procedimiento sencillo manual. Es preciso habilitar la instancia de Hybrid Runbook Worker en el área de trabajo de Log Analytics y, después, ejecutar un conjunto de comandos para registrar el equipo como un rol de trabajo y agregarlo a un grupo.

Antes de continuar, anote el área de trabajo de Log Analytics que está vinculado a su cuenta de Automation. Anote también la clave principal de la cuenta de Automation. Encontrará ambos datos en Azure Portal: seleccione la cuenta de Automation y, para el identificador de área de trabajo, seleccione **Área de trabajo** y, para la clave principal, seleccione **Claves**. Para más información sobre los puertos y las direcciones que se necesitan para Hybrid Runbook Worker, consulte [Configuración de la red](automation-hybrid-runbook-worker.md#network-planning).

>[!NOTE]
> La [cuenta nxautomation](automation-runbook-execution.md#log-analytics-agent-for-linux) con los permisos sudo correspondientes debe estar presente durante la instalación de la instancia de Hybrid Worker en Linux. Si intenta instalar el rol de trabajo y la cuenta no está presente o no tiene los permisos adecuados, no se realizará la instalación.

1. Habilite la instancia de Hybrid Runbook Worker en Azure con uno de los métodos siguientes:

   * Agregue la solución Hybrid Runbook Worker a la suscripción con el procedimiento que se describe en el artículo sobre la [adición de registros de Azure Monitor al área de trabajo](../log-analytics/log-analytics-add-solutions.md).
   * Ejecute el siguiente cmdlet:

        ```azurepowershell-interactive
         Set-AzOperationalInsightsIntelligencePack -ResourceGroupName  <ResourceGroupName> -WorkspaceName <WorkspaceName> -IntelligencePackName  "AzureAutomation" -Enabled $true
        ```

1. Instale el agente de Log Analytics para Linux con el comando siguiente. Reemplace \<WorkspaceID\> y \<WorkspaceKey\> por los valores correspondientes a su área de trabajo.

   [!INCLUDE [log-analytics-agent-note](../../includes/log-analytics-agent-note.md)]

   ```bash
   wget https://raw.githubusercontent.com/Microsoft/OMS-Agent-for-Linux/master/installer/scripts/onboard_agent.sh && sh onboard_agent.sh -w <WorkspaceID> -s <WorkspaceKey>
   ```

1. Ejecute el siguiente comando y cambie los valores de los parámetros *-w*, *-k*, *-g* y *-e*. Para el parámetro *-g*, reemplace el valor con el nombre del grupo de Hybrid Runbook Worker al que se debe unir el nuevo Hybrid Runbook Worker de Linux. Si el nombre no existe en su cuenta de Automation, se crea un nuevo grupo de Hybrid Runbook Worker con ese nombre.

   ```bash
   sudo python /opt/microsoft/omsconfig/modules/nxOMSAutomationWorker/DSCResources/MSFT_nxOMSAutomationWorkerResource/automationworker/scripts/onboarding.py --register -w <LogAnalyticsworkspaceId> -k <AutomationSharedKey> -g <hybridgroupname> -e <automationendpoint>
   ```

1. Una vez completado el comando, la página Grupos de Hybrid Worker de Azure Portal muestra el nuevo grupo y el número de miembros. Si se trata de un grupo existente, se incrementa el número de miembros. Puede seleccionar el grupo de la lista en la página Grupos de Hybrid Worker y seleccionar el icono **Hybrid Workers**. En la página Hybrid Workers, verá que aparece cada miembro del grupo.

> [!NOTE]
> Si usa la extensión de máquina virtual de Azure Monitor para una máquina virtual de Azure para Linux, se recomienda establecer `autoUpgradeMinorVersion` en false porque las versiones de actualización automática pueden provocar problemas en Hybrid Runbook Worker. Para obtener información sobre cómo actualizar manualmente la extensión, vea [Implementación de la CLI de Azure](../virtual-machines/extensions/oms-linux.md#azure-cli-deployment).

## <a name="turn-off-signature-validation"></a>Desactivación de la validación de firma

De forma predeterminada, las instancias de Hybrid Runbook Worker de Linux requieren la validación de la firma. Si ejecuta un runbook sin firmar en un trabajo, verá un error `Signature validation failed`. Para desactivar la validación de la firma, ejecute el siguiente comando. Reemplace el segundo parámetro por el identificador de área de trabajo de Log Analytics.

 ```bash
 sudo python /opt/microsoft/omsconfig/modules/nxOMSAutomationWorker/DSCResources/MSFT_nxOMSAutomationWorkerResource/automationworker/scripts/require_runbook_signature.py --false <LogAnalyticsworkspaceId>
 ```

## <a name="remove-the-hybrid-runbook-worker-from-an-on-premises-linux-computer"></a><a name="remove-linux-hybrid-runbook-worker"></a>Eliminación de la instancia de Hybrid Runbook Worker de un equipo de Linux local

Puede usar el comando `ls /var/opt/microsoft/omsagent` en Hybrid Runbook Worker para obtener el identificador del área de trabajo. Se creará una carpeta cuyo nombre será el identificador del área de trabajo.

```bash
sudo python onboarding.py --deregister --endpoint="<URL>" --key="<PrimaryAccessKey>" --groupname="Example" --workspaceid="<workspaceId>"
```

> [!NOTE]
> Este código no quita el agente de Log Analytics para Linux del equipo. Solo quita la funcionalidad y la configuración del rol de Hybrid Runbook Worker.

## <a name="remove-a-hybrid-worker-group"></a>Eliminación de un grupo de Hybrid Worker

Para eliminar un grupo de Hybrid Runbook Worker de equipos de Linux, use los mismos pasos que para un grupo de Hybrid Worker de Windows. Consulte [Eliminación de un grupo de Hybrid Worker](automation-windows-hrw-install.md#remove-a-hybrid-worker-group).

## <a name="next-steps"></a>Pasos siguientes

* Consulte [Ejecución de runbooks en Hybrid Runbook Worker](automation-hrw-run-runbooks.md) para más información sobre cómo configurar los runbooks para automatizar los procesos del centro de datos local o de otros entornos de nube.
* Para aprender a solucionar problemas de las instancias de Hybrid Runbook Worker, consulte [Solución de problemas de Hybrid Runbook Worker: Linux](troubleshoot/hybrid-runbook-worker.md#linux).
