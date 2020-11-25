---
title: Adición de regiones, cambio de la prioridad de conmutación por error o desencadenamiento de la conmutación por error de una cuenta de Azure Cosmos
description: Adición de regiones, cambio de la prioridad de conmutación por error o desencadenamiento de la conmutación por error de una cuenta de Azure Cosmos
author: markjbrown
ms.author: mjbrown
ms.service: cosmos-db
ms.topic: sample
ms.date: 07/29/2020
ms.openlocfilehash: 0ae724f9900eea713af7d295eba26bf03cdf471a
ms.sourcegitcommit: 04fb3a2b272d4bbc43de5b4dbceda9d4c9701310
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/12/2020
ms.locfileid: "94563246"
---
# <a name="add-regions-change-failover-priority-trigger-failover-for-an-azure-cosmos-account-using-azure-cli"></a>Adición de regiones, cambio de la prioridad de conmutación por error o desencadenamiento de la conmutación por error de una cuenta de Azure Cosmos con la CLI de Azure
[!INCLUDE[appliesto-all-apis](../../../includes/appliesto-all-apis.md)]

[!INCLUDE [azure-cli-prepare-your-environment.md](../../../../../includes/azure-cli-prepare-your-environment.md)]

- En este artículo se necesita la versión 2.9.1 de la CLI de Azure o cualquier versión posterior. Si usa Azure Cloud Shell, ya está instalada la versión más reciente.

## <a name="sample-script"></a>Script de ejemplo

Este script muestra tres operaciones.

- Agregar una región a una cuenta de Azure Cosmos existente.
- Cambiar la prioridad de la conmutación por error regional (se aplica a las cuentas que usan la conmutación automática por error).
- Desencadenar una conmutación por error manual de las regiones principales a las secundarias (se aplica a las cuentas que usan conmutación por error manual).

> [!NOTE]
> Las operaciones de agregar y quitar regiones en una cuenta de Cosmos no se pueden realizar mientras se cambian otras propiedades.

> [!NOTE]
> En este ejemplo se muestra el uso de una cuenta de SQL (Core) API, pero estas operaciones son idénticas en todas las API de base de datos en Cosmos DB.

[!code-azurecli-interactive[main](../../../../../cli_scripts/cosmosdb/common/regions.sh "Regional operations for Cosmos DB.")]

## <a name="clean-up-deployment"></a>Limpieza de la implementación

Después de ejecutar el script de ejemplo, se puede usar el comando siguiente para quitar el grupo de recursos y todos los recursos asociados.

```azurecli-interactive
az group delete --name $resourceGroupName
```

## <a name="script-explanation"></a>Explicación del script

Este script usa los siguientes comandos. Cada comando de la tabla crea un vínculo a documentación específica del comando.

| Get-Help | Notas |
|---|---|
| [az group create](/cli/azure/group#az-group-create) | Crea un grupo de recursos en el que se almacenan todos los recursos. |
| [az cosmosdb create](/cli/azure/cosmosdb#az-cosmosdb-create) | Crea una cuenta de Azure Cosmos DB. |
| [az cosmosdb update](/cli/azure/cosmosdb#az-cosmosdb-update) | Actualiza una cuenta de Azure Cosmos DB (agregar o quitar una región). |
| [az cosmosdb failover-priority-change](/cli/azure/cosmosdb#az-cosmosdb-failover-priority-change) | Actualiza la prioridad de la conmutación por error o desencadena la conmutación por error en una cuenta de Azure Cosmos DB. |
| [az group delete](/cli/azure/resource#az-resource-delete) | Elimina un grupo de recursos, incluidos todos los recursos anidados. |

## <a name="next-steps"></a>Pasos siguientes

Para más información, consulte la [documentación de la CLI de Azure Cosmos DB](/cli/azure/cosmosdb).

Encontrará más ejemplos de scripts de la CLI de Azure Cosmos DB en el [repositorio de la CLI de Azure Cosmos DB en GitHub](https://github.com/Azure-Samples/azure-cli-samples/tree/master/cosmosdb).
