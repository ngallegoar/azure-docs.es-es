---
title: Referencia de datos de supervisión de Azure Cosmos DB | Microsoft Docs
description: Referencia de registro y métricas para la supervisión de datos de Azure Cosmos DB.
author: bwren
services: azure-monitor
ms.service: azure-monitor
ms.topic: reference
ms.date: 11/11/2019
ms.author: bwren
ms.custom: subject-monitoring
ms.subservice: logs
ms.openlocfilehash: d243224192b5761af45d387690f5fb41b84481e6
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "77588729"
---
# <a name="azure-cosmos-db-monitoring-data-reference"></a>Referencia de datos de supervisión de Azure Cosmos DB
En este artículo se proporciona una referencia de datos de registro y métricas recopilados para analizar el rendimiento y la disponibilidad de Azure Cosmos DB. Consulte [Supervisión de Cosmos DB](monitor-cosmos-db.md) para obtener más información sobre la recopilación y el análisis de datos de supervisión para Azure Cosmos DB.


## <a name="resource-logs"></a>Registros del recurso
En la tabla siguiente se enumeran las propiedades de los registros de recursos de Azure Cosmos DB cuando se recopilan en registros de Azure Monitor o Azure Storage. En los registros de Azure Monitor, se recopilan en la tabla **AzureDiagnostics** con un valor **ResourceProvider** de *MICROSOFT.DOCUMENTDB*. 

| Propiedad o campo de Azure Storage | Propiedad de registros de Azure Monitor | Descripción |
| --- | --- | --- |
| **time** | **TimeGenerated** | La fecha y hora (UTC) cuando se produjo la operación. |
| **resourceId** | **Recurso** | La cuenta de Azure Cosmos DB para la cual los registros están habilitados.|
| **category** | **Categoría** | En el caso de los registros de Azure Cosmos DB, **DataPlaneRequests**, **MongoRequests**, **QueryRuntimeStatistics**, **PartitionKeyStatistics**, **PartitionKeyRUConsumption** y **ControlPlaneRequests** son los tipos de registro disponibles. |
| **operationName** | **OperationName** | Nombre de la operación. Este valor puede ser cualquiera de las siguientes operaciones: Create, Update, Read, ReadFeed, Delete, Replace, Execute, SqlQuery, Query, JSQuery, Head, HeadFeed o Upsert.   |
| **properties** | N/D | El contenido de este campo se describe en las filas siguientes. |
| **activityId** | **activityId_g** | GUID único para la operación registrada. |
| **userAgent** | **userAgent_s** | Una cadena que especifica el agente de usuario de cliente que realiza la solicitud. El formato es {nombre de agente de usuario}/{versión}.|
| **requestResourceType** | **requestResourceType_s** | Tipo de recurso al que se accede. Este valor puede ser cualquiera de los tipos de recursos siguientes: Database, Container, Document, Attachment, User, Permission, StoredProcedure, Trigger, UserDefinedFunction u Offer. |
| **statusCode** | **statusCode_s** | Estado de respuesta de la operación. |
| **requestResourceId** | **ResourceId** | El valor resourceId que pertenece a la solicitud. El valor puede apuntar a databaseRid, collectionRid o documentRid, según la operación realizada.|
| **clientIpAddress** | **clientIpAddress_s** | Dirección IP del cliente. |
| **requestCharge** | **requestCharge_s** | El número de RU que se utiliza en la operación |
| **collectionRid** | **collectionId_s** | Identificador único de la colección.|
| **duration** | **duration_s** | Duración de la operación en milisegundos. |
| **requestLength** | **requestLength_s** | Longitud de la solicitud, en bytes. |
| **responseLength** | **responseLength_s** | Longitud de la respuesta, en bytes.|
| **resourceTokenUserRid** | **resourceTokenUserRid_s** | Este valor no está vacío cuando se usan [tokens de recursos](https://docs.microsoft.com/azure/cosmos-db/secure-access-to-data#resource-tokens) para la autenticación. Los puntos de valor para el identificador de recurso del usuario. |

Para obtener una lista de todas las categorías de registros de Azure Monitor y los vínculos a los esquemas asociados, consulte [Categorías y esquemas de los registros de Azure Monitor](../azure-monitor/platform/diagnostic-logs-schema.md). 

## <a name="metrics"></a>Métricas
En las tablas siguientes se enumeran las métricas de plataforma recopiladas para Azure Cosmos DB. Todas las métricas se almacenan en el espacio de nombres de las **métricas estándar de Cosmos DB**.

Para obtener una lista de todas las métricas compatibles de Azure Monitor (incluido CosmosDB), consulte [Métricas compatibles con Azure Monitor](../azure-monitor/platform/metrics-supported.md). 

#### <a name="request-metrics"></a>Solicitud de métricas
            
|Métrica (nombre para mostrar de la métrica)|Unidad (tipo de agregación) |Descripción|Dimensions| Granularidades de tiempo| Asignación de métricas heredadas | Uso |
|---|---|---|---| ---| ---| ---|
| TotalRequests (solicitudes totales) | Count (recuento) | Número de solicitudes realizadas| DatabaseName, CollectionName, Region, StatusCode| All | TotalRequests, Http 2xx, Http 3xx, Http 400, Http 401, error interno del servidor, servicio no disponible, solicitudes limitadas, promedio de solicitudes por segundo | Se usa para supervisar las solicitudes por código de estado y contenedor en una granularidad por minuto. Para obtener el promedio de solicitudes por segundo, utilice la agregación Count en un minuto y divida por 60. |
| MetadataRequests (solicitudes de metadatos) |Count (recuento) | Recuento de las solicitudes de metadatos. Azure Cosmos DB mantiene un contenedor de metadatos del sistema para cada cuenta, lo que permite enumerar las colecciones, las bases de datos, etc., así como sus configuraciones de forma gratuita. | DatabaseName, CollectionName, Region, StatusCode| All| |Se usa para supervisar las limitaciones debido a las solicitudes de metadatos.|
| MongoRequests (solicitudes de Mongo) | Count (recuento) | Número de solicitudes de Mongo realizadas | DatabaseName, CollectionName, Region, CommandName, ErrorCode| All |Tasa de solicitudes de consultas de Mongo, tasa de solicitudes de actualización de Mongo, tasa de solicitudes de eliminación de Mongo, tasa de solicitudes de inserción de Mongo y tasa de solicitudes de recuento de Mongo| Se usa para supervisar los errores de solicitud de Mongo y los usos por de comando. |

#### <a name="request-unit-metrics"></a>Métricas de unidad de solicitud

|Métrica (nombre para mostrar de la métrica)|Unidad (tipo de agregación)|Descripción|Dimensions| Granularidades de tiempo| Asignación de métricas heredadas | Uso |
|---|---|---|---| ---| ---| ---|
| MongoRequestCharge (cargo de solicitud de Mongo) | Count (total) |Unidades de la solicitud de Mongo consumidas| DatabaseName, CollectionName, Region, CommandName, ErrorCode| All |Cargo de solicitudes de consultas de Mongo, cargo de solicitudes de actualización de Mongo, cargo de solicitudes de eliminación de Mongo, cargo de solicitudes de inserción de Mongo y cargo de solicitudes de recuento de Mongo| Se usa para supervisar las RU de recursos de Mongo en un minuto.|
| TotalRequestUnits (unidades de solicitud totales)| Count (total) | Unidades de solicitud consumidas| DatabaseName, CollectionName, Region, StatusCode |All| TotalRequestUnits| Se usa para supervisar el uso de RU total en una granularidad por minuto. Para obtener el promedio de RU consumidas por segundo, utilice la agregación Total en un minuto y divida por 60.|
| ProvisionedThroughput (rendimiento aprovisionado)| Count (máximo) |Rendimiento aprovisionado en la granularidad del contenedor| DatabaseName, ContainerName| 5M| | Se usa para supervisar el rendimiento aprovisionado por contenedor.|

#### <a name="storage-metrics"></a>Métricas de almacenamiento

|Métrica (nombre para mostrar de la métrica)|Unidad (tipo de agregación)|Descripción|Dimensions| Granularidades de tiempo| Asignación de métricas heredadas | Uso |
|---|---|---|---| ---| ---| ---|
| AvailableStorage (almacenamiento disponible) |Bytes (total) | Almacenamiento total disponible notificada a una granularidad de cinco minutos por región| DatabaseName, CollectionName, Region| 5M| Almacenamiento disponible| Se usa para supervisar la capacidad de almacenamiento disponible (aplicable solo para las colecciones de almacenamiento fijo). La granularidad mínima debe ser de cinco minutos.| 
| DataUsage (uso de datos) |Bytes (total) |Uso total de datos notificado a una granularidad de cinco minutos por región| DatabaseName, CollectionName, Region| 5M |Tamaño de los datos | Se utiliza para supervisar el uso total de los datos en el contenedor y la región, la granularidad mínima debe ser 5 minutos.|
| IndexUsage (uso de índices) | Bytes (total) |Uso total del índice notificado a una granularidad de cinco minutos por región| DatabaseName, CollectionName, Region| 5M| Tamaño de índice| Se utiliza para supervisar el uso total de los datos en el contenedor y la región, la granularidad mínima debe ser 5 minutos. |
| DocumentQuota (cuota de documento) | Bytes (total) | Cuota total de almacenamiento notificado a una granularidad de cinco minutos por región.| DatabaseName, CollectionName, Region| 5M |Capacidad de almacenamiento| Se utiliza para supervisar la cuota total en el contenedor y la región, la granularidad mínima debe ser 5 minutos.|
| DocumentCount (recuento de documentos) | Count (total) |Recuento total de documentos notificado a una granularidad de cinco minutos por región| DatabaseName, CollectionName, Region| 5M |Recuento de documentos|Se utiliza para supervisar el número de documentos en el contenedor y la región, la granularidad mínima debe ser 5 minutos.|

#### <a name="latency-metrics"></a>Métricas de latencia

|Métrica (nombre para mostrar de la métrica)|Unidad (tipo de agregación)|Descripción|Dimensions| Granularidades de tiempo| Uso |
|---|---|---|---| ---| ---|
| ReplicationLatency (latencia de replicación)| Milliseconds (mínimo, máximo, promedio) | Latencia de replicación P99 entre regiones de origen y destino para la cuenta habilitada geográficamente| SourceRegion, TargetRegion| All | Se usa para supervisar la latencia de replicación P99 entre dos regiones cualesquiera para una cuenta replicada geográficamente. |
| Latencia del servidor| Milisegundos (promedio) | Tiempo que tarda el servidor en procesar la solicitud. | CollectionName, ConnectionMode, DatabaseName, OperationType, PublicAPIType, Region | All | Se usa para supervisar la latencia de las solicitudes en el servidor de Azure Cosmos DB. |



#### <a name="availability-metrics"></a>Métricas de disponibilidad

|Métrica (nombre para mostrar de la métrica) |Unidad (tipo de agregación)|Descripción| Granularidades de tiempo| Asignación de métricas heredadas | Uso |
|---|---|---|---| ---| ---|
| ServiceAvailability (disponibilidad del servicio)| Percent (mínimo, máximo) | Solicitudes de cuenta en una granularidad por hora| 1H | Disponibilidad del servicio | Representa el porcentaje de solicitudes totales pasadas. Se considera que una solicitud ha dado error debido a un error del sistema si el código de estado es 410, 500 o 503. Se utiliza para supervisar la disponibilidad de la cuenta en una granularidad por hora. |


#### <a name="cassandra-api-metrics"></a>Métricas de Cassandra API

|Métrica (nombre para mostrar de la métrica)|Unidad (tipo de agregación)|Descripción|Dimensions| Granularidades de tiempo| Uso |
|---|---|---|---| ---| ---|
| CassandraRequests (solicitudes de Cassandra) | Count (recuento) | Número de solicitudes de Cassandra API realizadas| DatabaseName, CollectionName, ErrorCode, Region, OperationType, ResourceType| All| Se usa para supervisar las solicitudes de Cassandra en una granularidad por minuto. Para obtener el promedio de solicitudes por segundo, utilice la agregación Count en un minuto y divida por 60.|
| CassandraRequestsCharges (cargos de solicitudes de Cassandra) | Count (suma, mínimo, máximo, promedio) | Unidades de solicitud consumidas por solicitudes de Cassandra API| DatabaseName, CollectionName, Region, OperationType, ResourceType| All| Se usa para supervisar las RU por minuto por una cuenta de Cassandra API.|
| CassandraConnectionClosures (cierres de conexión de Cassandra) |Count (recuento) |Número de conexiones de Cassandra cerradas| ClosureReason, Region| All | Se usa para supervisar la conectividad entre los clientes y Cassandra API de Azure Cosmos DB.|

## <a name="see-also"></a>Consulte también

- Consulte [Supervisión de Azure Cosmos DB](monitor-cosmos-db.md) para obtener una descripción de la supervisión de Azure Cosmos DB.
- Para más información sobre la supervisión de recursos de Azure, consulte [Supervisión de recursos de Azure con Azure Monitor](../azure-monitor/insights/monitor-azure-resource.md).
