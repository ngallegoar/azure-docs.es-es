---
title: Optimización de la caché de Gen2
description: Obtenga información sobre cómo supervisar la caché de Gen2 mediante Azure Portal.
services: synapse-analytics
author: kevinvngo
manager: craigg
ms.service: synapse-analytics
ms.subservice: sql-dw
ms.topic: conceptual
ms.date: 09/06/2018
ms.author: kevin
ms.reviewer: igorstan
ms.custom: azure-synapse
ms.openlocfilehash: 4d66a1174b1b4adc94b24c6aecd55b2b8679f2f7
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "85211891"
---
# <a name="how-to-monitor-the-gen2-cache"></a>Cómo supervisar la caché de Gen2

En este artículo se describe cómo supervisar y solucionar problemas de rendimiento lento de las consultas determinando si la carga de trabajo aprovecha de forma óptima la caché de Gen2.

La arquitectura de almacenamiento Gen2 organiza de forma automática por niveles los segmentos de almacén de columnas consultados con más frecuencia en una memoria caché que reside en discos SSD basados en NVMe diseñados para almacenes de datos de Gen2. El aumento de rendimiento se consigue cuando las consultas recuperan segmentos que se encuentran en la caché.
 
## <a name="troubleshoot-using-the-azure-portal"></a>Solución de problemas mediante Azure Portal

Puede usar a Azure Monitor para ver las métricas de caché de Gen2 para solucionar los problemas de rendimiento de las consultas. En primera lugar, vaya a Azure Portal y haga clic en **Supervisar**, **Métricas** y **+ Seleccionar un ámbito**:

![Azure Monitor](./media/sql-data-warehouse-how-to-monitor-cache/cache-0.png)

Use las barras desplegable y de búsqueda para buscar el almacenamiento de datos. Luego seleccione Aplicar.

![Azure Monitor](./media/sql-data-warehouse-how-to-monitor-cache/cache-1.png)

Las métricas clave para solucionar problemas de la caché de Gen2 son **Porcentaje de aciertos de caché** y **Porcentaje de caché usada**. Seleccione **Porcentaje de aciertos de caché** y luego use el botón **Agregar métrica** para agregar **Porcentaje de caché usada**. 

![Métricas de caché](./media/sql-data-warehouse-how-to-monitor-cache/cache-2.png)

## <a name="cache-hit-and-used-percentage"></a>Aciertos de caché y Porcentaje de caché usada

En la tabla siguiente se describen escenarios en función de los valores de las métricas de caché:

|                                | **Porcentaje de aciertos de caché alto** | **Porcentaje de aciertos de caché bajo** |
| :----------------------------: | :---------------------------: | :--------------------------: |
| **Porcentaje de caché usada alto** |          Escenario 1           |          Escenario 2          |
| **Porcentaje de caché usada bajo**  |          Escenario 3           |          Escenario 4          |

**Escenario 1:** la caché se está usando de forma óptima. [Solucione los problemas](sql-data-warehouse-manage-monitor.md) de otras áreas que puedan ralentizar las consultas.

**Escenario 2:** el conjunto de datos de trabajo actual no cabe en la caché, lo que provoca un porcentaje de aciertos de caché bajo debido a las lecturas físicas. Considere la posibilidad de escalar verticalmente el nivel de rendimiento y vuelva a ejecutar la carga de trabajo para rellenar la caché.

**Escenario 3:** es probable que la consulta se ejecute lentamente debido a razones no relacionadas con la caché. [Solucione los problemas](sql-data-warehouse-manage-monitor.md) de otras áreas que puedan ralentizar las consultas. También puede considerar la posibilidad de [reducir verticalmente la instancia](sql-data-warehouse-manage-monitor.md) para reducir el tamaño de la caché con el fin de ahorrar costos. 

**Escenario 4:** tenía una caché inactiva que podría ser el motivo de la lentitud de la consulta. Considere la posibilidad de volver a ejecutar la consulta, ya que ahora el conjunto de datos de trabajo debería estar almacenado en la caché. 

> [!IMPORTANT]
> Si el porcentaje de aciertos de caché o el porcentaje de caché usada no se actualiza después de volver a ejecutar la carga de trabajo, es posible que el conjunto de trabajo ya se encuentre en la memoria. En la caché solo se guardan las tablas de almacén de columnas en clúster.

## <a name="next-steps"></a>Pasos siguientes
Para obtener más información sobre la optimización del rendimiento general de las consultas, vea [Supervisión de ejecuciones de consultas](sql-data-warehouse-manage-monitor.md#monitor-query-execution).
