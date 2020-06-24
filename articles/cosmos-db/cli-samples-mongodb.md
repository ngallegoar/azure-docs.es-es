---
title: Ejemplos de la CLI de Azure MongoDB API para Azure Cosmos DB
description: Ejemplos de la CLI de Azure MongoDB API para Azure Cosmos DB
author: markjbrown
ms.service: cosmos-db
ms.subservice: cosmosdb-mongo
ms.topic: how-to
ms.date: 06/03/2020
ms.author: mjbrown
ms.openlocfilehash: 88b795b52955a6bd323e7a900c0cd62dab1dd2d4
ms.sourcegitcommit: 635114a0f07a2de310b34720856dd074aaf4f9cd
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/23/2020
ms.locfileid: "85262946"
---
# <a name="azure-cli-samples-for-azure-cosmos-db-mongodb-api"></a>Ejemplos de la CLI de Azure MongoDB API para Azure Cosmos DB

En la tabla siguiente se incluyen vínculos a scripts de ejemplo de la CLI de Azure para MongoDB API para Azure Cosmos DB. Hay páginas de referencia para todos los comandos de CLI de Azure Cosmos DB disponibles en la [referencia de la CLI de Azure](/cli/azure/cosmosdb). Encontrará más ejemplos de scripts de la CLI de Azure Cosmos DB en el [repositorio de la CLI de Azure Cosmos DB en GitHub](https://github.com/Azure-Samples/azure-cli-samples/tree/master/cosmosdb).

> [!NOTE]
> Actualmente solo se puede crear una versión 3.2 (es decir, cuentas que usen el punto de conexión con formato `*.documents.azure.com`) de las cuentas de la API de Azure Cosmos DB para MongoDB mediante las plantillas de PowerShell, de la CLI y de Resource Manager. Para crear cuentas de la versión 3.6, use Azure Portal en su lugar.

| |  |
|---|---|
| [Crear una cuenta, una base de datos y una colección de Azure Cosmos](scripts/cli/mongodb/create.md?toc=%2fcli%2fazure%2ftoc.json)| Crea una cuenta de Azure Cosmos DB y una base de datos y una colección de MongoDB API. |
| [Cambiar la capacidad de proceso](scripts/cli/mongodb/throughput.md?toc=%2fcli%2fazure%2ftoc.json) | Actualiza la capacidad de proceso (RU/s) de una base de datos y una colección.|
| [Agregar o regiones de conmutación por error](scripts/cli/common/regions.md?toc=%2fcli%2fazure%2ftoc.json) | Agrega una región, cambia la prioridad de conmutación por error y desencadena una conmutación por error manual.|
| [Claves de cuenta y cadenas de conexión](scripts/cli/common/keys.md?toc=%2fcli%2fazure%2ftoc.json) | Muestra las claves de cuenta y las claves de solo lectura, vuelve a generar las claves y enumera las cadenas de conexión.|
| [Protección con firewall de IP](scripts/cli/common/ipfirewall.md?toc=%2fcli%2fazure%2ftoc.json)| Crea una cuenta de Cosmos con firewall de IP configurado.|
| [Proteger una cuenta nueva con puntos de conexión de servicio](scripts/cli/common/service-endpoints.md?toc=%2fcli%2fazure%2ftoc.json)| Crea una cuenta de Cosmos y la protege con puntos de conexión de servicio.|
| [Proteger una cuenta existente con puntos de conexión de servicio](scripts/cli/common/service-endpoints-ignore-missing-vnet.md?toc=%2fcli%2fazure%2ftoc.json)| Actualiza una cuenta de Cosmos para protegerla con puntos de conexión de servicio cuando la subred se configura finalmente.|
| [Bloquear recursos contra la eliminación](scripts/cli/mongodb/lock.md?toc=%2fcli%2fazure%2ftoc.json)| Bloquee los recursos para impedir que se eliminen.|
|||
