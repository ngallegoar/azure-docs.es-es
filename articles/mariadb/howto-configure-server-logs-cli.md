---
title: Acceso a registros de consultas lentos de Azure Database for MariaDB mediante la CLI de Azure
description: En este artículo se describe cómo acceder a los registros lentos de Azure Database for MariaDB mediante la utilidad de la línea de comandos de la CLI de Azure.
author: ajlam
ms.author: andrela
ms.service: mariadb
ms.devlang: azurecli
ms.topic: conceptual
ms.date: 3/18/2020
ms.openlocfilehash: f33a02ff0e287c135a7d63277cf3d8d3c0cd13d4
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "79527663"
---
# <a name="configure-and-access-slow-query-logs-by-using-azure-cli"></a>Configuración y acceso a los registros de consultas lentas con la CLI de Azure
Puede descargar los registros de consultas lentos de Azure Database for MySQL mediante la CLI de Azure, la utilidad de línea de comandos de Azure.

## <a name="prerequisites"></a>Prerequisites
Para seguir esta guía, necesitará:
- [Servidor de Azure Database for MariaDB](quickstart-create-mariadb-server-database-using-azure-cli.md)
- La [CLI de Azure](/cli/azure/install-azure-cli) o Azure Cloud Shell en el explorador

## <a name="configure-logging-for-azure-database-for-mariadb"></a>Configuración del registro para Azure Database for MariaDB
Puede configurar el servidor para acceder al registro de consultas lentas de MariaDB con los pasos siguientes:
1. Active el registro estableciendo el parámetro **slow\_query\_log** en ON.
2. Ajuste otros parámetros, como **long\_query\_time** y **log\_slow\_admin\_statements**.

Para aprender a establecer el valor de estos parámetros mediante la CLI de Azure, consulte [Cómo configurar parámetros del servidor](howto-configure-server-parameters-cli.md).

Por ejemplo, el siguiente comando de la CLI activará el registro de consultas lentas, establecerá el tiempo de consultas largas en 10 segundos y desactivará el registro de la instrucción de administración lenta. Por último, se muestran las opciones de configuración para su revisión.
```azurecli-interactive
az mariadb server configuration set --name slow_query_log --resource-group myresourcegroup --server mydemoserver --value ON
az mariadb server configuration set --name long_query_time --resource-group myresourcegroup --server mydemoserver --value 10
az mariadb server configuration set --name log_slow_admin_statements --resource-group myresourcegroup --server mydemoserver --value OFF
az mariadb server configuration list --resource-group myresourcegroup --server mydemoserver
```

## <a name="list-logs-for-azure-database-for-mariadb-server"></a>Lista de registros del servidor de Azure Database for MariaDB
Para mostrar la lista de archivos de registro de consultas lentas disponibles para el servidor, ejecute el comando [az mariadb server-logs list](/cli/azure/mariadb/server-logs#az-mariadb-server-logs-list).

Puede enumerar los archivos de registro del servidor **mydemoserver.mariadb.database.azure.com** en el grupo de recursos **myresourcegroup**. A continuación, dirija la lista de archivos de registro a un archivo de texto denominado **log\_files\_list.txt**.
```azurecli-interactive
az mariadb server-logs list --resource-group myresourcegroup --server mydemoserver > log_files_list.txt
```
## <a name="download-logs-from-the-server"></a>Descarga de registros del servidor
Con el comando [az mariadb server-logs download](/cli/azure/mariadb/server-logs#az-mariadb-server-logs-download), puede descargar archivos de registro individuales del servidor.

Use el ejemplo siguiente para descargar el archivo de registro específico del servidor **mydemoserver.mariadb.database.azure.com** en el grupo de recursos **myresourcegroup** a su entorno local.
```azurecli-interactive
az mariadb server-logs download --name mysql-slow-mydemoserver-2018110800.log --resource-group myresourcegroup --server mydemoserver
```

## <a name="next-steps"></a>Pasos siguientes
- Más información sobre los [registros de consultas lentas en Azure Database for MariaDB](concepts-server-logs.md).
