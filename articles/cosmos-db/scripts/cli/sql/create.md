---
title: Creación de una base de datos y un contenedor de Core (SQL) API para Azure Cosmos DB
description: Creación de una base de datos y un contenedor de Core (SQL) API para Azure Cosmos DB
author: markjbrown
ms.author: mjbrown
ms.service: cosmos-db
ms.subservice: cosmosdb-sql
ms.topic: sample
ms.date: 07/29/2020
ms.openlocfilehash: e181100d664d02193d248c8f925503523bbaa129
ms.sourcegitcommit: 04fb3a2b272d4bbc43de5b4dbceda9d4c9701310
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/12/2020
ms.locfileid: "94566850"
---
# <a name="create-an-azure-cosmos-core-sql-api-account-database-and-container-using-azure-cli"></a>Creación de una cuenta, una base de datos y un contenedor de Core (SQL) API para Azure Cosmos DB con la CLI de Azure
[!INCLUDE[appliesto-sql-api](../../../includes/appliesto-sql-api.md)]

[!INCLUDE [azure-cli-prepare-your-environment.md](../../../../../includes/azure-cli-prepare-your-environment.md)]

- En este artículo se necesita la versión 2.9.1 de la CLI de Azure o cualquier versión posterior. Si usa Azure Cloud Shell, ya está instalada la versión más reciente.

## <a name="sample-script"></a>Script de ejemplo

[!code-azurecli-interactive[main](../../../../../cli_scripts/cosmosdb/sql/create.sh "Create an Azure Cosmos DB SQL (Core) API account, database, and container.")]

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
| [az cosmosdb sql database create](/cli/azure/cosmosdb/sql/database#az-cosmosdb-sql-database-create) | Crea una base de datos de SQL (Core) para Azure Cosmos. |
| [az cosmosdb sql container create](/cli/azure/cosmosdb/sql/container#az-cosmosdb-sql-container-create) | Crea un contenedor de SQL (Core) para Azure Cosmos. |
| [az group delete](/cli/azure/resource#az-resource-delete) | Elimina un grupo de recursos, incluidos todos los recursos anidados. |

## <a name="next-steps"></a>Pasos siguientes

Para más información, consulte la [documentación de la CLI de Azure Cosmos DB](/cli/azure/cosmosdb).

Encontrará más ejemplos de scripts de la CLI de Azure Cosmos DB en el [repositorio de la CLI de Azure Cosmos DB en GitHub](https://github.com/Azure-Samples/azure-cli-samples/tree/master/cosmosdb).
