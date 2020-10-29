---
title: Script de PowerShell para las operaciones list y get para Gremlin API en Azure Cosmos DB
description: 'Script de Azure PowerShell: Operaciones List y Get para Gremlin API'
author: markjbrown
ms.service: cosmos-db
ms.subservice: cosmosdb-graph
ms.topic: sample
ms.date: 05/01/2020
ms.author: mjbrown
ms.openlocfilehash: d790164b21419794f47f6fd99ac55824dc8af1a9
ms.sourcegitcommit: 3bcce2e26935f523226ea269f034e0d75aa6693a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/23/2020
ms.locfileid: "92489190"
---
# <a name="list-and-get-databases-and-graphs-for-azure-cosmos-db---gremlin-api"></a>Enumeración y obtención de bases de datos y gráficos de Gremlin API en Azure Cosmos DB

[!INCLUDE [updated-for-az](../../../../../includes/updated-for-az.md)]

[!INCLUDE [sample-powershell-install](../../../../../includes/sample-powershell-install-no-ssh.md)]

## <a name="sample-script"></a>Script de ejemplo

[!code-powershell[main](../../../../../powershell_scripts/cosmosdb/gremlin/ps-gremlin-list-get.ps1 "List or get databases or graphs for Gremlin API")]

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
| [Get-AzCosmosDBGremlinDatabase](/powershell/module/az.cosmosdb/get-azcosmosdbgremlindatabase) | Enumera las bases datos de Gremlin API de una cuenta u obtiene una base de datos de Gremlin API especificada en una cuenta. |
| [Get-AzCosmosDBGremlinGraph](/powershell/module/az.cosmosdb/get-azcosmosdbgremlingraph) | Enumera los grafos en una base de datos de Gremlin API de una cuenta u obtiene una tabla especificada de una Gremlin API especificada. |
|**Grupos de recursos de Azure**| |
| [Remove-AzResourceGroup](/powershell/module/az.resources/remove-azresourcegroup) | Elimina un grupo de recursos, incluidos todos los recursos anidados. |
|||

## <a name="next-steps"></a>Pasos siguientes

Para más información sobre Azure PowerShell, consulte la [documentación de Azure PowerShell](/powershell/).