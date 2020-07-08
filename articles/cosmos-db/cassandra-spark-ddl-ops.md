---
title: Operaciones DDL en Cassandra API de Azure Cosmos DB de Spark
description: En este artículo se describe el espacio de claves y la tabla de operaciones DDL en Cassandra API de Azure Cosmos DB de Spark.
author: kanshiG
ms.author: govindk
ms.reviewer: sngun
ms.service: cosmos-db
ms.subservice: cosmosdb-cassandra
ms.topic: how-to
ms.date: 09/24/2018
ms.openlocfilehash: 30cac5894998ca2bb9c37217820e1000ed97ba5d
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/02/2020
ms.locfileid: "85260574"
---
# <a name="ddl-operations-in-azure-cosmos-db-cassandra-api-from-spark"></a>Operaciones DDL en Cassandra API de Azure Cosmos DB de Spark

En este artículo se describe el espacio de claves y la tabla de operaciones DDL en Cassandra API de Azure Cosmos DB de Spark.

## <a name="cassandra-api-related-configuration"></a>Configuración de Cassandra API 

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

## <a name="keyspace-ddl-operations"></a>Operaciones DDL de Keyspace

### <a name="create-a-keyspace"></a>Crear un espacio de claves

```scala
//Cassandra connector instance
val cdbConnector = CassandraConnector(sc)

// Create keyspace
cdbConnector.withSessionDo(session => session.execute("CREATE KEYSPACE IF NOT EXISTS books_ks WITH REPLICATION = {'class': 'SimpleStrategy', 'replication_factor': 1 } "))
```

#### <a name="validate-in-cqlsh"></a>Validar en cqlsh

Ejecute el siguiente comando en cqlsh y verá el espacio de claves que creó anteriormente.

```bash
DESCRIBE keyspaces;
```

### <a name="drop-a-keyspace"></a>Colocar un espacio de claves

```scala
val cdbConnector = CassandraConnector(sc)
cdbConnector.withSessionDo(session => session.execute("DROP KEYSPACE books_ks"))
```

#### <a name="validate-in-cqlsh"></a>Validar en cqlsh

```bash
DESCRIBE keyspaces;
```
## <a name="table-ddl-operations"></a>Operaciones DDL de tabla

**Consideraciones:**  

- El rendimiento puede asignarse a nivel de tabla; para ello, use la instrucción create table.  
- Una clave de partición puede almacenar 20 GB de datos.  
- Un registro puede almacenar un máximo de 2 MB de datos.  
- Un intervalo de claves de partición puede almacenar varias claves de partición.

### <a name="create-a-table"></a>Creación de una tabla

```scala
val cdbConnector = CassandraConnector(sc)
cdbConnector.withSessionDo(session => session.execute("CREATE TABLE IF NOT EXISTS books_ks.books(book_id TEXT PRIMARY KEY,book_author TEXT, book_name TEXT,book_pub_year INT,book_price FLOAT) WITH cosmosdb_provisioned_throughput=4000 , WITH default_time_to_live=630720000;"))
```

#### <a name="validate-in-cqlsh"></a>Validar en cqlsh

Ejecute el siguiente comando en cqlsh y verá la tabla denominada "books:" 

```bash
USE books_ks;
DESCRIBE books;
```

El rendimiento previsto y los valores TTL predeterminados no se muestran en la salida del comando anterior, pero puede obtener estos valores en el portal.

### <a name="alter-table"></a>Alter table

Puede modificar los siguientes valores mediante el comando alter table:

* rendimiento aprovisionado 
* valor del período de vida
<br>Actualmente no se admiten los cambios en columnas.

```scala
val cdbConnector = CassandraConnector(sc)
cdbConnector.withSessionDo(session => session.execute("ALTER TABLE books_ks.books WITH cosmosdb_provisioned_throughput=8000, WITH default_time_to_live=0;"))
```

### <a name="drop-table"></a>Quitar tabla

```scala
val cdbConnector = CassandraConnector(sc)
cdbConnector.withSessionDo(session => session.execute("DROP TABLE IF EXISTS books_ks.books;"))
```

#### <a name="validate-in-cqlsh"></a>Validar en cqlsh

Ejecute el siguiente comando en cqlsh y verá que la tabla "books" ya no está disponible:

```bash
USE books_ks;
DESCRIBE tables;
```

## <a name="next-steps"></a>Pasos siguientes

Después de crear el espacio de claves y la tabla, lea los siguientes artículos referentes a las operaciones CRUD y mucho más:
 
* [Create/insert operations](cassandra-spark-create-ops.md) (Operaciones de creación e inserción)  
* [Read operations](cassandra-spark-read-ops.md) (Operaciones de lectura)  
* [Upsert operations](cassandra-spark-upsert-ops.md) (Operaciones de upsert)  
* [Delete operations](cassandra-spark-delete-ops.md) (Operaciones de eliminación)  
* [Aggregation operations](cassandra-spark-aggregation-ops.md) (Operaciones de agregación)  
* [Table copy operations](cassandra-spark-table-copy-ops.md) (Operaciones de copia en tabla)  
