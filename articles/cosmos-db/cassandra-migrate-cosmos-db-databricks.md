---
title: Migración de datos de Apache Cassandra a Cassandra API de Azure Cosmos DB mediante Databricks (Spark)
description: Aprenda a migrar datos de la base de datos de Apache Cassandra a Cassandra API de Azure Cosmos DB mediante Azure Databricks y Spark.
author: TheovanKraay
ms.service: cosmos-db
ms.subservice: cosmosdb-cassandra
ms.topic: how-to
ms.date: 11/16/2020
ms.author: thvankra
ms.reviewer: thvankra
ms.openlocfilehash: 877aa96b189de47d158721df6585cb94ace4a855
ms.sourcegitcommit: 03c0a713f602e671b278f5a6101c54c75d87658d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/19/2020
ms.locfileid: "94932878"
---
# <a name="migrate-data-from-cassandra-to-azure-cosmos-db-cassandra-api-account-using-azure-databricks"></a>Migración de los datos de Cassandra a una cuenta de Cassandra API de Azure Cosmos DB mediante Azure Databricks
[!INCLUDE[appliesto-cassandra-api](includes/appliesto-cassandra-api.md)]

Cassandra API en Azure Cosmos DB se ha convertido en una excelente opción para cargas de trabajo empresariales que se ejecutan en Apache Cassandra por diversos motivos como, por ejemplo: 

* **No hay sobrecarga de administración y supervisión**: evita la sobrecarga que supone administrar y supervisar innumerables configuraciones en el sistema operativo, JVM, los archivos yaml y sus interacciones.

* **Importante ahorro de costos**: puede ahorrar costos con Azure Cosmos DB, como en el costo de las máquinas virtuales, del ancho de banda y de las licencias aplicables. Además, no tiene que administrar los centros de datos, los servidores, el almacenamiento en SSD, las redes y los costos de electricidad. 

* **Posibilidad de usar código y herramientas existentes**: Azure Cosmos DB proporciona compatibilidad de nivel de protocolo de conexión con SDK y herramientas existentes de Cassandra. Esta compatibilidad garantiza que pueda usar el código base existente con Cassandra API de Azure Cosmos DB con cambios triviales.

Existen varias maneras de migrar las cargas de trabajo de base de datos de una plataforma a otra. [Azure Databricks](https://azure.microsoft.com/services/databricks/) es una oferta de plataforma como servicio para [Apache Spark](https://spark.apache.org/) que proporciona una forma de realizar migraciones sin conexión a gran escala. En este artículo se describen los pasos necesarios para migrar datos de espacios de claves/tablas nativos de Apache Cassandra a Cassandra API de Azure Cosmos DB mediante Azure Databricks.

## <a name="prerequisites"></a>Prerrequisitos

* [Aprovisionar una cuenta de Cassandra API de Azure Cosmos DB](create-cassandra-dotnet.md#create-a-database-account)

* [Revisar los conceptos básicos de la conexión a Cassandra API de Azure Cosmos DB](cassandra-spark-generic.md)

* Revise las [características compatibles en Cassandra API de Azure Cosmos DB](cassandra-support.md) para garantizar la compatibilidad.

* Asegúrese de que ya ha creado un espacio de claves y tablas sin contenido en la cuenta de Cassandra API de Azure Cosmos DB de destino.

* [Uso de cqlsh o shell hospedado para la validación](cassandra-support.md#hosted-cql-shell-preview)

## <a name="provision-an-azure-databricks-cluster"></a>Aprovisionar un clúster de Azure Databricks

Puede seguir las instrucciones para [aprovisionar un clúster de Azure Databricks](/azure/databricks/scenarios/quickstart-create-databricks-workspace-portal). Sin embargo, tenga en cuenta que Apache Spark 3.x no es compatible actualmente con el conector de Apache Cassandra. Tendrá que aprovisionar un entorno de ejecución de Databricks con una versión v2.x compatible de Apache Spark. Se recomienda la versión 6.6 del entorno de ejecución de Databricks:

:::image type="content" source="./media/cassandra-migrate-cosmos-db-databricks/databricks-runtime.png" alt-text="Entorno de ejecución de Databricks":::


## <a name="add-dependencies"></a>Adición de dependencias

Tendrá que agregar la biblioteca de conectores de Cassandra de Apache Spark a su clúster para conectarse a los puntos de conexión nativos y de Cassandra de Cosmos DB. En el clúster, seleccione libraries -> install new -> maven -> search packages (bibliotecas > instalar nueva > maven > buscar paquetes):

:::image type="content" source="./media/cassandra-migrate-cosmos-db-databricks/databricks-search-packages.png" alt-text="Buscar paquetes de Databricks":::

Escriba `Cassandra` en el cuadro de búsqueda y seleccione el repositorio `spark-cassandra-connector` de Maven más reciente disponible y, a continuación, seleccione install (instalar):

:::image type="content" source="./media/cassandra-migrate-cosmos-db-databricks/databricks-search-packages-2.png" alt-text="Seleccionar paquetes de Databricks":::

> [!NOTE]
> Asegúrese de reiniciar el clúster de Databricks después de que se haya instalado la biblioteca de conectores de Cassandra.

## <a name="create-scala-notebook-for-migration"></a>Creación de un cuaderno de Scala para la migración

Cree un cuaderno de Scala en Databricks con el código siguiente. Reemplace las configuraciones de Cassandra de origen y de destino con las credenciales correspondientes, así como los espacios de claves y las tablas de origen y destino, y ejecute lo siguiente:

```scala
import com.datastax.spark.connector._
import com.datastax.spark.connector.cql._
import org.apache.spark.SparkContext

// source cassandra configs
val nativeCassandra = Map( 
    "spark.cassandra.connection.host" -> "<Source Cassandra Host>",
    "spark.cassandra.connection.port" -> "9042",
    "spark.cassandra.auth.username" -> "<USERNAME>",
    "spark.cassandra.auth.password" -> "<PASSWORD>",
    "spark.cassandra.connection.ssl.enabled" -> "false",
    "keyspace" -> "<KEYSPACE>",
    "table" -> "<TABLE>"
)

//target cassandra configs
val cosmosCassandra = Map( 
    "spark.cassandra.connection.host" -> "<USERNAME>.cassandra.cosmos.azure.com",
    "spark.cassandra.connection.port" -> "10350",
    "spark.cassandra.auth.username" -> "<USERNAME>",
    "spark.cassandra.auth.password" -> "<PASSWORD>",
    "spark.cassandra.connection.ssl.enabled" -> "true",
    "keyspace" -> "<KEYSPACE>",
    "table" -> "<TABLE>",
    //throughput related settings below - tweak these depending on data volumes. 
    "spark.cassandra.output.batch.size.rows"-> "1",
    "spark.cassandra.connection.connections_per_executor_max" -> "10",
    "spark.cassandra.output.concurrent.writes" -> "1000",
    "spark.cassandra.concurrent.reads" -> "512",
    "spark.cassandra.output.batch.grouping.buffer.size" -> "1000",
    "spark.cassandra.connection.keep_alive_ms" -> "600000000"
)

//Read from native Cassandra
val DFfromNativeCassandra = sqlContext
  .read
  .format("org.apache.spark.sql.cassandra")
  .options(nativeCassandra)
  .load
  
//Write to CosmosCassandra
DFfromNativeCassandra
  .write
  .format("org.apache.spark.sql.cassandra")
  .options(cosmosCassandra)
  .save
```

> [!NOTE]
> Las configuraciones de `spark.cassandra.output.concurrent.writes` y `connections_per_executor_max` son importantes para evitar la [limitación de velocidad](https://docs.microsoft.com/samples/azure-samples/azure-cosmos-cassandra-java-retry-sample/azure-cosmos-db-cassandra-java-retry-sample/), que sucede cuando las solicitudes a Cosmos DB superan el rendimiento aprovisionado ([unidades de solicitud](https://docs.microsoft.com/azure/cosmos-db/request-units)). Es posible que tenga que ajustar esta configuración en función del número de ejecutores del clúster de Spark y, potencialmente, del tamaño (y, por consiguiente, el costo de unidad de solicitud) de cada registro que se escribe en las tablas de destino.

## <a name="next-steps"></a>Pasos siguientes

* [Aprovisionamiento del rendimiento en contenedores y bases de datos](set-throughput.md) 
* [Procedimientos recomendados de las claves de partición](partitioning-overview.md#choose-partitionkey)
* [Estimación de RU/s mediante Capacity Planner de Azure Cosmos DB](estimate-ru-with-capacity-planner.md)
* [Escala elástica en Cassandra API de Azure Cosmos DB](manage-scale-cassandra.md)
