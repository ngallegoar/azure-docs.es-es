---
title: Actualización de versión principal de Azure Database for MySQL con un servidor único mediante Azure Portal
description: En este artículo se describe cómo actualizar la versión principal de Azure Database for MySQL con un servidor único mediante Azure Portal
author: ambhatna
ms.author: ambhatna
ms.service: mysql
ms.topic: how-to
ms.date: 11/16/2020
ms.openlocfilehash: 78c35e42cefa8897d9f93c3a941b4c0e8b81e5f9
ms.sourcegitcommit: c157b830430f9937a7fa7a3a6666dcb66caa338b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/17/2020
ms.locfileid: "94686548"
---
# <a name="major-version-upgrade-in-azure-database-for-mysql-single-server-using-the-azure-portal"></a>Actualización de versión principal en Azure Database for MySQL con servidor único mediante Azure Portal

> [!IMPORTANT]
> La actualización de versión principal para Azure Database for MySQL con servidor único se encuentra en versión preliminar.

En este artículo se describe cómo actualizar la versión principal local de Azure Database for MySQL con un servidor único.

Esta característica permitirá a los clientes realizar actualizaciones en contexto de sus servidores MySQL 5.6 a MySQL 5.7 con el clic de un botón sin necesidad de movimiento de datos o de cualquier cambio en la cadena de conexión de la aplicación.

> [!Note]
> * La actualización de versión principal solo está disponible para la actualización de versión principal de MySQL 5.6 a MySQL 5.7.<br>
> * No se admite la actualización de la versión principal en el servidor de réplica.

## <a name="prerequisites"></a>Requisitos previos
Para completar esta guía, necesita:
- Un [servidor único de Azure Database for MySQL](quickstart-create-mysql-server-database-using-azure-portal.md).

## <a name="perform-major-version-upgrade-from-mysql-56-to-mysql-57"></a>Actualización de la versión principal de MySQL 5.6 a MySQL 5.7

Siga estos pasos para realizar la actualización de la versión principal de su servidor de Azure Database for MySQL 5.6.

1. En [Azure Portal](https://portal.azure.com/), seleccione el servidor de Azure Database for MySQL 5.6 existente.

2. En la página **Información general**, haga clic en el botón **Actualizar** de la barra de herramientas.

3. En la sección **Actualizar**, seleccione **Aceptar** para actualizar el servidor de Azure Database for MySQL 5.6 a un servidor 5.7.

    :::image type="content" source="./media/how-to-major-version-upgrade-portal/upgrade.png" alt-text="Opción Actualizar en la información general de Azure Database for MySQL":::

4. Una notificación confirmará que la actualización se ha realizado correctamente.

## <a name="next-steps"></a>Pasos siguientes

Más información acerca de [Directiva de versión de Azure Database for MySQL](concepts-version-policy.md).