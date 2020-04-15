---
title: Indexación en la API de Azure Cosmos DB para MongoDB
description: Presenta información general sobre las funcionalidades de indexación con la API de Azure Cosmos DB para MongoDB.
ms.service: cosmos-db
ms.subservice: cosmosdb-mongo
ms.devlang: nodejs
ms.topic: conceptual
ms.date: 04/03/2020
author: timsander1
ms.author: tisande
ms.openlocfilehash: f3f369928270c77557337bfdb1037cc5174c39f2
ms.sourcegitcommit: 0450ed87a7e01bbe38b3a3aea2a21881f34f34dd
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/03/2020
ms.locfileid: "80637957"
---
# <a name="indexing-using-azure-cosmos-dbs-api-for-mongodb"></a>Indexación mediante la API de Azure Cosmos DB para MongoDB

La API de Azure Cosmos DB para MongoDB aprovecha las funcionalidades de administración automática de índices principales de Azure Cosmos DB. Este documento se centra en cómo agregar índices mediante la API de Azure Cosmos DB para MongoDB. También puede leer [información general sobre la indexación en Azure Cosmos DB](index-overview.md) pertinente para todas las API.

## <a name="indexing-for-version-36"></a>Indexación para la versión 3.6

El campo `_id` siempre se indexa de manera automática y no se puede quitar. La API de Azure Cosmos DB para MongoDB exige automáticamente la exclusividad del campo `_id` por clave de partición.

Para indexar campos adicionales, debe aplicar los comandos de administración de índices de MongoDB. Tal como ocurre en MongoDB, solo el campo `_id` se indexa de manera automática. Esta directiva de indexación predeterminada es distinta de la API SQL de Azure Cosmos DB, que indexa todos los campos de manera predeterminada.

Para aplicar una ordenación a una consulta, se debe crear un índice en los campos utilizados en la operación de ordenación.

## <a name="index-types"></a>Tipos de índice

### <a name="single-field"></a>Campo único

Puede crear índices en cualquier campo único. El criterio de ordenación del índice de campo único no es importante. El comando siguiente creará un índice en el campo `name`:

`db.coll.createIndex({name:1})`

Una consulta utilizará varios índices de campo único, siempre que estén disponibles. Puede crear hasta 500 índices de un solo campo por contenedor.

### <a name="compound-indexes-36"></a>Índices compuestos (3.6)

Los índices compuestos se admiten para las cuentas que usan el protocolo de conexión 3.6. Puede incluir hasta 8 campos en un índice compuesto. A diferencia de MongoDB, solo debe crear un índice compuesto si es necesario ordenar de manera eficaz la consulta en varios campos a la vez. En el caso de las consultas con varios filtros que no es necesario ordenar, debe crear varios índices de campo único en lugar de un único índice compuesto.

El comando siguiente creará un índice compuesto en los campos `name` y `age`:

`db.coll.createIndex({name:1,age:1})`

Los índices compuestos se pueden usar para ordenar de forma eficaz en varios campos a la vez, por ejemplo:

`db.coll.find().sort({name:1,age:1})`

El índice compuesto anterior también se puede usar para una ordenación eficaz en una consulta con el criterio de ordenación opuesto en todos los campos. Este es un ejemplo:

`db.coll.find().sort({name:-1,age:-1})`

Sin embargo, la secuencia de las rutas de acceso en el índice compuesto debe coincidir exactamente con la consulta. A continuación se muestra un ejemplo de una consulta que requeriría un índice compuesto adicional:

`db.coll.find().sort({age:1,name:1})`

### <a name="multikey-indexes"></a>Índice de varias claves

Azure Cosmos DB crea índices de varias claves para indexar el contenido almacenado en matrices. Si indexa un campo con un valor de matriz, Azure Cosmos DB indexa automáticamente todos los elementos de la matriz.

### <a name="geospatial-indexes"></a>Índices geoespaciales

Muchos operadores geoespaciales se beneficiarán de los índices geoespaciales. Actualmente, la API de Azure Cosmos DB para MongoDB admite índices `2dsphere`. Todavía no se admiten los índices `2d`.

A continuación se muestra un ejemplo para crear un índice geoespacial en el campo `location`:

`db.coll.createIndex({ location : "2dsphere" })`

### <a name="text-indexes"></a>Índices de texto

No se admiten actualmente los índices de texto. En el caso de las consultas de búsqueda de texto en cadenas, debe usar la integración de [Azure Cognitive Search](https://docs.microsoft.com/azure/search/search-howto-index-cosmosdb) con Azure Cosmos DB.

## <a name="index-properties"></a>Propiedades de índice

Las siguientes operaciones son comunes tanto para las cuentas que atienden la versión 3.6 del protocolo de conexión como para las que atienden a las versiones anteriores. También puede obtener más información sobre los [índices compatibles y las propiedades indexadas](mongodb-feature-support-36.md#indexes-and-index-properties).

### <a name="unique-indexes"></a>Índices únicos

Los [índices únicos](unique-keys.md) son útiles para exigir que dos o más documentos no contengan el mismo valor para los campos indexados.

>[!Important]
> Se pueden crear índices únicos solo cuando la colección está vacía (no contiene documentos).

El comando siguiente crea un índice único en el campo "student_id":

```shell
globaldb:PRIMARY> db.coll.createIndex( { "student_id" : 1 }, {unique:true} )
{
        "_t" : "CreateIndexesResponse",
        "ok" : 1,
        "createdCollectionAutomatically" : false,
        "numIndexesBefore" : 1,
        "numIndexesAfter" : 4
}
```

Para colecciones particionadas, la creación de un índice único requiere proporcionar la clave de partición. En otras palabras, todos los índices únicos de una colección particionada son índices compuestos donde uno de los campos es la clave de partición.

Los siguientes comandos crean una colección particionada ```coll``` (la clave de partición es ```university```) con un índice único en los campos student_id y university:

```shell
globaldb:PRIMARY> db.runCommand({shardCollection: db.coll._fullName, key: { university: "hashed"}});
{
        "_t" : "ShardCollectionResponse",
        "ok" : 1,
        "collectionsharded" : "test.coll"
}
globaldb:PRIMARY> db.coll.createIndex( { "student_id" : 1, "university" : 1 }, {unique:true})
{
        "_t" : "CreateIndexesResponse",
        "ok" : 1,
        "createdCollectionAutomatically" : false,
        "numIndexesBefore" : 3,
        "numIndexesAfter" : 4
}
```

En el ejemplo anterior, si se omite la cláusula ```"university":1```, se devolvería un error con el mensaje siguiente:

```"cannot create unique index over {student_id : 1.0} with shard key pattern { university : 1.0 }"```

### <a name="ttl-indexes"></a>Índices TTL

Para habilitar la caducidad de documentos en una colección determinada, se debe crear un ["Índice TTL" (índice de período de vida)](../cosmos-db/time-to-live.md). Un índice TTL es un índice en el campo _ts con un valor de "expireAfterSeconds".

Ejemplo:

```JavaScript
globaldb:PRIMARY> db.coll.createIndex({"_ts":1}, {expireAfterSeconds: 10})
```

El comando anterior provocará la eliminación de todos los documentos de la colección ```db.coll``` que no se han modificado en los últimos 10 segundos.

> [!NOTE]
> **_ts** es un campo específico de Azure Cosmos DB y no es accesible desde los clientes de MongoDB. Es una propiedad reservada (sistema) que contiene la marca de tiempo de la última modificación del documento.

## <a name="track-the-index-progress"></a>Seguimiento del progreso de indexación

La versión 3.6 de la API de Azure Cosmos DB para las cuentas de MongoDB admite el comando `currentOp()` para realizar un seguimiento del progreso de la indexación en una instancia de base de datos. Este comando devuelve un documento que contiene información sobre las operaciones en curso en una instancia de base de datos. El comando `currentOp` se usa para realizar un seguimiento de todas las operaciones en curso en el sistema MongoDB nativo, mientras que, en la API de Azure Cosmos DB para MongoDB, este comando solo admite el seguimiento de la operación de índice.

A continuación, se incluyen algunos ejemplos que muestran cómo usar el comando `currentOp` para realizar un seguimiento del progreso del índice:

* Obtener el progreso del índice de una colección:

   ```shell
   db.currentOp({"command.createIndexes": <collectionName>, "command.$db": <databaseName>})
   ```

* Obtener el progreso del índice de todas las colecciones de una base de datos:

  ```shell
  db.currentOp({"command.$db": <databaseName>})
  ```

* Obtener el progreso del índice de todas las bases de datos y colecciones de una cuenta de Azure Cosmos:

  ```shell
  db.currentOp({"command.createIndexes": { $exists : true } })
  ```

Los detalles del progreso del índice contienen el porcentaje del progreso de la operación de índice actual. En el ejemplo siguiente se muestra el formato del documento de salida de las diferentes fases de progreso del índice:

1. Si la operación de índice de una colección "foo" y una base de datos "bar" tiene un 60 % de la indexación completada, tendrá el siguiente documento de salida. `Inprog[0].progress.total` muestra 100 como finalización de destino.

   ```json
   {
        "inprog" : [
        {
                ………………...
                "command" : {
                        "createIndexes" : foo
                        "indexes" :[ ],
                        "$db" : bar
                },
                "msg" : "Index Build (background) Index Build (background): 60 %",
                "progress" : {
                        "done" : 60,
                        "total" : 100
                },
                …………..…..
        }
        ],
        "ok" : 1
   }
   ```

2. En el caso de una operación de índice que se acaba de iniciar en una colección "foo" y una base de datos "bar", el documento de salida puede mostrar un 0 % de progreso hasta que llegue a un nivel medible.

   ```json
   {
        "inprog" : [
        {
                ………………...
                "command" : {
                        "createIndexes" : foo
                        "indexes" :[ ],
                        "$db" : bar
                },
                "msg" : "Index Build (background) Index Build (background): 0 %",
                "progress" : {
                        "done" : 0,
                        "total" : 100
                },
                …………..…..
        }
        ],
       "ok" : 1
   }
   ```

3. Cuando se completa la operación de índice en curso, el documento de salida muestra las operaciones en curso vacías.

   ```json
   {
      "inprog" : [],
      "ok" : 1
   }
   ```

### <a name="background-index-updates"></a>Actualizaciones de índices en segundo plano

Independientemente del valor especificado para la propiedad **Background** del índice, las actualizaciones de los índices siempre se hacen en segundo plano. Las actualizaciones de índice consumen RU con una prioridad más baja que otras operaciones de base de datos. Por lo tanto, los cambios de índice no generarán ningún tiempo de inactividad para las operaciones de escritura, actualización o eliminación.

Al agregar un índice nuevo, las consultas lo utilizarán de inmediato. Esto significa que es posible que las consultas no devuelvan todos los resultados coincidentes y que lo harán sin devolver ningún error. Una vez completada la transformación de índice, los resultados de la consulta serán coherentes. Puede [hacer un seguimiento del progreso del índice](#track-the-index-progress).

## <a name="migrating-collections-with-indexes"></a>Migración de colecciones con índices

Actualmente, solo se pueden crear índices únicos cuando la colección no contiene documentos. Las herramientas de migración de MongoDB más populares intentan crear los índices únicos después de importar los datos. Para prevenir este problema, se recomienda que los usuarios creen manualmente las colecciones y los índices únicos correspondientes en lugar de permitir que lo haga la herramienta de migración (con ```mongorestore```, este comportamiento se logra mediante la marca `--noIndexRestore` en la línea de comandos).

## <a name="indexing-for-version-32"></a>Indexación para la versión 3.2

En el caso de las cuentas de Azure Cosmos DB que son compatibles con la versión 3.2 del protocolo de conexión de MongoDB, las características de indexación disponibles y los valores predeterminados son diferentes. Puede [comprobar la versión de la cuenta](mongodb-feature-support-36.md#protocol-support). Para actualizar a la versión 3.6, presente una [solicitud de soporte técnico](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade).

Si usa la versión 3.2, en esta sección se describen las diferencias principales que tiene con la versión 3.6.

### <a name="dropping-the-default-indexes-32"></a>Eliminación de los índices predeterminados (3.2)

A diferencia de la versión 3.6 de la API de Azure Cosmos DB para MongoDB, la versión 3.2 indexa todas las propiedades de manera predeterminada. El comando siguiente se puede utilizar para eliminar estos índices predeterminados de una colección ```coll```:

```JavaScript
> db.coll.dropIndexes()
{ "_t" : "DropIndexesResponse", "ok" : 1, "nIndexesWas" : 3 }
```

Después de colocar los índices predeterminados, puede agregar los índices adicionales, tal como se hace en la versión 3.6.

### <a name="compound-indexes-32"></a>Índices compuestos (3.2)

Los índices compuestos contienen referencias a varios campos de un documento. Si quiere crear un índice compuesto, actualice a la versión 3.6 mediante la presentación de una [solicitud de soporte técnico](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade).

## <a name="next-steps"></a>Pasos siguientes

* [Indexación en Azure Cosmos DB](../cosmos-db/index-policy.md)
* [Expiración automática de los datos de Azure Cosmos DB con período de vida](../cosmos-db/time-to-live.md)
