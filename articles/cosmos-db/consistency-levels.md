---
title: Niveles de coherencia en Azure Cosmos DB
description: Azure Cosmos DB tiene cinco niveles de coherencia para ayudar a equilibrar la coherencia, la disponibilidad y la latencia finales.
author: markjbrown
ms.author: mjbrown
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 04/06/2020
ms.openlocfilehash: 27c1a896d25a0db00ff5f263d949f6657a658e3d
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "91567210"
---
# <a name="what-are-consistency-levels-in-azure-cosmos-db"></a>¿Qué son los niveles de coherencia de Azure Cosmos DB?

Las bases de datos distribuidas que dependen de la replicación para su alta disponibilidad, su baja latencia o ambas, constituyen el compromiso fundamental entre la coherencia de lectura y la disponibilidad, la latencia y el rendimiento. La mayoría de las bases de datos distribuidas disponibles comercialmente solicitan a los desarrolladores que elijan entre los dos modelos de coherencia extrema: coherencia *alta* y *posible* coherencia. La linearización del modelo de coherencia fuerte es el estándar de oro de la programación de datos, pero obliga a pagar el precio de una latencia de escritura más alta (en estado estable) y una menor disponibilidad (durante los errores). Por otro lado, la posible coherencia ofrece una mayor disponibilidad y un mejor rendimiento, pero es difícil programar aplicaciones.

Azure Cosmos DB se aproxima a la coherencia de datos como un espectro de opciones en lugar de como dos extremos. Los desarrolladores pueden usar estas opciones para elegir opciones precisas y compensaciones pormenorizadas con respecto a la alta disponibilidad y al rendimiento.

Con Azure Cosmos DB, los desarrolladores pueden elegir entre cinco niveles de coherencia bien definidos en el espectro de coherencia. Estos niveles incluyen *Alta*, *Obsolescencia limitada*, *Sesión*, *Prefijo coherente* y Coherencia *final*. Los niveles están bien definidos, son intuitivos y se pueden usar en escenarios reales específicos. Cada nivel proporciona [compensaciones entre la disponibilidad y el rendimiento](consistency-levels-tradeoffs.md) y cuenta con el respaldo de Acuerdos de Nivel de Servicio. En la imagen siguiente se muestran los distintos niveles de coherencia como un espectro.

:::image type="content" source="./media/consistency-levels/five-consistency-levels.png" alt-text="Coherencia como espectro" border="false" :::

Los niveles de coherencia son independientes de la región y están garantizados para todas las operaciones, independientemente de la región desde la que se realizan las operaciones de lectura y escritura, del número de regiones asociadas con la cuenta de Azure Cosmos o de si la cuenta está configurada para una o varias regiones de escritura.

## <a name="scope-of-the-read-consistency"></a>Ámbito de la coherencia de lectura

La coherencia de lectura se aplica a una sola operación de lectura limitada a una partición lógica. Un cliente remoto o un procedimiento almacenado pueden emitir la operación de lectura.

## <a name="configure-the-default-consistency-level"></a>Configuración del nivel de coherencia predeterminado

Puede configurar el nivel de coherencia predeterminado de su cuenta de Azure Cosmos en cualquier momento. El nivel de coherencia predeterminado configurado en su cuenta se aplica a todas las bases de datos y contenedores de bases de datos de Azure Cosmos de esa cuenta. Todas las operaciones de lectura y consulta que se emitan con arreglo a un contenedor o una base de datos usan el nivel de coherencia especificado de forma predeterminada. Para más información, consulte cómo [configurar el nivel de coherencia predeterminado](how-to-manage-consistency.md#configure-the-default-consistency-level). También puede invalidar el nivel de coherencia predeterminado para una solicitud específica. Para obtener más información, consulte el artículo [Invalidación del nivel de coherencia predeterminado](how-to-manage-consistency.md?#override-the-default-consistency-level).

## <a name="guarantees-associated-with-consistency-levels"></a>Garantías asociadas a los niveles de coherencia

Los Acuerdos de Nivel de Servicio que proporciona Azure Cosmos DB garantizan que el 100 por ciento de las solicitudes de lectura cumplan con la garantía de coherencia en cualquier nivel de coherencia que elija. Una solicitud de lectura cumple el Acuerdo de Nivel de Servicio de coherencia si se satisfacen todas las garantías de coherencia asociadas al nivel de coherencia. Las definiciones precisas de los cinco niveles de coherencia de Azure Cosmos DB que usan el lenguaje de especificación TLA+ se proporcionan en el repositorio [azure-cosmos-tla](https://github.com/Azure/azure-cosmos-tla) de GitHub.

Aquí se describe la semántica de los cinco niveles de coherencia:

- **Alta**: La coherencia fuerte ofrece una garantía de linearización. La linearización hace referencia a la capacidad de servir solicitudes simultáneamente. Se garantiza que las lecturas devuelven la versión más reciente de un elemento. Un cliente nunca ve una escritura no confirmada ni parcial. Se garantiza que los usuarios siempre leerán la escritura confirmada más reciente.

  En el gráfico siguiente se ilustra la coherencia fuerte con notas musicales. Una vez que los datos se han escrito en la región "Oeste de EE. UU. 2", al leer los datos de otras regiones, obtiene el valor más reciente:

  :::image type="content" source="media/consistency-levels/strong-consistency.gif" alt-text="Coherencia como espectro" se puede configurar de dos maneras:

- El número de versiones (*K*) del elemento
- El intervalo de tiempo (*T*) que las lecturas pueden retrasarse con respecto a las escrituras

La obsolescencia limitada ofrece un orden global total fuera de la "ventana de obsolescencia". Cuando un cliente realiza operaciones de lectura en una región que acepta las escrituras, las garantías que proporciona la obsolescencia limitada son idénticas a las garantías por la coherencia fuerte.

Dentro de la ventana de obsolescencia, Obsolescencia limitada proporciona las siguientes garantías de coherencia:

- Coherencia para clientes de la misma región para una cuenta con región de escritura única = Alta
- Coherencia para clientes de distintas regiones para una cuenta con región de escritura única = De prefijo coherente
- Coherencia para clientes que escriben en una sola región para una cuenta con varias regiones de escritura = De prefijo coherente
- Coherencia para clientes que escriben en diferentes regiones para una cuenta con varias regiones de escritura = Final

  Las aplicaciones distribuidas globalmente que esperan bajas latencias de escritura pero que necesitan una garantía de orden global, suelen elegir la obsolescencia limitada. La obsolescencia limitada es ideal para las aplicaciones que se caracterizan por la colaboración en grupo y el uso compartido, el tablero de cotizaciones, la publicación o suscripción, la puesta en cola, etc. En el gráfico siguiente se ilustra la coherencia de obsolescencia limitada con notas musicales. Una vez que los datos se han escrito en la región "Oeste de EE. UU. 2", las regiones "Este de EE. UU. 2" y "Este de Australia" leen el valor escrito según el tiempo de retardo máximo configurado o las operaciones máximas:

  :::image type="content" source="media/consistency-levels/bounded-staleness-consistency.gif" alt-text="Coherencia como espectro" individual o el uso compartido del token de sesión para varios escritores.

Los clientes que están fuera de la sesión que realiza escrituras verán las garantías siguientes:

- Coherencia para clientes de la misma región para una cuenta con región de escritura única = De prefijo coherente
- Coherencia para clientes de distintas regiones para una cuenta con región de escritura única = De prefijo coherente
- Coherencia para clientes que escriben en una sola región para una cuenta con varias regiones de escritura = De prefijo coherente
- Coherencia para clientes que escriben en varias regiones para una cuenta con varias regiones de escritura = Final

  La coherencia de la sesión es el nivel de coherencia más usado para regiones únicas, así como para aplicaciones distribuidas globalmente. Proporciona latencias de escritura, disponibilidad y rendimiento de lectura equiparables a los de la coherencia final, pero también proporciona las garantías de coherencia que satisfacen las necesidades de aplicaciones escritas para funcionar en el contexto de un usuario. En el gráfico siguiente se ilustra la coherencia de la sesión con notas musicales. El "escritor de Oeste de EE. UU. 2" y el "lector de Oeste de EE. UU. 2" usan la misma sesión (sesión A), por lo que ambos leen los mismos datos al mismo tiempo. Sin embargo, la región "Este de Australia" usa la "sesión B", por lo que recibe los datos más tarde pero en el mismo orden en que se escriben.

  :::image type="content" source="media/consistency-levels/session-consistency.gif" alt-text="Coherencia como espectro":::

- **De prefijo coherente**: Las actualizaciones que se devuelven contienen prefijos para todas las actualizaciones, sin espacios. El nivel de coherencia del prefijo coherente garantiza que las lecturas nunca vean escrituras desordenadas.

Si se realizan escrituras en el orden `A, B, C`, un cliente ve `A`, `A,B` o `A,B,C`, pero nunca permutaciones fuera de servicio como `A,C` o `B,A,C`. El prefijo coherente proporciona latencias de escritura, disponibilidad y rendimiento de lectura equiparables a los de la coherencia final, pero también proporciona las garantías de orden que se ajustan a las necesidades de escenarios en los que el orden es importante. 

A continuación se muestran las garantías de coherencia para Prefijo coherente:

- Coherencia para clientes de la misma región para una cuenta con región de escritura única = De prefijo coherente
- Coherencia para clientes de distintas regiones para una cuenta con región de escritura única = De prefijo coherente
- Coherencia para clientes que escriben en una sola región para una cuenta con varias regiones de escritura = De prefijo coherente
- Coherencia para clientes que escriben en varias regiones para una cuenta con varias regiones de escritura = Final

En el gráfico siguiente se ilustra la coherencia del prefijo de coherencia con notas musicales. En todas las regiones, las lecturas no ven nunca las escrituras desordenadas:

  :::image type="content" source="media/consistency-levels/consistent-prefix.gif" alt-text="Coherencia como espectro":::

- **Ocasional**: No hay ninguna garantía de ordenación para las lecturas. En ausencia de escrituras adicionales, las réplicas terminarán por converger.  
La coherencia final es la forma más débil de coherencia, ya que un cliente puede leer los valores que son más antiguos que los que había leído antes. La coherencia final es adecuada cuando la aplicación no requiere ninguna garantía de ordenación. Entre los ejemplos se incluye el recuento de retweets, Me gusta o comentarios no encadenados. En el gráfico siguiente se ilustra la coherencia final con notas musicales.

  :::image type="content" source="media/consistency-levels/eventual-consistency.gif" alt-text="Coherencia como espectro":::

## <a name="additional-reading"></a>Lecturas adicionales

Para más información sobre los conceptos de coherencia, lea los artículos siguientes:

- [High-level TLA+ specifications for the five consistency levels offered by Azure Cosmos DB](https://github.com/Azure/azure-cosmos-tla) (Especificaciones TLA+ de alto nivel de los cinco niveles de coherencia que ofrece Azure Cosmos DB)
- [Replicated Data Consistency Explained Through Baseball (video)](https://www.youtube.com/watch?v=gluIh8zd26I) (Vídeo sobre Coherencia de datos replicados explicada mediante el béisbol), por Doug Terry
- [Replicated Data Consistency Explained Through Baseball (whitepaper)](https://www.microsoft.com/research/publication/replicated-data-consistency-explained-through-baseball/) (Coherencia de datos replicados explicada mediante el béisbol [notas del producto]), por Doug Terry
- [Session guarantees for weakly consistent replicated data](https://dl.acm.org/citation.cfm?id=383631) (Garantías de sesión para datos replicados con coherencia débil)
- [Consistency Tradeoffs in Modern Distributed Database Systems Design: CAP is Only Part of the Story](https://www.computer.org/csdl/magazine/co/2012/02/mco2012020037/13rRUxjyX7k) (Compromisos de coherencia en el diseño de sistemas modernos de bases de datos distribuidas: CAP es solo parte de la historia)
- [Probabilistic Bounded Staleness (PBS) for Practical Partial Quorums](https://vldb.org/pvldb/vol5/p776_peterbailis_vldb2012.pdf) [Obsolescencia limitada probabilística (PBS) para cuórums parciales prácticos].
- [Eventual Consistent - Revisited (Coherencia posible: revisión)](https://www.allthingsdistributed.com/2008/12/eventually_consistent.html)

## <a name="next-steps"></a>Pasos siguientes

Para más información sobre los niveles de coherencia de Azure Cosmos DB, lea los artículos siguientes:

- [Elección del nivel de coherencia adecuado para la aplicación](consistency-levels-choosing.md)
- [Niveles de coherencia en Azure Cosmos DB](consistency-levels-across-apis.md)
- [Availability and performance tradeoffs for various consistency levels](consistency-levels-tradeoffs.md) (Compromisos entre rendimiento y disponibilidad en los distintos niveles de coherencia)
- [Configuración del nivel de coherencia predeterminado](how-to-manage-consistency.md#configure-the-default-consistency-level)
- [Invalidación del nivel de coherencia predeterminado](how-to-manage-consistency.md#override-the-default-consistency-level)
