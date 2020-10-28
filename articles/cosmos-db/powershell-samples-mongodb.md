---
title: Ejemplos de Azure PowerShell para MongoDB API de Azure Cosmos DB
description: Obtenga ejemplos de Azure PowerShell para realizar tareas comunes en MongoDB API de Azure Cosmos DB.
author: markjbrown
ms.service: cosmos-db
ms.topic: sample
ms.date: 10/13/2020
ms.author: mjbrown
ms.openlocfilehash: 68c4c2236fa5bd595a64525672809a146edbdc64
ms.sourcegitcommit: b6f3ccaadf2f7eba4254a402e954adf430a90003
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/20/2020
ms.locfileid: "92282972"
---
# <a name="azure-powershell-samples-for-azure-cosmos-db-api-for-mongodb"></a>Ejemplos de Azure PowerShell para MongoDB API de Azure Cosmos DB

En la tabla siguiente se incluyen vínculos a scripts de Azure PowerShell usados habitualmente para Azure Cosmos DB. Use los vínculos de la derecha para ir a ejemplos específicos de la API. Los ejemplos comunes son los mismos en todas las API. En la [referencia de Azure PowerShell](/powershell/module/az.cosmosdb) hay páginas de referencia para todos los cmdlets de PowerShell de Azure Cosmos DB disponibles. Compruebe periódicamente si hay actualizaciones para `Az.CosmosDB`. También puede bifurcar estos ejemplos de PowerShell para Cosmos DB desde nuestro repositorio de GitHub, [Ejemplos de PowerShell para Cosmos DB en GitHub](https://github.com/Azure/azure-docs-powershell-samples/tree/master/cosmosdb).

## <a name="common-samples"></a>Ejemplos comunes

|Tarea | Descripción |
|---|---|
|[Actualización de una cuenta](scripts/powershell/common/account-update.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Actualice el nivel de coherencia predeterminado de una cuenta de Cosmos DB. |
|[Actualización de las regiones de una cuenta](scripts/powershell/common/update-region.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Actualice las regiones de una cuenta de Cosmos DB. |
|[Cambio de la prioridad de la conmutación por error o desencadenamiento de la conmutación por error](scripts/powershell/common/failover-priority-update.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Cambie la prioridad de la conmutación por error regional de una cuenta de Azure Cosmos o desencadene una conmutación por error manual. |
|[Claves de cuenta o cadenas de conexión](scripts/powershell/common/keys-connection-strings.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Obtenga las claves principales y secundarias, las cadenas de conexión o vuelva a generar una clave de cuenta de una cuenta de Azure Cosmos DB. |
|[Creación de una cuenta de Cosmos con firewall de IP](scripts/powershell/common/firewall-create.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Cree una cuenta de Azure Cosmos DB con firewall de IP habilitado. |
|||

## <a name="mongo-db-api-samples"></a>Ejemplos de Mongo DB API

|Tarea | Descripción |
|---|---|
|[Crear una cuenta, base de datos y colección](scripts/powershell/mongodb/create.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Cree una cuenta, una base de datos y una colección de Azure Cosmos. |
|[Creación de una cuenta, una base de datos y una colección con escalabilidad automática](scripts/powershell/mongodb/autoscale.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Crea una cuenta, una base de datos y una colección con escalabilidad automática de Azure Cosmos. |
|[Enumerar u obtener las bases de datos o colecciones](scripts/powershell/mongodb/list-get.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Enumere u obtenga una base de datos o colección. |
|[Operaciones de capacidad de proceso](scripts/powershell/mongodb/throughput.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Operaciones relacionadas con la capacidad de proceso de una base de datos o una colección, como obtener, actualizar y migrar entre la escalabilidad automática y la capacidad de proceso estándar. |
|[Bloquear recursos contra la eliminación](scripts/powershell/mongodb/lock.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Evite la eliminación de recursos con bloqueos de recursos. |
|||
