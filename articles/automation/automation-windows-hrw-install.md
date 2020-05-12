---
title: Hybrid Runbook Worker de Azure Automation en Windows
description: En este artículo encontrará información sobre cómo instalar una instancia de Hybrid Runbook Worker de Azure Automation para ejecutar runbooks en equipos con Windows en su centro de datos local o entorno de nube.
services: automation
ms.subservice: process-automation
ms.date: 12/10/2019
ms.topic: conceptual
ms.openlocfilehash: 163650a05bf47e6cb8a8832bb85477740d88b0cd
ms.sourcegitcommit: e0330ef620103256d39ca1426f09dd5bb39cd075
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/05/2020
ms.locfileid: "82787382"
---
# <a name="deploy-a-windows-hybrid-runbook-worker"></a>Implementación de Hybrid Runbook Worker en Windows

La característica Hybrid Runbook Worker de Azure Automation permite ejecutar runbooks directamente en el equipo que hospeda el rol y en los recursos del entorno para administrar dichos recursos locales. Azure Automation almacena y administra los runbooks y después los entrega a uno o más equipos designados. En este artículo, se describe cómo implementar Hybrid Runbook Worker en una máquina Windows.

Después de implementar correctamente un trabajo de runbook, revise la [ejecución de runbooks en Hybrid Runbook Worker](automation-hrw-run-runbooks.md) para más información sobre cómo configurar los runbooks para automatizar los procesos del centro de datos local o en otro entorno de nube.

[!INCLUDE [azure-monitor-log-analytics-rebrand](../../includes/azure-monitor-log-analytics-rebrand.md)]

>[!NOTE]
>Este artículo se ha actualizado para usar el nuevo módulo Az de Azure PowerShell. Aún puede usar el módulo de AzureRM que continuará recibiendo correcciones de errores hasta diciembre de 2020 como mínimo. Para más información acerca del nuevo módulo Az y la compatibilidad con AzureRM, consulte [Introducing the new Azure PowerShell Az module](https://docs.microsoft.com/powershell/azure/new-azureps-module-az?view=azps-3.5.0) (Presentación del nuevo módulo Az de Azure PowerShell). Para obtener instrucciones sobre la instalación del módulo Az en Hybrid Runbook Worker, consulte [Instalación del módulo de Azure PowerShell](https://docs.microsoft.com/powershell/azure/install-az-ps?view=azps-3.5.0). Puede actualizar los módulos de su cuenta de Automation a la versión más reciente mediante [Actualización de módulos de Azure PowerShell en Azure Automation](automation-update-azure-modules.md).

## <a name="windows-hybrid-runbook-worker-installation-and-configuration"></a>Instalación y configuración de Hybrid Runbook Worker en Windows

Para instalar y configurar una instancia de Hybrid Runbook Worker en Windows, puede usar uno de los métodos siguientes.

* En el caso de las máquinas virtuales de Azure, instale el agente de Log Analytics para Windows mediante la [extensión de máquina virtual para Windows](../virtual-machines/extensions/oms-windows.md). La extensión instala el agente de Log Analytics en Azure Virtual Machines e inscribe las máquinas virtuales en un área de trabajo de Log Analytics existente mediante una plantilla de Azure Resource Manager o PowerShell. Una vez instalado el agente, la máquina virtual se puede agregar a un grupo de Hybrid Runbook Worker de su cuenta de Automation. Consulte los pasos 3 y 4 de la sección [Implementación manual](#manual-deployment).

* Utilice un runbook de Automation a fin de automatizar completamente el proceso para configurar un equipo Windows. Este es el método recomendado para las máquinas de su centro de recursos u otro entorno en la nube.

* Siga un procedimiento paso a paso para instalar y configurar manualmente el rol de Hybrid Runbook Worker en una VM que no sea de Azure.

> [!NOTE]
> Para administrar la configuración de los servidores que admiten el rol de Hybrid Runbook Worker con Desired State Configuration (DSC), debe agregar dichos servidores como nodos de DSC.

### <a name="minimum-requirements-for-windows-hybrid-runbook-worker"></a>Requisitos mínimos de Hybrid Runbook Worker en Windows

Estos son los requisitos mínimos de Hybrid Runbook Worker en Windows:

* Windows Server 2012 o superior
* Windows PowerShell 5.1 o posterior ([descargar WMF 5.1](https://www.microsoft.com/download/details.aspx?id=54616)).
* .NET Framework 4.6.2, o posterior
* Dos núcleos
* 4 GB de RAM
* Puerto 443 (saliente)

### <a name="network-configuration"></a>Network configuration (Configuración de red)

Para obtener más requisitos de redes adicionales para Hybrid Runbook Worker, consulte el apartado sobre [configuración de la red](automation-hybrid-runbook-worker.md#network-planning).

### <a name="server-onboarding-for-management-with-state-configuration-dsc"></a>Incorporación de servidores para su administración con State Configuration (DSC)

Para obtener información sobre la incorporación de servidores para la administración con State Configuration (DSC), consulte [Incorporación de máquinas para administrarlas con State Configuration (DSC)](automation-dsc-onboarding.md).

Al habilitar [Update Management](automation-update-management.md), los equipos Windows conectados al área de trabajo de Log Analytics se configuran automáticamente como una instancia de Hybrid Runbook Worker para admitir actualizaciones del runbook. Sin embargo, este trabajo no se registra en ningún grupo de Hybrid Worker ya definido en la cuenta de Automation.

### <a name="addition-of-the-computer-to-a-hybrid-runbook-worker-group"></a>Incorporación del equipo a un grupo de Hybrid Runbook Worker

Puede agregar el equipo de trabajo a un grupo de Hybrid Runbook Worker en su cuenta de Automation. Tenga en cuenta que debe ser compatible con los runbooks de Automation mientras use la misma cuenta para la característica de Azure Automation y la pertenencia a grupos de Hybrid Runbook Worker. Esta funcionalidad se agregó a la versión 7.2.12024.0 de Hybrid Runbook Worker.

## <a name="automated-deployment"></a>Implementación automatizada

En la máquina de destino, realice los pasos siguientes para automatizar la instalación y configuración del rol Hybrid Worker en Windows.

### <a name="step-1---download-the-powershell-script"></a>Paso 1: Descarga del script de PowerShell

Descargue el script **New-OnPremiseHybridWorker.ps1** de la [Galería de PowerShell](https://www.powershellgallery.com/packages/New-OnPremiseHybridWorker). La descarga debe realizarse directamente desde el equipo que ejecuta el rol Hybrid Runbook Worker o desde otro equipo del entorno. Cuando haya descargado el script, cópielo a su equipo de trabajo. El script **New-OnPremiseHybridWorker.ps1** usa los parámetros descritos a continuación durante la ejecución.

| Parámetro | Status | Descripción |
| --------- | ------ | ----------- |
| `AAResourceGroupName` | Mandatory | el nombre del grupo de recursos que está asociado a su cuenta de Automation. |
| `AutomationAccountName` | Mandatory | el nombre de la cuenta de Automation.
| `Credential` | Opcional | credenciales que se usarán al iniciar sesión en el entorno de Azure. |
| `HybridGroupName` | Mandatory | el nombre de un grupo de Hybrid Runbook Worker que se especifica como destino para los runbooks que admiten este escenario. |
| `OMSResourceGroupName` | Opcional | el nombre del grupo de recursos para el área de trabajo de Log Analytics. Si no se especifica este grupo de recursos, se usa el valor de `AAResourceGroupName`. |
| `SubscriptionID` | Mandatory | identificador de la suscripción a Azure asociada a su cuenta de Automation. |
| `TenantID` | Opcional | identificador de la organización del inquilino asociado a su cuenta de Automation. |
| `WorkspaceName` | Opcional | Nombre del área de trabajo de Log Analytics. Si no tiene un área de trabajo de Log Analytics, el script creará y configurará una. |

> [!NOTE]
> Al habilitar las características, Azure Automation solo admite determinadas regiones para vincular un área de trabajo de Log Analytics y una cuenta de Automation. Para obtener una lista de los pares de asignación que se admiten, consulte [Asignación de región para la cuenta de Automation y el área de trabajo de Log Analytics](how-to/region-mappings.md).

### <a name="step-2---open-windows-powershell-command-line-shell"></a>Paso 2: Apertura del shell de la línea de comandos de Windows PowerShell

Abra **Windows PowerShell** desde la pantalla **Inicio** en modo de administrador.

### <a name="step-3---run-the-powershell-script"></a>Paso 3: Ejecución del script de PowerShell

En el shell de la línea de comandos de PowerShell, vaya a la carpeta que contiene el script que ha descargado. Cambie los valores de los parámetros `AutomationAccountName`, `AAResourceGroupName`, `OMSResourceGroupName`, `HybridGroupName`, `SubscriptionID` y `WorkspaceName`. Después, ejecute el script.

Se le solicitará que se autentique en Azure después de ejecutar el script. Debe iniciar sesión con una cuenta que sea miembro del rol Administradores de suscripciones y coadministrador de la suscripción.

```powershell-interactive
.\New-OnPremiseHybridWorker.ps1 -AutomationAccountName <NameofAutomationAccount> -AAResourceGroupName <NameofResourceGroup>`
-OMSResourceGroupName <NameofOResourceGroup> -HybridGroupName <NameofHRWGroup> `
-SubscriptionID <AzureSubscriptionId> -WorkspaceName <NameOfLogAnalyticsWorkspace>
```

### <a name="step-4---install-nuget"></a>Paso 4: Instalación de NuGet

Se le pedirá que acepte la instalación de NuGet y que se autentique con sus credenciales de Azure. Si no tiene la versión más reciente de NuGet, puede obtenerla en [Versiones de distribución de NuGet disponibles](https://www.nuget.org/downloads).

### <a name="step-5---verify-the-deployment"></a>Paso 5: Comprobación de la implementación

Una vez finalizado el script, la página Grupos de Hybrid Worker muestra el nuevo grupo y el número de miembros. Si se trata de un grupo existente, se incrementa el número de miembros. Puede seleccionar el grupo de la lista en la página Grupos de Hybrid Worker y elegir el icono **Hybrid Workers**. En la página Hybrid Workers, se mostrarán todos los miembros del grupo.

## <a name="manual-deployment"></a>Implementación manual

En el equipo de destino, realice los dos primeros pasos una vez para el entorno de Automation. A continuación, lleve a cabo los pasos restantes para cada equipo de trabajo.

### <a name="step-1---create-a-log-analytics-workspace"></a>Paso 1: Creación de un área de trabajo de Log Analytics

Si aún no tiene ningún área de trabajo de Log Analytics, revise la [guía de diseño de registros de Azure Monitor](../azure-monitor/platform/design-logs-deployment.md) antes de crear el área de trabajo.

### <a name="step-2---add-an-azure-automation-feature-to-the-log-analytics-workspace"></a>Paso 2: Incorporación de una característica de Azure Automation al área de trabajo de Log Analytics

Una característica de Automation agrega funcionalidad a Azure Automation, incluida la compatibilidad con la instancia de Hybrid Runbook Worker. Cuando se agrega una solución al área de trabajo de Log Analytics, se insertan automáticamente los componentes de trabajo al equipo del agente que instale como se describe en el paso siguiente.

Para agregar la solución Automation al área de trabajo, ejecute el cmdlet de PowerShell siguiente.

```powershell-interactive
Set-AzOperationalInsightsIntelligencePack -ResourceGroupName <logAnalyticsResourceGroup> -WorkspaceName <LogAnalyticsWorkspaceName> -IntelligencePackName "AzureAutomation" -Enabled $true -DefaultProfile <IAzureContextContainer>
```

### <a name="step-3---install-the-log-analytics-agent-for-windows"></a>Paso 3: Instalación del agente de Log Analytics para Windows

El agente de Log Analytics para Windows conecta los equipos a un área de trabajo de Log Analytics de Azure Monitor. Cuando instala el agente en el equipo y lo conecta al área de trabajo, se descargan automáticamente los componentes necesarios para la instancia de Hybrid Runbook Worker.

Para instalar el agente en el equipo, siga las instrucciones de [Conexión de equipos Windows a Azure Monitor](../log-analytics/log-analytics-windows-agent.md). Puede repetir este proceso para varios equipos para agregar varios trabajos a su entorno.

Una vez que el agente se ha conectado correctamente al área de trabajo de Log Analytics, transcurridos unos minutos, puede ejecutar la consulta siguiente para comprobar que envía datos de latido al área de trabajo.

```kusto
Heartbeat 
| where Category == "Direct Agent" 
| where TimeGenerated > ago(30m)
```

En los resultados de la búsqueda, verá los registros de latido del equipo que indican que está conectado y que está creando informes para el servicio. De forma predeterminada, cada agente reenvía un registro de latido a su área de trabajo asignada. 

Realice los siguientes pasos para completar la instalación y configuración del agente.

1. Habilite la solución para incorporar el equipo del agente. Vea [Incorporación de máquinas en el área de trabajo](https://docs.microsoft.com/azure/automation/automation-onboard-solutions-from-automation-account#onboard-machines-in-the-workspace).
2. Confirme que el agente ha descargado correctamente la solución de Automation. 
3. Para confirmar la versión de Hybrid Runbook Worker, vaya a **C:\Archivos de programa\Microsoft Monitoring Agent\Agent\AzureAutomation** y examine la subcarpeta **version**.

### <a name="step-4---install-the-runbook-environment-and-connect-to-azure-automation"></a>Paso 4: Instalación del entorno de runbook y conexión con Azure Automation

Cuando se configura un agente para que informe a un área de trabajo de Log Analytics, la solución de Automation inserta el módulo de PowerShell `HybridRegistration`, que contiene el cmdlet `Add-HybridRunbookWorker`. Este cmdlet se usa para instalar el entorno de runbook en el equipo y registrarlo en Azure Automation.

Abra una sesión de PowerShell en modo de Administrador y ejecute el comando siguientes para importar el módulo.

```powershell-interactive
cd "C:\Program Files\Microsoft Monitoring Agent\Agent\AzureAutomation\<version>\HybridRegistration"
Import-Module .\HybridRegistration.psd1
```

Ahora, ejecute el cmdlet `Add-HybridRunbookWorker` con la siguiente sintaxis.

```powershell-interactive
Add-HybridRunbookWorker –GroupName <String> -EndPoint <Url> -Token <String>
```

Puede obtener la información necesaria para este cmdlet en la página Administrar claves de Azure Portal. Para abrir esta página, seleccione **Claves** en la página Configuración de su cuenta de Automation.

![Página Administrar claves](media/automation-hybrid-runbook-worker/elements-panel-keys.png)

* En el parámetro `GroupName`, use el nombre del grupo de Hybrid Runbook Worker. Si este grupo ya existe en la cuenta de Automation, se le agregará el equipo actual. Si este grupo no existe, se agrega.
* En el parámetro `EndPoint`, use la entrada **Dirección URL** de la página Administrar claves.
* En el parámetro `Token`Token, use la entrada **CLAVE DE ACCESO PRINCIPAL** de la página Administrar claves.
* Si es necesario, establezca el parámetro `Verbose` para recibir detalles sobre la instalación.

### <a name="step-5----install-powershell-modules"></a>Paso 5: Instalación de módulos PowerShell

Los runbooks pueden usar cualquiera de las actividades y los cmdlets definidos en los módulos instalados en el entorno de Azure Automation. Dado que estos módulos no se implementan automáticamente en los equipos locales, debe instalarlos de forma manual. La excepción es el módulo de Azure. Este módulo se instala de manera predeterminada y ofrece acceso a los cmdlets para todos los servicios y actividades de Azure para Azure Automation.

Como el propósito principal de la instancia de Hybrid Runbook Worker es administrar recursos locales, es muy probable que deba instalar los módulos que admiten estos recursos, específicamente el módulo `PowerShellGet`. Para más información sobre cómo instalar módulos de Windows PowerShell, consulte [Windows PowerShell](https://docs.microsoft.com/powershell/scripting/developer/windows-powershell).

Los módulos instalados deben estar en una ubicación a la que haga referencia la variable de entorno `PSModulePath` para que Hybrid Worker los importe automáticamente. Para más información, consulte [Instalación de módulos en PSModulePath](https://docs.microsoft.com/powershell/scripting/developer/module/installing-a-powershell-module?view=powershell-7).

## <a name="remove-the-hybrid-runbook-worker-from-an-on-premises-windows-computer"></a><a name="remove-windows-hybrid-runbook-worker"></a>Eliminación de la instancia de Hybrid Runbook Worker de un equipo de Windows local

1. En Azure Portal, abra su cuenta de Automation.
2. En **Configuración de la cuenta**, seleccione **Claves** y anote los valores de **URL** y **Clave de acceso primaria**.

3. Abra una sesión de PowerShell en modo de administrador y ejecute el siguiente comando con los valores de la dirección URL y la clave de acceso principal. Utilice el parámetro `Verbose` para ver un registro detallado del proceso de eliminación. Para eliminar máquinas obsoletas del grupo Hybrid Worker, use el parámetro `machineName` opcional.

```powershell-interactive
Remove-HybridRunbookWorker -url <URL> -key <PrimaryAccessKey> -machineName <ComputerName>
```

## <a name="remove-a-hybrid-worker-group"></a>Eliminación de un grupo de Hybrid Worker

Para quitar un grupo de Hybrid Runbook Worker, primero debe quitar la instancia de Hybrid Runbook Worker de todos los equipos que sean miembros del grupo. Después, siga estos pasos para quitar el grupo:

1. Abra la cuenta de Automation en Azure Portal.
2. Seleccione **Grupos de Hybrid Worker** en **Automatización de procesos**. Seleccione el grupo que quiere eliminar. Aparece la página de propiedades de ese grupo.

   ![Página de propiedades](media/automation-hybrid-runbook-worker/automation-hybrid-runbook-worker-group-properties.png)

3. En la página de propiedades del grupo seleccionado, seleccione **Eliminar**. Un mensaje le solicita que confirme esta acción. Seleccione **Sí** si está seguro de que quiere continuar.

   ![Mensaje de confirmación](media/automation-hybrid-runbook-worker/automation-hybrid-runbook-worker-confirm-delete.png)

   Este proceso puede tardar varios segundos en finalizar. Puede realizar un seguimiento de su progreso en **Notificaciones** en el menú.

## <a name="next-steps"></a>Pasos siguientes

* Consulte [Ejecución de runbooks en Hybrid Runbook Worker](automation-hrw-run-runbooks.md) para más información sobre cómo configurar los runbooks para automatizar los procesos del centro de datos local o de otros entornos de nube.
* Para obtener más información sobre cómo solucionar problemas de las instancias de Hybrid Runbook Worker, consulte [Solución de problemas de instancias de Hybrid Runbook Worker](troubleshoot/hybrid-runbook-worker.md#windows).

