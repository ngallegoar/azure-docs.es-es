---
title: 'Denegación del acceso a una red pública: Azure portal - Azure Database for MySQL'
description: Información acerca de cómo configurar la denegación del acceso a una red pública mediante Azure Portal para Azure Database for MySQL
author: kummanish
ms.author: manishku
ms.service: mysql
ms.topic: conceptual
ms.date: 03/10/2020
ms.openlocfilehash: b5f93f3a3583900810ca75f925c6a88df9102652
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/28/2020
ms.locfileid: "79372682"
---
# <a name="deny-public-network-access-in-azure-database-for-mysql-using-azure-portal"></a>Denegación del acceso a una red pública en Azure Database for MySQL mediante Azure Portal

En este artículo se describe cómo puede configurar un servidor de Azure Database for MySQL para denegar todas las configuraciones públicas y permitir solo las conexiones a través de puntos de conexión privados para mejorar aún más la seguridad de la red.

## <a name="prerequisites"></a>Prerequisites

Para completar esta guía, necesita:

* Una base de datos de [Azure Database for MySQL](quickstart-create-mysql-server-database-using-azure-portal.md)

## <a name="set-deny-public-network-access"></a>Establecimiento de la denegación del acceso a una red pública

Siga estos pasos para establecer la denegación del acceso a una red pública del servidor de MySQL:

1. En [Azure Portal](https://portal.azure.com/), seleccione el servidor de Azure Database for MySQL existente.

1. En la página del servidor de MySQL, en **Configuración**, haga clic en **Seguridad de la conexión** para abrir la página de configuración de seguridad de la conexión.

1. En **Deny Public Network Access** (Denegar acceso de red pública), seleccione **Yes** (Sí) para habilitar la denegación del acceso público para el servidor de MySQL.

    ![Denegación del acceso de red de Azure Database for MySQL](./media/howto-deny-public-network-access/setting-deny-public-network-access.PNG)

1. Haga clic en **Guardar** para guardar los cambios.

1. Se enviara una notificación para confirmar la correcta habilitación de la configuración de seguridad de la conexión.

    ![Denegación correcta del acceso de red de Azure Database for MySQL](./media/howto-deny-public-network-access/setting-deny-public-network-access-success.png)

## <a name="next-steps"></a>Pasos siguientes

Aprenda sobre la [creación de alertas de métricas](howto-alert-on-metric.md).