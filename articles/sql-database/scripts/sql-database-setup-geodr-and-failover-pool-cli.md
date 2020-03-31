---
title: Ejemplo de la CLI para configurar la replicación geográfica activa para una base de datos agrupada de Azure SQL Database
description: Script de ejemplo de la CLI de Azure para configurar la replicación geográfica activa para una base de datos agrupada en Azure SQL Database y conmutarla por error.
services: sql-database
ms.service: sql-database
ms.subservice: high-availability
ms.custom: ''
ms.devlang: azurecli
ms.topic: sample
author: mashamsft
ms.author: mathoma
ms.reviewer: carlrab
ms.date: 03/12/2019
ms.openlocfilehash: 2646ed98f4a73c69d339df0134e8a565c958c514
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/24/2020
ms.locfileid: "80067354"
---
# <a name="use-cli-to-configure-active-geo-replication-for-a-pooled-database-in-azure-sql-database"></a>Uso de la CLI para configurar la replicación geográfica activa para una base de datos agrupada en Azure SQL Database

En este ejemplo de script de la CLI de Azure se configura la replicación geográfica activa para una base de datos agrupada en Azure SQL Database y se conmuta por error a una réplica secundaria de la base de datos.

Si decide instalar y usar la CLI localmente, para este artículo es preciso que ejecute la versión 2.0 o posterior de la CLI de Azure. Ejecute `az --version` para encontrar la versión. Si necesita instalarla o actualizarla, consulte [Instalación de la CLI de Azure](/cli/azure/install-azure-cli).

## <a name="sample-script"></a>Script de ejemplo

### <a name="sign-in-to-azure"></a>Inicio de sesión en Azure

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

```azurecli-interactive
$subscription = "<subscriptionId>" # add subscription here

az account set -s $subscription # ...or use 'az login'
```

### <a name="run-the-script"></a>Ejecute el script.

[!code-azurecli-interactive[main](../../../cli_scripts/sql-database/setup-geodr-and-failover/setup-geodr-and-failover-elastic-pool.sh "Set up active geo-replication for elastic pool")]

### <a name="clean-up-deployment"></a>Limpieza de la implementación

Use el siguiente comando para quitar el grupo de recursos y todos los recursos que tenga asociados.

```azurecli-interactive
az group delete --name $resource
az group delete --name $secondaryResource
```

## <a name="sample-reference"></a>Referencia de ejemplo

Este script usa los siguientes comandos. Cada comando de la tabla crea un vínculo a documentación específica del comando.

| | |
|---|---|
| [az sql elastic-pool](/cli/azure/sql/elastic-pool) | Comandos del grupo de bases de datos elásticas |
| [az sql db replica](/cli/azure/sql/db/replica) | Comandos de la replicación de base de datos. |

## <a name="next-steps"></a>Pasos siguientes

Para más información sobre la CLI de Azure, consulte la [documentación de la CLI de Azure](/cli/azure).

Encontrará más ejemplos de scripts de la CLI de SQL Database en la [documentación de Azure SQL Database](../sql-database-cli-samples.md).
