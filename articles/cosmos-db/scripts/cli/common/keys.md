---
title: Trabajo con claves de cuenta y cadenas de conexión para una cuenta de Azure Cosmos
description: Trabajo con claves de cuenta y cadenas de conexión para una cuenta de Azure Cosmos
author: markjbrown
ms.author: mjbrown
ms.service: cosmos-db
ms.topic: sample
ms.date: 07/29/2020
ms.openlocfilehash: af5f92168e6bcc23345e3a11727eefe76615422c
ms.sourcegitcommit: 04fb3a2b272d4bbc43de5b4dbceda9d4c9701310
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/12/2020
ms.locfileid: "94563263"
---
# <a name="work-with-account-keys-and-connection-strings-for-an-azure-cosmos-account-using-azure-cli"></a>Trabajo con claves de cuenta y cadenas de conexión para una cuenta de Azure Cosmos con la CLI de Azure
[!INCLUDE[appliesto-all-apis](../../../includes/appliesto-all-apis.md)]

[!INCLUDE [azure-cli-prepare-your-environment.md](../../../../../includes/azure-cli-prepare-your-environment.md)]

- En este artículo se necesita la versión 2.9.1 de la CLI de Azure o cualquier versión posterior. Si usa Azure Cloud Shell, ya está instalada la versión más reciente.

## <a name="sample-script"></a>Script de ejemplo

Este script muestra cuatro operaciones.

- Enumeración de todas las claves de cuenta
- Enumeración de claves de cuenta de solo lectura
- Enumeración de cadenas de conexión
- Regeneración de claves de cuenta

> [!NOTE]
> En este ejemplo se muestra el uso de una cuenta de SQL (Core) API, pero estas operaciones de clave de cuenta y cadena de conexión son idénticas en todas las API de base de datos en Cosmos DB.

[!code-azurecli-interactive[main](../../../../../cli_scripts/cosmosdb/common/keys.sh "Keys and connection string operations for Cosmos DB.")]

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
| [az cosmosdb keys list](/cli/azure/cosmosdb/keys#az-cosmosdb-keys-list) | Enumera las claves para una cuenta de Azure Cosmos DB. |
| [az cosmosdb list-read-only-keys](/cli/azure/cosmosdb#az-cosmosdb-list-read-only-keys) | Enumera las claves de solo lectura de una cuenta de Azure Cosmos DB. |
| [az cosmosdb list-connection-strings](/cli/azure/cosmosdb#az-cosmosdb-list-connection-strings) | Enumera las cadenas de conexión de una cuenta de Azure Cosmos DB. |
| [az cosmosdb regenerate-key](/cli/azure/cosmosdb#az-cosmosdb-regenerate-key) | Regenera las claves de una cuenta de Azure Cosmos DB. |
| [az group delete](/cli/azure/resource#az-resource-delete) | Elimina un grupo de recursos, incluidos todos los recursos anidados. |

## <a name="next-steps"></a>Pasos siguientes

Para más información, consulte la [documentación de la CLI de Azure Cosmos DB](/cli/azure/cosmosdb).

Encontrará más ejemplos de scripts de la CLI de Azure Cosmos DB en el [repositorio de la CLI de Azure Cosmos DB en GitHub](https://github.com/Azure-Samples/azure-cli-samples/tree/master/cosmosdb).
