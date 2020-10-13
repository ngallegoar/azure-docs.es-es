---
title: Notas de la versión y recursos del conector Cosmos DB Apache Spark para SQL API
description: Obtenga toda la información posible sobre el conector Azure Cosmos DB Apache Spark para SQL API, como las fechas de lanzamiento, las fechas de retirada y los cambios realizados entre las diferentes versiones de Azure Cosmos DB SQL Async Java SDK.
author: anfeldma-ms
ms.service: cosmos-db
ms.subservice: cosmosdb-sql
ms.devlang: java
ms.topic: reference
ms.date: 08/12/2020
ms.author: anfeldma
ms.custom: devx-track-java
ms.openlocfilehash: afee95f6a8776c3506e10c29cfd8e776734a915a
ms.sourcegitcommit: 32c521a2ef396d121e71ba682e098092ac673b30
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/25/2020
ms.locfileid: "91326684"
---
# <a name="azure-cosmos-db-apache-spark-connector-for-core-sql-api-release-notes-and-resources"></a>Conector Azure Cosmos DB Apache Spark para Core (SQL) API: Notas de la versión y recursos
> [!div class="op_single_selector"]
> * [.NET SDK v3](sql-api-sdk-dotnet-standard.md)
> * [SDK de .NET v2](sql-api-sdk-dotnet.md)
> * [SDK de .NET Core v2](sql-api-sdk-dotnet-core.md)
> * [SDK de fuente de cambios de .NET v2](sql-api-sdk-dotnet-changefeed.md)
> * [Node.js](sql-api-sdk-node.md)
> * [SDK de Java v4](sql-api-sdk-java-v4.md)
> * [Versión 2 del SDK de Java asincrónico](sql-api-sdk-async-java.md)
> * [SDK de Java v2 sincrónico](sql-api-sdk-java.md)
> * [Spring Data v2](sql-api-sdk-java-spring-v2.md)
> * [Spring Data v3](sql-api-sdk-java-spring-v3.md)
> * [Conector de Spark](sql-api-sdk-java-spark.md)
> * [Python](sql-api-sdk-python.md)
> * [REST](/rest/api/cosmos-db/)
> * [Proveedor de recursos de REST](/rest/api/cosmos-db-resource-provider/)
> * [SQL](sql-api-query-reference.md)
> * [Bulk Executor: .NET v2](sql-api-sdk-bulk-executor-dot-net.md)
> * [Bulk Executor: Java](sql-api-sdk-bulk-executor-java.md)

Puede acelerar el análisis de macrodatos utilizando el conector Apache Spark Connector para Core (SQL) de Azure Cosmos DB. El conector Spark permite ejecutar trabajos de [Spark](https://spark.apache.org/) en datos almacenados en Azure Cosmos DB. Se admite el procesamiento por lotes y por secuencias.

Este conector se puede usar con [Azure Databricks](https://azure.microsoft.com/services/databricks) o con [Azure HDInsight](https://azure.microsoft.com/services/hdinsight/), que proporcionan clústeres de Spark administrados en Azure. En la tabla siguiente, se muestran las versiones admitidas:

| Componente | Versión |
|---------|-------|
| Spark de Apache | 2.4.*x*, 2.3.*x*, 2.2.*x* y 2.1.*x* |
| Scala | 2.11 |
| Azure Databricks (versión en tiempo de ejecución) | Posterior a la 3.4 |

> [!WARNING]
> Este conector admite la API Core (SQL) de Azure Cosmos DB.
> En el caso de Cosmos DB API para MongoDB, utilice el [conector de MongoDB para Spark](https://docs.mongodb.com/spark-connector/master/).
> En el caso de Cosmos DB Cassandra API, use el [conector Spark de Cassandra](https://github.com/datastax/spark-cassandra-connector).
>

## <a name="resources"></a>Recursos

| Recurso | Vínculo |
|---|---|
| **Descarga del SDK** | [Descarga del archivo .jar más reciente](https://aka.ms/CosmosDB_OLTP_Spark_2.4_LKG), [Maven](https://search.maven.org/search?q=a:azure-cosmosdb-spark_2.4.0_2.11) |
|**Documentación de la API** | [Referencia del conector de Spark]() |
|**Contribuciones al SDK** | [Conector de Apache Spark para Azure Cosmos DB en GitHub](https://github.com/Azure/azure-cosmosdb-spark) | 
|**Introducción** | [Aceleración de análisis de macrodatos mediante el conector de Apache Spark a Azure Cosmos DB](https://docs.microsoft.com/azure/cosmos-db/spark-connector#bk_working_with_connector) <br> [Uso del flujo estructurado de Apache Spark con Apache Kafka y Azure Cosmos DB](https://docs.microsoft.com/azure/hdinsight/apache-kafka-spark-structured-streaming-cosmosdb?toc=/azure/cosmos-db/toc.json&bc=/azure/cosmos-db/breadcrumb/toc.json) | 

## <a name="release-history"></a>Historial de versiones

### <a name="330"></a>3.3.0
#### <a name="new-features"></a>Nuevas características
- Agrega una nueva opción de configuración, `changefeedstartfromdatetime`, que se puede usar para especificar la hora de inicio para cuando se debe procesar el cambio. Para más información, consulte [Opciones de configuración](https://github.com/Azure/azure-cosmosdb-spark/wiki/Configuration-references).

### <a name="320"></a>3.2.0
#### <a name="key-bug-fixes"></a>Correcciones de errores clave
- Corrige una regresión que causó un consumo excesivo de memoria en los ejecutores de grandes conjuntos de resultados (por ejemplo, con millones de filas), lo que en última instancia genera el error `java.lang.OutOfMemoryError: GC overhead limit exceeded`.

### <a name="311"></a>3.1.1
#### <a name="key-bug-fixes"></a>Correcciones de errores clave
* Se ha corregido un caso extremo relacionado con el punto de control de streaming en el que `ID` contenía el carácter de barra vertical (|) con la configuración `ChangeFeedMaxPagesPerBatch` aplicada.

### <a name="310"></a>3.1.0
#### <a name="new-features"></a>Nuevas características
* Ahora se pueden realizar actualizaciones en bloque cuando se usan claves de partición anidadas.
* Ahora se pueden utilizar tipos de datos decimal y float en las operaciones de escritura que se realizan en Azure Cosmos DB.
* Ahora se pueden utilizar tipos de marca de tiempo cuando se emplea Long (época Unix) como valor.

### <a name="308"></a>3.0.8
#### <a name="key-bug-fixes"></a>Correcciones de errores clave
* Se ha corregido la excepción de conversión de tipos que se producía al utilizar la configuración `WriteThroughputBudget`.

### <a name="307"></a>3.0.7
#### <a name="new-features"></a>Nuevas características
* Agrega información de error sobre errores en masa a la excepción y el registro.

### <a name="306"></a>3.0.6
#### <a name="key-bug-fixes"></a>Correcciones de errores clave
* Corrige problemas de punto de control de streaming.

### <a name="305"></a>3.0.5
#### <a name="key-bug-fixes"></a>Correcciones de errores clave
* Para reducir el ruido, se ha corregido el nivel de registro de los mensajes que accidentalmente se dejaban con el nivel ERROR.

### <a name="304"></a>3.0.4
#### <a name="key-bug-fixes"></a>Correcciones de errores clave
* Se ha corregido un error de streaming estructurado que se producía durante las divisiones de particiones. El error podía hacer que faltaran algunos registros de fuentes de cambios o generar excepciones nulas en las operaciones de escritura de puntos de comprobación.

### <a name="303"></a>3.0.3
#### <a name="key-bug-fixes"></a>Correcciones de errores clave
* Se ha corregido un error que causaba que se omitiera un esquema personalizado proporcionado para readStream.

### <a name="302"></a>3.0.2
#### <a name="key-bug-fixes"></a>Correcciones de errores clave
* Se ha corregido una regresión (un archivo JAR sin sombrear contenía todas las dependencias sombreadas) que incrementaba el tiempo de compilación en un 50 %.

### <a name="301"></a>3.0.1
#### <a name="key-bug-fixes"></a>Correcciones de errores clave
* Se ha corregido un problema con las dependencias que causaba la excepción RequestTimeoutException en el transporte directo a través de TCP.

### <a name="300"></a>3.0.0
#### <a name="new-features"></a>Nuevas características
* Se ha mejorado la administración y la agrupación de conexiones para reducir el número de llamadas de metadatos.

## <a name="faq"></a>Preguntas más frecuentes
[!INCLUDE [cosmos-db-sdk-faq](../../includes/cosmos-db-sdk-faq.md)]

## <a name="next-steps"></a>Pasos siguientes

Más información acerca de [Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db/).

Más información sobre [Apache Spark](https://spark.apache.org/).
