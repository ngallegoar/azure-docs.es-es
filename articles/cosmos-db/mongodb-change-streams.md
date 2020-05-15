---
title: Flujos de cambios en la API de Azure Cosmos DB para MongoDB
description: Obtenga información sobre cómo usar los flujos de cambios en la API de Azure Cosmos DB para MongoDB con el fin de obtener los cambios realizados en sus datos.
author: timsander1
ms.service: cosmos-db
ms.subservice: cosmosdb-mongo
ms.topic: conceptual
ms.date: 03/30/2020
ms.author: tisande
ms.openlocfilehash: 7a6060448175530ada5ba95ceda470056a7be002
ms.sourcegitcommit: f57297af0ea729ab76081c98da2243d6b1f6fa63
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/06/2020
ms.locfileid: "82872147"
---
# <a name="change-streams-in-azure-cosmos-dbs-api-for-mongodb"></a>Flujos de cambios en la API de Azure Cosmos DB para MongoDB

La compatibilidad con la [fuente de cambios](change-feed.md) en la API de Azure Cosmos DB para MongoDB está disponible mediante la API de flujos de cambios. Mediante el uso de la API de flujos de cambios, las aplicaciones pueden obtener los cambios realizados en la colección o en los elementos de una sola partición. Más adelante puede realizar otras acciones en función de los resultados. Los cambios en los elementos de la colección se capturan en el orden de su hora de modificación, y el criterio de ordenación se garantiza por clave de partición.

> [!NOTE]
> Para usar secuencias de cambios, cree la cuenta con la versión 3.6 de la API de Azure Cosmos DB para MongoDB, o una versión posterior. Si ejecuta ejemplos de secuencias de cambios con una versión anterior, es posible que vea el error `Unrecognized pipeline stage name: $changeStream`.

## <a name="current-limitations"></a>Limitaciones actuales

Se aplican las siguientes limitaciones al usar flujos de cambios:

* Todavía no se admiten las propiedades `operationType` y `updateDescription` en el documento de salida.
* Actualmente se admiten los tipos de operaciones `insert`, `update`y `replace`. 
* Todavía no se admiten la operación de eliminación ni otros eventos.

Debido a estas limitaciones, se requieren la fase $match, la fase $project y la opción fullDocument, tal y como se muestra en los ejemplos anteriores.

A diferencia de la fuente de cambios de la API SQL de Azure Cosmos DB, no hay una [biblioteca de procesadores de fuente de cambios](change-feed-processor.md) independiente para usar los flujos de cambios, ni es necesaria para un contenedor de concesiones. Actualmente no se admiten los [desencadenadores de Azure Functions](change-feed-functions.md) para procesar flujos de cambios.

## <a name="error-handling"></a>Control de errores

Se admiten los siguientes mensajes y códigos de error cuando se utilizan flujos de cambios:

* **Código de error HTTP 16500**: cuando se limita el flujo de cambios, se devuelve una página vacía.

* **NamespaceNotFound (OperationType Invalidate)** : Si ejecuta el flujo de cambios en la colección que no existe o si se quita la colección, se devuelve un error `NamespaceNotFound`. Dado que no se puede devolver la propiedad `operationType` en el documento de salida, en lugar del error `operationType Invalidate`, se devuelve el error `NamespaceNotFound`.

## <a name="examples"></a>Ejemplos

En el ejemplo siguiente se muestra cómo obtener flujos de cambios en todos los elementos de la colección. En este ejemplo se crea un cursor para inspeccionar los elementos cuando se insertan, actualizan o reemplazan. La fase `$match`, la fase `$project` y la opción `fullDocument` son necesarias para obtener los flujos de cambios. Actualmente no se admite la inspección de las operaciones de eliminación mediante flujos de cambios. Como solución alternativa, puede agregar un marcador flexible en los elementos que se van a eliminar. Por ejemplo, puede agregar un atributo en el elemento denominado "deleted". Si quiere eliminar el elemento, puede establecer "deleted" en `true` y establecer un TTL en el elemento. Ya que el cambio del elemento "deleted" a `true` es una actualización, este cambio será visible en el flujo de cambios.

# <a name="javascript"></a>[JavaScript](#tab/javascript)

```javascript
var cursor = db.coll.watch(
    [
        { $match: { "operationType": { $in: ["insert", "update", "replace"] } } },
        { $project: { "_id": 1, "fullDocument": 1, "ns": 1, "documentKey": 1 } }
    ],
    { fullDocument: "updateLookup" });

while (!cursor.isExhausted()) {
    if (cursor.hasNext()) {
        printjson(cursor.next());
    }
}
```
# <a name="c"></a>[C#](#tab/csharp)

```csharp
var pipeline = new EmptyPipelineDefinition<ChangeStreamDocument<BsonDocument>>()
    .Match(change => change.OperationType == ChangeStreamOperationType.Insert || change.OperationType == ChangeStreamOperationType.Update || change.OperationType == ChangeStreamOperationType.Replace)
    .AppendStage<ChangeStreamDocument<BsonDocument>, ChangeStreamDocument<BsonDocument>, BsonDocument>(
    "{ $project: { '_id': 1, 'fullDocument': 1, 'ns': 1, 'documentKey': 1 }}");

var options = new ChangeStreamOptions{
        FullDocument = ChangeStreamFullDocumentOption.UpdateLookup
    };

var enumerator = coll.Watch(pipeline, options).ToEnumerable().GetEnumerator();

while (enumerator.MoveNext()){
        Console.WriteLine(enumerator.Current);
    }

enumerator.Dispose();
```

## <a name="changes-within-a-single-shard"></a>Cambios en una sola partición

En el ejemplo siguiente se muestra cómo obtener los cambios en los elementos de una sola partición. En este ejemplo se obtienen los cambios de los elementos que tienen una clave de partición igual a "a" y el valor de clave de partición igual a "1". Es posible que distintos clientes lean los cambios de diferentes particiones en paralelo.

```javascript
var cursor = db.coll.watch(
    [
        {
            $match: {
                $and: [
                    { "fullDocument.a": 1 }, 
                    { "operationType": { $in: ["insert", "update", "replace"] } }
                ]
            }
        },
        { $project: { "_id": 1, "fullDocument": 1, "ns": 1, "documentKey": 1} }
    ],
    { fullDocument: "updateLookup" });

```

## <a name="next-steps"></a>Pasos siguientes

* [Usar el período de vida para expirar automáticamente los datos en la API de Azure Cosmos DB para MongoDB](mongodb-time-to-live.md)
* [Indexación en la API de Azure Cosmos DB para MongoDB](mongodb-indexing.md)
