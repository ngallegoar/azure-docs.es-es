---
title: Scripts de PowerShell para operaciones de capacidad de proceso (RU/s) para Gremlin API en Azure Cosmos DB
description: Scripts de PowerShell para operaciones de capacidad de proceso (RU/s) para Gremlin API
author: markjbrown
ms.service: cosmos-db
ms.subservice: cosmosdb-graph
ms.topic: sample
ms.date: 10/07/2020
ms.author: mjbrown
ms.openlocfilehash: c2c598c65b1277248fea545f6bb25e5bed5aedaf
ms.sourcegitcommit: b6f3ccaadf2f7eba4254a402e954adf430a90003
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/20/2020
ms.locfileid: "92280066"
---
# <a name="throughput-rus-operations-with-powershell-for-a-database-or-graph-for-azure-cosmos-db---gremlin-api"></a>Operaciones de capacidad de proceso (RU/s) con PowerShell para una base de datos o grafo de Gremlin API de Azure Cosmos DB

[!INCLUDE [updated-for-az](../../../../../includes/updated-for-az.md)]

[!INCLUDE [sample-powershell-install](../../../../../includes/sample-powershell-install-no-ssh.md)]

## <a name="get-throughput"></a>Obtención de rendimiento

[!code-powershell[main](../../../../../powershell_scripts/cosmosdb/gremlin/ps-gremlin-ru-get.ps1 "Get throughput on a database or graph for Gremlin API")]

## <a name="update-throughput"></a>Actualización del rendimiento

[!code-powershell[main](../../../../../powershell_scripts/cosmosdb/gremlin/ps-gremlin-ru-update.ps1 "Update throughput on a database or graph for Gremlin API")]

## <a name="migrate-throughput"></a>Migración de capacidad de proceso

[!code-powershell[main](../../../../../powershell_scripts/cosmosdb/gremlin/ps-gremlin-ru-migrate.ps1 "Migrate between standard and autoscale throughput on a database or graph for Gremlin API")]

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
| [Get-AzCosmosDBGremlinDatabaseThroughput](https://docs.microsoft.com/powershell/module/az.cosmosdb/get-azcosmosdbgremlindatabasethroughput) | Obtiene el valor de la capacidad de proceso de la base de datos de Gremlin API. |
| [Get-AzCosmosDBGremlinGraphThroughput](https://docs.microsoft.com/powershell/module/az.cosmosdb/get-azcosmosdbgremlingraphthroughput) | Obtiene el valor de la capacidad de proceso del grafo de Gremlin API. |
| [Update-AzCosmosDBGremlinDatabaseThroughput](https://docs.microsoft.com/powershell/module/az.cosmosdb/update-azcosmosdbgremlindatabasethroughput) | Actualiza el valor de la capacidad de proceso de la base de datos de Gremlin API. |
| [Update-AzCosmosDBGremlinGraphThroughput](https://docs.microsoft.com/powershell/module/az.cosmosdb/update-azcosmosdbgremlingraphthroughput) | Actualiza el valor de la capacidad de proceso del grafo de Gremlin API. |
| [Invoke-AzCosmosDBGremlinDatabaseThroughputMigration](https://docs.microsoft.com/powershell/module/az.cosmosdb/invoke-azcosmosdbgremlindatabasethroughputmigration) | Migra la capacidad de proceso de una base de datos de Gremlin API. |
| [Invoke-AzCosmosDBGremlinGraphThroughputMigration](https://docs.microsoft.com/powershell/module/az.cosmosdb/invoke-azcosmosdbgremlingraphthroughputmigration) | Migra la capacidad de proceso de un grafo de Gremlin API. |
|**Grupos de recursos de Azure**| |
| [Remove-AzResourceGroup](https://docs.microsoft.com/powershell/module/az.resources/remove-azresourcegroup) | Elimina un grupo de recursos, incluidos todos los recursos anidados. |
|||

## <a name="next-steps"></a>Pasos siguientes

Para más información sobre Azure PowerShell, consulte la [documentación de Azure PowerShell](https://docs.microsoft.com/powershell/).
