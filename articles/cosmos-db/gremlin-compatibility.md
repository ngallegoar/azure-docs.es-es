---
title: Compatibilidad de Gremlin de Azure Cosmos DB con las características de TinkerPop
description: Documentación de referencia para problemas de compatibilidad del motor Graph
author: SnehaGunda
ms.service: cosmos-db
ms.subservice: cosmosdb-graph
ms.topic: reference
ms.date: 09/10/2019
ms.author: sngun
ms.openlocfilehash: 989a033a843b861c34dc9dbdbced50399f8e5cd7
ms.sourcegitcommit: b55d7c87dc645d8e5eb1e8f05f5afa38d7574846
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/16/2020
ms.locfileid: "81449891"
---
# <a name="azure-cosmos-db-gremlin-compatibility"></a>Compatibilidad de Gremlin de Azure Cosmos DB
El motor Graph de Azure Cosmos DB sigue estrechamente la especificación de pasos transversales de [Apache TinkerPop](https://tinkerpop.apache.org/docs/current/reference/#graph-traversal-steps), pero con algunas diferencias.

## <a name="behavior-differences"></a>Diferencias de comportamiento

* El motor Graph de Azure Cosmos DB ejecuta el recorrido centrando la ***prioridad en la amplitud***, mientras que Gremlin de TinkerPop lo hace centrando la prioridad en la profundidad. Este comportamiento logra un mejor rendimiento en un sistema escalable horizontalmente como Cosmos DB. 

## <a name="unsupported-features"></a>Características no admitidas

* ***[Gremlin Bytecode](https://tinkerpop.apache.org/docs/current/tutorials/gremlin-language-variants/)*** es una especificación independiente del lenguaje de programación para recorridos de grafos. Cosmos DB Graph todavía no lo admite. Use `GremlinClient.SubmitAsync()` y pase el recorrido como una cadena de texto.

* Actualmente, no se admite el establecimiento de la cardinalidad ***`property(set, 'xyz', 1)`***. En su lugar, use `property(list, 'xyz', 1)`. Para más información, consulte [Propiedades Vertex con TinkerPop](http://tinkerpop.apache.org/docs/current/reference/#vertex-properties).

* ***`atch()`*** permite consultar grafos mediante la coincidencia de patrones declarativos. Esta funcionalidad no está disponible.

* No se admiten los ***objetos como propiedades*** en bordes ni vértices. Las propiedades solo pueden ser tipos primitivos o matrices.

* No se admite la ***ordenación por propiedades de la matriz*** `order().by(<array property>)`. Solo se admite la ordenación por tipos primitivos.

* No se admiten los ***tipos JSON no primitivos***. Use los tipos `string`, `number` o `true`/`false`. No se admiten los valores `null`. 

* El serializador ***GraphSONv3*** no se admite actualmente. Use las clases Serializer, Reader y Writer de `GraphSONv2` en la configuración de conexión. Los resultados devueltos por la API de Azure Cosmos DB Gremlin no tienen el mismo formato que el formato GraphSON. 

* Actualmente no se admiten las **expresiones y funciones lambda**. Esto incluye las funciones `.map{<expression>}`, `.by{<expression>}` y `.filter{<expression>}`. Para obtener más información y saber cómo volver a escribirlas con los pasos de Gremlin, vea [Una nota sobre las expresiones lambda](http://tinkerpop.apache.org/docs/current/reference/#a-note-on-lambdas).

* No se admiten las ***transacciones*** debido a la naturaleza distribuida del sistema.  Configure el modelo de coherencia adecuado en la cuenta de Gremlin para "leer sus propias escrituras", y use la simultaneidad optimista para resolver las escrituras conflictivas.

## <a name="next-steps"></a>Pasos siguientes
* Visite la página [Voz del usuario de Cosmos DB](https://feedback.azure.com/forums/263030-azure-cosmos-db) para compartir comentarios y ayudar al equipo a centrarse en las características que son importantes para usted.
