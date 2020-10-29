---
title: Script de PowerShell para cambiar la prioridad de conmutación por error para una cuenta de Azure Cosmos con una región de solo escritura.
description: Ejemplo de script de Azure PowerShell para cambiar la prioridad de conmutación por error o desencadenar la conmutación por error para una cuenta de Azure Cosmos con una región de solo escritura.
author: markjbrown
ms.service: cosmos-db
ms.topic: sample
ms.date: 03/18/2020
ms.author: mjbrown
ms.openlocfilehash: 05770ba1d2562ec94d8771495aa66ea69523fd06
ms.sourcegitcommit: 3bcce2e26935f523226ea269f034e0d75aa6693a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/23/2020
ms.locfileid: "92488255"
---
# <a name="change-failover-priority-or-trigger-failover-for-an-azure-cosmos-account-with-single-write-region-by-using-powershell"></a>Cambio de la prioridad de conmutación por error o desencadenamiento de la conmutación por error para una cuenta de Azure Cosmos con una región de solo escritura mediante PowerShell.

[!INCLUDE [updated-for-az](../../../../../includes/updated-for-az.md)]

[!INCLUDE [sample-powershell-install](../../../../../includes/sample-powershell-install-no-ssh.md)]

## <a name="sample-script"></a>Script de ejemplo

> [!NOTE]
> Cualquier cambio que se realice en una región con `failoverPriority=0` desencadena una conmutación por error manual y solo se puede realizar en una cuenta configurada para la conmutación por error manual. Los cambios en las restantes regiones simplemente cambian la prioridad de conmutación por error de una cuenta de Cosmos.
> [!NOTE]
> En este ejemplo se muestra el uso de una cuenta de SQL (Core) API. Para usar este ejemplo para otras API, copie las propiedades relacionadas y aplíquelas a su script específico de la API

[!code-powershell[main](../../../../../powershell_scripts/cosmosdb/common/ps-account-failover-priority-update.ps1 "Update failover priority for an Azure Cosmos account or trigger a manual failover")]

## <a name="clean-up-deployment"></a>Limpieza de la implementación

Después de ejecutar el script de ejemplo, se puede usar el comando siguiente para quitar el grupo de recursos y todos los recursos asociados.

```powershell
Remove-AzResourceGroup -ResourceGroupName "myResourceGroup"
```

## <a name="script-explanation"></a>Explicación del script

Este script usa los siguientes comandos. Cada comando de la tabla crea un vínculo a documentación específica del comando.

| Get-Help | Notas |
|---|---|
|**Azure Cosmos DB**| |
| [Get-AzCosmosDBAccount](/powershell/module/az.cosmosdb/get-azcosmosdbaccount) | Enumera cuentas de Cosmos DB u obtiene una cuenta de Cosmos DB especificada. |
| [Update-AzCosmosDBAccountFailoverPriority](/powershell/module/az.cosmosdb/update-azcosmosdbaccountfailoverpriority) | Actualice el orden de prioridad de la conmutación por error de las regiones de una cuenta de Cosmos DB. |
|**Grupos de recursos de Azure**| |
| [Remove-AzResourceGroup](/powershell/module/az.resources/remove-azresourcegroup) | Elimina un grupo de recursos, incluidos todos los recursos anidados. |
|||

## <a name="next-steps"></a>Pasos siguientes

Para más información sobre Azure PowerShell, consulte la [documentación de Azure PowerShell](/powershell/).