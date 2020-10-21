---
title: Supervisión de Azure Queue Storage | Microsoft Docs
description: Obtenga información sobre cómo supervisar el rendimiento y la disponibilidad de Azure Queue Storage. Supervise los datos de Azure Queue Storage, obtenga información sobre la configuración y analice los datos de métricas y registro.
author: normesta
services: storage
ms.service: storage
ms.topic: conceptual
ms.date: 10/02/2020
ms.author: normesta
ms.reviewer: fryu
ms.custom: monitoring, devx-track-csharp
ms.openlocfilehash: 3fe99543b821810b1479f1e504098d81fd20c534
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "91711088"
---
# <a name="monitoring-azure-queue-storage"></a>Supervisión de Azure Queue Storage

Si tiene aplicaciones y procesos empresariales críticos que dependen de recursos de Azure, querrá supervisar esos recursos para su disponibilidad, rendimiento y funcionamiento. En este artículo se describen los datos de supervisión que genera Azure Queue Storage y cómo puede usar las características de Azure Monitor para analizar las alertas sobre estos datos.

> [!NOTE]
> Los registros de Azure Storage en Azure Monitor están en versión preliminar pública, además de estar disponibles para pruebas de versión preliminar en todas las regiones de nube pública. Para inscribirse en la versión preliminar, visite [esta página](https://forms.microsoft.com/Pages/ResponsePage.aspx?id=v4j5cvGGr0GRqy180BHbRxW65f1VQyNCuBHMIMBV8qlUM0E0MFdPRFpOVTRYVklDSE1WUTcyTVAwOC4u). Esta versión preliminar habilita los registros de blobs (que incluye Azure Data Lake Storage Gen2), archivos, colas y tablas. Esta característica está disponible para todas las cuentas de almacenamiento que se crean con el modelo de implementación de Azure Resource Manager. Consulte [Introducción a las cuentas de almacenamiento](../common/storage-account-overview.md).

## <a name="monitor-overview"></a>Información general de supervisión

La página **Información general** de Azure Portal para cada recurso de Queue Storage incluye una breve vista del uso de los recursos, como las solicitudes y la facturación por hora. Esta información es útil, pero solo hay disponible una pequeña cantidad de datos de supervisión. Algunos de estos datos se recopilan automáticamente y están disponibles para su análisis en cuanto se crea el recurso. Puede habilitar tipos adicionales de recopilación de datos con cierta configuración adicional.

## <a name="what-is-azure-monitor"></a>¿Qué es Azure Monitor?
Azure Queue Storage crea los datos de supervisión mediante [Azure Monitor](../../azure-monitor/overview.md), que es un servicio de supervisión de pila completo en Azure. Azure Monitor proporciona un conjunto completo de características para supervisar los recursos de Azure y los recursos en otras nubes y en el entorno local. 

Comience con el artículo [Supervisión de recursos de Azure con Azure Monitor](../../azure-monitor/insights/monitor-azure-resource.md), que describe lo siguiente:

- ¿Qué es Azure Monitor?
- Costos asociados con la supervisión
- Datos de supervisión recopilados en Azure
- Configuración de la recolección de datos
- Herramientas estándar en Azure para analizar datos de supervisión y alertar sobre ellos

Las secciones siguientes complementan este artículo mediante la descripción de los datos específicos que se recopilan de Azure Storage. En los ejemplos se muestra cómo configurar la recopilación de datos y analizar estos datos con herramientas de Azure.

## <a name="monitoring-data"></a>Supervisión de datos

Azure Queue Storage recopila los mismos tipos de datos de supervisión que otros recursos de Azure, que se describen en [Supervisión de datos de recursos de Azure](../../azure-monitor/insights/monitor-azure-resource.md#monitoring-data). 

Consulte [Referencia de datos de supervisión de Azure Queue Storage](monitor-queue-storage-reference.md) para obtener información detallada sobre las métricas y las métricas de registros que crea Azure Queue Storage.

Las métricas y registros de Azure Monitor solo admiten cuentas de almacenamiento de Azure Resource Manager. Azure Monitor no admite cuentas de almacenamiento clásicas. Si desea usar las métricas o registros en una cuenta de almacenamiento clásica, es preciso migrar a una cuenta de almacenamiento de Azure Resource Manager. Consulte [Migración a Azure Resource Manager](https://docs.microsoft.com/azure/virtual-machines/windows/migration-classic-resource-manager-overview).

Si lo desea, puede seguir usando las métricas y registros clásicos. De hecho, las métricas y registros clásicos están disponibles en paralelo a las métricas y registros en Azure Monitor. La compatibilidad sigue siendo la misma hasta que Azure Storage finalice el servicio en las métricas y registros heredados.

## <a name="collection-and-routing"></a>Recopilación y enrutamiento

Las métricas de la plataforma y el registro de actividad se recopilan automáticamente, pero se pueden enrutar a otras ubicaciones mediante una configuración de diagnóstico. Debe crear una configuración de diagnóstico para recopilar registros de recurso. 

Para crear una configuración de diagnóstico mediante Azure Portal, la CLI de Azure o PowerShell, consulte [Creación de una configuración de diagnóstico para recopilar registros de plataforma y métricas en Azure](../../azure-monitor/platform/diagnostic-settings.md). 

Para ver una plantilla de Azure Resource Manager que crea una configuración de diagnóstico, consulte [Configuración de diagnóstico para Azure Storage](https://docs.microsoft.com/azure/azure-monitor/samples/resource-manager-diagnostic-settings#diagnostic-setting-for-azure-storage).

Al crear una configuración de diagnóstico, deberá elegir el tipo de almacenamiento para el que desea habilitar los registros, por ejemplo, blob, cola, tabla, archivo. En Queue Storage, elija **cola**. 

Si crea la configuración de diagnóstico en Azure Portal, puede seleccionar el recurso de una lista. Si usa PowerShell o la CLI de Azure, debe usar el identificador de recurso del punto de conexión de Queue Storage. Para encontrar el identificador de recurso en Azure Portal, abra la página **Propiedades** de la cuenta de almacenamiento.

También tendrá que especificar una de las siguientes categorías de operaciones para las que desea recopilar registros. 

| Category | Descripción |
|:---|:---|
| StorageRead | Operaciones de lectura en objetos. |
| StorageWrite | Operaciones de escritura en objetos. |
| StorageDelete | Operaciones de eliminación en objetos. |

## <a name="analyzing-metrics"></a>Análisis de métricas

Puede analizar las métricas de Azure Storage con métricas de otros servicios de Azure con el Explorador de métricas. Para abrir el Explorador de métricas, elija **Métricas** en el menú **Azure Monitor**. Para más información sobre esta herramienta, consulte [Introducción al Explorador de métricas de Azure](../../azure-monitor/platform/metrics-getting-started.md). 

En este ejemplo se muestra cómo ver **Transacciones** a nivel de cuenta.

![Captura de pantalla de acceso a métricas desde Azure Portal](./media/monitor-queue-storage/access-metrics-portal.png)

Para ver las métricas que admiten dimensiones, puede filtrar las métricas con el valor de dimensión deseado. En este ejemplo se muestra cómo ver **Transacciones** en el nivel de cuenta sobre una operación específica mediante la selección de los valores de la dimensión **Nombre de API**.

![Captura de pantalla de acceso a métricas con dimensión desde Azure Portal](./media/monitor-queue-storage/access-metrics-portal-with-dimension.png)

Para obtener una lista completa de las dimensiones que admite Azure Storage, consulte [Dimensiones de métricas](monitor-queue-storage-reference.md#metrics-dimensions).

Las métricas de Azure Queue Storage están en estos espacios de nombres: 

- Microsoft.Storage/storageAccounts
- Microsoft.Storage/storageAccounts/queueServices

Para obtener una lista de todas las métricas compatibles de Azure Monitor, lo que incluye Azure Queue Storage, consulte [Métricas compatibles con Azure Monitor](https://docs.microsoft.com/azure/azure-monitor/platform/metrics-supported).


### <a name="accessing-metrics"></a>Acceso a métricas

> [!TIP]
> Para ver ejemplos de la CLI de Azure o .NET, elija las pestañas correspondientes a continuación.

### <a name="powershell"></a>[PowerShell](#tab/azure-powershell)

#### <a name="list-the-metric-definition"></a>Enumeración de la definición de métricas

Puede mostrar la definición de la métrica de la cuenta de almacenamiento o el servicio Queue Storage. Use el cmdlet [Get-AzMetricDefinition](https://docs.microsoft.com/powershell/module/az.monitor/get-azmetricdefinition).

En este ejemplo, reemplace el marcador de posición `<resource-ID>` por el identificador de recurso de la cuenta de almacenamiento completa o el identificador de recurso del servicio Queue Storage.  Puede encontrar estos identificadores de recursos en las páginas **Propiedades** de su cuenta de almacenamiento en Azure Portal.

```powershell
   $resourceId = "<resource-ID>"
   Get-AzMetricDefinition -ResourceId $resourceId
```

#### <a name="reading-metric-values"></a>Lectura de valores de métricas

Puede leer valores de métricas de nivel de cuenta de la cuenta de almacenamiento o el servicio Queue Storage. Use el cmdlet [Get-AzMetric](https://docs.microsoft.com/powershell/module/Az.Monitor/Get-AzMetric).

```powershell
   $resourceId = "<resource-ID>"
   Get-AzMetric -ResourceId $resourceId -MetricNames "UsedCapacity" -TimeGrain 01:00:00
```

### <a name="azure-cli"></a>[CLI de Azure](#tab/azure-cli)

#### <a name="list-the-account-level-metric-definition"></a>Enumeración de la definición de métricas a nivel de cuenta

Puede mostrar la definición de la métrica de la cuenta de almacenamiento o el servicio Queue Storage. Use el comando [az monitor metrics list-definitions](https://docs.microsoft.com/cli/azure/monitor/metrics#az-monitor-metrics-list-definitions).
 
En este ejemplo, reemplace el marcador de posición `<resource-ID>` por el identificador de recurso de la cuenta de almacenamiento completa o el identificador de recurso del servicio Queue Storage. Puede encontrar estos identificadores de recursos en las páginas **Propiedades** de su cuenta de almacenamiento en Azure Portal.

```azurecli-interactive
   az monitor metrics list-definitions --resource <resource-ID>
```

#### <a name="read-account-level-metric-values"></a>Lectura de valores de métricas a nivel de cuenta

Puede leer los valores de métricas de la cuenta de almacenamiento o el servicio Queue Storage. Use el comando [az monitor metrics list](https://docs.microsoft.com/cli/azure/monitor/metrics#az-monitor-metrics-list).

```azurecli-interactive
   az monitor metrics list --resource <resource-ID> --metric "UsedCapacity" --interval PT1H
```

### <a name="net"></a>[.NET](#tab/dotnet)

Azure Monitor proporciona el [SDK de .NET](https://www.nuget.org/packages/Microsoft.Azure.Management.Monitor/) para leer la definición y los valores de las métricas. El [código de ejemplo](https://azure.microsoft.com/resources/samples/monitor-dotnet-metrics-api/) muestra cómo utilizar el SDK con parámetros diferentes. Debe usar `0.18.0-preview` o una versión posterior para las métricas de almacenamiento.
 
En estos ejemplos, reemplace el marcador de posición `<resource-ID>` por el identificador de recurso de la cuenta de almacenamiento completa o el servicio Queue Storage. Puede encontrar estos identificadores de recursos en las páginas **Propiedades** de su cuenta de almacenamiento en Azure Portal.

Reemplace la variable `<subscription-ID>` por el identificador de la suscripción. Para obtener una guía sobre cómo obtener los valores para `<tenant-ID>`, `<application-ID>` y `<AccessKey>`, consulta [Uso del portal para crear una aplicación de Azure AD y una entidad de servicio con acceso a los recursos](https://azure.microsoft.com/documentation/articles/resource-group-create-service-principal-portal/). 

#### <a name="list-the-account-level-metric-definition"></a>Enumeración de la definición de métricas a nivel de cuenta

En el ejemplo siguiente se muestra cómo enumerar la definición de una métrica a nivel de cuenta:

```csharp
    public static async Task ListStorageMetricDefinition()
    {
        var resourceId = "<resource-ID>";
        var subscriptionId = "<subscription-ID>";
        var tenantId = "<tenant-ID>";
        var applicationId = "<application-ID>";
        var accessKey = "<AccessKey>";


        MonitorManagementClient readOnlyClient = AuthenticateWithReadOnlyClient(tenantId, applicationId, accessKey, subscriptionId).Result;
        IEnumerable<MetricDefinition> metricDefinitions = await readOnlyClient.MetricDefinitions.ListAsync(resourceUri: resourceId, cancellationToken: new CancellationToken());

        foreach (var metricDefinition in metricDefinitions)
        {
            // Enumrate metric definition:
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

#### <a name="reading-account-level-metric-values"></a>Lectura de valores de métricas a nivel de cuenta

En el ejemplo siguiente se muestra cómo leer los datos de `UsedCapacity` a nivel de cuenta:

```csharp
    public static async Task ReadStorageMetricValue()
    {
        var resourceId = "<resource-ID>";
        var subscriptionId = "<subscription-ID>";
        var tenantId = "<tenant-ID>";
        var applicationId = "<application-ID>";
        var accessKey = "<AccessKey>";

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
            // Enumrate metric value
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

#### <a name="reading-multidimensional-metric-values"></a>Lectura de valores de métricas multidimensionales

En el caso de las métricas multidimensionales, debe definir el filtro de metadatos si desea leer datos de métricas sobre valores de dimensión específicos.

En el ejemplo siguiente se muestra cómo leer datos de métricas sobre una métrica que admite varias dimensiones:

```csharp
    public static async Task ReadStorageMetricValueTest()
    {
        // Resource ID for queue storage
        var resourceId = "/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Storage/storageAccounts/{storageAccountName}/queueServices/default";
        var subscriptionId = "<subscription-ID}";
        // How to identify Tenant ID, Application ID and Access Key: https://azure.microsoft.com/documentation/articles/resource-group-create-service-principal-portal/
        var tenantId = "<tenant-ID>";
        var applicationId = "<application-ID>";
        var accessKey = "<AccessKey>";

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

---

## <a name="analyzing-logs"></a>Análisis de datos

Puede acceder a los registros de los recursos como cola en una cuenta de almacenamiento, como datos de eventos o a través de consultas de Log Analytics.

Para una referencia detallada de los campos que aparecen en dichos registros, consulte [Referencia de datos de supervisión de Azure Queue Storage](monitor-queue-storage-reference.md).

> [!NOTE]
> Los registros de Azure Storage en Azure Monitor están en versión preliminar pública, además de estar disponibles para pruebas de versión preliminar en todas las regiones de nube pública. Para inscribirse en la versión preliminar, visite [esta página](https://forms.microsoft.com/Pages/ResponsePage.aspx?id=v4j5cvGGr0GRqy180BHbRxW65f1VQyNCuBHMIMBV8qlUM0E0MFdPRFpOVTRYVklDSE1WUTcyTVAwOC4u). Esta versión preliminar habilita los registros de blobs (lo que incluye Azure Data Lake Storage Gen2), archivos, colas, tablas, cuentas de almacenamiento Premium en cuentas de almacenamiento de uso general v1 y v2. Las cuentas de almacenamiento clásico no se admiten.

Las entradas del registro se crean solo si se presentan solicitudes al punto de conexión de servicio. Por ejemplo, si una cuenta de almacenamiento tiene actividad en el punto de conexión de la cola, pero no en los puntos de conexión de tabla o cola, solo se crean los registros correspondientes a Queue service. Los registros de Azure Storage contienen información detallada sobre las solicitudes correctas y erróneas realizadas a un servicio de almacenamiento. Esta información se puede utilizar para supervisar solicitudes concretas y para diagnosticar problemas con un servicio de almacenamiento. Las solicitudes se registran en función de la mejor opción.

### <a name="log-authenticated-requests"></a>Registro de solicitudes autenticadas

 Se registran los siguientes tipos de solicitudes autenticadas:

- Solicitudes correctas
- Solicitudes erróneas, incluidos errores de tiempo de espera, de limitación, de red, de autorización y de otro tipo
- Solicitudes que usan una firma de acceso compartido (SAS) u OAuth, incluidas las solicitudes correctas como con error
- Solicitudes de datos de análisis (datos de registro clásicos en el contenedor **$logs**, y datos de métricas de clase en las tablas **$metric**)

Las solicitudes realizadas por el propio servicio Queue Storage, como la creación o eliminación de registros, no se registran. Para encontrar una lista completa de los datos registrados, consulte [Operaciones y mensajes de estado registrados por Storage](/rest/api/storageservices/storage-analytics-logged-operations-and-status-messages) y [Formato del registro de Storage](monitor-queue-storage-reference.md).

### <a name="log-anonymous-requests"></a>Registro de solicitudes anónimas

 Se registran los siguientes tipos de solicitudes anónimas:

- Solicitudes correctas
- Errores del servidor
- Errores de tiempo de espera para el cliente y el servidor
- Solicitudes GET erróneas con el código de error 304 (No modificado)

El resto de las solicitudes anónimas con error no se registran. Para encontrar una lista completa de los datos registrados, consulte [Operaciones y mensajes de estado registrados por Storage](/rest/api/storageservices/storage-analytics-logged-operations-and-status-messages) y [Formato del registro de Storage](monitor-queue-storage-reference.md).

### <a name="accessing-logs-in-a-storage-account"></a>Acceso a registros en una cuenta de almacenamiento

Los registros aparecen como blobs almacenados en un contenedor en la cuenta de almacenamiento de destino. Los datos se recopilan y almacenan en un único blob como una carga JSON delimitada por líneas. El nombre del blob sigue la convención de nomenclatura siguiente:

`https://<destination-storage-account>.blob.core.windows.net/insights-logs-<storage-operation>/resourceId=/subscriptions/<subscription-ID>/resourceGroups/<resource-group-name>/providers/Microsoft.Storage/storageAccounts/<source-storage-account>/queueServices/default/y=<year>/m=<month>/d=<day>/h=<hour>/m=<minute>/PT1H.json`

Este es un ejemplo:

`https://mylogstorageaccount.blob.core.windows.net/insights-logs-storagewrite/resourceId=/subscriptions/`<br>`208841be-a4v3-4234-9450-08b90c09f4/resourceGroups/myresourcegroup/providers/Microsoft.Storage/storageAccounts/mystorageaccount/queueServices/default/y=2019/m=07/d=30/h=23/m=12/PT1H.json`

### <a name="accessing-logs-in-an-event-hub"></a>Acceso a los registros en un centro de eventos

Los registros que se envían a un centro de eventos no se almacenan como archivo, pero puede verificar que el centro de eventos ha recibido la información del registro. En Azure Portal, vaya al centro de eventos y verifique que el recuento de **mensajes entrantes** sea mayor que cero. 

![Registros de auditoría](media/monitor-queue-storage/event-hub-log.png)

Puede acceder y leer los datos de registro que se envían al centro de eventos mediante las herramientas de supervisión y administración de eventos y de información de seguridad. Consulte [¿Qué puedo hacer con los datos de supervisión que se envían a mi centro de eventos?](https://docs.microsoft.com/azure/azure-monitor/platform/stream-monitoring-data-event-hubs#what-can-i-do-with-the-monitoring-data-being-sent-to-my-event-hub) para obtener más información.

### <a name="accessing-logs-in-a-log-analytics-workspace"></a>Acceso a registros en un área de trabajo de Log Analytics

Puede acceder a los registros que se envían a un área de trabajo Log Analytics mediante consultas de registro de Azure Monitor.

Para más información, consulte [Introducción a los análisis de registros de Azure Monitor](https://docs.microsoft.com/azure/azure-monitor/log-query/get-started-portal).

Los datos se almacenan en la tabla **StorageQueueLogs**.  

#### <a name="sample-kusto-queries"></a>Ejemplos de consultas de Kusto

Estas son algunas consultas que puede escribir en la barra **Búsqueda de registros** para ayudarle a supervisar su instancia de Queue Storage. Estas consultas funcionan con el [nuevo lenguaje](https://docs.microsoft.com/azure/azure-monitor/log-query/log-query-overview).

> [!IMPORTANT]
> Al seleccionar **Registros** en el menú de grupo de recursos de la cuenta de almacenamiento, Log Analytics se abre con el ámbito de la consulta establecido en el grupo de recursos actual. Esto significa que las consultas de registro solo incluirán datos de ese grupo de recursos. Si desea ejecutar una consulta que incluya datos de otros recursos o de otros servicios de Azure, seleccione **Registros** en el menú **Azure Monitor**. Consulte [Ámbito e intervalo de tiempo de una consulta de registro en Log Analytics de Azure Monitor](/azure/azure-monitor/log-query/scope/) para obtener más información.

Utilice estas consultas para ayudar a supervisar las cuentas de Azure Storage:

* Para enumerar los 10 errores más comunes en los últimos tres días.

    ```Kusto
    StorageQueueLogs
    | where TimeGenerated > ago(3d) and StatusText !contains "Success"
    | summarize count() by StatusText
    | top 10 by count_ desc
    ```
* Para enumerar las 10 operaciones principales que causaron la mayoría de los errores en los últimos tres días.

    ```Kusto
    StorageQueueLogs
    | where TimeGenerated > ago(3d) and StatusText !contains "Success"
    | summarize count() by OperationName
    | top 10 by count_ desc
    ```
* Para enumerar las 10 operaciones principales con la mayor latencia de un extremo a otro en los últimos tres días.

    ```Kusto
    StorageQueueLogs
    | where TimeGenerated > ago(3d)
    | top 10 by DurationMs desc
    | project TimeGenerated, OperationName, DurationMs, ServerLatencyMs, ClientLatencyMs = DurationMs - ServerLatencyMs
    ```
* Para enumerar todas las operaciones que provocan errores de limitación del lado servidor durante los últimos tres días.

    ```Kusto
    StorageQueueLogs
    | where TimeGenerated > ago(3d) and StatusText contains "ServerBusy"
    | project TimeGenerated, OperationName, StatusCode, StatusText
    ```
* Para enumerar todas las solicitudes con acceso anónimo durante los últimos tres días.

    ```Kusto
    StorageBlobLogs
    | where TimeGenerated > ago(3d) and AuthenticationType == "Anonymous"
    | project TimeGenerated, OperationName, AuthenticationType, Uri
    ```
* Para crear un gráfico circular de las operaciones usadas en los últimos tres días.
    ```Kusto
    StorageQueueLogs
    | where TimeGenerated > ago(3d)
    | summarize count() by OperationName
    | sort by count_ desc 
    | render piechart
    ```
## <a name="faq"></a>Preguntas más frecuentes

**¿Admite Azure Storage métricas de discos administrados o no administrados?**

No. Azure Compute admite las métricas en discos. Para obtener más información, consulte [Métricas por disco para discos administrados y no administrados](https://azure.microsoft.com/blog/per-disk-metrics-managed-disks/).

## <a name="next-steps"></a>Pasos siguientes

- Para ver una referencia de los registros y las métricas creados por Azure Queue Storage, consulte [Referencia de datos de supervisión de Azure Queue Storage](monitor-queue-storage-reference.md).
- Para obtener más información sobre la supervisión de recursos de Azure, consulte [Supervisión de recursos de Azure con Azure Monitor](../../azure-monitor/insights/monitor-azure-resource.md).
- Para obtener más información sobre la migración de métricas, consulte [Migración de las métricas de Azure Storage](../common/storage-metrics-migration.md).
