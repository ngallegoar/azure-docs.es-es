---
title: Configuración de los monitores de estado de Azure Monitor para contenedores | Microsoft Docs
description: En este artículo se proporciona contenido que describe la configuración detallada de los monitores de estado en Azure Monitor para contenedores.
ms.topic: conceptual
ms.date: 12/01/2019
ms.openlocfilehash: 99ea6e96f5a8a486784cb3d633a6e031b60eaad7
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "80055700"
---
# <a name="azure-monitor-for-containers-health-monitor-configuration-guide"></a>Guía de configuración de los monitores de estado de Azure Monitor para contenedores

Los monitores son el elemento principal para medir el estado y detectar errores en Azure Monitor para contenedores. Este artículo le ayuda a comprender los conceptos de cómo se mide el estado y los elementos que componen el modelo de estado para supervisar y notificar el estado del clúster de Kubernetes con la característica [Estado (versión preliminar)](container-insights-health.md).

>[!NOTE]
>La característica Estado está en versión preliminar pública en este momento.
>

## <a name="monitors"></a>Monitores

Un monitor mide el estado de algún aspecto de un objeto administrado. Los monitores tienen dos o tres estados de mantenimiento. Un monitor estará en uno y solo uno de sus estados posibles en un momento dado. Cuando el agente en contenedor carga un monitor, se inicializa en un estado correcto. El estado cambia solo si se detectan las condiciones especificadas para otro estado.

El estado general de un objeto determinado se determina a partir del estado de cada uno de sus monitores. Esta jerarquía se muestra en el panel de la Jerarquía de estado en Azure Monitor para contenedores. Como parte de la configuración de los monitores agregados está la directiva de cómo se acumula el estado.

## <a name="types-of-monitors"></a>Tipos de monitores

|Supervisión | Descripción | 
|--------|-------------|
| Monitor de unidad |Un monitor de unidad mide algún aspecto de un recurso o una aplicación. Esto podría estar comprobando un contador de rendimiento para determinar el rendimiento del recurso o su disponibilidad. |
|Monitor agregado | Los monitores agregados agrupan varios monitores para proporcionar un estado de mantenimiento agregado de un solo mantenimiento. Normalmente, los monitores de unidad se configuran en un determinado monitor agregado. Por ejemplo, un monitor agregado de nodo acumula el estado del uso de CPU del nodo, el uso de memoria y el estado del nodo.
 |

### <a name="aggregate-monitor-health-rollup-policy"></a>Directiva de acumulación de estado de monitor agregado

Cada monitor agregado define una directiva de acumulación de estado, que es la lógica que se usa para determinar el estado del monitor agregado en función del estado de los monitores que se encuentra en él. Las posibles directivas de acumulación de estado para un monitor agregado son las siguientes:

#### <a name="worst-state-policy"></a>Directiva de peor estado

El estado del monitor agregado coincide con el estado del monitor secundario con el peor estado de mantenimiento. Esta es la directiva más común que usan los monitores agregados.

![Ejemplo de peor estado de acumulación de monitor agregado](./media/container-insights-health-monitoring-cfg/aggregate-monitor-rollup-worstof.png)

### <a name="percentage-policy"></a>Directiva de porcentaje

El objeto de origen coincide con el peor estado de un solo miembro de un porcentaje especificado de objetos de destino en el mejor estado. Esta directiva se usa cuando un determinado porcentaje de objetos de destino debe ser correcto para que el objeto de destino se considere correcto. La directiva de porcentaje ordena los monitores en orden descendente según la gravedad de estado, y el estado del monitor agregado se calcula como el peor estado de N% (N se determina por el parámetro de configuración *StateThresholdPercentage*).

Por ejemplo, supongamos que hay cinco instancias de contenedor de una imagen de contenedor y sus estados individuales son **Crítico**, **Advertencia**, **Correcto**, **Correcto**, **Correcto**.  El estado del monitor de uso de CPU de contenedor será **Crítico**, ya que el peor estado del 90 % de los contenedores es **Crítico** cuando se ordenan en orden descendente de gravedad.

## <a name="understand-the-monitoring-configuration"></a>Descripción de la configuración de la supervisión

Azure Monitor para contenedores incluye varios escenarios clave de supervisión que se configuran como se indica a continuación.

### <a name="unit-monitors"></a>Monitores de unidad

|**Nombre de monitor** | Tipo de monitor | **Descripción** | **Parámetro** | **Valor** |
|-----------------|--------------|-----------------|---------------|-----------|
|Utilización de memoria del nodo |Monitor de unidad |Este monitor evalúa el uso de memoria de un nodo cada minuto, utilizando los datos notificados de cadvisor. |ConsecutiveSamplesForStateTransition<br> FailIfGreaterThanPercentage<br> WarnIfGreaterThanPercentage | 3<br> 90<br> 80  ||
|Uso de la CPU del nodo |Monitor de unidad |Este monitor evalúa el uso de la CPU de un nodo cada minuto, utilizando los datos notificados de cadvisor. | ConsecutiveSamplesForStateTransition<br> FailIfGreaterThanPercentage<br> WarnIfGreaterThanPercentage | 3<br> 90<br> 80  ||
|Estado del nodo |Monitor de unidad |Este monitor comprueba las condiciones del nodo notificadas por Kubernetes.<br> Se comprueban las condiciones siguientes del nodo: La presión del disco, la presión de la memoria, la presión del PID, fuera del disco, la red no está disponible, estado listo para el nodo.<br> Fuera de las condiciones anteriores, si *fuera de disco* o *red no disponible* es **true**, el monitor cambia a estado **Crítico**.<br> Si las demás condiciones son iguales a **true**, excepto un estado de **Listo**, el monitor cambia a un estado de **Advertencia**. | NodeConditionTypeForFailedState | outofdisk,networkunavailable ||
|Uso de la memoria del contenedor |Monitor de unidad |Este monitor indica el estado de mantenimiento combinado del uso de memoria (RSS) de las instancias del contenedor.<br> Realiza una comparación simple que compara cada muestra con un umbral único y lo especifica el parámetro de configuración **ConsecutiveSamplesForStateTransition**.<br> Su estado se calcula como el peor estado del 90% de las instancias de contenedor (StateThresholdPercentage), ordenados en orden descendente de gravedad del estado de mantenimiento del contenedor (es decir, Crítico, de Advertencia, Correcto).<br> Si no se recibe ningún registro de una instancia de contenedor, el estado de mantenimiento de la instancia de contenedor se indica como **Desconocido** y tiene más prioridad en el orden de clasificación sobre el estado **Crítico**.<br> Cada estado de la instancia de contenedor individual se calcula con los umbrales especificados en la configuración. Si el uso está por encima del umbral crítico (90%), la instancia se encuentra en un estado **Crítico**, si es inferior al umbral crítico (90%), pero mayor que el umbral de advertencia (80%), la instancia está en un estado de **Advertencia**. De lo contrario, está en estado **Correcto**. |ConsecutiveSamplesForStateTransition<br> FailIfLessThanPercentage<br> StateThresholdPercentage<br> WarnIfGreaterThanPercentage| 3<br> 90<br> 90<br> 80 ||
|Uso de la CPU del contenedor |Monitor de unidad |Este monitor indica el estado de mantenimiento combinado del uso de la CPU de las instancias del contenedor.<br> Realiza una comparación simple que compara cada muestra con un umbral único y lo especifica el parámetro de configuración **ConsecutiveSamplesForStateTransition**.<br> Su estado se calcula como el peor estado del 90% de las instancias de contenedor (StateThresholdPercentage), ordenados en orden descendente de gravedad del estado de mantenimiento del contenedor (es decir, Crítico, de Advertencia, Correcto).<br> Si no se recibe ningún registro de una instancia de contenedor, el estado de mantenimiento de la instancia de contenedor se indica como **Desconocido** y tiene más prioridad en el orden de clasificación sobre el estado **Crítico**.<br> Cada estado de la instancia de contenedor individual se calcula con los umbrales especificados en la configuración. Si el uso está por encima del umbral crítico (90%), la instancia se encuentra en un estado **Crítico**, si es inferior al umbral crítico (90%), pero mayor que el umbral de advertencia (80%), la instancia está en un estado de **Advertencia**. De lo contrario, está en estado **Correcto**. |ConsecutiveSamplesForStateTransition<br> FailIfLessThanPercentage<br> StateThresholdPercentage<br> WarnIfGreaterThanPercentage| 3<br> 90<br> 90<br> 80 ||
|Pods de carga de trabajo del sistema listos |Monitor de unidad |Este monitor informa del estado basado en el porcentaje de pods en estado "Listo" en determinada carga de trabajo. Se establece su estado en **Crítico** si menos del 100% de los pods están en un estado **Correcto** |ConsecutiveSamplesForStateTransition<br> FailIfLessThanPercentage |2<br> 100 ||
|Estado de API de Kube |Monitor de unidad |Este monitor informa del estado del servicio de API de Kube. En caso de que el punto de conexión de API de Kube no esté disponible, el monitor estará en estado crítico. Para este monitor en particular, el estado se determina mediante la realización de una consulta al punto de conexión 'nodes' para el servidor API de Kube. Cualquier cosa que no sea un código de respuesta aceptable cambia el monitor a un estado **Crítico**. | Sin propiedades de configuración |||

### <a name="aggregate-monitors"></a>Monitores agregados

|**Nombre de monitor** | **Descripción** | **Algoritmo** |
|-----------------|-----------------|---------------|
|Nodo |Este monitor es un agregado de todos los monitores de nodo. Coincide con el estado del monitor secundario coincide con el peor estado de mantenimiento:<br> Uso de la CPU del nodo<br> Uso de la memoria del nodo<br> Estado del nodo | El peor de|
|Grupo de nodos |Este monitor informa del estado de mantenimiento combinado de todos los nodos del grupo de nodos *agentpool*. Se trata de un monitor de tres estados, cuyo estado se basa en el peor estado del 80% de los nodos del grupo de nodos, ordenados en orden de gravedad descendente de los estados del nodo (es decir, Crítico, de Advertencia, Correcto).|Porcentaje |
|Nodos (primario del grupo de nodos) |Se trata de un monitor agregado de todos los grupos de nodos. Su estado se basa en el peor estado de sus monitores secundarios (es decir, los grupos de nodos presentes en el clúster). |El peor de |
|Clúster (primario de nodos/<br> infraestructura de Kubernetes) |Este es el monitor principal que coincide con el estado del monitor secundario con el peor estado de mantenimiento, que es los nodos y la infraestructura de kubernetes. |El peor de |
|Infraestructura de Kubernetes |Este monitor informa del estado de mantenimiento combinado de los componentes de la infraestructura administrada del clúster. Su estado se calcula como 'el peor de' los estados del monitor secundario, es decir, las cargas de trabajo del sistema Kube y el estado del servidor de API. |El peor de|
|Carga de trabajo del sistema |Este monitor notifica el estado de mantenimiento de una carga de trabajo del sistema Kube. Este monitor empareja el estado del monitor secundario con el peor estado de mantenimiento (es decir, el monitor de los **Pods en estado listo** y los contenedores de la carga de trabajo). |El peor de |
|Contenedor |Este monitor informa del estado de mantenimiento general de un contenedor en una determinada carga de trabajo. Este monitor coincide con el estado del monitor secundario con el peor estado de mantenimiento, es decir, los monitores de **Uso de la CPU** y **Uso de la memoria**. |El peor de |

## <a name="next-steps"></a>Pasos siguientes

Vea [Supervisión del estado del clúster](container-insights-health.md) para obtener información sobre cómo ver el estado de mantenimiento del clúster de Kubernetes.
