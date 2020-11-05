---
title: Inicio o detención de un servidor flexible de Azure Database for MySQL mediante Azure Portal
description: En este artículo se explica cómo iniciar y detener operaciones en Azure Database for MySQL mediante Azure Portal.
author: mksuni
ms.author: sumuth
ms.service: mysql
ms.topic: how-to
ms.date: 09/29/2020
ms.openlocfilehash: 15b08ea67afe0d307470b5a4fb0f7d26e0f4ea82
ms.sourcegitcommit: 80034a1819072f45c1772940953fef06d92fefc8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/03/2020
ms.locfileid: "93241929"
---
# <a name="stopstart-an-azure-database-for-mysql---flexible-server-preview"></a>Inicio o detención de un servidor flexible de Azure Database for MySQL (versión preliminar)

> [!IMPORTANT]
> Actualmente, la opción de implementación Servidor flexible de Azure Database for MySQL se encuentra en versión preliminar pública.

En este artículo se proporciona un procedimiento detallado para iniciar y detener un servidor flexible.

## <a name="prerequisites"></a>Requisitos previos

Para completar esta guía, necesita:

-   Debe tener un servidor flexible de Azure Database for MySQL.

## <a name="stop-a-running-server"></a>Detención de un servidor en ejecución

1.  En [Azure Portal](https://portal.azure.com/), elija el servidor flexible que quiera detener.

2.  En la página **Información general** , haga clic en el botón **Detener** de la barra de herramientas.
    
    :::image type="content" source="media/how-to-stop-start-server-portal/stop-server.png" alt-text="Detenga el servidor flexible."::: 

3.  Haga clic en **Sí** para confirmar la detención del servidor.

    :::image type="content" source="media/how-to-stop-start-server-portal/confirm-stop.png" alt-text="Confirme la detención del servidor flexible."::: 

> [!NOTE]
> Una vez detenido el servidor, las restantes operaciones de administración no están disponibles para el servidor flexible.

## <a name="start-a-stopped-server"></a>Inicio de un servidor detenido

1.  En [Azure Portal](https://portal.azure.com/), elija el servidor flexible que desea iniciar.

2.  En la página **Información general** , haga clic en el botón **Iniciar** de la barra de herramientas.

    :::image type="content" source="media/how-to-stop-start-server-portal/start-server.png" alt-text="Inicie el servidor flexible.":::  

> [!NOTE]
> Una vez iniciado el servidor, todas las operaciones de administración están disponibles para el servidor flexible.

## <a name="next-steps"></a>Pasos siguientes
- Obtenga más información sobre la [conexión de red en servidores flexibles de Azure Database for MySQL](./concepts-networking.md).
- [Creación y administración de una red virtual para un servidor flexible de Azure Database for MySQL mediante Azure Portal](./how-to-manage-virtual-network-portal.md).

