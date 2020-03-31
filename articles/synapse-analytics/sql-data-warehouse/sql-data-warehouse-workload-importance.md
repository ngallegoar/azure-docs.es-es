---
title: Importancia de la carga de trabajo
description: Instrucciones para configurar la importancia de las consultas de SQL Analytics en Azure Synapse Analytics.
services: synapse-analytics
author: ronortloff
manager: craigg
ms.service: synapse-analytics
ms.topic: conceptual
ms.subservice: ''
ms.date: 02/04/2020
ms.author: rortloff
ms.reviewer: jrasnick
ms.custom: azure-synapse
ms.openlocfilehash: 3dde2ad4af17313bcfce28964f8be1e831317a5a
ms.sourcegitcommit: 8a9c54c82ab8f922be54fb2fcfd880815f25de77
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "80349961"
---
# <a name="azure-synapse-analytics-workload-importance"></a>Importancia de la carga de trabajo de Azure Synapse Analytics

En este artículo se explica cómo la importancia de la carga de trabajo puede influir en el orden de ejecución de las solicitudes de SQL Analytics en Azure Synapse.

## <a name="importance"></a>importancia

> [!Video https://www.youtube.com/embed/_2rLMljOjw8]

Las necesidades empresariales pueden requerir que las cargas de trabajo de almacenamiento de datos sean más importantes que otras.  Piense en un escenario donde los datos de ventas críticos se cargan antes del cierre del período fiscal.  Las cargas de datos de otros orígenes, como los datos meteorológicos, no tienen SLA estrictos. Establecer una importancia alta para una solicitud de carga de datos de ventas y una importancia baja para una solicitud de carga de datos meteorológicos garantiza que la carga de los datos de ventas obtienen el primer a acceso a los recursos y se completan más rápido.

## <a name="importance-levels"></a>Niveles de importancia

Hay cinco niveles de importancia: low (baja), below_normal (por debajo de la normal), normal, above_normal (por encima de la normal) y high (alta).  A las solicitudes donde no se establece ninguna importancia se les asigna el nivel predeterminado normal. Las solicitudes que tienen el mismo nivel de importancia tienen el mismo comportamiento de programación que existe actualmente.

## <a name="importance-scenarios"></a>Escenarios de importancia

Más allá del escenario de importancia básico ya descrito con datos de ventas y datos meteorológicos, existen otros escenarios donde la importancia de la carga de trabajo ayuda a cumplir con las necesidades de consulta y procesamiento de datos.

### <a name="locking"></a>Bloqueo

El acceso a bloqueos de la actividad de lectura y escritura es un área de contención natural. Las actividades como [modificación de particiones](/azure/sql-data-warehouse/sql-data-warehouse-tables-partition) o [RENAME OBJECT](/sql/t-sql/statements/rename-transact-sql?view=azure-sqldw-latest) requieren bloqueos elevados.  Sin importancia de la carga de trabajo, SQL Analytics en Azure Synapse optimiza el rendimiento. La optimización para el rendimiento significa que, cuando las solicitudes en ejecución y las que están en cola tienen las mismas necesidades de bloqueo y hay recursos disponibles, las solicitudes en cola pueden omitir las solicitudes con necesidades de bloqueo mayores que llegaron anteriormente a la cola de solicitud. Una vez que la importancia de la carga de trabajo se aplica a las solicitudes con necesidades de bloqueo más altas, se ejecutará la solicitud con la importancia de la solicitud antes de una solicitud con una menor importancia.

Considere el ejemplo siguiente:

- Q1 se ejecuta activamente y selecciona datos desde SalesFact.
- Q2 está en la cola y espera que Q1 se complete.  Se envió a las 9:00 a.m. e intenta modificar las particiones de los datos nuevos en SalesFact.
- Q3 se envía a las 9:01 a.m. y quiere seleccionar datos de SalesFact.

Si Q2 y Q3 tienen la misma importancia y Q1 todavía está en ejecución, se empezará a ejecutar Q3. Q2 seguirá esperando un bloqueo exclusivo en SalesFact.  Si Q2 tiene una mayor importancia que Q3, Q3 esperará hasta que Q2 se finalice antes de empezar a ejecutarse.

### <a name="non-uniform-requests"></a>Solicitudes no uniformes

Otro escenario donde la importancia puede ayudar a cumplir las demandas de consultas es cuando se envían solicitudes con clases de recursos distintas.  Como se mencionó anteriormente, con la misma importancia, SQL Analytics se optimiza para lograr un mejor rendimiento. Cuando se ponen en cola solicitudes de distinto tamaño (como smallrc o mediumrc), SQL Analytics elige la primera solicitud que llegue y que se ajuste a los recursos disponibles. Si se aplica la importancia de la carga de trabajo, la solicitud con la importancia más alta se programará a continuación.
  
Considere el ejemplo siguiente en DW500c:

- Q1, Q2, Q3 y Q4 son consultas smallrc en ejecución.
- Q5 se envía con la clase de recurso mediumrc a las 9:00 a.m.
- Q6 se envía con la clase de recurso smallrc a las 9:01 a.m.

Como Q5 es mediumrc, requiere dos espacios de simultaneidad. Q5 debe esperar que se completen dos de las consultas en ejecución.  Sin embargo, cuando se completa una de las consultas en ejecución (de Q1 a Q4), Q6 se programa de inmediato porque existen los recursos para ejecutar la consulta.  Si Q5 tiene una importancia más alta que Q6, Q6 espera hasta que se ejecute Q5 para empezar a ejecutarse.

## <a name="next-steps"></a>Pasos siguientes

- Para más información sobre cómo crear un clasificador, consulte [CREATE WORKLOAD CLASSIFIER (Transact-SQL)](/sql/t-sql/statements/create-workload-classifier-transact-sql).  
- Para más información sobre la clasificación de la carga de trabajo, consulte [Clasificación de la carga de trabajo](sql-data-warehouse-workload-classification.md).  
- Consulte el inicio rápido sobre cómo [crear un clasificador de carga de trabajo](quickstart-create-a-workload-classifier-tsql.md) para aprender a crear uno. 
- Consulte los artículos de procedimientos para [configurar la importancia de la carga de trabajo](sql-data-warehouse-how-to-configure-workload-importance.md) y sobre cómo [administrar y supervisar la administración de la carga de trabajo](sql-data-warehouse-how-to-manage-and-monitor-workload-importance.md).
- Consulte [sys.dm_pdw_exec_requests](/sql/relational-databases/system-dynamic-management-views/sys-dm-pdw-exec-requests-transact-sql?view=azure-sqldw-latest) para ver las consultas y la importancia asignada.
