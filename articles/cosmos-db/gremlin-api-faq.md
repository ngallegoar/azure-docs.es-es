---
title: Preguntas más frecuentes sobre la API de Gremlin en Azure Cosmos DB
description: Obtenga respuestas a las preguntas más frecuentes sobre la API de Gremlin para Azure Cosmos DB
author: SnehaGunda
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 04/28/2020
ms.author: sngun
ms.openlocfilehash: 416cf4f027b6f1e72641324be39ba0304301db37
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "82613985"
---
# <a name="frequently-asked-questions-about-the-gremlin-api-in-azure-cosmos-db"></a>Preguntas más frecuentes sobre la API de Gremlin en Azure Cosmos DB

En este artículo se explican las respuestas a algunas de las preguntas más frecuentes sobre la API de Gremlin en Azure Cosmos DB.

## <a name="how-to-evaluate-the-efficiency-of-gremlin-queries"></a>Evaluación de la eficiencia de las consultas de Gremlin

Se puede usar el paso de vista previa **executionProfile()** para realizar un análisis del plan de ejecución de la consulta. Este paso debe agregarse al final de cualquier consulta de Gremlin, tal y como se muestra en el ejemplo siguiente:

**Ejemplo de consulta**

```
g.V('mary').out('knows').executionProfile()
```

**Salida del ejemplo**

```json
[
  {
    "gremlin": "g.V('mary').out('knows').executionProfile()",
    "totalTime": 8,
    "metrics": [
      {
        "name": "GetVertices",
        "time": 3,
        "annotations": {
          "percentTime": 37.5
        },
        "counts": {
          "resultCount": 1
        }
      },
      {
        "name": "GetEdges",
        "time": 5,
        "annotations": {
          "percentTime": 62.5
        },
        "counts": {
          "resultCount": 0
        },
        "storeOps": [
          {
            "count": 0,
            "size": 0,
            "time": 0.6
          }
        ]
      },
      {
        "name": "GetNeighborVertices",
        "time": 0,
        "annotations": {
          "percentTime": 0
        },
        "counts": {
          "resultCount": 0
        }
      },
      {
        "name": "ProjectOperator",
        "time": 0,
        "annotations": {
          "percentTime": 0
        },
        "counts": {
          "resultCount": 0
        }
      }
    ]
  }
]
```

La salida del perfil anterior muestra cuánto tiempo se dedica a obtener los objetos de vértice y borde, así como el tamaño del conjunto de datos de trabajo. Esto tiene que ver con las mediciones de los costos estándar para las consultas de Azure Cosmos DB.

## <a name="other-frequently-asked-questions"></a>Otras preguntas frecuentes

### <a name="how-are-rus-charged-when-running-queries-on-a-graph-database"></a>¿Cómo se cobran las RU/s cuando se ejecutan consultas en una base de datos de grafos?

Todos los objetos de gráficos, vértices y bordes se muestran como documentos JSON en el servidor back-end. Como una consulta de Gremlin puede modificar uno o varios objetos de grafo a la vez, el costo asociado con él está directamente relacionado con los objetos, los bordes que la consulta procesa. Este es el mismo proceso que Azure Cosmos DB usa para las demás API. Para más información, vea [Unidades de solicitud en Azure Cosmos DB](request-units.md).

El cargo por las RU se basa en el conjunto de datos de trabajo del recorrido, y no en el conjunto de resultados. Por ejemplo, si una consulta pretende obtener como resultado un solo vértice pero, para ello, debe atravesar más de un objeto en el camino, el costo se basará en todos los objetos de gráfico que se necesiten para procesar el resultado de un vértice.

### <a name="whats-the-maximum-scale-that-a-graph-database-can-have-in-azure-cosmos-db-gremlin-api"></a>¿Cuál es la escala máxima que puede tener una base de datos de grafos de Gremlin API de Azure Cosmos DB?

Azure Cosmos DB usa [particiones horizontales](partition-data.md) para abordar automáticamente el aumento en los requisitos de almacenamiento y capacidad de proceso. La capacidad de rendimiento y la capacidad de almacenamiento máximas de una carga de trabajo vienen determinadas por el número de particiones que están asociadas con un contenedor determinada. Sin embargo, un contenedor de Gremlin API tiene un conjunto específico de directrices para garantizar una experiencia de rendimiento adecuada a escala. Para más información sobre la creación de particiones y procedimientos recomendados, vea el artículo sobre [creación de particiones en Azure Cosmos DB](partition-data.md).

### <a name="for-cnet-development-should-i-use-the-microsoftazuregraphs-package-or-gremlinnet"></a>¿Para desarrollar con C# o .NET, debo usar el paquete de Microsoft.Azure.Graphs o Gremlin.NET?

Gremlin API de Azure Cosmos DB aprovecha los controladores de código abierto como conectores principales para el servicio. Por lo tanto, la opción recomendada es usar [controladores que sean compatibles con Apache Tinkerpop](https://tinkerpop.apache.org/).

### <a name="how-can-i-protect-against-injection-attacks-using-gremlin-drivers"></a>¿Cómo me puedo proteger frente a ataques de inyección de código con los controladores Gremlin?

La mayoría de los controladores Gremlin de Apache Tinkerpop nativos ofrecen la opción de proporcionar un diccionario de parámetros para la ejecución de consultas. Este es un ejemplo de cómo hacerlo en [Gremlin.Net](https://tinkerpop.apache.org/docs/3.2.7/reference/#gremlin-DotNet) y en [Gremlin-Javascript](https://github.com/Azure-Samples/azure-cosmos-db-graph-nodejs-getting-started/blob/master/app.js).

### <a name="why-am-i-getting-the-gremlin-query-compilation-error-unable-to-find-any-method-error"></a>¿Por qué recibo el mensaje "Gremlin Query Compilation Error: Unable to find any method" (Error de compilación de consultas de Gremlin: no se encuentra ningún método)?

Gremlin API de Azure Cosmos DB implementa un subconjunto de la funcionalidad definida en el área expuesta de Gremlin. Para ver pasos admitidos y obtener más información, consulte el artículo sobre [compatibilidad con Gremlin](gremlin-support.md).

La mejor solución es volver a escribir los pasos necesarios de Gremlin con la funcionalidad admitida, porque todos los pasos de Gremlin esenciales son compatibles con Azure Cosmos DB.

### <a name="why-am-i-getting-the-websocketexception-the-server-returned-status-code-200-when-status-code-101-was-expected-error"></a>¿Por qué recibo el mensaje "WebSocketException: The server returned status code "200" when status code "101" was expected" (WebSocketException: el servidor devolvió el código de estado "200" cuando el código de estado esperado era "101")?

Este error probablemente se produce cuando se usa el punto de conexión incorrecto. El punto de conexión que genera este error tiene el siguiente patrón:

`https:// YOUR_DATABASE_ACCOUNT.documents.azure.com:443/`

Este es el punto de conexión de documentos de la base de datos de grafos.  El punto de conexión correcto que hay que usar es el punto de conexión de Gremlin, que tiene el formato siguiente:

`https://YOUR_DATABASE_ACCOUNT.gremlin.cosmosdb.azure.com:443/`

### <a name="why-am-i-getting-the-requestrateistoolarge-error"></a>¿Por qué recibo el error "RequestRateIsTooLarge"?

Este error significa que las unidades de solicitud por segundo asignadas no son suficientes para atender la consulta. Este error suele aparecer cuando se ejecuta una consulta que obtiene todos los vértices:

```
// Query example:
g.V()
```

Esta consulta intentará recuperar todos los vértices del grafo. Por lo tanto, el costo de esta consulta será igual que el número de vértices en términos de RU, como mínimo. El valor de RU/s se debería ajustar para poder atender esta consulta.

### <a name="why-do-my-gremlin-driver-connections-get-dropped-eventually"></a>¿Por qué las conexiones del controlador de Gremlin terminan por desconectarse?

Las conexiones de Gremlin se realizan a través de una conexión de WebSocket. Aunque las conexiones WebSocket no tienen un tiempo de vida específico, Gremlin API de Azure Cosmos DB terminará las conexiones inactivas después de 30 minutos de inactividad.

### <a name="why-cant-i-use-fluent-api-calls-in-the-native-gremlin-drivers"></a>¿Por qué no puedo usar llamadas a API fluida en los controladores nativos de Gremlin?

Gremlin API de Azure Cosmos DB aún no admite llamadas API fluidas. Las llamadas API fluidas requieren una característica de formato interna conocida como compatibilidad con código de bytes, que actualmente no se admite en Gremlin API de Azure Cosmos DB. Por el mismo motivo, el controlador más reciente de Gremlin-JavaScript tampoco es compatible.

## <a name="next-steps"></a>Pasos siguientes

* [Compatibilidad con protocolo de transferencia de Azure Cosmos DB con Gremlin](gremlin-support.md)
* Creación, consulta y recorrido de una base de datos de grafos de Azure Cosmos DB en la [consola de Gremlin](create-graph-gremlin-console.md)
