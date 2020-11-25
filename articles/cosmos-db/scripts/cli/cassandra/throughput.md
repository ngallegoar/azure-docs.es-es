---
title: Scripts de la CLI de Azure para operaciones de rendimiento (RU/s) para recursos de Cassandra API de Azure Cosmos DB
description: Scripts de la CLI de Azure para operaciones de rendimiento (RU/s) para recursos de Cassandra API de Azure Cosmos DB
author: markjbrown
ms.author: mjbrown
ms.service: cosmos-db
ms.subservice: cosmosdb-cassandra
ms.topic: sample
ms.date: 10/07/2020
ms.openlocfilehash: 1979c59af53ebeccdbd7c910a87fb4c2536fe403
ms.sourcegitcommit: 04fb3a2b272d4bbc43de5b4dbceda9d4c9701310
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/12/2020
ms.locfileid: "94565592"
---
# <a name="throughput-rus-operations-with-azure-cli-for-a-keyspace-or-table-for-azure-cosmos-db---cassandra-api"></a>Operaciones de rendimiento (RU/s) con la CLI de Azure para un espacio de claves o una tabla para Cassandra API de Azure Cosmos DB
[!INCLUDE[appliesto-cassandra-api](../../../includes/appliesto-cassandra-api.md)]

[!INCLUDE [azure-cli-prepare-your-environment.md](../../../../../includes/azure-cli-prepare-your-environment.md)]

- En este artículo se necesita la versión 2.12.1 de la CLI de Azure o cualquier versión posterior. Si usa Azure Cloud Shell, ya está instalada la versión más reciente.

## <a name="sample-script"></a>Script de ejemplo

Este script crea un espacio de claves de Cassandra con una capacidad de proceso compartida y una tabla de Cassandra con una capacidad de proceso dedicada y, a continuación, actualiza la capacidad de proceso del espacio de claves y la tabla. A continuación, el script migra del rendimiento estándar a escalado automático y, a continuación, lee el valor del rendimiento de escalado automático una vez que se ha migrado.

[!code-azurecli-interactive[main](../../../../../cli_scripts/cosmosdb/cassandra/throughput.sh "Throughput operations for Cassandra keyspace and table.")]

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
| [az cosmosdb cassandra keyspace create](/cli/azure/cosmosdb/cassandra/keyspace#az-cosmosdb-cassandra-keyspace-create) | Crea un espacio de claves de Cassandra para Azure Cosmos. |
| [az cosmosdb cassandra table create](/cli/azure/cosmosdb/cassandra/table#az-cosmosdb-cassandra-table-create) | Crea una tabla de Cassandra para Azure Cosmos. |
| [az cosmosdb cassandra keyspace throughput update](/cli/azure/cosmosdb/cassandra/keyspace/throughput#az-cosmosdb-cassandra-keyspace-throughput-update) | Actualiza la capacidad de proceso (RU/s) de un espacio de claves de Cassandra para Azure Cosmos. |
| [az cosmosdb cassandra table throughput update](/cli/azure/cosmosdb/cassandra/table/throughput#az-cosmosdb-cassandra-table-throughput-update) | Actualiza la capacidad de proceso (RU/s) de una tabla de Cassandra para Azure Cosmos. |
| [az cosmosdb cassandra keyspace throughput migrate](/cli/azure/cosmosdb/cassandra/keyspace/throughput#az_cosmosdb_cassandra_keyspace_throughput_migrate) | Migra el rendimiento de un espacio de claves de Cassandra de Azure Cosmos. |
| [az cosmosdb cassandra table throughput migrate](/cli/azure/cosmosdb/cassandra/table/throughput#az_cosmosdb_cassandra_table_throughput_migrate) | Migra el rendimiento de una tabla de Cassandra de Azure Cosmos. |
| [az group delete](/cli/azure/resource#az-resource-delete) | Elimina un grupo de recursos, incluidos todos los recursos anidados. |

## <a name="next-steps"></a>Pasos siguientes

Para más información, consulte la [documentación de la CLI de Azure Cosmos DB](/cli/azure/cosmosdb).

Encontrará más ejemplos de scripts de la CLI de Azure Cosmos DB en el [repositorio de la CLI de Azure Cosmos DB en GitHub](https://github.com/Azure-Samples/azure-cli-samples/tree/master/cosmosdb).
