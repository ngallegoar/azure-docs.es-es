---
title: 'Registros: Hiperescala (Citus) en Azure Database for PostgreSQL'
description: 'Procedimiento para acceder a los registros de base de datos para Azure Database for PostgreSQL: Hiperescala (Citus)'
author: jonels-msft
ms.author: jonels
ms.service: postgresql
ms.subservice: hyperscale-citus
ms.topic: conceptual
ms.date: 7/13/2020
ms.openlocfilehash: fac0db7f8c94113ff87d1c0a3091e4c0cbd5ed2b
ms.sourcegitcommit: 3bcce2e26935f523226ea269f034e0d75aa6693a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/23/2020
ms.locfileid: "92489853"
---
# <a name="logs-in-azure-database-for-postgresql---hyperscale-citus"></a>Registros en Hiperescala (Citus) para Azure Database for PostgreSQL

Los registros de PostgreSQL están disponibles en cada nodo de un grupo de servidores de Hiperescala (Citus). Puede enviar registros a un servidor de almacenamiento o a un servicio de análisis. Los registros se pueden usar para identificar, solucionar y reparar errores de configuración y casos de rendimiento no óptimo.

## <a name="accessing-logs"></a>Acceso a los registros

Para acceder a los registros de PostgreSQL para un nodo de trabajo o coordinador de Hiperescala (Citus), abra el nodo en Azure Portal:

:::image type="content" source="media/howto-hyperscale-logging/choose-node.png" alt-text="lista de nodos":::

En el nodo seleccionado, abra **Configuración de diagnóstico** y haga clic en **+ Agregar configuración de diagnóstico** .

:::image type="content" source="media/howto-hyperscale-logging/diagnostic-settings.png" alt-text="lista de nodos":::

Elija un nombre para la nueva configuración de diagnóstico y active la casilla **PostgreSQLLogs** .  Elija el o los destinos que deben recibir los registros.

:::image type="content" source="media/howto-hyperscale-logging/diagnostic-create-setting.png" alt-text="lista de nodos":::

## <a name="next-steps"></a>Pasos siguientes

- [Introducción a las consultas de Log Analytics](../azure-monitor/log-query/get-started-portal.md)
- Información acerca de [Azure Event Hubs](../event-hubs/event-hubs-about.md)