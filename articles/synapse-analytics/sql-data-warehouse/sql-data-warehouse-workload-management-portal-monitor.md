---
title: Supervisión del portal de administración de cargas de trabajo
description: Guía para la supervisión del portal de administración de cargas de trabajo en Azure Synapse Analytics.
services: synapse-analytics
author: ronortloff
manager: craigg
ms.service: synapse-analytics
ms.topic: conceptual
ms.subservice: sql-dw
ms.date: 02/04/2020
ms.author: rortloff
ms.reviewer: jrasnick
ms.custom: azure-synapse
ms.openlocfilehash: 13b0dc3af524b16430408f8a920c7477c412414d
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "91362736"
---
# <a name="azure-synapse-analytics--workload-management-portal-monitoring"></a>Azure Synapse Analytics: supervisión del portal de administración de cargas de trabajo

En este artículo se explica cómo supervisar la actividad de consultas y la utilización de recursos para [grupos de cargas de trabajo](sql-data-warehouse-workload-isolation.md#workload-groups).
Para más información sobre cómo configurar el Explorador de métricas de Azure consulte el artículo [Introducción al Explorador de métricas de Azure](../../azure-monitor/platform/metrics-getting-started.md?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json).  Consulte la sección [Utilización de recursos](sql-data-warehouse-concept-resource-utilization-query-activity.md#resource-utilization) en la documentación de Supervisión de Azure Synapse Analytics para obtener más información sobre cómo supervisar el consumo de recursos del sistema.
Hay dos categorías diferentes de métricas de grupo de cargas de trabajo que se proporcionan para supervisar la administración de cargas de trabajo: asignación de recursos y actividad de consultas.  Estas métricas se pueden dividir y filtrar por grupo de cargas de trabajo.  Las métricas se pueden dividir y filtrar en función de si están definidas por el sistema (grupos de cargas de trabajo de clase de recurso) o definidas por el usuario (creadas por el usuario con la sintaxis [CREATE WORKLOAD GROUP](/sql/t-sql/statements/create-workload-group-transact-sql?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest)).

## <a name="workload-management-metric-definitions"></a>Definiciones de métricas de administración de cargas de trabajo

|Nombre de la métrica                    |Descripción  |Tipo de agregación |
|-------------------------------|-------------|-----------------|
|Porcentaje máximo de recursos efectivo | *Porcentaje máximo de recursos efectivo* es un límite máximo en el porcentaje de recursos a los que puede acceder el grupo de cargas de trabajo, teniendo en cuenta el *porcentaje de recursos mínimo efectivo* asignado a otros grupos de cargas de trabajo. La métrica *Porcentaje máximo de recursos efectivo* se configura mediante el parámetro `CAP_PERCENTAGE_RESOURCE` en la sintaxis de [CREATE WORKLOAD GROUP](/sql/t-sql/statements/create-workload-group-transact-sql?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest).  El valor efectivo se describe aquí.<br><br>Por ejemplo, si se crea un grupo de cargas de trabajo `DataLoads` con `CAP_PERCENTAGE_RESOURCE` = 100 y se crea otro grupo de cargas de trabajo con un porcentaje de recursos mínimo efectivo del 25 %, el *porcentaje máximo de recursos efectivo* para el grupo de cargas de trabajo `DataLoads` es del 75 %.<br><br>La métrica *Porcentaje máximo de recursos efectivo* determina el límite superior de simultaneidad (y, por lo tanto, el rendimiento potencial) que puede lograr un grupo de cargas de trabajo.  Si se necesita rendimiento adicional más allá de lo que la métrica *Porcentaje máximo de recursos efectivos* muestra, aumente `CAP_PERCENTAGE_RESOURCE`, disminuya `MIN_PERCENTAGE_RESOURCE` de otros grupos de cargas de trabajo o escale verticalmente la instancia para agregar más recursos.  La disminución de `REQUEST_MIN_RESOURCE_GRANT_PERCENT` puede aumentar la simultaneidad, pero puede que no aumente el rendimiento general.| Min, Avg o Max |
|Porcentaje mínimo de recursos efectivo |*Porcentaje mínimo de recursos efectivo* es el porcentaje mínimo de recursos reservados y aislados para el grupo de cargas de trabajo teniendo en cuenta el nivel de servicio mínimo.  La métrica Porcentaje mínimo de recursos efectivo se configura mediante el parámetro `MIN_PERCENTAGE_RESOURCE` en la sintaxis de [CREATE WORKLOAD GROUP](/sql/t-sql/statements/create-workload-group-transact-sql?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest).  El valor efectivo se describe [aquí](/sql/t-sql/statements/create-workload-group-transact-sql?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest#effective-values).<br><br>Use el tipo de agregación Suma cuando esta métrica no esté filtrada ni dividida para supervisar el aislamiento de cargas de trabajo total configurado en el sistema.<br><br>El *porcentaje mínimo de recursos efectivo* determina el límite inferior de simultaneidad (y, por lo tanto, el rendimiento garantizado) que puede lograr un grupo de cargas de trabajo.  Si se necesitan recursos adicionales garantizados más allá de lo que la métrica *Porcentaje mínimo de recursos efectivo* muestra actualmente, aumente el parámetro `MIN_PERCENTAGE_RESOURCE` configurado para el grupo de cargas de trabajo.  La disminución de `REQUEST_MIN_RESOURCE_GRANT_PERCENT` puede aumentar la simultaneidad, pero puede que no aumente el rendimiento general. |Min, Avg o Max|
|Consultas activas de grupo de cargas de trabajo  |Esta métrica informa de las consultas activas dentro del grupo de cargas de trabajo.  El uso de esta métrica sin filtrar y sin dividir muestra todas las consultas activas que se ejecutan en el sistema.|Sum         |
|Asignación de grupos de cargas de trabajo por porcentaje máximo de recursos |Esta métrica muestra el porcentaje de asignación de recursos en relación con la métrica *Porcentaje máximo de recursos efectivo* por grupo de cargas de trabajo.  Esta métrica proporciona la utilización efectiva del grupo de cargas de trabajo.<br><br>Considere la posibilidad de usar un grupo de cargas de trabajo `DataLoads` con una métrica *Porcentaje máximo de recursos efectivo* del 75 % y `REQUEST_MIN_RESOURCE_GRANT_PERCENT` configurado en el 25 %.  El valor de *Asignación de grupos de cargas de trabajo por porcentaje máximo de recursos* filtrado a `DataLoads` sería del 33 % (25 %/75 %) si se estaba ejecutando una sola consulta en este grupo de cargas de trabajo.<br><br>Use esta métrica para identificar la utilización de un grupo de cargas de trabajo.  Un valor cercano al 100 % indica que se están utilizando todos los recursos disponibles para el grupo de cargas de trabajo.  Además, la métrica *Consultas en cola del grupo de cargas de trabajo* para el mismo grupo de cargas de trabajo que muestra un valor mayor que cero indicaría que el grupo de cargas de trabajo utilizaría recursos adicionales si está asignado.  Por el contrario, si esta métrica es constantemente baja y el valor de *Consultas en cola del grupo de cargas de trabajo* es bajo, no se está utilizando el grupo de cargas de trabajo.  Esta situación es especialmente problemática si el valor de *Porcentaje máximo de recursos efectivo* es mayor que cero, ya que esto indicaría un [aislamiento de cargas de trabajo infrautilizado](#underutilized-workload-isolation).|Min, Avg o Max |
|Asignación de grupos de cargas de trabajo por porcentaje del sistema | Esta métrica muestra el porcentaje de asignación de recursos en relación con todo el sistema.<br><br>Considere la posibilidad de un grupo de cargas de trabajo `DataLoads` con `REQUEST_MIN_RESOURCE_GRANT_PERCENT` configurado en el 25 %.  El valor de *Asignación de grupos de cargas de trabajo por porcentaje del sistema* filtrado a `DataLoads` sería del 25 % (25 %/100 %) si se estaba ejecutando una sola consulta en este grupo de cargas de trabajo.|Min, Avg o Max |
|Tiempos de espera de consultas de grupo de cargas de trabajo |Consultas para el grupo de cargas de trabajo que han agotado el tiempo de espera.  Los tiempos de espera de consulta que indica esta métrica son solo una vez que se ha iniciado la ejecución de la consulta (no incluye el tiempo de espera debido a las esperas de bloqueo o de recursos).<br><br>El tiempo de espera de consulta se configura mediante el parámetro `QUERY_EXECUTION_TIMEOUT_SEC` en la sintaxis [CREATE WORKLOAD GROUP](/sql/t-sql/statements/create-workload-group-transact-sql?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest).  El aumento del valor podría reducir el número de tiempos de espera de consulta.<br><br>Considere la posibilidad de aumentar el parámetro `REQUEST_MIN_RESOURCE_GRANT_PERCENT` del grupo de cargas de trabajo para reducir la cantidad de tiempos de espera y asignar más recursos por consulta.  Tenga en cuenta que al aumentar `REQUEST_MIN_RESOURCE_GRANT_PERCENT` se reduce la cantidad de simultaneidad para el grupo de cargas de trabajo. |Sum |
|Consultas en cola del grupo de cargas de trabajo | Consultas para el grupo de cargas de trabajo que están en cola actualmente en espera para iniciar la ejecución.  Las consultas se pueden poner en cola porque están esperando recursos o bloqueos.<br><br>Las consultas podrían estar esperando por diversos motivos.  Si el sistema está sobrecargado y la demanda de simultaneidad es mayor de la que está disponible, las consultas se pondrán en cola.<br><br>Considere la posibilidad de agregar más recursos al grupo de cargas de trabajo aumentando el parámetro `CAP_PERCENTAGE_RESOURCE` en la instrucción [CREATE WORKLOAD GROUP](/sql/t-sql/statements/create-workload-group-transact-sql?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest).  Si `CAP_PERCENTAGE_RESOURCE` es mayor que la métrica *Porcentaje máximo de recursos efectivo*, el aislamiento de cargas de trabajo configurado para otro grupo de cargas de trabajo afectará a los recursos asignados a este grupo de cargas de trabajo.  Considere la posibilidad de reducir `MIN_PERCENTAGE_RESOURCE` de otros grupos de cargas de trabajo o escale verticalmente la instancia para agregar más recursos. |Sum |

## <a name="monitoring-scenarios-and-actions"></a>Supervisión de escenarios y acciones

A continuación se muestran una serie de configuraciones de gráficos para resaltar el uso de métricas de administración de cargas de trabajo para la solución de problemas junto con acciones asociadas para hacer frente al problema.

### <a name="underutilized-workload-isolation"></a>Aislamiento de cargas de trabajo infrautilizado

Tenga en cuenta el siguiente grupo de cargas de trabajo y la configuración del clasificador donde se crea un grupo de cargas de trabajo denominado `wgPriority` y se le asigna *TheCEO* `membername` mediante el clasificador de cargas de trabajo `wcCEOPriority`.  El grupo de cargas de trabajo `wgPriority` tiene configurado un 25 % de aislamiento de carga de trabajo (`MIN_PERCENTAGE_RESOURCE` = 25).  Cada consulta enviada por *TheCEO* recibe el 5 % de los recursos del sistema (`REQUEST_MIN_RESOURCE_GRANT_PERCENT` = 5).

```sql
CREATE WORKLOAD GROUP wgPriority
WITH ( MIN_PERCENTAGE_RESOURCE = 25
      ,CAP_PERCENTAGE_RESOURCE = 50
      ,REQUEST_MIN_RESOURCE_GRANT_PERCENT = 5);

CREATE WORKLOAD CLASSIFIER wcCEOPriority
WITH ( WORKLOAD_GROUP = 'wgPriority'
      ,MEMBERNAME = 'TheCEO');
```

El gráfico siguiente está configurado de la siguiente manera:<br>
Métrica 1: *Porcentaje mínimo de recursos efectivo* (agregación de tipo Avg, `blue line`)<br>
Métrica 2: *Asignación de grupos de cargas de trabajo por porcentaje del sistema* (agregación de tipo Avg, `purple line`)<br>
Filtro: [Grupo de cargas de trabajo] = `wgPriority`<br>
![Captura de pantalla que muestra un gráfico con las dos métricas y el filtro.](./media/sql-data-warehouse-workload-management-portal-monitor/underutilized-wg.png)
El gráfico muestra que con un 25 % de aislamiento de cargas de trabajo, solo se usa un 10 % de media.  En este caso, el valor del parámetro `MIN_PERCENTAGE_RESOURCE` podría disminuir entre 10 o 15 y permitir que otras cargas de trabajo del sistema consuman los recursos.

### <a name="workload-group-bottleneck"></a>Cuello de botella de grupo de cargas de trabajo

Tenga en cuenta el siguiente grupo de cargas de trabajo y la configuración del clasificador donde se crea un grupo de cargas de trabajo denominado `wgDataAnalyst` y se le asigna *DataAnalyst* `membername` mediante el clasificador de cargas de trabajo `wcDataAnalyst`.  El grupo de cargas de trabajo `wgDataAnalyst` tiene configurado un 6 % de aislamiento de cargas de trabajo (`MIN_PERCENTAGE_RESOURCE` = 6) y un límite de recursos de 9 % (`CAP_PERCENTAGE_RESOURCE` = 9).  Cada consulta enviada por *DataAnalyst* recibe el 3 % de los recursos del sistema (`REQUEST_MIN_RESOURCE_GRANT_PERCENT` = 3).

```sql
CREATE WORKLOAD GROUP wgDataAnalyst  
WITH ( MIN_PERCENTAGE_RESOURCE = 6
      ,CAP_PERCENTAGE_RESOURCE = 9
      ,REQUEST_MIN_RESOURCE_GRANT_PERCENT = 3);

CREATE WORKLOAD CLASSIFIER wcDataAnalyst
WITH ( WORKLOAD_GROUP = 'wgDataAnalyst'
      ,MEMBERNAME = 'DataAnalyst');
```

El gráfico siguiente está configurado de la siguiente manera:<br>
Métrica 1: *Porcentaje máximo de recursos efectivo* (agregación de tipo Avg, `blue line`)<br>
Métrica 2: *Asignación de grupos de cargas de trabajo por porcentaje máximo de recursos* (agregación de tipo Avg, `purple line`)<br>
Métrica 3: *Consultas en cola del grupo de cargas de trabajo* (agregación de tipo Sum, `turquoise line`)<br>
Filtro: [Grupo de cargas de trabajo] = `wgDataAnalyst`<br>
![Captura de pantalla que muestra un gráfico con las tres métricas y el filtro.](./media/sql-data-warehouse-workload-management-portal-monitor/bottle-necked-wg.png)
El gráfico muestra que con un límite del 9 % en los recursos, el grupo de cargas de trabajo se utiliza en más de un 90 % (de la métrica *Asignación de grupos de cargas de trabajo por porcentaje máximo de recursos*).  Hay un almacenamiento en cola de las consultas estable, tal como se desprende de la métrica *Consultas en cola del grupo de cargas de trabajo*.  En este caso, el aumento de `CAP_PERCENTAGE_RESOURCE` a un valor superior al 9 % permitirá que se ejecuten más consultas simultáneamente.  Al aumentar `CAP_PERCENTAGE_RESOURCE` se supone que hay suficientes recursos disponibles y que no están aislados por otros grupos de cargas de trabajo.  Verifique el límite máximo aumentado comprobando la métrica *Porcentaje máximo de recursos efectivo*.  Si desea más rendimiento, considere la posibilidad de aumentar `REQUEST_MIN_RESOURCE_GRANT_PERCENT` a un valor superior a 3.  El aumento de `REQUEST_MIN_RESOURCE_GRANT_PERCENT` podría permitir que las consultas se ejecuten más rápido.

## <a name="next-steps"></a>Pasos siguientes

- [Inicio rápido: Configuración del aislamiento de cargas de trabajo mediante T-SQL](quickstart-configure-workload-isolation-tsql.md)<br>
- [CREATE WORKLOAD GROUP (Transact-SQL)](/sql/t-sql/statements/create-workload-group-transact-sql?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest)<br>
- [CREATE WORKLOAD CLASSIFIER (Transact-SQL)](/sql/t-sql/statements/create-workload-classifier-transact-sql?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest)<br>
- [Supervisión de la utilización de recursos](sql-data-warehouse-concept-resource-utilization-query-activity.md)
