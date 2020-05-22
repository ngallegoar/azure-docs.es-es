---
title: Características de Apache Cassandra admitidas por Cassandra API de Azure Cosmos DB
description: Obtenga información sobre la compatibilidad con las características de Apache Cassandra en Cassandra API de Azure Cosmos DB
author: kanshiG
ms.author: govindk
ms.reviewer: sngun
ms.service: cosmos-db
ms.subservice: cosmosdb-cassandra
ms.topic: overview
ms.date: 09/24/2018
ms.openlocfilehash: 978dbf3d8e6a92242c0a984b26bb35cf911a3369
ms.sourcegitcommit: bb0afd0df5563cc53f76a642fd8fc709e366568b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/19/2020
ms.locfileid: "83590439"
---
# <a name="apache-cassandra-features-supported-by-azure-cosmos-db-cassandra-api"></a>Características de Apache Cassandra admitidas por Cassandra API de Azure Cosmos DB 

Azure Cosmos DB es un servicio de base de datos con varios modelos y de distribución global de Microsoft. Se puede comunicar con Cassandra API de Azure Cosmos DB con los [controladores](https://cassandra.apache.org/doc/latest/getting_started/drivers.html?highlight=driver) de cliente de Cassandra, de código abierto y compatibles con el [protocolo de conexión](https://github.com/apache/cassandra/blob/trunk/doc/native_protocol_v4.spec) del lenguaje de consulta Cassandra (CQL) v4. 

Con Cassandra API de Azure Cosmos DB, puede disfrutar de las ventajas de las API de Apache Cassandra, así como de las funcionalidades empresariales que ofrece Azure Cosmos DB. Entre las funcionalidades empresariales se incluyen las siguientes: [distribución global](distribute-data-globally.md), [creación de particiones de escalado horizontal automático](partition-data.md), garantías de disponibilidad y latencia, cifrado en reposo y copias de seguridad, entre otras.

## <a name="cassandra-protocol"></a>Protocolo Cassandra 

Cassandra API de Azure Cosmos DB es compatible con CQL versión **v4**. A continuación se enumeran los comandos, las herramientas, las limitaciones y las excepciones de CQL. Cualquier controlador de cliente que reconozca estos protocolos podrá conectarse a Cassandra API de Azure Cosmos DB.

## <a name="cassandra-driver"></a>Controlador Cassandra

Cassandra API de Azure Cosmos DB admite las siguientes versiones de los controladores Cassandra:

* [Java 3.5 y versiones posteriores](https://github.com/datastax/java-driver)  
* [C# 3.5 y versiones posteriores](https://github.com/datastax/csharp-driver)  
* [Nodejs 3.5 y versiones posteriores](https://github.com/datastax/nodejs-driver)  
* [Python 3.15 y versiones posteriores](https://github.com/datastax/python-driver)  
* [C++ 2.9](https://github.com/datastax/cpp-driver)   
* [PHP 1.3](https://github.com/datastax/php-driver)  
* [Gocql](https://github.com/gocql/gocql)  
 
## <a name="cql-data-types"></a>Tipos de datos CQL 

Cassandra API de Azure Cosmos DB admite los siguientes tipos de datos CQL:

* ascii  
* bigint  
* blob  
* boolean  
* counter  
* date  
* Decimal  
* double  
* FLOAT  
* frozen  
* inet  
* int  
* list  
* set  
* SMALLINT  
* text  
* time  
* timestamp  
* timeuuid  
* TINYINT  
* tuple  
* uuid  
* varchar  
* varint  
* tuples  
* udts  
* map  

## <a name="cql-functions"></a>Funciones de CQL

Cassandra API de Azure Cosmos DB admite las siguientes funciones de CQL:

* Token  
* Funciones de agregado
  * min, max, avg, count
* Funciones de conversión de blobs 
  * typeAsBlob(value)  
  * blobAsType(value)
* Funciones UUID y timeuuid 
  * dateOf()  
  * now()  
  * minTimeuuid()  
  * unixTimestampOf()  
  * toDate(timeuuid)  
  * toTimestamp(timeuuid)  
  * toUnixTimestamp(timeuuid)  
  * toDate(timestamp)  
  * toUnixTimestamp(timestamp)  
  * toTimestamp(date)  
  * toUnixTimestamp(date) 
  


## <a name="cassandra-api-limits"></a>Límites de Cassandra API

Cassandra API de Azure Cosmos DB no tiene ningún límite en cuanto al tamaño de los datos almacenados en una tabla. Se pueden almacenar cientos de terabytes o petabytes de datos, con la seguridad de que se cumplirán los límites de la clave de partición. Del mismo modo, cada entidad o fila equivalente no tiene ningún límite en el número de columnas. Aunque el tamaño total de la entidad no debe superar los 2 MB. Los datos por clave de partición no pueden superar los 20 GB, al igual que en todas las demás API.

## <a name="tools"></a>Herramientas 

Cassandra API de Azure Cosmos DB es una plataforma de servicio administrada. No requiere ninguna administración adicional ni utilidades tales como el recolector de elementos no utilizados, Java Virtual Machine (JVM) o nodetool para administrar el clúster. Admite herramientas como cqlsh, que utiliza la compatibilidad con archivos binarios de CQLv4. 

* Para administrar la cuenta se admiten otros mecanismos, como el explorador de datos de Azure Portal, las métricas, los diagnósticos de registro, PowerShell y la CLI.

## <a name="hosted-cql-shell-preview"></a>Shell de CQL hospedado (versión preliminar)

Puede abrir un shell de Cassandra nativo hospedado (CQLSH v5.0.1) directamente desde el Explorador de datos en [Azure Portal](data-explorer.md) o el [Explorador de Azure Cosmos](https://cosmos.azure.com/). Antes de habilitar el shell de CQL, debe [habilitar la característica Notebooks](enable-notebooks.md) en su cuenta (si aún no está habilitada, se le pedirá que lo haga al hacer clic en `Open Cassandra Shell`). Compruebe la nota resaltada en [Habilitación de Notebooks para las cuentas de Azure Cosmos DB](enable-notebooks.md) para ver las regiones de Azure admitidas.

![CQLSH](./media/cassandra-support/cqlsh.png)

También puede conectarse a Cassandra API en Azure Cosmos DB mediante el CQLSH instalado en un equipo local. Viene con Apache Cassandra 3.1.1, y funciona de serie estableciendo las variables de entorno. En las secciones siguientes se incluyen las instrucciones para instalar, configurar y conectarse a Cassandra API en Azure Cosmos DB, en Windows o Linux con CQLSH.

**Windows:**

Si usa Windows, se recomienda habilitar el [sistema de archivos de Windows para Linux](https://docs.microsoft.com/windows/wsl/install-win10#install-the-windows-subsystem-for-linux). Después, puede seguir los comandos de Linux a continuación.

**Unix/Linux/Mac:**

```bash
# Install default-jre and default-jdk
sudo apt install default-jre
sudo apt-get update
sudo apt install default-jdk

# Import the Baltimore CyberTrust root certificate:
curl https://cacert.omniroot.com/bc2025.crt > bc2025.crt
keytool -importcert -alias bc2025ca -file bc2025.crt

# Install the Cassandra libraries in order to get CQLSH:
echo "deb http://www.apache.org/dist/cassandra/debian 311x main" | sudo tee -a /etc/apt/sources.list.d/cassandra.sources.list
curl https://downloads.apache.org/cassandra/KEYS | sudo apt-key add -
sudo apt-get update
sudo apt-get install cassandra

# Export the SSL variables:
export SSL_VERSION=TLSv1_2
export SSL_VALIDATE=false

# Connect to Azure Cosmos DB API for Cassandra:
cqlsh <YOUR_ACCOUNT_NAME>.cassandra.cosmosdb.azure.com 10350 -u <YOUR_ACCOUNT_NAME> -p <YOUR_ACCOUNT_PASSWORD> --ssl

```

## <a name="cql-commands"></a>Comandos de CQL

Azure Cosmos DB admite los siguientes comandos de base de datos en las cuentas de Cassandra API.

* CREATE KEYSPACE (se omite la configuración de replicación para este comando)
* CREATE TABLE 
* CREATE INDEX (sin especificar el nombre del índice; aún no se admiten los índices inmovilizados completos)
* ALLOW FILTERING
* ALTER TABLE 
* USE 
* INSERT 
* SELECT 
* UPDATE 
* BATCH (solo se admiten los comandos no registrados) 
* Delete

Cuando se ejecutan mediante un SDK compatible con CQL v4, todas las operaciones CRUD devuelven información adicional sobre el error o las unidades de solicitud consumidas. Los comandos DELETE y UPDATE se deben controlar teniendo en cuenta la gobernanza de recursos para garantizar el uso más eficaz del rendimiento aprovisionado.

* Tenga en cuenta que el valor gc_grace_seconds debe ser cero si se especifica.

```csharp
var tableInsertStatement = table.Insert(sampleEntity); 
var insertResult = await tableInsertStatement.ExecuteAsync(); 
 
foreach (string key in insertResult.Info.IncomingPayload) 
        { 
            byte[] valueInBytes = customPayload[key]; 
            double value = Encoding.UTF8.GetString(valueInBytes); 
            Console.WriteLine($"CustomPayload:  {key}: {value}"); 
        } 
```

## <a name="consistency-mapping"></a>Asignación de coherencia 

Cassandra API de Azure Cosmos DB ofrece opciones de coherencia para las operaciones de lectura.  La asignación de coherencia se detalla [aquí](consistency-levels-across-apis.md#cassandra-mapping).

## <a name="permission-and-role-management"></a>Administración de permisos y roles

Azure Cosmos DB admite el control de acceso basado en rol (RBAC) para el aprovisionamiento, la rotación de claves, la vista de las métricas y las claves o contraseñas de lectura y escritura y de solo lectura que se pueden obtener a través de [Azure Portal](https://portal.azure.com). Azure Cosmos DB no admite roles para las actividades de CRUD.

## <a name="keyspace-and-table-options"></a>Opciones de espacio de claves y de tabla

Actualmente se omiten las opciones de nombre de región, clase, factor de replicación y centro de datos del comando "Create Keyspace". El sistema utiliza el método de replicación de [distribución global](global-dist-under-the-hood.md) subyacente de Azure Cosmos DB para agregar las regiones. Si necesita la presencia de datos entre regiones, puede habilitarla en el nivel de cuenta con PowerShell, la CLI o el portal. Para más información, consulte el artículo sobre [cómo agregar regiones](how-to-manage-database-account.md#addremove-regions-from-your-database-account). Durable_writes no se puede deshabilitar porque Azure Cosmos DB garantiza que todas las escrituras son duraderas. En todas las regiones, Azure Cosmos DB replica los datos en el conjunto de réplicas que se compone de cuatro réplicas, y la [configuración](global-dist-under-the-hood.md) de este conjunto no se puede modificar.
 
Todas las opciones se omiten al crear la tabla, excepto gc_grace_seconds, que debe establecerse en cero.
El espacio de claves y la tabla tienen una opción adicional denominada "cosmosdb_provisioned_throughput" con un valor mínimo de 400 RU/s. El rendimiento del espacio de claves permite compartir el rendimiento entre varias tablas y resulta útil para los escenarios en los que no todas las tablas usan el rendimiento aprovisionado. El comando ALTER TABLE permite cambiar el rendimiento aprovisionado en todas las regiones. 

```
CREATE  KEYSPACE  sampleks WITH REPLICATION = {  'class' : 'SimpleStrategy'}   AND cosmosdb_provisioned_throughput=2000;  

CREATE TABLE sampleks.t1(user_id int PRIMARY KEY, lastname text) WITH cosmosdb_provisioned_throughput=2000; 

ALTER TABLE gks1.t1 WITH cosmosdb_provisioned_throughput=10000 ;

```


## <a name="usage-of-cassandra-retry-connection-policy"></a>Uso de la directiva de reintentos de conexión de Cassandra

Azure Cosmos DB es un sistema gobernado por recursos. Esto significa que puede realizar un determinado número de operaciones en un segundo determinado en función de las unidades de solicitud utilizadas por las operaciones. Si una aplicación supera ese límite en un segundo determinado, como las solicitudes tienen una frecuencia limitada, se producirán excepciones. Cassandra API de Azure Cosmos DB traslada estas excepciones como errores sobrecargados en el protocolo nativo de Cassandra. Para asegurarse de que la aplicación pueda interceptar y reintentar las solicitudes en caso de limitación de frecuencia, se proporcionan las extensiones de [Spark](https://mvnrepository.com/artifact/com.microsoft.azure.cosmosdb/azure-cosmos-cassandra-spark-helper) y [Java](https://github.com/Azure/azure-cosmos-cassandra-extensions). Vea también ejemplos de código de Java para la [versión 3](https://github.com/Azure-Samples/azure-cosmos-cassandra-java-retry-sample) y la [versión 4](https://github.com/Azure-Samples/azure-cosmos-cassandra-java-retry-sample-v4) de los controladores Datastax al conectarse a Cassandra API en Azure Cosmos DB. Si usa otros SDK para acceder a Cassandra API en Azure Cosmos DB, cree una directiva de conexión para volver a intentarlo después de estas excepciones.

## <a name="next-steps"></a>Pasos siguientes

- Introducción a la [creación de una cuenta de Cassandra API, una base de datos y una tabla](create-cassandra-api-account-java.md) usando una aplicación de Java
