---
title: Supervisión de datos de Azure Cosmos DB mediante la configuración de diagnóstico de Azure
description: Aprenda a usar la configuración de diagnóstico de Azure para supervisar el rendimiento y la disponibilidad de los datos almacenados en Azure Cosmos DB
author: SnehaGunda
services: cosmos-db
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 12/09/2019
ms.author: sngun
ms.openlocfilehash: 184fc65dae57292243be9abdca71a129512b3d0b
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "78252054"
---
# <a name="monitor-azure-cosmos-db-data-by-using-diagnostic-settings-in-azure"></a>Supervisión de datos de Azure Cosmos DB mediante la configuración de diagnóstico en Azure

La configuración de diagnóstico de Azure se usa para recopilar los registros de los recursos. Dichos registros de recursos de Azure los emite un recurso y proporcionan información detallada y frecuente sobre la operación de dicho recurso. Los registros se capturan por solicitud y también se denominan "registros de plano de datos". Algunos ejemplos de las operaciones del plano de datos son Delete, Insert y ReadFeed. El contenido de estos registros varía según el tipo de recurso.

Las métricas de la plataforma y el registro de actividad se recopilan automáticamente, mientras que debe crear una configuración de diagnóstico para recopilar registros de recursos o reenviarlos fuera de Azure Monitor. Puede activar la configuración de diagnóstico para las cuentas de Azure Cosmos mediante los siguientes pasos:

1. Inicie sesión en el [Portal de Azure](https://portal.azure.com).

1. Vaya a la cuenta de Azure Cosmos. Abra el panel **Configuración de diagnóstico** y, después, seleccione la opción **Agregar configuración de diagnóstico**.

1. En el panel **Configuración de diagnóstico**, rellene el formulario con la información siguiente: 

    * **Name**: Escriba un nombre para los registros que quiere crear.

    * Puede almacenar los registros para **Archivar en una cuenta de almacenamiento**, **Transmitir en secuencias a un centro de eventos** o **Enviar a Log Analytics**.

1. Cuando se crea una configuración de diagnóstico, se especifica qué categoría de registros se va a recopilar. A continuación se enumeran las categorías de registros que admite Azure Cosmos DB, junto con un ejemplo de los registros que recopilan:

 * **DataPlaneRequests**: seleccione esta opción para registrar las solicitudes de back-end a las API que incluyen las cuentas de SQL, Graph, MongoDB, Cassandra y Table API en Azure Cosmos DB. Las propiedades clave que se deben tener en cuenta son: `Requestcharge`, `statusCode`, `clientIPaddress` y `partitionID`.

    ```
    { "time": "2019-04-23T23:12:52.3814846Z", "resourceId": "/SUBSCRIPTIONS/<your_subscription_ID>/RESOURCEGROUPS/<your_resource_group>/PROVIDERS/MICROSOFT.DOCUMENTDB/DATABASEACCOUNTS/<your_database_account>", "category": "DataPlaneRequests", "operationName": "ReadFeed", "properties": {"activityId": "66a0c647-af38-4b8d-a92a-c48a805d6460","requestResourceType": "Database","requestResourceId": "","collectionRid": "","statusCode": "200","duration": "0","userAgent": "Microsoft.Azure.Documents.Common/2.2.0.0","clientIpAddress": "10.0.0.24","requestCharge": "1.000000","requestLength": "0","responseLength": "372","resourceTokenUserRid": "","region": "East US","partitionId": "062abe3e-de63-4aa5-b9de-4a77119c59f8","keyType": "PrimaryReadOnlyMasterKey","databaseName": "","collectionName": ""}}
    ```

* **MongoRequests**: seleccione esta opción para registrar las solicitudes iniciadas por el usuario procedentes del front-end con el fin de atender solicitudes para las API de Azure Cosmos DB de MongoDB. Este tipo de registro no está disponible para otras cuentas de API. Las solicitudes de MongoDB aparecerán en MongoRequests así como en DataPlaneRequests. Las propiedades que debe tener en cuenta son las siguientes: `Requestcharge`, `opCode`.

    ```
    { "time": "2019-04-10T15:10:46.7820998Z", "resourceId": "/SUBSCRIPTIONS/<your_subscription_ID>/RESOURCEGROUPS/<your_resource_group>/PROVIDERS/MICROSOFT.DOCUMENTDB/DATABASEACCOUNTS/<your_database_account>", "category": "MongoRequests", "operationName": "ping", "properties": {"activityId": "823cae64-0000-0000-0000-000000000000","opCode": "MongoOpCode_OP_QUERY","errorCode": "0","duration": "0","requestCharge": "0.000000","databaseName": "admin","collectionName": "$cmd","retryCount": "0"}}
    ```

* **QueryRuntimeStatistics**: seleccione esta opción para registrar el texto de la consulta que se ha ejecutado. Este tipo de registro solo está disponible para las cuentas de la API de SQL.

    ```
    { "time": "2019-04-14T19:08:11.6353239Z", "resourceId": "/SUBSCRIPTIONS/<your_subscription_ID>/RESOURCEGROUPS/<your_resource_group>/PROVIDERS/MICROSOFT.DOCUMENTDB/DATABASEACCOUNTS/<your_database_account>", "category": "QueryRuntimeStatistics", "properties": {"activityId": "278b0661-7452-4df3-b992-8aa0864142cf","databasename": "Tasks","collectionname": "Items","partitionkeyrangeid": "0","querytext": "{"query":"SELECT *\nFROM c\nWHERE (c.p1__10 != true)","parameters":[]}"}}
    ```

* **PartitionKeyStatistics**: seleccione esta opción para registrar las estadísticas de las claves de partición. Se representa con el tamaño de almacenamiento (KB) de las claves de partición. Consulte la sección [Solución de problemas mediante las consultas de diagnóstico de Azure](#diagnostic-queries) de este artículo. Por ejemplo, las consultas que usan "PartitionKeyStatistics". El registro se emite con las tres primeras claves de partición que ocupan la mayor parte del almacenamiento de datos. Este registro contiene datos como el identificador de la suscripción, el nombre de la región, el nombre de la base de datos, el nombre de la colección, la clave de partición y el tamaño de almacenamiento en KB.

    ```
    { "time": "2019-10-11T02:33:24.2018744Z", "resourceId": "/SUBSCRIPTIONS/<your_subscription_ID>/RESOURCEGROUPS/<your_resource_group>/PROVIDERS/MICROSOFT.DOCUMENTDB/DATABASEACCOUNTS/<your_database_account>", "category": "PartitionKeyStatistics", "properties": {"subscriptionId": "<your_subscription_ID>","regionName": "West US 2","databaseName": "KustoQueryResults","collectionname": "CapacityMetrics","partitionkey": "["CapacityMetricsPartition.136"]","sizeKb": "2048270"}}
    ```

* **PartitionKeyRUConsumption**: este registro informa del consumo de RU por segundo agregadas de las claves de partición. Actualmente, Azure Cosmos DB solo informa de las claves de partición para las cuentas de la API de SQL y para las operaciones de lectura/escritura y de procedimientos almacenados. No se admiten otras API o tipos de operaciones. Para las otras API, la columna de clave de partición de la tabla del registro de diagnóstico estará vacía. Este registro contiene datos como el identificador de la suscripción, el nombre de la región, el nombre de la base de datos, el nombre de la colección, la clave de partición, el tipo de operación y el cargo de la solicitud. Consulte la sección [Solución de problemas mediante las consultas de diagnóstico de Azure](#diagnostic-queries) de este artículo. Por ejemplo, las consultas que usan "PartitionKeyRUConsumption". 

* **ControlPlaneRequests**: Este registro contiene detalles sobre las operaciones del panel de control, tales como la creación de una cuenta, la adición o eliminación de una región, la actualización de la configuración de réplicas de la cuenta, etc. Este tipo de registro está disponible para todos los tipos de API que incluyen SQL (Core), MongoDB, Gremlin, Cassandra, Table API.

* **Solicitud**: seleccione esta opción para recopilar datos de métricas de Azure Cosmos DB a los destinos en la configuración de diagnóstico. Se trata de los mismos datos que se recopilan automáticamente en métricas de Azure. Recopile datos de métricas con registros de recursos para analizar ambos tipos de datos juntos y para enviar datos de métricas fuera de Azure Monitor.

Para obtener información detallada sobre cómo crear una configuración de diagnóstico mediante Azure Portal, la CLI o PowerShell, consulte el artículo [Creación de una configuración de diagnóstico para recopilar registros de plataforma y métricas en Azure](../azure-monitor/platform/diagnostic-settings.md).


## <a name="troubleshoot-issues-with-diagnostics-queries"></a><a id="diagnostic-queries"></a> Solución de problemas con las consultas de diagnóstico

1. ¿Cómo puedo obtener los cargos de solicitud para las consultas costosas?

   ```Kusto
   AzureDiagnostics
   | where ResourceProvider=="MICROSOFT.DOCUMENTDB" and Category=="DataPlaneRequests" and todouble(requestCharge_s) > 10.0
   | project activityId_g, requestCharge_s
   | join kind= inner (
   AzureDiagnostics
   | where ResourceProvider =="MICROSOFT.DOCUMENTDB" and Category == "QueryRuntimeStatistics"
   | project activityId_g, querytext_s
   ) on $left.activityId_g == $right.activityId_g
   | order by requestCharge_s desc
   | limit 100
   ```

1. ¿Cómo puedo averiguar qué operaciones consumen la mayor parte de las RU por segundo?

    ```Kusto
   AzureDiagnostics
   | where ResourceProvider=="MICROSOFT.DOCUMENTDB" and Category=="DataPlaneRequests"
   | where TimeGenerated >= ago(2h) 
   | summarize max(responseLength_s), max(requestLength_s), max(requestCharge_s), count = count() by OperationName, requestResourceType_s, userAgent_s, collectionRid_s, bin(TimeGenerated, 1h)
   ```
1. ¿Cómo puedo obtener la distribución de distintas operaciones?

   ```Kusto
   AzureDiagnostics
   | where ResourceProvider=="MICROSOFT.DOCUMENTDB" and Category=="DataPlaneRequests"
   | where TimeGenerated >= ago(2h) 
   | summarize count = count()  by OperationName, requestResourceType_s, bin(TimeGenerated, 1h) 
   ```

1. ¿Cuál es el rendimiento máximo que ha consumido una partición?

   ```Kusto
   AzureDiagnostics
   | where ResourceProvider=="MICROSOFT.DOCUMENTDB" and Category=="DataPlaneRequests"
   | where TimeGenerated >= ago(2h) 
   | summarize max(requestCharge_s) by bin(TimeGenerated, 1h), partitionId_g
   ```

1. ¿Cómo puedo obtener la información sobre el consumo de RU por segundo de las claves de partición?

   ```Kusto
   AzureDiagnostics 
   | where ResourceProvider == "MICROSOFT.DOCUMENTDB" and Category == "PartitionKeyRUConsumption" 
   | summarize total = sum(todouble(requestCharge_s)) by databaseName_s, collectionName_s, partitionKey_s, TimeGenerated 
   | order by TimeGenerated asc 
   ```

1. Cómo obtener el cargo de la solicitud de una clave de partición específica

   ```Kusto
   AzureDiagnostics 
   | where ResourceProvider == "MICROSOFT.DOCUMENTDB" and Category == "PartitionKeyRUConsumption" 
   | where parse_json(partitionKey_s)[0] == "2" 
   ```

1. ¿Cómo puedo obtener las principales claves de partición con el mayor número de RU por segundo consumidas en un período específico? 

   ```Kusto
   AzureDiagnostics 
   | where ResourceProvider == "MICROSOFT.DOCUMENTDB" and Category == "PartitionKeyRUConsumption" 
   | where TimeGenerated >= datetime("11/26/2019, 11:20:00.000 PM") and TimeGenerated <= datetime("11/26/2019, 11:30:00.000 PM") 
   | summarize total = sum(todouble(requestCharge_s)) by databaseName_s, collectionName_s, partitionKey_s 
   | order by total desc
    ```

1. ¿Cómo puedo obtener los registros de las claves de partición cuyo tamaño de almacenamiento es superior a los 8 GB?

   ```Kusto
   AzureDiagnostics
   | where ResourceProvider=="MICROSOFT.DOCUMENTDB" and Category=="PartitionKeyStatistics"
   | where todouble(sizeKb_d) > 800000
   ```

1. ¿Cómo puedo obtener las estadísticas de clave de partición para evaluar el sesgo entre las tres primeras particiones de la cuenta de base de datos?

    ```Kusto
    AzureDiagnostics 
    | where ResourceProvider=="MICROSOFT.DOCUMENTDB" and Category=="PartitionKeyStatistics" 
    | project SubscriptionId, regionName_s, databaseName_s, collectionname_s, partitionkey_s, sizeKb_s, ResourceId 
    ```

## <a name="next-steps"></a>Pasos siguientes

* [Azure Monitor para Azure Cosmos DB](../azure-monitor/insights/cosmosdb-insights-overview.md?toc=/azure/cosmos-db/toc.json)
* [Supervisión y depuración con métricas de Azure Cosmos DB](use-metrics.md)
