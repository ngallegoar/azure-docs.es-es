---
title: 'Script de la CLI: Cambio de parámetros del servidor de Azure Database for MySQL'
description: Este script de la CLI de ejemplo enumera todas las configuraciones del servidor disponibles y actualiza el valor de innodb_lock_wait_timeout.
author: savjani
ms.author: pariks
ms.service: mysql
ms.devlang: azurecli
ms.topic: sample
ms.custom: mvc, devx-track-azurecli
ms.date: 12/02/2019
ms.openlocfilehash: 72ebfd8c7aa16a22221173aebfa8a3c5e0f1f156
ms.sourcegitcommit: 6ab718e1be2767db2605eeebe974ee9e2c07022b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/12/2020
ms.locfileid: "94542378"
---
# <a name="list-and-update-configurations-of-an-azure-database-for-mysql-server-using-azure-cli"></a>Enumeración y actualización de configuraciones de un servidor de Azure Database for MySQL mediante la CLI de Azure
Este script de la CLI de ejemplo enumera todos los parámetros de configuración disponibles, así como sus valores permitidos para el servidor de Azure Database for MySQL y establece *innodb_lock_wait_timeout* en un valor distinto del predeterminado.

[!INCLUDE [azure-cli-prepare-your-environment.md](../../../includes/azure-cli-prepare-your-environment.md)]

- En este artículo se necesita la versión 2.0 o posterior de la CLI de Azure. Si usa Azure Cloud Shell, ya está instalada la versión más reciente. 

## <a name="sample-script"></a>Script de ejemplo
En este script de ejemplo, va a modificar las líneas resaltadas para actualizar el nombre de usuario administrador y la contraseña a los suyos propios.
[!code-azurecli-interactive[main](../../../cli_scripts/mysql/change-server-configurations/change-server-configurations.sh?highlight=15-16 "List and update configurations of Azure Database for MySQL.")]

## <a name="clean-up-deployment"></a>Limpieza de la implementación
Use el comando siguiente para quitar el grupo de recursos y todos los recursos asociados a él después de ejecutarse el script de ejemplo. 
[!code-azurecli-interactive[main](../../../cli_scripts/mysql/change-server-configurations/delete-mysql.sh  "Delete the resource group.")]

## <a name="script-explanation"></a>Explicación del script
Este script usa los comandos que se describen en la tabla siguiente:

| **Comando** | **Notas** |
|---|---|
| [az group create](/cli/azure/group#az-group-create) | Crea un grupo de recursos en el que se almacenan todos los recursos. |
| [az mysql server create](/cli/azure/mysql/server#az-mysql-server-create) | Crea un servidor MySQL que hospeda las bases de datos. |
| [az mysql server configuration list](/cli/azure/mysql/server/configuration#az-mysql-server-configuration-list) | Enumeración de las configuraciones de un servidor de Azure Database for MySQL. |
| [az mysql server configuration set](/cli/azure/mysql/server/configuration#az-mysql-server-configuration-set) | Actualice la configuración de un servidor de Azure Database for MySQL. |
| [az mysql server configuration show](/cli/azure/mysql/server/configuration#az-mysql-server-configuration-show) | Muestre la configuración de un servidor de Azure Database for MySQL. |
| [az group delete](/cli/azure/group#az-group-delete) | Elimina un grupo de recursos, incluidos todos los recursos anidados. |

## <a name="next-steps"></a>Pasos siguientes
- Para más información sobre la CLI de Azure: [Documentación de la CLI de Azure](/cli/azure).
- Pruebe otros scripts adicionales: [Ejemplos de la CLI de Azure para Azure Database for MySQL](../sample-scripts-azure-cli.md)
- Para obtener más información sobre los parámetros del servidor, consulte [Cómo configurar parámetros del servidor en Azure Database for MySQL](../howto-server-parameters.md).
