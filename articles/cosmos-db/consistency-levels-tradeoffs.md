---
title: Inconvenientes de la coherencia, disponibilidad y rendimiento en Azure Cosmos DB
description: Compromisos entre rendimiento y disponibilidad en los distintos niveles de coherencia en Azure Cosmos DB.
author: markjbrown
ms.author: mjbrown
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 04/23/2020
ms.reviewer: sngun
ms.openlocfilehash: 4de696e2538bf1fa4823aafe30f931b7852535a7
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/02/2020
ms.locfileid: "82191743"
---
# <a name="consistency-availability-and-performance-tradeoffs"></a>Inconvenientes de la coherencia, disponibilidad y rendimiento

Las bases de datos distribuidas que dependen de la replicación para la alta disponibilidad, la baja latencia o ambas deben realizar compensaciones. Tales compensaciones se realizan entre la coherencia y la disponibilidad de lectura, la latencia y el rendimiento.

Azure Cosmos DB se aproxima a la coherencia de datos como un espectro de opciones. Este enfoque abarca opciones que van más allá de los dos extremos de coherencia (alta y ocasional). Puede elegir entre cinco niveles bien definidos en el espectro de la coherencia. De más fuerte a más débil, los niveles son:

- *Fuerte*
- *Obsolescencia limitada*
- *De sesión*
- *De prefijo coherente*
- *Posible*

Cada nivel proporciona compensaciones entre la disponibilidad y el rendimiento y cuenta con el respaldo de Acuerdos de Nivel de Servicio completos.

## <a name="consistency-levels-and-latency"></a>Latencia y niveles de coherencia

Se garantiza que la latencia de lectura de todos los niveles de coherencia siempre es inferior a 10 milisegundos en el percentil 99. Esta latencia de escritura está respaldada por el Acuerdo de Nivel de Servicio. La latencia media de lectura (en el percentil 50) es normalmente de cuatro milisegundos o menos.

Se garantiza que la latencia de escritura de todos los niveles de coherencia siempre sea inferior a 10 milisegundos en el percentil 99. Esta latencia de escritura está respaldada por el Acuerdo de Nivel de Servicio. La latencia media de escritura (en el percentil 50) es normalmente de cinco milisegundos o menos. Las cuentas de Azure Cosmos que abarcan varias regiones y están configuradas con una coherencia alta son una excepción a esta garantía.

### <a name="write-latency-and-strong-consistency"></a>Latencia de escritura y coherencia fuerte

Para las cuentas de Azure Cosmos configuradas con coherencia sólida con más de una región, la latencia de escritura es igual al doble del tiempo de ida y vuelta (RTT) entre cualquiera de las dos regiones más alejadas, más de diez milisegundos en el percentil 99. El RTT de red alto entre las regiones se traducirá a una latencia mayor para solicitudes de Cosmos DB, ya que la coherencia fuerte completa una operación solo después de asegurarse de que se ha confirmado en todas las regiones de una cuenta.

La latencia de RTT exacta depende de la distancia a la velocidad de la luz y la topología de red de Azure. Redes de Azure no proporciona ningún Acuerdo de Nivel de Servicio de latencia para el RTT entre dos regiones de Azure. Para la cuenta de Azure Cosmos, las latencias de replicación se muestran en Azure Portal. Puede usar Azure Portal (vaya a la hoja Métrica, seleccione la pestaña Coherencia) para supervisar las latencias de replicación entre diversas regiones asociadas con su cuenta de Azure Cosmos.

> [!IMPORTANT]
> La coherencia fuerte para las cuentas con regiones que abarcan más de 5000 millas (8000 kilómetros) se bloquea de forma predeterminada debido a una elevada latencia de escritura. Para habilitar esta funcionalidad, póngase en contacto con el soporte técnico.

## <a name="consistency-levels-and-throughput"></a>Rendimiento y niveles de coherencia

- En el caso de la obsolescencia fuerte y limitada, las lecturas se realizan en dos réplicas en un conjunto de cuatro réplicas (cuórum minoritario) para proporcionar garantías de coherencia. Los niveles de sesión, prefijo coherente y posible realizan lecturas de réplica única. El resultado es que, para el mismo número de unidades de solicitud, el rendimiento de lectura para la obsolescencia fuerte y limitada es la mitad de los demás niveles de coherencia.

- Para un tipo determinado de operación de escritura (por ejemplo, Insert, Replace, Upsert o Delete), el rendimiento de escritura de las unidades de solicitud es idéntico para todos los niveles de coherencia.

|**Nivel de coherencia**|**Lecturas de cuórum**|**Escrituras de cuórum**|
|--|--|--|
|**Fuerte**|Minoría local|Mayoría global|
|**Obsolescencia limitada**|Minoría local|Mayoría local|
|**De sesión**|Réplica única (con token de sesión)|Mayoría local|
|**De prefijo coherente**|Réplica única|Mayoría local|
|**Posible**|Réplica única|Mayoría local|

## <a name="consistency-levels-and-data-durability"></a><a id="rto"></a>Durabilidad de los datos y niveles de coherencia

En un entorno de base de datos distribuida de forma global, existe una relación directa entre el nivel de coherencia y la durabilidad de los datos se produce una interrupción en toda la región. A medida que desarrolle el plan de continuidad empresarial, tendrá que saber el tiempo máximo aceptable para que la aplicación se recupere por completo tras un evento de interrupción. El tiempo necesario para que una aplicación se recupere totalmente se conoce como **objetivo de tiempo de recuperación** (**RTO**). También debe conocer el período máximo de actualizaciones de datos recientes que la aplicación puede tolerar perder al recuperarse después de un evento de interrupción. El período de tiempo de las actualizaciones que se puede permitir perder se conoce como **objetivo de punto de recuperación** (**RPO**).

En la tabla siguiente se define la relación entre el modelo de coherencia y la durabilidad de los datos si se produce una interrupción en toda la región. Es importante tener en cuenta que, en un sistema distribuido, aunque la coherencia sea sólida, el teorema de CAP determina que no es posible tener una base de datos distribuida con un RPO y un RTO de cero. Encontrará más información en [Niveles de coherencia en Azure Cosmos DB](consistency-levels.md).

|**Regiones**|**Modo de replicación**|**Nivel de coherencia**|**RPO**|**RTO**|
|---------|---------|---------|---------|---------|
|1|Arquitectura única o multimaestro|Cualquier nivel de coherencia|< 240 minutos|<1 semana|
|>1|Maestro único|Sesión, prefijo coherente, eventual|< 15 minutos|< 15 minutos|
|>1|Maestro único|De obsolescencia entrelazada|*K* & *T*|< 15 minutos|
|>1|Maestro único|Alta|0|< 15 minutos|
|>1|Arquitectura multimaestro|Sesión, prefijo coherente, eventual|< 15 minutos|0|
|>1|Arquitectura multimaestro|De obsolescencia entrelazada|*K* & *T*|0|

*K* = número de versiones *"K"* (es decir, actualizaciones) de un elemento.

*T* = intervalo de tiempo *"T"* desde la última actualización.

## <a name="strong-consistency-and-multi-master"></a>Coherencia fuerte y arquitectura multimaestro

Las cuentas de Cosmos configuradas para la arquitectura multimaestro no se pueden configurar para coherencia fuerte, ya que no es posible que un sistema distribuido proporcione un RPO de cero y un RTO de cero. Además, no hay ventaja en la latencia de escritura por usar la coherencia fuerte con la arquitectura multimaestro, ya que cualquier escritura en cualquier región debe replicarse y confirmarse en todas las regiones configuradas dentro de la cuenta. Esto produce la misma latencia de escritura que una sola cuenta maestra.

## <a name="next-steps"></a>Pasos siguientes

Obtenga más información sobre las compensaciones entre distribución global y coherencia general en sistemas distribuidos. Vea los artículos siguientes:

- [Consistency Tradeoffs in Modern Distributed Database Systems Design](https://www.computer.org/csdl/magazine/co/2012/02/mco2012020037/13rRUxjyX7k) (Compromisos de coherencia en el diseño de sistemas modernos de bases de datos distribuidas)
- [Alta disponibilidad](high-availability.md)
- [Acuerdo de Nivel de Servicio de Azure Cosmos DB](https://azure.microsoft.com/support/legal/sla/cosmos-db/v1_2/)
