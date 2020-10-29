---
title: 'La CLI de Azure: Crear una base de datos única'
description: Utilice este script de ejemplo de la CLI de Azure para crear una base de datos única.
services: sql-database
ms.service: sql-database
ms.subservice: single-database
ms.custom: sqldbrb=1, devx-track-azurecli
ms.devlang: azurecli
ms.topic: sample
author: stevestein
ms.author: sstein
ms.reviewer: ''
ms.date: 06/25/2019
ms.openlocfilehash: dc17ab55cb473036d8c40bd1e533bcc20c4c6259
ms.sourcegitcommit: 8c7f47cc301ca07e7901d95b5fb81f08e6577550
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/27/2020
ms.locfileid: "92742259"
---
# <a name="use-the-azure-cli-to-create-a-single-database-and-configure-a-firewall-rule"></a>Uso de la CLI de Azure para crear una base de datos única y configurar una regla de firewall

[!INCLUDE[appliesto-sqldb](../../includes/appliesto-sqldb.md)]

Este script de ejemplo de la CLI de Azure crea una base de datos única en Azure SQL Database y configura una regla de firewall en el nivel de servidor. Después de ejecutar el script correctamente, la base de datos será accesible desde todos los servicios de Azure y la dirección IP configurada.

Si decide instalar y usar la CLI localmente, para este tema es preciso que ejecute la CLI de Azure versión 2.0 o posterior. Ejecute `az --version` para encontrar la versión. Si necesita instalarla o actualizarla, consulte [Instalación de la CLI de Azure]( /cli/azure/install-azure-cli).

## <a name="sample-script"></a>Script de ejemplo

### <a name="sign-in-to-azure"></a>Inicio de sesión en Azure

[!INCLUDE [quickstarts-free-trial-note](../../../../includes/quickstarts-free-trial-note.md)]

### <a name="run-the-script"></a>Ejecute el script.

[!code-azurecli-interactive[main](../../../../cli_scripts/sql-database/create-and-configure-database/create-and-configure-database.sh "Create SQL Database")]

### <a name="clean-up-deployment"></a>Limpieza de la implementación

Use el siguiente comando para quitar el grupo de recursos y todos los recursos que tenga asociados.

```azurecli-interactive
az group delete --name $resource
```

## <a name="sample-reference"></a>Referencia de ejemplo

Este script usa los siguientes comandos. Cada comando de la tabla crea un vínculo a documentación específica del comando.

| Get-Help | Descripción |
|---|---|
| [az sql server](/cli/azure/sql/server#az-sql-server-create) | Comandos de servidor. |
| [az sql server firewall](/cli/azure/sql/server/firewall-rule#az-sql-server-firewall-rule-create) | Comandos de firewall del servidor. |
| [az sql db](/cli/azure/sql/db#az-sql-db-create) | Comandos de base de datos. |

## <a name="next-steps"></a>Pasos siguientes

Para más información sobre la CLI de Azure, consulte la [documentación de la CLI de Azure](/cli/azure).

Encontrará más ejemplos de scripts de la CLI de SQL Database en la [documentación de Azure SQL Database](../az-cli-script-samples-content-guide.md).
