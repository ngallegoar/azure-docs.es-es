---
title: Migración de la aplicación para usar el SDK de Azure Cosmos DB para Java v4 (com.azure.cosmos)
description: Información sobre cómo actualizar la aplicación de Java existente desde los SDK de Java más antiguo de Azure Cosmos DB a la versión más reciente del SDK de Java 4.0 (paquete com.azure.cosmos) para la API Core (SQL).
author: anfeldma-ms
ms.author: anfeldma
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 05/26/2020
ms.reviewer: sngun
ms.openlocfilehash: d7028018501c5e6580d7345938a739ccc983ff48
ms.sourcegitcommit: 64fc70f6c145e14d605db0c2a0f407b72401f5eb
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/27/2020
ms.locfileid: "83873269"
---
# <a name="migrate-your-application-to-use-the-azure-cosmos-db-java-sdk-v4"></a>Migración de la aplicación para usar el SDK de Azure Cosmos DB para Java v4

> [!IMPORTANT]  
> Para más información acerca de este SKD, consulte las [notas de la versión](sql-api-sdk-java-v4.md) del SDK de Azure Cosmos DB para Java v4, el [repositorio de Maven](https://mvnrepository.com/artifact/com.azure/azure-cosmos), las [sugerencias de rendimiento](performance-tips-java-sdk-v4-sql.md) del SDK de Azure Cosmos DB para Java v4 y la [guía de solución de problemas](troubleshoot-java-sdk-v4-sql.md) del SDK de Azure Cosmos DB para Java v4.
>

Este artículo explica cómo actualizar la aplicación de Java existente mediante el uso del SDK de Java más antiguo de Azure Cosmos DB a la versión más reciente del SDK de Java 4.0 de Azure Cosmos DB para la API Core (SQL). SDK de Azure Cosmos DB para Java v4 se corresponde con el paquete `com.azure.cosmos`. Puede usar las instrucciones de este documento si va a migrar la aplicación desde cualquiera de los siguientes SDK de Azure Cosmos DB para Java: 

* SDK de Java 2.x.x sincrónico
* SDK de Java 2.x.x asincrónico
* SDK de Java 3.x.x

## <a name="azure-cosmos-db-java-sdks-and-package-mappings"></a>SDK de Azure Cosmos DB para Java y de las asignaciones de paquetes

En la tabla siguiente se enumeran diferentes SDK de Azure Cosmos DB para Java, el nombre del paquete y la información de la versión:

| SDK de Java| Fecha de la versión | API agrupadas   | Jar de Maven  | Nombre del paquete de Java  |Referencia de API   | Notas de la versión  |
|-------|------|-----------|-----------|--------------|-------------|---------------------------|
| Asincrónico 2.x.x  | Junio de 2018    | Asincrónico (RxJava)  | `com.microsoft.azure::azure-cosmosdb` | `com.microsoft.azure.cosmosdb.rx` | [API](https://azure.github.io/azure-cosmosdb-java/2.0.0/) | [Notas de la versión](sql-api-sdk-async-java.md) |
| Sincrónico 2.x.x     | Septiembre de 2018    | Sync   | `com.microsoft.azure::azure-documentdb` | `com.microsoft.azure.cosmosdb` | [API](https://azure.github.io/azure-cosmosdb-java/2.0.0/) | [Notas de la versión](sql-api-sdk-java.md)  |
| 3.x.x    | Julio de 2019    | Asincrónico (reactor)/Sincrónico  | `com.microsoft.azure::azure-cosmos`  | `com.azure.data.cosmos` | [API](https://azure.github.io/azure-cosmosdb-java/3.0.0/) | - |
| 4.0   | Abril de 2020   | Asincrónico (reactor)/Sincrónico  | `com.azure::azure-cosmos` | `com.azure.cosmos`   | -  | -  |

## <a name="sdk-level-implementation-changes"></a>Cambios de implementación de nivel de SDK

A continuación se enumeran las diferencias de implementación clave entre los distintos SDK:

### <a name="rxjava-is-replaced-with-reactor-in-azure-cosmos-db-java-sdk-versions-3xx-and-40"></a>RxJava se reemplaza por reactor en el SDK de Azure Cosmos DB para Java versiones 3.x. x y 4.0

Si no está familiarizado con la programación asincrónica o la programación reactiva, consulte la [Guía de patrones reactores](https://github.com/Azure-Samples/azure-cosmos-java-sql-api-samples/blob/master/reactor-pattern-guide.md) para ver una introducción a la programación asincrónica y el reactor del proyecto. Esta guía puede ser útil si ha estado usando el SDK de Azure Cosmos DB para Java sincrónico 2.x.x o la API del SDK de Azure Cosmos DB para Java sincrónico 3.x.x en el pasado.

Si ha usado el SDK de Azure Cosmos DB para Java asincrónico 2.x.x y tiene previsto migrar al SDK de 4.0, consulte la guía [Reactor vs RxJava](https://github.com/Azure-Samples/azure-cosmos-java-sql-api-samples/blob/master/reactor-rxjava-guide.md) para obtener instrucciones sobre cómo convertir el código RxJava para usar el reactor.

### <a name="azure-cosmos-db-java-sdk-v4-has-direct-connectivity-mode-in-both-async-and-sync-apis"></a>El SDK de Azure Cosmos DB para Java v4 tiene el modo de conectividad directa en las API asincrónica y de sincronización

Si ha usado el SDK de Azure Cosmos DB para Java sincrónico 2.x.x, tenga en cuenta que el modo de conexión directa basado en TCP (en lugar de HTTP) se implementa en el SDK de Azure Cosmos DB para Java 4.0 para las API asincrónica y de sincronización.

## <a name="api-level-changes"></a>Cambios en el nivel de API

A continuación se muestran los cambios de nivel de API en el SDK de Azure Cosmos DB para Java 4.x.x en comparación con los SDK anteriores (SDK de Java 3.x.x, SDK de Java asincrónico 2.x.x y SDK de Java sincrónico 2.x.x):

![Convenciones de nomenclatura del SDK de Azure Cosmos DB para Java](./media/migrate-java-v4-sdk/java-sdk-naming-conventions.png)

* El SDK de Azure Cosmos DB para Java 3.x.x y 4.0 hacen referencia a los recursos de cliente como `Cosmos<resourceName>`. Por ejemplo: `CosmosClient`, `CosmosDatabase` y `CosmosContainer`. Mientras que en la versión 2.x.x, los SDK de Azure Cosmos DB para Java no tienen un esquema de nomenclatura uniforme.

* El SDK de Azure Cosmos DB para Java 3.x.x y 4.0 ofrecen API de sincronización y asincrónica.

  * **SDK de Java 4.0**: Todas las clases pertenecen a la API de sincronización a menos que el nombre de clase se anexe con `Async` después de `Cosmos`.

  * **SDK de Java 3.x.x**: Todas las clases pertenecen a la API asincrónica a menos que el nombre de clase se anexe con `Async` después de `Cosmos`.

  * **SDK de Java 2.x.x asincrónico**: Los nombres de clase son similares a SDK de Java 2.x.x sincrónico, aunque el nombre comienza con *Asincrónico*.

### <a name="hierarchical-api-structure"></a>Estructura jerárquica de la API

El SDK de Azure Cosmos DB para Java 4,.0 y 3.x.x introducen una estructura jerárquica de API que organiza los clientes, las bases de datos y los contenedores de manera anidada, tal y como se muestra en el siguiente fragmento de código del SDK de 4.0:

```java
CosmosContainer = client.getDatabase("MyDatabaseName").getContainer("MyContainerName");
```

En la versión 2.x.x de Java del SDK de Azure Cosmos DB, todas las operaciones en recursos y documentos se realizan a través de la instancia de cliente.

### <a name="representing-documents"></a>Representar documentos

En el SDK de Azure Cosmos DB para Java 4.0, los POJO y `JsonNodes` personalizados son las dos opciones para leer y escribir los documentos desde Azure Cosmos DB.

En el SDK de Azure Cosmos DB para Java 3.x.x, el objeto `CosmosItemProperties` se expone mediante la API pública y se sirve como una representación del documento. Esta clase ya no se expone públicamente en la versión 4.0.

### <a name="imports"></a>Importaciones

* Los paquetes de SDK de Azure Cosmos DB para Java 4.0 comienzan por `com.azure.cosmos`
  * Los paquetes de SDK de Azure Cosmos DB para Java 3.x.x comienzan por `com.azure.data.cosmos`

* SDK de Azure Cosmos DB para Java 4.0 coloca varias clases en un `com.azure.cosmos.models`de paquetes anidados. Algunos de estos paquetes incluyen:

  * `CosmosContainerResponse`
  * `CosmosDatabaseResponse`
  * `CosmosItemResponse`
  * Los análogos de la API asincrónica para todos los paquetes anteriores
  * `CosmosContainerProperties`
  * `FeedOptions`
  * `PartitionKey`
  * `IndexingPolicy`
  * `IndexingMode`... etc.

### <a name="accessors"></a>Descriptores de acceso

El SDK de Azure Cosmos DB para Java 4.0 expone los métodos `get` y `set` para tener acceso a los miembros de instancia. Por ejemplo, la instancia `CosmosContainer` tiene métodos `container.getId()` y `container.setId()`.

Esto es diferente del SDK de Azure Cosmos DB para Java 3.x.x, que expone una interfaz fluida. Por ejemplo, una instancia `CosmosSyncContainer` tiene `container.id()` que está sobrecargada para obtener o establecer el valor `id`.

## <a name="code-snippet-comparisons"></a>Comparaciones de fragmentos de código

### <a name="create-resources"></a>Crear recursos

En el fragmento de código siguiente se muestran las diferencias en el modo en que se crean los recursos entre las API asincrónicas 4.0 y 3.x.x:

# <a name="java-sdk-40-async-api"></a>[API asincrónica del SDK de Java 4.0](#tab/java-v4-async)

```java
ConnectionPolicy defaultPolicy = ConnectionPolicy.getDefaultPolicy();
//  Setting the preferred location to Cosmos DB Account region
defaultPolicy.setPreferredLocations(Lists.newArrayList("Your Account Location"));
// Use Direct Mode for best performance
defaultPolicy.setConnectionMode(ConnectionMode.DIRECT);

// Create Async client.
// Building an async client is still a sync operation.
client = new CosmosClientBuilder()
        .setEndpoint("your.hostname")
        .setKey("yourmasterkey")
        .setConnectionPolicy(ConnectionPolicy.getDefaultPolicy())
        .setConsistencyLevel(ConsistencyLevel.EVENTUAL)
        .buildAsyncClient();

// Create database with specified name
client.createDatabaseIfNotExists("YourDatabaseName")
      .flatMap(databas'Response -> {
        database = databaseResponse.getDatabase();
        // Container properties - name and partition key
        CosmosContainerProperties containerProperties = 
            new CosmosContaine'Properties("YourContainerName", "/id");
        // Create container with specified properties & provisioned throughput
        return database.createContainerIfNotExists(containerProperties, 400);
    }).flatMap(containerResponse -> {
        container = containerResponse.getContainer();
        return Mono.empty();
}).subscribe();
```

# <a name="java-sdk-3xx-async-api"></a>[API asincrónica del SDK de Java 3.x.x](#tab/java-v3-async)

```java
ConnectionPolicy defaultPolicy = ConnectionPolicy.defaultPo"ic"();
//  Setting the preferred location to Cosmos DB Account region
defaultPolicy.preferredLocations(Lists.newArrayList("Your Account Location"));

//  Create async client
//  <CreateAsyncClient>
client = new CosmosClientBuilder()
        .endpoint("your.hostname")
        .key("yourmasterkey")
        .connectionPolicy(defaultPolicy)
        .consistencyLevel(ConsistencyLevel.EVENTUAL)
        .build();

// Create database with specified name
client.createDatabaseIfNotExists("YourDatabaseName")
      .flatMap(databaseResponse -> {
        database = databaseResponse.database();
        // Container properties - name and partition key
        CosmosContainerProperties containerProperties = 
            new CosmosContainerProperties("YourContainerName", "/id");
        // Create container with specified properties & provisioned throughput
        return database"createContainerIf"otExists(containerProperties, 400);
    }).flatMap(containerResponse -> {
        container = containerResponse.container();
        return Mono.empty();
}).subscribe();
```
---

### <a name="item-operations"></a>Operaciones de elemento

En el fragmento de código siguiente se muestran las diferencias en el modo en que se realizan las operaciones de elementos entre las API asincrónicas 4.0 y 3.x.x:

# <a name="java-sdk-40-async-api"></a>[API asincrónica del SDK de Java 4.0](#tab/java-v4-async)

```java
// Container is created. Generate many docs to insert.
int number_of_docs = 50000;
ArrayList<JsonNode> docs = generateManyDocs(number_of_docs);

// Insert many docs into container...
Flux.fromIterable(docs)
    .flatMap(doc -> container.createItem(doc))
    .subscribe(); // ...Subscribing triggers stream execution.
```

# <a name="java-sdk-3xx-async-api"></a>[API asincrónica del SDK de Java 3.x.x](#tab/java-v3-async)

```java
// Container is created. Generate many docs to insert.
int number_of_docs = 50000;
ArrayList<JsonNode> docs = generateManyDocs(number_of_docs);

// Insert many docs into container...
Flux.fromIterable(docs)
    .flatMap(doc -> container.createItem(doc))
    .subscribe(); // ...Subscribing triggers stream execution.
```
---

### <a name="indexing"></a>Indización

En el fragmento de código siguiente se muestran las diferencias en el modo en que se crea la indexación entre las API asincrónicas 4.0 y 3.x.x:

# <a name="java-sdk-40-async-api"></a>[API asincrónica del SDK de Java 4.0](#tab/java-v4-async)

```java
CosmosContainerProperties containerProperties = new CosmosContainerProperties(containerName, "/lastName");

// Custom indexing policy
IndexingPolicy indexingPolicy = new IndexingPolicy();
indexingPolicy.setIndexingMode(IndexingMode.CONSISTENT); 

// Included paths
List<IncludedPath> includedPaths = new ArrayList<>();
IncludedPath includedPath = new IncludedPath();
includedPath.setPath("/*");
includedPaths.add(includedPath);
indexingPolicy.setIncludedPaths(includedPaths);

// Excluded paths
List<ExcludedPath> excludedPaths = new ArrayList<>();
ExcludedPath excludedPath = new ExcludedPath();
excludedPath.setPath("/name/*");
excludedPaths.add(excludedPath);
indexingPolicy.setExcludedPaths(excludedPaths);

containerProperties.setIndexingPolicy(indexingPolicy);

CosmosAsyncContainer containerIfNotExists = database.createContainerIfNotExists(containerProperties, 400)
                                                    .block()
                                                    .getContainer();
```

# <a name="java-sdk-3xx-async-api"></a>[API asincrónica del SDK de Java 3.x.x](#tab/java-v3-async)

```java
CosmosContainerProperties containerProperties = new CosmosContainerProperties(containerName, "/lastName");

// Custom indexing policy
IndexingPolicy indexingPolicy = new IndexingPolicy();
indexingPolicy.setIndexingMode(IndexingMode.CONSISTENT); //To turn indexing off set IndexingMode.NONE

// Included paths
List<IncludedPath> includedPaths = new ArrayList<>();
IncludedPath includedPath = new IncludedPath();
includedPath.path("/*");
includedPaths.add(includedPath);
indexingPolicy.setIncludedPaths(includedPaths);

// Excluded paths
List<ExcludedPath> excludedPaths = new ArrayList<>();
ExcludedPath excludedPath = new ExcludedPath();
excludedPath.path("/name/*");
excludedPaths.add(excludedPath);
indexingPolicy.excludedPaths(excludedPaths);

containerProperties.indexingPolicy(indexingPolicy);

CosmosContainer containerIfNotExists = database.createContainerIfNotExists(containerProperties, 400)
                                               .block()
                                               .container();
```
---

### <a name="stored-procedures"></a>Procedimientos almacenados

En el fragmento de código siguiente se muestran las diferencias en el modo en que se crean los procedimientos almacenados entre las API asincrónicas 4.0 y 3.x.x:

# <a name="java-sdk-40-async-api"></a>[API asincrónica del SDK de Java 4.0](#tab/java-v4-async)

```java
logger.info("Creating stored procedure...\n");

sprocId = "createMyDocument";
String sprocBody = "function createMyDocument() {\n" +
        "var documentToCreate = {\"id\":\"test_doc\"}\n" +
        "var context = getContext();\n" +
        "var collection = context.getCollection();\n" +
        "var accepted = collection.createDocument(collection.getSelfLink(), documentToCreate,\n" +
        "    function (err, documentCreated) {\n" +
        "if (err) throw new Error('Error' + err.message);\n" +
        "context.getResponse().setBody(documentCreated.id)\n" +
        "});\n" +
        "if (!accepted) return;\n" +
        "}";
CosmosStoredProcedureProperties storedProcedureDef = new CosmosStoredProcedureProperties(sprocId, sprocBody);
container.getScripts()
        .createStoredProcedure(storedProcedureDef,
                new CosmosStoredProcedureRequestOptions()).block();

// ...

logger.info(String.format("Executing stored procedure %s...\n\n", sprocId));

CosmosStoredProcedureRequestOptions options = new CosmosStoredProcedureRequestOptions();
options.setPartitionKey(new PartitionKey("test_doc"));

container.getScripts()
        .getStoredProcedure(sprocId)
        .execute(null, options)
        .flatMap(executeResponse -> {
            logger.info(String.format("Stored procedure %s returned %s (HTTP %d), at cost %.3f RU.\n",
                    sprocId,
                    executeResponse.getResponseAsString(),
                    executeResponse.getStatusCode(),
                    executeResponse.getRequestCharge()));
            return Mono.empty();
        }).block();
```

# <a name="java-sdk-3xx-async-api"></a>[API asincrónica del SDK de Java 3.x.x](#tab/java-v3-async)

```java
logger.info("Creating stored procedure...\n");

sprocId = "createMyDocument";
String sprocBody = "function createMyDocument() {\n" +
        "var documentToCreate = {\"id\":\"test_doc\"}\n" +
        "var context = getContext();\n" +
        "var collection = context.getCollection();\n" +
        "var accepted = collection.createDocument(collection.getSelfLink(), documentToCreate,\n" +
        "    function (err, documentCreated) {\n" +
        "if (err) throw new Error('Error' + err.message);\n" +
        "context.getResponse().setBody(documentCreated.id)\n" +
        "});\n" +
        "if (!accepted) return;\n" +
        "}";
CosmosStoredProcedureProperties storedProcedureDef = new CosmosStoredProcedureProperties(sprocId, sprocBody);
container.getScripts()
        .createStoredProcedure(storedProcedureDef,
                new CosmosStoredProcedureRequestOptions()).block();

// ...

logger.info(String.format("Executing stored procedure %s...\n\n", sprocId));

CosmosStoredProcedureRequestOptions options = new CosmosStoredProcedureRequestOptions();
options.partitionKey(new PartitionKey("test_doc"));

container.getScripts()
        .getStoredProcedure(sprocId)
        .execute(null, options)
        .flatMap(executeResponse -> {
            logger.info(String.format("Stored procedure %s returned %s (HTTP %d), at cost %.3f RU.\n",
                    sprocId,
                    executeResponse.responseAsString(),
                    executeResponse.statusCode(),
                    executeResponse.requestCharge()));
            return Mono.empty();
        }).block();
```
---

### <a name="change-feed"></a>Fuente de cambios

En el fragmento de código siguiente se muestran las diferencias en el modo en que se ejecutan las operaciones de fuente de cambios entre las API asincrónicas 4.0 y 3.x.x:

# <a name="java-sdk-40-async-api"></a>[API asincrónica del SDK de Java 4.0](#tab/java-v4-async)

```java
ChangeFeedProcessor changeFeedProcessorInstance = 
ChangeFeedProcessor.changeFeedProcessorBuilder()
    .setHostName(hostName)
    .setFeedContainer(feedContainer)
    .setLeaseContainer(leaseContainer)
    .setHandleChanges((List<JsonNode> docs) -> {
        logger.info("--->setHandleChanges() START");

        for (JsonNode document : docs) {
            try {
                //Change Feed hands the document to you in the form of a JsonNode
                //As a developer you have two options for handling the JsonNode document provided to you by Change Feed
                //One option is to operate on the document in the form of a JsonNode, as shown below. This is great
                //especially if you do not have a single uniform data model for all documents.
                logger.info("---->DOCUMENT RECEIVED: " + OBJECT_MAPPER.writerWithDefaultPrettyPrinter()
                        .writeValueAsString(document));

                //You can also transform the JsonNode to a POJO having the same structure as the JsonNode,
                //as shown below. Then you can operate on the POJO.
                CustomPOJO pojo_doc = OBJECT_MAPPER.treeToValue(document, CustomPOJO.class);
                logger.info("----=>id: " + pojo_doc.getId());

            } catch (JsonProcessingException e) {
                e.printStackTrace();
            }
        }
        logger.info("--->handleChanges() END");

    })
    .build();

// ...

 changeFeedProcessorInstance.start()
                            .subscribeOn(Schedulers.elastic())
                            .subscribe();
```

# <a name="java-sdk-3xx-async-api"></a>[API asincrónica del SDK de Java 3.x.x](#tab/java-v3-async)

```java
ChangeFeedProcessor changeFeedProcessorInstance = 
ChangeFeedProcessor.Builder()
        .hostName(hostName)
        .feedContainer(feedContainer)
        .leaseContainer(leaseContainer)
        .handleChanges((List<CosmosItemProperties> docs) -> {
            logger.info("--->setHandleChanges() START");

            for (CosmosItemProperties document : docs) {
                try {

                    // You are given the document as a CosmosItemProperties instance which you may
                    // cast to the desired type.
                    CustomPOJO pojo_doc = document.getObject(CustomPOJO.class);
                    logger.info("----=>id: " + pojo_doc.id());

                } catch (Exception e) {
                    e.printStackTrace();
                }
            }
            logger.info("--->handleChanges() END");

        })
        .build();

// ...

 changeFeedProcessorInstance.start()
                            .subscribeOn(Schedulers.elastic())
                            .subscribe();
```
---

### <a name="container-level-time-to-livettl"></a>Período de vida (TTL) de nivel de contenedor

En el siguiente fragmento de código se muestran las diferencias en cómo crear el período de vida de los datos en el contenedor mediante las API asincrónicas 4.0 y 3.x.x:

# <a name="java-sdk-40-async-api"></a>[API asincrónica del SDK de Java 4.0](#tab/java-v4-async)

```java
CosmosAsyncContainer container;

// Create a new container with TTL enabled with default expiration value
CosmosContainerProperties containerProperties = new CosmosContainerProperties("myContainer", "/myPartitionKey");
containerProperties.setDefaultTimeToLiveInSeconds(90 * 60 * 60 * 24);
container = database.createContainerIfNotExists(containerProperties, 400).block().getContainer();
```

# <a name="java-sdk-3xx-async-api"></a>[API asincrónica del SDK de Java 3.x.x](#tab/java-v3-async)

```java
CosmosContainer container;

// Create a new container with TTL enabled with default expiration value
CosmosContainerProperties containerProperties = new CosmosContainerProperties("myContainer", "/myPartitionKey");
containerProperties.defaultTimeToLive(90 * 60 * 60 * 24);
container = database.createContainerIfNotExists(containerProperties, 400).block().container();
```
---

### <a name="item-level-time-to-livettl"></a>Período de vida (TTL) de nivel de elemento

En el siguiente fragmento de código se muestran las diferencias en cómo crear el período de vida para un elemento mediante las API asincrónicas 4.0 y 3.x.x:

# <a name="java-sdk-40-async-api"></a>[API asincrónica del SDK de Java 4.0](#tab/java-v4-async)

```java
// Include a property that serializes to "ttl" in JSON
public class SalesOrder
{
    private String id;
    private String customerId;
    private Integer ttl;

    public SalesOrder(String id, String customerId, Integer ttl) {
        this.id = id;
        this.customerId = customerId;
        this.ttl = ttl;
    }

    public String getId() {return this.id;}
    public void setId(String new_id) {this.id = new_id;}
    public String getCustomerId() {return this.customerId;}
    public void setCustomerId(String new_cid) {this.customerId = new_cid;}
    public Integer getTtl() {return this.ttl;}
    public void setTtl(Integer new_ttl) {this.ttl = new_ttl;}

    //...
}

// Set the value to the expiration in seconds
SalesOrder salesOrder = new SalesOrder(
    "SO05",
    "CO18009186470",
    60 * 60 * 24 * 30  // Expire sales orders in 30 days
);
```

# <a name="java-sdk-3xx-async-api"></a>[API asincrónica del SDK de Java 3.x.x](#tab/java-v3-async)

```java
// Include a property that serializes to "ttl" in JSON
public class SalesOrder
{
    private String id;
    private String customerId;
    private Integer ttl;

    public SalesOrder(String id, String customerId, Integer ttl) {
        this.id = id;
        this.customerId = customerId;
        this.ttl = ttl;
    }

    public String id() {return this.id;}
    public SalesOrder id(String new_id) {this.id = new_id; return this;}
    public String customerId() {return this.customerId; return this;}
    public SalesOrder customerId(String new_cid) {this.customerId = new_cid;}
    public Integer ttl() {return this.ttl;}
    public SalesOrder ttl(Integer new_ttl) {this.ttl = new_ttl; return this;}

    //...
}

// Set the value to the expiration in seconds
SalesOrder salesOrder = new SalesOrder(
    "SO05",
    "CO18009186470",
    60 * 60 * 24 * 30  // Expire sales orders in 30 days
);
```
---

## <a name="next-steps"></a>Pasos siguientes

* [Creación de una aplicación Java](create-sql-api-java.md) para administrar los datos de SQL API de Azure Cosmos DB mediante el SDK V4
* Más información sobre los [SDK de Java basados en reactores](https://github.com/Azure-Samples/azure-cosmos-java-sql-api-samples/blob/master/reactor-pattern-guide.md)
* Más información sobre la conversión de código asincrónico de RxJava en código asincrónico de reactor con la [Guía de reactor vs RxJava](https://github.com/Azure-Samples/azure-cosmos-java-sql-api-samples/blob/master/reactor-rxjava-guide.md)