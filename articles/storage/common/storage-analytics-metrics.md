---
title: Métricas de Azure Storage Analytics (clásicas)
description: Obtenga información sobre cómo usar métricas en Azure Storage.
author: normesta
ms.service: storage
ms.topic: conceptual
ms.date: 03/11/2019
ms.author: normesta
ms.reviewer: fryu
ms.subservice: common
ms.openlocfilehash: 03e5f1e888680f6020b45f51103e7b5cb6dc86ab
ms.sourcegitcommit: 366e95d58d5311ca4b62e6d0b2b47549e06a0d6d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/01/2020
ms.locfileid: "82692730"
---
# <a name="azure-storage-analytics-metrics-classic"></a>Métricas de Azure Storage Analytics (clásicas)

Storage Analytics puede almacenar métricas que incluyen estadísticas acumuladas de las transacciones y datos de capacidad sobre las solicitudes realizadas a un servicio de almacenamiento. Las transacciones se notifican tanto en el nivel de operación de API como en el nivel de servicio de almacenamiento, y la capacidad se notifica en el nivel de servicio de almacenamiento. Los datos de las métricas se pueden utilizar para analizar el uso del servicio de almacenamiento, diagnosticar problemas con las solicitudes realizadas en el mismo y mejorar el rendimiento de las aplicaciones que utilizan un servicio.  

 Las métricas de Storage Analytics se habilitan de forma predeterminada para las nuevas cuentas de almacenamiento. Las métricas se pueden configurar en [Azure Portal](https://portal.azure.com/); para obtener más información, vea [Supervisión de una cuenta de almacenamiento en Azure Portal](/azure/storage/storage-monitor-storage-account). También puede habilitar Storage Analytics mediante programación a través de la API de REST o la biblioteca de cliente. Use las operaciones Set Service Properties para habilitar Storage Analytics en cada servicio.  

> [!NOTE]
> Las métricas de Storage Analytics están disponibles en los servicios Blob service, Queue service, Table service y File service.
> Ahora, las métricas de Storage Analytics son métricas clásicas. Microsoft recomienda usar [métricas de Azure Storage en Azure Monitor](monitor-storage.md) en lugar de métricas de Storage Analytics.

## <a name="transaction-metrics"></a>Métricas de transacciones  
 Se registra un conjunto sólido de datos a intervalos de cada hora o de cada minuto para cada servicio de almacenamiento y operación de API que se ha solicitado, incluidos entradas/salidas, disponibilidad, errores y porcentajes de solicitudes por categorías. Puede ver una lista completa de los detalles de transacción en el tema [Esquema de las tablas de métricas de Storage Analytics](/rest/api/storageservices/storage-analytics-metrics-table-schema).  

 Los datos de las transacciones se registran en dos niveles: el nivel de servicio y el nivel de operación de API. En el nivel de servicio, las estadísticas que resumen todas las operaciones de API solicitadas se escriben en una entidad de la tabla cada hora, aunque no se haya realizado ninguna solicitud al servicio. En el nivel de operación de API, las estadísticas se escriben únicamente en una entidad si la operación se solicitó durante esa hora.  

 Por ejemplo, si realiza una operación **GetBlob** en Blob service, las métricas de análisis de Storage registrarán la solicitud y la incluirán en los datos agregados tanto para Blob service como para la operación **GetBlob**. Pero si no se solicita ninguna operación **GetBlob** durante esa hora, no escribirá ninguna entidad en *$MetricsTransactionsBlob* para dicha operación.  

 Las métricas de transacciones se registran para las solicitudes del usuario y para las solicitudes realizadas por Storage Analytics. Por ejemplo, se registran las solicitudes realizadas por Storage Analytics para escribir registros y entidades de tabla.

## <a name="capacity-metrics"></a>Métricas de capacidad  

> [!NOTE]
>  Actualmente, las métricas de capacidad solo están disponibles para Blob service.

 Los datos de capacidad se registran diariamente para Blob service de una cuenta de almacenamiento, y se escriben dos entidades de tabla. Una entidad proporciona estadísticas para los datos de usuario y la otra proporciona estadísticas sobre el contenedor de blob `$logs` utilizado por Storage Analytics. La tabla *$MetricsCapacityBlob* incluye las estadísticas siguientes:  

- **Capacity**: cantidad de almacenamiento que ha usado la instancia de Blob service de la cuenta de almacenamiento, en bytes.  
- **ContainerCount**: número de contenedores de blobs en la instancia de Blob service de la cuenta de almacenamiento.  
- **ObjectCount**: número de blobs en bloque o en páginas confirmados y sin confirmar de la instancia de Blob service de la cuenta de almacenamiento.  

  Para obtener más información acerca de las métricas de capacidad, vea el [Esquema de las tablas de métricas de Storage Analytics](/rest/api/storageservices/storage-analytics-metrics-table-schema).  

## <a name="how-metrics-are-stored"></a>Cómo se almacenan las métricas  

 Todos los datos de métricas para cada uno de los servicios de almacenamiento se almacenan en tres tablas reservadas para ese servicio: una tabla para la información sobre transacciones, otra tabla para la información sobre las transacciones por minuto y una última tabla para la información sobre capacidad. La información sobre transacciones y transacciones por minuto consta de datos de solicitudes y respuestas, y la información sobre capacidad consta de datos de uso del almacenamiento. Se puede acceder a las métricas por hora y por minuto, y a la capacidad de Blob service de una cuenta de almacenamiento, en tablas cuyos nombres se describen en la tabla siguiente.  

|Nivel de métricas|Nombres de tabla|Versiones compatibles|  
|-------------------|-----------------|----------------------------|  
|Métricas por horas, ubicación principal|-   $MetricsTransactionsBlob<br />-   $MetricsTransactionsTable<br />-   $MetricsTransactionsQueue|Versiones anteriores a 2013-08-15 solamente. Si bien estos nombres todavía se admiten, se recomienda que empiece a usar las tablas que se muestran a continuación.|  
|Métricas por horas, ubicación principal|-   $MetricsHourPrimaryTransactionsBlob<br />-   $MetricsHourPrimaryTransactionsTable<br />-   $MetricsHourPrimaryTransactionsQueue<br />-   $MetricsHourPrimaryTransactionsFile|Todas las versiones. La compatibilidad con métricas del servicio File service está disponible únicamente en la versión 2015-04-05 en adelante.|  
|Métricas por minutos, ubicación principal|-   $MetricsMinutePrimaryTransactionsBlob<br />-   $MetricsMinutePrimaryTransactionsTable<br />-   $MetricsMinutePrimaryTransactionsQueue<br />-   $MetricsMinutePrimaryTransactionsFile|Todas las versiones. La compatibilidad con métricas del servicio File service está disponible únicamente en la versión 2015-04-05 en adelante.|  
|Métricas por horas, ubicación secundaria|-   $MetricsHourSecondaryTransactionsBlob<br />-   $MetricsHourSecondaryTransactionsTable<br />-   $MetricsHourSecondaryTransactionsQueue|Todas las versiones. Debe estar habilitada la replicación con redundancia geográfica con acceso de lectura.|  
|Métricas por minutos, ubicación secundaria|-   $MetricsMinuteSecondaryTransactionsBlob<br />-   $MetricsMinuteSecondaryTransactionsTable<br />-   $MetricsMinuteSecondaryTransactionsQueue|Todas las versiones. Debe estar habilitada la replicación con redundancia geográfica con acceso de lectura.|  
|Capacidad (solo Blob service)|$MetricsCapacityBlob|Todas las versiones.|  

 Estas tablas se crean automáticamente cuando Storage Analytics se habilita para un punto de conexión de servicio de almacenamiento. Se tiene acceso a ellas a través del espacio de nombres de la cuenta de almacenamiento, por ejemplo: `https://<accountname>.table.core.windows.net/Tables("$MetricsTransactionsBlob")`. Las tablas de métricas no aparecen en una operación de lista, y el acceso a ellas debe efectuarse directamente a través del nombre de la tabla.  

## <a name="enable-metrics-using-the-azure-portal"></a>Habilitar métricas mediante Azure Portal
Siga estos pasos para habilitar las métricas en [Azure Portal](https://portal.azure.com):

1. Vaya a la cuenta de almacenamiento.
1. Seleccione **Configuración de diagnóstico (clásica)** en el panel **Menú**.
1. Asegúrese de que la opción **Estado** está establecida en **Activada**.
1. Seleccione las métricas para los servicios que desea supervisar.
1. Especifique una directiva de retención para indicar cuánto tiempo se conservarán las métricas y los datos de registro.
1. Seleccione **Guardar**.

[Azure Portal](https://portal.azure.com) no permite actualmente configurar métricas por minuto en su cuenta de almacenamiento; debe habilitar las métricas por minuto con PowerShell o mediante programación.

## <a name="enable-storage-metrics-using-powershell"></a>Habilitar métricas de almacenamiento con PowerShell  
Puede usar PowerShell en la máquina local para configurar las métricas de almacenamiento en su cuenta de almacenamiento con los cmdlets de Azure PowerShell **Get-AzStorageServiceMetricsProperty** para recuperar la configuración actual y **Set-AzStorageServiceMetricsProperty** para cambiar la configuración actual.  

Los cmdlets que controlan las Métricas de almacenamiento usan los siguientes parámetros:  

* **ServiceType**; los valores posibles son **Blob**,  **Queue**, **Table** y **File**.
* **MetricsType**; los valores posibles son **Hour** y **Minute**.  
* **MetricsLevel**; los valores posibles son:
* **Ninguna**: desactiva la supervisión.
* **Servicio**: recopila métricas como entrada/salida, disponibilidad, latencia y porcentajes de éxito, que se agregan a los servicios Blob service, Table service y Queue service.
* **ServiceAndApi**: además de las métricas de servicio mínimas, recopila el mismo conjunto de métricas de cada operación de almacenamiento en la API de servicio Azure Storage.

Por ejemplo, el siguiente comando activa las métricas por minuto para Blob service en su cuenta de almacenamiento con un período de retención establecido en cinco días: 

> [!NOTE]
> Este comando da por supuesto que ha iniciado sesión en su suscripción de Azure mediante el comando `Connect-AzAccount`.

```powershell
$storageAccount = Get-AzStorageAccount -ResourceGroupName "<resource-group-name>" -AccountName "<storage-account-name>"

Set-AzStorageServiceMetricsProperty -MetricsType Minute -ServiceType Blob -MetricsLevel ServiceAndApi  -RetentionDays 5 -Context $storageAccount.Context
```  

* Reemplace el marcador de posición `<resource-group-name>` por el nombre del grupo de recursos.
        
* Reemplace el valor de marcador de posición `<storage-account-name>` por el nombre de la cuenta de almacenamiento.



El comando siguiente recupera el nivel de métricas por hora actual y los días de retención para el servicio BLOB en su cuenta de almacenamiento predeterminada:  

```powershell
Get-AzStorageServiceMetricsProperty -MetricsType Hour -ServiceType Blob -Context $storagecontext.Context
```  

Para más información sobre cómo configurar los cmdlets de Azure PowerShell para que funcionen con su suscripción de Azure y cómo seleccionar la cuenta de almacenamiento predeterminada que quiere usar, vea: [Cómo instalar y configurar Azure PowerShell](https://azure.microsoft.com/documentation/articles/install-configure-powershell/).  

## <a name="enable-storage-metrics-programmatically"></a>Habilitar métricas de almacenamiento mediante programación  
Aparte de usar Azure Portal o los cmdlets de Azure PowerShell para controlar métricas de almacenamiento, también se puede recurrir a una de las API de Azure Storage. Por ejemplo, si está utilizando un lenguaje .NET, puede usar la Biblioteca del cliente de almacenamiento.  

Las clases **CloudBlobClient**, **CloudQueueClient**, **CloudTableClient** y **CloudFileClient** tienen todas métodos como **SetServiceProperties** y **SetServicePropertiesAsync** que toman un objeto **ServiceProperties** como parámetro. Este objeto **ServiceProperties** se puede usar para configurar métricas de almacenamiento. Por ejemplo, el siguiente fragmento de código de C# muestra cómo cambiar el nivel de métricas y los días de retención de las métricas por hora de la cola:  

```csharp
var storageAccount = CloudStorageAccount.Parse(connStr);  
var queueClient = storageAccount.CreateCloudQueueClient();  
var serviceProperties = queueClient.GetServiceProperties();  

serviceProperties.HourMetrics.MetricsLevel = MetricsLevel.Service;  
serviceProperties.HourMetrics.RetentionDays = 10;  

queueClient.SetServiceProperties(serviceProperties);  
```  

Para obtener más información sobre cómo usar un lenguaje .NET para configurar métricas de almacenamiento, vea [Biblioteca de cliente de Azure Storage para .NET](https://msdn.microsoft.com/library/azure/mt347887.aspx).  

Para obtener información general sobre cómo configurar métricas de almacenamiento con la API de REST, vea [Habilitar y configurar Storage Analytics](/rest/api/storageservices/Enabling-and-Configuring-Storage-Analytics).  

##  <a name="viewing-storage-metrics"></a>Mostrar Métricas de almacenamiento  
Cuando haya configurado las métricas de Storage Analytics para supervisar su cuenta de almacenamiento, Storage Analytics se encargará de registrar las métricas en un conjunto de tablas conocidas en su cuenta de almacenamiento. Puede configurar gráficos para ver las métricas por hora en [Azure Portal](https://portal.azure.com):

1. Vaya a la cuenta de almacenamiento en [Azure Portal](https://portal.azure.com).
1. Seleccione **Métricas (clásicas)** en la hoja **Menú** del servicio cuyas métricas quiere ver.
1. Haga clic en el gráfico que quiera configurar.
1. En la hoja **Editar gráfico**, seleccione el **intervalo de tiempo**, el **tipo de gráfico** y las métricas que quiere mostrar en el gráfico.

En la sección **Supervisión (clásica)** de la hoja Menú de la cuenta de Azure Storage en Azure Portal, puede configurar [reglas de alerta](#metrics-alerts), como el envío de alertas de correo electrónico para avisarle cuando una métrica específica alcance un valor determinado.

Si quiere descargar las métricas para almacenarlas de forma prolongada o para analizarlas localmente, debe usar una herramienta o escribir código para leer las tablas. Debe descargar las métricas por minuto para el análisis. Las tablas no aparecen si lista todas las tablas en su cuenta de almacenamiento, pero puede acceder a ellas directamente por su nombre. Muchas herramientas de exploración de almacenamiento tienen en cuenta estas tablas y permiten verlas directamente; vaya a [Herramientas de cliente de Azure Storage](/azure/storage/storage-explorers) para ver una lista de herramientas disponibles.

||||  
|-|-|-|  
|**Métricas**|**Nombres de tabla**|**Notas**|  
|Métricas por hora|$MetricsHourPrimaryTransactionsBlob<br /><br /> $MetricsHourPrimaryTransactionsTable<br /><br /> $MetricsHourPrimaryTransactionsQueue<br /><br /> $MetricsHourPrimaryTransactionsFile|En versiones anteriores a 2013-08-15, estas tablas se denominaban:<br /><br /> $MetricsTransactionsBlob<br /><br /> $MetricsTransactionsTable<br /><br /> $MetricsTransactionsQueue<br /><br /> Las métricas del servicio File service están disponibles a partir de la versión 2015-04-05.|  
|Métricas por minuto|$MetricsMinutePrimaryTransactionsBlob<br /><br /> $MetricsMinutePrimaryTransactionsTable<br /><br /> $MetricsMinutePrimaryTransactionsQueue<br /><br /> $MetricsMinutePrimaryTransactionsFile|Solo se pueden habilitar con PowerShell o mediante programación.<br /><br /> Las métricas del servicio File service están disponibles a partir de la versión 2015-04-05.|  
|Capacity|$MetricsCapacityBlob|Solo Blob service.|  

Puede encontrar detalles completos de los esquemas para estas tablas en [Esquema de las tablas de métricas de Storage Analytics](/rest/api/storageservices/storage-analytics-metrics-table-schema). Las filas de ejemplo que tiene a continuación muestran solo un subconjunto de columnas disponibles, pero ilustran algunas características importantes acerca de la manera en que las Métricas de almacenamiento guardan estas métricas:  

||||||||||||  
|-|-|-|-|-|-|-|-|-|-|-|  
|**PartitionKey**|**RowKey**|**Timestamp**|**TotalRequests**|**TotalBillableRequests**|**TotalIngress**|**TotalEgress**|**Disponibilidad**|**AverageE2ELatency**|**AverageServerLatency**|**PercentSuccess**|  
|20140522T1100|user;All|2014-05-22T11:01:16.7650250Z|7|7|4003|46801|100|104.4286|6.857143|100|  
|20140522T1100|user;QueryEntities|2014-05-22T11:01:16.7640250Z|5|5|2694|45951|100|143.8|7.8|100|  
|20140522T1100|user;QueryEntity|2014-05-22T11:01:16.7650250Z|1|1|538|633|100|3|3|100|  
|20140522T1100|user;UpdateEntity|2014-05-22T11:01:16.7650250Z|1|1|771|217|100|9|6|100|  

En este ejemplo de datos de métricas por minuto, la clave de partición usa el tiempo en la resolución de minutos. La clave de fila identifica el tipo de información que se almacena en la fila y se compone de dos fragmentos de información, el tipo de acceso y el tipo de solicitud:  

-   El tipo de acceso es **user** o **system**, donde **user** hace referencia a todas las solicitudes de usuario al servicio de almacenamiento y **system**, a las solicitudes realizadas por Storage Analytics.  

-   El tipo de solicitud es **all**, en cuyo caso es una línea de resumen, o identifica una API específica como **QueryEntity** o **UpdateEntity**.  

Los datos de ejemplo anteriores muestran todos los registros relativos a un único minuto (a partir de las 11:00 A.M.), por lo que el número de solicitudes **QueryEntities** más el número de solicitudes **QueryEntity** más el número de solicitudes **UpdateEntity** suman siete, que es el total que se muestra en la fila **user:All**. De forma similar, puede derivar la latencia promedio de un extremo a otro 104,4286 en la fila **user:All** calculando ((143,8 x 5) + 3 + 9):7.  

## <a name="metrics-alerts"></a>Alertas de métricas
Debe considerar la posibilidad de configurar alertas en [Azure Portal](https://portal.azure.com) para avisarle automáticamente de los cambios importantes en el comportamiento de los servicios de almacenamiento. Si usa una herramienta de exploración de almacenamiento para descargar estos datos de métricas en un formato delimitado, puede usar Microsoft Excel para analizar los datos. Consulte [Herramientas de cliente de Azure Storage](/azure/storage/storage-explorers) para ver una lista de las herramientas del explorador de almacenamiento disponible. Puede configurar alertas en la hoja **Alerta (clásica)** , a la que se accede desde **Supervisión (clásica)** en la hoja Menú de la cuenta de Storage.

> [!IMPORTANT]
> Puede producirse un retraso entre un evento de almacenamiento y cuándo se registran los datos de métricas por hora o minuto correspondientes. En el caso de las métricas por minuto, es posible que se escriban varios minutos de datos de una vez. Esto puede conllevar que las transacciones de minutos anteriores se sumen a la transacción del minuto actual. Cuando esto sucede, es posible que el servicio de alerta carezca de todos los datos de métricas disponibles para el intervalo de alerta configurado, lo que podría provocar que se activaran alertas de forma inesperada.
>

## <a name="accessing-metrics-data-programmatically"></a>Acceso a los datos de métricas mediante programación  
El listado siguiente muestra código C# de ejemplo que accede a las métricas por minuto para un intervalo de minutos y muestra los resultados en una ventana de la consola. En el código de ejemplo se usa la biblioteca de cliente de Azure Storage versión 4.x o posterior, que incluye la clase **CloudAnalyticsClient**, que simplifica el acceso a las tablas de métricas en el almacenamiento.  

```csharp
private static void PrintMinuteMetrics(CloudAnalyticsClient analyticsClient, DateTimeOffset startDateTime, DateTimeOffset endDateTime)  
{  
 // Convert the dates to the format used in the PartitionKey  
 var start = startDateTime.ToUniversalTime().ToString("yyyyMMdd'T'HHmm");  
 var end = endDateTime.ToUniversalTime().ToString("yyyyMMdd'T'HHmm");  

 var services = Enum.GetValues(typeof(StorageService));  
 foreach (StorageService service in services)  
 {  
     Console.WriteLine("Minute Metrics for Service {0} from {1} to {2} UTC", service, start, end);  
     var metricsQuery = analyticsClient.CreateMinuteMetricsQuery(service, StorageLocation.Primary);  
     var t = analyticsClient.GetMinuteMetricsTable(service);  
     var opContext = new OperationContext();  
     var query =  
             from entity in metricsQuery  
             // Note, you can't filter using the entity properties Time, AccessType, or TransactionType  
             // because they are calculated fields in the MetricsEntity class.  
             // The PartitionKey identifies the DataTime of the metrics.  
             where entity.PartitionKey.CompareTo(start) >= 0 && entity.PartitionKey.CompareTo(end) <= 0   
             select entity;  

     // Filter on "user" transactions after fetching the metrics from Table Storage.  
     // (StartsWith is not supported using LINQ with Azure Table storage)  
     var results = query.ToList().Where(m => m.RowKey.StartsWith("user"));  
     var resultString = results.Aggregate(new StringBuilder(), (builder, metrics) => builder.AppendLine(MetricsString(metrics, opContext))).ToString();  
     Console.WriteLine(resultString);  
 }  
}  

private static string MetricsString(MetricsEntity entity, OperationContext opContext)  
{  
 var entityProperties = entity.WriteEntity(opContext);  
 var entityString =  
         string.Format("Time: {0}, ", entity.Time) +  
         string.Format("AccessType: {0}, ", entity.AccessType) +  
         string.Format("TransactionType: {0}, ", entity.TransactionType) +  
         string.Join(",", entityProperties.Select(e => new KeyValuePair<string, string>(e.Key.ToString(), e.Value.PropertyAsObject.ToString())));  
 return entityString;  
}  
```  

## <a name="billing-on-storage-metrics"></a>Facturación de las métricas de almacenamiento  
Las solicitudes por escrito para crear entidades de tabla para métricas se cobran con las tarifas estándar aplicables a todas las operaciones de Azure Storage.  

Las solicitudes de lectura y eliminación de métricas realizadas por un cliente también se facturan con las tarifas estándar. Si ha configurado una directiva de retención de datos, no se le cobrará cuando Azure Storage elimine datos de métricas antiguos. Sin embargo, si elimina datos de análisis, se cobrará a su cuenta las operaciones de eliminación.  

La capacidad usada por las tablas de métricas también es facturable. Puede usar las opciones siguientes para calcular la cantidad de capacidad usada para almacenar datos de métricas:  

-   Si cada hora un servicio utiliza todas las API en todos los servicios, se almacenan aproximadamente 148 KB de datos cada hora en las tablas de transacciones de métricas, si se ha habilitado el resumen tanto de nivel servicio como de nivel de API.  
-   Si, dentro de una misma hora, un servicio utiliza todas las API en el servicio, se almacenan aproximadamente 12 KB de datos cada hora en las tablas de transacciones de métricas, si ha habilitado solo el resumen de nivel de servicio.  
-   A la tabla de capacidad de blobs se le agregan dos filas cada día, siempre que se haya suscrito a los registros. Esto implica que todos los días el tamaño de esta tabla aumenta en 300 bytes aproximadamente.

## <a name="next-steps"></a>Pasos siguientes
* [Supervisión de una cuenta de almacenamiento en Azure Portal](https://www.windowsazure.com/manage/services/storage/how-to-monitor-a-storage-account/)   
* [Esquema de las tablas de métricas de Storage Analytics](/rest/api/storageservices/storage-analytics-metrics-table-schema)   
* [Operaciones y mensajes de estado registrados por Storage Analytics](/rest/api/storageservices/storage-analytics-logged-operations-and-status-messages)   
* [Registro de Storage Analytics](storage-analytics-logging.md)
