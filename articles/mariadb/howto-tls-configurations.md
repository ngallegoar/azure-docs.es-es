---
title: Configuración de TLS de Azure Database for MariaDB en Azure Portal
description: Aprenda a configurar los valores de TLS mediante Azure Portal para Azure Database for MariaDB
author: kummanish
ms.author: manishku
ms.service: mariadb
ms.topic: how-to
ms.date: 06/02/2020
ms.openlocfilehash: fac719daf05e8b319db7c86d0dbc61c2814b0a0c
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "86120354"
---
# <a name="configuring-tls-settings-in-azure-database-for-mariadb-using-azure-portal"></a>Configuración de los valores de TLS en Azure Database for MariaDB mediante Azure Portal

En este artículo se describe cómo configurar un servidor de Azure Database for MariaDB para aplicar la versión de TLS mínima para que pasen las conexiones y se denieguen todas las conexiones con una versión de TLS inferior a la versión de TLS mínima configurada, lo que mejora la seguridad de la red.

Puede aplicar la versión de TLS para conectarse a una Azure Database for MariaDB estableciendo la versión de TLS mínima para un servidor de bases de datos. Por ejemplo, si se establece la versión de TLS mínima en TLS 1.0, significa que el servidor permitirá conexiones de los clientes que usen TLS 1.0, 1.1 y 1.2 y versiones posteriores. Como alternativa, si se establece en 1.2 significa que solo se permiten conexiones de los clientes que usan TLS 1.2 o versión posterior y se rechazan todas las conexiones con TLS 1.0 y TLS 1.1.

## <a name="prerequisites"></a>Requisitos previos

Para completar esta guía, necesita:

* Una instancia de [Azure Database for MariaDB](quickstart-create-mariaDB-server-database-using-azure-portal.md)

## <a name="set-tls-configurations-for-azure-database-for-mariadb"></a>Establecimiento de las configuraciones de TLS para Azure Database for MariaDB

Siga estos pasos para establecer la versión de TLS mínima del servidor de MariaDB:

1. En [Azure Portal](https://portal.azure.com/), seleccione el servidor de Azure Database for MariaDB existente.

1. En la página del servidor de MariaDB, en **Configuración**, haga clic en **Seguridad de la conexión** para abrir la página de configuración de seguridad de la conexión.

1. En **Versión de TLS mínima**, seleccione **1.2** para denegar las conexiones con una versión de TLS inferior a TLS 1.2 para el servidor de MariaDB.

    ![Configuración de TLS para Azure Database for MariaDB](./media/howto-tls-configurations/tls-configurations.png)

1. Haga clic en **Guardar** para guardar los cambios.

1. Se enviará una notificación para confirmar la correcta habilitación de la configuración de seguridad de la conexión.

    ![Éxito de la configuración de TLS para Azure Database for MariaDB](./media/howto-tls-configurations/tls-configurations-success.png)

## <a name="next-steps"></a>Pasos siguientes

Información acerca de la [creación de alertas basadas en métricas](howto-alert-metric.md).