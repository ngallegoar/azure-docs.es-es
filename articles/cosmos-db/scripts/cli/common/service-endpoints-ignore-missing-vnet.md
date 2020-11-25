---
title: Conexión de una cuenta de Azure Cosmos existente con puntos de conexión de servicio de red virtual
description: Conexión de una cuenta de Azure Cosmos existente con puntos de conexión de servicio de red virtual
author: markjbrown
ms.author: mjbrown
ms.service: cosmos-db
ms.topic: sample
ms.date: 07/29/2020
ms.openlocfilehash: f66d219eff1919e62088e5c4f7aa72aab97ea4f9
ms.sourcegitcommit: 04fb3a2b272d4bbc43de5b4dbceda9d4c9701310
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/12/2020
ms.locfileid: "94566238"
---
# <a name="connect-an-existing-azure-cosmos-account-with-virtual-network-service-endpoints-using-azure-cli"></a>Conexión de una cuenta existente de Azure Cosmos con puntos de conexión de servicio de red virtual con la CLI de Azure
[!INCLUDE[appliesto-all-apis](../../../includes/appliesto-all-apis.md)]

[!INCLUDE [azure-cli-prepare-your-environment.md](../../../../../includes/azure-cli-prepare-your-environment.md)]

- En este artículo se necesita la versión 2.9.1 de la CLI de Azure o cualquier versión posterior. Si usa Azure Cloud Shell, ya está instalada la versión más reciente.

## <a name="sample-script"></a>Script de ejemplo

Este ejemplo está diseñado para mostrar cómo conectar una cuenta de Azure Cosmos existente a una nueva red virtual existente en la que aún no se ha configurado la subred para los puntos de conexión de servicio mediante el parámetro `ignore-missing-vnet-service-endpoint`. Esto permite que la configuración de la cuenta de Cosmos se complete sin errores antes de que se complete la configuración de la subred de la red virtual. Una vez completada la configuración de la subred, se podrá acceder a la cuenta de Cosmos mediante la subred configurada.

> [!NOTE]
> En este ejemplo se muestra el uso de una cuenta de SQL (Core) API. Para usar este ejemplo para otras API, aplique los parámetros `enable-virtual-network` y `virtual-network-rules` del script siguiente al script específico de la API.

[!code-azurecli-interactive[main](../../../../../cli_scripts/cosmosdb/common/service-endpoints-ignore-missing-vnet.sh "Create an Azure Cosmos account with service endpoints.")]

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
| [az network vnet create](/cli/azure/network/vnet#az-network-vnet-create) | Crea una red virtual de Azure. |
| [az network vnet subnet create](/cli/azure/network/vnet/subnet#az-network-vnet-subnet-create) | Crea una subred para una red virtual de Azure. |
| [az network vnet subnet show](/cli/azure/network/vnet/subnet#az-network-vnet-subnet-show) | Devuelve una subred para una red virtual de Azure. |
| [az cosmosdb create](/cli/azure/cosmosdb#az-cosmosdb-create) | Crea una cuenta de Azure Cosmos DB. |
| [az network vnet subnet update](/cli/azure/network/vnet/subnet#az-network-vnet-subnet-update) | Actualiza una subred de una red virtual de Azure. |
| [az group delete](/cli/azure/resource#az-resource-delete) | Elimina un grupo de recursos, incluidos todos los recursos anidados. |

## <a name="next-steps"></a>Pasos siguientes

Para más información, consulte la [documentación de la CLI de Azure Cosmos DB](/cli/azure/cosmosdb).

Encontrará más ejemplos de scripts de la CLI de Azure Cosmos DB en el [repositorio de la CLI de Azure Cosmos DB en GitHub](https://github.com/Azure-Samples/azure-cli-samples/tree/master/cosmosdb).
