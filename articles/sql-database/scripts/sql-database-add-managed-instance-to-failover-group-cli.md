---
title: 'Ejemplo de CLI: grupo de conmutación por error en una instancia administrada de Azure SQL Database'
description: Script de ejemplo de la CLI de Azure para crear una instancia administrada de Azure SQL Database, agregarla a un grupo de conmutación por error y probar la conmutación por error.
services: sql-database
ms.service: sql-database
ms.subservice: high-availability
ms.custom: ''
ms.devlang: azurecli
ms.topic: sample
author: MashaMSFT
ms.author: mathoma
ms.reviewer: carlrab
ms.date: 07/16/2019
ms.openlocfilehash: 8ffe40662ffaf8a1fb35a3d31acfaea78ea0fbeb
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/24/2020
ms.locfileid: "80061918"
---
# <a name="use-cli-to-add-an-azure-sql-database-managed-instance-to-a-failover-group"></a>Uso de la CLI para agregar una instancia administrada de Azure SQL Database a un grupo de conmutación por error

Este ejemplo de la CLI de Azure crea dos instancias administradas, las agrega a un grupo de conmutación por error y, a continuación, prueba la conmutación por error de la instancia administrada principal en la instancia administrada secundaria.

Si decide instalar y usar la CLI localmente, para este artículo es preciso que ejecute la versión 2.0 o posterior de la CLI de Azure. Ejecute `az --version` para encontrar la versión. Si necesita instalarla o actualizarla, consulte [Instalación de la CLI de Azure](/cli/azure/install-azure-cli).

## <a name="sample-script"></a>Script de ejemplo

### <a name="sign-in-to-azure"></a>Inicio de sesión en Azure

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

### <a name="run-the-script"></a>Ejecute el script.

[!code-azurecli-interactive[main](../../../cli_scripts/sql-database/failover-groups/add-managed-instance-to-failover-group-az-cli.sh "Add managed instance to a failover group")]

### <a name="clean-up-deployment"></a>Limpieza de la implementación

Use el siguiente comando para quitar el grupo de recursos y todos los recursos que tenga asociados. Tendrá que quitar el grupo de recursos dos veces. Al quitar el grupo de recursos la primera vez, se quitarán la instancia administrada y los clústeres virtuales, pero se producirá el mensaje de error `az group delete : Long running operation failed with status 'Conflict'.`. Ejecute el comando az group delete una segunda vez para quitar los recursos residuales y el grupo de recursos.

```azurecli-interactive
az group delete --name $resource
```

## <a name="sample-reference"></a>Referencia de ejemplo

Este script usa los siguientes comandos. Cada comando de la tabla crea un vínculo a documentación específica del comando.

| | |
|---|---|
| [az network vnet](/cli/azure/network/vnet) | Comandos de red virtual.  |
| [az network vnet subnet](/cli/azure/network/vnet/subnet) | Comandos de subred de red virtual. |
| [az network nsg](/cli/azure/network/nsg) | Comandos de grupo de seguridad de red. |
| [az network route-table](/cli/azure/network/route-table) | Comandos de tabla de rutas. |
| [az sql mi](/cli/azure/sql/mi) | Comandos de instancia administrada. |
| [az network public-ip](/cli/azure/network/public-ip) | Comandos de dirección IP pública de red. |
| [az network vnet-gateway](/cli/azure/network/vnet-gateway) | Problemas de la puerta de enlace de red virtual. |
| [az sql instance-failover-group](/cli/azure/sql/instance-failover-group) | Comandos de grupo de conmutación por error de instancia administrada. |

## <a name="next-steps"></a>Pasos siguientes

Para más información sobre la CLI de Azure, consulte la [documentación de la CLI de Azure](/cli/azure).

Encontrará más ejemplos de scripts de la CLI de SQL Database en la [documentación de Azure SQL Database](../sql-database-cli-samples.md).
