---
title: Guía de ajuste de tamaño del clúster de Interactive Query en Azure HDInsight
description: Guía de ajuste de tamaño de Interactive Query en Azure HDInsight
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.date: 05/08/2020
ms.openlocfilehash: a81eff1dcf48996c319933aa4dd46170043b943b
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "83663653"
---
# <a name="interactive-query-cluster-sizing-guide-in-azure-hdinsight"></a>Guía de ajuste de tamaño del clúster de Interactive Query en Azure HDInsight

En este documento se describe el ajuste de tamaño del clúster de HDInsight Interactive Query (LLAP) de una carga de trabajo típica para lograr un rendimiento razonable. Las recomendaciones proporcionadas en este documento son genéricas y las cargas de trabajo específicas pueden necesitar un ajuste específico.

## <a name="default-vm-types-for-interactive-query"></a>Tipos de VM predeterminados para Interactive Query

| Tipo de nodo | Instancia | Size |
|---|---|---|
| Head | D13 v2 | 8 vCPU, 56 GB de RAM, SSD de 400 GB |
| Trabajo | D14 v2 | 16 vCPU, 112 GB de RAM, SSD de 800 GB |
| ZooKeeper | A4 v2 | 4 vCPU, 8 GB de RAM, SSD de 40 GB |

## <a name="recommended-configurations"></a>Configuraciones recomendadas

Los valores de las configuraciones recomendadas se basan en el nodo de trabajo de tipo D14 v2.

| Clave | Value | Descripción |
|---|---|---|
| yarn.nodemanager.resource.memory-mb | 102400 (MB) | Memoria total proporcionada, en MB, para todos los contenedores de YARN de un nodo. |
| yarn.scheduler.maximum-allocation-mb | 102400 (MB) | La asignación máxima para cada solicitud de contenedor en RM, en MB. Las solicitudes de memoria que superen este valor no surtirán efecto. |
| yarn.scheduler.maximum-allocation-vcores | 12 |Número máximo de núcleos de CPU para cada solicitud de contenedor en Resource Manager. Las solicitudes que superen este valor no surtirán efecto. |
| yarn.scheduler.capacity.root.llap.capacity | 90% | Asignación de la capacidad de YARN para la cola de LLAP.  |
| hive.server2.tez.sessions.per.default.queue | number_of_worker_nodes |Número de sesiones de cada cola mencionada en hive.server2.tez.default.queues. Este número corresponde al número de coordinadores de consultas (AM de Tez). |
| tez.am.resource.memory.mb | 4096 (MB) | Cantidad de memoria en MB que va a usar AppMaster de Tez. |
| hive.tez.container.size | 4096 (MB) | Tamaño de contenedor de Tez especificado en MB. |
| hive.llap.daemon.num.executors | 12 | Número de ejecutores por demonio de LLAP. |
| hive.llap.io.threadpool.size | 12 | Tamaño del grupo de subprocesos para los ejecutores. |
| hive.llap.daemon.yarn.container.mb | 86016 (MB) | Memoria total en MB usada por demonios de LLAP individuales (memoria por demonio).|
| hive.llap.io.memory.size | 409600 (MB) | Tamaño de caché en MB por demonio de LLAP si la caché del disco SSD está habilitada. |
| hive.auto.convert.join.noconditionaltask.size | 2048 (MB) | Tamaño de memoria en MB para realizar la combinación de asignación. |

## <a name="llap-daemon-size-estimations"></a>Estimaciones de tamaño de demonio de LLAP  

### <a name="yarnnodemanagerresourcememory-mb"></a>yarn.nodemanager.resource.memory-mb

Este valor indica una suma máxima de memoria en MB utilizada por los contenedores de YARN en cada nodo. Especifica la cantidad de memoria que YARN puede usar en este nodo, por lo que este valor debe ser menor que la memoria total en ese nodo.

Establezca este valor = [total de memoria física en el nodo] – [memoria para SO + otros servicios].

Se recomienda establecer este valor en aproximadamente un 90 % de la RAM disponible. Para D14 v2, se recomienda un valor de **102400 MB**.

### <a name="yarnschedulermaximum-allocation-mb"></a>yarn.scheduler.maximum-allocation-mb

Este valor indica la asignación máxima para cada solicitud de contenedor en Resource Manager, en MB. Las solicitudes de memoria que superen el valor especificado no surtirán efecto. Resource Manager solo puede proporcionar memoria a los contenedores en incrementos de `yarn.scheduler.minimum-allocation-mb` y no puede superar el tamaño especificado por `yarn.scheduler.maximum-allocation-mb`. Este valor no debe ser mayor que la memoria total especificada del nodo, que se especifica mediante `yarn.nodemanager.resource.memory-mb`.

Para los nodos de trabajo D14 v2, se recomienda un valor de **102400 MB**.

### <a name="yarnschedulermaximum-allocation-vcores"></a>yarn.scheduler.maximum-allocation-vcores

Esta configuración indica el número máximo de núcleos de CPU virtuales para cada solicitud de contenedor en Resource Manager. La solicitud de un valor mayor al de esta configuración no surtirá efecto. Esta configuración es una propiedad global del programador de YARN. Para el contenedor del demonio de LLAP, este valor se puede establecer en el 75 % del total de núcleos virtuales disponibles. El 25 % restante se debe reservar para NodeManager, DataNode y otros servicios que se ejecutan en los nodos de trabajo.  

En el caso de los nodos de trabajo D14 v2, existen 16 núcleos virtuales, de los cuales se puede proporcionar un 75 %. Por lo tanto, el valor recomendado para el contenedor de demonio de LLAP es **12**.

### <a name="hiveserver2tezsessionsperdefaultqueue"></a>hive.server2.tez.sessions.per.default.queue

Este valor de configuración determina el número de sesiones de Tez que deben iniciarse en paralelo para cada una de las colas especificadas por `hive.server2.tez.default.queues`. El valor corresponde al número de AM de Tez (coordinadores de consultas). Se recomienda que sea el mismo que el número de nodos de trabajo para tener un AM de Tez por nodo. El número de AM de Tez puede ser mayor que el número de nodos de demonio de LLAP. Su responsabilidad principal es coordinar la ejecución de la consulta y asignar los fragmentos del plan de consulta a los demonios de LLAP correspondientes para su ejecución. Se recomienda mantenerlo como múltiplo de varios nodos de demonio de LLAP para lograr un mayor rendimiento.  

El clúster de HDInsight predeterminado tiene cuatro demonios de LLAP que se ejecutan en cuatro nodos de trabajo, por lo que el valor recomendado es **4**.  

### <a name="tezamresourcememorymb-hivetezcontainersize"></a>tez.am.resource.memory.mb, hive.tez.container.size

`tez.am.resource.memory.mb` define el tamaño del maestro de la aplicación Tez.  
Se recomienda un valor de **4096 MB**.

`hive.tez.container.size` define la cantidad de memoria proporcionada para el contenedor de Tez. Este valor se debe establecer entre el tamaño mínimo (`yarn.scheduler.minimum-allocation-mb`) y el tamaño máximo (`yarn.scheduler.maximum-allocation-mb`) del contenedor de YARN.  
Se recomienda establecerlo en **4096 MB**.  

Una regla general es mantenerlo en un valor inferior a la cantidad de memoria por procesador si se tiene en cuenta un procesador por contenedor. Memoria `Rreserve` para el número de AM de Tez en un nodo antes de proporcionar la memoria para el demonio de LLAP. Por ejemplo, si usa dos AM de Tez (4 GB cada uno) por nodo, proporcione 82 GB de 90 GB para el demonio de LLAP y reserve 8 GB para dos AM de Tez.

### <a name="yarnschedulercapacityrootllapcapacity"></a>yarn.scheduler.capacity.root.llap.capacity

Este valor indica un porcentaje de la capacidad proporcionada para la cola de LLAP. El clúster de HDInsights Interactive Query proporciona el 90 % de la capacidad total para la cola de LLAP, y el 10 % restante se establece en la cola predeterminada para otras asignaciones de contenedor.  
Para los nodos de trabajo D14v2, se recomienda un valor de **90** para la cola de LLAP.

### <a name="hivellapdaemonyarncontainermb"></a>hive.llap.daemon.yarn.container.mb

El tamaño total de la memoria para el demonio de LLAP depende de los siguientes componentes:

* Configuración del tamaño del contenedor de YARN (`yarn.scheduler.maximum-allocation-mb`, `yarn.scheduler.maximum-allocation-mb`, `yarn.nodemanager.resource.memory-mb`)

* Memoria de montón usada por los ejecutores (Xmx)

    Su cantidad de memoria RAM disponible después de sacar el tamaño de la capacidad de aumento.  
    Para D14 v2, HDI 4.0: este valor es (86 GB - 6 GB) = 80 GB  
    Para D14 v2, HDI 3.6: este valor es (84 GB - 6 GB) = 78 GB

* Caché en memoria fuera del montón por demonio (hive.llap.io.memory.size)

* Capacidad de aumento

    Se trata de una parte de la memoria fuera del montón usada para la sobrecarga de VM Java (metaespacio, pila de subprocesos, estructuras de datos de GC, etc.). Esta parte se observa en torno al 6 % del tamaño del montón (Xmx). Para ir a lo seguro, se puede calcular como el 6 % del tamaño total de la memoria del demonio de LLAP. Dado que es posible cuando la memoria caché de SSD está habilitada, permitirá que el demonio de LLAP use todo el espacio en memoria disponible para su uso solo en el montón.  
    Para D14 v2, el valor recomendado es ceil (86 GB x 0,06) ~= **6 GB**.  

Memoria por demonio = [tamaño de caché en memoria] + [tamaño del montón] + [capacidad de aumento].

Se puede calcular de la siguiente manera:

Memoria de AM de Tez por nodo = [(número de AM de Tez/número de nodos de demonio de LLAP) * tamaño de AM de Tez].
Tamaño de contenedor de demonio de LLAP = [90 % de la memoria máxima del contenedor de YARN] – [memoria de AM de Tez por nodo].

Para el nodo de trabajo D14 v2, HDI 4.0: el valor recomendado es (90 - (1/1 * 4 GB)) = **86 GB**.
(Para HDI 3.6, el valor recomendado es **84 GB** porque debe reservar unos 2 GB para el AM del control deslizante).  

### <a name="hivellapiomemorysize"></a>hive.llap.io.memory.size

Esta configuración es la cantidad de memoria disponible como memoria caché para el demonio de LLAP. Los demonios de LLAP pueden usar el disco SSD como caché. Si se establece `hive.llap.io.allocator.mmap` = true, se habilitará el almacenamiento en caché de SSD. D14 v2 incluye unos 800 GB de SSD y el almacenamiento en caché de SSD está habilitado de forma predeterminada para el clúster de Interactive Query (LLAP). Está configurado para usar el 50 % del espacio del disco SSD para la caché fuera del montón.

Para D14 v2, se recomienda un valor de **409600 MB**.  

Para otras VM, sin el almacenamiento en caché de SSD habilitado, es beneficioso proporcionar una parte de la RAM disponible para el almacenamiento en caché de LLAP a fin de lograr un mejor rendimiento. Ajuste el tamaño total de la memoria para el demonio de LLAP como se indica a continuación:  

Memoria de demonio de LLAP total = [tamaño de caché de LLAP] + [tamaño del montón] + [capacidad de aumento].

Se recomienda ajustar el tamaño de la memoria caché y el tamaño del montón más adecuados para la carga de trabajo.  

### <a name="hivellapdaemonnumexecutors"></a>hive.llap.daemon.num.executors

Esta configuración controla el número de ejecutores que pueden ejecutar tareas en paralelo por demonio de LLAP. Este valor es un equilibrio entre el número de núcleos virtuales disponibles, la cantidad de memoria proporcionada por ejecutor y la memoria total disponible por demonio de LLAP. Normalmente, nos gustaría que este valor fuera lo más próximo posible al número de núcleos.

En D14 v2, hay 16 núcleos virtuales disponibles; sin embargo, no se pueden proporcionar todos. Los nodos de trabajo también ejecutan otros servicios, como NodeManager, DataNode y el monitor de métricas, que necesitan una parte de los núcleos virtuales disponibles. Este valor se puede configurar hasta el 75 % del total de núcleos virtuales disponibles en ese nodo.

Para D14 v2, se recomienda un valor de (0,75 x 16) = **12**

Se recomienda reservar unos 6 GB de espacio de montón por ejecutor. Ajuste el número de ejecutores según el tamaño de demonio de LLAP disponible y el número de núcleos virtuales disponibles por nodo.  

### <a name="hivellapiothreadpoolsize"></a>hive.llap.io.threadpool.size

Este valor especifica el tamaño del grupo de subprocesos para los ejecutores. Puesto que los ejecutores son fijos tal y como se han especificado, coincidirá con el número de ejecutores por demonio de LLAP.

Para D14 v2, se recomienda establecer el valor en **12**.

Esta configuración no puede superar el valor de `yarn.nodemanager.resource.cpu-vcores`.

### <a name="hiveautoconvertjoinnoconditionaltasksize"></a>hive.auto.convert.join.noconditionaltask.size

Asegúrese de que tiene `hive.auto.convert.join.noconditionaltask` habilitado para que este parámetro surta efecto. Esta configuración permite al usuario especificar el tamaño de las tablas que pueden caber en la memoria para realizar la combinación de asignación. Si la suma del tamaño de n-1 del `tables/partitions` para la combinación de n-way es menor que el valor configurado, se elegirá la combinación de asignación. El tamaño de memoria del ejecutor de LLAP debe usarse para calcular el umbral de conversión automática para la combinación de asignación.

Para D14 v2, se recomienda establecer el valor en **2048 MB**.

Se recomienda ajustar un valor que sea adecuado para la carga de trabajo, porque si es demasiado bajo, es posible que no se use la característica de conversión automática. Si se establece en un valor demasiado alto, se pueden producir pausas de GC, lo que puede afectar negativamente al rendimiento de las consultas.

## <a name="next-steps"></a>Pasos siguientes

* [Directrices de puerta de enlace](gateway-best-practices.md)
* [Desmitificación de la optimización de memoria de Apache Tez: paso a paso](https://community.cloudera.com/t5/Community-Articles/Demystify-Apache-Tez-Memory-Tuning-Step-by-Step/ta-p/245279)
* [Ajuste del tamaño de la memoria de combinación de asignación para LLAP](https://community.cloudera.com/t5/Community-Articles/Map-Join-Memory-Sizing-For-LLAP/ta-p/247462)
* [LLAP: información general sobre la arquitectura de una página](https://community.cloudera.com/t5/Community-Articles/LLAP-a-one-page-architecture-overview/ta-p/247439)
* [Análisis detallado de Hive LLAP](https://community.cloudera.com/t5/Community-Articles/Hive-LLAP-deep-dive/ta-p/248893)
