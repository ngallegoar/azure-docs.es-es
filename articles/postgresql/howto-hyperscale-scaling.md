---
title: Escalado de un grupo de servidor de Hiperescala (Citus) en Azure Database for PostgreSQL
description: Ajuste la memoria del grupo de servidores, el disco y los recursos de CPU para tratar un aumento de la carga
author: jonels-msft
ms.author: jonels
ms.service: postgresql
ms.subservice: hyperscale-citus
ms.topic: conceptual
ms.date: 3/16/2020
ms.openlocfilehash: f8e8e1672f754e843a3bd1c75d496599d31e5f11
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/02/2020
ms.locfileid: "82584003"
---
# <a name="scale-a-hyperscale-citus-server-group"></a>Escalado de un grupo de servidores Hiperescala (Citus)

Azure Database for PostgreSQL:Hiperescala (Citus) proporciona escalado de autoservicio para tratar el aumento de la carga. Azure Portal permite agregar nuevos nodos de trabajo y aumentar los núcleos virtuales de los nodos existentes.

## <a name="add-worker-nodes"></a>Incorporación de nodos de trabajo

Para agregar nodos, vaya a la pestaña **Configurar** en el grupo de servidores de Hiperescala (Citus).  Arrastre el control deslizante de **Números de nodo de trabajo** para cambiar el valor.

![Controles deslizantes de recursos](./media/howto-hyperscale-scaling/01-sliders-workers.png)

Haga clic en el botón **Guardar** para que el valor cambiado surta efecto.

> [!NOTE]
> Una vez que se ha aumentado y guardado, el número de nodos de trabajo no se puede reducir con el control deslizante.

### <a name="rebalance-shards"></a>Reequilibrio de particiones

Para aprovechar los nodos recién agregados, tiene que reequilibrar la tabla distribuida [particiones de base de datos](concepts-hyperscale-distributed-data.md#shards), lo que significa mover algunas particiones de los nodos existentes a las nuevas. En primer lugar, compruebe que los trabajos nuevos hayan finalizado correctamente el aprovisionamiento. Luego, inicie el reequilibrador de particiones conectándose al nodo coordinador de clústeres con psql y ejecute:

```sql
SELECT rebalance_table_shards('distributed_table_name');
```

La función `rebalance_table_shards` vuelve a equilibrar todas las tablas del grupo [colocación](concepts-hyperscale-colocation.md)de la tabla nombrada en su argumento. Por lo tanto, no tiene que llamar a la función para cada tabla distribuida, simplemente llámela en una tabla representativa de cada grupo de ubicación.

## <a name="increase-or-decrease-vcores-on-nodes"></a>Aumentar o disminuir los núcleos virtuales de los nodos

> [!NOTE]
> Esta funcionalidad actualmente está en su versión preliminar. Para solicitar un cambio en los núcleos virtuales de los nodos del grupo de servidores, [póngase en contacto con el servicio de soporte técnico de Azure](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade).

Además de agregar nodos nuevos, puede aumentar las capacidades de los nodos existentes. Ajustar la capacidad de proceso hacia arriba y hacia abajo puede ser útil en los experimentos de rendimiento, así como en el caso de cambios a corto o largo plazo en las demandas de tráfico.

Para cambiar los núcleos virtuales de todos los nodos de trabajo, ajuste el control deslizante **Núcleos virtuales** en **Configuración (por nodo de trabajo)** . Los núcleos virtuales del nodo de coordinación se pueden ajustar de forma independiente. Haga clic en el vínculo **Cambiar configuración** en el **nodo de coordinación**. Aparecerá un cuadro de diálogo con controles deslizantes para los núcleos virtuales y la capacidad de almacenamiento del coordinador. Cambie los controles deslizantes como prefiera y seleccione **Aceptar**.

## <a name="next-steps"></a>Pasos siguientes

Más información sobre el grupo de servidores en [opciones de rendimiento](concepts-hyperscale-configuration-options.md).
