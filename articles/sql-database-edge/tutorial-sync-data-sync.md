---
title: Sincronización de datos desde Azure SQL Database Edge mediante SQL Data Sync | Microsoft Docs
description: Aprenda a sincronizar datos desde Azure SQL Database Edge mediante Azure SQL Data Sync
keywords: sql database edge,sync data from sql database edge, sql database edge data sync
services: sql-database-edge
ms.service: sql-database-edge
ms.topic: tutorial
author: SQLSourabh
ms.author: sourabha
ms.reviewer: sstein
ms.date: 11/04/2019
ms.openlocfilehash: 7c38ba6dbabef4affd8672295a93d46fd4b0e494
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/24/2020
ms.locfileid: "74384179"
---
# <a name="tutorial-sync-data-from-sql-database-edge-to-azure-sql-database-by-using-sql-data-sync"></a>Tutorial: Sincronización de datos desde SQL Database Edge a Azure SQL Database mediante SQL Data Sync

En este tutorial, aprenderá a usar un *grupo de sincronización* de Azure SQL Data Sync para sincronizar incrementalmente los datos de Azure SQL Database Edge con Azure SQL Database. SQL Data Sync es un servicio basado en Azure SQL Database que permite sincronizar los datos seleccionados de manera bidireccional entre varias bases de datos SQL e instancias de SQL Server. Para más información sobre SQL Data Sync, consulte [Azure SQL Data Sync](../sql-database/sql-database-sync-data.md).

Dado que SQL Database Edge se basa en las versiones más recientes del [Motor de base de datos de SQL Server](/sql/sql-server/sql-server-technical-documentation/), cualquier mecanismo de sincronización de datos que sea aplicable a una instancia de SQL Server local también se puede usar para sincronizar datos con la instancia de SQL Database Edge que se ejecute en un dispositivo perimetral.

## <a name="prerequisites"></a>Prerrequisitos

Para este tutorial se requiere un equipo Windows configurado con [Data Sync Agent para Azure SQL Data Sync](../sql-database/sql-database-data-sync-agent.md).

## <a name="before-you-begin"></a>Antes de empezar

* Cree una base de datos de Azure SQL. Para obtener información sobre cómo crear una base de datos de Azure SQL desde Azure Portal, consulte [Creación de una base de datos única en Azure SQL Database](../sql-database/sql-database-single-database-get-started.md?tabs=azure-portal).

* Cree las tablas y otros objetos necesarios en la implementación de Azure SQL Database.

* Cree las tablas y objetos necesarios en la implementación de Azure SQL Database Edge. Para más información, vea [Uso de paquetes DAC de SQL Database con SQL Database Edge](stream-analytics.md).

* Registre la instancia de Azure SQL Database Edge con Data Sync Agent para Azure SQL Data Sync. Para más información, consulte [Incorporación de una base de datos local de SQL Server](../sql-database/sql-database-get-started-sql-data-sync.md#add-on-prem).

## <a name="sync-data-between-an-azure-sql-database-and-sql-database-edge"></a>Sincronización de datos entre una base de datos de Azure SQL y SQL Database Edge

La configuración de la sincronización entre una base de datos de Azure SQL y una instancia de SQL Database Edge mediante SQL Data Sync implica tres pasos clave:  

1. Use Azure Portal para crear un grupo de sincronización. Para más información, consulte [Creación de un área de trabajo](../sql-database/sql-database-get-started-sql-data-sync.md#create-sync-group). Puede usar una sola base de datos *Centro* para crear varios grupos de sincronización para sincronizar datos de distintas instancias de SQL Database Edge con una o varias bases de datos SQL en Azure.

2. Agregue miembros de sincronización al grupo de sincronización. Para más información, consulte [Adición de miembros de sincronización](../sql-database/sql-database-get-started-sql-data-sync.md#add-sync-members).

3. Configure el grupo de sincronización para seleccionar las tablas que formarán parte de la sincronización. Para más información, consulte [Configuración de un grupo de sincronización](../sql-database/sql-database-get-started-sql-data-sync.md#add-sync-members).

Después de completar los pasos anteriores, tendrá un grupo de sincronización que incluye una base de datos de Azure SQL y una instancia de SQL Database Edge.

Para más información sobre SQL Data Sync, consulte estos artículos:

* [Data Sync Agent para Azure SQL Data Sync](../sql-database/sql-database-data-sync-agent.md)

* [Procedimientos recomendados](../sql-database/sql-database-best-practices-data-sync.md) y [Solución de problemas de Azure SQL Data Sync](../sql-database/sql-database-troubleshoot-data-sync.md)

* [Monitor SQL Data Sync with Azure Monitor logs](../sql-database/sql-database-sync-monitor-oms.md) (Supervisión de SQL Data Sync con registros de Azure Monitor)

* [Actualización del esquema de sincronización con Transact-SQL](../sql-database/sql-database-update-sync-schema.md) o [PowerShell](../sql-database/scripts/sql-database-sync-update-schema.md)

## <a name="next-steps"></a>Pasos siguientes

* [Uso de PowerShell para sincronizar entre Azure SQL Database y Azure SQL Database Edge](../sql-database/scripts/sql-database-sync-data-between-azure-onprem.md). En el tutorial, reemplace los detalles de la base de datos `OnPremiseServer` por los detalles de Azure SQL Database Edge.
