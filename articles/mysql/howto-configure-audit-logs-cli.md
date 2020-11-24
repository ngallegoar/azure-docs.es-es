---
title: 'Acceso a los registros de auditoría: CLI de Azure: Azure Database for MySQL'
description: En este artículo se describe cómo configurar los registros de auditoría de Azure Database for MySQL y cómo acceder a ellos desde la CLI de Azure.
author: savjani
ms.author: pariks
ms.service: mysql
ms.topic: how-to
ms.date: 6/24/2020
ms.custom: devx-track-azurecli
ms.openlocfilehash: db7ffabae785a589bacf349356079f6046039f9c
ms.sourcegitcommit: 6ab718e1be2767db2605eeebe974ee9e2c07022b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/12/2020
ms.locfileid: "94542004"
---
# <a name="configure-and-access-audit-logs-in-the-azure-cli"></a>Configuración y acceso a los registros de auditoría en la CLI de Azure

Puede configurar los [registros de auditoría de Azure Database for MySQL](concepts-audit-logs.md) desde la CLI de Azure.

## <a name="prerequisites"></a>Requisitos previos

Para seguir esta guía paso a paso:

- Necesita un [servidor de Azure Database for MySQL](quickstart-create-mysql-server-database-using-azure-portal.md).

[!INCLUDE [azure-cli-prepare-your-environment-no-header.md](../../includes/azure-cli-prepare-your-environment-no-header.md)]

- En este artículo se necesita la versión 2.0 o posterior de la CLI de Azure. Si usa Azure Cloud Shell, ya está instalada la versión más reciente.

## <a name="configure-audit-logging"></a>Configuración del registro de auditoría

>[!IMPORTANT]
> Se recomienda registrar solo los tipos de evento y los usuarios necesarios con fines de auditoría para asegurarse de que el rendimiento del servidor no se ve afectado en gran medida.

Habilite y configure el registro de auditoría mediante los pasos siguientes:

1. Para activar los registros de auditoría, establezca el parámetro **audit_logs_enabled** en "ON". 
    ```azurecli-interactive
    az mysql server configuration set --name audit_log_enabled --resource-group myresourcegroup --server mydemoserver --value ON
    ```

1. Seleccione los [tipos de eventos](concepts-audit-logs.md#configure-audit-logging) que se registrarán actualizando el parámetro **audit_log_events**.
    ```azurecli-interactive
    az mysql server configuration set --name audit_log_events --resource-group myresourcegroup --server mydemoserver --value "ADMIN,CONNECTION"
    ```

1. Agregue a cualquier usuario de MySQL que se excluya del registro actualizando el parámetro **audit_log_exclude_users**. Especifique los usuarios proporcionando su nombre de usuario de MySQL.
    ```azurecli-interactive
    az mysql server configuration set --name audit_log_exclude_users --resource-group myresourcegroup --server mydemoserver --value "azure_superuser"
    ```

1. Agregue los usuarios de MySQL específicos que se deban incluir para el registro mediante la actualización del parámetro **audit_log_include_users**. Especifique los usuarios proporcionando su nombre de usuario de MySQL.
    ```azurecli-interactive
    az mysql server configuration set --name audit_log_include_users --resource-group myresourcegroup --server mydemoserver --value "sampleuser"
    ```

## <a name="next-steps"></a>Pasos siguientes
- Obtenga más información sobre los [registros de auditoría](concepts-audit-logs.md) en Azure Database for MySQL.
- Aprenda a configurar los registros de auditoría en [Azure Portal](howto-configure-audit-logs-portal.md).
