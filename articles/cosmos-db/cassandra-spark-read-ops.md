---
title: Lectura de los datos de la tabla de Cassandra API con Spark
titleSufix: Azure Cosmos DB
description: En este artículo se describe cómo leer datos almacenados en tablas de Cassandra API en Azure Cosmos DB.
author: TheovanKraay
ms.author: thvankra
ms.reviewer: sngun
ms.service: cosmos-db
ms.subservice: cosmosdb-cassandra
ms.topic: how-to
ms.date: 06/02/2020
ms.custom: seodec18
ms.openlocfilehash: 68a64ad1ddb955ccebdcddca996959f1bb5f932b
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "91840969"
---
# <a name="read-data-from-azure-cosmos-db-cassandra-api-tables-using-spark"></a>Lectura de datos de tablas de Cassandra API de Azure Cosmos DB con Spark

 En este artículo se describe cómo leer los datos almacenados en Cassandra API de Azure Cosmos DB desde Spark.

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

### <a name="read-table-using-sessionreadformat-command"></a>Lectura de la tabla con el comando session.read.format

```scala
val readBooksDF = sqlContext
  .read
  .format("org.apache.spark.sql.cassandra")
  .options(Map( "table" -> "books", "keyspace" -> "books_ks"))
  .load

readBooksDF.explain
readBooksDF.show
```
### <a name="read-table-using-sparkreadcassandraformat"></a>Lectura de la tabla con spark.read.cassandraFormat 

```scala
val readBooksDF = spark.read.cassandraFormat("books", "books_ks", "").load()
```

### <a name="read-specific-columns-in-table"></a>Lectura de columnas específicas de la tabla

```scala
val readBooksDF = spark
  .read
  .format("org.apache.spark.sql.cassandra")
  .options(Map( "table" -> "books", "keyspace" -> "books_ks"))
  .load
  .select("book_name","book_author", "book_pub_year")

readBooksDF.printSchema
readBooksDF.explain
readBooksDF.show
```

### <a name="apply-filters"></a>Aplicación de filtros

Puede insertar predicados en la base de datos para permitir consultas Spark mejor optimizadas. Un predicado es una condición de una consulta que devuelve true o false, y que normalmente se encuentra en la cláusula WHERE. Un predicado inserta filtros en los datos de la consulta de base de datos, lo que reduce el número de entradas recuperadas de la base de datos y mejora el rendimiento de las consultas. De forma predeterminada, la API Dataset de Spark insertará automáticamente cláusulas WHERE válidas en la base de datos. 

```scala
val df = spark.read.cassandraFormat("books", "books_ks").load
df.explain
val dfWithPushdown = df.filter(df("book_pub_year") > 1891)
dfWithPushdown.explain

readBooksDF.printSchema
readBooksDF.explain
readBooksDF.show
```

La sección PushedFilters del plan físico incluye el filtro de inserción GreaterThan. 

:::image type="content" source="./media/cassandra-spark-read-ops/pushdown-predicates.png" alt-text="particiones":::

## <a name="rdd-api"></a>RDD API

### <a name="read-table"></a>Lectura de tabla
```scala
val bookRDD = sc.cassandraTable("books_ks", "books")
bookRDD.take(5).foreach(println)
```

### <a name="read-specific-columns-in-table"></a>Lectura de columnas específicas de la tabla

```scala
val booksRDD = sc.cassandraTable("books_ks", "books").select("book_id","book_name").cache
booksRDD.take(5).foreach(println)
```

## <a name="sql-views"></a>Vistas SQL 

### <a name="create-a-temporary-view-from-a-dataframe"></a>Creación de una vista temporal desde un dataframe

```scala
spark
  .read
  .format("org.apache.spark.sql.cassandra")
  .options(Map( "table" -> "books", "keyspace" -> "books_ks"))
  .load.createOrReplaceTempView("books_vw")
```

### <a name="run-queries-against-the-view"></a>Ejecución de consultas en la vista

```sql
select * from books_vw where book_pub_year > 1891
```

## <a name="next-steps"></a>Pasos siguientes

Los siguientes son artículos adicionales sobre cómo trabajar con Cassandra API de Azure Cosmos DB desde Spark:
 
 * [Upsert operations](cassandra-spark-upsert-ops.md) (Operaciones de upsert)
 * [Delete operations](cassandra-spark-delete-ops.md) (Operaciones de eliminación)
 * [Aggregation operations](cassandra-spark-aggregation-ops.md) (Operaciones de agregación)
 * [Table copy operations](cassandra-spark-table-copy-ops.md) (Operaciones de copia en tabla)

