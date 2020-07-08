---
title: Creación o inserción de datos en Cassandra API de Azure Cosmos DB desde Spark
description: En este artículo se detalla cómo insertar datos de ejemplo en tablas de Cassandra API de Azure Cosmos DB
author: kanshiG
ms.author: govindk
ms.reviewer: sngun
ms.service: cosmos-db
ms.subservice: cosmosdb-cassandra
ms.topic: how-to
ms.date: 09/24/2018
ms.openlocfilehash: 56899b7d81f17e678026b1aad7f3e2d9f8b9b0b0
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/02/2020
ms.locfileid: "85260532"
---
# <a name="createinsert-data-into-azure-cosmos-db-cassandra-api-from-spark"></a>Creación e inserción de datos en Cassandra API de Azure Cosmos DB desde Spark
 
En este artículo se describe cómo insertar datos de ejemplo en una tabla de Cassandra API de Azure Cosmos DB desde Spark.

## <a name="cassandra-api-configuration"></a>Configuración de Cassandra API

```scala
import org.apache.spark.sql.cassandra._
//Spark connector
import com.datastax.spark.connector._
import com.datastax.spark.connector.cql.CassandraConnector

//CosmosDB library for multiple retry
import com.microsoft.azure.cosmosdb.cassandra

//Connection-related
spark.conf.set("spark.cassandra.connection.host","YOUR_ACCOUNT_NAME.cassandra.cosmosdb.azure.com")
spark.conf.set("spark.cassandra.connection.port","10350")
spark.conf.set("spark.cassandra.connection.ssl.enabled","true")
spark.conf.set("spark.cassandra.auth.username","YOUR_ACCOUNT_NAME")
spark.conf.set("spark.cassandra.auth.password","YOUR_ACCOUNT_KEY")
spark.conf.set("spark.cassandra.connection.factory", "com.microsoft.azure.cosmosdb.cassandra.CosmosDbConnectionFactory")
//Throughput-related...adjust as needed
spark.conf.set("spark.cassandra.output.batch.size.rows", "1")
spark.conf.set("spark.cassandra.connection.connections_per_executor_max", "10")
spark.conf.set("spark.cassandra.output.concurrent.writes", "1000")
spark.conf.set("spark.cassandra.concurrent.reads", "512")
spark.conf.set("spark.cassandra.output.batch.grouping.buffer.size", "1000")
spark.conf.set("spark.cassandra.connection.keep_alive_ms", "600000000")
```
## <a name="dataframe-api"></a>Dataframe API

### <a name="create-a-dataframe-with-sample-data"></a>Creación de una trama con datos de ejemplo

```scala
// Generate a dataframe containing five records
val booksDF = Seq(
   ("b00001", "Arthur Conan Doyle", "A study in scarlet", 1887),
   ("b00023", "Arthur Conan Doyle", "A sign of four", 1890),
   ("b01001", "Arthur Conan Doyle", "The adventures of Sherlock Holmes", 1892),
   ("b00501", "Arthur Conan Doyle", "The memoirs of Sherlock Holmes", 1893),
   ("b00300", "Arthur Conan Doyle", "The hounds of Baskerville", 1901)
).toDF("book_id", "book_author", "book_name", "book_pub_year")

//Review schema
booksDF.printSchema

//Print
booksDF.show
```

> [!NOTE]
> Aún no se admite la funcionalidad "Crear si no existe" a nivel de registro.

### <a name="persist-to-azure-cosmos-db-cassandra-api"></a>Persistencia en Cassandra API de Azure Cosmos DB

Al guardar los datos, también puede establecer el período de vida y la configuración de la directiva de coherencia como se muestra en el siguiente ejemplo:

```scala
//Persist
booksDF.write
  .mode("append")
  .format("org.apache.spark.sql.cassandra")
  .options(Map( "table" -> "books", "keyspace" -> "books_ks", "output.consistency.level" -> "ALL", "ttl" -> "10000000"))
  .save()
```

> [!NOTE]
> No se admite todavía el período de vida a nivel de columna.

#### <a name="validate-in-cqlsh"></a>Validar en cqlsh

```sql
use books_ks;
select * from books;
```

## <a name="resilient-distributed-database-rdd-api"></a>Resilient Distributed Database (RDD) API

### <a name="create-a-rdd-with-sample-data"></a>Creación de una RDD con datos de ejemplo
```scala
//Delete records created in the previous section 
val cdbConnector = CassandraConnector(sc)
cdbConnector.withSessionDo(session => session.execute("delete from books_ks.books where book_id in ('b00300','b00001','b00023','b00501','b09999','b01001','b00999','b03999','b02999');"))

//Create RDD
val booksRDD = sc.parallelize(Seq(
   ("b00001", "Arthur Conan Doyle", "A study in scarlet", 1887),
   ("b00023", "Arthur Conan Doyle", "A sign of four", 1890),
   ("b01001", "Arthur Conan Doyle", "The adventures of Sherlock Holmes", 1892),
   ("b00501", "Arthur Conan Doyle", "The memoirs of Sherlock Holmes", 1893),
   ("b00300", "Arthur Conan Doyle", "The hounds of Baskerville", 1901)
))

//Review
booksRDD.take(2).foreach(println)
```

> [!NOTE]
> Aún no se admite la funcionalidad "Crear si no existe".

### <a name="persist-to-azure-cosmos-db-cassandra-api"></a>Persistencia en Cassandra API de Azure Cosmos DB

Al guardar los datos en Cassandra API, también puede establecer el período de vida y la configuración de la directiva de coherencia como se muestra en el siguiente ejemplo:

```scala
import com.datastax.spark.connector.writer._

//Persist
booksRDD.saveToCassandra("books_ks", "books", SomeColumns("book_id", "book_author", "book_name", "book_pub_year"),writeConf = WriteConf(ttl = TTLOption.constant(900000),consistencyLevel = ConsistencyLevel.ALL))
```

#### <a name="validate-in-cqlsh"></a>Validar en cqlsh

```sql
use books_ks;
select * from books;
```

## <a name="next-steps"></a>Pasos siguientes

Tras insertar los datos en la tabla de Cassandra API de Azure Cosmos DB, continúe con los siguientes artículos para realizar otras operaciones en los datos almacenados en Cassandra API de Cosmos DB:
 
* [Read operations](cassandra-spark-read-ops.md) (Operaciones de lectura)
* [Upsert operations](cassandra-spark-upsert-ops.md) (Operaciones de upsert)
* [Delete operations](cassandra-spark-delete-ops.md) (Operaciones de eliminación)
* [Aggregation operations](cassandra-spark-aggregation-ops.md) (Operaciones de agregación)
* [Table copy operations](cassandra-spark-table-copy-ops.md) (Operaciones de copia en tabla)

