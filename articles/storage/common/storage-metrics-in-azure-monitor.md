---
title: Métricas de Azure Storage en Azure Monitor | Microsoft Docs
description: Obtenga información acerca de las nuevas métricas que ofrece Azure Monitor.
author: normesta
ms.service: storage
ms.topic: conceptual
ms.date: 09/05/2017
ms.author: normesta
ms.reviewer: fryu
ms.subservice: common
ms.openlocfilehash: 73181222bf3f15bbbac24fc253eddfea1c57bc6c
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "80247102"
---
# <a name="azure-storage-metrics-in-azure-monitor"></a>Métricas de Azure Storage en Azure Monitor

Con las métricas de Azure Storage puede analizar tendencias de uso, hacer un seguimiento de las solicitudes y diagnosticar los problemas de su cuenta de almacenamiento.

Azure Monitor proporciona interfaces de usuario unificadas para la supervisión de distintos servicios de Azure. Para más información, consulte [Azure Monitor](../../monitoring-and-diagnostics/monitoring-overview.md). Azure Storage integra Azure Monitor mediante el envío de datos de métricas a la plataforma de Azure Monitor.

## <a name="access-metrics"></a>Acceso a la métrica

Azure Monitor proporciona varias maneras de tener acceso a las métricas. Se puede acceder a ellas desde [Azure Portal](https://portal.azure.com), las API de Azure Monitor (REST y .NET) y soluciones de análisis, como Event Hubs. Para más información, consulte [Información general sobre las métricas en Microsoft Azure](../../monitoring-and-diagnostics/monitoring-overview-metrics.md).

De forma predeterminada, las métricas están habilitadas y puede acceder a datos de los últimos 93 días. Si es necesario conservar los datos durante un periodo mayor, se pueden archivar en una cuenta de Azure Storage. Esto se configura en la [configuración de diagnóstico](../../azure-monitor/platform/platform-logs-overview.md) de Azure Monitor.

### <a name="access-metrics-in-the-azure-portal"></a>Métricas de Azure en Azure Portal

Una vez transcurrido un tiempo, las métricas se pueden supervisar en Azure Portal. A continuación puede ver un ejemplo en el que se muestra cómo ver **Transacciones** a nivel de cuenta.

![captura de pantalla de acceso a métricas desde Azure Portal](./media/storage-metrics-in-azure-monitor/access-metrics-in-portal.png)

Para ver las métricas que admiten dimensiones, puede filtrar las métricas con el valor de dimensión deseado. En el ejemplo siguiente se muestra cómo ver **transacciones** en el nivel de cuenta en una operación específica mediante la selección de los valores de la dimensión **Nombre de API**.

![captura de pantalla de acceso a métricas con dimensión desde Azure Portal](./media/storage-metrics-in-azure-monitor/access-metrics-in-portal-with-dimension.png)

### <a name="access-metrics-with-the-rest-api"></a>Acceso a métricas con la API de REST

Azure Monitor proporciona [API de REST](/rest/api/monitor/) para leer la definición y los valores de las métricas. En esta sección se muestra cómo leer las métricas de almacenamiento. El identificador de recurso se usa en todas las API de REST. Para más información, lea Descripción del identificador de recursos en los servicios de Storage.

En el ejemplo siguiente se muestra cómo usar [ArmClient](https://github.com/projectkudu/ARMClient) en la línea de comandos para simplificar la realización de pruebas con la API de REST.

#### <a name="list-account-level-metric-definition-with-the-rest-api"></a>Lista de definición de métricas a nivel de cuenta con la API de REST

En el ejemplo siguiente se muestra cómo enumerar la definición de métricas a nivel de cuenta:

```
# Login to Azure and enter your credentials when prompted.
> armclient login

> armclient GET /subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Storage/storageAccounts/{storageAccountName}/providers/microsoft.insights/metricdefinitions?api-version=2018-01-01

```

Si desea enumerar las definiciones de métricas de un blob, una tabla, un archivo o una cola, debe especificar identificadores de recursos diferentes para cada servicio con la API.

La respuesta contiene la definición de métricas en formato JSON:

```Json
{
  "value": [
    {
      "id": "/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Storage/storageAccounts/{storageAccountName}/providers/microsoft.insights/metricdefinitions/UsedCapacity",
      "resourceId": "/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Storage/storageAccounts/{storageAccountName}",
      "category": "Capacity",
      "name": {
        "value": "UsedCapacity",
        "localizedValue": "Used capacity"
      },
      "isDimensionRequired": false,
      "unit": "Bytes",
      "primaryAggregationType": "Average",
      "metricAvailabilities": [
        {
          "timeGrain": "PT1M",
          "retention": "P30D"
        },
        {
          "timeGrain": "PT1H",
          "retention": "P30D"
        }
      ]
    },
    ... next metric definition
  ]
}

```

#### <a name="read-account-level-metric-values-with-the-rest-api"></a>Lectura de valores de métricas a nivel de cuenta con la API de REST

En el ejemplo siguiente se muestra cómo leer los datos de métricas a nivel de cuenta:

```
> armclient GET "/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Storage/storageAccounts/{storageAccountName}/providers/microsoft.insights/metrics?metricnames=Availability&api-version=2018-01-01&aggregation=Average&interval=PT1H"

```

En el ejemplo anterior, si desea leer los valores de métricas de un blob, una tabla, un archivo o una cola, debe especificar identificadores de recursos diferente para cada servicio con la API.

La respuesta siguiente contiene valores de métricas en formato JSON:

```Json
{
  "cost": 0,
  "timespan": "2017-09-07T17:27:41Z/2017-09-07T18:27:41Z",
  "interval": "PT1H",
  "value": [
    {
      "id": "/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Storage/storageAccounts/{storageAccountName}/providers/Microsoft.Insights/metrics/Availability",
      "type": "Microsoft.Insights/metrics",
      "name": {
        "value": "Availability",
        "localizedValue": "Availability"
      },
      "unit": "Percent",
      "timeseries": [
        {
          "metadatavalues": [],
          "data": [
            {
              "timeStamp": "2017-09-07T17:27:00Z",
              "average": 100.0
            }
          ]
        }
      ]
    }
  ]
}

```

### <a name="access-metrics-with-the-net-sdk"></a>Acceso a métricas con el SDK de .NET

Azure Monitor proporciona el [SDK de .NET](https://www.nuget.org/packages/Microsoft.Azure.Management.Monitor/) para leer la definición y los valores de las métricas. El [código de ejemplo](https://azure.microsoft.com/resources/samples/monitor-dotnet-metrics-api/) muestra cómo utilizar el SDK con parámetros diferentes. Debe usar `0.18.0-preview` o una versión posterior para las métricas de almacenamiento. El identificador de recurso se usa en el SDK de .NET. Para más información, lea Descripción del identificador de recursos en los servicios de Storage.

En el ejemplo siguiente se muestra cómo usar el SDK de .NET de Azure Monitor para leer las métricas de almacenamiento.

#### <a name="list-account-level-metric-definition-with-the-net-sdk"></a>Lista de definición de métricas a nivel de cuenta con el SDK de .NET

En el ejemplo siguiente se muestra cómo enumerar la definición de métricas a nivel de cuenta:

```csharp
    public static async Task ListStorageMetricDefinition()
    {
        // Resource ID for storage account
        var resourceId = "/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Storage/storageAccounts/{storageAccountName}";
        var subscriptionId = "{SubscriptionID}";
        // How to identify Tenant ID, Application ID and Access Key: https://azure.microsoft.com/documentation/articles/resource-group-create-service-principal-portal/
        var tenantId = "{TenantID}";
        var applicationId = "{ApplicationID}";
        var accessKey = "{AccessKey}";

        // Using metrics in Azure Monitor is currently free. However, if you use additional solutions ingesting metrics data, you may be billed by these solutions. For example, you are billed by Azure Storage if you archive metrics data to an Azure Storage account. Or you are billed by Operation Management Suite (OMS) if you stream metrics data to OMS for advanced analysis.
        MonitorManagementClient readOnlyClient = AuthenticateWithReadOnlyClient(tenantId, applicationId, accessKey, subscriptionId).Result;
        IEnumerable<MetricDefinition> metricDefinitions = await readOnlyClient.MetricDefinitions.ListAsync(resourceUri: resourceId, cancellationToken: new CancellationToken());

        foreach (var metricDefinition in metricDefinitions)
        {
            //Enumrate metric definition:
            //    Id
            //    ResourceId
            //    Name
            //    Unit
            //    MetricAvailabilities
            //    PrimaryAggregationType
            //    Dimensions
            //    IsDimensionRequired
        }
    }

```

Si desea enumerar las definiciones de métricas de un blob, una tabla, un archivo o una cola, debe especificar identificadores de recursos diferentes para cada servicio con la API.

#### <a name="read-metric-values-with-the-net-sdk"></a>Lectura de los valores de métrica con el SDK de .NET

En el ejemplo siguiente se muestra cómo leer los datos de `UsedCapacity` a nivel de cuenta:

```csharp
    public static async Task ReadStorageMetricValue()
    {
        // Resource ID for storage account
        var resourceId = "/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Storage/storageAccounts/{storageAccountName}";
        var subscriptionId = "{SubscriptionID}";
        // How to identify Tenant ID, Application ID and Access Key: https://azure.microsoft.com/documentation/articles/resource-group-create-service-principal-portal/
        var tenantId = "{TenantID}";
        var applicationId = "{ApplicationID}";
        var accessKey = "{AccessKey}";

        MonitorClient readOnlyClient = AuthenticateWithReadOnlyClient(tenantId, applicationId, accessKey, subscriptionId).Result;

        Microsoft.Azure.Management.Monitor.Models.Response Response;

        string startDate = DateTime.Now.AddHours(-3).ToUniversalTime().ToString("o");
        string endDate = DateTime.Now.ToUniversalTime().ToString("o");
        string timeSpan = startDate + "/" + endDate;

        Response = await readOnlyClient.Metrics.ListAsync(
            resourceUri: resourceId,
            timespan: timeSpan,
            interval: System.TimeSpan.FromHours(1),
            metricnames: "UsedCapacity",

            aggregation: "Average",
            resultType: ResultType.Data,
            cancellationToken: CancellationToken.None);

        foreach (var metric in Response.Value)
        {
            //Enumrate metric value
            //    Id
            //    Name
            //    Type
            //    Unit
            //    Timeseries
            //        - Data
            //        - Metadatavalues
        }
    }

```

En el ejemplo anterior, si desea leer los valores de métricas de un blob, una tabla, un archivo o una cola, debe especificar identificadores de recursos diferente para cada servicio con la API.

#### <a name="read-multi-dimensional-metric-values-with-the-net-sdk"></a>Lectura de valores de métrica multidimensionales con el SDK de .NET

En el caso de las métricas multidimensionales, debe definir el filtro de metadatos si desea leer datos de métricas sobre un valor de dimensión específico.

En el ejemplo siguiente se muestra cómo leer datos de métricas sobre la métrica que admite varias dimensiones:

```csharp
    public static async Task ReadStorageMetricValueTest()
    {
        // Resource ID for blob storage
        var resourceId = "/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Storage/storageAccounts/{storageAccountName}/blobServices/default";
        var subscriptionId = "{SubscriptionID}";
        // How to identify Tenant ID, Application ID and Access Key: https://azure.microsoft.com/documentation/articles/resource-group-create-service-principal-portal/
        var tenantId = "{TenantID}";
        var applicationId = "{ApplicationID}";
        var accessKey = "{AccessKey}";

        MonitorManagementClient readOnlyClient = AuthenticateWithReadOnlyClient(tenantId, applicationId, accessKey, subscriptionId).Result;

        Microsoft.Azure.Management.Monitor.Models.Response Response;

        string startDate = DateTime.Now.AddHours(-3).ToUniversalTime().ToString("o");
        string endDate = DateTime.Now.ToUniversalTime().ToString("o");
        string timeSpan = startDate + "/" + endDate;
        // It's applicable to define meta data filter when a metric support dimension
        // More conditions can be added with the 'or' and 'and' operators, example: BlobType eq 'BlockBlob' or BlobType eq 'PageBlob'
        ODataQuery<MetadataValue> odataFilterMetrics = new ODataQuery<MetadataValue>(
            string.Format("BlobType eq '{0}'", "BlockBlob"));

        Response = readOnlyClient.Metrics.List(
                        resourceUri: resourceId,
                        timespan: timeSpan,
                        interval: System.TimeSpan.FromHours(1),
                        metricnames: "BlobCapacity",
                        odataQuery: odataFilterMetrics,
                        aggregation: "Average",
                        resultType: ResultType.Data);

        foreach (var metric in Response.Value)
        {
            //Enumrate metric value
            //    Id
            //    Name
            //    Type
            //    Unit
            //    Timeseries
            //        - Data
            //        - Metadatavalues
        }
    }

```

## <a name="understanding-resource-id-for-services-in-azure-storage"></a>Descripción de identificador de recurso para los servicios de Azure Storage

El identificador de recurso es un identificador único de un recurso de Azure. Cuando use la API de REST de Azure Monitor para leer las definiciones o los valores de las métricas, debe usar el identificador del recurso en el que va a operar. Este es el formato de la plantilla del identificador de recurso:

```
/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/{resourceProviderNamespace}/{resourceType}/{resourceName}
```

Storage proporciona métricas tanto a nivel de la cuenta de almacenamiento como al nivel de servicio con Azure Monitor. Por ejemplo, puede recuperar métricas solo de Blob Storage. Cada nivel tiene su propio identificador de recurso, que se utiliza para recuperar las métricas solo de ese nivel.

### <a name="resource-id-for-a-storage-account"></a>Identificador de recurso de una cuenta de almacenamiento

A continuación se muestra el formato para especificar el identificador de recurso de una cuenta de almacenamiento.

```
/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Storage/storageAccounts/{storageAccountName}
```

### <a name="resource-id-for-the-storage-services"></a>Identificador de recurso de los servicios de almacenamiento

A continuación se muestra el formato para especificar el identificador de recurso de los servicios de almacenamiento.

* Identificador de recurso de Blob service
```
/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Storage/storageAccounts/{storageAccountName}/blobServices/default
```
* Identificador de recurso de Table service
```
/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Storage/storageAccounts/{storageAccountName}/tableServices/default
```
* Identificador de recurso de Queue service
```
/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Storage/storageAccounts/{storageAccountName}/queueServices/default
```
* Identificador de recurso de File service
```
/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Storage/storageAccounts/{storageAccountName}/fileServices/default
```

### <a name="resource-id-in-azure-monitor-rest-api"></a>Identificador de recurso en la API de REST de Azure Monitor

A continuación se muestra el patrón que se utiliza cuando se llama a la API de REST de Azure Monitor.

```
GET {resourceId}/providers/microsoft.insights/metrics?{parameters}
```

## <a name="capacity-metrics"></a>Métricas de capacidad
Los valores de las métricas de capacidad se envían a Azure Monitor cada hora. Los valores se actualizan a diario. El intervalo de agregación define el intervalo de tiempo para el que se presentan los valores de las métricas. El intervalo de agregación que se admite en las métricas de capacidad es una hora (PT1H).

Azure Storage proporciona las siguientes métricas de capacidad en Azure Monitor.

### <a name="account-level"></a>Nivel de cuenta

| Nombre de la métrica | Descripción |
| ------------------- | ----------------- |
| UsedCapacity | La cantidad de almacenamiento que utiliza la cuenta de almacenamiento. En las cuentas de almacenamiento estándar, es la suma de la capacidad usada por un blob, una tabla, un archivo y una cola. Tanto en las cuentas de almacenamiento Premium como en las cuentas de Blob Storage, coincide con BlobCapacity. <br/><br/> Unidad: Bytes <br/> Tipo de agregación: Average <br/> Ejemplo de valor: 1024 |

### <a name="blob-storage"></a>Almacenamiento de blobs

| Nombre de la métrica | Descripción |
| ------------------- | ----------------- |
| BlobCapacity | El total de almacenamiento de blobs que se utiliza en la cuenta de almacenamiento. <br/><br/> Unidad: Bytes <br/> Tipo de agregación: Average <br/> Ejemplo de valor: 1024 <br/> Dimensiones: **BlobType** y **BlobTier** ([Definition](#metrics-dimensions)) |
| BlobCount    | El número de objetos BLOB almacenados en la cuenta de almacenamiento. <br/><br/> Unidad: Count <br/> Tipo de agregación: Average <br/> Ejemplo de valor: 1024 <br/> Dimensiones: **BlobType** y **BlobTier** ([Definition](#metrics-dimensions)) |
| ContainerCount    | El número de contenedores que hay en la cuenta de almacenamiento. <br/><br/> Unidad: Count <br/> Tipo de agregación: Average <br/> Ejemplo de valor: 1024 |
| IndexCapacity     | Cantidad de almacenamiento que usa el índice jerárquico de ADLS Gen2. <br/><br/> Unidad: Bytes <br/> Tipo de agregación: Average <br/> Ejemplo de valor: 1024 |

### <a name="table-storage"></a>Almacenamiento de tablas

| Nombre de la métrica | Descripción |
| ------------------- | ----------------- |
| TableCapacity | La cantidad de almacenamiento de tablas que utiliza la cuenta de almacenamiento. <br/><br/> Unidad: Bytes <br/> Tipo de agregación: Average <br/> Ejemplo de valor: 1024 |
| TableCount   | El número de tablas de la cuenta de almacenamiento. <br/><br/> Unidad: Count <br/> Tipo de agregación: Average <br/> Ejemplo de valor: 1024 |
| TableEntityCount | El número de entidades de tabla que hay en la cuenta de almacenamiento. <br/><br/> Unidad: Count <br/> Tipo de agregación: Average <br/> Ejemplo de valor: 1024 |

### <a name="queue-storage"></a>Queue Storage

| Nombre de la métrica | Descripción |
| ------------------- | ----------------- |
| QueueCapacity | La cantidad de almacenamiento de colas que utiliza la cuenta de almacenamiento. <br/><br/> Unidad: Bytes <br/> Tipo de agregación: Average <br/> Ejemplo de valor: 1024 |
| QueueCount   | El número de colas que hay en la cuenta de almacenamiento. <br/><br/> Unidad: Count <br/> Tipo de agregación: Average <br/> Ejemplo de valor: 1024 |
| QueueMessageCount | El número de mensajes de la colas no expirados que hay en la cuenta de almacenamiento. <br/><br/>Unidad: Count <br/> Tipo de agregación: Average <br/> Ejemplo de valor: 1024 |

### <a name="file-storage"></a>File Storage

| Nombre de la métrica | Descripción |
| ------------------- | ----------------- |
| FileCapacity | La cantidad de almacenamiento de archivos que utiliza la cuenta de almacenamiento. <br/><br/> Unidad: Bytes <br/> Tipo de agregación: Average <br/> Ejemplo de valor: 1024 |
| FileCount   | El número de archivos que hay en la cuenta de almacenamiento. <br/><br/> Unidad: Count <br/> Tipo de agregación: Average <br/> Ejemplo de valor: 1024 |
| FileShareCount | El número de recursos compartidos de archivo que hay en la cuenta de almacenamiento. <br/><br/> Unidad: Count <br/> Tipo de agregación: Average <br/> Ejemplo de valor: 1024 |

## <a name="transaction-metrics"></a>Métricas de transacciones

Se emiten en todas las solicitudes enviadas a una cuenta de almacenamiento de Azure Storage a Azure Monitor. En caso de que no haya actividad en la cuenta de almacenamiento, no habrá ningún dato en las métricas de transacciones del período. Todas las métricas de transacciones están disponibles a nivel tanto de cuenta como de servicio (Blob Storage, Table Storage, Azure Files y Queue Storage). El intervalo de agregación define el intervalo de tiempo en el que se presentan los valores de las métricas. Los intervalos de agregación compatible para todas las métricas de transacciones son PT1H y PT1M.

Azure Storage proporciona las siguientes métricas de transacciones en Azure Monitor.

| Nombre de la métrica | Descripción |
| ------------------- | ----------------- |
| Transacciones | El número de solicitudes realizadas a un servicio de almacenamiento o la operación de API especificada. Este número incluye solicitudes correctas y con errores, así como las solicitudes que generaron errores. <br/><br/> Unidad: Count <br/> Tipo de agregación: Total <br/> Dimensiones aplicables: ResponseType, GeoType, ApiName y Authentication ([Definición](#metrics-dimensions))<br/> Ejemplo de valor: 1024 |
| Entrada | La cantidad de datos de entrada. Este número incluye la entradas desde un cliente externo en Azure Storage, así como la entrada dentro de Azure. <br/><br/> Unidad: Bytes <br/> Tipo de agregación: Total <br/> Dimensiones aplicables: GeoType, ApiName y Authentication ([definición](#metrics-dimensions)) <br/> Ejemplo de valor: 1024 |
| Salida | La cantidad de datos de salida. Este número incluye la salida de un cliente externo en Azure Storage, así como la salida dentro de Azure. En consecuencia, este número no refleja las salidas facturables. <br/><br/> Unidad: Bytes <br/> Tipo de agregación: Total <br/> Dimensiones aplicables: GeoType, ApiName y Authentication ([definición](#metrics-dimensions)) <br/> Ejemplo de valor: 1024 |
| SuccessServerLatency | El tiempo medio que se usa para que Azure Storage procese una solicitud correcta . Este valor no incluye la latencia de red especificada en SuccessE2ELatency. <br/><br/> Unidad: Milisegundos <br/> Tipo de agregación: Average <br/> Dimensiones aplicables: GeoType, ApiName y Authentication ([definición](#metrics-dimensions)) <br/> Ejemplo de valor: 1024 |
| SuccessE2ELatency | La latencia media de un extremo a otro de las solicitudes correctas realizadas a un servicio de almacenamiento o a la operación de API especificada. Este valor incluye el tiempo de procesamiento requerido dentro de Azure Storage para leer la solicitud, enviar la respuesta y recibir la confirmación de la respuesta. <br/><br/> Unidad: Milisegundos <br/> Tipo de agregación: Average <br/> Dimensiones aplicables: GeoType, ApiName y Authentication ([definición](#metrics-dimensions)) <br/> Ejemplo de valor: 1024 |
| Disponibilidad | El porcentaje de disponibilidad para el servicio de almacenamiento o la operación de API especificada. Para calcular la disponibilidad hay que tomar el número total de solicitudes facturables y dividirlo por el número de solicitudes aplicables, incluidas las solicitudes que generaron errores inesperados. Todos los errores inesperados reducen la disponibilidad del servicio de almacenamiento o de la operación de API especificada. <br/><br/> Unidad: Percent <br/> Tipo de agregación: Average <br/> Dimensiones aplicables: GeoType, ApiName y Authentication ([definición](#metrics-dimensions)) <br/> Ejemplo de valor: 99,99 |

## <a name="metrics-dimensions"></a>Dimensiones de métricas

Azure Storage admite los siguientes dimensiones para las métricas en Azure Monitor.

| Nombre de dimensión | Descripción |
| ------------------- | ----------------- |
| **BlobType** | Solo el tipo de blob de las métricas de Blob. Los valores admitidos son **BlockBlob**, **PageBlob** y **Azure Data Lake Storage**. Anexar Blob se incluye en BlockBlob. |
| **BlobTier** | Azure Storage ofrece distintos niveles de acceso que le permiten almacenar datos de objeto de blob de la manera más rentable. Obtenga más información en [Nivel de blob de Azure Storage](../blobs/storage-blob-storage-tiers.md). Entre los valores admitidos se incluyen: <br/> <li>**Acceso frecuente**: nivel de acceso frecuente</li> <li>**Acceso esporádico**: nivel de acceso esporádico</li> <li>**Archivo**: nivel de archivo</li> <li>**Premium**: nivel Premium para blob en bloques</li> <li>**P4/P6/P10/P15/P20/P30/P40/P50/P60**: tipos de nivel para blob en páginas premium</li> <li>**Estándar**: tipo de nivel para blob en páginas estándar</li> <li>**Sin nivel**: Tipo de nivel para cuentas de almacenamiento de uso general v1</li> |
| **GeoType** | Transacción de clúster principal o secundario. Los valores disponibles incluyen **Principal** y **Secundario**. Se aplica al Almacenamiento con redundancia geográfica con acceso de lectura (RA-GRS) al leer objetos de un inquilino secundario. |
| **ResponseType** | Tipo de respuesta de la transacción. Los valores disponibles son: <br/><br/> <li>**ServerOtherError**: todos los errores del servidor, excepto los descritos. </li> <li>**ServerBusyError**: solicitud autenticada que devolvió un código de estado HTTP 503. </li> <li>**ServerTimeoutError**: solicitud autenticada que ha superado el tiempo de espera y que devolvió un código de estado HTTP 500. El tiempo de espera se superó debido a un error del servidor. </li> <li>**AuthorizationError**: solicitud autenticada errónea debido a un acceso no autorizado de los datos o a un error de autorización. </li> <li>**NetworkError**: solicitud autenticada errónea debido a errores de red. Normalmente se produce cuando un cliente cierra prematuramente una conexión antes de que se haya superado el tiempo de expiración. </li>  <li>**ClientAccountBandwidthThrottlingError**: la solicitud está limitada por el ancho de banda para superar los [límites de escalabilidad de la cuenta de almacenamiento ](scalability-targets-standard-account.md).</li><li>**ClientAccountRequestThrottlingError**: la solicitud está limitada por la tasa de solicitud para superar los [límites de escalabilidad de la cuenta de almacenamiento ](scalability-targets-standard-account.md).<li>**ClientThrottlingError**: Otro error de limitación del lado del cliente. ClientAccountBandwidthThrottlingError y ClientAccountRequestThrottlingError se excluyen.</li> <li>**ClientTimeoutError**: solicitud autenticada que ha superado el tiempo de espera y que devolvió un código de estado HTTP 500. Si el tiempo de expiración de la red del cliente o el tiempo de expiración de la solicitud se establece en un valor menor de lo que espera el servicio de almacenamiento, es un tiempo de expiración esperado. De lo contrario, se notifica como un error ServerTimeoutError.</li> </li> <li>**ClientOtherError**: todos los errores del lado cliente, excepto los descritos. </li> <li>**Correcto**: solicitud correcta.</li> <li> **SuccessWithThrottling**: solicitud correcta cuando un cliente SMB obtiene una limitación en el primer intento, pero se ejecuta correctamente después de varios reintentos.</li> |
| **ApiName** | El nombre de la operación. Por ejemplo: <br/> <li>**CreateContainer**</li> <li>**DeleteBlob**</li> <li>**GetBlob**</li> Para ver los nombres de todas las operaciones, consulte este [documento](/rest/api/storageservices/storage-analytics-logged-operations-and-status-messages). |
| **Autenticación** | Tipos de autenticación que se usan en las transacciones. Los valores disponibles son: <br/> <li>**AccountKey**: la transacción se autentica con la clave de la cuenta de almacenamiento.</li> <li>**SAS**: la transacción se autentica con firmas de acceso compartido.</li> <li>**OAuth**: la transacción se autentica con tokens de acceso de OAuth.</li> <li>**Anonymous**: la transacción se solicita de forma anónima. No incluye las solicitudes preparatorias.</li> <li>**AnonymousPreflight**: la transacción es una solicitud preparatoria.</li> |

En el caso de las dimensiones compatibles con métricas, es preciso especificar el valor de la dimensión para ver los valores correspondientes de las métricas. Por ejemplo, si observa el valor **Transactions** en las respuestas correctas, es preciso filtrar la dimensión **ResponseType** por **Success**. O bien si examina el valor **BlobCount** de Blob en bloques, es preciso filtrar la dimensión **BlobType** por **BlockBlob**.

## <a name="service-continuity-of-legacy-metrics"></a>Continuidad en el servicio de métricas heredadas

Las métricas heredades están disponibles en paralelo con las métricas de Azure Monitor. La compatibilidad sigue siendo la misma hasta que Azure Storage finaliza el servicio en las métricas heredadas.

## <a name="faq"></a>Preguntas más frecuentes

**¿Es compatible la nueva métrica con la cuenta de almacenamiento clásica?**

No, la métrica nueva de Azure Monitor solo admite cuentas de almacenamiento de Azure Resource Manager. Si desea usar la métricas en cuentas de almacenamiento, es preciso que realice la migración a una cuenta de almacenamiento de Azure Resource Manager. Consulte [Migración a Azure Resource Manager](https://docs.microsoft.com/azure/virtual-machines/windows/migration-classic-resource-manager-overview).

**¿Admite Azure Storage métricas de discos administrados o no administrados?**

No, Azure Compute admite las métricas en discos. Consulte el [artículo](https://azure.microsoft.com/blog/per-disk-metrics-managed-disks/) para más información.

**¿Cómo asignar y migrar métricas clásicas con nuevas métricas?**

Puede encontrar asignaciones detalladas entre métricas clásicas y nuevas métricas en [Migración de las métricas de Azure Storage](./storage-metrics-migration.md).

## <a name="next-steps"></a>Pasos siguientes

* [Azure Monitor](../../monitoring-and-diagnostics/monitoring-overview.md)
