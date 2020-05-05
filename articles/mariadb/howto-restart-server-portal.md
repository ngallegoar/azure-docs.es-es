---
title: Reinicio de un servidor de Azure Database for MariaDB mediante Azure Portal
description: En este artículo se describe cómo reiniciar un servidor de Azure Database for MariaDB mediante Azure Portal.
author: ajlam
ms.author: andrela
ms.service: mariadb
ms.topic: conceptual
ms.date: 3/18/2020
ms.openlocfilehash: ed95304807a1a03880cc824c1a58f010203d418d
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/28/2020
ms.locfileid: "79534718"
---
# <a name="restart-azure-database-for-mariadb-server-using-azure-portal"></a>Reinicio de un servidor de Azure Database for MariaDB mediante Azure Portal
En este tema se describe cómo reiniciar un servidor de Azure Database for MariaDB. Es posible que deba reiniciar el servidor por motivos de mantenimiento, lo que causa una breve interrupción del servicio mientras el servidor realiza la operación.

Si el servicio está ocupado, se bloqueará el reinicio del servidor. Por ejemplo, el servicio puede estar procesando una operación solicitada anteriormente, como el escalado de núcleos virtuales.

El tiempo necesario para completar un reinicio depende el proceso de recuperación de MariaDB. Para reducir el tiempo de reinicio, se recomienda que minimice la cantidad de actividades que se ejecutan en el servidor antes del reinicio.

## <a name="prerequisites"></a>Prerrequisitos
Para completar esta guía, necesita:
- Un [servidor de Azure Database for MariaDB](./quickstart-create-mariadb-server-database-using-azure-portal.md)

## <a name="perform-server-restart"></a>Realización del reinicio del servidor

Los pasos siguientes reinician el servidor de MariaDB:

1. En Azure Portal, seleccione el servidor de Azure Database for MariaDB.

2. En la barra de herramientas de la página de **información general** del servidor, haga clic en **Restart** (reiniciar).

   ![Azure Database for MariaDB - Información general - botón Reiniciar](./media/howto-restart-server-portal/2-server.png)

3. Haga clic en **Sí** para confirmar el reinicio del servidor.

   ![Azure Database for MariaDB - Confirmación del reinicio](./media/howto-restart-server-portal/3-restart-confirm.png)

4. Observe que el estado del servidor cambia a "Reiniciando".

   ![Azure Database for MariaDB - Estado del reinicio](./media/howto-restart-server-portal/4-restarting-status.png)

5. Confirme que el reinicio del servidor es correcto.

   ![Azure Database for MariaDB - Reinicio correcto](./media/howto-restart-server-portal/5-restart-success.png)

## <a name="next-steps"></a>Pasos siguientes

[Inicio rápido: Creación de un servidor de Azure Database for MariaDB mediante Azure Portal](./quickstart-create-mariadb-server-database-using-azure-portal.md)