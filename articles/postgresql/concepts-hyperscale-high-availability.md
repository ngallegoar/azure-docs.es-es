---
title: 'Alta disponibilidad en Azure Database for PostgreSQL: Hiperescala (Citus)'
description: Conceptos de alta disponibilidad y recuperación ante desastres
author: jonels-msft
ms.author: jonels
ms.service: postgresql
ms.subservice: hyperscale-citus
ms.topic: conceptual
ms.date: 11/04/2019
ms.openlocfilehash: 10679ab02826fb606af65c72621f2afb609bc81b
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "74975540"
---
# <a name="high-availability-in-azure-database-for-postgresql--hyperscale-citus"></a>Alta disponibilidad en Azure Database for PostgreSQL: Hiperescala (Citus)

La alta disponibilidad (HA) evita el tiempo de inactividad de la base de datos al mantener réplicas en espera de todos los nodos de un grupo de servidores. Si un nodo deja de funcionar, Hiperescala cambia las conexiones entrantes del nodo con errores a su estado de espera. La conmutación por error se produce en pocos minutos y los nodos promocionados siempre tienen datos actualizados a través de la replicación de transmisión sincrónica por secuencias de PostgreSQL.

Para aprovechar la alta disponibilidad en el nodo coordinador, las aplicaciones de base de datos deben detectar y reintentar las conexiones quitadas y las transacciones con errores. Se podrá acceder al coordinador recién promocionado con la misma cadena de conexión.

La recuperación puede dividirse en tres fases: detección, conmutación por error y recuperación completa.  Hiperescala realiza comprobaciones periódicas de mantenimiento en todos los nodos y después de cuatro comprobaciones con errores, determina que un nodo está fuera de servicio. Después, Hiperescala promueve un estado de espera a un nodo principal (conmutación por error) y aprovisiona un nuevo modo de espera.
La replicación de transmisión por secuencias comienza y el nuevo nodo se actualiza.  Una vez que se han replicado todos los datos, el nodo ha alcanzado la recuperación completa.

### <a name="next-steps"></a>Pasos siguientes

- Obtenga información acerca de cómo [habilitar la alta disponibilidad](howto-hyperscale-high-availability.md) en un grupo de servidores de Hiperescala.
