---
title: 'Denegación del acceso a una red pública: Azure portal - Azure Database for MariaDB'
description: Información acerca de cómo configurar la denegación del acceso a una red pública mediante Azure Portal para Azure Database for MariaDB
author: kummanish
ms.author: manishku
ms.service: mariadb
ms.topic: conceptual
ms.date: 03/10/2020
ms.openlocfilehash: 87b6033e486e9009573436628d0183c8a022aced
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "79372770"
---
# <a name="deny-public-network-access-in-azure-database-for-mariadb-using-azure-portal"></a>Denegación del acceso a una red pública en Azure Database for MariaDB mediante Azure Portal

En este artículo se describe cómo puede configurar un servidor de Azure Database for MariaDB para denegar todas las configuraciones públicas y permitir solo las conexiones a través de puntos de conexión privados para mejorar aún más la seguridad de la red.

## <a name="prerequisites"></a>Prerrequisitos

Para completar esta guía, necesita:

* Una instancia de [Azure Database for MariaDB](quickstart-create-MariaDB-server-database-using-azure-portal.md)

## <a name="set-deny-public-network-access"></a>Establecimiento de la denegación del acceso a una red pública

Siga estos pasos para establecer la denegación del acceso a una red pública del servidor de MariaDB:

1. En [Azure Portal](https://portal.azure.com/), seleccione el servidor de Azure Database for MariaDB existente.

1. En la página del servidor de MariaDB, en **Configuración**, haga clic en **Seguridad de la conexión** para abrir la página de configuración de seguridad de la conexión.

1. En Deny Public Network Access (Denegar el acceso a la red pública), seleccione **Sí** para habilitar la denegación del acceso público para el servidor de MariaDB.

    ![Denegación del acceso de red de Azure Database for MariaDB](./media/howto-deny-public-network-access/deny-public-network-access.PNG)

1. Haga clic en **Guardar** para guardar los cambios.

1. Se enviara una notificación para confirmar la correcta habilitación de la configuración de seguridad de la conexión.

    ![Denegación correcta del acceso de red de Azure Database for MariaDB](./media/howto-deny-public-network-access/deny-public-network-access-success.png)

## <a name="next-steps"></a>Pasos siguientes

Aprenda sobre la [creación de alertas de métricas](howto-alert-metric.md).