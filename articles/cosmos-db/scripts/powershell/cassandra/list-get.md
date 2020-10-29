---
title: Script de PowerShell para enumerar y obtener recursos de Cassandra API en Azure Cosmos DB
description: 'Script de Azure PowerShell: Operaciones de lista y obtención para Cassandra API en Azure Cosmos DB'
author: markjbrown
ms.service: cosmos-db
ms.subservice: cosmosdb-cassandra
ms.topic: sample
ms.date: 03/18/2020
ms.author: mjbrown
ms.openlocfilehash: 8cd9abf02d66c6bed5720129d0b19c9a7545fac7
ms.sourcegitcommit: 3bcce2e26935f523226ea269f034e0d75aa6693a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/23/2020
ms.locfileid: "92488323"
---
# <a name="list-and-get-keyspaces-and-tables-for-azure-cosmos-db---cassandra-api"></a>Enumerar y obtener espacios de claves y tablas de Azure Cosmos DB: Cassandra API

[!INCLUDE [updated-for-az](../../../../../includes/updated-for-az.md)]

[!INCLUDE [sample-powershell-install](../../../../../includes/sample-powershell-install-no-ssh.md)]

## <a name="sample-script"></a>Script de ejemplo

[!code-powershell[main](../../../../../powershell_scripts/cosmosdb/cassandra/ps-cassandra-list-get.ps1 "List or get keyspace or table for Cassandra API")]

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
| [Get-AzCosmosDBCassandraKeyspace](/powershell/module/az.cosmosdb/get-azcosmosdbcassandrakeyspace) | Muestra los espacios de claves de Cassandra API en Cosmos DB en una cuenta u obtiene un espacio de claves especificado de Cassandra API en Cosmos DB en una cuenta. |
| [Get-AzCosmosDBCassandraTable](/powershell/module/az.cosmosdb/get-azcosmosdbcassandratable) | Enumera las tablas de Cassandra API en Cosmos DB en una cuenta u obtiene una tabla especificada de Cassandra API en Cosmos DB en una cuenta. |
|**Grupos de recursos de Azure**| |
| [Remove-AzResourceGroup](/powershell/module/az.resources/remove-azresourcegroup) | Elimina un grupo de recursos, incluidos todos los recursos anidados. |
|||

## <a name="next-steps"></a>Pasos siguientes

Para más información sobre Azure PowerShell, consulte la [documentación de Azure PowerShell](/powershell/).