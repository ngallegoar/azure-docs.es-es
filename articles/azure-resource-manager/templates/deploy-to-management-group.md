---
title: Implementación de recursos en el grupo de administración
description: Se describe cómo implementar recursos en el ámbito de un grupo de administración en una plantilla de Azure Resource Manager.
ms.topic: conceptual
ms.date: 10/22/2020
ms.openlocfilehash: 084ab69f463334569d37efd9187bfe587bfc524d
ms.sourcegitcommit: 4cb89d880be26a2a4531fedcc59317471fe729cd
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/27/2020
ms.locfileid: "92668939"
---
# <a name="management-group-deployments-with-arm-templates"></a>Implementaciones de grupos de administración con plantillas de Resource Manager

A medida que la organización madura, puede implementar una plantilla de Azure Resource Manager (plantilla de ARM) para crear recursos a nivel de grupo de administración. Por ejemplo, es posible que deba definir y asignar [directivas](../../governance/policy/overview.md) o el [control de acceso basado en rol de Azure (RBAC de Azure)](../../role-based-access-control/overview.md) para un grupo de administración. Con las plantillas de nivel de grupo de administración, puede aplicar directivas y asignar roles mediante declaración en el nivel de grupo de administración.

## <a name="supported-resources"></a>Recursos compatibles

No todos los tipos de recursos se pueden implementar en el nivel de grupo de administración. En esta sección se enumeran los tipos de recursos que se admiten.

Para Azure Blueprints, use:

* [artifacts](/azure/templates/microsoft.blueprint/blueprints/artifacts)
* [blueprints](/azure/templates/microsoft.blueprint/blueprints)
* [blueprintAssignments](/azure/templates/microsoft.blueprint/blueprintassignments)
* [versions](/azure/templates/microsoft.blueprint/blueprints/versions)

Para las directivas de Azure, use:

* [policyAssignments](/azure/templates/microsoft.authorization/policyassignments)
* [policyDefinitions](/azure/templates/microsoft.authorization/policydefinitions)
* [policySetDefinitions](/azure/templates/microsoft.authorization/policysetdefinitions)
* [remediations](/azure/templates/microsoft.policyinsights/remediations)

Para el control de acceso basado en rol de Azure (Azure RBAC), use:

* [roleAssignments](/azure/templates/microsoft.authorization/roleassignments)
* [roleDefinitions](/azure/templates/microsoft.authorization/roledefinitions)

Para plantillas anidadas que se implementan en suscripciones o grupos de recursos, use:

* [deployments](/azure/templates/microsoft.resources/deployments)

Para administrar los recursos, use:

* [etiquetas](/azure/templates/microsoft.resources/tags)

## <a name="schema"></a>Schema

El esquema que se usa para las implementaciones de nivel de grupo de administración es diferente del esquema de las implementaciones de grupo de recursos.

Para las plantillas, use:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2019-08-01/managementGroupDeploymentTemplate.json#",
    ...
}
```

El esquema de un archivo de parámetros es el mismo para todos los ámbitos de implementación. Para los archivos de parámetros, use:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentParameters.json#",
    ...
}
```

## <a name="deployment-commands"></a>Comandos de implementación

Para implementar en un grupo de administración, use los comandos de implementación de grupos de administración.

# <a name="azure-cli"></a>[CLI de Azure](#tab/azure-cli)

Para la CLI de Azure, use [az deployment mg create](/cli/azure/deployment/mg#az-deployment-mg-create):

```azurecli-interactive
az deployment mg create \
  --name demoMGDeployment \
  --location WestUS \
  --management-group-id myMG \
  --template-uri "https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/management-level-deployment/azuredeploy.json"
```

# <a name="powershell"></a>[PowerShell](#tab/azure-powershell)

En el caso de Azure PowerShell, use [New-AzManagementGroupDeployment](/powershell/module/az.resources/new-azmanagementgroupdeployment).

```azurepowershell-interactive
New-AzManagementGroupDeployment `
  -Name demoMGDeployment `
  -Location "West US" `
  -ManagementGroupId "myMG" `
  -TemplateUri "https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/management-level-deployment/azuredeploy.json"
```

---

Para obtener información más detallada sobre los comandos de implementación y las opciones para implementar plantillas de Resource Manager, consulte:

* [Implementación de recursos con Azure Portal y plantillas de Resource Manager](deploy-portal.md)
* [Implementación de recursos con plantillas de ARM y la CLI de Azure](deploy-cli.md)
* [Implementación de recursos con las plantillas de ARM y Azure PowerShell](deploy-powershell.md)
* [Implementación de recursos con plantillas de Resource Manager y la API de REST de Azure Resource Manager](deploy-rest.md)
* [Usar un botón de implementación para implementar plantillas desde el repositorio de GitHub](deploy-to-azure-button.md)
* [Implementación de plantillas de Resource Manager desde Cloud Shell](deploy-cloud-shell.md)

## <a name="deployment-scopes"></a>Ámbitos de implementación

Al implementar en un grupo de administración, puede implementar los recursos en:

* el grupo de administración de destino de la operación
* otro grupo de administración en el inquilino
* suscripciones en el grupo de administración
* grupos de recursos del grupo de administración (a través de dos implementaciones anidadas)
* se pueden aplicar [recursos de extensión](scope-extension-resources.md) a los recursos

El usuario que implementa la plantilla debe tener acceso al ámbito especificado.

En esta sección se muestra cómo especificar distintos ámbitos. Puede combinar estos distintos ámbitos en una sola plantilla.

### <a name="scope-to-target-management-group"></a>Ámbito del grupo de administración de destino

Los recursos definidos en la sección de recursos de la plantilla se aplican al grupo de administración del comando de implementación.

:::code language="json" source="~/resourcemanager-templates/azure-resource-manager/scope/default-mg.json" highlight="5":::

### <a name="scope-to-another-management-group"></a>Ámbito de otro grupo de administración

Para establecer como destino otro grupo de administración, agregue una implementación anidada y especifique la propiedad `scope`. Establezca la propiedad `scope` en un valor con el formato `Microsoft.Management/managementGroups/<mg-name>`.

:::code language="json" source="~/resourcemanager-templates/azure-resource-manager/scope/scope-mg.json" highlight="10,17,22":::

### <a name="scope-to-subscription"></a>Ámbito de la suscripción

También puede establecer como destino suscripciones con un grupo de administración. El usuario que implementa la plantilla debe tener acceso al ámbito especificado.

Para establecer como destino una suscripción dentro del grupo de administración, use una implementación anidada y la propiedad `subscriptionId`.

:::code language="json" source="~/resourcemanager-templates/azure-resource-manager/scope/mg-to-subscription.json" highlight="10,18":::

### <a name="scope-to-resource-group"></a>Ámbito del grupo de recursos

Para establecer como destino un grupo de recursos dentro de esa suscripción, agregue dos implementaciones anidadas. La primera tiene como destino la suscripción que tiene el grupo de recursos. La segunda tiene como destino el grupo de recursos al establecer la propiedad `resourceGroup`.

:::code language="json" source="~/resourcemanager-templates/azure-resource-manager/scope/mg-to-resource-group.json" highlight="10,21,25":::

Para usar la implementación de un grupo de administración para crear un grupo de recursos dentro de una suscripción e implementar una cuenta de almacenamiento en ese grupo de recursos, vea [Implementación a una suscripción y a un grupo de recursos](#deploy-to-subscription-and-resource-group).

## <a name="deployment-location-and-name"></a>Ubicación y nombre de la implementación

En el caso de las implementaciones de nivel de grupo de administración, debe proporcionar una ubicación para la implementación. La ubicación de la implementación es independiente de la ubicación de los recursos que se implementan. La ubicación de implementación especifica dónde se almacenarán los datos de la implementación.

Puede proporcionar un nombre para la implementación o usar el nombre de implementación predeterminado. El nombre predeterminado es el nombre del archivo de plantilla. Por ejemplo, al implementar una plantilla llamada **azuredeploy.json** , se crea un nombre de predeterminado **azuredeploy**.

Para cada nombre de implementación, la ubicación es inmutable. No se puede crear una implementación en una ubicación si ya existe una implementación con el mismo nombre en otra ubicación. Si recibe el código de error `InvalidDeploymentLocation`, use un nombre diferente o utilice la ubicación de la implementación anterior que tenía ese mismo nombre.

## <a name="azure-policy"></a>Azure Policy

Las definiciones de directivas personalizadas que se implementan en un grupo de administración son extensiones del grupo de administración. Para obtener el identificador de una definición de directiva personalizada, use la función [extensionResourceId()](template-functions-resource.md#extensionresourceid). Las definiciones de directivas integradas son recursos del nivel de inquilino. Para obtener el identificador de una definición de directiva integrada, use la función [tenantResourceId](template-functions-resource.md#tenantresourceid).

En el ejemplo siguiente se muestra cómo [definir](../../governance/policy/concepts/definition-structure.md) una directiva en el nivel de grupo de administración y cómo asignarla.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2019-08-01/managementGroupDeploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "targetMG": {
            "type": "string",
            "metadata": {
                "description": "Target Management Group"
            }
        },
        "allowedLocations": {
            "type": "array",
            "defaultValue": [
                "australiaeast",
                "australiasoutheast",
                "australiacentral"
            ],
            "metadata": {
                "description": "An array of the allowed locations, all other locations will be denied by the created policy."
            }
        }
    },
    "variables": {
        "mgScope": "[tenantResourceId('Microsoft.Management/managementGroups', parameters('targetMG'))]",
        "policyDefinition": "LocationRestriction"
    },
    "resources": [
        {
            "type": "Microsoft.Authorization/policyDefinitions",
            "name": "[variables('policyDefinition')]",
            "apiVersion": "2019-09-01",
            "properties": {
                "policyType": "Custom",
                "mode": "All",
                "parameters": {
                },
                "policyRule": {
                    "if": {
                        "not": {
                            "field": "location",
                            "in": "[parameters('allowedLocations')]"
                        }
                    },
                    "then": {
                        "effect": "deny"
                    }
                }
            }
        },
        {
            "type": "Microsoft.Authorization/policyAssignments",
            "name": "location-lock",
            "apiVersion": "2019-09-01",
            "dependsOn": [
                "[variables('policyDefinition')]"
            ],
            "properties": {
                "scope": "[variables('mgScope')]",
                "policyDefinitionId": "[extensionResourceId(variables('mgScope'), 'Microsoft.Authorization/policyDefinitions', variables('policyDefinition'))]"
            }
        }
    ]
}
```

## <a name="deploy-to-subscription-and-resource-group"></a>Implementación a una suscripción y a un grupo de recursos

En una implementación de nivel de grupo de administración, puede establecer como destino una suscripción dentro del grupo de administración. En el ejemplo siguiente se crea un grupo de recursos en una suscripción y se implementa una cuenta de almacenamiento en el grupo de recursos.

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2019-08-01/managementGroupDeploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "nestedsubId": {
      "type": "string"
    },
    "nestedRG": {
      "type": "string"
    },
    "storageAccountName": {
      "type": "string"
    },
    "nestedLocation": {
      "type": "string"
    }
  },
  "resources": [
    {
      "type": "Microsoft.Resources/deployments",
      "apiVersion": "2020-06-01",
      "name": "nestedSub",
      "location": "[parameters('nestedLocation')]",
      "subscriptionId": "[parameters('nestedSubId')]",
      "properties": {
        "mode": "Incremental",
        "template": {
          "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
          "contentVersion": "1.0.0.0",
          "parameters": {
          },
          "variables": {
          },
          "resources": [
            {
              "type": "Microsoft.Resources/resourceGroups",
              "apiVersion": "2020-06-01",
              "name": "[parameters('nestedRG')]",
              "location": "[parameters('nestedLocation')]",
            },
            {
              "type": "Microsoft.Resources/deployments",
              "apiVersion": "2020-06-01",
              "name": "nestedSubRG",
              "resourceGroup": "[parameters('nestedRG')]",
              "dependsOn": [
                "[parameters('nestedRG')]"
              ],
              "properties": {
                "mode": "Incremental",
                "template": {
                  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
                  "contentVersion": "1.0.0.0",
                  "resources": [
                    {
                      "type": "Microsoft.Storage/storageAccounts",
                      "apiVersion": "2019-04-01",
                      "name": "[parameters('storageAccountName')]",
                      "location": "[parameters('nestedLocation')]",
                      "sku": {
                        "name": "Standard_LRS"
                      }
                    }
                  ]
                }
              }
            }
          ]
        }
      }
    }
  ]
}
```

## <a name="next-steps"></a>Pasos siguientes

* Para más información sobre la asignación de roles, consulte [Incorporación de asignaciones de roles mediante plantillas de Azure Resource Manager](../../role-based-access-control/role-assignments-template.md).
* Para un ejemplo de implementación de la configuración del área de trabajo para Azure Security Center, consulte [deployASCwithWorkspaceSettings.json](https://github.com/krnese/AzureDeploy/blob/master/ARM/deployments/deployASCwithWorkspaceSettings.json).
* También puede implementar plantillas en el [nivel de suscripción](deploy-to-subscription.md) y en el [nivel de inquilino](deploy-to-tenant.md).
