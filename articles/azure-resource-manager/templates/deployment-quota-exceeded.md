---
title: Cuota de implementaciones superada
description: En este artículo se describe cómo resolver el error de tener más de 800 implementaciones en el historial del grupo de recursos.
ms.topic: troubleshooting
ms.date: 10/04/2019
ms.openlocfilehash: 919cd9a3482401cd47516e2677b0bf58387488b0
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "80245096"
---
# <a name="resolve-error-when-deployment-count-exceeds-800"></a>Resolución de error cuando el recuento de implementaciones es superior a 800

Cada grupo de recursos está limitado a 800 implementaciones en su historial de implementaciones. En este artículo se describe el error que se recibe cuando se produce un error en una implementación porque se superarían las 800 implementaciones permitidas. Para resolver este error, elimine implementaciones del historial del grupo de recursos. La eliminación de una implementación del historial no afecta a ninguno de los recursos implementados.

## <a name="symptom"></a>Síntoma

Durante una implementación, se recibe un error que indica que la implementación actual superará la cuota de 800 implementaciones.

## <a name="solution"></a>Solución

### <a name="azure-cli"></a>Azure CLI

Utilice el comando [az deployment group delete](/cli/azure/group/deployment) para eliminar implementaciones del historial.

```azurecli-interactive
az deployment group delete --resource-group exampleGroup --name deploymentName
```

Para eliminar todas las implementaciones de más de cinco días, use:

```azurecli-interactive
startdate=$(date +%F -d "-5days")
deployments=$(az deployment group list --resource-group exampleGroup --query "[?properties.timestamp<'$startdate'].name" --output tsv)

for deployment in $deployments
do
  az deployment group delete --resource-group exampleGroup --name $deployment
done
```

Puede obtener el recuento actual del historial de implementaciones con el siguiente comando:

```azurecli-interactive
az deployment group list --resource-group exampleGroup --query "length(@)"
```

### <a name="azure-powershell"></a>Azure PowerShell

Utilice el comando [Remove-AzResourceGroupDeployment](/powershell/module/az.resources/remove-azresourcegroupdeployment) para eliminar implementaciones del historial.

```azurepowershell-interactive
Remove-AzResourceGroupDeployment -ResourceGroupName exampleGroup -Name deploymentName
```

Para eliminar todas las implementaciones de más de cinco días, use:

```azurepowershell-interactive
$deployments = Get-AzResourceGroupDeployment -ResourceGroupName exampleGroup | Where-Object Timestamp -lt ((Get-Date).AddDays(-5))

foreach ($deployment in $deployments) {
  Remove-AzResourceGroupDeployment -ResourceGroupName exampleGroup -Name $deployment.DeploymentName
}
```

Puede obtener el recuento actual del historial de implementaciones con el siguiente comando:

```azurepowershell-interactive
(Get-AzResourceGroupDeployment -ResourceGroupName exampleGroup).Count
```

## <a name="third-party-solutions"></a>Soluciones de terceros

Las siguientes soluciones externas abordan escenarios específicos:

* [Soluciones de Azure Logic Apps y PowerShell](https://devkimchi.com/2018/05/30/managing-excessive-arm-deployment-histories-with-logic-apps/)
* [Extensión AzDevOps](https://github.com/christianwaha/AzureDevOpsExtensionCleanRG)
