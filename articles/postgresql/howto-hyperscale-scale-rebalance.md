---
title: 'Reequilibrio de particiones en Hiperescala (Citus): Azure Database for PostgreSQL'
description: Distribuya las particiones de manera uniforme entre servidores para mejorar el rendimiento.
author: jonels-msft
ms.author: jonels
ms.service: postgresql
ms.subservice: hyperscale-citus
ms.topic: how-to
ms.date: 11/17/2020
ms.openlocfilehash: b69c4fc3ff16cf30181a3529f61af7126b92950b
ms.sourcegitcommit: 10d00006fec1f4b69289ce18fdd0452c3458eca5
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/21/2020
ms.locfileid: "95026393"
---
# <a name="rebalance-shards-in-hyperscale-citus-server-group"></a>Reequilibrio de particiones en un grupo de servidores de Hiperescala (Citus)

Para aprovechar los nodos recién agregados, tiene que reequilibrar la tabla distribuida [particiones de base de datos](concepts-hyperscale-distributed-data.md#shards), lo que significa mover algunas particiones de los nodos existentes a las nuevas. En primer lugar, compruebe que los trabajos nuevos hayan finalizado correctamente el aprovisionamiento. Luego, inicie el reequilibrador de particiones conectándose al nodo coordinador de clústeres con psql y ejecute:

```sql
SELECT rebalance_table_shards('distributed_table_name');
```

La función [rebalance_table_shards](reference-hyperscale-functions.md#rebalance_table_shards) vuelve a equilibrar todas las tablas del grupo [colocación](concepts-hyperscale-colocation.md) de la tabla nombrada en su argumento. Por lo tanto, no tiene que llamar a la función para cada tabla distribuida, simplemente llámela en una tabla representativa de cada grupo de ubicación.

**Pasos siguientes**


- Más información sobre el grupo de servidores en [opciones de rendimiento](concepts-hyperscale-configuration-options.md).
- [Escalado de un grupo de servidores](howto-hyperscale-scale-grow.md), vertical u horizontal.
- Consulte el material de referencia de [rebalance_table_shards](reference-hyperscale-functions.md#rebalance_table_shards).
