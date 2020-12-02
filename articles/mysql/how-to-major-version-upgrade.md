---
title: Actualización de versión principal de Azure Database for MySQL con un servidor único mediante Azure Portal
description: En este artículo se describe cómo actualizar la versión principal de Azure Database for MySQL con un servidor único mediante Azure Portal
author: ambhatna
ms.author: ambhatna
ms.service: mysql
ms.topic: how-to
ms.date: 11/16/2020
ms.openlocfilehash: 080d7c09b180d5943216793119718eb6a2add79e
ms.sourcegitcommit: 1bf144dc5d7c496c4abeb95fc2f473cfa0bbed43
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/24/2020
ms.locfileid: "95753058"
---
# <a name="major-version-upgrade-in-azure-database-for-mysql-single-server-using-the-azure-portal"></a>Actualización de versión principal en Azure Database for MySQL con servidor único mediante Azure Portal

> [!IMPORTANT]
> La actualización de versión principal para Azure Database for MySQL con servidor único se encuentra en versión preliminar.

En este artículo se describe cómo actualizar la versión principal local de Azure Database for MySQL con un servidor único.

Esta característica permitirá a los clientes realizar actualizaciones en contexto de sus servidores MySQL 5.6 a MySQL 5.7 con el clic de un botón sin necesidad de movimiento de datos o de cualquier cambio en la cadena de conexión de la aplicación.

> [!Note]
> * La actualización de versión principal solo está disponible para la actualización de versión principal de MySQL 5.6 a MySQL 5.7.<br>
> * No se admite la actualización de la versión principal en el servidor de réplica.
> * El servidor no estará disponible durante la operación de actualización. Por lo tanto, se recomienda realizar las actualizaciones durante la ventana de mantenimiento planeado.

## <a name="prerequisites"></a>Prerrequisitos
Para completar esta guía, necesita:
- Un [servidor único de Azure Database for MySQL](quickstart-create-mysql-server-database-using-azure-portal.md).

## <a name="perform-major-version-upgrade-from-mysql-56-to-mysql-57"></a>Actualización de la versión principal de MySQL 5.6 a MySQL 5.7

Siga estos pasos para realizar la actualización de la versión principal de su servidor de Azure Database for MySQL 5.6.

> [!IMPORTANT]
> Se recomienda realizar la actualización primero en la copia restaurada del servidor, en lugar de actualizar directamente la copia de producción. Consulte [Procedimientos para realizar una restauración a un momento dado](howto-restore-server-portal.md#point-in-time-restore). 

1. En [Azure Portal](https://portal.azure.com/), seleccione el servidor de Azure Database for MySQL 5.6 existente.

2. En la página **Información general**, haga clic en el botón **Actualizar** de la barra de herramientas.

3. En la sección **Actualizar**, seleccione **Aceptar** para actualizar el servidor de Azure Database for MySQL 5.6 a un servidor 5.7.

    :::image type="content" source="./media/how-to-major-version-upgrade-portal/upgrade.png" alt-text="Opción Actualizar en la información general de Azure Database for MySQL":::

4. Una notificación confirmará que la actualización se ha realizado correctamente.

## <a name="frequently-asked-questions"></a>Preguntas más frecuentes

**1. ¿Cuándo estará en disponibilidad general esta característica de actualización, ya que tenemos MySQL versión 5.6 en nuestro entorno de producción y necesitamos actualizar?**

La disponibilidad general de esta característica está planeada antes de la retirada de MySQL versión 5.6. Sin embargo, la característica está preparada para producción y es totalmente compatible con Azure, por lo que puede ejecutarla con confianza en su entorno. Como procedimiento recomendado, sugerimos que la ejecute y pruebe primero en una copia restaurada del servidor para que pueda calcular el tiempo de inactividad de la actualización, así como que realice una prueba de compatibilidad de aplicaciones antes de ejecutarla en producción. Para obtener más información, consulte cómo realizar una [restauración a un momento dado](howto-restore-server-portal.md#point-in-time-restore) para crear una copia del servidor en un momento dado. 

**2. ¿El servidor tendrá un tiempo de inactividad del servidor? Si es este el caso, ¿cuánto tiempo dura?**

Sí, el servidor no estará disponible durante el proceso de actualización, por lo que se recomienda realizar esta operación durante la ventana de mantenimiento planeado. El tiempo de inactividad estimado depende del tamaño de la base de datos, el tamaño de almacenamiento aprovisionado (IOPS aprovisionado) y el número de tablas en la base de datos. El tiempo que demora la actualización es directamente proporcional al número de tablas del servidor. Se espera que las actualizaciones de los servidores de SKU básicos tarden más tiempo en la plataforma de almacenamiento estándar. Para calcular el tiempo de inactividad del entorno de su servidor, se recomienda realizar primero la actualización en la copia restaurada del servidor.  

**3. Se indica que esta opción no se admite aún en el servidor de réplica. ¿Qué implica esto específicamente?**

Actualmente, la actualización de la versión principal no se admite para el servidor de réplicas, lo que significa que no debe ejecutarse en los servidores implicados en la replicación (ya sea en el servidor de origen o de réplica). Si le gustaría probar la actualización de los servidores implicados en la replicación antes de agregar la compatibilidad con réplicas para la característica de actualización, se recomienda seguir los pasos siguientes:

1. Durante el mantenimiento planeado, [detenga la replicación y elimine el servidor de réplica](howto-read-replicas-portal.md) después de capturar su nombre y toda la información de configuración (configuración de firewall, configuración de parámetros de servidor si es distinta de la del servidor de origen).
2. Realice una actualización del servidor de origen.
3. Aprovisione un nuevo servidor de réplica de lectura con el mismo nombre y la misma configuración que capturó en el paso 1. El nuevo servidor de réplica usará la versión 5.7 automáticamente después de que el servidor de origen se actualice a la versión 5.7.

**4. ¿Qué ocurrirá si decidimos no actualizar el servidor de MySQL versión 5.6 antes del 5 de febrero de 2021?**

Todavía podrá seguir ejecutando el servidor de MySQL versión 5.6 como antes. Azure **nunca** realizará una actualización forzada en el servidor. Sin embargo, se aplicarán las restricciones documentadas en la [directiva de versión de Azure Database for MySQL](concepts-version-policy.md).

## <a name="next-steps"></a>Pasos siguientes

Más información acerca de [Directiva de versión de Azure Database for MySQL](concepts-version-policy.md).
