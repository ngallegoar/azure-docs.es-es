---
title: Habilitación de Azure Monitor para VM con PowerShell o plantillas
description: En este artículo se describe cómo habilitar Azure Monitor para VM para una o más máquinas virtuales o conjuntos de escalado de máquinas virtuales de Azure mediante Azure PowerShell o plantillas de Azure Resource Manager.
ms.subservice: ''
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 10/14/2019
ms.openlocfilehash: 021e5e8ee01745dd0b534e53dbb6d75b5b78c419
ms.sourcegitcommit: dccb85aed33d9251048024faf7ef23c94d695145
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/28/2020
ms.locfileid: "87298372"
---
# <a name="enable-azure-monitor-for-vms-using-azure-powershell-or-resource-manager-templates"></a>Habilitación de Azure Monitor para VM mediante plantillas de Resource Manager o Azure PowerShell

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

En este artículo se explica cómo habilitar Azure Monitor para VM para conjuntos de escalado de máquinas virtuales o máquinas virtuales de Azure, mediante plantillas de Azure Resource Manager o Azure PowerShell. Al final de este proceso, habrá empezado a supervisar todas sus máquinas virtuales y a aprender si alguna está experimentando problemas de rendimiento o disponibilidad.

## <a name="set-up-a-log-analytics-workspace"></a>Configure un área de trabajo de Log Analytics

Si no tiene un área de trabajo de Log Analytics deberá crear una. Revise los métodos que se sugieren en la sección [Requisitos previos](vminsights-enable-overview.md#log-analytics) antes de continuar con los pasos para configurarla. Después puede finalizar la implementación de Azure Monitor para VM mediante el método de plantillas de Azure Resource Manager.

### <a name="install-the-vminsights-solution"></a>Instalación de la solución VMInsights

Este método incluye una plantilla JSON que especifica la configuración para habilitar los componentes de la solución en el área de trabajo de Log Analytics.

Si no sabe cómo implementar los recursos mediante una plantilla, consulte:
* [Implementación de recursos con las plantillas de Resource Manager y Azure PowerShell](../../azure-resource-manager/templates/deploy-powershell.md)
* [Implementación de recursos con plantillas de Resource Manager y la CLI de Azure](../../azure-resource-manager/templates/deploy-cli.md)

Para usar la CLI de Azure, primero debe instalar y usar la CLI localmente. Debe ejecuta la versión 2.0.27 de la CLI de Azure, o cualquier versión posterior. Para identificar la versión, ejecute `az --version`. Para instalar o actualizar la CLI de Azure, consulte [Instalación de la CLI de Azure](/cli/azure/install-azure-cli).

1. Copie y pegue la siguiente sintaxis JSON en el archivo:

    ```json
    {
        "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "contentVersion": "1.0.0.0",
        "parameters": {
            "WorkspaceName": {
                "type": "string"
            },
            "WorkspaceLocation": {
                "type": "string"
            }
        },
        "resources": [
            {
                "apiVersion": "2017-03-15-preview",
                "type": "Microsoft.OperationalInsights/workspaces",
                "name": "[parameters('WorkspaceName')]",
                "location": "[parameters('WorkspaceLocation')]",
                "resources": [
                    {
                        "apiVersion": "2015-11-01-preview",
                        "location": "[parameters('WorkspaceLocation')]",
                        "name": "[concat('VMInsights', '(', parameters('WorkspaceName'),')')]",
                        "type": "Microsoft.OperationsManagement/solutions",
                        "dependsOn": [
                            "[concat('Microsoft.OperationalInsights/workspaces/', parameters('WorkspaceName'))]"
                        ],
                        "properties": {
                            "workspaceResourceId": "[resourceId('Microsoft.OperationalInsights/workspaces/', parameters('WorkspaceName'))]"
                        },

                        "plan": {
                            "name": "[concat('VMInsights', '(', parameters('WorkspaceName'),')')]",
                            "publisher": "Microsoft",
                            "product": "[Concat('OMSGallery/', 'VMInsights')]",
                            "promotionCode": ""
                        }
                    }
                ]
            }
        ]
    }
    ```

1. Guarde este archivo como *installsolutionsforvminsights.json* en una carpeta local.

1. Capture los valores de *WorkspaceName*, *ResourceGroupName* y *WorkspaceLocation*. El valor de *WorkspaceName* es el nombre del área de trabajo de Log Analytics. El valor de *WorkspaceLocation* es la región en la que el área de trabajo está definida.

1. Ya está listo para implementar esta plantilla.

    * Use los siguientes comandos de PowerShell en la carpeta que contenga la plantilla:

        ```powershell
        New-AzResourceGroupDeployment -Name DeploySolutions -TemplateFile InstallSolutionsForVMInsights.json -ResourceGroupName <ResourceGroupName> -WorkspaceName <WorkspaceName> -WorkspaceLocation <WorkspaceLocation - example: eastus>
        ```

        El cambio de configuración puede tardar unos minutos en finalizar. Cuando finalice, se muestra un mensaje que incluye el resultado y que es similar al siguiente:

        ```output
        provisioningState       : Succeeded
        ```

    * Para ejecutar el comando siguiente mediante la CLI de Azure:

        ```azurecli
        az login
        az account set --subscription "Subscription Name"
        az group deployment create --name DeploySolutions --resource-group <ResourceGroupName> --template-file InstallSolutionsForVMInsights.json --parameters WorkspaceName=<workspaceName> WorkspaceLocation=<WorkspaceLocation - example: eastus>
        ```

        El cambio de configuración puede tardar unos minutos en finalizar. Cuando finalice, se muestra un mensaje que incluye el resultado y que es similar al siguiente:

        ```output
        provisioningState       : Succeeded
        ```

## <a name="enable-with-azure-resource-manager-templates"></a>Habilitar las plantillas de Azure Resource Manager

Hemos creado plantillas de Azure Resource Manager de ejemplo para incorporar sus máquinas virtuales y conjuntos de escalado de máquinas virtuales. Estas plantillas incluyen escenarios que puede usar para habilitar la supervisión de un recurso existente y para crear un nuevo recurso que tenga la supervisión habilitada.

>[!NOTE]
>La plantilla debe implementarse en el mismo grupo de recursos que el recurso que se incorporará.

Si no sabe cómo implementar los recursos mediante una plantilla, consulte:
* [Implementación de recursos con las plantillas de Resource Manager y Azure PowerShell](../../azure-resource-manager/templates/deploy-powershell.md)
* [Implementación de recursos con plantillas de Resource Manager y la CLI de Azure](../../azure-resource-manager/templates/deploy-cli.md)

Para usar la CLI de Azure, primero debe instalar y usar la CLI localmente. Debe ejecuta la versión 2.0.27 de la CLI de Azure, o cualquier versión posterior. Para identificar la versión, ejecute `az --version`. Para instalar o actualizar la CLI de Azure, consulte [Instalación de la CLI de Azure](/cli/azure/install-azure-cli).

### <a name="download-templates"></a>Descargar las plantillas

Las plantillas de Azure Resource Manager se suministran en un archivo de almacenamiento (.zip) que puede [descargar](https://aka.ms/VmInsightsARMTemplates) desde nuestro repositorio de GitHub. El contenido del archivo incluye carpetas que representan cada escenario de implementación con un archivo de plantilla y un archivo de parámetros. Antes de ejecutarlos, modifique el archivo de parámetros y especifique los valores necesarios. No modifique el archivo de plantilla a menos que necesite personalizarlo para que admita unos requisitos determinados. Tras modificar el archivo de parámetros, puede implementarlo mediante los métodos que se describen más adelante en este artículo.

El archivo de descarga contiene las siguientes plantillas para distintos escenarios:

- La plantilla **ExistingVmOnboarding** habilita Azure Monitor para VM si la máquina virtual ya existe.
- La plantilla **NewVmOnboarding** crea una máquina virtual y habilita Azure Monitor para VM para que la supervise.
- La plantilla **ExistingVmssOnboarding** habilita Azure Monitor para VM si el conjunto de escalado de máquinas virtuales ya existe.
- La plantilla **NewVmssOnboarding** crea conjuntos de escalado de máquinas virtuales y habilita Azure Monitor para VM para que lo supervise.
- La plantilla **ConfigureWorkspace** configura el área de trabajo de Log Analytics para que admita Azure Monitor para VM habilitando las soluciones y la colección de contadores de rendimiento de los sistemas operativos Linux y Windows.

>[!NOTE]
>Si ya había conjuntos de escalado de máquinas virtuales y la directiva de actualización está establecida en **Manual**, Azure Monitor para VM no se habilitará para las instancias de manera predeterminada tras ejecutar la plantilla de Azure Resource Manager **ExistingVmssOnboarding**. Tendrá que actualizar manualmente las instancias.

### <a name="deploy-by-using-azure-powershell"></a>Implementación mediante Azure PowerShell

El paso siguiente habilita la supervisión mediante Azure PowerShell.

```powershell
New-AzResourceGroupDeployment -Name OnboardCluster -ResourceGroupName <ResourceGroupName> -TemplateFile <Template.json> -TemplateParameterFile <Parameters.json>
```
El cambio de configuración puede tardar unos minutos en finalizar. Cuando finalice, se muestra un mensaje que incluye el resultado y que es similar al siguiente:

```output
provisioningState       : Succeeded
```

### <a name="deploy-by-using-the-azure-cli"></a>Implementación mediante la CLI de Azure

El paso siguiente habilita la supervisión mediante la CLI de Azure.

```azurecli
az login
az account set --subscription "Subscription Name"
az group deployment create --resource-group <ResourceGroupName> --template-file <Template.json> --parameters <Parameters.json>
```

La salida debe ser similar a la siguiente:

```output
provisioningState       : Succeeded
```

## <a name="enable-with-powershell"></a>Habilitar con PowerShell

Para habilitar Azure Monitor para VM en varias máquinas virtuales o conjuntos de escalado de máquinas virtuales, use el script de PowerShell [Install-VMInsights.ps1](https://www.powershellgallery.com/packages/Install-VMInsights). Está disponible en la Galería de Azure PowerShell. Este script procesa una iteración en:

- Cada máquina virtual y conjunto de escalado de máquinas virtuales de su suscripción.
- El grupo de recursos con ámbito especificado por *ResourceGroup*.
- Una única VM o conjunto de escalado de máquinas virtuales que se especifica mediante *Name*.

En cada máquina virtual o conjunto de escalado de máquinas virtuales, el script comprueba si la extensión de la máquina virtual ya está instalada. Si lo está, el script vuelve a intentar instalarla. Si la extensión de máquina virtual no está instalada, el script instala las extensiones de máquina virtual del agente de Log Analytics y de Dependency Agent.

Compruebe que usa la versión Az 1.0.0 o posterior del módulo de Azure PowerShell con los alias de compatibilidad `Enable-AzureRM` habilitados. Ejecute `Get-Module -ListAvailable Az` para encontrar la versión. Si necesita actualizarla, consulte [Instalación del módulo de Azure PowerShell](/powershell/azure/install-az-ps). Si PowerShell se ejecuta localmente, también debe ejecutar `Connect-AzAccount` para crear una conexión con Azure.

Para obtener una lista de los detalles de los argumentos del script y un ejemplo de uso, ejecute `Get-Help`.

```powershell
Get-Help .\Install-VMInsights.ps1 -Detailed

SYNOPSIS
    This script installs VM extensions for Log Analytics and the Dependency agent as needed for VM Insights.


SYNTAX
    .\Install-VMInsights.ps1 [-WorkspaceId] <String> [-WorkspaceKey] <String> [-SubscriptionId] <String> [[-ResourceGroup]
    <String>] [[-Name] <String>] [[-PolicyAssignmentName] <String>] [-ReInstall] [-TriggerVmssManualVMUpdate] [-Approve] [-WorkspaceRegion] <String>
    [-WhatIf] [-Confirm] [<CommonParameters>]


DESCRIPTION
    This script installs or reconfigures the following on VMs and virtual machine scale sets:
    - Log Analytics VM extension configured to supplied Log Analytics workspace
    - Dependency agent VM extension

    Can be applied to:
    - Subscription
    - Resource group in a subscription
    - Specific VM or virtual machine scale set
    - Compliance results of a policy for a VM or VM extension

    Script will show you a list of VMs or virtual machine scale sets that will apply to and let you confirm to continue.
    Use -Approve switch to run without prompting, if all required parameters are provided.

    If the extensions are already installed, they will not install again.
    Use -ReInstall switch if you need to, for example, update the workspace.

    Use -WhatIf if you want to see what would happen in terms of installs, what workspace configured to, and status of the extension.


PARAMETERS
    -WorkspaceId <String>
        Log Analytics WorkspaceID (GUID) for the data to be sent to

    -WorkspaceKey <String>
        Log Analytics Workspace primary or secondary key

    -SubscriptionId <String>
        SubscriptionId for the VMs/VM Scale Sets
        If using PolicyAssignmentName parameter, subscription that VMs are in

    -ResourceGroup <String>
        <Optional> Resource Group to which the VMs or VM Scale Sets belong

    -Name <String>
        <Optional> To install to a single VM/VM Scale Set

    -PolicyAssignmentName <String>
        <Optional> Take the input VMs to operate on as the Compliance results from this Assignment
        If specified will only take from this source.

    -ReInstall [<SwitchParameter>]
        <Optional> If VM/VM Scale Set is already configured for a different workspace, set this to change to the new workspace

    -TriggerVmssManualVMUpdate [<SwitchParameter>]
        <Optional> Set this flag to trigger update of VM instances in a scale set whose upgrade policy is set to Manual

    -Approve [<SwitchParameter>]
        <Optional> Gives the approval for the installation to start with no confirmation prompt for the listed VMs/VM Scale Sets

    -WorkspaceRegion <String>
        Region the Log Analytics Workspace is in
        Supported values: "East US","eastus","Southeast Asia","southeastasia","West Central US","westcentralus","West Europe","westeurope"
        For Health supported is: "East US","eastus","West Central US","westcentralus"

    -WhatIf [<SwitchParameter>]
        <Optional> See what would happen in terms of installs.
        If extension is already installed will show what workspace is currently configured, and status of the VM extension

    -Confirm [<SwitchParameter>]
        <Optional> Confirm every action

    <CommonParameters>
        This cmdlet supports the common parameters: Verbose, Debug,
        ErrorAction, ErrorVariable, WarningAction, WarningVariable,
        OutBuffer, PipelineVariable, and OutVariable. For more information, see
        about_CommonParameters (https:/go.microsoft.com/fwlink/?LinkID=113216).

    -------------------------- EXAMPLE 1 --------------------------
    .\Install-VMInsights.ps1 -WorkspaceRegion eastus -WorkspaceId <WorkspaceId>-WorkspaceKey <WorkspaceKey> -SubscriptionId <SubscriptionId>
    -ResourceGroup <ResourceGroup>

    Install for all VMs in a resource group in a subscription

    -------------------------- EXAMPLE 2 --------------------------
    .\Install-VMInsights.ps1 -WorkspaceRegion eastus -WorkspaceId <WorkspaceId>-WorkspaceKey <WorkspaceKey> -SubscriptionId <SubscriptionId>
    -ResourceGroup <ResourceGroup> -ReInstall

    Specify to reinstall extensions even if already installed, for example, to update to a different workspace

    -------------------------- EXAMPLE 3 --------------------------
    .\Install-VMInsights.ps1 -WorkspaceRegion eastus -WorkspaceId <WorkspaceId>-WorkspaceKey <WorkspaceKey> -SubscriptionId <SubscriptionId>
    -PolicyAssignmentName a4f79f8ce891455198c08736 -ReInstall

    Specify to use a PolicyAssignmentName for source and to reinstall (move to a new workspace)
```

En el ejemplo siguiente se muestra cómo utilizar los comandos de PowerShell en la carpeta para habilitar Azure Monitor para VM y comprender el resultado esperado:

```powershell
$WorkspaceId = "<GUID>"
$WorkspaceKey = "<Key>"
$SubscriptionId = "<GUID>"
.\Install-VMInsights.ps1 -WorkspaceId $WorkspaceId -WorkspaceKey $WorkspaceKey -SubscriptionId $SubscriptionId -WorkspaceRegion eastus

Getting list of VMs or virtual machine scale sets matching criteria specified

VMs or virtual machine scale sets matching criteria:

db-ws-1 VM running
db-ws2012 VM running

This operation will install the Log Analytics and Dependency agent extensions on the previous two VMs or virtual machine scale sets.
VMs in a non-running state will be skipped.
Extension will not be reinstalled if already installed. Use -ReInstall if desired, for example, to update workspace.

Confirm
Continue?
[Y] Yes  [N] No  [S] Suspend  [?] Help (default is "Y"): y

db-ws-1 : Deploying DependencyAgentWindows with name DAExtension
db-ws-1 : Successfully deployed DependencyAgentWindows
db-ws-1 : Deploying MicrosoftMonitoringAgent with name MMAExtension
db-ws-1 : Successfully deployed MicrosoftMonitoringAgent
db-ws2012 : Deploying DependencyAgentWindows with name DAExtension
db-ws2012 : Successfully deployed DependencyAgentWindows
db-ws2012 : Deploying MicrosoftMonitoringAgent with name MMAExtension
db-ws2012 : Successfully deployed MicrosoftMonitoringAgent

Summary:

Already onboarded: (0)

Succeeded: (4)
db-ws-1 : Successfully deployed DependencyAgentWindows
db-ws-1 : Successfully deployed MicrosoftMonitoringAgent
db-ws2012 : Successfully deployed DependencyAgentWindows
db-ws2012 : Successfully deployed MicrosoftMonitoringAgent

Connected to different workspace: (0)

Not running - start VM to configure: (0)

Failed: (0)
```

## <a name="next-steps"></a>Pasos siguientes

Ahora que la supervisión está habilitada para las máquinas virtuales, esta información está disponible para analizarse con Azure Monitor para VM.

- Para ver las dependencias de las aplicaciones detectadas, consulte [Uso de la asignación de Azure Monitor para VM (versión preliminar) para conocer los componentes de una aplicación](vminsights-maps.md).

- Para identificar los cuellos de botella y el uso general con el rendimiento de la máquina virtual, vea [Cómo representar el rendimiento en gráficos con Azure Monitor para VM (versión preliminar)](vminsights-performance.md).
