---
title: Ejemplos de Azure PowerShell para Azure Cosmos DB
description: Obtenga ejemplos de Azure PowerShell para realizar tareas comunes en Azure Cosmos DB
author: markjbrown
ms.service: cosmos-db
ms.topic: sample
ms.date: 10/07/2020
ms.author: mjbrown
ms.openlocfilehash: 0c2d13ef49676917bc48dd5a2e682471558567f3
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "91842978"
---
# <a name="azure-powershell-samples-for-azure-cosmos-db"></a>Ejemplos de Azure PowerShell para Azure Cosmos DB

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

## <a name="core-sql-api-samples"></a>Ejemplos de API Core (SQL)

|Tarea | Descripción |
|---|---|
|[Crear una cuenta, base de datos y contenedor](scripts/powershell/sql/create.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Cree una cuenta, una base de datos y un contenedor de Azure Cosmos DB. |
|[Creación de una cuenta, una base de datos y un contenedor con escalabilidad automática](scripts/powershell/sql/autoscale.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Cree una cuenta, una base de datos y un contenedor de Azure Cosmos DB con escalabilidad automática. |
|[Creación de un contenedor con una clave de partición de gran tamaño](scripts/powershell/sql/create-large-partition-key.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Cree un contenedor con una clave de partición de gran tamaño. |
|[Creación de un contenedor sin directiva de índice](scripts/powershell/sql/create-index-none.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Cree un contenedor de Azure Cosmos con directiva de índice desactivada.|
|[Enumerar u obtener las bases de datos o contenedores](scripts/powershell/sql/list-get.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Enumere u obtenga las bases de datos o contenedores. |
|[Operaciones de capacidad de proceso](scripts/powershell/sql/throughput.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Operaciones relacionadas con la capacidad de proceso de una base de datos o un contenedor, como obtener, actualizar y migrar entre la escalabilidad automática y la capacidad de proceso estándar. |
|[Bloquear recursos contra la eliminación](scripts/powershell/sql/lock.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Evite la eliminación de recursos con bloqueos de recursos. |
|||

## <a name="cassandra-api-samples"></a>Ejemplos de Cassandra API

|Tarea | Descripción |
|---|---|
|[Crear una cuenta, un espacio de claves y una tabla](scripts/powershell/cassandra/create.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Cree una cuenta, un espacio de claves y una tabla de Azure Cosmos. |
|[Creación de una cuenta, un espacio de claves y una tabla con escalabilidad automática](scripts/powershell/cassandra/autoscale.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Crea una cuenta, un espacio de claves y una tabla con escalabilidad automática de Azure Cosmos. |
|[Enumerar u obtener espacios de claves o tablas](scripts/powershell/cassandra/list-get.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Enumere u obtenga espacios de claves o tablas. |
|[Operaciones de capacidad de proceso](scripts/powershell/cassandra/throughput.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Operaciones relacionadas con la capacidad de proceso de un espacio de claves o una tabla, como obtener, actualizar y migrar entre la escalabilidad automática y la capacidad de proceso estándar. |
|[Bloquear recursos contra la eliminación](scripts/powershell/cassandra/lock.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Evite la eliminación de recursos con bloqueos de recursos. |
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

## <a name="gremlin-api-samples"></a>Ejemplos de Gremlin API

|Tarea | Descripción |
|---|---|
|[Crear una cuenta, base de datos y gráfico](scripts/powershell/gremlin/create.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Cree una cuenta, una base de datos y un gráfico de Azure Cosmos. |
|[Creación de una cuenta, una base de datos y un grafo con escalabilidad automática](scripts/powershell/gremlin/autoscale.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Crea una cuenta, una base de datos y un grafo con escalabilidad automática de Azure Cosmos. |
|[Enumerar u obtener las bases de datos o gráficos](scripts/powershell/gremlin/list-get.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Enumere u obtenga las bases de datos o gráficos. |
|[Operaciones de capacidad de proceso](scripts/powershell/gremlin/throughput.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Operaciones relacionadas con la capacidad de proceso de una base de datos o un grafo, como obtener, actualizar y migrar entre la escalabilidad automática y la capacidad de proceso estándar. |
|[Bloquear recursos contra la eliminación](scripts/powershell/gremlin/lock.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Evite la eliminación de recursos con bloqueos de recursos. |
|||

## <a name="table-api-samples"></a>Ejemplos de Table API

|Tarea | Descripción |
|---|---|
|[Crear una cuenta y tabla](scripts/powershell/table/create.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Cree una cuenta y una tabla de Azure Cosmos. |
|[Creación de una cuenta y una tabla con escalabilidad automática](scripts/powershell/table/autoscale.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Crea una cuenta y una tabla con escalabilidad automática de Azure Cosmos. |
|[Enumerar u obtener tablas](scripts/powershell/table/list-get.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Enumere u obtenga las tablas. |
|[Operaciones de capacidad de proceso](scripts/powershell/table/throughput.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Operaciones relacionadas con la capacidad de proceso de una tabla, como obtener, actualizar y migrar entre la escalabilidad automática y la capacidad de proceso estándar. |
|[Bloquear recursos contra la eliminación](scripts/powershell/table/lock.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Evite la eliminación de recursos con bloqueos de recursos. |
|||
