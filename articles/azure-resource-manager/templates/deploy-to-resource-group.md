---
title: Implementación de recursos en grupos de recursos
description: Se describe cómo implementar recursos en una plantilla de Azure Resource Manager. Se muestra cómo seleccionar más de un grupo de recursos como destino.
ms.topic: conceptual
ms.date: 10/26/2020
ms.openlocfilehash: fd211641d7fcc02a1db154053597497583b21ae5
ms.sourcegitcommit: 4cb89d880be26a2a4531fedcc59317471fe729cd
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/27/2020
ms.locfileid: "92681337"
---
# <a name="resource-group-deployments-with-arm-templates"></a>Implementaciones de grupos de recursos con plantillas de Resource Manager

En este artículo se describe cómo limitar la implementación a un grupo de recursos. Para la implementación puede usar una plantilla de Azure Resource Manager. En el artículo también se muestra cómo expandir el ámbito más allá del grupo de recursos en la operación de implementación.

## <a name="supported-resources"></a>Recursos compatibles

La mayoría de los recursos se pueden implementar en un grupo de recursos. Para obtener una lista de los recursos disponibles, vea [Referencia de plantillas de Resource Manager](/azure/templates).

## <a name="schema"></a>Schema

Para las plantillas, usa el esquema siguiente:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentTemplate.json#",
    ...
}
```

Para los archivos de parámetros, use:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentParameters.json#",
    ...
}
```

## <a name="deployment-commands"></a>Comandos de implementación

Para implementar en un grupo de recursos, use los comandos de implementación de grupos de recursos.

# <a name="azure-cli"></a>[CLI de Azure](#tab/azure-cli)

Para la CLI de Azure, use [az deployment group create](/cli/azure/deployment/group#az_deployment_group_create). El ejemplo siguiente implementa una plantilla para crear un grupo de recursos:

```azurecli-interactive
az deployment group create \
  --name demoRGDeployment \
  --resource-group ExampleGroup \
  --template-uri "https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-storage-account-create/azuredeploy.json" \
  --parameters storageAccountType=Standard_GRS
```

# <a name="powershell"></a>[PowerShell](#tab/azure-powershell)

Para el comando de implementación de PowerShell, use [New-AzResourceGroupDeployment](/powershell/module/az.resources/new-azresourcegroupdeployment). El ejemplo siguiente implementa una plantilla para crear un grupo de recursos:

```azurepowershell-interactive
New-AzResourceGroupDeployment `
  -Name demoRGDeployment `
  -ResourceGroupName ExampleGroup `
  -TemplateUri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-storage-account-create/azuredeploy.json `
  -storageAccountType Standard_GRS `
```

---

Para obtener información más detallada sobre los comandos y las opciones de implementación para la implementación de plantillas de Resource Manager, vea:

* [Implementación de recursos con Azure Portal y plantillas de Resource Manager](deploy-portal.md)
* [Implementación de recursos con plantillas de ARM y la CLI de Azure](deploy-cli.md)
* [Implementación de recursos con las plantillas de ARM y Azure PowerShell](deploy-powershell.md)
* [Implementación de recursos con plantillas de Resource Manager y la API REST de Azure Resource Manager](deploy-rest.md)
* [Usar un botón de implementación para implementar plantillas desde el repositorio de GitHub](deploy-to-azure-button.md)
* [Implementación de plantillas de Resource Manager desde Cloud Shell](deploy-cloud-shell.md)

## <a name="deployment-scopes"></a>Ámbitos de implementación

Al implementar en un grupo de recursos, puede implementar los recursos en:

* el grupo de recursos de destino de la operación
* otros grupos de recursos de la misma suscripción o de otras
* se pueden aplicar [recursos de extensión](scope-extension-resources.md) a los recursos

El usuario que implementa la plantilla debe tener acceso al ámbito especificado.

En esta sección se muestra cómo especificar ámbitos diferentes. Puede combinar estos ámbitos diferentes en una sola plantilla.

### <a name="scope-to-target-resource-group"></a>Ámbito de destino del grupo de recursos

Para implementar recursos en el recurso de destino, agregue esos recursos a la sección de recursos de la plantilla.

:::code language="json" source="~/resourcemanager-templates/azure-resource-manager/scope/default-rg.json" highlight="5":::

### <a name="scope-to-resource-group-in-same-subscription"></a>Ámbito en el grupo de recursos de la misma suscripción

Para implementar recursos en otro grupo de recursos de la misma suscripción, agregue una implementación anidada e incluya la propiedad `resourceGroup`. Si no se especifica el grupo de recursos o el identificador de la suscripción, se utilizan los de la plantilla primaria. Todos los grupos de recursos deben existir antes de que se ejecute la implementación.

En el ejemplo siguiente, la implementación anidada tiene como destino un grupo de recursos denominado `demoResourceGroup`.

:::code language="json" source="~/resourcemanager-templates/azure-resource-manager/scope/same-sub-to-resource-group.json" highlight="9,13":::

### <a name="scope-to-resource-group-in-different-subscription"></a>Ámbito en el grupo de recursos de otra suscripción

Para implementar recursos en un grupo de recursos de otra suscripción, agregue una implementación anidada e incluya las propiedades `subscriptionId` y `resourceGroup`. En el ejemplo siguiente, la implementación anidada tiene como destino un grupo de recursos denominado `demoResourceGroup`.

:::code language="json" source="~/resourcemanager-templates/azure-resource-manager/scope/different-sub-to-resource-group.json" highlight="9,10,14":::

## <a name="cross-resource-groups"></a>Grupos de recursos cruzados

Puede implementar en más de un grupo de recursos en una misma plantilla de Resource Manager. Para establecer como destino un grupo de recursos diferente al de la plantilla primaria, use una [plantilla anidada o vinculada](linked-templates.md). En el tipo de recurso de implementación, especifique los valores para el identificador de suscripción y el grupo de recursos en que quiere realizar la implementación de la plantilla anidada. Los grupos de recursos pueden existir en suscripciones distintas.

> [!NOTE]
> Puede implementar hasta **800 grupos de recursos** en una única implementación. Normalmente, esta limitación significa que puede implementar en un grupo de recursos especificado para la plantilla primaria y hasta en 799 grupos de recursos en implementaciones anidadas o vinculadas. Sin embargo, si la plantilla principal contiene solo plantillas anidadas o vinculadas y no implementa por sí misma ningún recurso, puede incluir hasta 800 grupos de recursos en las implementaciones anidadas o vinculadas.

En el ejemplo siguiente se implementan dos cuentas de almacenamiento. La primera cuenta de almacenamiento se implementa en el grupo de recursos especificado en la operación de implementación. La segunda cuenta de almacenamiento se implementa en el grupo de recursos especificado en los parámetros `secondResourceGroup` y `secondSubscriptionID`:

:::code language="json" source="~/resourcemanager-templates/azure-resource-manager/crosssubscription.json":::

Si establece `resourceGroup` en el nombre de un grupo de recursos que no existe, la implementación generará un error.

Para probar la plantilla anterior y ver los resultados, use PowerShell o la CLI de Azure.

# <a name="powershell"></a>[PowerShell](#tab/azure-powershell)

Para implementar dos cuentas de almacenamiento en dos grupos de recursos de la **misma suscripción** , use:

```azurepowershell-interactive
$firstRG = "primarygroup"
$secondRG = "secondarygroup"

New-AzResourceGroup -Name $firstRG -Location southcentralus
New-AzResourceGroup -Name $secondRG -Location eastus

New-AzResourceGroupDeployment `
  -ResourceGroupName $firstRG `
  -TemplateUri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/crosssubscription.json `
  -storagePrefix storage `
  -secondResourceGroup $secondRG `
  -secondStorageLocation eastus
```

Para implementar dos cuentas de almacenamiento en **dos suscripciones** , use:

```azurepowershell-interactive
$firstRG = "primarygroup"
$secondRG = "secondarygroup"

$firstSub = "<first-subscription-id>"
$secondSub = "<second-subscription-id>"

Select-AzSubscription -Subscription $secondSub
New-AzResourceGroup -Name $secondRG -Location eastus

Select-AzSubscription -Subscription $firstSub
New-AzResourceGroup -Name $firstRG -Location southcentralus

New-AzResourceGroupDeployment `
  -ResourceGroupName $firstRG `
  -TemplateUri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/crosssubscription.json `
  -storagePrefix storage `
  -secondResourceGroup $secondRG `
  -secondStorageLocation eastus `
  -secondSubscriptionID $secondSub
```

# <a name="azure-cli"></a>[CLI de Azure](#tab/azure-cli)

Para implementar dos cuentas de almacenamiento en dos grupos de recursos de la **misma suscripción** , use:

```azurecli-interactive
firstRG="primarygroup"
secondRG="secondarygroup"

az group create --name $firstRG --location southcentralus
az group create --name $secondRG --location eastus
az deployment group create \
  --name ExampleDeployment \
  --resource-group $firstRG \
  --template-uri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/crosssubscription.json \
  --parameters storagePrefix=tfstorage secondResourceGroup=$secondRG secondStorageLocation=eastus
```

Para implementar dos cuentas de almacenamiento en **dos suscripciones** , use:

```azurecli-interactive
firstRG="primarygroup"
secondRG="secondarygroup"

firstSub="<first-subscription-id>"
secondSub="<second-subscription-id>"

az account set --subscription $secondSub
az group create --name $secondRG --location eastus

az account set --subscription $firstSub
az group create --name $firstRG --location southcentralus

az deployment group create \
  --name ExampleDeployment \
  --resource-group $firstRG \
  --template-uri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/crosssubscription.json \
  --parameters storagePrefix=storage secondResourceGroup=$secondRG secondStorageLocation=eastus secondSubscriptionID=$secondSub
```

---

## <a name="next-steps"></a>Pasos siguientes

* Para un ejemplo de implementación de la configuración del área de trabajo para Azure Security Center, consulte [deployASCwithWorkspaceSettings.json](https://github.com/krnese/AzureDeploy/blob/master/ARM/deployments/deployASCwithWorkspaceSettings.json).
