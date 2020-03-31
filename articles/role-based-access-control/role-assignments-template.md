---
title: Adición de asignaciones de roles mediante RBAC y plantillas de Azure Resource Manager
description: Aprenda a conceder acceso a recursos de Azure para usuarios, grupos, entidades de servicio e identidades administradas mediante el control de acceso basado en rol (RBAC) y plantillas de Azure Resource Manager.
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
ms.service: role-based-access-control
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 11/25/2019
ms.author: rolyon
ms.reviewer: bagovind
ms.openlocfilehash: 9f817880f938f5d03024e3aacd9b84817a5ac721
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "77138297"
---
# <a name="add-role-assignments-using-azure-rbac-and-azure-resource-manager-templates"></a>Adición de asignaciones de roles mediante RBAC de Azure y plantillas de Azure Resource Manager

[!INCLUDE [Azure RBAC definition grant access](../../includes/role-based-access-control-definition-grant.md)] Además de usar Azure PowerShell o la CLI de Azure, puede asignar roles mediante [plantillas de Azure Resource Manager](../azure-resource-manager/templates/template-syntax.md). Las plantillas se pueden usar si necesita implementar recursos de manera repetida y uniforme. En este artículo se describe cómo asignar roles mediante plantillas.

## <a name="get-object-ids"></a>Obtener los identificadores de objeto

Para asignar un rol, debe especificar el identificador del usuario, del grupo o de la aplicación a los que quiere asignar el rol. El identificador tiene el formato: `11111111-1111-1111-1111-111111111111`. Puede obtenerlo mediante Azure Portal, Azure PowerShell o la CLI de Azure.

### <a name="user"></a>Usuario

Para obtener el identificador de un usuario, puede usar los comandos [Get-AzADUser](/powershell/module/az.resources/get-azaduser) o [az ad user show](/cli/azure/ad/user#az-ad-user-show).

```azurepowershell
$objectid = (Get-AzADUser -DisplayName "{name}").id
```

```azurecli
objectid=$(az ad user show --id "{email}" --query objectId --output tsv)
```

### <a name="group"></a>Grupo

Para obtener el identificador de un grupo, puede usar los comandos [Get-AzADGroup](/powershell/module/az.resources/get-azadgroup) o [az ad group show](/cli/azure/ad/group#az-ad-group-show).

```azurepowershell
$objectid = (Get-AzADGroup -DisplayName "{name}").id
```

```azurecli
objectid=$(az ad group show --group "{name}" --query objectId --output tsv)
```

### <a name="application"></a>Application

Para obtener el identificador de una entidad de servicio (identidad usada por una aplicación), puede usar los comandos [Get-AzADServicePrincipal](/powershell/module/az.resources/get-azadserviceprincipal) o [az ad sp list](/cli/azure/ad/sp#az-ad-sp-list). Para una entidad de servicio, use el id. de objeto y **no** el de aplicación.

```azurepowershell
$objectid = (Get-AzADServicePrincipal -DisplayName "{name}").id
```

```azurecli
objectid=$(az ad sp list --display-name "{name}" --query [].objectId --output tsv)
```

## <a name="add-a-role-assignment"></a>Adición de una asignación de roles

En RBAC, para conceder acceso es preciso agregar una asignación de roles.

### <a name="resource-group-without-parameters"></a>Grupo de recursos (sin parámetros)

La plantilla siguiente muestra una manera básica de agregar una asignación de roles. Algunos valores se especifican en la plantilla. La plantilla siguiente muestra:

-  Cómo asignar el rol de [lector](built-in-roles.md#reader) a un usuario, un grupo o una aplicación, en el ámbito de un grupo de recursos

Para usar la plantilla, debe hacer lo siguiente:

- Cree un archivo JSON y copie la plantilla.
- Reemplace `<your-principal-id>` por el identificador de un usuario, de un grupo o de una aplicación a los que se va a asignar el rol.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "resources": [
        {
            "type": "Microsoft.Authorization/roleAssignments",
            "apiVersion": "2018-09-01-preview",
            "name": "[guid(resourceGroup().id)]",
            "properties": {
                "roleDefinitionId": "[concat('/subscriptions/', subscription().subscriptionId, '/providers/Microsoft.Authorization/roleDefinitions/', 'acdd72a7-3385-48ef-bd42-f606fba81ae7')]",
                "principalId": "<your-principal-id>"
            }
        }
    ]
}
```

Los comandos [New-AzResourceGroupDeployment](/powershell/module/az.resources/new-azresourcegroupdeployment) y [az group deployment create](/cli/azure/group/deployment#az-group-deployment-create) son ejemplos de cómo iniciar la implementación de un grupo de recursos llamado ExampleGroup.

```azurepowershell
New-AzResourceGroupDeployment -ResourceGroupName ExampleGroup -TemplateFile rbac-test.json
```

```azurecli
az group deployment create --resource-group ExampleGroup --template-file rbac-test.json
```

A continuación se muestra un ejemplo de la asignación del rol de lector a un usuario de un grupo de recursos después de implementar la plantilla.

![Asignación de roles en el ámbito de un grupo de recursos](./media/role-assignments-template/role-assignment-template.png)

### <a name="resource-group-or-subscription"></a>Grupo de recursos o suscripción

La plantilla anterior no es muy flexible. La siguiente plantilla usa parámetros y se puede emplear en distintos ámbitos. La plantilla siguiente muestra:

- Cómo asignar un rol a un usuario, un grupo o una aplicación en el ámbito de un grupo de recursos o de una suscripción
- Cómo especificar los roles de propietario, colaborador y lector como parámetro

Para usar la plantilla, debe especificar las siguientes entradas:

- El identificador de un usuario, de un grupo o de una aplicación al que se asignará el rol.
- Un identificador único que se usará para la asignación de roles (también puede usar el identificador predeterminado).

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "principalId": {
            "type": "string",
            "metadata": {
                "description": "The principal to assign the role to"
            }
        },
        "builtInRoleType": {
            "type": "string",
            "allowedValues": [
                "Owner",
                "Contributor",
                "Reader"
            ],
            "metadata": {
                "description": "Built-in role to assign"
            }
        },
        "roleNameGuid": {
            "type": "string",
            "defaultValue": "[newGuid()]",
            "metadata": {
                "description": "A new GUID used to identify the role assignment"
            }
        }
    },
    "variables": {
        "Owner": "[concat('/subscriptions/', subscription().subscriptionId, '/providers/Microsoft.Authorization/roleDefinitions/', '8e3af657-a8ff-443c-a75c-2fe8c4bcb635')]",
        "Contributor": "[concat('/subscriptions/', subscription().subscriptionId, '/providers/Microsoft.Authorization/roleDefinitions/', 'b24988ac-6180-42a0-ab88-20f7382dd24c')]",
        "Reader": "[concat('/subscriptions/', subscription().subscriptionId, '/providers/Microsoft.Authorization/roleDefinitions/', 'acdd72a7-3385-48ef-bd42-f606fba81ae7')]"
    },
    "resources": [
        {
            "type": "Microsoft.Authorization/roleAssignments",
            "apiVersion": "2018-09-01-preview",
            "name": "[parameters('roleNameGuid')]",
            "properties": {
                "roleDefinitionId": "[variables(parameters('builtInRoleType'))]",
                "principalId": "[parameters('principalId')]"
            }
        }
    ]
}
```

> [!NOTE]
> Esta plantilla no es idempotente, a menos que se proporcione el mismo valor `roleNameGuid` como parámetro para cada implementación de la plantilla. Si no se proporciona `roleNameGuid`, de manera predeterminada se generará un nuevo GUID en cada implementación y se producirá un error `Conflict: RoleAssignmentExists` en las implementaciones posteriores.

El ámbito de la asignación de roles se determina a partir del nivel de la implementación. Los comandos [New-AzResourceGroupDeployment](/powershell/module/az.resources/new-azresourcegroupdeployment) y [az group deployment create](/cli/azure/group/deployment#az-group-deployment-create) son ejemplos de cómo iniciar la implementación en el ámbito de un grupo de recursos.

```azurepowershell
New-AzResourceGroupDeployment -ResourceGroupName ExampleGroup -TemplateFile rbac-test.json -principalId $objectid -builtInRoleType Reader
```

```azurecli
az group deployment create --resource-group ExampleGroup --template-file rbac-test.json --parameters principalId=$objectid builtInRoleType=Reader
```

Los comandos [New-AzDeployment](/powershell/module/az.resources/new-azdeployment) y [az deployment create](/cli/azure/deployment#az-deployment-create) son ejemplos de cómo iniciar la implementación en el ámbito de una suscripción y especificar la ubicación.

```azurepowershell
New-AzDeployment -Location centralus -TemplateFile rbac-test.json -principalId $objectid -builtInRoleType Reader
```

```azurecli
az deployment create --location centralus --template-file rbac-test.json --parameters principalId=$objectid builtInRoleType=Reader
```

### <a name="resource"></a>Resource

Si necesita agregar una asignación de roles en el nivel de un recurso, el formato de la asignación de roles es diferente. Proporcione el espacio de nombres del proveedor de recursos y el tipo de recurso del recurso al que se va a asignar el rol. Incluya también el nombre del recurso en el nombre de la asignación de roles.

Para el tipo y el nombre de la asignación de roles, use el siguiente formato:

```json
"type": "{resource-provider-namespace}/{resource-type}/providers/roleAssignments",
"name": "{resource-name}/Microsoft.Authorization/{role-assign-GUID}"
```

La plantilla siguiente muestra:

- Procedimientos para crear una cuenta de almacenamiento
- Cómo asignar un rol a un usuario, un grupo o una aplicación, en el ámbito de una cuenta de almacenamiento
- Cómo especificar los roles de propietario, colaborador y lector como parámetro

Para usar la plantilla, debe especificar las siguientes entradas:

- El identificador de un usuario, de un grupo o de una aplicación al que se asignará el rol.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "principalId": {
            "type": "string",
            "metadata": {
                "description": "The principal to assign the role to"
            }
        },
        "builtInRoleType": {
            "type": "string",
            "allowedValues": [
                "Owner",
                "Contributor",
                "Reader"
            ],
            "metadata": {
                "description": "Built-in role to assign"
            }
        },
        "location": {
            "type": "string",
            "defaultValue": "[resourceGroup().location]"
        }
    },
    "variables": {
        "Owner": "[concat('/subscriptions/', subscription().subscriptionId, '/providers/Microsoft.Authorization/roleDefinitions/', '8e3af657-a8ff-443c-a75c-2fe8c4bcb635')]",
        "Contributor": "[concat('/subscriptions/', subscription().subscriptionId, '/providers/Microsoft.Authorization/roleDefinitions/', 'b24988ac-6180-42a0-ab88-20f7382dd24c')]",
        "Reader": "[concat('/subscriptions/', subscription().subscriptionId, '/providers/Microsoft.Authorization/roleDefinitions/', 'acdd72a7-3385-48ef-bd42-f606fba81ae7')]",
        "storageName": "[concat('storage', uniqueString(resourceGroup().id))]"
    },
    "resources": [
        {
            "apiVersion": "2019-04-01",
            "type": "Microsoft.Storage/storageAccounts",
            "name": "[variables('storageName')]",
            "location": "[parameters('location')]",
            "sku": {
                "name": "Standard_LRS"
            },
            "kind": "Storage",
            "properties": {}
        },
        {
            "type": "Microsoft.Storage/storageAccounts/providers/roleAssignments",
            "apiVersion": "2018-09-01-preview",
            "name": "[concat(variables('storageName'), '/Microsoft.Authorization/', guid(uniqueString(variables('storageName'))))]",
            "dependsOn": [
                "[variables('storageName')]"
            ],
            "properties": {
                "roleDefinitionId": "[variables(parameters('builtInRoleType'))]",
                "principalId": "[parameters('principalId')]"
            }
        }
    ]
}
```

Para implementar la plantilla anterior, usará los comandos de grupo de recursos. Los comandos [New-AzResourceGroupDeployment](/powershell/module/az.resources/new-azresourcegroupdeployment) y [az group deployment create](/cli/azure/group/deployment#az-group-deployment-create) son ejemplos de cómo iniciar la implementación en el ámbito de un recurso.

```azurepowershell
New-AzResourceGroupDeployment -ResourceGroupName ExampleGroup -TemplateFile rbac-test.json -principalId $objectid -builtInRoleType Contributor
```

```azurecli
az group deployment create --resource-group ExampleGroup --template-file rbac-test.json --parameters principalId=$objectid builtInRoleType=Contributor
```

A continuación se muestra un ejemplo de una asignación de rol de colaborador a un usuario de una cuenta de almacenamiento después de implementar la plantilla.

![Asignación de roles en el ámbito de un recurso](./media/role-assignments-template/role-assignment-template-resource.png)

### <a name="new-service-principal"></a>Nueva entidad de servicio

Si crea una entidad de servicio e inmediatamente intenta asignarle un rol, esa asignación de roles puede producir un error en algunos casos. Por ejemplo, si crea una identidad administrada y luego intenta asignarle un rol en la misma plantilla de Azure Resource Manager, la asignación de roles podría producir un error. Es probable que el motivo de este error sea un retraso en la replicación. La entidad de servicio se crea en una región; sin embargo, la asignación de roles puede tener lugar en una región distinta que todavía no haya replicado la entidad de servicio. Para abordar este escenario, debe establecer la propiedad `principalType` en `ServicePrincipal` al crear la asignación de roles.

La plantilla siguiente muestra:

- Cómo crear una entidad de servicio de identidad administrada
- Cómo especificar `principalType`
- Cómo asignar el rol de colaborador a esa entidad de servicio en el ámbito de un grupo de recursos

Para usar la plantilla, debe especificar las siguientes entradas:

- El nombre base de la identidad administrada, o puede usar la cadena predeterminada.

```json
{
    "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "baseName": {
            "type": "string",
            "defaultValue": "msi-test"
        }
    },
    "variables": {
        "identityName": "[concat(parameters('baseName'), '-bootstrap')]",
        "bootstrapRoleAssignmentId": "[guid(concat(resourceGroup().id, 'contributor'))]",
        "contributorRoleDefinitionId": "[concat('/subscriptions/', subscription().subscriptionId, '/providers/Microsoft.Authorization/roleDefinitions/', 'b24988ac-6180-42a0-ab88-20f7382dd24c')]"
    },
    "resources": [
        {
            "type": "Microsoft.ManagedIdentity/userAssignedIdentities",
            "name": "[variables('identityName')]",
            "apiVersion": "2018-11-30",
            "location": "[resourceGroup().location]"
        },
        {
            "type": "Microsoft.Authorization/roleAssignments",
            "apiVersion": "2018-09-01-preview",
            "name": "[variables('bootstrapRoleAssignmentId')]",
            "dependsOn": [
                "[resourceId('Microsoft.ManagedIdentity/userAssignedIdentities', variables('identityName'))]"
            ],
            "properties": {
                "roleDefinitionId": "[variables('contributorRoleDefinitionId')]",
                "principalId": "[reference(resourceId('Microsoft.ManagedIdentity/userAssignedIdentities', variables('identityName')), '2018-11-30').principalId]",
                "scope": "[resourceGroup().id]",
                "principalType": "ServicePrincipal"
            }
        }
    ]
}
```

Los comandos [New-AzResourceGroupDeployment](/powershell/module/az.resources/new-azresourcegroupdeployment) y [az group deployment create](/cli/azure/group/deployment#az-group-deployment-create) son ejemplos de cómo iniciar la implementación en el ámbito de un grupo de recursos.

```azurepowershell
New-AzResourceGroupDeployment -ResourceGroupName ExampleGroup2 -TemplateFile rbac-test.json
```

```azurecli
az group deployment create --resource-group ExampleGroup2 --template-file rbac-test.json
```

A continuación se muestra un ejemplo de la asignación del rol de colaborador a una nueva entidad de servicio de identidad administrada después de implementar la plantilla.

![Asignación de roles para una nueva entidad de servicio de identidad administrada](./media/role-assignments-template/role-assignment-template-msi.png)

## <a name="next-steps"></a>Pasos siguientes

- [Guía de inicio rápido: Creación e implementación de plantillas de Azure Resource Manager mediante Azure Portal](../azure-resource-manager/templates/quickstart-create-templates-use-the-portal.md)
- [Nociones sobre la estructura y la sintaxis de las plantillas de Azure Resource Manager](../azure-resource-manager/templates/template-syntax.md)
- [Creación de grupos de recursos y otros recursos en el nivel de suscripción](../azure-resource-manager/templates/deploy-to-subscription.md)
- [Plantillas de inicio rápido de Azure](https://azure.microsoft.com/resources/templates/?term=rbac)
