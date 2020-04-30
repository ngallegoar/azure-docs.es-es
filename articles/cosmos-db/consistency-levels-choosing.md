---
title: Selección del nivel de coherencia adecuado en la aplicación de Azure Cosmos DB
description: Elija el nivel de coherencia adecuado para la aplicación en Azure Cosmos DB.
author: markjbrown
ms.author: mjbrown
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 04/23/2020
ms.reviewer: sngun
ms.openlocfilehash: aa8a020dc4a7e9d4e7de0671b566755e413caeb2
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/28/2020
ms.locfileid: "82191760"
---
# <a name="choose-the-right-consistency-level"></a>Selección del nivel de coherencia adecuado 

Las bases de datos distribuidas que dependen de la replicación para su alta disponibilidad, su baja latencia o ambas, constituyen el compromiso fundamental entre la coherencia de lectura y la disponibilidad, la latencia y el rendimiento. La mayoría de las bases de datos distribuidas disponibles comercialmente solicitan a los desarrolladores que elijan entre los dos modelos de coherencia extrema: coherencia *alta* y *posible* coherencia. Azure Cosmos DB permite a los desarrolladores elegir entre cinco modelos de coherencia bien definidos: *alta*, de *obsolescencia limitada*, de *sesión*, de *prefijo coherente* y *final*. Cada uno de estos modelos de coherencia está bien definido, es intuitivo y puede utilizarse para escenarios específicos del mundo real. Cada uno de los cinco modelos de coherencia ofrecen [contrapartidas entre rendimiento y disponibilidad](consistency-levels-tradeoffs.md) precisas, y están respaldados por Acuerdos de Nivel de Servicio. Las siguientes consideraciones sencillas le ayudarán a tomar la decisión correcta en muchos escenarios comunes.

## <a name="sql-api-and-table-api"></a>SQL API y Table API

Tenga en cuenta los siguientes aspectos si la aplicación se compila mediante la SQL API o Table API:

- En muchos escenarios del mundo real, la coherencia de la sesión es óptima y es la opción recomendada. Para más información, vea [Procedimientos para administrar el token de la sesión para la aplicación](how-to-manage-consistency.md#utilize-session-tokens).

- Si la aplicación requiere una sólida coherencia, se recomienda usar el nivel de coherencia de obsolescencia limitada.

- Si necesita garantías de coherencia más estrictas que las proporcionadas por la coherencia de la sesión y la latencia de un solo dígito y milisegundo para las escrituras, se recomienda que utilice el nivel de coherencia de obsolescencia limitada.  

- Si la aplicación requiere una posible coherencia, se recomienda usar el nivel de coherencia de prefijo coherente.

- Si necesita garantías de coherencia menos estrictas que las que proporciona la coherencia de la sesión, se recomienda utilizar el nivel de coherencia de prefijo coherente.

- Si necesita la máxima disponibilidad y latencia más baja, use el nivel de posible coherencia.

- Si necesita obtener una mayor durabilidad de los datos sin tener que sacrificar el rendimiento, puede crear un nivel de coherencia personalizado en el nivel de aplicación. Para obtener más información, consulte [How-to implement custom synchronization in your applications](how-to-custom-synchronization.md) (Cómo implementar la sincronización personalizada en las aplicaciones).

## <a name="cassandra-mongodb-and-gremlin-apis"></a>Cassandra, MongoDB y Gremlin API

- Para más información sobre la asignación entre el nivel de coherencia de lectura ofrecido en Apache Cassandra y los niveles de coherencia de Cosmos DB, consulte [Niveles de coherencia y API de Cosmos DB](consistency-levels-across-apis.md#cassandra-mapping).

- Para obtener más información sobre la asignación entre "Read Concern" de MongoDB y los niveles de coherencia de Azure Cosmos DB, consulte [Consistency levels and Cosmos DB APIs](consistency-levels-across-apis.md#mongo-mapping) (Niveles de coherencia y API de Cosmos DB).

## <a name="consistency-guarantees-in-practice"></a>Garantías de coherencia en la práctica

En la práctica, es posible que obtenga mayores garantías de coherencia. Las garantías de coherencia para una operación de lectura se corresponden con la actualización y el orden del estado de la base de datos que solicita. La coherencia de lectura está asociada con la ordenación y la propagación de las operaciones de escritura/actualización.  

* Cuando se establece el nivel de coherencia en **obsolescencia limitada**, Cosmos DB garantiza que los clientes siempre leerán el valor de una escritura anterior, con un intervalo limitado por la ventana de obsolescencia.

* Cuando se establece el nivel de coherencia en **alto**, la ventana de obsolescencia equivale a cero y se garantiza que los clientes leerán el último valor confirmado de la operación de escritura.

* Para los tres niveles de coherencia restantes, la ventana de obsolescencia depende en gran medida la carga de trabajo. Por ejemplo, si no hay ninguna operación de escritura en la base de datos, una operación de lectura con los niveles de coherencia **posible**, **sesión** o **prefijo coherente** es probable que produzca los mismos resultados que una operación de lectura con el nivel de coherencia alto.

Si la cuenta de Azure Cosmos está configurada con un nivel de coherencia que no es el de coherencia alta, puede averiguar la probabilidad de que los clientes obtengan lecturas de coherencia alta para sus cargas de trabajo consultando la métrica de *obsolescencia limitada de probabilidad* (PBS). Esta métrica se expone en Azure Portal; para obtener más información, consulte [Supervisión de la métrica de obsolescencia limitada de manera probabilística (PBS)](how-to-manage-consistency.md#monitor-probabilistically-bounded-staleness-pbs-metric).

La obsolescencia limitada de probabilidad muestra cómo de posible es la coherencia final. Esta métrica proporciona una visión general de la frecuencia con la que puede obtener una coherencia mayor que el nivel de coherencia que tiene configurado actualmente en su cuenta de Azure Cosmos. En otras palabras, puede ver la probabilidad (en milisegundos) de obtener lecturas con coherencia alta para una combinación de regiones de escritura y lectura.

## <a name="next-steps"></a>Pasos siguientes

Obtenga más información acerca de los niveles de coherencia en los siguientes artículos:

* [Consistency level mapping across Cosmos DB APIs](consistency-levels-across-apis.md) (Asignación de niveles de coherencia en las API de Cosmos DB)
* [Availability and performance tradeoffs for various consistency levels](consistency-levels-tradeoffs.md) (Compromisos entre rendimiento y disponibilidad en los distintos niveles de coherencia)
* [How to manage the session token for your application](how-to-manage-consistency.md#utilize-session-tokens) (Cómo administrar el token de sesión de su aplicación)
* [Supervisión de la métrica de obsolescencia limitada de manera probabilística (PBS)](how-to-manage-consistency.md#monitor-probabilistically-bounded-staleness-pbs-metric)
