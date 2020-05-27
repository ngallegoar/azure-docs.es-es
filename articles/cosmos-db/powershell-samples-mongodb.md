---
title: 'Ejemplos de Azure PowerShell para Azure Cosmos DB: MongoDB API'
description: Obtenga los ejemplos de Azure PowerShell para realizar varias tareas comunes en la API para MongoDB de Azure Cosmos DB.
author: markjbrown
ms.service: cosmos-db
ms.topic: sample
ms.date: 05/13/2020
ms.author: mjbrown
ms.openlocfilehash: c8e0a7a60a3512d19a1dfdfdb07b20e523ce7b92
ms.sourcegitcommit: fdec8e8bdbddcce5b7a0c4ffc6842154220c8b90
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/19/2020
ms.locfileid: "83649718"
---
# <a name="azure-powershell-samples-for-azure-cosmos-db-mongodb-api"></a>Ejemplos de Azure PowerShell para Azure Cosmos DB: MongoDB API

En la tabla siguiente se incluyen vínculos a scripts de Azure PowerShell de ejemplo para Azure Cosmos DB para MongoDB API.

> [!NOTE]
> Actualmente solo se puede crear una versión 3.2 (es decir, cuentas que usen el punto de conexión con formato `*.documents.azure.com`) de las cuentas de la API de Azure Cosmos DB para MongoDB mediante las plantillas de PowerShell, de la CLI y de Resource Manager. Para crear cuentas de la versión 3.6, use Azure Portal en su lugar.

> [!NOTE]
> En los ejemplos, se usan los cmdlets de administración de [Az.CosmosDB](https://docs.microsoft.com/powershell/module/az.cosmosdb). Compruebe periódicamente si hay actualizaciones para `Az.CosmosDB`.

| | |
|---|---|
|[Crear una cuenta, base de datos y colección](scripts/powershell/mongodb/ps-mongodb-create.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Cree una cuenta, una base de datos y una colección de Azure Cosmos. |
|[Enumerar u obtener las bases de datos o colecciones](scripts/powershell/mongodb/ps-mongodb-list-get.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Enumere u obtenga una base de datos o colección. |
|[Obtener RU/s](scripts/powershell/mongodb/ps-mongodb-ru-get.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Obtenga RU/s para una base de datos o colección. |
|[Actualizar RU/s](scripts/powershell/mongodb/ps-mongodb-ru-update.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Actualice RU/s para una base de datos o colección. |
|[Actualización de una cuenta o incorporación de una región](scripts/powershell/common/ps-account-update.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Agregue una región a una cuenta de Cosmos. También se puede usar para modificar otras propiedades de la cuenta, pero estas modificaciones se deben realizar de forma independiente a los cambios en las regiones. |
|[Cambio de la prioridad de la conmutación por error o desencadenamiento de la conmutación por error](scripts/powershell/common/ps-account-failover-priority-update.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Cambie la prioridad de la conmutación por error regional de una cuenta de Azure Cosmos o desencadene una conmutación por error manual. |
|[Claves de cuenta o cadenas de conexión](scripts/powershell/common/ps-account-keys-connection-strings.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Obtenga claves principales y secundarias, cadenas de conexión o vuelva a generar una clave de cuenta de una cuenta de Azure Cosmos. |
|[Creación de una cuenta de Cosmos con firewall de IP](scripts/powershell/common/ps-account-firewall-create.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Cree una cuenta de Azure Cosmos con firewall de IP habilitado. |
|||
