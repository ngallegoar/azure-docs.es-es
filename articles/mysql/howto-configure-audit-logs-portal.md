---
title: Acceso a registros de auditoría mediante Azure Portal en Azure Database for MySQL
description: En este artículo se describe cómo configurar los registros de auditoría de Azure Database for MySQL, y acceder a ellos, mediante Azure Portal.
author: ajlam
ms.author: andrela
ms.service: mysql
ms.topic: how-to
ms.date: 6/24/2020
ms.openlocfilehash: 508e2d229c067ac84d4c8d6338e658df8d3fa932
ms.sourcegitcommit: d7008edadc9993df960817ad4c5521efa69ffa9f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/08/2020
ms.locfileid: "86113214"
---
# <a name="configure-and-access-audit-logs-for-azure-database-for-mysql-in-the-azure-portal"></a>Configuración de los registros de auditoría de Azure Database for MySQL y acceso a ellos en Azure Portal

Puede configurar los [registros de auditoría de Azure Database for MySQL](concepts-audit-logs.md) y realizar un diagnóstico de la configuración desde Azure Portal.

## <a name="prerequisites"></a>Requisitos previos

Para seguir esta guía, necesitará:

- [Servidor de Azure Database for MySQL](quickstart-create-mysql-server-database-using-azure-portal.md)

## <a name="configure-audit-logging"></a>Configuración del registro de auditoría

>[!IMPORTANT]
> Se recomienda registrar solo los tipos de evento y los usuarios necesarios con fines de auditoría para asegurarse de que el rendimiento del servidor no se ve afectado en gran medida.

Habilite y configure el registro de auditoría.

1. Inicie sesión en [Azure Portal](https://portal.azure.com/).

1. Seleccione un servidor de Azure Database for MySQL.

1. En la sección **Configuración**, seleccione **Parámetros del servidor**.
    ![Parámetros del servidor](./media/howto-configure-audit-logs-portal/server-parameters.png)

1. Actualice el parámetro **audit_log_enabled** a ACTIVADO.
    ![Habilitar los registros de auditoría](./media/howto-configure-audit-logs-portal/audit-log-enabled.png)

1. Seleccione los [tipos de eventos](concepts-audit-logs.md#configure-audit-logging) que se registrarán actualizando el parámetro **audit_log_events**.
    ![Eventos del registro de auditoría](./media/howto-configure-audit-logs-portal/audit-log-events.png)

1. Agregue a cualquier usuario de MySQL que se excluya del registro actualizando el parámetro **audit_log_exclude_users**. Especifique los usuarios proporcionando su nombre de usuario de MySQL.
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

- Obtenga más información sobre los [registros de auditoría](concepts-audit-logs.md) en Azure Database for MySQL.
- Aprenda a configurar los registros de auditoría en la [CLI de Azure](howto-configure-audit-logs-cli.md).