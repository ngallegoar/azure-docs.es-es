---
title: Guía de ajuste de tamaño del clúster de HDInsight Interactive Query (LLAP)
description: Guía de ajuste de tamaño de LLAP
ms.service: hdinsight
ms.topic: troubleshooting
author: aniket-ms
ms.author: aadnaik
ms.reviewer: HDI HiveLLAP Team
ms.date: 05/05/2020
ms.openlocfilehash: a9b86f09ade0d437436779ef3e4a17fcdede2cf0
ms.sourcegitcommit: fdec8e8bdbddcce5b7a0c4ffc6842154220c8b90
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/19/2020
ms.locfileid: "83663693"
---
# <a name="azure-hdinsight-interactive-query-cluster-hive-llap-sizing-guide"></a>Guía de ajuste de tamaño del clúster de Azure HDInsight Interactive Query (Hive LLAP)

En este documento se describe el ajuste de tamaño del clúster de HDInsight Interactive Query (clúster Hive LLAP) de una carga de trabajo típica para lograr un rendimiento razonable. Tenga en cuenta que las recomendaciones proporcionadas en este documento son directrices genéricas y las cargas de trabajo específicas pueden necesitar  un ajuste específico.

### <a name="azure-default-vm-types-for-hdinsight-interactive-query-clusterllap"></a>**Tipos de VM predeterminadas de Azure para el clúster de HDInsight Interactive Query (LLAP)**

| Tipo de nodo      | Instancia | Size     |
| :---        |    :----:   | :---     |
| Head      | D13 v2       | 8 vCPU, 56 GB de RAM, SSD de 400 GB   |
| Trabajo   | **D14 v2**        | **16 vCPU, 112 GB de RAM, SSD de 800 GB**       |
| ZooKeeper   | A4 v2        | 4 vCPU, 8 GB de RAM, SSD de 40 GB       |

***Nota: Todos los valores de las configuraciones recomendadas se basan en el nodo de trabajo de tipo D14 v2***.  

### <a name="configuration"></a>**Configuración:**    
| Clave de configuración      | Valor recomendado  | Descripción |
| :---        |    :----:   | :---     |
| yarn.nodemanager.resource.memory-mb | 102400 (MB) | Memoria total proporcionada, en MB, para todos los contenedores de YARN de un nodo. | 
| yarn.scheduler.maximum-allocation-mb | 102400 (MB) | La asignación máxima para cada solicitud de contenedor en RM, en MB. Las solicitudes de memoria que superen este valor no surtirán efecto. |
| yarn.scheduler.maximum-allocation-vcores | 12 |Número máximo de núcleos de CPU para cada solicitud de contenedor en Resource Manager. Las solicitudes que superen este valor no surtirán efecto. |
| yarn.nodemanager.resource.cpu-vcores | 12 | Número de núcleos de CPU por NodeManager que se pueden asignar para contenedores. |
| yarn.scheduler.capacity.root.llap.capacity | 80 (%) | Asignación de la capacidad de YARN para la cola de LLAP.  |
| tez.am.resource.memory.mb | 4096 (MB) | Cantidad de memoria en MB que va a usar AppMaster de Tez. |
| hive.server2.tez.sessions.per.default.queue | <number_of_worker_nodes> |Número de sesiones de cada cola mencionada en hive.server2.tez.default.queues. Este número corresponde al número de coordinadores de consultas (AM de Tez). |
| hive.tez.container.size | 4096 (MB) | Tamaño de contenedor de Tez especificado en MB. |
| hive.llap.daemon.num.executors | 12 | Número de ejecutores por demonio de LLAP. | 
| hive.llap.io.threadpool.size | 12 | Tamaño del grupo de subprocesos para los ejecutores. |
| hive.llap.daemon.yarn.container.mb | 77824 (MB) | Memoria total en MB usada por demonios de LLAP individuales (memoria por demonio).
| hive.llap.io.memory.size | 235520 (MB) | Tamaño de caché en MB por demonio de LLAP si la caché del disco SSD está habilitada. |
| hive.auto.convert.join.noconditionaltask.size | 2048 (MB) | Tamaño de memoria en MB para realizar la combinación de asignación. |

### <a name="llap-daemon-size-estimations"></a>**Estimaciones de tamaño de demonio de LLAP:**  

#### <a name="1-determining-total-yarn-memory-allocation-for-all-containers-on-a-node"></a>**1. Determinación de la asignación de memoria total de YARN para todos los contenedores de un nodo**    
Configuración: ***yarn.nodemanager.resource.memory-mb***  

Este valor indica una suma máxima de memoria en MB que pueden usar los contenedores de YARN en cada nodo. El valor especificado debe ser inferior a la cantidad total de memoria física en ese nodo.   
Memoria total para todos los contenedores de YARN en un nodo = [memoria física total] – [memoria para SO + otros servicios]  
Establezca este valor en aproximadamente un 90 % del tamaño de RAM disponible.  
Para D14 v2, se recomienda un valor de  **102400 MB**. 

#### <a name="2-determining-maximum-amount-of-memory-per-yarn-container-request"></a>**2. Determinación de la cantidad máxima de memoria por solicitud de contenedor de YARN**  
Configuración: ***yarn.scheduler.maximum-allocation-mb***

Este valor indica la asignación máxima para cada solicitud de contenedor en Resource Manager, en MB. Las solicitudes de memoria que superen el valor especificado no surtirán efecto. Resource Manager puede proporcionar memoria a los contenedores en incrementos de *yarn.scheduler.minimum-allocation-mb* y no puede superar el tamaño especificado por *yarn.scheduler.maximum-allocation-mb*. El valor especificado no debe ser mayor que la memoria total proporcionada para todos los contenedores en el nodo especificado por *yarn.nodemanager.resource.memory-mb*.    
Para los nodos de trabajo D14 v2, se recomienda un valor de **102400 MB**.

#### <a name="3-determining-maximum-amount-of-vcores-per-yarn-container-request"></a>**3. Determinación de la cantidad máxima de núcleos virtuales por solicitud de contenedor de YARN**  
Configuración: ***yarn.scheduler.maximum-allocation-vcores***  

Este valor indica el número máximo de núcleos de CPU virtuales para cada solicitud de contenedor en Resource Manager. La solicitud de un número de núcleos virtuales mayor que este valor no surtirá efecto. Esta es una propiedad global del programador de YARN. Para el contenedor del demonio de LLAP, este valor se puede establecer en el 75 % del total de núcleos virtuales disponibles. El 25 % restante se debe reservar para NodeManager, DataNode y otros servicios que se ejecutan en los nodos de trabajo.  
Hay 16 núcleos virtuales en las VM de D14 v2, de los cuales el contenedor de demonio de LLAP puede usar un 75 %.  
Para D14 v2, se recomienda un valor de **12**.  

#### <a name="4-number-of-concurrent-queries"></a>**4. Número de consultas simultáneas**  
Configuración: ***hive.server2.tez.sessions.per.default.queue***

Este valor de configuración determina el número de sesiones de Tez que deben iniciarse en paralelo. Estas sesiones de Tez se iniciarán para cada una de las colas especificadas por "hive.server2.tez.default.queues". Corresponde al número de AM de Tez (coordinadores de consultas). Se recomienda que sea el mismo que el número de nodos de trabajo. El número de AM de Tez puede ser mayor que el número de nodos de demonio de LLAP. La responsabilidad principal de los AM de Tez es coordinar la ejecución de la consulta y asignar los fragmentos del plan de consulta a los demonios de LLAP correspondientes para su ejecución. Mantenga este valor como múltiplo de varios nodos de demonio de LLAP para lograr un mayor rendimiento.  

El clúster de HDInsight predeterminado tiene cuatro demonios de LLAP que se ejecutan en cuatro nodos de trabajo, por lo que el valor recomendado es **4**.  

#### <a name="5-tez-container-and-tez-application-master-size"></a>**5. Tamaño del maestro de la aplicación Tez y el contenedor de Tez**    
Configuración: ***tez.am.resource.memory.mb, hive.tez.container.size***  

*tez.am.resource.memory.mb* - define el tamaño del maestro de la aplicación Tez.  
Se recomienda un valor de **4096 MB**.
   
*hive.tez.container.size*: define la cantidad de memoria proporcionada para el contenedor de Tez. Este valor se debe establecer entre el tamaño mínimo de contenedor de YARN (*yarn.scheduler.minimum-allocation-mb*) y el tamaño máximo del contenedor de YARN (*yarn.scheduler.maximum-allocation-mb*). Los ejecutores del demonio de LLAP usan este valor para limitar el uso de memoria por ejecutor.  
Se recomienda un valor de **4096 MB**.  

#### <a name="6-llap-queue-capacity-allocation"></a>**6. Asignación de la capacidad de la cola de LLAP**   
Configuración: ***yarn.scheduler.capacity.root.llap.capacity***  

Este valor indica un porcentaje de la capacidad proporcionada a la cola de LLAP. Las asignaciones de capacidad pueden tener valores diferentes para las distintas cargas de trabajo en función de cómo estén configuradas las colas de YARN. Si la carga de trabajo es de operaciones de solo lectura, configurar esta opción en un máximo del 90 % de la capacidad debería funcionar. Sin embargo, si la carga de trabajo es una combinación de operaciones de actualización, eliminación y combinación con tablas administradas, se recomienda proporcionar un 80 % de la capacidad para la cola de LLAP. Otras tareas como la compactación, etc., pueden usar el 20 % de capacidad restante para asignar contenedores desde la cola predeterminada. De este modo, las tareas de la cola predeterminada no desautorizarán los recursos de YARN.    
Para los nodos de trabajo D14v2, se recomienda un valor de **80** para la cola de LLAP.   
(Para cargas de trabajo de solo lectura, se puede aumentar hasta 90 según sea necesario).  

#### <a name="7-llap-daemon-container-size"></a>**7. Tamaño del contenedor del demonio de LLAP**    
Configuración: ***hive.llap.daemon.yarn.container.mb***  
   
El demonio de LLAP se ejecuta como un contenedor de YARN en cada nodo de trabajo. El tamaño total de la memoria para el demonio de LLAP depende de los siguientes factores:    
*  Configuraciones de tamaño de contenedor de YARN (yarn.scheduler.minimum-allocation-mb, yarn.scheduler.maximum-allocation-mb, yarn.nodemanager.resource.memory-mb)
*  Número de AM de Tez en un nodo
*  Memoria total configurada para todos los contenedores en un nodo y capacidad de la cola de LLAP  

La memoria que necesitan los maestros de la aplicación Tez (AM de Tez) se puede calcular de la manera siguiente.  
De forma predeterminada, para un clúster de HDInsight Interactive existe un AM de Tez por nodo de trabajo que actúa como coordinador de consultas. El número de AM de Tez puede configurarse en función del número de consultas simultáneas que se van a atender.
Se recomienda tener 4 GB de memoria por AM de Tez.

Memoria de AM de Tez por nodo = [ceil (número de AM de Tez/número de nodos de demonio de LLAP)] x [tamaño de contenedor de AM de Tez].  
En D14 V2, la configuración predeterminada tiene cuatro AM de Tez y cuatro nodos de demonio de LLAP.  
Memoria de AM de Tez por nodo = (ceil(4/4) x 4 GB) = 4 GB

La memoria total disponible para la cola de LLAP por nodo de trabajo se puede calcular de la manera siguiente: Este valor depende de la cantidad total de memoria disponible para todos los contenedores de YARN de un nodo (*yarn.nodemanager.resource.memory-mb*) y el porcentaje de capacidad configurada para la cola de LLAP (*yarn.scheduler.capacity.root.llap.capacity*).  
Memoria total para la cola de LLAP en el nodo de trabajo = memoria total disponible para todos los contenedores de YARN de un nodo x porcentaje de capacidad para la cola de LLAP.  
Para D14 v2, este valor es [100 GB x 0,80] = 80 GB.

El tamaño del contenedor del demonio de LLAP se calcula como se indica a continuación:

**Tamaño del contenedor del demonio de LLAP = [memoria total disponible para la cola de LLAP] – [Memoria de AM de Tez por nodo]**  
    
Para el nodo de trabajo D14 v2, HDI 4.0: el valor recomendado es (80 GB - 4 GB)) = **76 GB**   
(Para HDI 3.6, el valor recomendado es **74 GB** porque debe reservar unos 2 GB adicionales para el AM del control deslizante).  

#### <a name="8-determining-number-of-executors-per-llap-daemon"></a>**8. Determinación del número de ejecutores por demonio de LLAP**  
Configuración: ***hive.llap.daemon.num.executors***, ***hive.llap.io.threadpool.size***

***hive.llap.daemon.num.executors***:   
Esta configuración controla el número de ejecutores que pueden ejecutar tareas en paralelo por demonio de LLAP. Este valor depende del número de núcleos virtuales, la cantidad de memoria proporcionada por ejecutor y la cantidad de memoria total disponible por demonio de LLAP. Normalmente, nos gustaría que este valor fuera lo más próximo posible al número de núcleos virtuales.
Hay 16 núcleos virtuales en las VM de D14 v2. Sin embargo, no se pueden tomar todos los núcleos virtuales, porque otros servicios, como NodeManager, DataNode, el monitor de métricas, etc. también necesitan parte de los núcleos virtuales disponibles. 

Si necesita ajustar el número de ejecutores, se recomienda que considere 4 GB de memoria por ejecutor según lo especificado por *hive.tez.container.size* y que se asegure de que la memoria total necesaria para todos los ejecutores no supera la memoria total disponible para el contenedor del demonio de LLAP.  
Este valor se puede configurar hasta el 75 % del total de núcleos virtuales disponibles en ese nodo.  
Para D14 v2, se recomienda un valor de (0,75 x 16) = **12**

***hive.llap.io.threadpool.size***:   
Este valor especifica el tamaño del grupo de subprocesos para los ejecutores. Puesto que los ejecutores son fijos tal y como se han especificado, coincidirá con el número de ejecutores por demonio de LLAP.   
Para D14 v2, se recomienda un valor de **12**.

#### <a name="9-determining-llap-daemon-cache-size"></a>**9. Determinación del tamaño de la caché del demonio de LLAP**  
Configuración: ***hive.llap.io.memory.size***

La memoria del contenedor del demonio de LLAP consta de los siguientes componentes:
*  Capacidad de aumento
*  Memoria de montón usada por los ejecutores (Xmx)
*  Caché en memoria por demonio (su tamaño de memoria fuera del montón, no aplicable cuando la memoria caché de SSD está habilitada)
*  Tamaño de los metadatos de caché en memoria (solo se aplica cuando la memoria caché de SSD está habilitada)

**Tamaño de la capacidad de aumento**: este tamaño indica una parte de la memoria fuera del montón usada para la sobrecarga de VM Java (metaespacio, pila de subprocesos, estructuras de datos de GC, etc.). Por lo general, esta sobrecarga es aproximadamente el 6 % del tamaño del montón (Xmx). Para ir a lo seguro, este valor se puede calcular como el 6 % del tamaño total de la memoria del demonio de LLAP.  
Para D14 v2, el valor recomendado es ceil (76 GB x 0,06) ~= **5 GB**.  

**Tamaño del montón (Xmx)** : cantidad de memoria en el montón disponible para todos los ejecutores.
Tamaño total del montón = número de ejecutores x 4 GB.  
En D14 v2, este valor es 12 x 4 GB = **48 GB**.  

Si la memoria caché de SSD está deshabilitada, la caché en memoria es la cantidad de memoria que queda después de tomar el tamaño de la capacidad de aumento y el tamaño del montón del tamaño del contenedor del demonio de LLAP.

El cálculo del tamaño de la caché es diferente cuando la memoria caché de SSD está habilitada.
Si establece *hive.llap.io.allocator.mmap* = true, se habilitará el almacenamiento en caché de SSD.
Si se habilita la memoria caché de SSD, se usará parte de la memoria para almacenar los metadatos de la memoria caché de SSD. Los metadatos se almacenan en memoria y se espera que ocupen aproximadamente el 10 % del tamaño de la caché de SSD.   
Tamaño de metadatos en memoria de la caché de SSD = [tamaño del contenedor del demonio de LLAP] - [capacidad de aumento + tamaño del montón].  
Para D14 v2, con HDI 4.0, el tamaño de los metadatos en memoria de la caché de SSD = [76 GB] - [5 GB + 48 GB] = **23 GB**.  
Para D14 v2, con HDI 3.6, el tamaño de los metadatos en memoria de la caché de SSD = [76 GB] - [control deslizante de 5 GB + 48 GB + 2 GB] = **21 GB**.

Dado el tamaño de la memoria disponible para almacenar los metadatos de la memoria caché de SSD, podemos calcular el tamaño de la memoria caché de SSD que se puede admitir.  
Tamaño de los metadatos en memoria para la caché de SSD = 10 % del tamaño de la memoria caché de SSD.       
Tamaño de la caché de SSD = tamaño de los metadatos en memoria para la caché de SSD x 10.  

Para D14 v2 y HDI 4.0, el tamaño de la caché de SSD recomendado es de 23 GB x 10 = **230 GB**.  
Para D14 v2 y HDI 3.6, el tamaño de la caché de SSD recomendado es de 21 GB x 10 = **210 GB**.

#### <a name="10-adjusting-map-join-memory"></a>**10. Ajuste de la memoria de combinación de asignación**   
Configuración: ***hive.auto.convert.join.noconditionaltask.size***

Asegúrese de que tiene *hive.auto.convert.join.noconditionaltask* habilitado para que este parámetro surta efecto.
Esta configuración permite al usuario especificar el tamaño de las tablas que pueden caber en la memoria para realizar la combinación de asignación. Si la suma del tamaño de n-1 tablas o particiones para la combinación de n-way es menor que el valor configurado, se elegirá la combinación de asignación. El tamaño de memoria del ejecutor de LLAP debe usarse para calcular el umbral de conversión automática para la combinación de asignación.
Se supone que cada ejecutor tiene 4 GB de tamaño en el montón, pero no todos ellos estarán disponibles para la combinación de asignación. También se utilizará parte de la memoria en el montón para los búferes de ordenación, los búferes aleatorios, las tablas hash, etc. en otras operaciones. Por lo tanto, puede proporcionar un 50 % de la memoria en el montón de 4 GB para la combinación de asignación.  
Nota: Este valor puede requerir ajustes que sean adecuados para la carga de trabajo. Si se establece un valor demasiado bajo, es posible que no se use la característica de conversión automática. Asimismo, si se establece un valor demasiado alto, se pueden producir excepciones de memoria insuficiente o pausas de GC que puedan dar lugar a un rendimiento adverso.  
En D14 v2, con 4 GB de memoria por ejecutor, se recomienda establecer este valor en **2048 MB**.


#### <a name="next-steps"></a>**Pasos siguientes**
Si con el establecimiento de este valor no se resuelve el problema, elija una de las opciones siguientes:

* Obtenga respuestas de expertos de Azure mediante el [soporte técnico de la comunidad de Azure](https://azure.microsoft.com/support/community/).

* Póngase en contacto con [@AzureSupport](https://twitter.com/azuresupport), la cuenta oficial de Microsoft Azure para mejorar la experiencia del cliente, que pone en contacto a la comunidad de Azure con los recursos adecuados: respuestas, soporte técnico y expertos.

* Si necesita más ayuda, puede enviar una solicitud de soporte técnico desde [Azure Portal](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade/). Seleccione **Soporte técnico** en la barra de menús o abra la central **Ayuda + soporte técnico**. Para obtener información más detallada, revise [Creación de una solicitud de soporte técnico de Azure](https://docs.microsoft.com/azure/azure-portal/supportability/how-to-create-azure-support-request). La suscripción a Microsoft Azure incluye acceso al soporte técnico para facturación y administración de suscripciones. El soporte técnico se proporciona a través de uno de los [planes de soporte técnico de Azure](https://azure.microsoft.com/support/plans/).  

* ##### <a name="other-references"></a>**Otras referencias:**
  * [Configuración de otras propiedades de LLAP](https://docs.cloudera.com/HDPDocuments/HDP3/HDP-3.1.5/performance-tuning/content/hive_setup_llap.html)  
  * [Configuración del tamaño del montón del servidor de Hive](https://docs.cloudera.com/HDPDocuments/HDP3/HDP-3.1.5/performance-tuning/content/hive_hiveserver_heap_sizing.html)  
  * [Ajuste del tamaño de la memoria de combinación de asignación para LLAP](https://community.cloudera.com/t5/Community-Articles/Map-Join-Memory-Sizing-For-LLAP/ta-p/247462)  
  * [Propiedades del motor de ejecución de Tez](https://docs.cloudera.com/HDPDocuments/HDP3/HDP-3.1.5/performance-tuning/content/hive_tez_engine_properties.html)  
  * [Análisis detallado de Hive LLAP](https://community.cloudera.com/t5/Community-Articles/Hive-LLAP-deep-dive/ta-p/248893)  
