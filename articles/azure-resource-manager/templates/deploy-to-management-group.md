---
title: Implementación de recursos en el grupo de administración
description: Se describe cómo implementar recursos en el ámbito de un grupo de administración en una plantilla de Azure Resource Manager.
ms.topic: conceptual
ms.date: 03/16/2020
ms.openlocfilehash: 863d1330412fa238b820eb0f1f05351fc723de6f
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "79460320"
---
# <a name="create-resources-at-the-management-group-level"></a>Creación de recursos a nivel de grupo de administración

A medida que la organización madura, es posible que necesite definir y asignar [directivas](../../governance/policy/overview.md) o [controles de acceso basados en rol](../../role-based-access-control/overview.md) para un grupo de administración. Con las plantillas de nivel de grupo de administración, puede aplicar directivas y asignar roles mediante declaración en el nivel de grupo de administración.

## <a name="supported-resources"></a>Recursos compatibles

Puede implementar los siguientes tipos de recursos en el nivel de grupo de administración:

* [implementaciones](/azure/templates/microsoft.resources/deployments): para plantillas anidadas que se implementan en suscripciones o grupos de recursos.
* [policyAssignments](/azure/templates/microsoft.authorization/policyassignments)
* [policyDefinitions](/azure/templates/microsoft.authorization/policydefinitions)
* [policySetDefinitions](/azure/templates/microsoft.authorization/policysetdefinitions)
* [roleAssignments](/azure/templates/microsoft.authorization/roleassignments)
* [roleDefinitions](/azure/templates/microsoft.authorization/roledefinitions)

### <a name="schema"></a>Schema

El esquema que se usa para las implementaciones de nivel de grupo de administración es diferente del esquema de las implementaciones de grupo de recursos.

Para las plantillas, use:

```json
https://schema.management.azure.com/schemas/2019-08-01/managementGroupDeploymentTemplate.json#
```

El esquema de un archivo de parámetros es el mismo para todos los ámbitos de implementación. Para los archivos de parámetros, use:

```json
https://schema.management.azure.com/schemas/2019-04-01/deploymentParameters.json#
```

## <a name="deployment-commands"></a>Comandos de implementación

Los comandos para las implementaciones de grupos de administración son diferentes de los comandos para las implementaciones de grupos de recursos.

Para la CLI de Azure, use [az deployment mg create](/cli/azure/deployment/mg?view=azure-cli-latest#az-deployment-mg-create):

```azurecli-interactive
az deployment mg create \
  --name demoMGDeployment \
  --location WestUS \
  --management-group-id myMG \
  --template-uri "https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/management-level-deployment/azuredeploy.json"
```

En el caso de Azure PowerShell, use [New-AzManagementGroupDeployment](/powershell/module/az.resources/new-azmanagementgroupdeployment).

```azurepowershell-interactive
New-AzManagementGroupDeployment `
  -Name demoMGDeployment `
  -Location "West US" `
  -ManagementGroupId "myMG" `
  -TemplateUri "https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/management-level-deployment/azuredeploy.json"
```

Para la API REST, use [Deployments: Create At Management Group Scope](/rest/api/resources/deployments/createorupdateatmanagementgroupscope).

## <a name="deployment-location-and-name"></a>Ubicación y nombre de la implementación

En el caso de las implementaciones de nivel de grupo de administración, debe proporcionar una ubicación para la implementación. La ubicación de la implementación es independiente de la ubicación de los recursos que se implementan. La ubicación de implementación especifica dónde se almacenarán los datos de la implementación.

Puede proporcionar un nombre para la implementación o usar el nombre de implementación predeterminado. El nombre predeterminado es el nombre del archivo de plantilla. Por ejemplo, al implementar una plantilla llamada **azuredeploy.json**, se crea un nombre de predeterminado **azuredeploy**.

Para cada nombre de implementación, la ubicación es inmutable. No se puede crear una implementación en una ubicación si ya existe una implementación con el mismo nombre en otra ubicación. Si recibe el código de error `InvalidDeploymentLocation`, use un nombre diferente o utilice la ubicación de la implementación anterior que tenía ese mismo nombre.

## <a name="use-template-functions"></a>Usar funciones de plantillas

En las implementaciones de nivel de grupo de administración, hay algunas consideraciones importantes que deben tenerse en cuenta al usar las funciones de plantilla:

* La función [resourceGroup()](template-functions-resource.md#resourcegroup)**no** se admite.
* La función [subscription()](template-functions-resource.md#subscription)**no** se admite.
* Se admiten las funciones [reference()](template-functions-resource.md#reference) y [list()](template-functions-resource.md#list).
* La función [resourceId()](template-functions-resource.md#resourceid) sí se admite. Utilícela para obtener el identificador de los recursos que se utilizan en las implementaciones de nivel de grupo de administración. No proporcione un valor para el parámetro del grupo de recursos.

  Por ejemplo, para obtener el identificador de recurso de una definición de directiva, utilice:
  
  ```json
  resourceId('Microsoft.Authorization/policyDefinitions/', parameters('policyDefinition'))
  ```
  
  El identificador de recurso devuelto tiene el formato siguiente:
  
  ```json
  /providers/{resourceProviderNamespace}/{resourceType}/{resourceName}
  ```

## <a name="create-policies"></a>Creación de directivas

### <a name="define-policy"></a>Definición de directiva

En el ejemplo siguiente se muestra cómo [definir](../../governance/policy/concepts/definition-structure.md) una directiva en el nivel de grupo de administración.

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2019-08-01/managementGroupDeploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {},
  "variables": {},
  "resources": [
    {
      "type": "Microsoft.Authorization/policyDefinitions",
      "apiVersion": "2018-05-01",
      "name": "locationpolicy",
      "properties": {
        "policyType": "Custom",
        "parameters": {},
        "policyRule": {
          "if": {
            "field": "location",
            "equals": "northeurope"
          },
          "then": {
            "effect": "deny"
          }
        }
      }
    }
  ]
}
```

### <a name="assign-policy"></a>Asignación de directiva

En el ejemplo siguiente se asigna una definición de directiva existente al grupo de administración. Si la directiva toma parámetros, proporciónelos como un objeto. Si la directiva no toma parámetros, use el objeto vacío predeterminado.

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2019-08-01/managementGroupDeploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "policyDefinitionID": {
      "type": "string"
    },
    "policyName": {
      "type": "string"
    },
    "policyParameters": {
      "type": "object",
      "defaultValue": {}
    }
  },
  "variables": {},
  "resources": [
    {
      "type": "Microsoft.Authorization/policyAssignments",
      "apiVersion": "2018-03-01",
      "name": "[parameters('policyName')]",
      "properties": {
        "policyDefinitionId": "[parameters('policyDefinitionID')]",
        "parameters": "[parameters('policyParameters')]"
      }
    }
  ]
}
```

## <a name="template-sample"></a>Ejemplo de plantilla

* [Cree un grupo de recursos, una directiva y una asignación de directiva](https://github.com/Azure/azure-docs-json-samples/blob/master/management-level-deployment/azuredeploy.json).

## <a name="next-steps"></a>Pasos siguientes

* Para aprender sobre los roles de asignación, consulte [Administración del acceso a los recursos de Azure mediante RBAC y plantillas de Azure Resource Manager](../../role-based-access-control/role-assignments-template.md).
* Para un ejemplo de implementación de la configuración del área de trabajo para Azure Security Center, consulte [deployASCwithWorkspaceSettings.json](https://github.com/krnese/AzureDeploy/blob/master/ARM/deployments/deployASCwithWorkspaceSettings.json).
* También puede implementar plantillas en el [nivel de suscripción](deploy-to-subscription.md) y en el [nivel de inquilino](deploy-to-tenant.md).
