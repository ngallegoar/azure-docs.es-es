---
title: Notas de la versión y recursos de Spring Data Azure Cosmos DB v3 para API SQL
description: Conozca Spring Data Azure Cosmos DB v3 para la API de SQL, incluidas las fechas de lanzamiento y de retirada, y los cambios realizados entre las versiones del SDK de Java asincrónico para SQL de Azure Cosmos DB.
author: anfeldma-ms
ms.service: cosmos-db
ms.subservice: cosmosdb-sql
ms.devlang: java
ms.topic: reference
ms.date: 08/18/2020
ms.author: anfeldma
ms.custom: devx-track-java
ms.openlocfilehash: 9e5b92918d93109183740be555bb805877862407
ms.sourcegitcommit: 0fd1f3fe7817ad44d878d580ec167e1508051795
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/19/2020
ms.locfileid: "90817877"
---
# <a name="spring-data-azure-cosmos-db-v3-for-core-sql-api-release-notes-and-resources"></a>Spring Data Azure Cosmos DB v3 para la API Core (SQL): Notas de la versión y recursos
> [!div class="op_single_selector"]
> * [.NET SDK v3](sql-api-sdk-dotnet-standard.md)
> * [SDK de .NET v2](sql-api-sdk-dotnet.md)
> * [SDK de .NET Core v2](sql-api-sdk-dotnet-core.md)
> * [SDK de fuente de cambios de .NET v2](sql-api-sdk-dotnet-changefeed.md)
> * [Node.js](sql-api-sdk-node.md)
> * [SDK de Java v4](sql-api-sdk-java-v4.md)
> * [Versión 2 del SDK de Java asincrónico](sql-api-sdk-async-java.md)
> * [SDK de Java v2 sincrónico](sql-api-sdk-java.md)
> * [Spring Data v2](sql-api-sdk-java-spring-v2.md)
> * [Spring Data v3](sql-api-sdk-java-spring-v3.md)
> * [Conector de Spark](sql-api-sdk-java-spark.md)
> * [Python](sql-api-sdk-python.md)
> * [REST](/rest/api/cosmos-db/)
> * [Proveedor de recursos de REST](/rest/api/cosmos-db-resource-provider/)
> * [SQL](sql-api-query-reference.md)
> * [Bulk Executor: .NET v2](sql-api-sdk-bulk-executor-dot-net.md)
> * [Bulk Executor: Java](sql-api-sdk-bulk-executor-java.md)

La versión 3 de Spring Data Azure Cosmos DB para Core (SQL) permite que los desarrolladores utilicen Azure Cosmos DB en aplicaciones de Spring. Spring Data Azure Cosmos DB expone la interfaz de Spring Data para manipular las bases de datos y las colecciones, trabajar con documentos y emitir consultas. Las API sincrónicas y las API asincrónicas (reactivas) se admiten en el mismo artefacto Maven. 

Spring Data Azure Cosmos DB tiene una dependencia en la plataforma de Spring Data. El equipo del SDK de Azure Cosmos DB publica los artefactos de Maven para las versiones 2.2.2 y 2.2.3 de Spring Data.

[Spring Framework](https://spring.io/projects/spring-framework) es un modelo de programación y configuración que simplifica el desarrollo de aplicaciones Java. Spring simplifica los "recursos" de las aplicaciones mediante la inserción de dependencias. Son muchos los desarrolladores que prefieren Spring, porque la compilación y prueba de las aplicaciones resultan más sencillas. [Spring Boot](https://spring.io/projects/spring-boot) amplía este control de los recursos pensando en la implementación de microservicios y aplicaciones web. [Spring Data](https://spring.io/projects/spring-data) es un modelo y una plataforma de programación para acceder a almacenes de datos como Azure Cosmos DB desde el contexto de una aplicación de Spring o Spring Boot. 

Puede usar Spring Data Azure Cosmos DB en las aplicaciones de [Azure Spring Cloud](https://azure.microsoft.com/services/spring-cloud/).

> [!IMPORTANT]  
> Estas notas de la versión corresponden a la versión 3 de Spring Data Azure Cosmos DB. Puede encontrar las [notas de la versión 2 aquí](sql-api-sdk-java-spring-v2.md). 
>
> Spring Data Azure Cosmos DB solo admite la API de SQL.
>
> Consulte estos artículos para obtener información sobre Spring Data en otras API de Azure Cosmos DB:
> * [Spring Data para Apache Cassandra con Azure Cosmos DB](https://docs.microsoft.com/azure/developer/java/spring-framework/configure-spring-data-apache-cassandra-with-cosmos-db)
> * [Spring Data MongoDB con Azure Cosmos DB](https://docs.microsoft.com/azure/developer/java/spring-framework/configure-spring-data-mongodb-with-cosmos-db)
> * [Spring Data Gremlin con Azure Cosmos DB](https://docs.microsoft.com/azure/developer/java/spring-framework/configure-spring-data-gremlin-java-app-with-cosmos-db)
>

## <a name="start-here"></a>Comience aquí

# <a name="explore"></a>[Explorar](#tab/explore)

<img src="media/sql-api-sdk-java-spring-v3/up-arrow.png" alt="explore the tabs above" width="80"/>

#### <a name="these-tabs-contain-basic-spring-data-azure-cosmos-db-samples"></a>Estas pestañas contienen ejemplos básicos de Spring Data Azure Cosmos DB.

# <a name="pomxml"></a>[pom.xml](#tab/pom)

### <a name="configure-dependencies"></a>Configuración de dependencias

  ```xml
  <dependency>
      <groupId>com.azure</groupId>
      <artifactId>azure-spring-data-cosmos</artifactId>
      <version>latest</version>
  </dependency>
  ```

# <a name="connect"></a>[Conexión](#tab/connect)

### <a name="connect"></a>Conectar

Especifique los detalles de una cuenta y un contenedor de Azure Cosmos DB. Spring Data Azure Cosmos DB crea automáticamente el cliente y se conecta al contenedor.

[application.properties](https://github.com/Azure-Samples/azure-spring-data-cosmos-java-sql-api-getting-started/blob/main/azure-spring-data-cosmos-java-getting-started/src/main/resources/application.properties):
```
cosmos.uri=${ACCOUNT_HOST}
cosmos.key=${ACCOUNT_KEY}
cosmos.secondaryKey=${SECONDARY_ACCOUNT_KEY}

dynamic.collection.name=spel-property-collection
# Populate query metrics
cosmos.queryMetricsEnabled=true
```

# <a name="doc-ops"></a>[Operaciones de documento](#tab/docs)

### <a name="document-operations"></a>Operaciones de documento

Crear:
:::code language="java" source="~/spring-data-azure-cosmos-db-sql-tutorial/azure-spring-data-cosmos-java-getting-started/src/main/java/com/azure/spring/data/cosmostutorial/SampleApplication.java" ID="Create":::

Eliminar:
:::code language="java" source="~/spring-data-azure-cosmos-db-sql-tutorial/azure-spring-data-cosmos-java-getting-started/src/main/java/com/azure/spring/data/cosmostutorial/SampleApplication.java" ID="Delete":::

# <a name="query"></a>[Consultar](#tab/queries)

Consulta:
:::code language="java" source="~/spring-data-azure-cosmos-db-sql-tutorial/azure-spring-data-cosmos-java-getting-started/src/main/java/com/azure/spring/data/cosmostutorial/SampleApplication.java" ID="Query":::

---

## <a name="resources"></a>Recursos

* **Contribución al SDK**: [Repositorio de Spring Data Azure Cosmos DB en GitHub](https://github.com/Azure/azure-sdk-for-java/tree/master/sdk/cosmos/azure-spring-data-cosmos)

* **Tutorial**: [Tutorial de Spring Data Azure Cosmos DB en GitHub](https://github.com/Azure-Samples/azure-spring-data-cosmos-java-sql-api-getting-started) 

## <a name="release-history"></a>Historial de versiones

### <a name="300-beta2-september-17-2020"></a>3.0.0-beta.2 (17 de septiembre de 2020)

#### <a name="new-features"></a>Nuevas características

* Identificador de artefacto actualizado a `azure-spring-data-cosmos`.
* Dependencia de azure-cosmos actualizada a `4.5.0`.
* Compatibilidad de `Query Annotation` con consultas nativas.
* Compatibilidad con Java 11.
* Se ha agregado compatibilidad con la clave de partición anidada al exponer campo `partitionKeyPath` en la anotación `@Container`.
* Se ha agregado compatibilidad con el tipo de consulta `limit` que permite usar `top` y `first` al definir las API del repositorio.

#### <a name="key-bug-fixes"></a>Correcciones de errores clave

* Se corrigió el error de clave de partición anidada cuando se utiliza con la anotación `@GeneratedValue`.

### <a name="300-beta1-august-17-2020"></a>3.0.0-beta.1 (17 de agosto de 2020)

#### <a name="new-features"></a>Nuevas características

* Actualiza el identificador de grupo a `com.azure`.
* Actualiza el identificador de artefacto a `azure-spring-data-2-3-cosmos`.
* Actualiza la dependencia del SDK de azure-cosmos a `4.3.2-beta.2`.
* Agrega compatibilidad con entidades de auditoría: administración automática de los campos anotados `createdBy`, `createdDate`, `lastModifiedBy`y `lastModifiedDate`.
* Agrega compatibilidad de anotaciones `@GeneratedValue` con la generación automática de identificadores para los campos de identificador de tipo `String`.
* Agrega compatibilidad de configuración de varias bases de datos con cuentas individuales de Azure Cosmos DB con varias bases de datos y varias cuentas de Azure Cosmos DB con varias bases de datos.
* Agrega compatibilidad con la anotación `@Version` en cualquier campo de cadena.
* Las API de sincronización actualizadas devuelven tipos a tipos `Iterable` en lugar de `List`.
* Expone `CosmosClientBuilder` del SDK de Azure Cosmos DB como bean de Spring para la clase `@Configuration`.
* Actualiza `CosmosConfig` para contener métricas de consulta y la implementación del procesador de diagnóstico de respuesta.
* Agrega compatibilidad con la devolución del tipo de datos `Optional` en consultas de un solo resultado.

#### <a name="renames"></a>Cambios de nombre

* De `CosmosDbFactory` a `CosmosFactory`.
* De `CosmosDBConfig` a `CosmosConfig`.
* De `CosmosDBAccessException` a `CosmosAccessException`.
* De anotación `Document` a anotación `Container`.
* De anotación `DocumentIndexingPolicy` a anotación `CosmosIndexingPolicy`.
* De `DocumentQuery` a `CosmosQuery`.
* Marca application.properties de `populateQueryMetrics` a `queryMetricsEnabled`.

#### <a name="key-bug-fixes"></a>Correcciones de errores clave

* Programación de la tarea de registro de diagnósticos en subprocesos `Parallel` para evitar el bloqueo de subprocesos de E/S de Netty.
* Corrige el bloqueo optimista en la operación de eliminación.
* Corrige un problema relacionado con las consultas de escape para la cláusula `IN`.
* Corrige un problema al permitir el tipo de datos `long` para `@Id`.
* Corrige un problema permitiendo `boolean`, `long`, `int`y `double` como tipos de datos para la anotación `@PartitionKey`.
* Corrige las palabras clave `IgnoreCase` y `AllIgnoreCase` en las consultas que no distinguen entre mayúsculas y minúsculas.
* Quita el valor de unidad de solicitud predeterminado de 4000 cuando se crean contenedores automáticamente.

## <a name="faq"></a>Preguntas más frecuentes

[!INCLUDE [cosmos-db-sdk-faq](../../includes/cosmos-db-sdk-faq.md)]

## <a name="next-steps"></a>Pasos siguientes

Más información acerca de [Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db/).

Más información sobre [Spring Framework](https://spring.io/projects/spring-framework).

Más información sobre [Spring Boot](https://spring.io/projects/spring-boot).

Más información sobre [Spring Data](https://spring.io/projects/spring-data).