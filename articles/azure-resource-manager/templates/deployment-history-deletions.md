---
title: Eliminaciones del historial de implementación
description: Describe cómo Azure Resource Manager elimina automáticamente las implementaciones del historial de implementaciones. Las implementaciones se eliminan cuando el historial está próximo a superar el límite de 800.
ms.topic: conceptual
ms.date: 05/27/2020
ms.openlocfilehash: 3e48b2da00986da00f7597cf887aa74f84587710
ms.sourcegitcommit: 6a9f01bbef4b442d474747773b2ae6ce7c428c1f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/27/2020
ms.locfileid: "84122341"
---
# <a name="automatic-deletions-from-deployment-history"></a>Eliminaciones automáticas del historial de implementaciones

Cada vez que se implementa una plantilla, la información sobre la implementación se escribe en el historial de implementaciones. Cada grupo de recursos está limitado a 800 implementaciones en su historial de implementaciones.

A partir de junio de 2020, Azure Resource Manager elimina automáticamente las implementaciones del historial a medida que se acerca al límite. La eliminación automática es un cambio respecto al comportamiento anterior. Anteriormente, tenía que eliminar manualmente las implementaciones del historial de implementaciones para evitar un error.

> [!NOTE]
> La eliminación de una implementación del historial no afecta a ninguno de los recursos implementados.

## <a name="when-deployments-are-deleted"></a>Cuando las implementaciones se eliminan

Las implementaciones se eliminan del historial de implementaciones solo cuando se alcanza el límite de 800. Azure Resource Manager elimina un pequeño conjunto de las implementaciones más antiguas para liberar espacio para futuras implementaciones. La mayor parte del historial permanece sin cambios. Las implementaciones más antiguas siempre se eliminan primero.

:::image type="content" border="false" source="./media/deployment-history-deletions/deployment-history.svg" alt-text="Eliminaciones del historial de implementaciones":::

Además de las implementaciones, también se desencadenan eliminaciones al ejecutar la [operación what-if](template-deploy-what-if.md) o validar una implementación.

Cuando asigna a una implementación el mismo nombre que el de una existente en el historial, se restablece su lugar en el historial. La implementación se mueve a la posición más reciente en el historial. El lugar de una implementación también se restablece cuando se [revierte a esa implementación](rollback-on-error.md) después de un error.

## <a name="opt-out-of-automatic-deletions"></a>Rechazo de eliminaciones automáticas

Puede rechazar las eliminaciones automáticas del historial. **Use esta opción solamente si desea administrar el historial de implementaciones usted mismo.** Se sigue aplicando el límite de 800 implementaciones en el historial. Si supera las 800 implementaciones, recibirá un error y la implementación no se realizará.

Para deshabilitar las eliminaciones automáticas, registre la marca de característica `Microsoft.Resources/DisableDeploymentGrooming`. Cuando registra la marca de característica, se rechazan las eliminaciones automáticas de toda la suscripción de Azure. No puede rechazar solamente un grupo de recursos concreto.

# <a name="powershell"></a>[PowerShell](#tab/azure-powershell)

Para PowerShell, use [Register-AzProviderFeature](/powershell/module/az.resources/Register-AzProviderFeature).

```azurepowershell-interactive
Register-AzProviderFeature -ProviderNamespace Microsoft.Resources -FeatureName DisableDeploymentGrooming
```

Para ver el estado actual de la suscripción, use:

```azurepowershell-interactive
Get-AzProviderFeature -ProviderNamespace Microsoft.Resources -FeatureName DisableDeploymentGrooming
```

# <a name="azure-cli"></a>[CLI de Azure](#tab/azure-cli)

Para la CLI de Azure, use [az feature register](/cli/azure/feature#az-feature-register).

```azurecli-interactive
az feature register --namespace Microsoft.Resources --name DisableDeploymentGrooming
```

Para ver el estado actual de la suscripción, use:

```azurecli-interactive
az feature show --namespace Microsoft.Resources --name DisableDeploymentGrooming
```

# <a name="rest"></a>[REST](#tab/rest)

Para API REST, use [Características - Registrar](/rest/api/resources/features/register).

```rest
POST https://management.azure.com/subscriptions/{subscriptionId}/providers/Microsoft.Features/providers/Microsoft.Resources/features/DisableDeploymentGrooming/register?api-version=2015-12-01
```

Para ver el estado actual de la suscripción, use:

```rest
GET https://management.azure.com/subscriptions/{subscriptionId}/providers/Microsoft.Features/providers/Microsoft.Resources/features/DisableDeploymentGrooming/register?api-version=2015-12-01
```

---

## <a name="next-steps"></a>Pasos siguientes

* Para obtener información sobre cómo ver el historial de implementaciones, vea [Visualización del historial de implementación con Azure Resource Manager](deployment-history.md).
