---
title: Configuración de TLS mediante Azure Portal para el servidor único de Azure Database for PostgreSQL
description: Aprenda a configurar los valores de TLS mediante Azure Portal para el servidor único de Azure Database for PostgreSQL
author: kummanish
ms.author: manishku
ms.service: postgresql
ms.topic: how-to
ms.date: 06/02/2020
ms.openlocfilehash: 4cf491a27fbe53a5f5bf0e8351e5bb684b3492f1
ms.sourcegitcommit: d7008edadc9993df960817ad4c5521efa69ffa9f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/08/2020
ms.locfileid: "86101994"
---
# <a name="configuring-tls-settings-in-azure-database-for-postgresql-single---server-using-azure-portal"></a>Configuración de TLS en el servidor único de Azure Database for PostgreSQL mediante Azure Portal

En este artículo se describe cómo configurar una instancia de Azure Database for PostgreSQL para aplicar la versión de TLS mínima permitida para las conexiones y para que se denieguen todas las conexiones con una versión de TLS inferior a la versión de TLS mínima configurada, lo cual mejora la seguridad de la red.

Puede aplicar la versión de TLS para conectarse a su instancia de Azure Database for PostgreSQL. Los clientes tienen ahora la opción de establecer la versión de TLS mínima para su servidor de bases de datos. Por ejemplo, si se establece la versión de TLS mínima en TLS 1.0, significa que el servidor permitirá conexiones de los clientes que usen TLS 1.0, 1.1 y 1.2 y versiones posteriores. En su lugar, si se establece en 1.2 o versiones posteriores significa que solo se permitirán conexiones de los clientes que usan TLS 1.2 o versiones posteriores, y se rechazarán todas las conexiones con TLS 1.0 y TLS 1.1.

## <a name="prerequisites"></a>Requisitos previos

Para completar esta guía, necesita:

* Una instancia de [Azure Database for PostgreSQL](quickstart-create-server-database-portal.md)

## <a name="set-tls-configurations-for-azure-database-for-postgresql---single-server"></a>Establecimiento de las configuraciones de TLS para el servidor único de Azure Database for PostgreSQL

Siga estos pasos para establecer la versión de TLS mínima de PostgreSQL:

1. En [Azure Portal](https://portal.azure.com/), seleccione un servidor existente de Azure Database for PostgreSQL.

1.  En la página del servidor único de Azure Database for PostgreSQL, en **Configuración**, haga clic en **Seguridad de la conexión** para abrir la página de configuración de seguridad de la conexión.

1. En **Versión de TLS mínima**, seleccione **1.2** para denegar las conexiones con una versión de TLS inferior a TLS 1.2 para el servidor único de PostgreSQL.

    ![Configuración de TLS para el servidor único de Azure Database for PostgreSQL](./media/howto-tls-configurations/setting-tls-value.png)

1. Haga clic en **Guardar** para guardar los cambios.

1. Se enviará una notificación para confirmar la correcta habilitación de la configuración de seguridad de la conexión.

    ![Configuración correcta de TLS para el servidor único de Azure Database for PostgreSQL](./media/howto-tls-configurations/setting-tls-value-success.png)

## <a name="next-steps"></a>Pasos siguientes

Información acerca de la [creación de alertas basadas en métricas](howto-alert-on-metric.md).