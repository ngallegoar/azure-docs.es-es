---
title: Uso de plantillas de Azure Resource Manager para incorporar Update Management | Microsoft Docs
description: Puede usar una plantilla de Azure Resource Manager para incorporar la solución Update Management de Azure Automation.
ms.service: automation
ms.subservice: update-management
ms.topic: conceptual
author: mgoedtel
ms.author: magoedte
ms.date: 03/30/2020
ms.openlocfilehash: e69f3d7350d0da9f364983eae0935532b576bd76
ms.sourcegitcommit: 27bbda320225c2c2a43ac370b604432679a6a7c0
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/31/2020
ms.locfileid: "80411469"
---
# <a name="onboard-update-management-solution-using-azure-resource-manager-template"></a>Incorporación de la solución Update Management con una plantilla de Azure Resource Manager

Puede usar [plantillas de Azure Resource Manager](../azure-resource-manager/templates/template-syntax.md) para habilitar la solución Update Management de Azure Automation en un grupo de recursos. En este artículo se proporciona una plantilla de ejemplo que automatiza lo siguiente:

* La creación de un área de trabajo de Log Analytics en Azure Monitor.
* El registro de una cuenta de Azure Automation.
* El vínculo de la cuenta de Automation al área de trabajo de Log Analytics si aún no está vinculada.
* Incorporación de la solución Update Management de Azure Automation

La plantilla no automatiza la incorporación de una o varias máquinas virtuales de Azure o que no son de Azure.

Si ya tiene un área de trabajo de Log Analytics y una cuenta de Automation implementadas en una región admitida de su suscripción, no están vinculadas y el área de trabajo aún no tiene implementada la solución Update Management, esta plantilla crea correctamente el vínculo e implementa dicha solución. 

## <a name="api-versions"></a>Versiones de API

En la tabla siguiente se muestra la versión de API de los recursos usados en este ejemplo.

| Resource | Tipo de recurso | Versión de API |
|:---|:---|:---|
| Área de trabajo | workspaces | 2017-03-15-preview |
| Cuenta de Automation | automation | 2015-10-31 | 
| Solución | solutions | 2015-11-01-preview |

## <a name="before-using-the-template"></a>Antes de usar la plantilla

Si decide instalar y usar PowerShell de forma local, para realizar los pasos de este artículo necesita el módulo Az de Azure PowerShell. Ejecute `Get-Module -ListAvailable Az` para encontrar la versión. Si necesita actualizarla, consulte [Instalación del módulo de Azure PowerShell](/powershell/azure/install-az-ps). Si PowerShell se ejecuta localmente, también debe ejecutar `Connect-AzAccount` para crear una conexión con Azure. Con Azure PowerShell, la implementación usa [New-AzResourceGroupDeployment](/powershell/module/az.resources/new-azresourcegroupdeployment).

Si decide instalar y usar la CLI localmente, para este artículo es preciso que ejecute la versión 2.1.0 o posterior de la CLI de Azure. Ejecute `az --version` para encontrar la versión. Si necesita instalarla o actualizarla, vea [Instalación de la CLI de Azure](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest). Con la CLI de Azure, esta implementación usa [az group deployment create](https://docs.microsoft.com/cli/azure/group/deployment?view=azure-cli-latest#az-group-deployment-create). 

La plantilla JSON se configura para pedir al usuario:

* El nombre del área de trabajo
* La región en la que crear un área de trabajo
* El nombre de la cuenta de Automation
* La región en la que crear la cuenta

La plantilla JSON especifica un valor predeterminado para el resto de parámetros que es probable que se utilice como configuración estándar en el entorno. Puede almacenar la plantilla en una cuenta de Azure Storage para el acceso compartido en la organización. Para más información sobre cómo trabajar con plantillas, consulte [Implementación de recursos con plantillas de Resource Manager y la CLI de Azure](../azure-resource-manager/templates/deploy-cli.md).

Los siguientes parámetros de la plantilla se configuran con un valor predeterminado para el área de trabajo de Log Analytics:

* SKU: el valor predeterminado es el nuevo plan de tarifa por GB publicado en el modelo de precios de abril de 2018
* Retención de datos: el valor predeterminado es de 30 días
* Reserva de capacidad: el valor predeterminado es de 100 GB.

>[!WARNING]
>Si se crea o configura un área de trabajo de Log Analytics en una suscripción que ha elegido el nuevo modelo de precios de abril de 2018, el único plan de tarifa válido de Log Analytics es **PerGB2018**.
>

>[!NOTE]
>Antes de usar esta plantilla, revise los [detalles adicionales](../azure-monitor/platform/template-workspace-configuration.md#create-a-log-analytics-workspace) para saber totalmente las opciones de configuración del área de trabajo, como el modo de control de acceso, el plan de tarifa, la retención y el nivel de reserva de capacidad. Si no está familiarizado con los registros de Azure Monitor y aún no ha implementado un área de trabajo, debe revisar las instrucciones de [diseño del área de trabajo](../azure-monitor/platform/design-logs-deployment.md) para obtener información sobre el control de acceso y conocer las estrategias de implementación de diseño que se recomiendan en la organización.

## <a name="deploy-template"></a>Implementar plantilla

1. Copie y pegue la siguiente sintaxis JSON en el archivo:

    ```json
    {
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "workspaceName": {
            "type": "string",
            "metadata": {
                "description": "Workspace name"
            }
        },
        "sku": {
            "type": "string",
            "allowedValues": [
                "pergb2018",
                "Free",
                "Standalone",
                "PerNode",
                "Standard",
                "Premium"
            ],
            "defaultValue": "pergb2018",
            "metadata": {
                "description": "Pricing tier: perGB2018 or legacy tiers (Free, Standalone, PerNode, Standard or Premium) which are not available to all customers."
            }
        },
        "dataRetention": {
            "type": "int",
            "defaultValue": 30,
            "minValue": 7,
            "maxValue": 730,
            "metadata": {
                "description": "Number of days of retention. Workspaces in the legacy Free pricing tier can only have 7 days."
            }
        },
        "immediatePurgeDataOn30Days": {
            "type": "bool",
            "defaultValue": "[bool('false')]",
            "metadata": {
                "description": "If set to true when changing retention to 30 days, older data will be immediately deleted. Use this with extreme caution. This only applies when retention is being set to 30 days."
            }
        },
        "location": {
            "type": "string",
            "allowedValues": [
                "australiacentral",
                "australiaeast",
                "australiasoutheast",
                "brazilsouth",
                "canadacentral",
                "centralindia",
                "centralus",
                "eastasia",
                "eastus",
                "eastus2",
                "francecentral",
                "japaneast",
                "koreacentral",
                "northcentralus",
                "northeurope",
                "southafricanorth",
                "southcentralus",
                "southeastasia",
                "uksouth",
                "ukwest",
                "westcentralus",
                "westeurope",
                "westus",
                "westus2"
            ],
            "metadata": {
                "description": "Specifies the location in which to create the workspace."
            }
        },
        "automationAccountName": {
            "type": "string",
            "metadata": {
                "description": "Automation account name"
            }
        },
        "automationAccountLocation": {
            "type": "string",
            "metadata": {
                "description": "Specify the location in which to create the Automation account."
            }
        }
    },
    "variables": {
        "Updates": {
            "name": "[concat('Updates', '(', parameters('workspaceName'), ')')]",
            "galleryName": "Updates"
        }
    },
    "resources": [
        {
        "type": "Microsoft.OperationalInsights/workspaces",
            "name": "[parameters('workspaceName')]",
            "apiVersion": "2017-03-15-preview",
            "location": "[parameters('location')]",
            "properties": {
                "sku": {
                    "Name": "[parameters('sku')]",
                    "name": "CapacityReservation",
                    "capacityReservationLevel": 100
                },
                "retentionInDays": "[parameters('dataRetention')]",
                "features": {
                    "searchVersion": 1,
                    "legacy": 0,
                    "enableLogAccessUsingOnlyResourcePermissions": true
                }
            },
            "resources": [
                {
                    "apiVersion": "2015-11-01-preview",
                    "location": "[resourceGroup().location]",
                    "name": "[variables('Updates').name]",
                    "type": "Microsoft.OperationsManagement/solutions",
                    "id": "[concat('/subscriptions/', subscription().subscriptionId, '/resourceGroups/', resourceGroup().name, '/providers/Microsoft.OperationsManagement/solutions/', variables('Updates').name)]",
                    "dependsOn": [
                        "[concat('Microsoft.OperationalInsights/workspaces/', parameters('workspaceName'))]"
                    ],
                    "properties": {
                        "workspaceResourceId": "[resourceId('Microsoft.OperationalInsights/workspaces/', parameters('workspaceName'))]"
                    },
                    "plan": {
                        "name": "[variables('Updates').name]",
                        "publisher": "Microsoft",
                        "promotionCode": "",
                        "product": "[concat('OMSGallery/', variables('Updates').galleryName)]"
                    }
                }
            ]
        },
        {
            "type": "Microsoft.Automation/automationAccounts",
            "apiVersion": "2015-01-01-preview",
            "name": "[parameters('automationAccountName')]",
            "location": "[parameters('automationAccountLocation')]",
            "dependsOn": [],
            "tags": {},
            "properties": {
                "sku": {
                    "name": "Basic"
                }
            },
        },
        {
            "apiVersion": "2015-11-01-preview",
            "type": "Microsoft.OperationalInsights/workspaces/linkedServices",
            "name": "[concat(parameters('workspaceName'), '/' , 'Automation')]",
            "location": "[resourceGroup().location]",
            "dependsOn": [
                "[concat('Microsoft.OperationalInsights/workspaces/', parameters('workspaceName'))]",
                "[concat('Microsoft.Automation/automationAccounts/', parameters('automationAccountName'))]"
            ],
            "properties": {
                "resourceId": "[resourceId('Microsoft.Automation/automationAccounts/', parameters('automationAccountName'))]"
            }
        }
      ]
    }
    ```

2. Edite la plantilla para adecuarla a sus requisitos. Plantéese la posibilidad de crear un [archivo de parámetros de Resource Manager](../azure-resource-manager/templates/parameter-files.md) en lugar de pasar los parámetros como valores insertados.

3. Guarde este archivo como deployUMSolutiontemplate.json en una carpeta local.

4. Está listo para implementar esta plantilla. Puede usar la CLI de Azure o PowerShell. Cuando se le pida un nombre de cuenta de Automation o un área de trabajo, proporcione un nombre que sea globalmente único en todas las suscripciones de Azure.

    **PowerShell**

    ```powershell
    New-AzResourceGroupDeployment -Name <deployment-name> -ResourceGroupName <resource-group-name> -TemplateFile deployUMSolutiontemplate.json
    ```

    **CLI de Azure**

    ```cli
    az group deployment create --resource-group <my-resource-group> --name <my-deployment-name> --template-file deployUMSolutiontemplate.json
    ```

    La implementación puede demorar unos minutos en completarse. Cuando termine, verá un mensaje similar al siguiente que incluye el resultado:

    ![Resultado de ejemplo cuando se completa la implementación](media/automation-update-management-deploy-template/template-output.png)

## <a name="next-steps"></a>Pasos siguientes

Ahora que tiene implementada la solución Update Management, puede habilitar las máquinas virtuales para la administración, revisar las evaluaciones de las actualizaciones e implementar las actualizaciones para someterlas a cumplimiento.

- Desde la [cuenta de Azure Automation](automation-onboard-solutions-from-automation-account.md) para una o varias máquinas de Azure y manualmente para máquinas que no son de Azure.

- Para una sola máquina virtual de Azure desde la página de la máquina virtual en Azure Portal. Este escenario está disponible para las VM [Linux](../virtual-machines/linux/tutorial-config-management.md#enable-update-management) y [Windows](../virtual-machines/windows/tutorial-config-management.md#enable-update-management).

- Para [varias máquinas virtuales de Azure](manage-update-multi.md), selecciónelas en la página **Máquinas virtuales** de Azure Portal. 