---
title: Denegación del acceso a una red pública para el servidor único de Azure Database for PostgreSQL con Azure Portal
description: Información acerca de cómo configurar la denegación del acceso a una red pública mediante el servidor único de Azure Database for PostgreSQL con Azure Portal
author: mksuni
ms.author: sumuth
ms.service: postgresql
ms.topic: how-to
ms.date: 03/10/2020
ms.openlocfilehash: bd777fe42dc8fa3ec4643fa9607f5ca8b7aac795
ms.sourcegitcommit: 80034a1819072f45c1772940953fef06d92fefc8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/03/2020
ms.locfileid: "93240331"
---
# <a name="deny-public-network-access-in-azure-database-for-postgresql-single-server-using-azure-portal"></a>Denegación del acceso a una red pública en el servidor único de Azure Database for PostgreSQL con Azure Portal

En este artículo se describe cómo puede configurar un servidor único de Azure Database for PostgreSQL para denegar todas las configuraciones públicas y permitir solo las conexiones mediante puntos de conexión privados para mejorar aún más la seguridad de la red.

## <a name="prerequisites"></a>Requisitos previos

Para completar esta guía, necesita:

* Un [servidor único de Azure Database for PostgreSQL](quickstart-create-server-database-portal.md)

## <a name="set-deny-public-network-access"></a>Establecimiento de la denegación del acceso a una red pública

Siga estos pasos para establecer la denegación del acceso a una red pública del servidor único de PostgreSQL:

1. En [Azure Portal](https://portal.azure.com/), seleccione un servidor único existente de Azure Database for PostgreSQL.

1. En la página del servidor único de PostgreSQL, en **Configuración** , haga clic en **Seguridad de la conexión** para abrir la página de configuración de seguridad de la conexión.

1. En **Denegar acceso de red pública** , seleccione **Sí** para habilitar la denegación del acceso público para el servidor único de PostgreSQL.

    :::image type="content" source="./media/howto-deny-public-network-access/deny-public-network-access.PNG" alt-text="Denegación de acceso de red del servidor único de Azure Database for PostgreSQL":::

1. Haga clic en **Guardar** para guardar los cambios.

1. Se enviará una notificación para confirmar la correcta habilitación de la configuración de seguridad de la conexión.

    :::image type="content" source="./media/howto-deny-public-network-access/deny-public-network-access-success.png" alt-text="Denegación de acceso de red correcta para el servidor único de Azure Database for PostgreSQL":::

## <a name="next-steps"></a>Pasos siguientes

Aprenda sobre la [creación de alertas de métricas](howto-alert-on-metric.md).
