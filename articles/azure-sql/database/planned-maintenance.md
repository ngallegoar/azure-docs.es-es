---
title: Planeación de los eventos de mantenimiento de Azure
description: Obtenga información sobre cómo prepararse para los eventos de mantenimiento planeado en Azure SQL Database e Instancia administrada de Azure SQL.
services: sql-database
ms.service: sql-database
ms.subservice: operations
ms.custom: sqldbrb=1
ms.devlang: ''
ms.topic: conceptual
author: aamalvea
ms.author: aamalvea
ms.reviewer: carlrab
ms.date: 01/30/2019
ms.openlocfilehash: 2f5f69a5f145ae8bbf23aa1e5dbca07b30db0e21
ms.sourcegitcommit: 053e5e7103ab666454faf26ed51b0dfcd7661996
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/27/2020
ms.locfileid: "84027756"
---
# <a name="planning-for-azure-maintenance-events-in-azure-sql-database-and-azure-sql-managed-instance"></a>Planeación de eventos de mantenimiento de Azure en Azure SQL Database e Instancia administrada de Azure SQL
[!INCLUDE[appliesto-sqldb-sqlmi](../includes/appliesto-sqldb-sqlmi.md)]

Obtenga información sobre cómo prepararse para los eventos de mantenimiento planeado en la base de datos en Azure SQL Database e Instancia administrada de Azure SQL.

## <a name="what-is-a-planned-maintenance-event"></a>Qué es un evento de mantenimiento planeado

Para cada base de datos, Azure SQL Database e Instancia administrada de Azure SQL mantienen un cuórum de réplicas de base de datos en el que una de ellas es la principal. En todo momento una réplica principal debe realizar mantenimiento en línea, mientras que al menos una réplica secundaria debe estar en buen estado. Durante el mantenimiento planeado, los miembros del cuórum de la base de datos se quedarán sin conexión una a la vez, con la intención de que haya una réplica principal respondiendo y al menos una réplica secundaria en línea, de forma que no haya tiempo de inactividad en el cliente. Cuando sea necesario que la réplica principal esté sin conexión, se producirá un proceso de reconfiguración o conmutación por error en el que una réplica secundaria se convertirá en la nueva réplica principal.  

## <a name="what-to-expect-during-a-planned-maintenance-event"></a>Qué esperar durante un evento de mantenimiento planeado

Las reconfiguraciones o conmutaciones por error normalmente se completan en 30 segundos; el promedio es de 8 segundos. Si ya está conectada, la aplicación debe volver a conectarse a la nueva copia correcta de la réplica principal de la base de datos. Si se intenta establecer una nueva conexión mientras la base de datos está realizando una reconfiguración antes de que la nueva réplica esté en línea, se devolverá el error 40613 (base de datos no disponible): "La base de datos '{nombre de la base de datos}' del servidor '{nombre del servidor}' no está disponible actualmente. Vuelva a intentar la conexión más tarde". Si la base de datos tiene una consulta de larga duración, esta consulta se interrumpirá durante la reconfiguración y deberá reiniciarse.

## <a name="retry-logic"></a>Lógica de reintento

Todas las aplicaciones cliente de producción que se conecten a un servicio de base de datos en la nube deben implementar una [lógica de reintento](troubleshoot-common-connectivity-issues.md#retry-logic-for-transient-errors) de conexión sólida. Así, ayudarán a mitigar estas situaciones y, por lo general, harán que el usuario final no tenga noticias de los errores.

## <a name="frequency"></a>Frecuencia

En promedio, se producen 1,7 eventos de mantenimiento planeado cada mes.

## <a name="resource-health"></a>Estado de los recursos

Si la base de datos experimenta errores de inicio de sesión, compruebe la ventana de [Resource Health](../../service-health/resource-health-overview.md#get-started) en [Azure Portal](https://portal.azure.com) para conocer el estado actual. La sección del historial de estado contiene el motivo del tiempo de inactividad de cada evento (si está disponible).

## <a name="next-steps"></a>Pasos siguientes

- Más información sobre [Resource Health](resource-health-to-troubleshoot-connectivity.md) para Azure SQL Database e Instancia administrada de Azure SQL
- Para obtener más información sobre la lógica de reintento, vea [Lógica de reintento para errores transitorios](troubleshoot-common-connectivity-issues.md#retry-logic-for-transient-errors).
