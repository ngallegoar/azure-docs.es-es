---
title: 'Ejemplos de Azure PowerShell para Azure Cosmos DB: SQL (Core) API'
description: Obtenga los ejemplos de Azure PowerShell para realizar varias tareas comunes en cuentas de SQL API de Azure Cosmos DB.
author: markjbrown
ms.service: cosmos-db
ms.topic: sample
ms.date: 06/12/2020
ms.author: mjbrown
ms.openlocfilehash: fc4ec916e71f2fcfd3b411420879d42b2fa90f18
ms.sourcegitcommit: 73ac360f37053a3321e8be23236b32d4f8fb30cf
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/30/2020
ms.locfileid: "85563842"
---
# <a name="azure-powershell-samples-for-azure-cosmos-db---sql-core-api"></a>Ejemplos de Azure PowerShell para Azure Cosmos DB: SQL (Core) API

En la tabla siguiente se incluyen vínculos a scripts de Azure PowerShell usados habitualmente para la API Azure Cosmos DB for SQL (Core). Si desea bifurcar estos ejemplos de PowerShell para Cosmos DB desde nuestro repositorio de GitHub, visite los [ejemplos de PowerShell para Cosmos DB en GitHub](https://github.com/Azure/azure-docs-powershell-samples/tree/master/cosmosdb).

Para ver más ejemplos de PowerShell para Cosmos DB para SQL (Core) API y documentación al respecto, consulte [Administración de recursos de SQL API de Azure Cosmos DB mediante PowerShell](manage-with-powershell.md). Para ver ejemplos de PowerShell para Cosmos DB para otras API consulte, [Cassandra API](powershell-samples-cassandra.md), [MongoDB API](powershell-samples-mongodb.md), [Gremlin API](powershell-samples-gremlin.md) y [Table API](powershell-samples-table.md).

> [!NOTE]
> En los ejemplos, se usan los cmdlets de administración de [Az.CosmosDB](https://docs.microsoft.com/powershell/module/az.cosmosdb). Compruebe periódicamente si hay actualizaciones para `Az.CosmosDB`.

|Tarea | Descripción |
|---|---|
|[Crear una cuenta, base de datos y contenedor](scripts/powershell/sql/ps-sql-create.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Cree una cuenta, una base de datos y un contenedor de Azure Cosmos DB. |
|[Creación de un contenedor con una clave de partición de gran tamaño](scripts/powershell/sql/ps-sql-container-create-large-partition-key.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Cree un contenedor con una clave de partición de gran tamaño. |
|[Enumerar u obtener las bases de datos o contenedores](scripts/powershell/sql/ps-sql-list-get.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Enumere u obtenga las bases de datos o contenedores. |
|[Obtener RU/s](scripts/powershell/sql/ps-sql-ru-get.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Obtenga RU/s para una base de datos o contenedor. |
|[Actualizar RU/s](scripts/powershell/sql/ps-sql-ru-update.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Actualice RU/s para una base de datos o contenedor. |
|[Creación de un contenedor sin directiva de índice](scripts/powershell/sql/ps-sql-container-create-index-none.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Cree un contenedor de Azure Cosmos con directiva de índice desactivada.|
|[Actualización de una cuenta](scripts/powershell/common/ps-account-update.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Actualice el nivel de coherencia predeterminado de una cuenta de Cosmos DB. |
|[Actualización de las regiones de una cuenta](scripts/powershell/common/ps-account-update-region.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Actualice las regiones de una cuenta de Cosmos DB. |
|[Cambio de la prioridad de la conmutación por error o desencadenamiento de la conmutación por error](scripts/powershell/common/ps-account-failover-priority-update.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Cambie la prioridad de la conmutación por error regional de una cuenta de Azure Cosmos o desencadene una conmutación por error manual. |
|[Claves de cuenta o cadenas de conexión](scripts/powershell/common/ps-account-keys-connection-strings.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Obtenga las claves principales y secundarias, las cadenas de conexión o vuelva a generar una clave de cuenta de una cuenta de Azure Cosmos DB. |
|[Creación de una cuenta de Cosmos con firewall de IP](scripts/powershell/common/ps-account-firewall-create.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Cree una cuenta de Azure Cosmos DB con firewall de IP habilitado. |
|[Bloquear recursos contra la eliminación](scripts/powershell/sql/powershell-sql-lock.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Evite la eliminación de recursos con bloqueos de recursos. |
|||
