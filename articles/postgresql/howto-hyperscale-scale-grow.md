---
title: Escalado de un grupo de servidor de Hiperescala (Citus) en Azure Database for PostgreSQL
description: Ajuste la memoria del grupo de servidores, el disco y los recursos de CPU para tratar un aumento de la carga
author: jonels-msft
ms.author: jonels
ms.service: postgresql
ms.subservice: hyperscale-citus
ms.topic: how-to
ms.date: 11/17/2020
ms.openlocfilehash: 59e6e73c99569b0a35c56d65c1a7ccdfcb394c0f
ms.sourcegitcommit: 10d00006fec1f4b69289ce18fdd0452c3458eca5
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/21/2020
ms.locfileid: "95026427"
---
# <a name="scale-a-hyperscale-citus-server-group"></a>Escalado de un grupo de servidores Hiperescala (Citus)

Azure Database for PostgreSQL:Hiperescala (Citus) proporciona escalado de autoservicio para tratar el aumento de la carga. Azure Portal permite agregar nuevos nodos de trabajo y aumentar los núcleos virtuales de los nodos existentes. Agregar nodos no provoca ningún tiempo de inactividad e incluso mover particiones a los nuevos nodos (llamado [reequilibrio de particiones](howto-hyperscale-scale-rebalance.md)) se produce sin interrumpir las consultas.

## <a name="add-worker-nodes"></a>Incorporación de nodos de trabajo

Para agregar nodos, vaya a la pestaña **Compute + storage** (Proceso y almacenamiento) en el grupo de servidores de Hiperescala (Citus).  Arrastre el control deslizante de **Números de nodo de trabajo** para cambiar el valor.

:::image type="content" source="./media/howto-hyperscale-scaling/01-sliders-workers.png" alt-text="Controles deslizantes de recursos":::

Haga clic en el botón **Guardar** para que el valor cambiado surta efecto.

> [!NOTE]
> Una vez que se ha aumentado y guardado, el número de nodos de trabajo no se puede reducir con el control deslizante.

> [!NOTE]
> Para aprovechar los nodos recién agregados, tiene que [reequilibrar las particiones de la tabla distribuida](howto-hyperscale-scale-rebalance.md), lo que significa mover algunas [particiones](concepts-hyperscale-distributed-data.md#shards) de los nodos existentes a los nuevos.

## <a name="increase-or-decrease-vcores-on-nodes"></a>Aumentar o disminuir los núcleos virtuales de los nodos

Además de agregar nodos nuevos, puede aumentar las capacidades de los nodos existentes. Ajustar la capacidad de proceso hacia arriba y hacia abajo puede ser útil en los experimentos de rendimiento, así como en el caso de cambios a corto o largo plazo en las demandas de tráfico.

Para cambiar los núcleos virtuales de todos los nodos de trabajo, ajuste el control deslizante **Núcleos virtuales** en **Configuración (por nodo de trabajo)** . Los núcleos virtuales del nodo de coordinación se pueden ajustar de forma independiente. Ajuste el control deslizante **Núcleos virtuales** en **Configuración (nodo de coordinación)** .

## <a name="next-steps"></a>Pasos siguientes

- Más información sobre el grupo de servidores en [opciones de rendimiento](concepts-hyperscale-configuration-options.md).
- [Reequilibre las particiones de tablas distribuidas](howto-hyperscale-scale-rebalance.md) de modo que todos los nodos de trabajo puedan participar en consultas paralelas.
