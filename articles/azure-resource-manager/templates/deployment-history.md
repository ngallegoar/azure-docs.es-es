---
title: Historial de implementación
description: Se describe cómo ver las operaciones de implementación de Azure Resource Manager con el portal, PowerShell, CLI de Azure y API de REST.
tags: top-support-issue
ms.topic: conceptual
ms.date: 11/26/2019
ms.openlocfilehash: b0f196f86bed05094b04bfc20c7cef2248a91c65
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/28/2020
ms.locfileid: "79460303"
---
# <a name="view-deployment-history-with-azure-resource-manager"></a>Visualización del historial de implementación con Azure Resource Manager

Azure Resource Manager permite ver el historial de implementación y examinar operaciones específicas de implementaciones anteriores. Puede ver los recursos que se han implementado y obtener información sobre los errores.

Para obtener ayuda con la resolución de errores de implementación concretos, consulte [Solución de problemas comunes al implementar recursos en Azure con Azure Resource Manager](common-deployment-errors.md).

## <a name="get-deployments-and-correlation-id"></a>Obtención de las implementaciones y el Id. de correlación

Se pueden ver los detalles sobre una implementación mediante Azure Portal, PowerShell, la CLI de Azure o la API REST. Cada implementación tiene un id. de correlación que se usa para realizar el seguimiento de los eventos relacionados. Puede resultar útil al trabajar con el soporte técnico para solucionar problemas de una implementación.

# <a name="portal"></a>[Portal](#tab/azure-portal)

1. Seleccione el grupo de recursos que quiera examinar.

1. Seleccione el vínculo situado bajo **Implementaciones**.

   ![Selección del historial de implementación](./media/deployment-history/select-deployment-history.png)

1. Seleccione una de las implementaciones del historial de implementación.

   ![Selección de la implementación](./media/deployment-history/select-details.png)

1. Se muestra un resumen de la implementación, incluido el id. de correlación.

    ![Resumen de implementación](./media/deployment-history/show-correlation-id.png)

# <a name="powershell"></a>[PowerShell](#tab/azure-powershell)

Para enumerar todas las implementaciones de un grupo de recursos, use el comando [Get-AzResourceGroupDeployment](/powershell/module/az.resources/Get-AzResourceGroupDeployment).

```azurepowershell-interactive
Get-AzResourceGroupDeployment -ResourceGroupName ExampleGroup
```

Para obtener una implementación específica de un grupo de recursos, agregue el parámetro **DeploymentName**.

```azurepowershell-interactive
Get-AzResourceGroupDeployment -ResourceGroupName ExampleGroup -DeploymentName ExampleDeployment
```

Para obtener el Id. de correlación, use:

```azurepowershell-interactive
(Get-AzResourceGroupDeployment -ResourceGroupName ExampleGroup -DeploymentName ExampleDeployment).CorrelationId
```

# <a name="azure-cli"></a>[CLI de Azure](#tab/azure-cli)

Para mostrar la implementación de un grupo de recursos, use [az deployment group list](/cli/azure/group/deployment?view=azure-cli-latest#az-deployment-group-list).

```azurecli-interactive
az deployment group list --resource-group ExampleGroup
```

Para obtener una implementación concreta, use [az deployment group show](/cli/azure/group/deployment?view=azure-cli-latest#az-deployment-group-show).

```azurecli-interactive
az deployment group show --resource-group ExampleGroup --name ExampleDeployment
```

Para obtener el Id. de correlación, use:

```azurecli-interactive
az deployment group show --resource-group ExampleGroup --name ExampleDeployment --query properties.correlationId
```

# <a name="http"></a>[HTTP](#tab/http)

Para enumerar las implementaciones de un grupo de recursos, use la operación siguiente. Para obtener el número de versión de la API más reciente que se va a usar en la solicitud, vea [Implementaciones: mostrar por grupo de recursos](/rest/api/resources/deployments/listbyresourcegroup).

```
GET https://management.azure.com/subscriptions/{subscriptionId}/resourcegroups/{resourceGroupName}/providers/Microsoft.Resources/deployments/?api-version={api-version}
```

Para obtener una implementación específica. Utilice la siguiente operación. Para obtener el número de versión de la API más reciente que se va a usar en la solicitud, vea [Implementaciones: obtener](/rest/api/resources/deployments/get).

```
GET https://management.azure.com/subscriptions/{subscription-id}/resourcegroups/{resource-group-name}/providers/microsoft.resources/deployments/{deployment-name}?api-version={api-version}
```

La respuesta incluye el id. de correlación.

```json
{
 ...
 "properties": {
   "mode": "Incremental",
   "provisioningState": "Failed",
   "timestamp": "2019-11-26T14:18:36.4518358Z",
   "duration": "PT26.2091817S",
   "correlationId": "47ff4228-bf2e-4ee5-a008-0b07da681230",
   ...
 }
}
```

---

## <a name="get-deployment-operations-and-error-message"></a>Obtención de operaciones de implementación y mensajes de error

Cada implementación puede incluir varias operaciones. Para ver más detalles acerca de una implementación, vea las operaciones de implementación. Cuando se produce un error en una implementación, las operaciones de implementación incluyen un mensaje de error.

# <a name="portal"></a>[Portal](#tab/azure-portal)

1. En el Resumen de una implementación, seleccione **Detalles de la operación**.

    ![Selección de operaciones de implementación](./media/deployment-history/get-operation-details.png)

1. Verá los detalles de ese paso de la implementación. Cuando se produce un error, los detalles incluyen el mensaje de error.

    ![Representación de los detalles de la operación](./media/deployment-history/see-operation-details.png)

# <a name="powershell"></a>[PowerShell](#tab/azure-powershell)

Con el fin de ver las operaciones de implementación para la implementación en un grupo de recursos, use el comando [Get-AzResourceGroupDeploymentOperation](/powershell/module/az.resources/get-azdeploymentoperation).

```azurepowershell-interactive
Get-AzResourceGroupDeploymentOperation -ResourceGroupName ExampleGroup -DeploymentName ExampleDeploy
```

Para ver las operaciones con errores, filtre las operaciones con el estado **Erróneas**.

```azurepowershell-interactive
(Get-AzResourceGroupDeploymentOperation -ResourceGroupName ExampleGroup -DeploymentName ExampleDeploy).Properties | Where-Object ProvisioningState -eq Failed
```

Para obtener el mensaje de estado de las operaciones erróneas, use el comando siguiente:

```azurepowershell-interactive
((Get-AzResourceGroupDeploymentOperation -ResourceGroupName ExampleGroup -DeploymentName ExampleDeploy ).Properties | Where-Object ProvisioningState -eq Failed).StatusMessage.error
```

# <a name="azure-cli"></a>[CLI de Azure](#tab/azure-cli)

Con el fin de ver las operaciones de implementación para la implementación en un grupo de recursos, use el comando [az deployment group operation list](/cli/azure/group/deployment/operation?view=azure-cli-latest#az-deployment-group-operation-list).

```azurecli-interactive
az deployment group operation list --resource-group ExampleGroup --name ExampleDeployment
```

Para ver las operaciones con errores, filtre las operaciones con el estado **Erróneas**.

```azurecli-interactive
az deployment group operation list --resource-group ExampleGroup --name ExampleDeploy --query "[?properties.provisioningState=='Failed']"
```

Para obtener el mensaje de estado de las operaciones erróneas, use el comando siguiente:

```azurecli-interactive
az deployment group operation list --resource-group ExampleGroup --name ExampleDeploy --query "[?properties.provisioningState=='Failed'].properties.statusMessage.error"
```

# <a name="http"></a>[HTTP](#tab/http)

Para obtener las operaciones de implementación, use la operación siguiente. Para obtener el número de versión de la API más reciente que se va a usar en la solicitud, vea [Operaciones de implementación: mostrar](/rest/api/resources/deploymentoperations/list).

```
GET https://management.azure.com/subscriptions/{subscription-id}/resourcegroups/{resource-group-name}/providers/microsoft.resources/deployments/{deployment-name}/operations?$skiptoken={skiptoken}&api-version={api-version}
```

La respuesta incluye un mensaje de error.

```json
{
  "value": [
    {
      "id": "/subscriptions/xxxx/resourceGroups/examplegroup/providers/Microsoft.Resources/deployments/exampledeploy/operations/13EFD9907103D640",
      "operationId": "13EFD9907103D640",
      "properties": {
        "provisioningOperation": "Create",
        "provisioningState": "Failed",
        "timestamp": "2019-11-26T14:18:36.3177613Z",
        "duration": "PT21.0580179S",
        "trackingId": "9d3cdac4-54f8-486c-94bd-10c20867b8bc",
        "serviceRequestId": "01a9d0fe-896b-4c94-a30f-60b70a8f1ad9",
        "statusCode": "BadRequest",
        "statusMessage": {
          "error": {
            "code": "InvalidAccountType",
            "message": "The AccountType Standard_LRS1 is invalid. For more information, see - https://aka.ms/storageaccountskus"
          }
        },
        "targetResource": {
          "id": "/subscriptions/xxxx/resourceGroups/examplegroup/providers/Microsoft.Storage/storageAccounts/storageq2czadzfgizc2",
          "resourceType": "Microsoft.Storage/storageAccounts",
          "resourceName": "storageq2czadzfgizc2"
        }
      }
    },
    ...
  ]
}
```

---

## <a name="next-steps"></a>Pasos siguientes

* Para obtener ayuda con la resolución de errores de implementación concretos, consulte [Solución de problemas comunes al implementar recursos en Azure con Azure Resource Manager](common-deployment-errors.md).
* Para obtener información sobre cómo usar los registros de actividad para supervisar otros tipos de acciones, vea [View activity logs to manage Azure resources](../management/view-activity-logs.md) (Ver registros de actividad para administrar recursos de Azure).
* Para validar la implementación antes de ejecutarla, consulte [Implementación de recursos con las plantillas de Resource Manager y Azure PowerShell](deploy-powershell.md).

