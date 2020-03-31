---
title: Ejemplo de CLI para crear una instancia administrada en Azure SQL Database
description: Script de ejemplo de la CLI de Azure para crear una instancia administrada en Azure SQL Database
services: sql-database
ms.service: sql-database
ms.subservice: managed-instance
ms.custom: ''
ms.devlang: azurecli
ms.topic: sample
author: stevestein
ms.author: sstein
ms.reviewer: carlrab
ms.date: 03/25/2019
ms.openlocfilehash: 998bd8e39863dd9520d1a05e7fff52095c3be6c1
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/24/2020
ms.locfileid: "80067444"
---
# <a name="use-cli-to-create-an-azure-sql-database-managed-instance"></a>Uso de la CLI para crear una instancia administrada de Azure SQL Database

En este ejemplo de script de la CLI de Azure se crea una instancia administrada de Azure SQL Database en una subred dedicada de una red virtual nueva. También se configura una tabla de rutas y un grupo de seguridad de red para la red virtual. Una vez que el script se ha ejecutado correctamente, se puede acceder a la instancia administrada desde dentro de la red virtual o desde un entorno local. Consulte [Configuración de una máquina virtual de Azure para la conexión a una Instancia administrada de Azure SQL Database](../sql-database-managed-instance-configure-vm.md) y [Configuración de una conexión de punto a sitio en una Instancia administrada de Azure SQL Database desde un entorno local](../sql-database-managed-instance-configure-p2s.md).

> [!IMPORTANT]
> Para conocer las limitaciones, consulte las [regiones admitidas](../sql-database-managed-instance-resource-limits.md#supported-regions) y los [tipos de suscripción admitidos](../sql-database-managed-instance-resource-limits.md#supported-subscription-types).

Si decide instalar y usar la CLI localmente, para este artículo es preciso que ejecute la versión 2.0 o posterior de la CLI de Azure. Ejecute `az --version` para encontrar la versión. Si necesita instalarla o actualizarla, consulte [Instalación de la CLI de Azure](/cli/azure/install-azure-cli).

## <a name="sample-script"></a>Script de ejemplo

### <a name="sign-in-to-azure"></a>Inicio de sesión en Azure

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

### <a name="run-the-script"></a>Ejecute el script.

[!code-azurecli-interactive[main](../../../cli_scripts/sql-database/managed-instance/create-managed-instance.sh "Create managed instance")]

### <a name="clean-up-deployment"></a>Limpieza de la implementación

Use el siguiente comando para quitar el grupo de recursos y todos los recursos que tenga asociados.

```azurecli-interactive
az group delete --name $resource
```

## <a name="sample-reference"></a>Referencia de ejemplo

Este script usa los siguientes comandos. Cada comando de la tabla crea un vínculo a documentación específica del comando.

| | |
|---|---|
| [az network vnet](/cli/azure/network/vnet) | Comandos de red virtual. |
| [az network vnet subnet](/cli/azure/network/vnet/subnet) | Comandos de subred de red virtual. |
| [az network route-table](/cli/azure/network/route-table) | Comandos de tabla de rutas de red. |
| [az sql mi](/cli/azure/sql/mi) | Comandos de instancia administrada. |

## <a name="next-steps"></a>Pasos siguientes

Para más información sobre la CLI de Azure, consulte la [documentación de la CLI de Azure](/cli/azure).

Encontrará más ejemplos de scripts de la CLI de SQL Database en la [documentación de Azure SQL Database](../sql-database-cli-samples.md).
