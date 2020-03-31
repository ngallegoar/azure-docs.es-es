---
title: Flujos de cambios en la API de Azure Cosmos DB para MongoDB
description: Obtenga información sobre cómo usar los flujos de cambios en la API de Azure Cosmos DB para MongoDB con el fin de obtener los cambios realizados en sus datos.
author: srchi
ms.service: cosmos-db
ms.subservice: cosmosdb-mongo
ms.topic: conceptual
ms.date: 11/16/2019
ms.author: srchi
ms.openlocfilehash: ec1ec1a8a80953f8988355341ee7128bd29b982d
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "77467784"
---
# <a name="change-streams-in-azure-cosmos-dbs-api-for-mongodb"></a>Flujos de cambios en la API de Azure Cosmos DB para MongoDB

La compatibilidad con la [fuente de cambios](change-feed.md) en la API de Azure Cosmos DB para MongoDB está disponible mediante la API de flujos de cambios. Mediante el uso de la API de flujos de cambios, las aplicaciones pueden obtener los cambios realizados en la colección o en los elementos de una sola partición. Más adelante puede realizar otras acciones en función de los resultados. Los cambios en los elementos de la colección se capturan en el orden de su hora de modificación, y el criterio de ordenación se garantiza por clave de partición.

> [!NOTE]
> Para usar secuencias de cambios, cree la cuenta con la versión 3.6 de la API de Azure Cosmos DB para MongoDB, o una versión posterior. Si ejecuta ejemplos de secuencias de cambios con una versión anterior, es posible que vea el error `Unrecognized pipeline stage name: $changeStream`. 

En el ejemplo siguiente se muestra cómo obtener flujos de cambios en todos los elementos de la colección. En este ejemplo se crea un cursor para inspeccionar los elementos cuando se insertan, actualizan o reemplazan. La fase $match, la fase $project y la opción fullDocument son necesarias para obtener los flujos de cambios. Actualmente no se admite la inspección de las operaciones de eliminación mediante flujos de cambios. Como solución alternativa, puede agregar un marcador flexible en los elementos que se van a eliminar. Por ejemplo, puede agregar un atributo en el elemento denominado "deleted" y establecerlo en "true" y establecer un TTL en el elemento, de modo que pueda eliminarlo automáticamente, así como realizarle un seguimiento.

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

En el ejemplo siguiente se muestra cómo obtener cambios en los elementos de una sola partición. En este ejemplo se obtienen los cambios de los elementos que tienen una clave de partición igual a "a" y el valor de clave de partición igual a "1".

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

## <a name="current-limitations"></a>Limitaciones actuales

Se aplican las siguientes limitaciones al usar flujos de cambios:

* Todavía no se admiten las propiedades `operationType` y `updateDescription` en el documento de salida.
* Actualmente se admiten los tipos de operaciones `insert`, `update`y `replace`. Todavía no se admiten la operación de eliminación ni otros eventos.

Debido a estas limitaciones, se requieren la fase $match, la fase $project y la opción fullDocument, tal y como se muestra en los ejemplos anteriores.

## <a name="error-handling"></a>Control de errores

Se admiten los siguientes mensajes y códigos de error cuando se utilizan flujos de cambios:

* **Código de error HTTP 429**: Cuando se limita el flujo de cambios, devuelve una página vacía.

* **NamespaceNotFound (OperationType Invalidate)** : Si ejecuta el flujo de cambios en la colección que no existe o si se quita la colección, se devuelve un error `NamespaceNotFound`. Dado que no se puede devolver la propiedad `operationType` en el documento de salida, en lugar del error `operationType Invalidate`, se devuelve el error `NamespaceNotFound`.

## <a name="next-steps"></a>Pasos siguientes

* [Usar el período de vida para expirar automáticamente los datos en la API de Azure Cosmos DB para MongoDB](mongodb-time-to-live.md)
* [Indexación en la API de Azure Cosmos DB para MongoDB](mongodb-indexing.md)
