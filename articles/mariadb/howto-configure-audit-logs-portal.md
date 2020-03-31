---
title: Acceso a los registros de auditoría de Azure Database for MariaDB mediante Azure Portal
description: En este artículo se describe cómo configurar los registros de auditoría de Azure Database for MariaDB, y acceder a ellos, mediante Azure Portal.
author: ajlam
ms.author: andrela
ms.service: mariadb
ms.topic: conceptual
ms.date: 3/18/2020
ms.openlocfilehash: d31b7531e00199ea63c7b057dacff1b67d7cedc6
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "79528938"
---
# <a name="configure-and-access-audit-logs-in-the-azure-portal"></a>Configuración y acceso a los registros de auditoría en Azure Portal

Puede configurar los [registros de auditoría de Azure Database for MariaDB](concepts-audit-logs.md) y realizar un diagnóstico de la configuración desde Azure Portal.

> [!IMPORTANT]
> La funcionalidad del registro de auditoría se encuentra actualmente en versión preliminar.

## <a name="prerequisites"></a>Prerequisites

Para seguir esta guía, necesitará:

- [Servidor de Azure Database for MariaDB](quickstart-create-mariadb-server-database-using-azure-portal.md)

## <a name="configure-audit-logging"></a>Configuración del registro de auditoría

Habilite y configure el registro de auditoría.

1. Inicie sesión en [Azure Portal](https://portal.azure.com/).

1. Seleccione un servidor de Azure Database for MariaDB.

1. En la sección **Configuración**, seleccione **Parámetros del servidor**.
    ![Parámetros del servidor](./media/howto-configure-audit-logs-portal/server-parameters.png)

1. Actualice el parámetro **audit_log_enabled** a ACTIVADO.
    ![Habilitar los registros de auditoría](./media/howto-configure-audit-logs-portal/audit-log-enabled.png)

1. Seleccione los [tipos de eventos](concepts-audit-logs.md#configure-audit-logging) que se registrarán actualizando el parámetro **audit_log_events**.
    ![Eventos del registro de auditoría](./media/howto-configure-audit-logs-portal/audit-log-events.png)

1. Agregue a cualquier usuario de MariaDB que se excluya del registro actualizando el parámetro **audit_log_exclude_users**. Especifique los usuarios proporcionando su nombre de usuario de MariaDB.
    ![Excluir usuarios del registro de auditoría](./media/howto-configure-audit-logs-portal/audit-log-exclude-users.png)

1. Cuando haya cambiado los parámetros, haga clic en **Guardar**. O bien puede **descartar** los cambios.
    ![Guardar](./media/howto-configure-audit-logs-portal/save-parameters.png)

## <a name="set-up-diagnostic-logs"></a>Configuración de registros de diagnósticos

1. En la sección **Supervisión** de la barra lateral, seleccione **Configuración de diagnóstico**.

1. Haga clic en "+ Agregar configuración de diagnóstico" ![Agregar configuración de diagnóstico](./media/howto-configure-audit-logs-portal/add-diagnostic-setting.png)

1. Proporcione un nombre de configuración de diagnóstico.

1. Especifique qué datos se enviarán para enviar los registros de auditoría (cuenta de almacenamiento, centro de eventos y área de trabajo de Log Analytics).

1. Seleccione "MySqlAuditLogs" como el tipo de registro.
![Configurar las opciones de diagnóstico](./media/howto-configure-audit-logs-portal/configure-diagnostic-setting.png)

1. Una vez que haya configurado los receptores de datos para canalizar los registros de auditoría, puede hacer clic en **Guardar**.
![Guardar la configuración de diagnóstico](./media/howto-configure-audit-logs-portal/save-diagnostic-setting.png)

1. Acceda a los registros de auditoría mediante los receptores de datos que configuró. Los registros pueden tardar hasta 10 minutos en aparecer.

## <a name="next-steps"></a>Pasos siguientes

- Obtenga más información sobre los [registros de auditoría](concepts-audit-logs.md) en Azure Database for MariaDB.