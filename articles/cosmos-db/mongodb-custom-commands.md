---
title: Comandos de la extensión de MongoDB para administrar los datos en la API de Azure Cosmos DB para MongoDB
description: En este artículo se describen los comandos de la extensión de MongoDB para administrar los datos almacenados en la API de Azure Cosmos DB para MongoDB.
author: SnehaGunda
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 03/26/2019
ms.author: sngun
ms.openlocfilehash: f57b274715eb1c8a4d517f5655c09c366574d412
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "75445222"
---
# <a name="use-mongodb-extension-commands-to-manage-data-stored-in-azure-cosmos-dbs-api-for-mongodb"></a>Usa comandos de la extensión de MongoDB para administrar los datos almacenados en la API de Azure Cosmos DB para MongoDB 

Azure Cosmos DB es un servicio de base de datos con varios modelos y de distribución global de Microsoft. Puede comunicarse con la API de Azure Cosmos DB para MongoDB con cualquiera de los [controladores del cliente de MongoDB](https://docs.mongodb.org/ecosystem/drivers) de código abierto. La API de Azure Cosmos DB para MongoDB permite usar los controladores de cliente existentes mediante la adhesión al [protocolo de conexión de MongoDB](https://docs.mongodb.org/manual/reference/mongodb-wire-protocol).

Mediante las API de Azure Cosmos DB para MongoDB, puede disfrutar de las ventajas de Cosmos DB, como distribución global, particionamiento automático, alta disponibilidad, garantías de latencia, cifrado en reposo automático, copias de seguridad, y muchas más, a la vez que conserva su inversión en la aplicación de MongoDB.

## <a name="mongodb-protocol-support"></a>Compatibilidad de protocolo para MongoDB

De forma predeterminada, la API de Azure Cosmos DB para MongoDB es compatible con el servidor de MongoDB versión 3.2; para más información, consulte las [características y sintaxis admitidas](mongodb-feature-support.md). Las características o los operadores de consulta que se han agregado en la versión 3.4 de MongoDB están actualmente en versión preliminar en la API de Azure Cosmos DB para MongoDB. Los siguientes comandos de extensión admiten la funcionalidad específica de Azure Cosmos DB al realizar operaciones CRUD en los datos almacenados en la API de Azure Cosmos DB para MongoDB:

* [Crear una base de datos](#create-database)
* [Actualizar una base de datos](#update-database)
* [Obtener una base de datos](#get-database)
* [Crear una colección](#create-collection)
* [Actualizar una colección](#update-collection)
* [Obtener una colección](#get-collection)

## <a name="create-database"></a><a id="create-database"></a> Crear una base de datos

El comando de extensión de creación de base de datos crea una nueva base de datos de MongoDB. El nombre de la base de datos se usa desde el contexto de la base de datos donde se ejecuta el comando. El formato del comando CreateDatabase es el siguiente:

```
{
  customAction: "CreateDatabase",
  offerThroughput: <Throughput that you want to provision on the database>
}
```

En la siguiente tabla se describen los parámetros del comando:

|**Campo**|**Tipo** |**Descripción** |
|---------|---------|---------|
| customAction   |  string  |   Nombre del comando personalizado, debe ser "CreateDatabase".      |
| offerThroughput | int  | Rendimiento aprovisionado que establece en la base de datos. Este parámetro es opcional. |

### <a name="output"></a>Output

Devuelve una respuesta predeterminada del comando personalizado. Consulte la [salida predeterminada](#default-output) del comando personalizado para los parámetros en la salida.

### <a name="examples"></a>Ejemplos

**Creación de una base de datos**

Para crear una base de datos denominada "test", use el comando siguiente:

```shell
use test
db.runCommand({customAction: "CreateDatabase"});
```

**Crear una base de datos con rendimiento**

Para crear una base de datos denominada "test" y el rendimiento aprovisionado de 1000 RU, use el siguiente comando:

```shell
use test
db.runCommand({customAction: "CreateDatabase", offerThroughput: 1000 });
```

## <a name="update-database"></a><a id="update-database"></a> Actualizar una base de datos

El comando de extensión de actualización de la base de datos actualiza las propiedades asociadas con la base de datos especificada. Actualmente, solo se puede actualizar la propiedad "offerThroughput".

```
{
  customAction: "UpdateDatabase",
  offerThroughput: <New throughput that you want to provision on the database> 
}
```

En la siguiente tabla se describen los parámetros del comando:

|**Campo**|**Tipo** |**Descripción** |
|---------|---------|---------|
| customAction    |    string     |   Nombre del comando personalizado. Debe ser "UpdateDatabase".      |
|  offerThroughput   |  int       |     Nuevo rendimiento aprovisionado que quiere establecer en la base de datos.    |

### <a name="output"></a>Output

Devuelve una respuesta predeterminada del comando personalizado. Consulte la [salida predeterminada](#default-output) del comando personalizado para los parámetros en la salida.

### <a name="examples"></a>Ejemplos

**Actualizar el rendimiento aprovisionado asociado con una base de datos**

Para actualizar el rendimiento aprovisionado de una base de datos con nombre "test" a 1200 RU, use el siguiente comando:

```shell
use test
db.runCommand({customAction: "UpdateDatabase", offerThroughput: 1200 });
```

## <a name="get-database"></a><a id="get-database"></a> Obtener una base de datos

El comando de extensión de obtención de una base de datos devuelve el objeto de base de datos. El nombre de la base de datos se usa desde el contexto de la base de datos donde se ejecuta el comando.

```
{
  customAction: "GetDatabase"
}
```

En la siguiente tabla se describen los parámetros del comando:


|**Campo**|**Tipo** |**Descripción** |
|---------|---------|---------|
|  customAction   |   string      |   Nombre del comando personalizado. Debe ser "GetDatabase"|
        
### <a name="output"></a>Output

Si el comando se ejecuta correctamente, la respuesta contiene un documento con los siguientes campos:

|**Campo**|**Tipo** |**Descripción** |
|---------|---------|---------|
|  `ok`   |   `int`     |   Estado de la respuesta. 1 == correcto. 0 == erróneo.      |
| `database`    |    `string`        |   Nombre de la base de datos.      |
|   `provisionedThroughput`  |    `int`      |    Rendimiento aprovisionado que se establece en la base de datos. Se trata de un parámetro opcional de respuesta.     |

Si no se puede ejecutar el comando, se devuelve una respuesta de comando personalizado de forma predeterminada. Consulte la [salida predeterminada](#default-output) del comando personalizado para los parámetros en la salida.

### <a name="examples"></a>Ejemplos

**Obtener la base de datos**

Para crear el objeto de base de datos para una base de datos denominada "test", use el comando siguiente:

```shell
use test
db.runCommand({customAction: "GetDatabase"});
```

## <a name="create-collection"></a><a id="create-collection"></a> Crear una colección

El comando de extensión de creación de colección crea una nueva colección de MongoDB. El nombre de la base de datos se usa desde el contexto de la base de datos donde se ejecuta el comando. El formato del comando CreateCollection es el siguiente:

```
{
  customAction: "CreateCollection",
  collection: <Collection Name>,
  offerThroughput: <Throughput that you want to provision on the collection>,
  shardKey: <Shard key path>  
}
```

En la siguiente tabla se describen los parámetros del comando:

|**Campo**|**Tipo** |**Descripción** |
|---------|---------|---------|
| customAction    | string | Nombre del comando personalizado. Debe ser "CreateCollection".     |
| collection      | string | Nombre de la colección                                   |
| offerThroughput | int    | Rendimiento aprovisionado para establecer en la base de datos. Es un parámetro opcional. |
| shardKey        | string | Ruta de acceso de la clave de partición para crear una colección con particiones. Es un parámetro opcional. |

### <a name="output"></a>Output

Devuelve una respuesta predeterminada del comando personalizado. Consulte la [salida predeterminada](#default-output) del comando personalizado para los parámetros en la salida.

### <a name="examples"></a>Ejemplos

**Crear una colección sin particiones**

Para crear una colección sin particiones con el nombre "testCollection" y el rendimiento aprovisionado de 1000 RU, use el siguiente comando: 

```shell
use test
db.runCommand({customAction: "CreateCollection", collection: "testCollection", offerThroughput: 1000});
``` 

**Crear una colección con particiones**

Para crear una colección con particiones con el nombre "testCollection" y el rendimiento aprovisionado de 1000 RU, use el siguiente comando:

```shell
use test
db.runCommand({customAction: "CreateCollection", collection: "testCollection", offerThroughput: 1000, shardKey: "a.b" });
```

## <a name="update-collection"></a><a id="update-collection"></a> Actualizar una colección

El comando de extensión de actualización de la colección actualiza las propiedades asociadas con la colección especificada.

```
{
  customAction: "UpdateCollection",
  collection: <Name of the collection that you want to update>,
  offerThroughput: <New throughput that you want to provision on the collection> 
}
```

En la siguiente tabla se describen los parámetros del comando:

|**Campo**|**Tipo** |**Descripción** |
|---------|---------|---------|
|  customAction   |   string      |   Nombre del comando personalizado. Debe ser "UpdateCollection".      |
|  collection   |   string      |   Nombre de la colección.       |
| offerThroughput   |int|   Rendimiento aprovisionado para establecer en la colección.|

## <a name="output"></a>Output

Devuelve una respuesta predeterminada del comando personalizado. Consulte la [salida predeterminada](#default-output) del comando personalizado para los parámetros en la salida.

### <a name="examples"></a>Ejemplos

**Actualizar el rendimiento aprovisionado asociado con una colección**

Para actualizar el rendimiento aprovisionado de una colección con nombre "testCollection" a 1200 RU, use el siguiente comando:

```shell
use test
db.runCommand({customAction: "UpdateCollection", collection: "testCollection", offerThroughput: 1200 });
```

## <a name="get-collection"></a><a id="get-collection"></a> Obtener una colección

El comando personalizado de obtención de una colección devuelve el objeto de colección.

```
{
  customAction: "GetCollection",
  collection: <Name of the collection>
}
```

En la siguiente tabla se describen los parámetros del comando:


|**Campo**|**Tipo** |**Descripción** |
|---------|---------|---------|
| customAction    |   string      |   Nombre del comando personalizado. Debe ser "GetCollection".      |
| collection    |    string     |    Nombre de la colección.     |

### <a name="output"></a>Output

Si el comando se ejecuta correctamente, la respuesta contiene un documento con los siguientes campos:


|**Campo**|**Tipo** |**Descripción** |
|---------|---------|---------|
|  `ok`   |    `int`     |   Estado de la respuesta. 1 == correcto. 0 == erróneo.      |
| `database`    |    `string`     |   Nombre de la base de datos.      |
| `collection`    |    `string`     |    Nombre de la colección.     |
|  `shardKeyDefinition`   |   `document`      |  Documento de especificaciones de índice que se usa como clave de partición. Se trata de un parámetro opcional de respuesta.       |
|  `provisionedThroughput`   |   `int`      |    Rendimiento aprovisionado para establecer en la colección. Se trata de un parámetro opcional de respuesta.     |

Si no se puede ejecutar el comando, se devuelve una respuesta de comando personalizado de forma predeterminada. Consulte la [salida predeterminada](#default-output) del comando personalizado para los parámetros en la salida.

### <a name="examples"></a>Ejemplos

**Obtener la colección**

Para obtener el objeto de colección para una colección denominada "testCollection", use el comando siguiente:

```shell
use test
db.runCommand({customAction: "GetCollection", collection: "testCollection"});
```

## <a name="default-output-of-a-custom-command"></a><a id="default-output"></a> Salida predeterminada de un comando personalizado

Si no se especifica, la respuesta personalizada contiene un documento con los siguientes campos:

|**Campo**|**Tipo** |**Descripción** |
|---------|---------|---------|
|  `ok`   |    `int`     |   Estado de la respuesta. 1 == correcto. 0 == erróneo.      |
| `code`    |   `int`      |   Solo se devuelve cuando el comando es erróneo (es decir, ok == 0). Contiene el código de error de MongoDB. Se trata de un parámetro opcional de respuesta.      |
|  `errMsg`   |  `string`      |    Solo se devuelve cuando el comando es erróneo (es decir, ok == 0). Contiene un mensaje de error descriptivo. Se trata de un parámetro opcional de respuesta.      |

## <a name="next-steps"></a>Pasos siguientes

Luego, puede obtener información sobre los siguientes conceptos de Azure Cosmos DB: 

* [Indexación en Azure Cosmos DB](../cosmos-db/index-policy.md)
* [Expiración automática de los datos de Azure Cosmos DB con período de vida](../cosmos-db/time-to-live.md)
