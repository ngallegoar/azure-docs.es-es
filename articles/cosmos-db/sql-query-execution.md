---
title: Ejecución de consultas SQL en Azure Cosmos DB
description: Obtenga información sobre cómo crear una consulta SQL y ejecutarla en Azure Cosmos DB. En este artículo se describe cómo crear y ejecutar una consulta SQL con la API de REST, el SDK de .Net, el SDK de JavaScript y otros varios SDK.
author: timsander1
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 12/02/2019
ms.author: tisande
ms.openlocfilehash: 70eb81b6d13c57a7ebc131244c7aa318cb2b2fd4
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "74871268"
---
# <a name="azure-cosmos-db-sql-query-execution"></a>Ejecución de consultas SQL en Azure Cosmos DB

Cualquier lenguaje capaz de hacer solicitudes HTTP/HTTPS puede llamar a la API REST de Cosmos DB. Cosmos DB también ofrece bibliotecas de programación para los lenguajes de programación .NET, Node.js, JavaScript y Python. La API REST y las bibliotecas admiten consultas con SQL, y el SDK de .NET también admite [consultas LINQ](sql-query-linq-to-sql.md).

En los ejemplos siguientes se muestra cómo crear una consulta y enviarla a una cuenta de la base de datos de Cosmos.

## <a name="rest-api"></a><a id="REST-API"></a>API DE REST

Cosmos DB ofrece un modelo de programación RESTful sobre HTTP. El modelo de recursos consta de un conjunto de recursos en una cuenta de base de datos, que una suscripción a Azure aprovisiona. La cuenta de base de datos consta de un conjunto de *bases de datos*, cada una de las cuales puede contener varios *contenedores* que, a su vez, contienen *elementos*, funciones definidas por el usuario y otros tipos de recursos. Cada recurso de Cosmos DB es direccionable mediante un URI lógico y estable. Un conjunto de recursos se denomina *fuente*. 

El modelo de interacción básico con estos recursos se lleva a cabo con los verbos HTTP `GET`, `PUT`, `POST` y `DELETE`, con sus interpretaciones estándar. Use `POST` para crear un nuevo recurso, ejecutar un procedimiento almacenado o emitir una consulta de Cosmos DB. Las consultas siempre son operaciones de solo lectura sin efectos secundarios.

Los ejemplos siguientes muestran un `POST` para una consulta de la API de SQL en los elementos de ejemplo. La consulta tiene un filtro sencillo por la propiedad `name` de JSON. Los encabezados `x-ms-documentdb-isquery` y Content-Type: `application/query+json` denotan que la operación es una consulta. Reemplace `mysqlapicosmosdb.documents.azure.com:443` con el URI de la cuenta de Cosmos DB.

```json
    POST https://mysqlapicosmosdb.documents.azure.com:443/docs HTTP/1.1
    ...
    x-ms-documentdb-isquery: True
    Content-Type: application/query+json

    {
        "query": "SELECT * FROM Families f WHERE f.id = @familyId",
        "parameters": [
            {"name": "@familyId", "value": "AndersenFamily"}
        ]
    }
```

Los resultados son:

```json
    HTTP/1.1 200 Ok
    x-ms-activity-id: 8b4678fa-a947-47d3-8dd3-549a40da6eed
    x-ms-item-count: 1
    x-ms-request-charge: 0.32

    {  
       "_rid":"u1NXANcKogE=",
       "Documents":[  
          {  
             "id":"AndersenFamily",
             "lastName":"Andersen",
             "parents":[  
                {  
                   "firstName":"Thomas"
                },
                {  
                   "firstName":"Mary Kay"
                }
             ],
             "children":[  
                {  
                   "firstName":"Henriette Thaulow",
                   "gender":"female",
                   "grade":5,
                   "pets":[  
                      {  
                         "givenName":"Fluffy"
                      }
                   ]
                }
             ],
             "address":{  
                "state":"WA",
                "county":"King",
                "city":"Seattle"
             },
             "_rid":"u1NXANcKogEcAAAAAAAAAA==",
             "_ts":1407691744,
             "_self":"dbs\/u1NXAA==\/colls\/u1NXANcKogE=\/docs\/u1NXANcKogEcAAAAAAAAAA==\/",
             "_etag":"00002b00-0000-0000-0000-53e7abe00000",
             "_attachments":"_attachments\/"
          }
       ],
       "count":1
    }
```

La siguiente consulta más compleja devuelve varios resultados de una combinación:

```json
    POST https://https://mysqlapicosmosdb.documents.azure.com:443/docs HTTP/1.1
    ...
    x-ms-documentdb-isquery: True
    Content-Type: application/query+json

    {
        "query": "SELECT
                     f.id AS familyName,
                     c.givenName AS childGivenName,
                     c.firstName AS childFirstName,
                     p.givenName AS petName
                  FROM Families f
                  JOIN c IN f.children
                  JOIN p in c.pets",
        "parameters": []
    }
```

Los resultados son: 

```json
    HTTP/1.1 200 Ok
    x-ms-activity-id: 568f34e3-5695-44d3-9b7d-62f8b83e509d
    x-ms-item-count: 1
    x-ms-request-charge: 7.84

    {  
       "_rid":"u1NXANcKogE=",
       "Documents":[  
          {  
             "familyName":"AndersenFamily",
             "childFirstName":"Henriette Thaulow",
             "petName":"Fluffy"
          },
          {  
             "familyName":"WakefieldFamily",
             "childGivenName":"Jesse",
             "petName":"Goofy"
          },
          {  
             "familyName":"WakefieldFamily",
             "childGivenName":"Jesse",
             "petName":"Shadow"
          }
       ],
       "count":3
    }
```

Si los resultados de una consulta no caben en una sola página, la API REST devuelve un token de continuación con el encabezado de respuesta `x-ms-continuation-token`. Los clientes pueden paginar los resultados incluyendo el encabezado en los resultados posteriores. También puede controlar el número de resultados por página con el encabezado numérico `x-ms-max-item-count`.

Si una consulta tiene una función de agregación como COUNT, la página de consulta puede devolver un valor parcialmente agregado sobre una sola página de resultados. Los clientes deben realizar una agregación de segundo nivel con estos resultados para generar los resultados finales. Por ejemplo, sume los recuentos devueltos en las páginas individuales para devolver el recuento total.

Para administrar la directiva de coherencia de datos para las consultas, use el encabezado `x-ms-consistency-level` como en todas las solicitudes de la API REST. La coherencia de sesión también requiere devolver el último encabezado de cookie `x-ms-session-token` en la solicitud de la consulta. La directiva de indexación del contenedor consultado también puede afectar a la coherencia de los resultados de la consulta. En el caso de los contenedores, con la configuración de la directiva de indexación predeterminada, el índice siempre está actualizado con el contenido del elemento y los resultados de la consulta coinciden con la coherencia elegida para los datos. Para más información, consulte [Niveles de coherencia de Azure Cosmos DB][consistency-levels].

Si la directiva de indexación configurada en el contenedor no puede admitir la consulta especificada, el servidor de Azure Cosmos DB devuelve el error 400 "Solicitud incorrecta". Este mensaje de error se devuelve para las consultas donde las rutas de acceso se han excluido explícitamente de la indexación. Puede especificar el encabezado `x-ms-documentdb-query-enable-scan` para permitir que la consulta realice un examen si algún índice no está disponible.

Puede obtener métricas detalladas sobre la ejecución de consultas si establece el encabezado `x-ms-documentdb-populatequerymetrics` en `true`. Para más información, vea [SQL query metrics for Azure Cosmos DB](sql-api-query-metrics.md) (Métricas de consulta de SQL para la API de Azure Cosmos DB).

## <a name="c-net-sdk"></a>C# (SDK de .NET)

El SDK de .NET admite la realización de consultas de LINQ y SQL. En el ejemplo siguiente se muestra cómo realizar la consulta de filtro anterior con .NET:

```csharp
    foreach (var family in client.CreateDocumentQuery(containerLink,
        "SELECT * FROM Families f WHERE f.id = \"AndersenFamily\""))
    {
        Console.WriteLine("\tRead {0} from SQL", family);
    }

    SqlQuerySpec query = new SqlQuerySpec("SELECT * FROM Families f WHERE f.id = @familyId");
    query.Parameters = new SqlParameterCollection();
    query.Parameters.Add(new SqlParameter("@familyId", "AndersenFamily"));

    foreach (var family in client.CreateDocumentQuery(containerLink, query))
    {
        Console.WriteLine("\tRead {0} from parameterized SQL", family);
    }

    foreach (var family in (
        from f in client.CreateDocumentQuery(containerLink)
        where f.Id == "AndersenFamily"
        select f))
    {
        Console.WriteLine("\tRead {0} from LINQ query", family);
    }

    foreach (var family in client.CreateDocumentQuery(containerLink)
        .Where(f => f.Id == "AndersenFamily")
        .Select(f => f))
    {
        Console.WriteLine("\tRead {0} from LINQ lambda", family);
    }
```

En el ejemplo siguiente se compara la igualdad de dos propiedades en cada elemento y se usan proyecciones anónimas.

```csharp
    foreach (var family in client.CreateDocumentQuery(containerLink,
        @"SELECT {""Name"": f.id, ""City"":f.address.city} AS Family
        FROM Families f
        WHERE f.address.city = f.address.state"))
    {
        Console.WriteLine("\tRead {0} from SQL", family);
    }

    foreach (var family in (
        from f in client.CreateDocumentQuery<Family>(containerLink)
        where f.address.city == f.address.state
        select new { Name = f.Id, City = f.address.city }))
    {
        Console.WriteLine("\tRead {0} from LINQ query", family);
    }

    foreach (var family in
        client.CreateDocumentQuery<Family>(containerLink)
        .Where(f => f.address.city == f.address.state)
        .Select(f => new { Name = f.Id, City = f.address.city }))
    {
        Console.WriteLine("\tRead {0} from LINQ lambda", family);
    }
```

En el ejemplo siguiente se muestran combinaciones, expresadas con `SelectMany` de LINQ.

```csharp
    foreach (var pet in client.CreateDocumentQuery(containerLink,
          @"SELECT p
            FROM Families f
                 JOIN c IN f.children
                 JOIN p in c.pets
            WHERE p.givenName = ""Shadow"""))
    {
        Console.WriteLine("\tRead {0} from SQL", pet);
    }

    // Equivalent in Lambda expressions:
    foreach (var pet in
        client.CreateDocumentQuery<Family>(containerLink)
        .SelectMany(f => f.children)
        .SelectMany(c => c.pets)
        .Where(p => p.givenName == "Shadow"))
    {
        Console.WriteLine("\tRead {0} from LINQ lambda", pet);
    }
```

El cliente de .NET itera automáticamente en todas las páginas de resultados de la consulta en los bloques `foreach`, como se muestra en el ejemplo anterior. Las opciones de consulta especificadas en la sección de la [API REST](#REST-API) también están disponibles en el SDK de .NET mediante las clases `FeedOptions` y `FeedResponse` del método `CreateDocumentQuery`. Puede controlar el número de páginas con el valor `MaxItemCount`.

Puede controlar expresamente la paginación creando `IDocumentQueryable` mediante el objeto `IQueryable`; después, leyendo los valores `ResponseContinuationToken` y devolviéndolos como `RequestContinuationToken` en `FeedOptions`. Puede establecer `EnableScanInQuery` para habilitar los exámenes cuando la directiva de indexación configurada no admita la consulta. En el caso de los contenedores con particiones, puede usar `PartitionKey` para ejecutar la consulta en una sola partición, aunque Azure Cosmos DB puede extraer este valor automáticamente del texto de consulta. Puede usar `EnableCrossPartitionQuery` para ejecutar consultas en varias particiones.

Para obtener más ejemplos de .NET con consultas, consulte los [ejemplos de .NET de Azure Cosmos DB](https://github.com/Azure/azure-cosmos-dotnet-v3) en GitHub.

## <a name="javascript-server-side-api"></a><a id="JavaScript-server-side-API"></a>API del servidor de JavaScript

Azure Cosmos DB proporciona un modelo de programación para [ejecutar la lógica de aplicación basada en JavaScript](stored-procedures-triggers-udfs.md) directamente en contenedores mediante desencadenadores y procedimientos almacenados. La lógica de JavaScript registrada en los contenedores puede emitir operaciones de base de datos en los elementos del contenedor especificado, encapsulados en transacciones ACID ambientales.

En el ejemplo siguiente se muestra cómo usar `queryDocuments` en la API del servidor de JavaScript para hacer consultas a partir de desencadenadores y procedimientos almacenados:

```javascript
    function findName(givenName, familyName) {
        var context = getContext();
        var containerManager = context.getCollection();
        var containerLink = containerManager.getSelfLink()

        // create a new item.
        containerManager.createDocument(containerLink,
            { givenName: givenName, familyName: familyName },
            function (err, documentCreated) {
                if (err) throw new Error(err.message);

                // filter items by familyName
                var filterQuery = "SELECT * from root r WHERE r.familyName = 'Wakefield'";
                containerManager.queryDocuments(containerLink,
                    filterQuery,
                    function (err, matchingDocuments) {
                        if (err) throw new Error(err.message);
    context.getResponse().setBody(matchingDocuments.length);

                        // Replace the familyName for all items that satisfied the query.
                        for (var i = 0; i < matchingDocuments.length; i++) {
                            matchingDocuments[i].familyName = "Robin Wakefield";
                            // we don't need to execute a callback because they are in parallel
                            containerManager.replaceDocument(matchingDocuments[i]._self,
                                matchingDocuments[i]);
                        }
                    })
            });
    }
```

## <a name="next-steps"></a>Pasos siguientes

- [Introducción a Azure Cosmos DB](introduction.md)
- [Ejemplos de .NET de Azure Cosmos DB](https://github.com/Azure/azure-cosmos-dotnet-v3)
- [Niveles de coherencia de Azure Cosmos DB](consistency-levels.md)
