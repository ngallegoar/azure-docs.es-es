---
title: 'Tutorial: Replicación geográfica y conmutación por error en el portal'
description: Configure la replicación geográfica de una base de datos única o agrupada en Azure SQL Database mediante Azure Portal e inicie la conmutación por error.
services: sql-database
ms.service: sql-database
ms.subservice: high-availability
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: anosov1960
ms.author: sashan
ms.reviewer: mathoma, carlrab
ms.date: 02/13/2019
ms.openlocfilehash: 601c537a51e29ae1f107127e1b83c07448eee9ab
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "79228688"
---
# <a name="configure-active-geo-replication-for-azure-sql-database-in-the-azure-portal-and-initiate-failover"></a>Configuración de replicación geográfica activa para Azure SQL Database en Azure Portal e inicio de la conmutación por error

En este artículo se muestra cómo configurar la [replicación geográfica activa para bases de datos únicas y agrupadas](sql-database-active-geo-replication.md#active-geo-replication-terminology-and-capabilities) en Azure SQL Database mediante [Azure Portal](https://portal.azure.com) y cómo iniciar la conmutación por error.

Para información sobre los grupos de conmutación por error automática con bases de datos únicas y agrupadas, consulte [Procedimientos recomendados para usar grupos de conmutación por error con bases de datos únicas y agrupadas](sql-database-auto-failover-group.md#best-practices-of-using-failover-groups-with-single-databases-and-elastic-pools). Para información sobre los grupos de conmutación por error automática con instancias administradas, consulte [Procedimientos recomendados para usar grupos de conmutación por error con instancias administradas](sql-database-auto-failover-group.md#best-practices-of-using-failover-groups-with-managed-instances).

## <a name="prerequisites"></a>Prerrequisitos

Para configurar la replicación geográfica activa mediante Azure Portal, necesita el siguiente recurso:

* Una base de datos de Azure SQL: la base de datos principal que quiere replicar en una región geográfica diferente.

> [!Note]
> Al usar Azure Portal, solo se puede crear una base de datos secundaria en la misma suscripción que la principal. Si la base de datos secundaria debe estar en una suscripción diferente, use [Create Database REST API](https://docs.microsoft.com/rest/api/sql/databases/createorupdate) (Creación de la API REST de base de datos) o [ALTER DATABASE (Transact-SQL)](https://docs.microsoft.com/sql/t-sql/statements/alter-database-transact-sql).

## <a name="add-a-secondary-database"></a>Adición de una base de datos secundaria

Los pasos siguientes crean otra base de datos secundaria en una asociación de replicación geográfica.  

Para agregar una base de datos secundaria, debe ser el propietario o copropietario de la suscripción.

La base de datos secundaria tiene el mismo nombre que la principal y, de forma predeterminada, presentan el mismo nivel de servicio y tamaño de proceso. La base de datos secundaria puede ser una base de datos única o una base de datos agrupada. Para obtener más información, consulte los artículos sobre el [modelo de compra basado en DTU](sql-database-service-tiers-dtu.md) y el [modelo de compra basado en núcleo virtual](sql-database-service-tiers-vcore.md).
Después de crear e inicializar la base de datos secundaria, los datos comienzan a replicarse desde la base de datos principal a la nueva base de datos secundaria.

> [!NOTE]
> Si la base de datos del asociado ya existe (por ejemplo, como resultado de la terminación de una relación de replicación geográfica anterior), se produce un error en el comando.

1. En [Azure Portal](https://portal.azure.com), vaya a la base de datos que desea configurar para replicación geográfica.
2. En la página de SQL Database, seleccione **Replicación geográfica** y, luego, seleccione la región para crear la base de datos secundaria. Puede seleccionar cualquier región menos la que hospeda la base de datos principal, pero se recomienda la [región emparejada](../best-practices-availability-paired-regions.md).

    ![Configuración de la replicación geográfica](./media/sql-database-geo-replication-portal/configure-geo-replication.png)
3. Seleccione o configure el servidor y el plan de tarifa de la base de datos secundaria.

    ![Configuración de la secundaria](./media/sql-database-geo-replication-portal/create-secondary.png)
4. También puede agregar una base de datos secundaria a un grupo elástico. Para crear una base de datos secundaria en un grupo, haga clic en **Grupo elástico** y seleccione un grupo del servidor de destino. Ya debe existir un grupo en el servidor de destino. Este flujo de trabajo no crea un grupo.
5. Haga clic en **Crear** para agregar la base de datos secundaria.
6. Se crea la base de datos secundaria y comienza el proceso de inicialización.

    ![Configuración de la secundaria](./media/sql-database-geo-replication-portal/seeding0.png)
7. Cuando se completa el proceso de propagación, la base de datos secundaria muestra su estado.

    ![Propagación completa](./media/sql-database-geo-replication-portal/seeding-complete.png)

## <a name="initiate-a-failover"></a>Inicio de una conmutación por error

La base de datos secundaria se puede cambiar para convertirse en la principal.  

1. En [Azure Portal](https://portal.azure.com), vaya a la base de datos principal de la asociación de replicación geográfica.
2. En la hoja SQL Database, seleccione **All settings** (Toda la configuración)  > **Replicación geográfica**.
3. En la lista **SECUNDARIAS**, seleccione la base de datos que quiere convertir en la nueva base de datos principal y haga clic en **Conmutación por error**.

    ![failover](./media/sql-database-geo-replication-failover-portal/secondaries.png)
4. Haga clic en **Sí** para iniciar la conmutación por error.

El comando cambiará inmediatamente la base de datos secundaria al rol principal. Este proceso normalmente tardará 30 segundos o menos.

Hay un breve período durante el que ambas bases de datos no están disponibles (del orden de 0 a 25 segundos) mientras se cambian los roles. Si la base de datos principal tiene varias bases de datos secundarias, el comando reconfigura automáticamente las demás secundarias para conectarse a la nueva principal. En circunstancias normales, toda la operación debería tardar menos de un minuto en completarse.

> [!NOTE]
> Este comando está diseñado para una rápida recuperación de la base de datos en el caso de una interrupción. Desencadena una conmutación por error sin sincronización de datos (conmutación por error forzada).  Si la principal está conectada y confirmando transacciones cuando se emite el comando, es posible que produzca alguna pérdida de datos.

## <a name="remove-secondary-database"></a>Quitar una base de datos secundaria

Esta operación termina de forma permanente la replicación en la base de datos secundaria y el rol de la base de datos secundaria cambia al de una base de datos de lectura y escritura normal. Si se interrumpe la conectividad con la base de datos secundaria, el comando se ejecuta correctamente, pero la base de datos secundaria no pasa a ser de lectura y escritura hasta después de restaurarse la conectividad.  

1. En [Azure Portal](https://portal.azure.com), vaya a la base de datos principal de la asociación de replicación geográfica.
2. En la página de SQL Database, seleccione **Replicación geográfica**.
3. En la lista **SECUNDARIAS**, seleccione la base de datos que desee quitar de la asociación de replicación geográfica.
4. Haga clic en **Detener replicación**.

    ![Quitar secundaria](./media/sql-database-geo-replication-portal/remove-secondary.png)
5. Se abrirá una ventana de confirmación. Haga clic en **Sí** para quitar la base de datos de la asociación de replicación geográfica. (Establezca el valor en una base de datos de lectura y escritura que no forme parte de ninguna replicación).

## <a name="next-steps"></a>Pasos siguientes

* Para obtener más información sobre la replicación geográfica activa, consulte [Replicación geográfica activa](sql-database-active-geo-replication.md).
* Para información sobre los grupos de conmutación por error automática, consulte [Grupos de conmutación por error automática](sql-database-auto-failover-group.md).
* Para obtener una descripción general y los escenarios de la continuidad empresarial, consulte [Información general sobre la continuidad empresarial](sql-database-business-continuity.md).
