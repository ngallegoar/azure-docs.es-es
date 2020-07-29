---
title: Métricas compatibles de Azure Monitor por tipo de recurso
description: Lista de métricas disponibles para cada tipo de recurso con Azure Monitor.
author: rboucher
services: azure-monitor
ms.topic: reference
ms.date: 06/16/2020
ms.author: robb
ms.subservice: metrics
ms.openlocfilehash: e8bae2062051156d6de378e54bc354b3f785e403
ms.sourcegitcommit: 3543d3b4f6c6f496d22ea5f97d8cd2700ac9a481
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/20/2020
ms.locfileid: "86515468"
---
# <a name="supported-metrics-with-azure-monitor"></a>Métricas compatibles con Azure Monitor

> [!NOTE]
> Esta lista se genera de forma automática a partir de la API REST de métricas de Azure Monitor. Cualquier modificación realizada en esta lista a través de GitHub puede sobrescribirse sin previo aviso. Póngase en contacto con el autor de este artículo para obtener más información sobre cómo hacer actualizaciones permanentes.

Azure Monitor proporciona varias maneras de interactuar con las métricas, como la representación en gráficos en el portal, el acceso a ellas a través de la API de REST o consultarlas con PowerShell o la CLI. 

Este artículo es una lista completa de todas las métricas de la plataforma (recopiladas automáticamente) que actualmente están disponibles con la canalización de métricas consolidada de Azure Monitor. La lista se actualizó por última vez el 27 de marzo de 2020. Es posible que las métricas cambiadas o agregadas después de esta fecha no aparezcan a continuación. Para consultar y obtener acceso a la lista de métricas mediante programación, use [2018-01-01 api-version](/rest/api/monitor/metricdefinitions). Otras métricas que no aparecen en esta lista pueden estar disponibles en el portal o mediante las API heredadas.

Las métricas están organizadas en función de los proveedores de recursos y el tipo de recurso. Para obtener una lista de los servicios y los proveedores de recursos que pertenecen a las métricas, consulte [Proveedores de recursos para servicios de Azure](../../azure-resource-manager/management/azure-services-resource-providers.md). 


## <a name="guest-os-metrics"></a>Métricas de SO invitado

Las métricas del sistema operativo invitado (SO invitado) que se ejecuta en Azure Virtual Machines, Service Fabric y Cloud Services **NO** aparecen aquí. En su lugar, las métricas de rendimiento del sistema operativo invitado se deben recopilar a través de uno o varios agentes que se ejecuten en el sistema operativo invitado o como parte de él.  Las métricas del sistema operativo invitado incluyen los contadores de rendimiento que realizan el seguimiento del porcentaje de las CPU invitadas o el uso de la memoria, ya que se usan con frecuencia para el realizar el escalado automático o las alertas.  Mediante la [extensión de Azure Diagnostics](diagnostics-extension-overview.md), puede enviar métricas de rendimiento del sistema operativo invitado a la misma base de datos donde se almacenan las métricas de la plataforma. Esta opción enruta las métricas del sistema operativo invitado a través de la API de [métricas personalizadas](metrics-custom-overview.md). A continuación, puede crear un gráfico, alertas o usar las métricas del sistema operativo invitado como las métricas de la plataforma. Para más información, consulte [Información general sobre los agentes de supervisión](agents-overview.md).    

## <a name="routing-platform-metrics-to-other-locations"></a>Enrutamiento de métricas de plataforma a otras ubicaciones

Puede usar la [configuración de diagnósticos](diagnostic-settings.md) para enrutar las métricas de la plataforma a Azure Storage, a los registros de Azure Monitor (y, por tanto, Log Analytics) y a Event Hubs.  

Existen algunas limitaciones en cuanto al contenido que se puede enrutar y al formulario en el que se almacenan las métricas. 
- Recuerde que no todas las métricas son exportables a otras ubicaciones. Para obtener una lista de las métricas de plataforma exportables a través de la configuración de diagnóstico, consulte [este artículo](metrics-supported-export-diagnostic-settings.md).

- Actualmente no se admite el envío de métricas de varias dimensiones a otras ubicaciones a través de la configuración de diagnóstico. Las métricas con dimensiones se exportan como métricas unidimensionales planas agregadas a través de los valores de dimensión.
*Por ejemplo*: la métrica "Mensajes entrantes" de una instancia de Event Hub se puede explorar y representar gráficamente por colas. Sin embargo, cuando se exporta a través de la configuración de diagnóstico, la métrica se representará con todos los mensajes entrantes de todas las colas de Event Hub.


## <a name="microsoftanalysisservicesservers"></a>Microsoft.AnalysisServices/servers

|Métrica|Nombre de métrica para mostrar|Unidad|Tipo de agregación|Descripción|Dimensions|
|---|---|---|---|---|---|
|qpu_metric|QPU|Count|Average|QPU. Intervalo de 0-100 para S1, 0-200 para S2 y 0-400 para S4|ServerResourceType|
|memory_metric|Memoria|Bytes|Average|Memoria. Intervalo de 0-25 GB para S1, 0-50 GB para S2 y 0-100 GB para S4|ServerResourceType|
|private_bytes_metric|Bytes privados|Bytes|Average|Bytes privados.|ServerResourceType|
|virtual_bytes_metric|Bytes virtuales|Bytes|Average|Bytes virtuales.|ServerResourceType|
|TotalConnectionRequests|Número total de solicitudes de conexión|Count|Average|Número total de solicitudes de conexión. Se trata de llegadas.|ServerResourceType|
|SuccessfullConnectionsPerSec|Conexiones correctas por segundo|CountPerSecond|Average|Tasa de finalizaciones de conexión correctas.|ServerResourceType|
|TotalConnectionFailures|Número total de errores de conexión|Count|Average|Número total de intentos de conexión con error.|ServerResourceType|
|CurrentUserSessions|Sesiones de usuario actuales|Count|Average|Número actual de sesiones de usuario establecidas.|ServerResourceType|
|QueryPoolBusyThreads|Subprocesos ocupados de grupo de consultas|Count|Average|Número de subprocesos ocupados del grupo de subprocesos de consulta.|ServerResourceType|
|CommandPoolJobQueueLength|Longitud de cola de trabajos de grupo de comandos|Count|Average|Número de trabajos en la cola del grupo de subprocesos de comandos.|ServerResourceType|
|ProcessingPoolJobQueueLength|Longitud de cola de trabajos de grupo de procesamiento|Count|Average|Número de trabajos que no son de E/S en la cola del grupo de subprocesos de procesamiento.|ServerResourceType|
|CurrentConnections|Conexión: Conexiones actuales|Count|Average|Número actual de conexiones de cliente establecidas.|ServerResourceType|
|CleanerCurrentPrice|Memoria: precio actual de limpieza|Count|Average|Precio actual de memoria, USD/byte/tiempo, normalizado a 1000.|ServerResourceType|
|CleanerMemoryShrinkable|Memoria: memoria de limpieza reducible|Bytes|Average|Cantidad de memoria, en bytes, que purgará el limpiador en segundo plano.|ServerResourceType|
|CleanerMemoryNonshrinkable|Memoria: memoria de limpieza no reducible|Bytes|Average|Cantidad de memoria, en bytes, que no purgará el limpiador en segundo plano.|ServerResourceType|
|MemoryUsage|Memoria: Uso de la memoria|Bytes|Average|Uso de memoria del proceso de servidor tal como se usa para calcular el precio de la memoria del limpiador. Es igual al contador Proceso\Bytes privados más el tamaño de los datos asignados a la memoria; se ignora la memoria que ha asignado VertiPaq superior al límite de memoria de VertiPaq.|ServerResourceType|
|MemoryLimitHard|Memoria: límite de memoria física|Bytes|Average|Límite de memoria física del archivo de configuración.|ServerResourceType|
|MemoryLimitHigh|Memoria: límite alto de memoria|Bytes|Average|Límite alto de memoria del archivo de configuración.|ServerResourceType|
|MemoryLimitLow|Memoria: límite bajo de memoria|Bytes|Average|Límite bajo de memoria del archivo de configuración.|ServerResourceType|
|MemoryLimitVertiPaq|Memoria: VertiPaq de límite de memoria|Bytes|Average|Límite en memoria del archivo de configuración.|ServerResourceType|
|Quota|Memoria: Quota|Bytes|Average|Cuota de memoria actual, en bytes. La cuota de memoria también se denomina concesión de memoria o reserva de memoria.|ServerResourceType|
|QuotaBlocked|Memoria: cuota bloqueada|Count|Average|Número actual de solicitudes de cuota que están bloqueadas hasta que se liberen otras cuotas de memoria.|ServerResourceType|
|VertiPaqNonpaged|Memoria: VertiPaq no paginado|Bytes|Average|Bytes de memoria bloqueados en el espacio de trabajo para que los use el motor en memoria.|ServerResourceType|
|VertiPaqPaged|Memoria: VertiPaq paginado|Bytes|Average|Bytes de memoria paginada en uso para datos en memoria.|ServerResourceType|
|RowsReadPerSec|Procesamiento: filas leídas por segundo|CountPerSecond|Average|Velocidad de filas leídas de todas las bases de datos relacionales.|ServerResourceType|
|RowsConvertedPerSec|Procesamiento: filas convertidas por segundo|CountPerSecond|Average|Velocidad de filas convertidas durante el procesamiento.|ServerResourceType|
|RowsWrittenPerSec|Procesamiento: filas escritas por segundo|CountPerSecond|Average|Velocidad de filas escritas durante el procesamiento.|ServerResourceType|
|CommandPoolBusyThreads|Subprocesos: subprocesos ocupados del grupo de comandos|Count|Average|Número de subprocesos ocupados del grupo de subprocesos de comandos.|ServerResourceType|
|CommandPoolIdleThreads|Subprocesos: subprocesos inactivos del grupo de comandos|Count|Average|Número de subprocesos inactivos del grupo de subprocesos de comandos.|ServerResourceType|
|LongParsingBusyThreads|Subprocesos: subprocesos ocupados en análisis largos|Count|Average|Número de subprocesos ocupados del grupo de subprocesos de análisis largos.|ServerResourceType|
|LongParsingIdleThreads|Subprocesos: subprocesos inactivos en análisis largos|Count|Average|Número de subprocesos inactivos del grupo de subprocesos en análisis largos.|ServerResourceType|
|LongParsingJobQueueLength|Subprocesos: longitud de cola de trabajos en análisis largos|Count|Average|Número de trabajos en la cola del grupo de subprocesos en análisis largos.|ServerResourceType|
|ProcessingPoolBusyIOJobThreads|Subprocesos: subprocesos de trabajo de E/S ocupados del grupo de procesamiento|Count|Average|Número de subprocesos que ejecutan trabajos de E/S en el grupo de subprocesos de procesamiento.|ServerResourceType|
|ProcessingPoolBusyNonIOThreads|Subprocesos: subprocesos de trabajo ocupados que no son de E/S del grupo de procesamiento|Count|Average|Número de subprocesos que ejecutan trabajos que no son de E/S del grupo de subprocesos de procesamiento.|ServerResourceType|
|ProcessingPoolIOJobQueueLength|Subprocesos: Longitud de cola de trabajos de E/S de grupo de procesamiento|Count|Average|Número de trabajos de E/S en la cola del grupo de subprocesos de procesamiento.|ServerResourceType|
|ProcessingPoolIdleIOJobThreads|Subprocesos: subprocesos de trabajo de E/S inactivos del grupo de procesamiento|Count|Average|Número de subprocesos inactivos para trabajos de E/S en el grupo de subprocesos de procesamiento.|ServerResourceType|
|ProcessingPoolIdleNonIOThreads|Subprocesos: subprocesos de trabajo inactivos que no son de E/S del grupo de procesamiento|Count|Average|Número de subprocesos inactivos del grupo de subprocesos de procesamiento dedicado a trabajos que no son de E/S.|ServerResourceType|
|QueryPoolIdleThreads|Subprocesos: subprocesos inactivos del grupo de consultas|Count|Average|Número de subprocesos inactivos para trabajos de E/S en el grupo de subprocesos de procesamiento.|ServerResourceType|
|QueryPoolJobQueueLength|Subprocesos: longitud de cola de trabajos del grupo de consultas|Count|Average|Número de trabajos en la cola del grupo de subprocesos de consultas.|ServerResourceType|
|ShortParsingBusyThreads|Subprocesos: subprocesos ocupados en análisis cortos|Count|Average|Número de subprocesos ocupados del grupo de subprocesos de análisis cortos.|ServerResourceType|
|ShortParsingIdleThreads|Subprocesos: subprocesos inactivos en análisis cortos|Count|Average|Número de subprocesos inactivos del grupo de subprocesos de análisis cortos.|ServerResourceType|
|ShortParsingJobQueueLength|Subprocesos: longitud de cola de trabajos en análisis cortos|Count|Average|Número de trabajos en la cola del grupo de subprocesos en análisis cortos.|ServerResourceType|
|memory_thrashing_metric|Paginación excesiva de memoria|Percent|Average|Paginación excesiva media de memoria.|ServerResourceType|
|mashup_engine_qpu_metric|QPU de motor M|Count|Average|Uso de QPU por los procesos del motor de mashup|ServerResourceType|
|mashup_engine_memory_metric|Memoria del motor M|Bytes|Average|Uso de memoria por los procesos del motor de mashup|ServerResourceType|
|mashup_engine_private_bytes_metric|Bytes privados del motor M|Bytes|Average|Uso de bytes privados en procesos de motor de mashup.|ServerResourceType|
|mashup_engine_virtual_bytes_metric|Bytes virtuales del motor M|Bytes|Average|Uso de bytes virtuales en procesos de motor de mashup.|ServerResourceType|


## <a name="microsoftapimanagementservice"></a>Microsoft.ApiManagement/service

|Métrica|Nombre de métrica para mostrar|Unidad|Tipo de agregación|Descripción|Dimensions|
|---|---|---|---|---|---|
|TotalRequests|Total Gateway Requests (Deprecated) [Solicitudes de puerta de enlace en total (en desuso)]|Count|Total|Número de solicitudes de puerta de enlace: use la métrica de solicitud de varias dimensiones con la dimensión GatewayResponseCodeCategory en su lugar.|Location,Hostname|
|SuccessfulRequests|Successful Gateway Requests (Deprecated) [Solicitudes de puerta de enlace correctas (en desuso)]|Count|Total|Número de solicitudes de puerta de enlace correctas: use la métrica de solicitud de varias dimensiones con la dimensión GatewayResponseCodeCategory en su lugar.|Location,Hostname|
|UnauthorizedRequests|Unauthorized Gateway Requests (Deprecated) [Solicitudes de puerta de enlace no autorizadas (en desuso)]|Count|Total|Número de solicitudes de puerta de enlace no autorizadas: use la métrica de solicitud de varias dimensiones con la dimensión GatewayResponseCodeCategory en su lugar.|Location,Hostname|
|FailedRequests|Failed Gateway Requests (Deprecated) [Solicitudes de puerta de enlace erróneas (en desuso)]|Count|Total|Número de errores en las solicitudes de puerta de enlace: use la métrica de solicitud de varias dimensiones con la dimensión GatewayResponseCodeCategory en su lugar.|Location,Hostname|
|OtherRequests|Other Gateway Requests (Deprecated) [Otras solicitudes de puerta de enlace (en desuso)]|Count|Total|Número de solicitudes de puerta de enlace de otro tipo: use la métrica de solicitud de varias dimensiones con la dimensión GatewayResponseCodeCategory en su lugar.|Location,Hostname|
|Duration|Duración total de las solicitudes de puerta de enlace|Milisegundos|Average|Duración total de las solicitudes de puerta de enlace en milisegundos|Location,Hostname|
|BackendDuration|Duration of Backend Requests (Duración de las solicitudes de back-end)|Milisegundos|Average|Duración de las solicitudes de back-end en milisegundos|Location,Hostname|
|Capacity|Capacity|Percent|Average|Métrica de uso para el servicio ApiManagement|Location|
|EventHubTotalEvents|Eventos totales de EventHub|Count|Total|Número de eventos enviados a EventHub|Location|
|EventHubSuccessfulEvents|Eventos EventHub correctos|Count|Total|Número de eventos EventHub correctos|Location|
|EventHubTotalFailedEvents|Eventos EventHub con errores|Count|Total|Número de eventos EventHub con errores|Location|
|EventHubRejectedEvents|Eventos EventHub rechazados|Count|Total|Número de eventos EventHub rechazados (configuración incorrecta o no autorizados)|Location|
|EventHubThrottledEvents|Eventos EventHub limitados|Count|Total|Número de eventos EventHub limitados|Location|
|EventHubTimedoutEvents|Eventos EventHub con tiempo de espera agotado|Count|Total|Número de eventos EventHub con tiempo de espera agotado|Location|
|EventHubDroppedEvents|Eventos EventHub quitados|Count|Total|Número de eventos omitidos por haber alcanzado el límite del tamaño de la cola|Location|
|EventHubTotalBytesSent|Tamaño de los eventos EventHub|Bytes|Total|Tamaño total de los eventos EventHub en bytes|Location|
|Requests|Requests|Count|Total|Métricas de solicitud de puerta de enlace con varias dimensiones|Location,Hostname,LastErrorReason,BackendResponseCode,GatewayResponseCode,BackendResponseCodeCategory,GatewayResponseCodeCategory|
|NetworkConnectivity|Estado de conectividad de red de los recursos (versión preliminar)|Count|Total|Estado de conectividad de red de los tipos de recursos dependientes del servicio de API Management|Location,ResourceType|


## <a name="microsoftappconfigurationconfigurationstores"></a>Microsoft.AppConfiguration/configurationStores

|Métrica|Nombre de métrica para mostrar|Unidad|Tipo de agregación|Descripción|Dimensions|
|---|---|---|---|---|---|
|HttpIncomingRequestCount|HttpIncomingRequestCount|Count|Count|Número total de solicitudes HTTP entrantes.|StatusCode|
|HttpIncomingRequestDuration|HttpIncomingRequestDuration|Count|Average|Latencia de una solicitud HTTP.|StatusCode|


## <a name="microsoftappplatformspring"></a>Microsoft.AppPlatform/Spring

|Métrica|Nombre de métrica para mostrar|Unidad|Tipo de agregación|Descripción|Dimensions|
|---|---|---|---|---|---|
|SystemCpuUsagePercentage|System CPU Usage Percentage (Porcentaje de uso de CPU del sistema)|Percent|Average|Uso de CPU reciente de todo el sistema|AppName,Pod|
|AppCpuUsagePercentage|App CPU Usage Percentage (Porcentaje de uso de CPU de aplicaciones)|Percent|Average|Porcentaje de uso de CPU de la JVM de aplicaciones|AppName,Pod|
|AppMemoryCommitted|App Memory Assigned (Memoria de la aplicación asignada)|Bytes|Average|Memoria asignada a la JVM en bytes|AppName,Pod|
|AppMemoryUsed|App Memory Used (Memoria de la aplicación utilizada)|Bytes|Average|Memoria de la aplicación utilizada en bytes|AppName,Pod|
|AppMemoryMax|App Memory Max (Memoria máxima de la aplicación)|Bytes|Máxima|Cantidad máxima de memoria en bytes que se puede usar para la administración de memoria.|AppName,Pod|
|MaxOldGenMemoryPoolBytes|Max Available Old Generation Data Size (Tamaño máximo de datos de generación anterior disponible)|Bytes|Average|Tamaño máximo del grupo de memoria de generación anterior|AppName,Pod|
|OldGenMemoryPoolBytes|Old Generation Data Size (Tamaño de datos de generación anterior)|Bytes|Average|Tamaño máximo del grupo de memoria de generación anterior después de una GC|AppName,Pod|
|OldGenPromotedBytes|Promote to Old Generation Data Size (Promoción al tamaño de datos de generación anterior)|Bytes|Máxima|Recuento de aumentos positivos en el tamaño del grupo de memoria de generación anterior entre antes y después de una GC|AppName,Pod|
|YoungGenPromotedBytes|Promote to Young Generation Data Size (Promoción al tamaño de datos de nueva generación)|Bytes|Máxima|Se incrementa por un aumento del tamaño del grupo de memoria de generación nueva después de una GC y antes de la siguiente.|AppName,Pod|
|GCPauseTotalCount|GC Pause Count (Recuento de pausas de la GC)|Count|Total|Recuento de pausas de la GC|AppName,Pod|
|GCPauseTotalTime|GC Pause Total Time (Tiempo total de pausas de la GC)|Milisegundos|Total|Tiempo total de pausas de la GC|AppName,Pod|
|TomcatSentBytes|Tomcat Total Sent Bytes (Total de bytes enviados de Tomcat)|Bytes|Total|Total de bytes enviados de Tomcat|AppName,Pod|
|TomcatReceivedBytes|Tomcat Total Received Bytes (Total de bytes recibidos de Tomcat)|Bytes|Total|Total de bytes recibidos de Tomcat|AppName,Pod|
|TomcatRequestTotalTime|Tomcat Request Total Times (Tiempo total de la solicitud de Tomcat)|Milisegundos|Total|Tiempo total de las solicitudes de Tomcat|AppName,Pod|
|TomcatRequestTotalCount|Tomcat Request Total Count (Recuento total de las solicitudes de Tomcat)|Count|Total|Recuento total de las solicitudes de Tomcat|AppName,Pod|
|TomcatResponseAvgTime|Tomcat Request Average Time (Tiempo medio de las solicitudes de Tomcat)|Milisegundos|Average|Tiempo medio de las solicitudes de Tomcat|AppName,Pod|
|TomcatRequestMaxTime|Tomcat Request Max Time (Tiempo máximo de las solicitudes de Tomcat)|Milisegundos|Máxima|Tiempo máximo de las solicitudes de Tomcat|AppName,Pod|
|TomcatErrorCount|Tomcat Global Error (Error global de Tomcat)|Count|Total|Error global de Tomcat|AppName,Pod|
|TomcatSessionActiveMaxCount|Tomcat Session Max Active Count (Recuento activo máximo de las sesiones de Tomcat)|Count|Total|Recuento activo máximo de las sesiones de Tomcat|AppName,Pod|
|TomcatSessionAliveMaxTime|Tomcat Session Max Alive Time (Tempo activo máximo de las sesiones de Tomcat)|Milisegundos|Máxima|Tempo activo máximo de las sesiones de Tomcat|AppName,Pod|
|TomcatSessionActiveCurrentCount|Tomcat Session Alive Count (Recuento activo de las sesiones de Tomcat)|Count|Total|Recuento activo de las sesiones de Tomcat|AppName,Pod|
|TomcatSessionCreatedCount|Tomcat Session Created Count (Recuento de sesiones creadas de Tomcat)|Count|Total|Recuento de sesiones creadas de Tomcat|AppName,Pod|
|TomcatSessionExpiredCount|Tomcat Session Expired Count (Recuento de sesiones expiradas de Tomcat)|Count|Total|Recuento de sesiones expiradas de Tomcat|AppName,Pod|
|TomcatSessionRejectedCount|Tomcat Session Rejected Count (Recuento de sesiones rechazadas de Tomcat)|Count|Total|Recuento de sesiones rechazadas de Tomcat|AppName,Pod|


## <a name="microsoftautomationautomationaccounts"></a>Microsoft.Automation/automationAccounts

|Métrica|Nombre de métrica para mostrar|Unidad|Tipo de agregación|Descripción|Dimensions|
|---|---|---|---|---|---|
|TotalJob|Número total de trabajos|Count|Total|El número total de trabajos (jobs)|Runbook,Status|
|TotalUpdateDeploymentRuns|Ejecuciones de implementación de actualizaciones totales|Count|Total|Ejecuciones de implementación de actualizaciones de software totales|SoftwareUpdateConfigurationName,Status|
|TotalUpdateDeploymentMachineRuns|Ejecuciones de máquina de implementación de actualizaciones totales|Count|Total|Ejecuciones de máquina de implementación de actualizaciones de software totales en una ejecución de implementación de actualizaciones de software|SoftwareUpdateConfigurationName,Status,TargetComputer,SoftwareUpdateConfigurationRunId|


## <a name="microsoftbatchbatchaccounts"></a>Microsoft.Batch/batchAccounts

|Métrica|Nombre de métrica para mostrar|Unidad|Tipo de agregación|Descripción|Dimensions|
|---|---|---|---|---|---|
|CoreCount|Recuento de núcleos dedicados|Count|Total|Número total de núcleos dedicados de la cuenta de Batch|None|
|TotalNodeCount|Recuento de nodos dedicados|Count|Total|Número total de nodos dedicados de la cuenta de Batch|None|
|LowPriorityCoreCount|Recuento de núcleos de baja prioridad|Count|Total|Número total de núcleos de baja prioridad de la cuenta de Batch|None|
|TotalLowPriorityNodeCount|Recuento de nodos de baja prioridad|Count|Total|Número total de nodos de baja prioridad de la cuenta de Batch|None|
|CreatingNodeCount|Recuento de nodos creados|Count|Total|Número de nodos que se van a crear|None|
|StartingNodeCount|Recuento de nodos de inicio|Count|Total|Número de nodos que se van a iniciar|None|
|WaitingForStartTaskNodeCount|Recuento de nodos esperando a la tarea de inicio|Count|Total|Número de nodos que esperan a que se complete la tarea de inicio|None|
|StartTaskFailedNodeCount|Recuento de nodos con error de la tarea de inicio|Count|Total|Número de nodos con error en la tarea de inicio|None|
|IdleNodeCount|Recuento de nodos inactivos|Count|Total|Número de nodos inactivos|None|
|OfflineNodeCount|Recuento de nodos sin conexión|Count|Total|Número de nodos sin conexión|None|
|RebootingNodeCount|Recuento de nodos de reinicio|Count|Total|Número de nodos de reinicio|None|
|ReimagingNodeCount|Recuento de nodos de restablecimiento de imagen inicial|Count|Total|Número de nodos de restablecimiento de imagen inicial|None|
|RunningNodeCount|Recuento de nodos en ejecución|Count|Total|Número de nodos en ejecución|None|
|LeavingPoolNodeCount|Recuento de nodos que salen del grupo|Count|Total|Número de nodos que abandonan el grupo|None|
|UnusableNodeCount|Recuento de nodos no utilizables|Count|Total|Número de nodos inutilizables|None|
|PreemptedNodeCount|Recuento de nodos con prioridad|Count|Total|Número de nodos con prioridad|None|
|TaskStartEvent|Eventos de inicio de tarea|Count|Total|Número total de tareas que se han iniciado|poolId,jobId|
|TaskCompleteEvent|Eventos de tarea completada|Count|Total|Número total de tareas que se han completado|poolId,jobId|
|TaskFailEvent|Eventos de error en tareas|Count|Total|Número total de tareas que se han completado con errores|poolId,jobId|
|PoolCreateEvent|Eventos de creación de grupo|Count|Total|Número total de grupos que se han creado|poolId|
|PoolResizeStartEvent|Eventos de inicio de cambio de tamaño de grupo|Count|Total|Número total de cambios de tamaño de grupo que se han iniciado|poolId|
|PoolResizeCompleteEvent|Eventos de finalización de cambio de tamaño de grupo|Count|Total|Número total de cambios de tamaño de grupo completados|poolId|
|PoolDeleteStartEvent|Eventos de inicio de eliminación de grupo|Count|Total|Número total de eliminaciones de grupo que se han iniciado|poolId|
|PoolDeleteCompleteEvent|Eventos de finalización de eliminaciones de grupo|Count|Total|Número total de eliminaciones de grupo que se han completado|poolId|
|JobDeleteCompleteEvent|Eventos de finalización de eliminaciones de trabajo|Count|Total|Número total de trabajos que se han eliminado correctamente.|jobId|
|JobDeleteStartEvent|Eventos de inicio de eliminaciones de trabajo|Count|Total|Número total de trabajos cuya eliminación se ha solicitado.|jobId|
|JobDisableCompleteEvent|Eventos de finalización de deshabilitación de trabajo|Count|Total|Número total de trabajos que se han deshabilitado correctamente.|jobId|
|JobDisableStartEvent|Eventos de inicio de deshabilitación de trabajo|Count|Total|Número total de trabajos cuya deshabilitación se ha solicitado.|jobId|
|JobStartEvent|Eventos de inicio de trabajo|Count|Total|Número total de trabajos que se han iniciado correctamente.|jobId|
|JobTerminateCompleteEvent|Eventos de finalización de terminaciones de trabajo|Count|Total|Número total de trabajos que se han finalizado correctamente.|jobId|
|JobTerminateStartEvent|Eventos de inicio de terminaciones de trabajo|Count|Total|Número total de trabajos cuya terminación se ha solicitado.|jobId|


## <a name="microsoftbatchaiworkspaces"></a>Microsoft.BatchAI/workspaces

|Métrica|Nombre de métrica para mostrar|Unidad|Tipo de agregación|Descripción|Dimensions|
|---|---|---|---|---|---|
|Job Submitted|Job Submitted (Trabajo enviado)|Count|Total|Número de trabajos enviados|Scenario,ClusterName|
|Job Completed|Job Completed (Trabajo completado)|Count|Total|Trabajo completado|Scenario,ClusterName,ResultType|
|Total Nodes|Total Nodes (Nodos totales)|Count|Average|Número de nodos totales|Scenario,ClusterName|
|Active Nodes|Active Nodes (Nodos activos)|Count|Average|Número de nodos en ejecución|Scenario,ClusterName|
|Idle Nodes|Idle Nodes (Nodos inactivos)|Count|Average|Número de nodos inactivos|Scenario,ClusterName|
|Nodos no utilizables|Nodos no utilizables|Count|Average|Número de nodos inutilizables|Scenario,ClusterName|
|Nodos con prioridad|Preempted Nodes (Nodos con prioridad)|Count|Average|Número de nodos con prioridad|Scenario,ClusterName|
|Leaving Nodes|Leaving Nodes (Nodos de salida)|Count|Average|Número de nodos de salida|Scenario,ClusterName|
|Núcleos totales|Total Cores (Núcleos totales)|Count|Average|Número de núcleos totales|Scenario,ClusterName|
|Núcleos activos|Active Cores (Núcleos activos)|Count|Average|Número de núcleos activos|Scenario,ClusterName|
|Núcleos inactivos|Idle Cores (Núcleos inactivos)|Count|Average|Número de núcleos inactivos|Scenario,ClusterName|
|Núcleos no utilizables|Unusable Cores (Núcleos no utilizables)|Count|Average|Número de núcleos no utilizables|Scenario,ClusterName|
|Núcleos con prioridad|Preempted Cores (Núcleos con prioridad)|Count|Average|Número de núcleos con prioridad|Scenario,ClusterName|
|Núcleos de salida|Leaving Cores (Núcleos de salida)|Count|Average|Número de núcleos de salida|Scenario,ClusterName|
|Porcentaje de uso de la cuota|Porcentaje de uso de la cuota|Count|Average|Porcentaje de cuota utilizada|Scenario,ClusterName,VmFamilyName,VmPriority|

## <a name="microsoftblockchainblockchainmembers"></a>Microsoft.Blockchain/blockchainMembers

|Métrica|Nombre de métrica para mostrar|Unidad|Tipo de agregación|Descripción|Dimensions|
|---|---|---|---|---|---|
|CpuUsagePercentageInDouble|CPU Usage Percentage (Porcentaje de uso de CPU)|Percent|Máxima|Porcentaje de uso de CPU|Nodo|
|MemoryUsage|Uso de la memoria|Bytes|Average|Uso de la memoria|Nodo|
|MemoryLimit|Memory Limit (Límite de memoria)|Bytes|Average|Límite de memoria|Nodo|
|MemoryUsagePercentageInDouble|Memory Usage Percentage (Porcentaje de uso de memoria)|Percent|Average|Porcentaje de uso de memoria|Nodo|
|StorageUsage|Storage Usage (Uso del almacenamiento)|Bytes|Average|Uso del almacenamiento|Nodo|
|IOReadBytes|IO Read Bytes (Bytes de lectura de E/S)|Bytes|Total|Bytes de lectura de E/S|Nodo|
|IOWriteBytes|IO Write Bytes (Bytes de escritura de E/S)|Bytes|Total|Bytes de escritura de E/S|Nodo|
|ConnectionAccepted|Accepted Connections (Conexiones aceptadas)|Count|Total|Conexiones aceptadas|Nodo|
|ConnectionHandled|Handled Connections (Conexiones controladas)|Count|Total|Conexiones controladas|Nodo|
|ConnectionActive|Conexiones activas|Count|Average|Conexiones activas|Nodo|
|RequestHandled|Handled Requests (Solicitudes controladas)|Count|Total|Solicitudes controladas|Nodo|
|ProcessedBlocks|Processed Blocks (Bloques procesados)|Count|Total|Bloques procesados|Nodo|
|ProcessedTransactions|Processed Transactions (Transacciones procesadas)|Count|Total|Transacciones procesadas|Nodo|
|PendingTransactions|Pending Transactions (Transacciones pendientes)|Count|Average|Transacciones pendientes|Nodo|
|QueuedTransactions|Queued Transactions (Transacciones en cola)|Count|Average|Transacciones en cola|Nodo|



## <a name="microsoftcacheredis"></a>Microsoft.Cache/redis

|Métrica|Nombre de métrica para mostrar|Unidad|Tipo de agregación|Descripción|Dimensions|
|---|---|---|---|---|---|
|connectedclients|Clientes conectados|Count|Máxima||ShardId|
|totalcommandsprocessed|Total de operaciones|Count|Total||ShardId|
|cachehits|Aciertos de caché|Count|Total||ShardId|
|cachemisses|Errores de caché|Count|Total||ShardId|
|cachemissrate|Tasa de errores de caché|Percent|cachemissrate||ShardId|
|getcommands|Gets|Count|Total||ShardId|
|setcommands|Conjuntos|Count|Total||ShardId|
|operationsPerSecond|Operaciones por segundo|Count|Máxima||ShardId|
|evictedkeys|Claves expulsadas|Count|Total||ShardId|
|totalkeys|Total de claves|Count|Máxima||ShardId|
|expiredkeys|Claves expiradas|Count|Total||ShardId|
|usedmemory|Memoria usada|Bytes|Máxima||ShardId|
|usedmemorypercentage|Porcentaje de memoria usado|Percent|Máxima||ShardId|
|usedmemoryRss|Memoria RSS usada|Bytes|Máxima||ShardId|
|serverLoad|Carga de servidor|Percent|Máxima||ShardId|
|cacheWrite|Escritura de caché|BytesPerSecond|Máxima||ShardId|
|cacheRead|Lectura de caché|BytesPerSecond|Máxima||ShardId|
|percentProcessorTime|CPU|Percent|Máxima||ShardId|
|cacheLatency|Microsegundos de latencia de la memoria caché (versión preliminar)|Count|Average||ShardId|
|errors|Errors|Count|Máxima||ShardId,ErrorType|
|connectedclients0|Clientes conectados (partición 0)|Count|Máxima||None|
|totalcommandsprocessed0|Total de operaciones (partición 0)|Count|Total||None|
|cachehits0|Aciertos de caché (partición 0)|Count|Total||None|
|cachemisses0|Errores de caché (partición 0)|Count|Total||None|
|getcommands0|Obtenciones (partición 0)|Count|Total||None|
|setcommands0|Definiciones (partición 0)|Count|Total||None|
|operationsPerSecond0|Operaciones por segundo (partición 0)|Count|Máxima||None|
|evictedkeys0|Claves expulsadas (partición 0)|Count|Total||None|
|totalkeys0|Total de claves (partición 0)|Count|Máxima||None|
|expiredkeys0|Claves expiradas (partición 0)|Count|Total||None|
|usedmemory0|Memoria usada (partición 0)|Bytes|Máxima||None|
|usedmemoryRss0|Memoria RSS usada (partición 0)|Bytes|Máxima||None|
|serverLoad0|Carga de servidor (partición 0)|Percent|Máxima||None|
|cacheWrite0|Escritura de caché (partición 0)|BytesPerSecond|Máxima||None|
|cacheRead0|Lectura de caché (partición 0)|BytesPerSecond|Máxima||None|
|percentProcessorTime0|CPU (partición 0)|Percent|Máxima||None|
|connectedclients1|Clientes conectados (partición 1)|Count|Máxima||None|
|totalcommandsprocessed1|Total de operaciones (partición 1)|Count|Total||None|
|cachehits1|Aciertos de caché (partición 1)|Count|Total||None|
|cachemisses1|Errores de caché (partición 1)|Count|Total||None|
|getcommands1|Obtenciones (partición 1)|Count|Total||None|
|setcommands1|Definiciones (partición 1)|Count|Total||None|
|operationsPerSecond1|Operaciones por segundo (partición 1)|Count|Máxima||None|
|evictedkeys1|Claves expulsadas (partición 1)|Count|Total||None|
|totalkeys1|Total de claves (partición 1)|Count|Máxima||None|
|expiredkeys1|Claves expiradas (partición 1)|Count|Total||None|
|usedmemory1|Memoria usada (partición 1)|Bytes|Máxima||None|
|usedmemoryRss1|Memoria RSS usada (partición 1)|Bytes|Máxima||None|
|serverLoad1|Carga de servidor (partición 1)|Percent|Máxima||None|
|cacheWrite1|Escritura de caché (partición 1)|BytesPerSecond|Máxima||None|
|cacheRead1|Lectura de caché (partición 1)|BytesPerSecond|Máxima||None|
|percentProcessorTime1|CPU (partición 1)|Percent|Máxima||None|
|connectedclients2|Clientes conectados (partición 2)|Count|Máxima||None|
|totalcommandsprocessed2|Total de operaciones (partición 2)|Count|Total||None|
|cachehits2|Aciertos de caché (partición 2)|Count|Total||None|
|cachemisses2|Errores de caché (partición 2)|Count|Total||None|
|getcommands2|Obtenciones (partición 2)|Count|Total||None|
|setcommands2|Definiciones (partición 2)|Count|Total||None|
|operationsPerSecond2|Operaciones por segundo (partición 2)|Count|Máxima||None|
|evictedkeys2|Claves expulsadas (partición 2)|Count|Total||None|
|totalkeys2|Total de claves (partición 2)|Count|Máxima||None|
|expiredkeys2|Claves expiradas (partición 2)|Count|Total||None|
|usedmemory2|Memoria usada (partición 2)|Bytes|Máxima||None|
|usedmemoryRss2|Memoria RSS usada (partición 2)|Bytes|Máxima||None|
|serverLoad2|Carga de servidor (partición 2)|Percent|Máxima||None|
|cacheWrite2|Escritura de caché (partición 2)|BytesPerSecond|Máxima||None|
|cacheRead2|Lectura de caché (partición 2)|BytesPerSecond|Máxima||None|
|percentProcessorTime2|CPU (partición 2)|Percent|Máxima||None|
|connectedclients3|Clientes conectados (partición 3)|Count|Máxima||None|
|totalcommandsprocessed3|Total de operaciones (partición 3)|Count|Total||None|
|cachehits3|Aciertos de caché (partición 3)|Count|Total||None|
|cachemisses3|Errores de caché (partición 3)|Count|Total||None|
|getcommands3|Obtenciones (partición 3)|Count|Total||None|
|setcommands3|Definiciones (partición 3)|Count|Total||None|
|operationsPerSecond3|Operaciones por segundo (partición 3)|Count|Máxima||None|
|evictedkeys3|Claves expulsadas (partición 3)|Count|Total||None|
|totalkeys3|Total de claves (partición 3)|Count|Máxima||None|
|expiredkeys3|Claves expiradas (partición 3)|Count|Total||None|
|usedmemory3|Memoria usada (partición 3)|Bytes|Máxima||None|
|usedmemoryRss3|Memoria RSS usada (partición 3)|Bytes|Máxima||None|
|serverLoad3|Carga de servidor (partición 3)|Percent|Máxima||None|
|cacheWrite3|Escritura de caché (partición 3)|BytesPerSecond|Máxima||None|
|cacheRead3|Lectura de caché (partición 3)|BytesPerSecond|Máxima||None|
|percentProcessorTime3|CPU (partición 3)|Percent|Máxima||None|
|connectedclients4|Clientes conectados (partición 4)|Count|Máxima||None|
|totalcommandsprocessed4|Total de operaciones (partición 4)|Count|Total||None|
|cachehits4|Aciertos de caché (partición 4)|Count|Total||None|
|cachemisses4|Errores de caché (partición 4)|Count|Total||None|
|getcommands4|Obtenciones (partición 4)|Count|Total||None|
|setcommands4|Definiciones (partición 4)|Count|Total||None|
|operationsPerSecond4|Operaciones por segundo (partición 4)|Count|Máxima||None|
|evictedkeys4|Claves expulsadas (partición 4)|Count|Total||None|
|totalkeys4|Total de claves (partición 4)|Count|Máxima||None|
|expiredkeys4|Claves expiradas (partición 4)|Count|Total||None|
|usedmemory4|Memoria usada (partición 4)|Bytes|Máxima||None|
|usedmemoryRss4|Memoria RSS usada (partición 4)|Bytes|Máxima||None|
|serverLoad4|Carga de servidor (partición 4)|Percent|Máxima||None|
|cacheWrite4|Escritura de caché (partición 4)|BytesPerSecond|Máxima||None|
|cacheRead4|Lectura de caché (partición 4)|BytesPerSecond|Máxima||None|
|percentProcessorTime4|CPU (partición 4)|Percent|Máxima||None|
|connectedclients5|Clientes conectados (partición 5)|Count|Máxima||None|
|totalcommandsprocessed5|Total de operaciones (partición 5)|Count|Total||None|
|cachehits5|Aciertos de caché (partición 5)|Count|Total||None|
|cachemisses5|Errores de caché (partición 5)|Count|Total||None|
|getcommands5|Obtenciones (partición 5)|Count|Total||None|
|setcommands5|Definiciones (partición 5)|Count|Total||None|
|operationsPerSecond5|Operaciones por segundo (partición 5)|Count|Máxima||None|
|evictedkeys5|Claves expulsadas (partición 5)|Count|Total||None|
|totalkeys5|Total de claves (partición 5)|Count|Máxima||None|
|expiredkeys5|Claves expiradas (partición 5)|Count|Total||None|
|usedmemory5|Memoria usada (partición 5)|Bytes|Máxima||None|
|usedmemoryRss5|Memoria RSS usada (partición 5)|Bytes|Máxima||None|
|serverLoad5|Carga de servidor (partición 5)|Percent|Máxima||None|
|cacheWrite5|Escritura de caché (partición 5)|BytesPerSecond|Máxima||None|
|cacheRead5|Lectura de caché (partición 5)|BytesPerSecond|Máxima||None|
|percentProcessorTime5|CPU (partición 5)|Percent|Máxima||None|
|connectedclients6|Clientes conectados (partición 6)|Count|Máxima||None|
|totalcommandsprocessed6|Total de operaciones (partición 6)|Count|Total||None|
|cachehits6|Aciertos de caché (partición 6)|Count|Total||None|
|cachemisses6|Errores de caché (partición 6)|Count|Total||None|
|getcommands6|Obtenciones (partición 6)|Count|Total||None|
|setcommands6|Definiciones (partición 6)|Count|Total||None|
|operationsPerSecond6|Operaciones por segundo (partición 6)|Count|Máxima||None|
|evictedkeys6|Claves expulsadas (partición 6)|Count|Total||None|
|totalkeys6|Total de claves (partición 6)|Count|Máxima||None|
|expiredkeys6|Claves expiradas (partición 6)|Count|Total||None|
|usedmemory6|Memoria usada (partición 6)|Bytes|Máxima||None|
|usedmemoryRss6|Memoria RSS usada (partición 6)|Bytes|Máxima||None|
|serverLoad6|Carga de servidor (partición 6)|Percent|Máxima||None|
|cacheWrite6|Escritura de caché (partición 6)|BytesPerSecond|Máxima||None|
|cacheRead6|Lectura de caché (partición 6)|BytesPerSecond|Máxima||None|
|percentProcessorTime6|CPU (partición 6)|Percent|Máxima||None|
|connectedclients7|Clientes conectados (partición 7)|Count|Máxima||None|
|totalcommandsprocessed7|Total de operaciones (partición 7)|Count|Total||None|
|cachehits7|Aciertos de caché (partición 7)|Count|Total||None|
|cachemisses7|Errores de caché (partición 7)|Count|Total||None|
|getcommands7|Obtenciones (partición 7)|Count|Total||None|
|setcommands7|Definiciones (partición 7)|Count|Total||None|
|operationsPerSecond7|Operaciones por segundo (partición 7)|Count|Máxima||None|
|evictedkeys7|Claves expulsadas (partición 7)|Count|Total||None|
|totalkeys7|Total de claves (partición 7)|Count|Máxima||None|
|expiredkeys7|Claves expiradas (partición 7)|Count|Total||None|
|usedmemory7|Memoria usada (partición 7)|Bytes|Máxima||None|
|usedmemoryRss7|Memoria RSS usada (partición 7)|Bytes|Máxima||None|
|serverLoad7|Carga de servidor (partición 7)|Percent|Máxima||None|
|cacheWrite7|Escritura de caché (partición 7)|BytesPerSecond|Máxima||None|
|cacheRead7|Lectura de caché (partición 7)|BytesPerSecond|Máxima||None|
|percentProcessorTime7|CPU (partición 7)|Percent|Máxima||None|
|connectedclients8|Clientes conectados (partición 8)|Count|Máxima||None|
|totalcommandsprocessed8|Total de operaciones (partición 8)|Count|Total||None|
|cachehits8|Aciertos de caché (partición 8)|Count|Total||None|
|cachemisses8|Errores de caché (partición 8)|Count|Total||None|
|getcommands8|Obtenciones (partición 8)|Count|Total||None|
|setcommands8|Definiciones (partición 8)|Count|Total||None|
|operationsPerSecond8|Operaciones por segundo (partición 8)|Count|Máxima||None|
|evictedkeys8|Claves expulsadas (partición 8)|Count|Total||None|
|totalkeys8|Total de claves (partición 8)|Count|Máxima||None|
|expiredkeys8|Claves expiradas (partición 8)|Count|Total||None|
|usedmemory8|Memoria usada (partición 8)|Bytes|Máxima||None|
|usedmemoryRss8|Memoria RSS usada (partición 8)|Bytes|Máxima||None|
|serverLoad8|Carga de servidor (partición 8)|Percent|Máxima||None|
|cacheWrite8|Escritura de caché (partición 8)|BytesPerSecond|Máxima||None|
|cacheRead8|Lectura de caché (partición 8)|BytesPerSecond|Máxima||None|
|percentProcessorTime8|CPU (partición 8)|Percent|Máxima||None|
|connectedclients9|Clientes conectados (partición 9)|Count|Máxima||None|
|totalcommandsprocessed9|Total de operaciones (partición 9)|Count|Total||None|
|cachehits9|Aciertos de caché (partición 9)|Count|Total||None|
|cachemisses9|Errores de caché (partición 9)|Count|Total||None|
|getcommands9|Obtenciones (partición 9)|Count|Total||None|
|setcommands9|Definiciones (partición 9)|Count|Total||None|
|operationsPerSecond9|Operaciones por segundo (partición 9)|Count|Máxima||None|
|evictedkeys9|Claves expulsadas (partición 9)|Count|Total||None|
|totalkeys9|Total de claves (partición 9)|Count|Máxima||None|
|expiredkeys9|Claves expiradas (partición 9)|Count|Total||None|
|usedmemory9|Memoria usada (partición 9)|Bytes|Máxima||None|
|usedmemoryRss9|Memoria RSS usada (partición 9)|Bytes|Máxima||None|
|serverLoad9|Carga de servidor (partición 9)|Percent|Máxima||None|
|cacheWrite9|Escritura de caché (partición 9)|BytesPerSecond|Máxima||None|
|cacheRead9|Lectura de caché (partición 9)|BytesPerSecond|Máxima||None|
|percentProcessorTime9|CPU (partición 9)|Percent|Máxima||None|




## <a name="microsoftcdncdnwebapplicationfirewallpolicies"></a>Microsoft.Cdn/cdnwebapplicationfirewallpolicies

|Métrica|Nombre de métrica para mostrar|Unidad|Tipo de agregación|Descripción|Dimensions|
|---|---|---|---|---|---|
|WebApplicationFirewallRequestCount|Recuento de solicitudes del firewall de aplicaciones web|Count|Total|Número de solicitudes de cliente procesadas por el firewall de aplicaciones web|PolicyName,RuleName,Action|


## <a name="microsoftclassiccomputevirtualmachines"></a>Microsoft.ClassicCompute/virtualMachines

|Métrica|Nombre de métrica para mostrar|Unidad|Tipo de agregación|Descripción|Dimensions|
|---|---|---|---|---|---|
|Porcentaje de CPU|Porcentaje de CPU|Percent|Average|El porcentaje de unidades de proceso asignadas que las máquinas virtuales usan actualmente.|None|
|Red interna|Red interna|Bytes|Total|El número de bytes recibidos en todas las interfaces de red por las máquinas virtuales (tráfico entrante).|None|
|Red interna|Red interna|Bytes|Total|El número de bytes enviados en todas las interfaces de red por las máquinas virtuales (tráfico saliente).|None|
|Disk Read Bytes/Sec|Lectura de disco|BytesPerSecond|Average|Promedio de bytes que se leen desde el disco durante el período de supervisión.|None|
|Disk Write Bytes/Sec|Escritura de disco|BytesPerSecond|Average|Promedio de bytes que se escriben en el disco durante el período de supervisión.|None|
|Operaciones de lectura de disco por segundo|Operaciones de lectura de disco por segundo|CountPerSecond|Average|E/S por segundo de lectura de disco.|None|
|Operaciones de escritura por segundo en disco|Operaciones de escritura por segundo en disco|CountPerSecond|Average|E/S por segundo de escritura en disco.|None|


## <a name="microsoftclassiccomputedomainnamesslotsroles"></a>Microsoft.ClassicCompute/domainNames/slots/roles

|Métrica|Nombre de métrica para mostrar|Unidad|Tipo de agregación|Descripción|Dimensions|
|---|---|---|---|---|---|
|Porcentaje de CPU|Porcentaje de CPU|Percent|Average|El porcentaje de unidades de proceso asignadas que las máquinas virtuales usan actualmente.|RoleInstanceId|
|Red interna|Red interna|Bytes|Total|El número de bytes recibidos en todas las interfaces de red por las máquinas virtuales (tráfico entrante).|RoleInstanceId|
|Red interna|Red interna|Bytes|Total|El número de bytes enviados en todas las interfaces de red por las máquinas virtuales (tráfico saliente).|RoleInstanceId|
|Disk Read Bytes/Sec|Lectura de disco|BytesPerSecond|Average|Promedio de bytes que se leen desde el disco durante el período de supervisión.|RoleInstanceId|
|Disk Write Bytes/Sec|Escritura de disco|BytesPerSecond|Average|Promedio de bytes que se escriben en el disco durante el período de supervisión.|RoleInstanceId|
|Operaciones de lectura de disco por segundo|Operaciones de lectura de disco por segundo|CountPerSecond|Average|E/S por segundo de lectura de disco.|RoleInstanceId|
|Operaciones de escritura por segundo en disco|Operaciones de escritura por segundo en disco|CountPerSecond|Average|E/S por segundo de escritura en disco.|RoleInstanceId|



## <a name="microsoftclassicstoragestorageaccounts"></a>Microsoft.ClassicStorage/storageAccounts

|Métrica|Nombre de métrica para mostrar|Unidad|Tipo de agregación|Descripción|Dimensions|
|---|---|---|---|---|---|
|UsedCapacity|Capacidad usada|Bytes|Average|Capacidad usada de la cuenta|None|
|Transacciones|Transacciones|Count|Total|El número de solicitudes realizadas a un servicio de almacenamiento o la operación de API especificada. Este número incluye solicitudes correctas y con errores, así como las solicitudes que generaron errores. Use la dimensión ResponseType para el número de diferentes tipos de respuesta.|ResponseType,GeoType,ApiName,Authentication|
|Entrada|Entrada|Bytes|Total|La cantidad de datos de entrada, en bytes. Este número incluye la entradas desde un cliente externo en Azure Storage, así como la entrada dentro de Azure.|GeoType,ApiName,Authentication|
|Salida|Salida|Bytes|Total|La cantidad de datos de salida, en bytes. Este número incluye la salida de un cliente externo en Azure Storage, así como la salida dentro de Azure. En consecuencia, este número no refleja las salidas facturables.|GeoType,ApiName,Authentication|
|SuccessServerLatency|Latencia del servidor correcta|Milisegundos|Average|Latencia utilizada por Azure Storage para procesar una solicitud correcta, en milisegundos. Este valor no incluye la latencia de red especificada en SuccessE2ELatency.|GeoType,ApiName,Authentication|
|SuccessE2ELatency|Latencia de E2E correcta|Milisegundos|Average|Latencia media de un extremo a otro de las solicitudes correctas realizadas a un servicio de almacenamiento o a la operación de API especificada, en milisegundos. Este valor incluye el tiempo de procesamiento requerido dentro de Azure Storage para leer la solicitud, enviar la respuesta y recibir la confirmación de la respuesta.|GeoType,ApiName,Authentication|
|Disponibilidad|Disponibilidad|Percent|Average|El porcentaje de disponibilidad para el servicio de almacenamiento o la operación de API especificada. Para calcular la disponibilidad hay que tomar el valor deTotalBillableRequests y dividirlo por el número de solicitudes aplicables, incluidas aquellas que generaron errores inesperados. Todos los errores inesperados reducen la disponibilidad del servicio de almacenamiento o de la operación de API especificada.|GeoType,ApiName,Authentication|

## <a name="microsoftclassicstoragestorageaccountsblobservices"></a>Microsoft.ClassicStorage/storageAccounts/blobServices

|Métrica|Nombre de métrica para mostrar|Unidad|Tipo de agregación|Descripción|Dimensions|
|---|---|---|---|---|---|
|BlobCapacity|Capacidad de Blob|Bytes|Average|Cantidad de almacenamiento que ha usado la instancia de Blob service de la cuenta de almacenamiento, en bytes.|BlobType,Tier|
|BlobCount|Recuento de blobs|Count|Average|Número de blobs en la instancia de Blob service de la cuenta de almacenamiento.|BlobType,Tier|
|ContainerCount|Recuento de contenedores de blobs|Count|Average|Número de contenedores en la instancia de Blob service de la cuenta de almacenamiento.|None|
|IndexCapacity|Capacidad de índice|Bytes|Average|Cantidad de almacenamiento que usa el índice (jerárquico) de ADLS Gen2 en bytes.|None|
|Transacciones|Transacciones|Count|Total|El número de solicitudes realizadas a un servicio de almacenamiento o la operación de API especificada. Este número incluye solicitudes correctas y con errores, así como las solicitudes que generaron errores. Use la dimensión ResponseType para el número de diferentes tipos de respuesta.|ResponseType,GeoType,ApiName,Authentication|
|Entrada|Entrada|Bytes|Total|La cantidad de datos de entrada, en bytes. Este número incluye la entradas desde un cliente externo en Azure Storage, así como la entrada dentro de Azure.|GeoType,ApiName,Authentication|
|Salida|Salida|Bytes|Total|La cantidad de datos de salida, en bytes. Este número incluye la salida de un cliente externo en Azure Storage, así como la salida dentro de Azure. En consecuencia, este número no refleja las salidas facturables.|GeoType,ApiName,Authentication|
|SuccessServerLatency|Latencia del servidor correcta|Milisegundos|Average|Latencia utilizada por Azure Storage para procesar una solicitud correcta, en milisegundos. Este valor no incluye la latencia de red especificada en SuccessE2ELatency.|GeoType,ApiName,Authentication|
|SuccessE2ELatency|Latencia de E2E correcta|Milisegundos|Average|Latencia media de un extremo a otro de las solicitudes correctas realizadas a un servicio de almacenamiento o a la operación de API especificada, en milisegundos. Este valor incluye el tiempo de procesamiento requerido dentro de Azure Storage para leer la solicitud, enviar la respuesta y recibir la confirmación de la respuesta.|GeoType,ApiName,Authentication|
|Disponibilidad|Disponibilidad|Percent|Average|El porcentaje de disponibilidad para el servicio de almacenamiento o la operación de API especificada. Para calcular la disponibilidad hay que tomar el valor deTotalBillableRequests y dividirlo por el número de solicitudes aplicables, incluidas aquellas que generaron errores inesperados. Todos los errores inesperados reducen la disponibilidad del servicio de almacenamiento o de la operación de API especificada.|GeoType,ApiName,Authentication|

## <a name="microsoftclassicstoragestorageaccountstableservices"></a>Microsoft.ClassicStorage/storageAccounts/tableServices

|Métrica|Nombre de métrica para mostrar|Unidad|Tipo de agregación|Descripción|Dimensions|
|---|---|---|---|---|---|
|TableCapacity|Capacidad de Table|Bytes|Average|Cantidad de almacenamiento que ha utilizado la instancia de Table service de la cuenta de almacenamiento, en bytes.|None|
|TableCount|Recuento de tablas|Count|Average|Número de tablas en la instancia de Table service de la cuenta de almacenamiento.|None|
|TableEntityCount|Recuento de entidades de Table|Count|Average|Número de entidades de tabla en la instancia de Table service de la cuenta de almacenamiento.|None|
|Transacciones|Transacciones|Count|Total|El número de solicitudes realizadas a un servicio de almacenamiento o la operación de API especificada. Este número incluye solicitudes correctas y con errores, así como las solicitudes que generaron errores. Use la dimensión ResponseType para el número de diferentes tipos de respuesta.|ResponseType,GeoType,ApiName,Authentication|
|Entrada|Entrada|Bytes|Total|La cantidad de datos de entrada, en bytes. Este número incluye la entradas desde un cliente externo en Azure Storage, así como la entrada dentro de Azure.|GeoType,ApiName,Authentication|
|Salida|Salida|Bytes|Total|La cantidad de datos de salida, en bytes. Este número incluye la salida de un cliente externo en Azure Storage, así como la salida dentro de Azure. En consecuencia, este número no refleja las salidas facturables.|GeoType,ApiName,Authentication|
|SuccessServerLatency|Latencia del servidor correcta|Milisegundos|Average|Latencia utilizada por Azure Storage para procesar una solicitud correcta, en milisegundos. Este valor no incluye la latencia de red especificada en SuccessE2ELatency.|GeoType,ApiName,Authentication|
|SuccessE2ELatency|Latencia de E2E correcta|Milisegundos|Average|Latencia media de un extremo a otro de las solicitudes correctas realizadas a un servicio de almacenamiento o a la operación de API especificada, en milisegundos. Este valor incluye el tiempo de procesamiento requerido dentro de Azure Storage para leer la solicitud, enviar la respuesta y recibir la confirmación de la respuesta.|GeoType,ApiName,Authentication|
|Disponibilidad|Disponibilidad|Percent|Average|El porcentaje de disponibilidad para el servicio de almacenamiento o la operación de API especificada. Para calcular la disponibilidad hay que tomar el valor deTotalBillableRequests y dividirlo por el número de solicitudes aplicables, incluidas aquellas que generaron errores inesperados. Todos los errores inesperados reducen la disponibilidad del servicio de almacenamiento o de la operación de API especificada.|GeoType,ApiName,Authentication|

## <a name="microsoftclassicstoragestorageaccountsfileservices"></a>Microsoft.ClassicStorage/storageAccounts/fileServices

|Métrica|Nombre de métrica para mostrar|Unidad|Tipo de agregación|Descripción|Dimensions|
|---|---|---|---|---|---|
|FileCapacity|Capacidad de File|Bytes|Average|Cantidad de almacenamiento que ha utilizado el servicio File de la cuenta de almacenamiento, en bytes.|FileShare|
|FileCount|Recuento de archivos|Count|Average|Número de archivos en el servicio File de la cuenta de almacenamiento.|FileShare|
|FileShareCount|Recuento de recursos compartidos de archivos|Count|Average|Número de recursos compartidos de archivos en el servicio File de la cuenta de almacenamiento.|None|
|FileShareSnapshotCount|File Share Snapshot Count (Recuento de instantáneas del recurso compartido de archivos)|Count|Average|Número de instantáneas presentes en el recurso compartido en el servicio Files de la cuenta de almacenamiento.|FileShare|
|FileShareSnapshotSize|File Share Snapshot Size (Tamaño de instantánea del recurso compartido de archivos)|Bytes|Average|Cantidad de almacenamiento que usan las instantáneas del servicio Files de la cuenta de almacenamiento en bytes.|FileShare|
|FileShareCapacityQuota|File share quota size (Tamaño de cuota del recurso compartido de archivos)|Bytes|Average|Límite superior de la cantidad de almacenamiento que puede usar el servicio Azure Files en bytes.|FileShare|
|Transacciones|Transacciones|Count|Total|El número de solicitudes realizadas a un servicio de almacenamiento o la operación de API especificada. Este número incluye solicitudes correctas y con errores, así como las solicitudes que generaron errores. Use la dimensión ResponseType para el número de diferentes tipos de respuesta.|ResponseType,GeoType,ApiName,Authentication,FileShare|
|Entrada|Entrada|Bytes|Total|La cantidad de datos de entrada, en bytes. Este número incluye la entradas desde un cliente externo en Azure Storage, así como la entrada dentro de Azure.|GeoType,ApiName,Authentication,FileShare|
|Salida|Salida|Bytes|Total|La cantidad de datos de salida, en bytes. Este número incluye la salida de un cliente externo en Azure Storage, así como la salida dentro de Azure. En consecuencia, este número no refleja las salidas facturables.|GeoType,ApiName,Authentication,FileShare|
|SuccessServerLatency|Latencia del servidor correcta|Milisegundos|Average|Latencia utilizada por Azure Storage para procesar una solicitud correcta, en milisegundos. Este valor no incluye la latencia de red especificada en SuccessE2ELatency.|GeoType,ApiName,Authentication,FileShare|
|SuccessE2ELatency|Latencia de E2E correcta|Milisegundos|Average|Latencia media de un extremo a otro de las solicitudes correctas realizadas a un servicio de almacenamiento o a la operación de API especificada, en milisegundos. Este valor incluye el tiempo de procesamiento requerido dentro de Azure Storage para leer la solicitud, enviar la respuesta y recibir la confirmación de la respuesta.|GeoType,ApiName,Authentication,FileShare|
|Disponibilidad|Disponibilidad|Percent|Average|El porcentaje de disponibilidad para el servicio de almacenamiento o la operación de API especificada. Para calcular la disponibilidad hay que tomar el valor deTotalBillableRequests y dividirlo por el número de solicitudes aplicables, incluidas aquellas que generaron errores inesperados. Todos los errores inesperados reducen la disponibilidad del servicio de almacenamiento o de la operación de API especificada.|GeoType,ApiName,Authentication,FileShare|

## <a name="microsoftclassicstoragestorageaccountsqueueservices"></a>Microsoft.ClassicStorage/storageAccounts/queueServices

|Métrica|Nombre de métrica para mostrar|Unidad|Tipo de agregación|Descripción|Dimensions|
|---|---|---|---|---|---|
|QueueCapacity|Capacidad de Queue|Bytes|Average|Cantidad de almacenamiento que ha utilizado la instancia de Queue service de la cuenta de almacenamiento, en bytes.|None|
|QueueCount|Recuento de colas|Count|Average|Número de colas en la instancia de Queue service de la cuenta de almacenamiento.|None|
|QueueMessageCount|Recuento de mensajes de Queue|Count|Average|Número aproximado de mensajes de cola en la instancia de Queue service de la cuenta de almacenamiento.|None|
|Transacciones|Transacciones|Count|Total|El número de solicitudes realizadas a un servicio de almacenamiento o la operación de API especificada. Este número incluye solicitudes correctas y con errores, así como las solicitudes que generaron errores. Use la dimensión ResponseType para el número de diferentes tipos de respuesta.|ResponseType,GeoType,ApiName,Authentication|
|Entrada|Entrada|Bytes|Total|La cantidad de datos de entrada, en bytes. Este número incluye la entradas desde un cliente externo en Azure Storage, así como la entrada dentro de Azure.|GeoType,ApiName,Authentication|
|Salida|Salida|Bytes|Total|La cantidad de datos de salida, en bytes. Este número incluye la salida de un cliente externo en Azure Storage, así como la salida dentro de Azure. En consecuencia, este número no refleja las salidas facturables.|GeoType,ApiName,Authentication|
|SuccessServerLatency|Latencia del servidor correcta|Milisegundos|Average|Latencia utilizada por Azure Storage para procesar una solicitud correcta, en milisegundos. Este valor no incluye la latencia de red especificada en SuccessE2ELatency.|GeoType,ApiName,Authentication|
|SuccessE2ELatency|Latencia de E2E correcta|Milisegundos|Average|Latencia media de un extremo a otro de las solicitudes correctas realizadas a un servicio de almacenamiento o a la operación de API especificada, en milisegundos. Este valor incluye el tiempo de procesamiento requerido dentro de Azure Storage para leer la solicitud, enviar la respuesta y recibir la confirmación de la respuesta.|GeoType,ApiName,Authentication|
|Disponibilidad|Disponibilidad|Percent|Average|El porcentaje de disponibilidad para el servicio de almacenamiento o la operación de API especificada. Para calcular la disponibilidad hay que tomar el valor deTotalBillableRequests y dividirlo por el número de solicitudes aplicables, incluidas aquellas que generaron errores inesperados. Todos los errores inesperados reducen la disponibilidad del servicio de almacenamiento o de la operación de API especificada.|GeoType,ApiName,Authentication|


## <a name="microsoftcognitiveservicesaccounts"></a>Microsoft.CognitiveServices/accounts

|Métrica|Nombre de métrica para mostrar|Unidad|Tipo de agregación|Descripción|Dimensions|
|---|---|---|---|---|---|
|TotalCalls|Total de llamadas|Count|Total|Número total de llamadas.|ApiName,OperationName,Region|
|SuccessfulCalls|Llamadas correctas|Count|Total|Número de llamadas correctas.|ApiName,OperationName,Region|
|TotalErrors|Total de errores|Count|Total|Número total de llamadas con respuesta de error (código de respuesta HTTP 4xx o 5xx).|ApiName,OperationName,Region|
|BlockedCalls|Llamadas bloqueadas|Count|Total|Número de llamadas que han superado la tasa o el límite de cuota.|ApiName,OperationName,Region|
|ServerErrors|Errores del servidor|Count|Total|Número de llamadas con error interno del servicio (código de respuesta HTTP 5xx).|ApiName,OperationName,Region|
|ClientErrors|Errores de cliente|Count|Total|Número de llamadas con error interno del lado cliente (código de respuesta HTTP 4xx).|ApiName,OperationName,Region|
|DataIn|Entrada de datos|Bytes|Total|Tamaño de los datos de entrada en bytes.|ApiName,OperationName,Region|
|DataOut|Salida de datos|Bytes|Total|Tamaño de los datos de salida en bytes.|ApiName,OperationName,Region|
|Latencia|Latencia|MilliSeconds|Average|Latencia en milisegundos.|ApiName,OperationName,Region|
|TotalTokenCalls|Llamadas de token totales|Count|Total|Número total de llamadas de token.|ApiName,OperationName,Region|
|CharactersTranslated|Caracteres traducidos|Count|Total|Número total de caracteres de la solicitud entrante de texto.|ApiName,OperationName,Region|
|CharactersTrained|Caracteres entrenados|Count|Total|Número total de caracteres entrenados.|ApiName,OperationName,Region|
|SpeechSessionDuration|Duración de la sesión de voz|Segundos|Total|Duración total de la sesión de voz en segundos.|ApiName,OperationName,Region|
|TotalTransactions|Transacciones totales|Count|Total|Número total de transacciones.|None|
|ProcessedImages|Imágenes procesadas|Count|Total| Número de transacciones para el procesamiento de imágenes.|ApiName,FeatureName,Channel,Region|

## <a name="microsoftcomputevirtualmachines"></a>Microsoft.Compute/virtualMachines

|Métrica|Nombre de métrica para mostrar|Unidad|Tipo de agregación|Descripción|Dimensions|
|---|---|---|---|---|---|
|Porcentaje de CPU|Porcentaje de CPU|Percent|Average|El porcentaje de unidades de proceso asignadas que las máquinas virtuales usan actualmente|None|
|Red interna|Network In Billable (Deprecated) [Entrada de red facturable (en desuso)]|Bytes|Total|Número de bytes facturables que las máquinas virtuales recibieron en todas las interfaces de red (tráfico entrante) (en desuso).|None|
|Red interna|Network Out Billable (Deprecated) [Salida de red facturable (en desuso)]|Bytes|Total|Número de bytes facturables que las máquinas virtuales enviaron en todas las interfaces de red (tráfico saliente) (en desuso)|None|
|Bytes de lectura de disco|Bytes de lectura de disco|Bytes|Total|Bytes que se leen desde el disco durante el período de supervisión|None|
|Bytes de escritura de disco|Bytes de escritura de disco|Bytes|Total|Bytes que se escriben en el disco durante el período de supervisión|None|
|Operaciones de lectura de disco por segundo|Operaciones de lectura de disco por segundo|CountPerSecond|Average|E/S por segundo de lectura de disco|None|
|Operaciones de escritura por segundo en disco|Operaciones de escritura por segundo en disco|CountPerSecond|Average|E/S por segundo de escritura en disco|None|
|Créditos de CPU restantes|Créditos de CPU restantes|Count|Average|Número total de créditos disponibles para irrumpir|None|
|Créditos de CPU consumidos|Créditos de CPU consumidos|Count|Average|Número total de créditos consumidos por la máquina virtual|None|
|Bytes de lectura de discos por segundo|Bytes de lectura de discos de datos por segundo [(en desuso)](portal-disk-metrics-deprecation.md)|CountPerSecond|Average|Bytes por segundo de lectura de un único disco durante el período de supervisión|SlotId|
|Bytes de escritura de discos por segundo|Bytes de escritura de discos de datos por segundo [(en desuso)](portal-disk-metrics-deprecation.md)|CountPerSecond|Average|Bytes por segundo escritos en un único disco durante el período de supervisión|SlotId|
|Operaciones de lectura de discos por segundo|Operaciones de lectura de discos de datos por segundo [(en desuso)](portal-disk-metrics-deprecation.md)|CountPerSecond|Average|IOPS de lectura de un único disco durante el período de supervisión|SlotId|
|Operaciones de escritura de discos por segundo|Operaciones de escritura de discos de datos por segundo [(en desuso)](portal-disk-metrics-deprecation.md)|CountPerSecond|Average|IOPS de escritura de un único disco durante el período de supervisión|SlotId|
|Por disco QD|Profundidad de la cola por disco de datos [(en desuso)](portal-disk-metrics-deprecation.md)(portal-disk-metrics-deprecation.md)|Count|Average|Profundidad de la cola del disco de datos (o longitud de la cola)|SlotId|
|Sistema operativo por bytes de lectura de discos por segundo|Bytes de lectura de discos de SO por segundo [(en desuso)](portal-disk-metrics-deprecation.md)|CountPerSecond|Average|Bytes por segundo de lectura de un único disco durante el período de supervisión del disco del sistema operativo|None|
|Sistema operativo por bytes de escritura de disco por segundo|Bytes de escritura de discos de SO por segundo [(en desuso)](portal-disk-metrics-deprecation.md)|CountPerSecond|Average|Bytes por segundo escritos en un único disco durante el período de supervisión del disco del sistema operativo|None|
|Sistema operativo por operaciones de lectura de discos por segundo|Operaciones de lectura de discos de SO por segundo [(en desuso)](portal-disk-metrics-deprecation.md)|CountPerSecond|Average|IOPS de lectura de un único disco durante el período de supervisión para el disco de SO|None|
|Sistema operativo operaciones de escritura de discos por segundo|Operaciones de escritura de discos de SO por segundo [(en desuso)](portal-disk-metrics-deprecation.md)|CountPerSecond|Average|IOPS de escritura de un único disco durante el período de supervisión para el disco de SO|None|
|Sistema operativo por disco QD|Profundidad de la cola de disco de SO [(en desuso)](portal-disk-metrics-deprecation.md)|Count|Average|Profundidad de la cola de discos del sistema operativo (o longitud de la cola)|None|
|Bytes de lectura de discos de datos por segundo|Bytes de lectura de discos de datos por segundo (versión preliminar)|CountPerSecond|Average|Bytes por segundo de lectura de un único disco durante el período de supervisión|LUN|
|Bytes de escritura de discos de datos por segundo|Bytes de escritura de discos de datos por segundo (versión preliminar)|CountPerSecond|Average|Bytes por segundo escritos en un único disco durante el período de supervisión|LUN|
|Operaciones de lectura de discos de datos por segundo|Operaciones de lectura de discos de datos por segundo (versión preliminar)|CountPerSecond|Average|IOPS de lectura de un único disco durante el período de supervisión|LUN|
|Operaciones de escritura de discos de datos por segundo|Operaciones de lectura de discos de datos por segundo (versión preliminar)|CountPerSecond|Average|IOPS de escritura de un único disco durante el período de supervisión|LUN|
|Profundidad de cola de discos de datos|Profundidad de cola de discos de datos (versión preliminar)|Count|Average|Profundidad de la cola del disco de datos (o longitud de la cola)|LUN|
|Bytes de lectura de discos del sistema operativo por segundo|Bytes de lectura de discos del sistema operativo por segundo (versión preliminar)|CountPerSecond|Average|Bytes por segundo de lectura de un único disco durante el período de supervisión del disco del sistema operativo|None|
|Bytes de escritura en discos del SO por segundo|Bytes de escritura de discos del sistema operativo por segundo (versión preliminar)|CountPerSecond|Average|Bytes por segundo escritos en un único disco durante el período de supervisión del disco del sistema operativo|None|
|Operaciones de lectura de discos de SO por segundo|Operaciones de lectura de discos del sistema operativo por segundo (versión preliminar)|CountPerSecond|Average|IOPS de lectura de un único disco durante el período de supervisión para el disco de SO|None|
|Operaciones de escritura en discos de SO por segundo|Operaciones de escritura de discos del sistema operativo por segundo (versión preliminar)|CountPerSecond|Average|IOPS de escritura de un único disco durante el período de supervisión para el disco de SO|None|
|Profundidad de cola de discos de SO|Profundidad de cola de discos de SO (versión preliminar)|Count|Average|Profundidad de la cola de discos del sistema operativo (o longitud de la cola)|None|
|Flujos de entrada|Flujos de entrada|Count|Average|Los flujos de entrada son el número de flujos actuales en la dirección entrante (el tráfico que va a la máquina virtual)|None|
|Flujos de salida|Flujos de salida|Count|Average|Los flujos de salida son el número de flujos actuales en la dirección saliente (el tráfico que sale de la máquina virtual)|None|
|Tasa de creación máxima de flujos de entrada|Tasa de creación máxima de flujos de entrada|CountPerSecond|Average|La tasa de creación máxima de flujos de entrada (el tráfico que va a la máquina virtual)|None|
|Tasa de creación máxima de flujos de salida|Tasa de creación máxima de flujos de salida|CountPerSecond|Average|La tasa de creación máxima de flujos de salida (el tráfico que sale de la máquina virtual)|None|
|Acierto de lectura de la caché de discos de datos premium|Acierto de lectura de la caché de discos de datos premium (versión preliminar)|Percent|Average|Acierto de lectura de la caché de discos de datos premium|LUN|
|Error de lectura de la caché de discos de datos premium|Error de lectura de la caché de discos de datos premium (versión preliminar)|Percent|Average|Error de lectura de la caché de discos de datos premium|LUN|
|Acierto de lectura de la caché de discos de SO premium|Acierto de lectura de la caché de discos de SO premium (versión preliminar)|Percent|Average|Acierto de lectura de la caché de discos de SO premium|None|
|Error de lectura de la caché de discos de SO premium|Error de lectura de la caché de discos de SO premium (versión preliminar)|Percent|Average|Error de lectura de la caché de discos de SO premium|None|
|Red entrante total|Red entrante total|Bytes|Total|El número de bytes recibidos en todas las interfaces de red por las máquinas virtuales (tráfico entrante)|None|
|Red saliente total|Red saliente total|Bytes|Total|El número de bytes enviados en todas las interfaces de red por las máquinas virtuales (tráfico saliente)|None|


## <a name="microsoftcomputevirtualmachinescalesets"></a>Microsoft.Compute/virtualMachineScaleSets

|Métrica|Nombre de métrica para mostrar|Unidad|Tipo de agregación|Descripción|Dimensions|
|---|---|---|---|---|---|
|Porcentaje de CPU|Porcentaje de CPU|Percent|Average|El porcentaje de unidades de proceso asignadas que las máquinas virtuales usan actualmente|VMName|
|Red interna|Network In Billable (Deprecated) [Entrada de red facturable (en desuso)]|Bytes|Total|Número de bytes facturables que las máquinas virtuales recibieron en todas las interfaces de red (tráfico entrante) (en desuso).|VMName|
|Red interna|Network Out Billable (Deprecated) [Salida de red facturable (en desuso)]|Bytes|Total|Número de bytes facturables que las máquinas virtuales enviaron en todas las interfaces de red (tráfico saliente) (en desuso)|VMName|
|Bytes de lectura de disco|Bytes de lectura de disco|Bytes|Total|Bytes que se leen desde el disco durante el período de supervisión|VMName|
|Bytes de escritura de disco|Bytes de escritura de disco|Bytes|Total|Bytes que se escriben en el disco durante el período de supervisión|VMName|
|Operaciones de lectura de disco por segundo|Operaciones de lectura de disco por segundo|CountPerSecond|Average|E/S por segundo de lectura de disco|VMName|
|Operaciones de escritura por segundo en disco|Operaciones de escritura por segundo en disco|CountPerSecond|Average|E/S por segundo de escritura en disco|VMName|
|Créditos de CPU restantes|Créditos de CPU restantes|Count|Average|Número total de créditos disponibles para irrumpir|None|
|Créditos de CPU consumidos|Créditos de CPU consumidos|Count|Average|Número total de créditos consumidos por la máquina virtual|None|
|Bytes de lectura de discos por segundo|Bytes de lectura de discos de datos por segundo [(en desuso)](portal-disk-metrics-deprecation.md)|CountPerSecond|Average|Bytes por segundo de lectura de un único disco durante el período de supervisión|SlotId|
|Bytes de escritura de discos por segundo|Bytes de escritura de discos de datos por segundo [(en desuso)](portal-disk-metrics-deprecation.md)|CountPerSecond|Average|Bytes por segundo escritos en un único disco durante el período de supervisión|SlotId|
|Operaciones de lectura de discos por segundo|Operaciones de lectura de discos de datos por segundo [(en desuso)](portal-disk-metrics-deprecation.md)|CountPerSecond|Average|IOPS de lectura de un único disco durante el período de supervisión|SlotId|
|Operaciones de escritura de discos por segundo|Operaciones de escritura de discos de datos por segundo [(en desuso)](portal-disk-metrics-deprecation.md)|CountPerSecond|Average|IOPS de escritura de un único disco durante el período de supervisión|SlotId|
|Por disco QD|Profundidad de la cola de disco de datos [(en desuso)](portal-disk-metrics-deprecation.md)|Count|Average|Profundidad de la cola del disco de datos (o longitud de la cola)|SlotId|
|Sistema operativo por bytes de lectura de discos por segundo|Bytes de lectura de discos de SO por segundo [(en desuso)](portal-disk-metrics-deprecation.md)|CountPerSecond|Average|Bytes por segundo de lectura de un único disco durante el período de supervisión del disco del sistema operativo|None|
|Sistema operativo por bytes de escritura de disco por segundo|Bytes de escritura de discos de SO por segundo [(en desuso)](portal-disk-metrics-deprecation.md)|CountPerSecond|Average|Bytes por segundo escritos en un único disco durante el período de supervisión del disco del sistema operativo|None|
|Sistema operativo por operaciones de lectura de discos por segundo|Operaciones de lectura de discos de SO por segundo [(en desuso)](portal-disk-metrics-deprecation.md)|CountPerSecond|Average|IOPS de lectura de un único disco durante el período de supervisión para el disco de SO|None|
|Sistema operativo operaciones de escritura de discos por segundo|Operaciones de escritura de discos de SO por segundo [(en desuso)](portal-disk-metrics-deprecation.md)|CountPerSecond|Average|IOPS de escritura de un único disco durante el período de supervisión para el disco de SO|None|
|Sistema operativo por disco QD|Profundidad de la cola de disco de SO [(en desuso)](portal-disk-metrics-deprecation.md)|Count|Average|Profundidad de la cola de discos del sistema operativo (o longitud de la cola)|None|
|Bytes de lectura de discos de datos por segundo|Bytes de lectura de discos de datos por segundo (versión preliminar)|CountPerSecond|Average|Bytes por segundo de lectura de un único disco durante el período de supervisión|LUN,VMName|
|Bytes de escritura de discos de datos por segundo|Bytes de escritura de discos de datos por segundo (versión preliminar)|CountPerSecond|Average|Bytes por segundo escritos en un único disco durante el período de supervisión|LUN,VMName|
|Operaciones de lectura de discos de datos por segundo|Operaciones de lectura de discos de datos por segundo (versión preliminar)|CountPerSecond|Average|IOPS de lectura de un único disco durante el período de supervisión|LUN,VMName|
|Operaciones de escritura de discos de datos por segundo|Operaciones de lectura de discos de datos por segundo (versión preliminar)|CountPerSecond|Average|IOPS de escritura de un único disco durante el período de supervisión|LUN,VMName|
|Profundidad de cola de discos de datos|Profundidad de cola de discos de datos (versión preliminar)|Count|Average|Profundidad de la cola del disco de datos (o longitud de la cola)|LUN,VMName|
|Bytes de lectura de discos del sistema operativo por segundo|Bytes de lectura de discos del sistema operativo por segundo (versión preliminar)|CountPerSecond|Average|Bytes por segundo de lectura de un único disco durante el período de supervisión del disco del sistema operativo|VMName|
|Bytes de escritura en discos del SO por segundo|Bytes de escritura de discos del sistema operativo por segundo (versión preliminar)|CountPerSecond|Average|Bytes por segundo escritos en un único disco durante el período de supervisión del disco del sistema operativo|VMName|
|Operaciones de lectura de discos de SO por segundo|Operaciones de lectura de discos del sistema operativo por segundo (versión preliminar)|CountPerSecond|Average|IOPS de lectura de un único disco durante el período de supervisión para el disco de SO|VMName|
|Operaciones de escritura en discos de SO por segundo|Operaciones de escritura de discos del sistema operativo por segundo (versión preliminar)|CountPerSecond|Average|IOPS de escritura de un único disco durante el período de supervisión para el disco de SO|VMName|
|Profundidad de cola de discos de SO|Profundidad de cola de discos de SO (versión preliminar)|Count|Average|Profundidad de la cola de discos del sistema operativo (o longitud de la cola)|VMName|
|Flujos de entrada|Flujos de entrada|Count|Average|Los flujos de entrada son el número de flujos actuales en la dirección entrante (el tráfico que va a la máquina virtual)|VMName|
|Flujos de salida|Flujos de salida|Count|Average|Los flujos de salida son el número de flujos actuales en la dirección saliente (el tráfico que sale de la máquina virtual)|VMName|
|Tasa de creación máxima de flujos de entrada|Tasa de creación máxima de flujos de entrada|CountPerSecond|Average|La tasa de creación máxima de flujos de entrada (el tráfico que va a la máquina virtual)|VMName|
|Tasa de creación máxima de flujos de salida|Tasa de creación máxima de flujos de salida|CountPerSecond|Average|La tasa de creación máxima de flujos de salida (el tráfico que sale de la máquina virtual)|VMName|
|Acierto de lectura de la caché de discos de datos premium|Acierto de lectura de la caché de discos de datos premium (versión preliminar)|Percent|Average|Acierto de lectura de la caché de discos de datos premium|LUN,VMName|
|Error de lectura de la caché de discos de datos premium|Error de lectura de la caché de discos de datos premium (versión preliminar)|Percent|Average|Error de lectura de la caché de discos de datos premium|LUN,VMName|
|Acierto de lectura de la caché de discos de SO premium|Acierto de lectura de la caché de discos de SO premium (versión preliminar)|Percent|Average|Acierto de lectura de la caché de discos de SO premium|VMName|
|Error de lectura de la caché de discos de SO premium|Error de lectura de la caché de discos de SO premium (versión preliminar)|Percent|Average|Error de lectura de la caché de discos de SO premium|VMName|
|Red entrante total|Red entrante total|Bytes|Total|El número de bytes recibidos en todas las interfaces de red por las máquinas virtuales (tráfico entrante)|VMName|
|Red saliente total|Red saliente total|Bytes|Total|El número de bytes enviados en todas las interfaces de red por las máquinas virtuales (tráfico saliente)|VMName|


## <a name="microsoftcomputevirtualmachinescalesetsvirtualmachines"></a>Microsoft.Compute/virtualMachineScaleSets/virtualMachines

|Métrica|Nombre de métrica para mostrar|Unidad|Tipo de agregación|Descripción|Dimensions|
|---|---|---|---|---|---|
|Porcentaje de CPU|Porcentaje de CPU|Percent|Average|El porcentaje de unidades de proceso asignadas que las máquinas virtuales usan actualmente|None|
|Red interna|Network In Billable (Deprecated) [Entrada de red facturable (en desuso)]|Bytes|Total|Número de bytes facturables que las máquinas virtuales recibieron en todas las interfaces de red (tráfico entrante) (en desuso).|None|
|Red interna|Network Out Billable (Deprecated) [Salida de red facturable (en desuso)]|Bytes|Total|Número de bytes facturables que las máquinas virtuales enviaron en todas las interfaces de red (tráfico saliente) (en desuso)|None|
|Bytes de lectura de disco|Bytes de lectura de disco|Bytes|Total|Bytes que se leen desde el disco durante el período de supervisión|None|
|Bytes de escritura de disco|Bytes de escritura de disco|Bytes|Total|Bytes que se escriben en el disco durante el período de supervisión|None|
|Operaciones de lectura de disco por segundo|Operaciones de lectura de disco por segundo|CountPerSecond|Average|E/S por segundo de lectura de disco|None|
|Operaciones de escritura por segundo en disco|Operaciones de escritura por segundo en disco|CountPerSecond|Average|E/S por segundo de escritura en disco|None|
|Créditos de CPU restantes|Créditos de CPU restantes|Count|Average|Número total de créditos disponibles para irrumpir|None|
|Créditos de CPU consumidos|Créditos de CPU consumidos|Count|Average|Número total de créditos consumidos por la máquina virtual|None|
|Bytes de lectura de discos por segundo|Bytes de lectura de discos de datos por segundo [(en desuso)](portal-disk-metrics-deprecation.md)|CountPerSecond|Average|Bytes por segundo de lectura de un único disco durante el período de supervisión|SlotId|
|Bytes de escritura de discos por segundo|Bytes de escritura de discos de datos por segundo [(en desuso)](portal-disk-metrics-deprecation.md)|CountPerSecond|Average|Bytes por segundo escritos en un único disco durante el período de supervisión|SlotId|
|Operaciones de lectura de discos por segundo|Operaciones de lectura de discos de datos por segundo [(en desuso)](portal-disk-metrics-deprecation.md)|CountPerSecond|Average|IOPS de lectura de un único disco durante el período de supervisión|SlotId|
|Operaciones de escritura de discos por segundo|Operaciones de escritura de discos de datos por segundo [(en desuso)](portal-disk-metrics-deprecation.md)|CountPerSecond|Average|IOPS de escritura de un único disco durante el período de supervisión|SlotId|
|Por disco QD|Profundidad de la cola de disco de datos [(en desuso)](portal-disk-metrics-deprecation.md)|Count|Average|Profundidad de la cola del disco de datos (o longitud de la cola)|SlotId|
|Sistema operativo por bytes de lectura de discos por segundo|Bytes de lectura de discos de SO por segundo [(en desuso)](portal-disk-metrics-deprecation.md)|CountPerSecond|Average|Bytes por segundo de lectura de un único disco durante el período de supervisión del disco del sistema operativo|None|
|Sistema operativo por bytes de escritura de disco por segundo|Bytes de escritura de discos de SO por segundo [(en desuso)](portal-disk-metrics-deprecation.md)|CountPerSecond|Average|Bytes por segundo escritos en un único disco durante el período de supervisión del disco del sistema operativo|None|
|Sistema operativo por operaciones de lectura de discos por segundo|Operaciones de lectura de discos de SO por segundo [(en desuso)](portal-disk-metrics-deprecation.md)|CountPerSecond|Average|IOPS de lectura de un único disco durante el período de supervisión para el disco de SO|None|
|Sistema operativo operaciones de escritura de discos por segundo|Operaciones de escritura de discos de SO por segundo [(en desuso)](portal-disk-metrics-deprecation.md)|CountPerSecond|Average|IOPS de escritura de un único disco durante el período de supervisión para el disco de SO|None|
|Sistema operativo por disco QD|Profundidad de la cola de disco de SO [(en desuso)](portal-disk-metrics-deprecation.md)|Count|Average|Profundidad de la cola de discos del sistema operativo (o longitud de la cola)|None|
|Bytes de lectura de discos de datos por segundo|Bytes de lectura de discos de datos por segundo (versión preliminar)|CountPerSecond|Average|Bytes por segundo de lectura de un único disco durante el período de supervisión|LUN|
|Bytes de escritura de discos de datos por segundo|Bytes de escritura de discos de datos por segundo (versión preliminar)|CountPerSecond|Average|Bytes por segundo escritos en un único disco durante el período de supervisión|LUN|
|Operaciones de lectura de discos de datos por segundo|Operaciones de lectura de discos de datos por segundo (versión preliminar)|CountPerSecond|Average|IOPS de lectura de un único disco durante el período de supervisión|LUN|
|Operaciones de escritura de discos de datos por segundo|Operaciones de lectura de discos de datos por segundo (versión preliminar)|CountPerSecond|Average|IOPS de escritura de un único disco durante el período de supervisión|LUN|
|Profundidad de cola de discos de datos|Profundidad de cola de discos de datos (versión preliminar)|Count|Average|Profundidad de la cola del disco de datos (o longitud de la cola)|LUN|
|Bytes de lectura de discos del sistema operativo por segundo|Bytes de lectura de discos del sistema operativo por segundo (versión preliminar)|CountPerSecond|Average|Bytes por segundo de lectura de un único disco durante el período de supervisión del disco del sistema operativo|None|
|Bytes de escritura en discos del SO por segundo|Bytes de escritura de discos del sistema operativo por segundo (versión preliminar)|CountPerSecond|Average|Bytes por segundo escritos en un único disco durante el período de supervisión del disco del sistema operativo|None|
|Operaciones de lectura de discos de SO por segundo|Operaciones de lectura de discos del sistema operativo por segundo (versión preliminar)|CountPerSecond|Average|IOPS de lectura de un único disco durante el período de supervisión para el disco de SO|None|
|Operaciones de escritura en discos de SO por segundo|Operaciones de escritura de discos del sistema operativo por segundo (versión preliminar)|CountPerSecond|Average|IOPS de escritura de un único disco durante el período de supervisión para el disco de SO|None|
|Profundidad de cola de discos de SO|Profundidad de cola de discos de SO (versión preliminar)|Count|Average|Profundidad de la cola de discos del sistema operativo (o longitud de la cola)|None|
|Flujos de entrada|Flujos de entrada|Count|Average|Los flujos de entrada son el número de flujos actuales en la dirección entrante (el tráfico que va a la máquina virtual)|None|
|Flujos de salida|Flujos de salida|Count|Average|Los flujos de salida son el número de flujos actuales en la dirección saliente (el tráfico que sale de la máquina virtual)|None|
|Tasa de creación máxima de flujos de entrada|Tasa de creación máxima de flujos de entrada|CountPerSecond|Average|La tasa de creación máxima de flujos de entrada (el tráfico que va a la máquina virtual)|None|
|Tasa de creación máxima de flujos de salida|Tasa de creación máxima de flujos de salida|CountPerSecond|Average|La tasa de creación máxima de flujos de salida (el tráfico que sale de la máquina virtual)|None|
|Acierto de lectura de la caché de discos de datos premium|Acierto de lectura de la caché de discos de datos premium (versión preliminar)|Percent|Average|Acierto de lectura de la caché de discos de datos premium|LUN|
|Error de lectura de la caché de discos de datos premium|Error de lectura de la caché de discos de datos premium (versión preliminar)|Percent|Average|Error de lectura de la caché de discos de datos premium|LUN|
|Acierto de lectura de la caché de discos de SO premium|Acierto de lectura de la caché de discos de SO premium (versión preliminar)|Percent|Average|Acierto de lectura de la caché de discos de SO premium|None|
|Error de lectura de la caché de discos de SO premium|Error de lectura de la caché de discos de SO premium (versión preliminar)|Percent|Average|Error de lectura de la caché de discos de SO premium|None|
|Red entrante total|Red entrante total|Bytes|Total|El número de bytes recibidos en todas las interfaces de red por las máquinas virtuales (tráfico entrante)|None|
|Red saliente total|Red saliente total|Bytes|Total|El número de bytes enviados en todas las interfaces de red por las máquinas virtuales (tráfico saliente)|None|

## <a name="microsoftcontainerinstancecontainergroups"></a>Microsoft.ContainerInstance/containerGroups

|Métrica|Nombre de métrica para mostrar|Unidad|Tipo de agregación|Descripción|Dimensions|
|---|---|---|---|---|---|
|CpuUsage|Uso de CPU|Count|Average|Uso de la CPU en todos los núcleos en millares de núcleos.|containerName|
|MemoryUsage|Uso de la memoria|Bytes|Average|Uso de memoria total en bytes.|containerName|
|NetworkBytesReceivedPerSecond|Bytes de red recibidos por segundo|Bytes|Average|Bytes de red recibidos por segundo.|None|
|NetworkBytesTransmittedPerSecond|Bytes de red transmitidos por segundo|Bytes|Average|Bytes de red transmitidos por segundo.|None|

## <a name="microsoftcontainerregistryregistries"></a>Microsoft.ContainerRegistry/registries

|Métrica|Nombre de métrica para mostrar|Unidad|Tipo de agregación|Descripción|Dimensions|
|---|---|---|---|---|---|
|TotalPullCount|Recuento total de extracciones|Count|Average|Número de extracciones de imágenes en total|None|
|SuccessfulPullCount|Recuento de extracciones correctas|Count|Average|Número de extracciones de imágenes correctas|None|
|TotalPushCount|Recuento total de inserciones|Count|Average|Número de inserciones de imágenes en total|None|
|SuccessfulPushCount|Recuento de inserciones correctas|Count|Average|Número de inserciones de imágenes correctas|None|
|RunDuration|Duración de la ejecución|Milisegundos|Total|Duración de la ejecución en milisegundos|None|


## <a name="microsoftcontainerservicemanagedclusters"></a>Microsoft.ContainerService/managedClusters

|Métrica|Nombre de métrica para mostrar|Unidad|Tipo de agregación|Descripción|Dimensions|
|---|---|---|---|---|---|
|kube_node_status_allocatable_cpu_cores|Número total de núcleos de CPU disponibles en un clúster administrado|Count|Average|Número total de núcleos de CPU disponibles en un clúster administrado|None|
|kube_node_status_allocatable_memory_bytes|Cantidad total de memoria disponible en un clúster administrado|Bytes|Average|Cantidad total de memoria disponible en un clúster administrado|None|
|kube_pod_status_ready|Número de pods con estado Listo|Count|Average|Número de pods con estado Listo|namespace,pod|
|kube_node_status_condition|Estados de diversas condiciones de nodo|Count|Average|Estados de diversas condiciones de nodo|condition,status,status2,node|
|kube_pod_status_phase|Número de pods por fase|Count|Average|Número de pods por fase|phase,namespace,pod|



## <a name="microsoftcustomprovidersresourceproviders"></a>Microsoft.CustomProviders/resourceproviders

|Métrica|Nombre de métrica para mostrar|Unidad|Tipo de agregación|Descripción|Dimensions|
|---|---|---|---|---|---|
|SuccessfullRequests|Solicitudes correctas|Count|Total|Solicitudes correctas realizadas por el proveedor personalizado|HttpMethod,CallPath,StatusCode|
|FailedRequests|Solicitudes con error|Count|Total|Obtiene los registros disponibles para proveedores de recursos personalizados|HttpMethod,CallPath,StatusCode|

## <a name="microsoftdataboxedgedataboxedgedevices"></a>Microsoft.DataBoxEdge/dataBoxEdgeDevices

|Métrica|Nombre de métrica para mostrar|Unidad|Tipo de agregación|Descripción|Dimensions|
|---|---|---|---|---|---|
|NICReadThroughput|Rendimiento de lectura (red)|BytesPerSecond|Average|El rendimiento de lectura de la interfaz de red en el dispositivo en el período de informe para todos los volúmenes en la puerta de enlace.|InstanceName|
|NICWriteThroughput|Rendimiento de escritura (red)|BytesPerSecond|Average|El rendimiento de escritura de la interfaz de red en el dispositivo en el período de informe para todos los volúmenes en la puerta de enlace.|InstanceName|
|CloudReadThroughputPerShare|Rendimiento de descarga en la nube (recurso compartido)|BytesPerSecond|Average|El rendimiento de descarga en Azure desde un recurso compartido durante el período de informe.|Compartir|
|CloudUploadThroughputPerShare|Rendimiento de carga en la nube (recurso compartido)|BytesPerSecond|Average|El rendimiento de carga en Azure desde un recurso compartido durante el período de informe.|Compartir|
|BytesUploadedToCloudPerShare|Bytes cargados en la nube (recurso compartido)|Bytes|Average|El número total de bytes que se carga en Azure desde un recurso compartido durante el período de informe.|Compartir|
|TotalCapacity|Capacidad total|Bytes|Average|Capacidad total|None|
|AvailableCapacity|Capacidad disponible|Bytes|Average|La capacidad disponible en bytes durante el período de informe.|None|
|CloudUploadThroughput|Rendimiento de carga en la nube|BytesPerSecond|Average|El rendimiento de carga en la nube en Azure durante el período de informe.|None|
|CloudReadThroughput|Rendimiento de descarga en la nube|BytesPerSecond|Average|El rendimiento de descarga en la nube en Azure durante el período de informe.|None|
|BytesUploadedToCloud|Bytes cargados en la nube (dispositivo)|Bytes|Average|El número total de bytes que se carga en Azure desde un dispositivo durante el período de informe.|None|
|HyperVVirtualProcessorUtilization|Proceso perimetral: porcentaje de CPU|Percent|Average|Porcentaje de uso de CPU|InstanceName|
|HyperVMemoryUtilization|Proceso perimetral: uso de memoria|Percent|Average|Cantidad de RAM en uso|InstanceName|


## <a name="microsoftdatafactorydatafactories"></a>Microsoft.DataFactory/datafactories

|Métrica|Nombre de métrica para mostrar|Unidad|Tipo de agregación|Descripción|Dimensions|
|---|---|---|---|---|---|
|FailedRuns|Ejecuciones con error|Count|Total||pipelineName,activityName|
|SuccessfulRuns|Ejecuciones correctas|Count|Total||pipelineName,activityName|


## <a name="microsoftdatafactoryfactories"></a>Microsoft.DataFactory/factories

|Métrica|Nombre de métrica para mostrar|Unidad|Tipo de agregación|Descripción|Dimensions|
|---|---|---|---|---|---|
|PipelineFailedRuns|Métricas de ejecuciones de canalización erróneas|Count|Total||FailureType,Name|
|PipelineSucceededRuns|Las métricas de ejecuciones de canalización se realizaron correctamente|Count|Total||FailureType,Name|
|PipelineCancelledRuns|Cancelled pipeline runs metrics (Métricas de ejecuciones de canalización canceladas)|Count|Total||FailureType,Name|
|ActivityFailedRuns|Métricas de ejecuciones de actividad erróneas|Count|Total||ActivityType,PipelineName,FailureType,Name|
|ActivitySucceededRuns|Métricas de ejecución de actividad realizadas correctamente|Count|Total||ActivityType,PipelineName,FailureType,Name|
|ActivityCancelledRuns|Cancelled activity runs metrics (Métricas de ejecuciones de actividad canceladas)|Count|Total||ActivityType,PipelineName,FailureType,Name|
|TriggerFailedRuns|Métricas de ejecuciones de desencadenador erróneas|Count|Total||Name,FailureType|
|TriggerSucceededRuns|Métricas de ejecuciones de desencadenador realizadas correctamente|Count|Total||Name,FailureType|
|TriggerCancelledRuns|Cancelled trigger runs metrics (Métricas de ejecuciones de desencadenador canceladas)|Count|Total||Name,FailureType|
|IntegrationRuntimeCpuPercentage|Uso de la CPU de entorno de ejecución de integración|Percent|Average||IntegrationRuntimeName,NodeName|
|IntegrationRuntimeAvailableMemory|Memoria disponible de entorno de ejecución de integración|Bytes|Average||IntegrationRuntimeName,NodeName|
|IntegrationRuntimeAverageTaskPickupDelay|Integration runtime queue duration (Duración de cola de entorno de ejecución de integración)|Segundos|Average||IntegrationRuntimeName|
|IntegrationRuntimeQueueLength|Integration runtime queue length (Longitud de cola de entorno de ejecución de integración)|Count|Average||IntegrationRuntimeName|
|IntegrationRuntimeAvailableNodeNumber|Recuento de nodos disponibles de Integration Runtime|Count|Average||IntegrationRuntimeName|
|MaxAllowedResourceCount|Recuento del máximo de entidades permitidas|Count|Máxima||None|
|MaxAllowedFactorySizeInGbUnits|Tamaño de fábrica máximo permitido (unidad de GB)|Count|Máxima||None|
|ResourceCount|Recuento total de entidades|Count|Máxima||None|
|FactorySizeInGbUnits|Tamaño total de fábrica (unidad de GB)|Count|Máxima||None|

## <a name="microsoftdatalakeanalyticsaccounts"></a>Microsoft.DataLakeAnalytics/accounts

|Métrica|Nombre de métrica para mostrar|Unidad|Tipo de agregación|Descripción|Dimensions|
|---|---|---|---|---|---|
|JobEndedSuccess|Trabajos correctos|Count|Total|Recuento de trabajos realizados correctamente|None|
|JobEndedFailure|Trabajos con error|Count|Total|Recuento de trabajos con error|None|
|JobEndedCancelled|Trabajos cancelados|Count|Total|Recuento de trabajos cancelados|None|
|JobAUEndedSuccess|Tiempo AU correcto|Segundos|Total|Tiempo total AU para trabajos realizados correctamente|None|
|JobAUEndedFailure|Error de tiempo AU|Segundos|Total|Tiempo total AU para trabajos con error|None|
|JobAUEndedCancelled|Tiempo AU para cancelados|Segundos|Total|Tiempo total AU para trabajos cancelados|None|
|JobStage|Trabajos en fase|Count|Total|Número de trabajos en cada fase.|None|


## <a name="microsoftdatalakestoreaccounts"></a>Microsoft.DataLakeStore/accounts

|Métrica|Nombre de métrica para mostrar|Unidad|Tipo de agregación|Descripción|Dimensions|
|---|---|---|---|---|---|
|TotalStorage|Almacenamiento total|Bytes|Máxima|Cantidad total de datos almacenados en la cuenta.|None|
|DataWritten|Datos escritos|Bytes|Total|Cantidad total de datos escritos en la cuenta.|None|
|DataRead|Datos leídos|Bytes|Total|Cantidad total de datos leídos de la cuenta.|None|
|WriteRequests|Solicitudes de escritura|Count|Total|Número de solicitudes de escritura de datos en la cuenta.|None|
|ReadRequests|Solicitudes de lectura|Count|Total|Número de solicitudes de lectura de datos de la cuenta.|None|


## <a name="microsoftdatashareaccounts"></a>Microsoft.DataShare/accounts

|Métrica|Nombre de métrica para mostrar|Unidad|Tipo de agregación|Descripción|Dimensions|
|---|---|---|---|---|---|
|ShareCount|Recursos compartidos enviados|Count|Máxima|Número de recursos compartidos enviados en la cuenta|ShareName|
|ShareSubscriptionCount|Recursos compartidos recibidos|Count|Máxima|Número de recursos compartidos recibidos en la cuenta|ShareSubscriptionName|
|SucceededShareSynchronizations|Instantáneas de recurso compartido enviado correctamente|Count|Count|Número de instantáneas de recurso compartido enviado correctamente en la cuenta|None|
|FailedShareSynchronizations|Instantáneas de recurso compartido enviado con error|Count|Count|Número de instantáneas de recurso compartido enviado con error en la cuenta|None|
|SucceededShareSubscriptionSynchronizations|Instantáneas de recurso compartido recibido correctamente|Count|Count|Número de instantáneas de recurso compartido recibido correctamente en la cuenta|None|
|FailedShareSubscriptionSynchronizations|Instantáneas de recurso compartido recibido con error|Count|Count|Número de instantáneas de recurso compartido recibido con error en la cuenta|None|


## <a name="microsoftdbformariadbservers"></a>Microsoft.DBforMariaDB/servers

|Métrica|Nombre de métrica para mostrar|Unidad|Tipo de agregación|Descripción|Dimensions|
|---|---|---|---|---|---|
|cpu_percent|Porcentaje de CPU|Percent|Average|Porcentaje de CPU|None|
|memory_percent|Porcentaje de memoria|Percent|Average|Porcentaje de memoria|None|
|io_consumption_percent|Porcentaje de E/S|Percent|Average|Porcentaje de E/S|None|
|storage_percent|Porcentaje de almacenamiento|Percent|Average|Porcentaje de almacenamiento|None|
|storage_used|Almacenamiento utilizado|Bytes|Average|Almacenamiento utilizado|None|
|storage_limit|Límite de almacenamiento|Bytes|Máxima|Límite de almacenamiento|None|
|serverlog_storage_percent|Porcentaje de almacenamiento del registro del servidor|Percent|Average|Porcentaje de almacenamiento del registro del servidor|None|
|serverlog_storage_usage|Almacenamiento del registro del servidor usado|Bytes|Average|Almacenamiento del registro del servidor usado|None|
|serverlog_storage_limit|Límite de almacenamiento del registro del servidor|Bytes|Average|Límite de almacenamiento del registro del servidor|None|
|active_connections|Conexiones activas|Count|Average|Conexiones activas|None|
|connections_failed|Conexiones con errores|Count|Total|Conexiones con errores|None|
|seconds_behind_master|Intervalo de replicación en segundos|Count|Máxima|Intervalo de replicación en segundos|None|
|backup_storage_used|Almacenamiento de copia de seguridad utilizado|Bytes|Average|Almacenamiento de copia de seguridad utilizado|None|
|network_bytes_egress|Red interna|Bytes|Total|Red externa a través de conexiones activas|None|
|network_bytes_ingress|Red interna|Bytes|Total|Red interna a través de conexiones activas|None|


## <a name="microsoftdbformysqlservers"></a>Microsoft.DBforMySQL/servers

|Métrica|Nombre de métrica para mostrar|Unidad|Tipo de agregación|Descripción|Dimensions|
|---|---|---|---|---|---|
|cpu_percent|Porcentaje de CPU|Percent|Average|Porcentaje de CPU|None|
|memory_percent|Porcentaje de memoria|Percent|Average|Porcentaje de memoria|None|
|io_consumption_percent|Porcentaje de E/S|Percent|Average|Porcentaje de E/S|None|
|storage_percent|Porcentaje de almacenamiento|Percent|Average|Porcentaje de almacenamiento|None|
|storage_used|Almacenamiento utilizado|Bytes|Average|Almacenamiento utilizado|None|
|storage_limit|Límite de almacenamiento|Bytes|Máxima|Límite de almacenamiento|None|
|serverlog_storage_percent|Porcentaje de almacenamiento del registro del servidor|Percent|Average|Porcentaje de almacenamiento del registro del servidor|None|
|serverlog_storage_usage|Almacenamiento del registro del servidor usado|Bytes|Average|Almacenamiento del registro del servidor usado|None|
|serverlog_storage_limit|Límite de almacenamiento del registro del servidor|Bytes|Máxima|Límite de almacenamiento del registro del servidor|None|
|active_connections|Conexiones activas|Count|Average|Conexiones activas|None|
|connections_failed|Conexiones con errores|Count|Total|Conexiones con errores|None|
|seconds_behind_master|Intervalo de replicación en segundos|Count|Máxima|Intervalo de replicación en segundos|None|
|backup_storage_used|Almacenamiento de copia de seguridad utilizado|Bytes|Average|Almacenamiento de copia de seguridad utilizado|None|
|network_bytes_egress|Red interna|Bytes|Total|Red externa a través de conexiones activas|None|
|network_bytes_ingress|Red interna|Bytes|Total|Red interna a través de conexiones activas|None|


## <a name="microsoftdbforpostgresqlservers"></a>Microsoft.DBforPostgreSQL/servers

|Métrica|Nombre de métrica para mostrar|Unidad|Tipo de agregación|Descripción|Dimensions|
|---|---|---|---|---|---|
|cpu_percent|Porcentaje de CPU|Percent|Average|Porcentaje de CPU|None|
|memory_percent|Porcentaje de memoria|Percent|Average|Porcentaje de memoria|None|
|io_consumption_percent|Porcentaje de E/S|Percent|Average|Porcentaje de E/S|None|
|storage_percent|Porcentaje de almacenamiento|Percent|Average|Porcentaje de almacenamiento|None|
|storage_used|Almacenamiento utilizado|Bytes|Average|Almacenamiento utilizado|None|
|storage_limit|Límite de almacenamiento|Bytes|Máxima|Límite de almacenamiento|None|
|serverlog_storage_percent|Porcentaje de almacenamiento del registro del servidor|Percent|Average|Porcentaje de almacenamiento del registro del servidor|None|
|serverlog_storage_usage|Almacenamiento del registro del servidor usado|Bytes|Average|Almacenamiento del registro del servidor usado|None|
|serverlog_storage_limit|Límite de almacenamiento del registro del servidor|Bytes|Máxima|Límite de almacenamiento del registro del servidor|None|
|active_connections|Conexiones activas|Count|Average|Conexiones activas|None|
|connections_failed|Conexiones con errores|Count|Total|Conexiones con errores|None|
|backup_storage_used|Almacenamiento de copia de seguridad utilizado|Bytes|Average|Almacenamiento de copia de seguridad utilizado|None|
|network_bytes_egress|Red interna|Bytes|Total|Red externa a través de conexiones activas|None|
|network_bytes_ingress|Red interna|Bytes|Total|Red interna a través de conexiones activas|None|
|pg_replica_log_delay_in_seconds|Retraso entre réplicas|Segundos|Máxima|Intervalo de replicación en segundos|None|
|pg_replica_log_delay_in_bytes|Retraso máximo entre réplicas|Bytes|Máxima|Retardo en bytes de la réplica con más retraso|None|


## <a name="microsoftdbforpostgresqlserversv2"></a>Microsoft.DBforPostgreSQL/serversv2

|Métrica|Nombre de métrica para mostrar|Unidad|Tipo de agregación|Descripción|Dimensions|
|---|---|---|---|---|---|
|cpu_percent|Porcentaje de CPU|Percent|Average|Porcentaje de CPU|None|
|memory_percent|Porcentaje de memoria|Percent|Average|Porcentaje de memoria|None|
|iops|E/S|Count|Average|Operaciones de E/S por segundo|None|
|storage_percent|Porcentaje de almacenamiento|Percent|Average|Porcentaje de almacenamiento|None|
|storage_used|Almacenamiento utilizado|Bytes|Average|Almacenamiento utilizado|None|
|active_connections|Conexiones activas|Count|Average|Conexiones activas|None|
|network_bytes_egress|Red interna|Bytes|Total|Red externa a través de conexiones activas|None|
|network_bytes_ingress|Red interna|Bytes|Total|Red interna a través de conexiones activas|None|


## <a name="microsoftdbforpostgresqlsingleservers"></a>Microsoft.DBforPostgreSQL/singleservers

|Métrica|Nombre de métrica para mostrar|Unidad|Tipo de agregación|Descripción|Dimensions|
|---|---|---|---|---|---|
|cpu_percent|Porcentaje de CPU|Percent|Average|Porcentaje de CPU|None|
|memory_percent|Porcentaje de memoria|Percent|Average|Porcentaje de memoria|None|
|iops|E/S|Count|Average|Operaciones de E/S por segundo|None|
|storage_percent|Porcentaje de almacenamiento|Percent|Average|Porcentaje de almacenamiento|None|
|storage_used|Almacenamiento utilizado|Bytes|Average|Almacenamiento utilizado|None|
|active_connections|Conexiones activas|Count|Average|Conexiones activas|None|
|network_bytes_egress|Red interna|Bytes|Total|Red externa a través de conexiones activas|None|
|network_bytes_ingress|Red interna|Bytes|Total|Red interna a través de conexiones activas|None|
|connections_failed|Conexiones con errores|Count|Total|Conexiones con errores|None|
|connections_succeeded|Conexiones correctas|Count|Total|Conexiones correctas|None|
|maximum_used_transactionIDs|Máximo de identificadores de transacción usados|Count|Average|Máximo de identificadores de transacción usados|None|





## <a name="microsoftdevicesiothubs"></a>Microsoft.Devices/IotHubs

|Métrica|Nombre de métrica para mostrar|Unidad|Tipo de agregación|Descripción|Dimensions|
|---|---|---|---|---|---|
|d2c.telemetry.ingress.allProtocol|Intentos de envío de mensajes de telemetría|Count|Total|Número de mensajes de telemetría de dispositivo a la nube para enviar a IoT Hub|None|
|d2c.telemetry.ingress.success|Mensajes de telemetría enviados|Count|Total|Número de mensajes de telemetría de dispositivo a la nube enviados correctamente a IoT Hub|None|
|c2d.commands.egress.complete.success|C2D message deliveries completed (Entregas de mensajes de C2D completadas)|Count|Total|Número de entregas de mensajes de la nube al dispositivo que el dispositivo ha completado correctamente.|None|
|c2d.commands.egress.abandon.success|C2D messages abandoned (Mensajes de C2D abandonados)|Count|Total|Número de mensajes de la nube al dispositivo que el dispositivo ha abandonado.|None|
|c2d.commands.egress.reject.success|C2D messages rejected (Mensajes de C2D rechazados)|Count|Total|Número de mensajes de la nube al dispositivo que el dispositivo ha rechazado.|None|
|C2DMessagesExpired|C2D Messages Expired (preview) [Mensajes de C2D expirados (versión preliminar)]|Count|Total|Número de mensajes de la nube al dispositivo expirados|None|
|devices.totalDevices|Total de dispositivos (en desuso)|Count|Total|Número de dispositivos registrados en IoT Hub|None|
|devices.connectedDevices.allProtocol|Dispositivos conectados (en desuso) |Count|Total|Número de dispositivos conectados a IoT Hub|None|
|d2c.telemetry.egress.success|Enrutamiento: mensajes de telemetría entregados|Count|Total|Número de veces que los mensajes se han entregado correctamente a todos los puntos de conexión mediante el enrutamiento de IoT Hub. Si un mensaje se enruta a varios puntos de conexión, este valor aumenta en uno por cada entrega correcta. Si un mensaje se enruta al mismo punto de conexión varias veces, este valor aumenta en uno por cada entrega correcta.|None|
|d2c.telemetry.egress.dropped|Enrutamiento: mensajes de telemetría quitados |Count|Total|Número de veces que se han quitado los mensajes con el enrutamiento de IoT Hub a causa de puntos de conexión inactivos. Este valor no cuenta los mensajes entregados a la ruta de reserva porque los mensajes quitados no se entregan allí.|None|
|d2c.telemetry.egress.orphaned|Enrutamiento: mensajes de telemetría huérfanos |Count|Total|Número de veces que los mensajes eran huérfanos a causa del enrutamiento de IoT Hub porque no coincidían con las reglas de enrutamiento (incluida la regla de reserva). |None|
|d2c.telemetry.egress.invalid|Enrutamiento: mensajes de telemetría incompatibles|Count|Total|Número de veces que el enrutamiento de IoT Hub no ha podido entregar mensajes debido a una incompatibilidad con el punto de conexión. Este valor no incluye los reintentos.|None|
|d2c.telemetry.egress.fallback|Enrutamiento: mensajes entregados a la reserva|Count|Total|Número de veces que el enrutamiento de IoT Hub ha entregado mensajes al punto de conexión asociado con la ruta de reserva.|None|
|d2c.endpoints.egress.eventHubs|Enrutamiento: mensajes entregados al centro de eventos|Count|Total|Número de veces que el enrutamiento de IoT Hub ha entregado mensajes correctamente a los puntos de conexión del centro de eventos.|None|
|d2c.endpoints.latency.eventHubs|Enrutamiento: latencia de mensajes para el centro de eventos|Milisegundos|Average|Latencia media (milisegundos) entre la entrada de mensajes a IoT Hub y la entrada de mensajes a un punto de conexión del centro de eventos.|None|
|d2c.endpoints.egress.serviceBusQueues|Enrutamiento: mensajes entregados a la cola de Service Bus|Count|Total|Número de veces que el enrutamiento de IoT Hub ha entregado mensajes correctamente a los puntos de conexión de la cola de Service Bus.|None|
|d2c.endpoints.latency.serviceBusQueues|Enrutamiento: latencia de mensajes de la cola de Service Bus|Milisegundos|Average|Latencia media (milisegundos) entre la entrada de mensajes a IoT Hub y la entrada de mensajes de telemetría a un punto de conexión de la cola de Service Bus.|None|
|d2c.endpoints.egress.serviceBusTopics|Enrutamiento: mensajes entregados al tema de Service Bus|Count|Total|Número de veces que el enrutamiento de IoT Hub ha entregado mensajes correctamente a los puntos de conexión del tema de Service Bus.|None|
|d2c.endpoints.latency.serviceBusTopics|Enrutamiento: latencia de mensajes del tema de Service Bus|Milisegundos|Average|Latencia media (milisegundos) entre la entrada de mensajes a IoT Hub y la entrada de mensajes de telemetría a un punto de conexión del tema de Service Bus.|None|
|d2c.endpoints.egress.builtIn.events|Enrutamiento: mensajes entregados a mensajes/eventos|Count|Total|Número de veces que el enrutamiento de IoT Hub ha entregado mensajes correctamente al punto de conexión integrado (mensajes/eventos).|None|
|d2c.endpoints.latency.builtIn.events|Enrutamiento: latencia de mensajes para mensajes/eventos|Milisegundos|Average|Latencia media (milisegundos) entre la entrada de mensajes a IoT Hub y la entrada de mensajes de telemetría al punto de conexión integrado (mensajes/eventos).|None|
|d2c.endpoints.egress.storage|Enrutamiento: mensajes entregados al almacenamiento|Count|Total|Número de veces que el enrutamiento de IoT Hub ha entregado mensajes correctamente a los puntos de conexión de almacenamiento.|None|
|d2c.endpoints.latency.storage|Enrutamiento: latencia de mensajes para almacenamiento|Milisegundos|Average|Latencia media (milisegundos) entre la entrada de mensajes a IoT Hub y la entrada de mensajes de telemetría a un punto de conexión de almacenamiento.|None|
|d2c.endpoints.egress.storage.bytes|Enrutamiento: datos entregados al almacenamiento|Bytes|Total|Cantidad de datos (bytes) que el enrutamiento de IoT Hub ha entregado a los punto de conexión de almacenamiento.|None|
|d2c.endpoints.egress.storage.blobs|Enrutamiento: blobs entregados al almacenamiento|Count|Total|Número de veces que el enrutamiento de IoT Hub ha entregado blobs a los puntos de conexión de almacenamiento.|None|
|EventGridDeliveries|Entregas de Event Grid (versión preliminar)|Count|Total|El número de eventos de IoT Hub publicados en Event Grid. Use la dimensión de resultado para ver el número de solicitudes correctas e incorrectas. La dimensión EventType muestra el tipo de evento (https://aka.ms/ioteventgrid).|ResourceId,Result,EventType|
|EventGridLatency|Latencia de Event Grid (versión preliminar)|Milisegundos|Average|La latencia media (milisegundos) desde el momento en que se generó el evento de IoT Hub hasta el momento en que se publicó el evento en Event Grid. Este número es un promedio entre todos los tipos de evento. Use la dimensión EventType para ver la latencia de un tipo específico de evento.|ResourceId,EventType|
|RoutingDeliveries|Entregas de enrutamiento (versión preliminar)|Milisegundos|Total|Número de veces que IoT Hub intentó la entrega de mensajes a todos los puntos de conexión mediante el enrutamiento. Para ver el número de intentos completados correctamente o con error, use la dimensión Result. Para ver el motivo del error, como no válido, quitado o huérfano, use la dimensión FailureReasonCategory. También puede usar las dimensiones EndpointName y EndpointType para entender cuántos mensajes se entregaron a los distintos puntos de conexión. El valor de la métrica se incrementa en uno por cada intento de entrega, incluido si el mensaje se entrega a varios puntos de conexión o si el mensaje se entrega varias veces en el mismo punto de conexión.|ResourceId,EndpointType,EndpointName,FailureReasonCategory,Result,RoutingSource|
|RoutingDeliveryLatency|Latencia de entrega de enrutamiento (versión preliminar)|Milisegundos|Average|Latencia media (milisegundos) entre la entrada de mensajes a IoT Hub y la entrada de mensajes de telemetría a un punto de conexión. Puede usar las dimensiones EndpointName y EndpointType para comprender la latencia de los distintos puntos de conexión.|ResourceId,EndpointType,EndpointName,RoutingSource|
|d2c.twin.read.success|Lecturas gemelas correctas de los dispositivos|Count|Total|El recuento de todas las lecturas gemelas correctas iniciadas por el dispositivo.|None|
|d2c.twin.read.failure|Lecturas gemelas con error de los dispositivos|Count|Total|El recuento de todas las lecturas gemelas con error iniciadas por el dispositivo.|None|
|d2c.twin.read.size|Tamaño de la respuesta de las lecturas gemelas de dispositivos|Bytes|Average|El valor medio, mínimo y máximo de todas las lecturas gemelas correctas iniciadas por el dispositivo.|None|
|d2c.twin.update.success|Actualizaciones gemelas correctas de los dispositivos|Count|Total|El recuento de todas las actualizaciones gemelas correctas iniciadas por el dispositivo.|None|
|d2c.twin.update.failure|Actualizaciones gemelas con error de los dispositivos|Count|Total|El recuento de todas las actualizaciones gemelas con error iniciadas por el dispositivo.|None|
|d2c.twin.update.size|Tamaño de las actualizaciones gemelas de los dispositivos|Bytes|Average|El tamaño medio, mínimo y máximo de todas las actualizaciones gemelas correctas iniciadas por el dispositivo.|None|
|c2d.methods.success|Invocaciones correctas al método directo|Count|Total|El número de todas las llamadas correctas al método directo.|None|
|c2d.methods.failure|Invocaciones al método directo con error|Count|Total|El número de todas las llamadas al método directo con error.|None|
|c2d.methods.requestSize|Tamaño de la solicitud de las invocaciones a métodos directos|Bytes|Average|El valor medio, mínimo y máximo de todas las solicitudes correctas de método directo.|None|
|c2d.methods.responseSize|Tamaño de la respuesta de las invocaciones a métodos directos|Bytes|Average|El valor medio, mínimo y máximo de todas las respuestas correctas de método directo.|None|
|c2d.twin.read.success|Lecturas gemelas correctas del back-end|Count|Total|El recuento de todas las lecturas gemelas correctas iniciadas por el back-end.|None|
|c2d.twin.read.failure|Lecturas gemelas con error del back-end|Count|Total|El recuento de todas las lecturas gemelas con error iniciadas por el back-end.|None|
|c2d.twin.read.size|Tamaño de la respuesta de las lecturas gemelas del back-end|Bytes|Average|El valor medio, mínimo y máximo de todas las lecturas gemelas correctas iniciadas por el back-end.|None|
|c2d.twin.update.success|Actualizaciones gemelas correctas del back-end|Count|Total|El recuento de todas las actualizaciones gemelas correctas iniciadas por el back-end.|None|
|c2d.twin.update.failure|Actualizaciones gemelas con error del back-end|Count|Total|El recuento de todas las actualizaciones gemelas con error iniciadas por el back-end.|None|
|c2d.twin.update.size|Tamaño de las actualizaciones gemelas del back-end|Bytes|Average|El tamaño medio, mínimo y máximo de todas las actualizaciones gemelas correctas iniciadas por el back-end.|None|
|twinQueries.success|Consultas gemelas correctas|Count|Total|El recuento de todas las consultas gemelas correctas.|None|
|twinQueries.failure|Consultas gemelas con error|Count|Total|El recuento de todas las consultas gemelas con error.|None|
|twinQueries.resultSize|Tamaño de resultado de consultas gemelas|Bytes|Average|El valor medio, mínimo y máximo del tamaño del resultado de todas las consultas gemelas correctas.|None|
|jobs.createTwinUpdateJob.success|Creaciones correctas de trabajos de actualización gemelos|Count|Total|El recuento de todas las creaciones correctas de trabajos de actualización gemelos.|None|
|jobs.createTwinUpdateJob.failure|Creaciones con error de trabajos de actualización gemelos|Count|Total|El recuento de todas las creaciones con error de trabajos de actualización gemelos.|None|
|jobs.createDirectMethodJob.success|Creaciones correctas de trabajos de invocación de método|Count|Total|El recuento de todas las creaciones correctas de los trabajos de invocación de método directos.|None|
|jobs.createDirectMethodJob.failure|Creaciones con error de trabajos de invocación de método|Count|Total|El recuento de todas las creaciones con error de los trabajos de invocación de método directos.|None|
|jobs.listJobs.success|Llamadas correctas para enumerar trabajos|Count|Total|El recuento de todas las llamadas correctas para enumerar trabajos.|None|
|jobs.listJobs.failure|Llamadas con error para enumerar trabajos|Count|Total|El recuento de todas las llamadas con error para enumerar trabajos.|None|
|jobs.cancelJob.success|Cancelaciones de trabajos correctas|Count|Total|El recuento de todas las llamadas correctas para cancelar un trabajo.|None|
|jobs.cancelJob.failure|Cancelaciones de trabajos con error|Count|Total|El recuento de todas las llamadas con error para cancelar un trabajo.|None|
|jobs.queryJobs.success|Consultas de trabajo correctas|Count|Total|El recuento de todas las llamadas correctas para consultar trabajos.|None|
|jobs.queryJobs.failure|Consultas de trabajo con error|Count|Total|El recuento de todas las llamadas con error para consultar trabajos.|None|
|jobs.completed|Trabajos completados|Count|Total|El recuento de todos los trabajos completados.|None|
|jobs.failed|Trabajos con error|Count|Total|El recuento de todos los trabajos con error.|None|
|d2c.telemetry.ingress.sendThrottle|Número de errores de limitación|Count|Total|Número de errores de limitación debido a las limitaciones de rendimiento del dispositivo|None|
|dailyMessageQuotaUsed|Número total de mensajes usados|Count|Average|Número de mensajes totales usados hoy|None|
|deviceDataUsage|Uso total de datos del dispositivo|Bytes|Total|Bytes transferidos hacia y desde cualquier dispositivo conectado al Centro de IOT|None|
|deviceDataUsageV2|Uso total de datos del dispositivo (versión preliminar)|Bytes|Total|Bytes transferidos hacia y desde cualquier dispositivo conectado al Centro de IOT|None|
|totalDeviceCount|Total de dispositivos (versión preliminar)|Count|Average|Número de dispositivos registrados en IoT Hub|None|
|connectedDeviceCount|Dispositivos conectados (versión preliminar)|Count|Average|Número de dispositivos conectados a IoT Hub|None|
|configuraciones|Métricas de configuración|Count|Total|Métricas de las operaciones de configuración|None|


## <a name="microsoftdevicesprovisioningservices"></a>Microsoft.Devices/provisioningServices

|Métrica|Nombre de métrica para mostrar|Unidad|Tipo de agregación|Descripción|Dimensions|
|---|---|---|---|---|---|
|RegistrationAttempts|Intentos de registro|Count|Total|Número de intentos de registro de dispositivos|ProvisioningServiceName,IotHubName,Status|
|DeviceAssignments|Dispositivos asignados|Count|Total|Número de dispositivos asignados a un IoT Hub|ProvisioningServiceName,IotHubName|
|AttestationAttempts|Intentos de atestación|Count|Total|Número de intentos de atestación de dispositivos|ProvisioningServiceName,Status,Protocol|




## <a name="microsoftdocumentdbdatabaseaccounts"></a>Microsoft.DocumentDB/databaseAccounts

|Métrica|Nombre de métrica para mostrar|Unidad|Tipo de agregación|Descripción|Dimensions|
|---|---|---|---|---|---|
|AddRegion|Región agregada|Count|Count|Región agregada|Region|
|AvailableStorage|Almacenamiento disponible|Bytes|Total|Almacenamiento total disponible notificado a una granularidad de cinco minutos|CollectionName,DatabaseName,Region|
|CassandraConnectionClosures|Cierres de conexión de Cassandra|Count|Total|Número de conexiones de Cassandra que se han cerrado, notificadas en una granularidad de 1 minuto|APIType,Region,ClosureReason|
|CassandraKeyspaceDelete|Espacio de claves de Cassandra eliminado|Count|Count|Espacio de claves de Cassandra eliminado|ResourceName,ApiKind,ApiKindResourceType,OperationType|
|CassandraKeyspaceThroughputUpdate|Rendimiento de un espacio de claves de Cassandra actualizado|Count|Count|Rendimiento de un espacio de claves de Cassandra actualizado|ResourceName,ApiKind,ApiKindResourceType,IsThroughputRequest|
|CassandraKeyspaceUpdate|Espacio de claves de Cassandra actualizado|Count|Count|Espacio de claves de Cassandra actualizado|ResourceName,ApiKind,ApiKindResourceType,IsThroughputRequest|
|CassandraRequestCharges|Cargos de solicitud de Cassandra|Count|Total|Unidades de solicitud consumidas para las solicitudes de Cassandra realizadas|APIType,DatabaseName,CollectionName,Region,OperationType,ResourceType|
|CassandraRequests|Solicitudes de Cassandra|Count|Count|Número de solicitudes de Cassandra realizadas|APIType,DatabaseName,CollectionName,Region,OperationType,ResourceType,ErrorCode|
|CassandraTableDelete|Tabla de Cassandra eliminada|Count|Count|Tabla de Cassandra eliminada|ResourceName,ChildResourceName,ApiKind,ApiKindResourceType,OperationType|
|CassandraTableThroughputUpdate|Rendimiento de una tabla de Cassandra actualizado|Count|Count|Rendimiento de una tabla de Cassandra actualizado|ResourceName,ChildResourceName,ApiKind,ApiKindResourceType,IsThroughputRequest|
|CassandraTableUpdate|Tabla de Cassandra actualizada|Count|Count|Tabla de Cassandra actualizada|ResourceName,ChildResourceName,ApiKind,ApiKindResourceType,IsThroughputRequest|
|CreateAccount|Cuenta creada|Count|Count|Cuenta creada|None|
|DataUsage|Uso de datos|Bytes|Total|Uso total de datos notificado a una granularidad de cinco minutos|CollectionName,DatabaseName,Region|
|DeleteAccount|Cuenta eliminada|Count|Count|Cuenta eliminada|None|
|DocumentCount|Recuento de documentos|Count|Total|Recuento total de documentos notificado a una granularidad de cinco minutos|CollectionName,DatabaseName,Region|
|DocumentQuota|Cuota de documentos|Bytes|Total|Cuota total de almacenamiento notificada a una granularidad de cinco minutos|CollectionName,DatabaseName,Region|
|GremlinDatabaseDelete|Base de datos de Gremlin eliminada|Count|Count|Base de datos de Gremlin eliminada|ResourceName,ApiKind,ApiKindResourceType,OperationType|
|GremlinDatabaseThroughputUpdate|Rendimiento de una base de datos de Gremlin actualizado|Count|Count|Rendimiento de una base de datos de Gremlin actualizado|ResourceName,ApiKind,ApiKindResourceType,IsThroughputRequest|
|GremlinDatabaseUpdate|Base de datos de Gremlin actualizada|Count|Count|Base de datos de Gremlin actualizada|ResourceName,ApiKind,ApiKindResourceType,IsThroughputRequest|
|GremlinGraphDelete|Gráfico de Gremlin eliminado|Count|Count|Gráfico de Gremlin eliminado|ResourceName,ChildResourceName,ApiKind,ApiKindResourceType,OperationType|
|GremlinGraphThroughputUpdate|Rendimiento de un grafo de Gremlin actualizado|Count|Count|Rendimiento de un grafo de Gremlin actualizado|ResourceName,ChildResourceName,ApiKind,ApiKindResourceType,IsThroughputRequest|
|GremlinGraphUpdate|Grafo de Gremlin actualizado|Count|Count|Grafo de Gremlin actualizado|ResourceName,ChildResourceName,ApiKind,ApiKindResourceType,IsThroughputRequest|
|IndexUsage|Uso de índice|Bytes|Total|Uso total del índice notificado a una granularidad de cinco minutos|CollectionName,DatabaseName,Region|
|MetadataRequests|Solicitudes de metadatos|Count|Count|Recuento de las solicitudes de metadatos. Cosmos DB mantiene la colección de metadatos del sistema para cada cuenta, lo que le permite enumerar las colecciones, las bases de datos y sus configuraciones de forma gratuita.|DatabaseName,CollectionName,Region,StatusCode,Role|
|MongoCollectionDelete|Colección de Mongo eliminada|Count|Count|Colección de Mongo eliminada|ResourceName,ChildResourceName,ApiKind,ApiKindResourceType,OperationType|
|MongoCollectionThroughputUpdate|Rendimiento de la colección de Mongo actualizado|Count|Count|Rendimiento de la colección de Mongo actualizado|ResourceName,ChildResourceName,ApiKind,ApiKindResourceType,IsThroughputRequest|
|MongoCollectionUpdate|Colección de Mongo actualizada|Count|Count|Colección de Mongo actualizada|ResourceName,ChildResourceName,ApiKind,ApiKindResourceType,IsThroughputRequest|
|MongoDBDatabaseUpdate|Base de datos de Mongo actualizada|Count|Count|Base de datos de Mongo actualizada|ResourceName,ApiKind,ApiKindResourceType,IsThroughputRequest|
|MongoDatabaseDelete|Base de datos de Mongo eliminada|Count|Count|Base de datos de Mongo eliminada|ResourceName,ApiKind,ApiKindResourceType,OperationType|
|MongoDatabaseThroughputUpdate|Rendimiento de la base de datos de Mongo actualizado|Count|Count|Rendimiento de la base de datos de Mongo actualizado|ResourceName,ApiKind,ApiKindResourceType,IsThroughputRequest|
|MongoRequestCharge|Cargo de la solicitud de Mongo|Count|Total|Unidades de la solicitud de Mongo consumidas|DatabaseName,CollectionName,Region,CommandName,ErrorCode,Status|
|MongoRequests|Solicitudes de Mongo|Count|Count|Número de solicitudes de Mongo realizadas|DatabaseName,CollectionName,Region,CommandName,ErrorCode,Status|
|MongoRequestsCount|Mongo Request Rate (Velocidad de solicitudes de Mongo)|CountPerSecond|Average|Recuento de solicitudes de Mongo por segundo|DatabaseName,CollectionName,Region,CommandName,ErrorCode|
|MongoRequestsDelete|Velocidad de la solicitud de eliminación de Mongo|CountPerSecond|Average|Solicitudes de eliminación de Mongo por segundo|DatabaseName,CollectionName,Region,CommandName,ErrorCode|
|MongoRequestsInsert|Velocidad de la solicitud de inserción de Mongo|CountPerSecond|Average|Recuento de inserciones de Mongo por segundo|DatabaseName,CollectionName,Region,CommandName,ErrorCode|
|MongoRequestsQuery|Velocidad de la solicitud de consulta de Mongo|CountPerSecond|Average|Recuento de consultas de Mongo por segundo|DatabaseName,CollectionName,Region,CommandName,ErrorCode|
|MongoRequestsUpdate|Mongo Update Request Rate (Velocidad de solicitudes de actualización de Mongo)|CountPerSecond|Average|Solicitudes de actualización de Mongo por segundo|DatabaseName,CollectionName,Region,CommandName,ErrorCode|
|NormalizedRUConsumption|Consumo de RU normalizado|Percent|Máxima|Porcentaje máximo de consumo de RU por minuto|CollectionName,DatabaseName,Region|
|ProvisionedThroughput|Rendimiento aprovisionado|Count|Máxima|Rendimiento aprovisionado|DatabaseName,CollectionName|
|RegionFailover|Región conmutada por error|Count|Count|Región conmutada por error|None|
|RemoveRegion|Región quitada|Count|Count|Región quitada|Region|
|ReplicationLatency|Latencia de replicación P99|MilliSeconds|Average|Latencia de replicación P99 entre regiones de origen y destino para la cuenta habilitada geográficamente|SourceRegion,TargetRegion|
|ServerSideLatency|Latencia del servidor|MilliSeconds|Average|Latencia del servidor|DatabaseName,CollectionName,Region,ConnectionMode,OperationType,PublicAPIType|
|ServiceAvailability|Disponibilidad del servicio|Percent|Average|Disponibilidad de solicitudes de cuenta en una granularidad por hora, día o mes|None|
|SqlContainerDelete|Contenedor SQL eliminado|Count|Count|Contenedor SQL eliminado|ResourceName,ChildResourceName,ApiKind,ApiKindResourceType,OperationType|
|SqlContainerThroughputUpdate|Rendimiento de un contenedor de SQL actualizado|Count|Count|Rendimiento de un contenedor de SQL actualizado|ResourceName,ChildResourceName,ApiKind,ApiKindResourceType,IsThroughputRequest|
|SqlContainerUpdate|Contenedor SQL actualizado|Count|Count|Contenedor SQL actualizado|ResourceName,ChildResourceName,ApiKind,ApiKindResourceType,IsThroughputRequest|
|SqlDatabaseDelete|Base de datos SQL eliminada|Count|Count|Base de datos SQL eliminada|ResourceName,ApiKind,ApiKindResourceType,OperationType|
|SqlDatabaseThroughputUpdate|Rendimiento de una base de datos SQL actualizado|Count|Count|Rendimiento de una base de datos SQL actualizado|ResourceName,ApiKind,ApiKindResourceType,IsThroughputRequest|
|SqlDatabaseUpdate|Base de datos SQL actualizada|Count|Count|Base de datos SQL actualizada|ResourceName,ApiKind,ApiKindResourceType,IsThroughputRequest|
|TableTableDelete|Tabla AzureTable eliminada|Count|Count|Tabla AzureTable eliminada|ResourceName,ApiKind,ApiKindResourceType,OperationType|
|TableTableThroughputUpdate|Rendimiento de la tabla AzureTable actualizado|Count|Count|Rendimiento de la tabla AzureTable actualizado|ResourceName,ApiKind,ApiKindResourceType,IsThroughputRequest|
|TableTableUpdate|Tabla AzureTable actualizada|Count|Count|Tabla AzureTable actualizada|ResourceName,ApiKind,ApiKindResourceType,IsThroughputRequest|
|TotalRequestUnits|Unidades de solicitud totales|Count|Total|Unidades de solicitud consumidas|DatabaseName,CollectionName,Region,StatusCode,OperationType,Status|
|TotalRequests|Total de solicitudes|Count|Count|Número de solicitudes realizadas|DatabaseName,CollectionName,Region,StatusCode,OperationType,Status|
|UpdateAccountKeys|Claves de cuenta actualizadas|Count|Count|Claves de cuenta actualizadas|KeyType|
|UpdateAccountNetworkSettings|Configuración de red de la cuenta actualizada|Count|Count|Configuración de red de la cuenta actualizada|None|
|UpdateAccountReplicationSettings|Configuración de replicación de cuenta actualizada|Count|Count|Configuración de replicación de cuenta actualizada|None|
|UpdateDiagnosticsSettings|Configuración de diagnóstico de cuenta actualizada|Count|Count|Configuración de diagnóstico de cuenta actualizada|DiagnosticSettingsName,ResourceGroupName|



## <a name="microsoftenterpriseknowledgegraphservices"></a>Microsoft.EnterpriseKnowledgeGraph/services

|Métrica|Nombre de métrica para mostrar|Unidad|Tipo de agregación|Descripción|Dimensions|
|---|---|---|---|---|---|
|TransactionCount|Transaction Count (Recuento de transacciones)|Count|Count|Recuento total de transacciones|TransactionCount|
|SuccessCount|Success Count (Recuento de transacciones correctas)|Count|Count|Recuento de transacciones correctas|SuccessCount|
|FailureCount|Failure Count (Recuento de errores)|Count|Count|Recuento de transacciones con errores|FailureCount|
|SuccessLatency|Success Latency (Latencia de transacciones correctas)|MilliSeconds|Average|Latencia de transacciones correctas|SuccessCount|

## <a name="microsofteventgriddomains"></a>Microsoft.EventGrid/domains

|Métrica|Nombre de métrica para mostrar|Unidad|Tipo de agregación|Descripción|Dimensions|
|---|---|---|---|---|---|
|PublishSuccessCount|Eventos publicados|Count|Total|Número total de eventos publicados en este tema|Tema|
|PublishFailCount|Eventos no publicados|Count|Total|Número total de eventos que no se han podido publicar en este tema|Topic,ErrorType,Error|
|PublishSuccessLatencyInMs|Latencia de publicación correcta|Milisegundos|Total|Latencia de publicación correcta en milisegundos|None|
|MatchedEventCount|Eventos coincidentes|Count|Total|Número total de eventos que coinciden con esta suscripción de eventos|Topic,EventSubscriptionName,DomainEventSubscriptionName|
|DeliveryAttemptFailCount|Eventos de error en la entrega|Count|Total|Número total de eventos que no se han podido entregar a esta suscripción de eventos|Topic,EventSubscriptionName,DomainEventSubscriptionName,Error,ErrorType|
|DeliverySuccessCount|Eventos entregados|Count|Total|Número total de eventos que se han entregado a esta suscripción de eventos|Topic,EventSubscriptionName,DomainEventSubscriptionName|
|DestinationProcessingDurationInMs|Duración del procesamiento de destino|Milisegundos|Average|Duración del procesamiento de destino en milisegundos|Topic,EventSubscriptionName,DomainEventSubscriptionName|
|DroppedEventCount|Eventos quitados|Count|Total|Número total de eventos quitados que coinciden con esta suscripción de eventos|Topic,EventSubscriptionName,DomainEventSubscriptionName,DropReason|
|DeadLetteredCount|Eventos en la cola de mensajes fallidos|Count|Total|Número total de eventos en la cola de eventos fallidos que coinciden con esta suscripción de eventos|Topic,EventSubscriptionName,DomainEventSubscriptionName,DeadLetterReason|

## <a name="microsofteventgridtopics"></a>Microsoft.EventGrid/topics

|Métrica|Nombre de métrica para mostrar|Unidad|Tipo de agregación|Descripción|Dimensions|
|---|---|---|---|---|---|
|PublishSuccessCount|Eventos publicados|Count|Total|Número total de eventos publicados en este tema|None|
|PublishFailCount|Eventos no publicados|Count|Total|Número total de eventos que no se han podido publicar en este tema|ErrorType,Error|
|UnmatchedEventCount|Eventos no coincidentes|Count|Total|Número total de eventos que no coinciden con ninguna de las suscripciones de eventos de este tema|None|
|PublishSuccessLatencyInMs|Latencia de publicación correcta|Milisegundos|Total|Latencia de publicación correcta en milisegundos|None|
|MatchedEventCount|Eventos coincidentes|Count|Total|Número total de eventos que coinciden con esta suscripción de eventos|EventSubscriptionName|
|DeliveryAttemptFailCount|Eventos de error en la entrega|Count|Total|Número total de eventos que no se han podido entregar a esta suscripción de eventos|Error,ErrorType,EventSubscriptionName|
|DeliverySuccessCount|Eventos entregados|Count|Total|Número total de eventos que se han entregado a esta suscripción de eventos|EventSubscriptionName|
|DestinationProcessingDurationInMs|Duración del procesamiento de destino|Milisegundos|Average|Duración del procesamiento de destino en milisegundos|EventSubscriptionName|
|DroppedEventCount|Eventos quitados|Count|Total|Número total de eventos quitados que coinciden con esta suscripción de eventos|DropReason,EventSubscriptionName|
|DeadLetteredCount|Eventos en la cola de mensajes fallidos|Count|Total|Número total de eventos en la cola de eventos fallidos que coinciden con esta suscripción de eventos|DeadLetterReason,EventSubscriptionName|

## <a name="microsofteventgridsystemtopics"></a>Microsoft.EventGrid/systemTopics

|Métrica|Nombre de métrica para mostrar|Unidad|Tipo de agregación|Descripción|Dimensions|
|---|---|---|---|---|---|
|PublishSuccessCount|Eventos publicados|Count|Total|Número total de eventos publicados en este tema|None|
|PublishFailCount|Eventos no publicados|Count|Total|Número total de eventos que no se han podido publicar en este tema|ErrorType,Error|
|UnmatchedEventCount|Eventos no coincidentes|Count|Total|Número total de eventos que no coinciden con ninguna de las suscripciones de eventos de este tema|None|
|PublishSuccessLatencyInMs|Latencia de publicación correcta|Milisegundos|Total|Latencia de publicación correcta en milisegundos|None|
|MatchedEventCount|Eventos coincidentes|Count|Total|Número total de eventos que coinciden con esta suscripción de eventos|EventSubscriptionName|
|DeliveryAttemptFailCount|Eventos de error en la entrega|Count|Total|Número total de eventos que no se han podido entregar a esta suscripción de eventos|Error,ErrorType,EventSubscriptionName|
|DeliverySuccessCount|Eventos entregados|Count|Total|Número total de eventos que se han entregado a esta suscripción de eventos|EventSubscriptionName|
|DestinationProcessingDurationInMs|Duración del procesamiento de destino|Milisegundos|Average|Duración del procesamiento de destino en milisegundos|EventSubscriptionName|
|DroppedEventCount|Eventos quitados|Count|Total|Número total de eventos quitados que coinciden con esta suscripción de eventos|DropReason,EventSubscriptionName|
|DeadLetteredCount|Eventos en la cola de mensajes fallidos|Count|Total|Número total de eventos en la cola de eventos fallidos que coinciden con esta suscripción de eventos|DeadLetterReason,EventSubscriptionName|

## <a name="microsofteventgrideventsubscriptions"></a>Microsoft.EventGrid/eventSubscriptions

|Métrica|Nombre de métrica para mostrar|Unidad|Tipo de agregación|Descripción|Dimensions|
|---|---|---|---|---|---|
|MatchedEventCount|Eventos coincidentes|Count|Total|Número total de eventos que coinciden con esta suscripción de eventos|None|
|DeliveryAttemptFailCount|Eventos de error en la entrega|Count|Total|Número total de eventos que no se han podido entregar a esta suscripción de eventos|Error,ErrorType|
|DeliverySuccessCount|Eventos entregados|Count|Total|Número total de eventos que se han entregado a esta suscripción de eventos|None|
|DestinationProcessingDurationInMs|Duración del procesamiento de destino|Milisegundos|Average|Duración del procesamiento de destino en milisegundos|None|
|DroppedEventCount|Eventos quitados|Count|Total|Número total de eventos quitados que coinciden con esta suscripción de eventos|DropReason|
|DeadLetteredCount|Eventos en la cola de mensajes fallidos|Count|Total|Número total de eventos en la cola de eventos fallidos que coinciden con esta suscripción de eventos|DeadLetterReason|

## <a name="microsofteventgridextensiontopics"></a>Microsoft.EventGrid/extensionTopics

|Métrica|Nombre de métrica para mostrar|Unidad|Tipo de agregación|Descripción|Dimensions|
|---|---|---|---|---|---|
|PublishSuccessCount|Eventos publicados|Count|Total|Número total de eventos publicados en este tema|None|
|PublishFailCount|Eventos no publicados|Count|Total|Número total de eventos que no se han podido publicar en este tema|ErrorType,Error|
|UnmatchedEventCount|Eventos no coincidentes|Count|Total|Número total de eventos que no coinciden con ninguna de las suscripciones de eventos de este tema|None|
|PublishSuccessLatencyInMs|Latencia de publicación correcta|Milisegundos|Total|Latencia de publicación correcta en milisegundos|None|




## <a name="microsofteventhubnamespaces"></a>Microsoft.EventHub/namespaces

|Métrica|Nombre de métrica para mostrar|Unidad|Tipo de agregación|Descripción|Dimensions|
|---|---|---|---|---|---|
|SuccessfulRequests|Solicitudes correctas|Count|Total|Solicitudes correctas de Microsoft.EventHub.|EntityName,OperationResult|
|ServerErrors|Errores del servidor.|Count|Total|Errores del servidor de Microsoft.EventHub.|EntityName,OperationResult|
|UserErrors|Errores de usuario.|Count|Total|Errores de usuario de Microsoft.EventHub.|EntityName,OperationResult|
|QuotaExceededErrors|Cuota de errores superada.|Count|Total|Cuota de errores superada de Microsoft.EventHub.|EntityName,OperationResult|
|ThrottledRequests|Solicitudes limitadas.|Count|Total|Solicitudes limitadas de Microsoft.EventHub.|EntityName,OperationResult|
|IncomingRequests|Solicitudes entrantes|Count|Total|Solicitudes entrantes de Microsoft.EventHub.|EntityName|
|IncomingMessages|Mensajes entrantes|Count|Total|Mensajes entrantes de Microsoft.EventHub.|EntityName|
|OutgoingMessages|Mensajes salientes|Count|Total|Mensajes salientes de Microsoft.EventHub.|EntityName|
|IncomingBytes|Bytes entrantes.|Bytes|Total|Bytes entrantes de Microsoft.EventHub.|EntityName|
|OutgoingBytes|Bytes salientes.|Bytes|Total|Bytes salientes de Microsoft.EventHub.|EntityName|
|ActiveConnections|ActiveConnections|Count|Average|Total de conexiones activas de Microsoft.EventHub.|None|
|ConnectionsOpened|Conexiones abiertas.|Count|Average|Conexiones abiertas de Microsoft.EventHub.|EntityName|
|ConnectionsClosed|Conexiones cerradas.|Count|Average|Conexiones cerradas de Microsoft.EventHub.|EntityName|
|CaptureBacklog|Capturar el trabajo pendiente.|Count|Total|Capturar el trabajo pendiente de Microsoft.EventHub.|EntityName|
|CapturedMessages|Mensajes capturados.|Count|Total|Mensajes capturados de Microsoft.EventHub.|EntityName|
|CapturedBytes|Bytes capturados.|Bytes|Total|Bytes capturados de Microsoft.EventHub.|EntityName|
|Size|Size|Bytes|Average|Tamaño de EventHub en bytes.|EntityName|
|INREQS|Solicitudes entrantes (en desuso)|Count|Total|Total de solicitudes de envío entrantes de un espacio de nombres (en desuso)|None|
|SUCCREQ|Solicitudes correctas (en desuso)|Count|Total|Total de solicitudes correctas para un espacio de nombres (en desuso)|None|
|FAILREQ|Solicitudes con error (en desuso)|Count|Total|Total de solicitudes con error para un espacio de nombres (en desuso)|None|
|SVRBSY|Errores de servidor ocupado (en desuso)|Count|Total|Errores totales de servidor ocupado para un espacio de nombres (en desuso)|None|
|INTERR|Errores internos del servidor (en desuso)|Count|Total|Errores totales de servidor interno para un espacio de nombres (en desuso)|None|
|MISCERR|Otros errores (en desuso)|Count|Total|Total de solicitudes con error para un espacio de nombres (en desuso)|None|
|INMSGS|Incoming Messages (obsolete) (Deprecated) [Mensajes entrantes (obsoletos) (en desuso)]|Count|Total|Total de mensajes entrantes para un espacio de nombres. Esta métrica está en desuso. En su lugar, use la métrica Mensajes entrantes (en desuso)|None|
|EHINMSGS|Mensajes entrantes (en desuso)|Count|Total|Total de mensajes entrantes para un espacio de nombres (en desuso)|None|
|OUTMSGS|Outgoing Messages (obsolete) (Deprecated) [Mensajes salientes (obsoletos) (en desuso)]|Count|Total|Total de mensajes salientes para un espacio de nombres. Esta métrica está en desuso. En su lugar, use la métrica Mensajes salientes (en desuso)|None|
|EHOUTMSGS|Mensajes salientes (en desuso)|Count|Total|Total de mensajes salientes para un espacio de nombres (en desuso)|None|
|EHINMBS|Incoming bytes (obsolete) (Deprecated) [Bytes entrantes (obsoletos) (en desuso)]|Bytes|Total|Rendimiento de mensajes entrantes del centro de eventos para un espacio de nombres. Esta métrica está en desuso. En su lugar, use la métrica Bytes de entrada (en desuso)|None|
|EHINBYTES|Bytes de entrada (en desuso)|Bytes|Total|Rendimiento de mensajes entrantes del centro de eventos para un espacio de nombres (en desuso)|None|
|EHOUTMBS|Outgoing bytes (obsolete) (Deprecated) [Bytes salientes (obsoletos) (en desuso)]|Bytes|Total|Rendimiento de mensajes salientes del centro de eventos para un espacio de nombres. Esta métrica está en desuso. En su lugar, use la métrica Bytes de salida (en desuso)|None|
|EHOUTBYTES|Bytes de salida (en desuso)|Bytes|Total|Rendimiento de mensajes salientes del centro de eventos para un espacio de nombres (en desuso)|None|
|EHABL|Mensajes de trabajo pendiente de archivado (en desuso)|Count|Total|Mensajes de archivado del centro de eventos en el trabajo pendiente para un espacio de nombres (en desuso)|None|
|EHAMSGS|Mensajes de archivado (en desuso)|Count|Total|Mensajes de archivado del centro de eventos en un espacio de nombres (en desuso)|None|
|EHAMBS|Rendimiento de mensajes de archivado (en desuso)|Bytes|Total|Rendimiento de mensajes de archivado del centro de eventos en un espacio de nombres (en desuso)|None|

## <a name="microsofteventhubclusters"></a>Microsoft.EventHub/clusters

|Métrica|Nombre de métrica para mostrar|Unidad|Tipo de agregación|Descripción|Dimensions|
|---|---|---|---|---|---|
|SuccessfulRequests|Solicitudes correctas|Count|Total|Solicitudes correctas de Microsoft.EventHub.|OperationResult|
|ServerErrors|Errores del servidor.|Count|Total|Errores del servidor de Microsoft.EventHub.|OperationResult|
|UserErrors|Errores de usuario.|Count|Total|Errores de usuario de Microsoft.EventHub.|OperationResult|
|QuotaExceededErrors|Cuota de errores superada.|Count|Total|Cuota de errores superada de Microsoft.EventHub.|OperationResult|
|ThrottledRequests|Solicitudes limitadas.|Count|Total|Solicitudes limitadas de Microsoft.EventHub.|OperationResult|
|IncomingRequests|Solicitudes entrantes|Count|Total|Solicitudes entrantes de Microsoft.EventHub.|None|
|IncomingMessages|Mensajes entrantes|Count|Total|Mensajes entrantes de Microsoft.EventHub.|None|
|OutgoingMessages|Mensajes salientes|Count|Total|Mensajes salientes de Microsoft.EventHub.|None|
|IncomingBytes|Bytes entrantes.|Bytes|Total|Bytes entrantes de Microsoft.EventHub.|None|
|OutgoingBytes|Bytes salientes.|Bytes|Total|Bytes salientes de Microsoft.EventHub.|None|
|ActiveConnections|ActiveConnections|Count|Average|Total de conexiones activas de Microsoft.EventHub.|None|
|ConnectionsOpened|Conexiones abiertas.|Count|Average|Conexiones abiertas de Microsoft.EventHub.|None|
|ConnectionsClosed|Conexiones cerradas.|Count|Average|Conexiones cerradas de Microsoft.EventHub.|None|
|CaptureBacklog|Capturar el trabajo pendiente.|Count|Total|Capturar el trabajo pendiente de Microsoft.EventHub.|None|
|CapturedMessages|Mensajes capturados.|Count|Total|Mensajes capturados de Microsoft.EventHub.|None|
|CapturedBytes|Bytes capturados.|Bytes|Total|Bytes capturados de Microsoft.EventHub.|None|
|CPU|CPU|Percent|Máxima|Uso de la CPU para el clúster del centro de eventos como porcentaje|Role|
|AvailableMemory|Memoria disponible|Percent|Máxima|Memoria disponible para el clúster del centro de eventos como un porcentaje de la memoria total.|Role|
|Size|Tamaño de EventHub en bytes.|Bytes|Average|Tamaño de EventHub en bytes.|Role|


## <a name="microsofthdinsightclusters"></a>Microsoft.HDInsight/clusters

|Métrica|Nombre de métrica para mostrar|Unidad|Tipo de agregación|Descripción|Dimensions|
|---|---|---|---|---|---|
|GatewayRequests|Solicitudes de puerta de enlace|Count|Total|Número de solicitudes de puerta de enlace|HttpStatus|
|CategorizedGatewayRequests|Solicitudes de puerta de enlace categorizadas|Count|Total|Número de solicitudes de puerta de enlace por categorías (1xx/2xx/3xx/4xx/5xx)|HttpStatus|
|NumActiveWorkers|Número de trabajos activos|Count|Máxima|Número de trabajos activos|MetricName|


## <a name="microsoftinsightsautoscalesettings"></a>Microsoft.Insights/AutoscaleSettings

|Métrica|Nombre de métrica para mostrar|Unidad|Tipo de agregación|Descripción|Dimensions|
|---|---|---|---|---|---|
|ObservedMetricValue|Valor de métrica observado|Count|Average|El valor calculado por el escalado automático cuando se ejecuta|MetricTriggerSource|
|MetricThreshold|Umbral de métrica|Count|Average|El umbral configurado de escalado automático cuando se ejecutó el escalado automático.|MetricTriggerRule|
|ObservedCapacity|Capacidad observada|Count|Average|La capacidad informada al escalado automático cuando se ejecutó.|None|
|ScaleActionsInitiated|Acciones de escalado iniciadas|Count|Total|La dirección de la operación de escalado.|ScaleDirection|

## <a name="microsoftinsightscomponents"></a>Microsoft.Insights/Components

|Métrica|Nombre de métrica para mostrar|Unidad|Tipo de agregación|Descripción|Dimensions|
|---|---|---|---|---|---|
|availabilityResults/availabilityPercentage|Disponibilidad|Percent|Average|Porcentaje de pruebas de disponibilidad completadas correctamente|availabilityResult/name,availabilityResult/location|
|availabilityResults/count|Pruebas de disponibilidad|Count|Count|Recuento de pruebas de disponibilidad|availabilityResult/name,availabilityResult/location,availabilityResult/success|
|availabilityResults/duration|Duración de la prueba de disponibilidad|MilliSeconds|Average|Duración de la prueba de disponibilidad|availabilityResult/name,availabilityResult/location,availabilityResult/success|
|browserTimings/networkDuration|Tiempo de conexión de red de carga de página|MilliSeconds|Average|Tiempo transcurrido entre la solicitud del usuario y la conexión de red. Incluye la búsqueda de DNS y la conexión de transporte.|None|
|browserTimings/processingDuration|Tiempo de procesamiento del cliente|MilliSeconds|Average|Tiempo transcurrido entre la recepción del último byte de un documento hasta que se carga el DOM. Todavía se pueden procesar solicitudes asincrónicas.|None|
|browserTimings/receiveDuration|Tiempo de recepción de respuesta|MilliSeconds|Average|Tiempo transcurrido entre el primer y el último byte, o hasta la desconexión.|None|
|browserTimings/sendDuration|Tiempo de solicitud de envío|MilliSeconds|Average|Tiempo transcurrido entre la conexión de red y la recepción del primer byte.|None|
|browserTimings/totalDuration|Tiempo de carga de página del explorador|MilliSeconds|Average|Tiempo que transcurre entre la solicitud del usuario hasta que se cargan el DOM, las hojas de estilo, los scripts y las imágenes.|None|
|dependencies/count|Llamadas de dependencia|Count|Count|Número de llamadas realizadas por la aplicación a recursos externos.|dependency/type,dependency/performanceBucket,dependency/success,dependency/target,dependency/resultCode,operation/synthetic,cloud/roleInstance,cloud/roleName|
|dependencies/duration|Duración de la dependencia|MilliSeconds|Average|Duración de las llamadas realizadas por la aplicación a recursos externos.|dependency/type,dependency/performanceBucket,dependency/success,dependency/target,dependency/resultCode,operation/synthetic,cloud/roleInstance,cloud/roleName|
|dependencies/failed|Errores de llamadas de dependencia|Count|Count|Número de llamadas de dependencia erróneas realizadas por la aplicación a recursos externos.|dependency/type,dependency/performanceBucket,dependency/success,dependency/target,dependency/resultCode,operation/synthetic,cloud/roleInstance,cloud/roleName|
|pageViews/count|Vistas de página|Count|Count|Número de vistas de página.|operation/synthetic,cloud/roleName|
|pageViews/duration|Tiempo de carga de la vista de página|MilliSeconds|Average|Tiempo de carga de la vista de página|operation/synthetic,cloud/roleName|
|performanceCounters/requestExecutionTime|Tiempo de ejecución de solicitud HTTP|MilliSeconds|Average|Tiempo de ejecución de la solicitud más reciente.|cloud/roleInstance|
|performanceCounters/requestsInQueue|Solicitudes HTTP en la cola de la aplicación|Count|Average|Longitud de la cola de solicitudes de aplicación.|cloud/roleInstance|
|performanceCounters/requestsPerSecond|Tasa de solicitudes HTTP|CountPerSecond|Average|Velocidad de todas las solicitudes a la aplicación por segundo desde ASP.NET.|cloud/roleInstance|
|performanceCounters/exceptionsPerSecond|Velocidad de excepciones|CountPerSecond|Average|Recuento de excepciones controladas y no controladas de las cuales se informó a Windows, incluidas las excepciones de .NET y las excepciones no administradas que se convierten en excepciones de .NET.|cloud/roleInstance|
|performanceCounters/processIOBytesPerSecond|Velocidad de E/S del proceso|BytesPerSecond|Average|Número total de bytes por segundo leídos y escritos en los archivos, la red y los dispositivos.|cloud/roleInstance|
|performanceCounters/processCpuPercentage|CPU de procesos|Percent|Average|Porcentaje de tiempo transcurrido que todos los subprocesos del proceso han usado el procesador para ejecutar instrucciones. Puede variar entre 0 y 100. Esta métrica indica el rendimiento solo del proceso w3wp.|cloud/roleInstance|
|performanceCounters/processorCpuPercentage|Tiempo de procesador|Percent|Average|Porcentaje de tiempo que invierte el procesador en subprocesos no inactivos.|cloud/roleInstance|
|performanceCounters/memoryAvailableBytes|Memoria disponible|Bytes|Average|Memoria física disponible de inmediato para su asignación a un proceso o para uso del sistema.|cloud/roleInstance|
|performanceCounters/processPrivateBytes|Bytes privados del proceso|Bytes|Average|Memoria asignada exclusivamente a los procesos de la aplicación supervisada.|cloud/roleInstance|
|requests/duration|Tiempo de respuesta del servidor|MilliSeconds|Average|Tiempo entre que se recibe una solicitud HTTP y se termina de enviar la respuesta.|request/performanceBucket,request/resultCode,operation/synthetic,cloud/roleInstance,request/success,cloud/roleName|
|requests/count|Solicitudes de servidor|Count|Count|Número de solicitudes HTTP completadas.|request/performanceBucket,request/resultCode,operation/synthetic,cloud/roleInstance,request/success,cloud/roleName|
|requests/failed|Error en las solicitudes|Count|Count|Número de solicitudes HTTP marcadas como erróneas. En la mayoría de los casos, se trata de solicitudes con un código de respuesta > = 400 y no igual a 401.|request/performanceBucket,request/resultCode,request/success,operation/synthetic,cloud/roleInstance,cloud/roleName|
|requests/rate|Tasa de solicitudes del servidor|CountPerSecond|Average|Tasa de solicitudes del servidor por segundo|request/performanceBucket,request/resultCode,operation/synthetic,cloud/roleInstance,request/success,cloud/roleName|
|exceptions/count|Excepciones|Count|Count|Recuento combinado de todas las excepciones no detectadas.|cloud/roleName,cloud/roleInstance,client/type|
|exceptions/browser|Excepciones de explorador|Count|Count|Recuento de excepciones no detectadas en el explorador.|client/isServer,cloud/roleName|
|exceptions/server|Excepciones de servidor|Count|Count|Número de excepciones no detectadas producidas en la aplicación de servidor.|client/isServer,cloud/roleName,cloud/roleInstance|
|traces/count|Traces|Count|Count|Número de documentos de seguimiento|trace/severityLevel,operation/synthetic,cloud/roleName,cloud/roleInstance|


## <a name="microsoftiotcentraliotapps"></a>Microsoft.IoTCentral/IoTApps

|Métrica|Nombre de métrica para mostrar|Unidad|Tipo de agregación|Descripción|Dimensions|
|---|---|---|---|---|---|
|connectedDeviceCount|Dispositivos conectados totales|Count|Average|Número de dispositivos conectados a IoT Central|None|
|c2d.property.read.success|Lecturas de propiedades de dispositivo correctas de IoT Central|Count|Total|Recuento de todas las lecturas de propiedades correctas iniciadas desde IoT Central|None|
|c2d.property.read.failure|Lecturas de propiedades de dispositivo con error de IoT Central|Count|Total|Recuento de todas las lecturas de propiedades con error iniciadas desde IoT Central|None|
|d2c.property.read.success|Lecturas de propiedades de dispositivo correctas de los dispositivos|Count|Total|Recuento de todas las lecturas de propiedades correctas iniciadas desde dispositivos|None|
|d2c.property.read.failure|Lecturas de propiedades de dispositivo con error de los dispositivos|Count|Total|Recuento de todas las lecturas de propiedades con error iniciadas desde dispositivos|None|
|c2d.property.update.success|Actualizaciones de propiedades de dispositivo correctas de IoT Central|Count|Total|Recuento de todas las actualizaciones de propiedades correctas iniciadas desde IoT Central|None|
|c2d.property.update.failure|Actualizaciones de propiedades de dispositivo con error de IoT Central|Count|Total|Recuento de todas las actualizaciones de propiedades con error iniciadas desde IoT Central|None|
|d2c.property.update.success|Actualizaciones de propiedades de dispositivo correctas de los dispositivos|Count|Total|Recuento de todas las actualizaciones de propiedades correctas iniciadas desde los dispositivos|None|
|d2c.property.update.failure|Actualizaciones de propiedades de dispositivo con errores de los dispositivos|Count|Total|Recuento de todas las actualizaciones de propiedades con error iniciadas desde dispositivos|None|


## <a name="microsoftkeyvaultvaults"></a>Microsoft.KeyVault/vaults

|Métrica|Nombre de métrica para mostrar|Unidad|Tipo de agregación|Descripción|Dimensions|
|---|---|---|---|---|---|
|ServiceApiHit|Número total de visitas de la API de servicio|Count|Count|Número total de visitas de la API de servicio|ActivityType,ActivityName|
|ServiceApiLatency|Latencia general de la API de servicio|Milisegundos|Average|Latencia general de las solicitudes de la API de servicio|ActivityType,ActivityName,StatusCode,StatusCodeClass|
|ServiceApiResult|Número total de resultados de la API de servicio|Count|Count|Número total de resultados de la API de servicio|ActivityType,ActivityName,StatusCode,StatusCodeClass|
|SaturationShoebox|Saturación general del almacén|Percent|Average|Capacidad usada del almacén|ActivityType,ActivityName,TransactionType|
|Disponibilidad|Disponibilidad total del almacén|Percent|Average|Disponibilidad de solicitudes del almacén|ActivityType,ActivityName,StatusCode,StatusCodeClass|

## <a name="microsoftkustoclusters"></a>Microsoft.Kusto/Clusters

|Métrica|Nombre de métrica para mostrar|Unidad|Tipo de agregación|Descripción|Dimensions|
|---|---|---|---|---|---|
|CacheUtilization|Uso de la caché|Percent|Average|Nivel de uso en el ámbito de clúster|None|
|QueryDuration|Duración de la consulta|Milisegundos|Average|Duración de las consultas en segundos|QueryStatus|
|IngestionUtilization|Uso de la ingesta|Percent|Average|Proporción de ranuras de ingesta usadas en el clúster|None|
|KeepAlive|Mantener conexión|Count|Average|La comprobación de integridad indica las respuestas del clúster a las consultas|None|
|IngestionVolumeInMB|Volumen de ingesta (en MB)|Count|Total|Volumen global de la ingesta de datos del clúster (en MB)|Base de datos|
|IngestionLatencyInSeconds|Ingestion latency (in seconds) [Latencia de la ingesta (en segundos)]|Segundos|Average|Tiempo de ingesta del origen (p. ej., el mensaje está en el centro de eventos) al clúster en segundos|None|
|EventsProcessedForEventHubs|Events processed (for Event/IoT Hubs) [Eventos procesados (para instancias de Event Hubs o IoT Hub)]|Count|Total|Número de eventos que ha procesado el clúster en la ingesta desde el centro de eventos o Iot Hub.|EventStatus|
|IngestionResult|Resultado de la ingesta|Count|Count|Número de operaciones de ingesta|IngestionResultDetails|
|CPU|CPU|Percent|Average|Nivel de uso de CPU|None|
|ContinuousExportNumOfRecordsExported|Exportación continua: número de registros exportados|Count|Total|Número de registros exportados que se han activado para cada artefacto de almacenamiento escrito durante la operación de exportación.|ContinuousExportName,Database|
|ExportUtilization|Utilización de la exportación|Percent|Máxima|Utilización de la exportación|None|
|ContinuousExportPendingCount|Recuento pendiente de exportación continua|Count|Máxima|El número de trabajos de exportación continua pendientes listos para su ejecución|None|
|ContinuousExportMaxLatenessMinutes|Máx. de demora en la exportación continua|Count|Máxima|Demora (en minutos) indicada por los trabajos de exportación continua del clúster|None|
|ContinuousExportResult|Continuous Export Result (Resultado de la exportación continua)|Count|Count|Indica si la exportación continua se realizó correctamente o no.|ContinuousExportName,Result,Database|
|StreamingIngestDuration|Streaming Ingest Duration (Duración de la ingesta de streaming)|Milisegundos|Average|Duración de la ingesta de streaming en milisegundos|None|
|StreamingIngestDataRate|Streaming Ingest Data Rate (Velocidad de datos de la ingesta de streaming)|Count|Average|Velocidad de datos de la ingesta de streaming (MB por segundo)|None|
|SteamingIngestRequestRate|Streaming Ingest Request Rate [Velocidad de solicitudes de ingesta de streaming]|Count|RateRequestsPerSecond|Tasa de solicitudes de la ingesta de streaming (solicitudes por segundo)|None|
|StreamingIngestResults|Streaming Ingest Result (Resultado de la ingesta de streaming)|Count|Average|Resultado de la ingesta de streaming|Resultado|
|TotalNumberOfConcurrentQueries|Número total de consultas simultáneas|Count|Total|Número total de consultas simultáneas|None|
|TotalNumberOfThrottledQueries|Número total de consultas limitadas|Count|Total|Número total de consultas limitadas|None|
|TotalNumberOfThrottledCommands|Número total de comandos limitados|Count|Total|Número total de comandos limitados|CommandType|
|TotalNumberOfExtents|Número total de extensiones|Count|Total|Número total de extensiones de datos|None|
|InstanceCount|Recuento de instancias|Count|Average|Recuento total de instancias|None|


## <a name="microsoftlogicworkflows"></a>Microsoft.Logic/workflows

|Métrica|Nombre de métrica para mostrar|Unidad|Tipo de agregación|Descripción|Dimensions|
|---|---|---|---|---|---|
|RunsStarted|Ejecuciones iniciadas|Count|Total|Número de ejecuciones de flujo de trabajo iniciadas|None|
|RunsCompleted|Ejecuciones completadas|Count|Total|Número de ejecuciones de flujo de trabajo completadas|None|
|RunsSucceeded|Ejecuciones correctas|Count|Total|Número de ejecuciones de flujo de trabajo correctas|None|
|RunsFailed|Ejecuciones con errores|Count|Total|Número de ejecuciones de flujo de trabajo con errores|None|
|RunsCancelled|Ejecuciones canceladas|Count|Total|Número de ejecuciones de flujo de trabajo canceladas|None|
|RunLatency|Latencia de ejecución|Segundos|Average|Latencia de ejecuciones de flujo de trabajo completadas|None|
|RunSuccessLatency|Latencia de ejecuciones correctas|Segundos|Average|Latencia de ejecuciones de flujo de trabajo correctas|None|
|RunThrottledEvents|Ejecución de eventos limitados|Count|Total|Número de eventos limitados de desencadenador o acciones de flujo de trabajo|None|
|RunStartThrottledEvents|Ejecución de eventos limitados de inicio|Count|Total|Número de eventos limitados de inicio de ejecución de flujo de trabajo.|None|
|RunFailurePercentage|Porcentaje de errores de ejecución|Percent|Total|Porcentaje de ejecuciones de flujo de trabajo con errores.|None|
|ActionsStarted|Acciones iniciadas |Count|Total|Número de acciones de flujo de trabajo iniciadas|None|
|ActionsCompleted|Acciones completadas |Count|Total|Número de acciones de flujo de trabajo completadas|None|
|ActionsSucceeded|Acciones correctas |Count|Total|Número de acciones de flujo de trabajo correctas|None|
|ActionsFailed|Acciones con errores |Count|Total|Número de acciones de flujo de trabajo con errores|None|
|ActionsSkipped|Acciones omitidas |Count|Total|Número de acciones de flujo de trabajo omitidas|None|
|ActionLatency|Latencia de acciones |Segundos|Average|Latencia de acciones de flujo de trabajo completadas|None|
|ActionSuccessLatency|Latencia de acciones correctas |Segundos|Average|Latencia de acciones de flujo de trabajo correctas|None|
|ActionThrottledEvents|Eventos limitados de acciones|Count|Total|Número de eventos limitados de acciones de flujo de trabajo|None|
|TriggersStarted|Desencadenadores iniciados |Count|Total|Número de desencadenadores de flujo de trabajo iniciados|None|
|TriggersCompleted|Desencadenadores completados |Count|Total|Número de desencadenadores de flujo de trabajo completados|None|
|TriggersSucceeded|Desencadenadores correctos |Count|Total|Número de desencadenadores de flujo de trabajo correctos|None|
|TriggersFailed|Desencadenadores con errores |Count|Total|Número de desencadenadores de flujo de trabajo con errores|None|
|TriggersSkipped|Desencadenadores omitidos|Count|Total|Número de desencadenadores de flujo de trabajo omitidos|None|
|TriggersFired|Desencadenadores activados |Count|Total|Número de desencadenadores de flujo de trabajo activados|None|
|TriggerLatency|Latencia de desencadenador |Segundos|Average|Latencia de desencadenadores de flujo de trabajo completados|None|
|TriggerFireLatency|Latencia de desencadenador activado |Segundos|Average|Latencia de desencadenadores de flujo de trabajo activados|None|
|TriggerSuccessLatency|Latencia de desencadenadores correctos |Segundos|Average|Latencia de desencadenadores de flujo de trabajo correctos|None|
|TriggerThrottledEvents|Eventos limitados de desencadenadores|Count|Total|Número de eventos limitados de desencadenador de flujo de trabajo|None|
|BillableActionExecutions|Ejecuciones de acciones facturables|Count|Total|Número de ejecuciones de acciones de flujo de trabajo que se facturan.|None|
|BillableTriggerExecutions|Ejecuciones del desencadenador facturable|Count|Total|Número de ejecuciones del desencadenador de flujo de trabajo que se factura.|None|
|TotalBillableExecutions|Total de ejecuciones facturables|Count|Total|Número de ejecuciones de flujo de trabajo que se factura.|None|
|BillingUsageNativeOperation|Uso de facturación para las ejecuciones de la operación nativa|Count|Total|Número de ejecuciones de la operación nativa que se facturan.|None|
|BillingUsageStandardConnector|Uso de facturación para las ejecuciones del conector estándar|Count|Total|Número de ejecuciones del conector estándar que se facturan.|None|
|BillingUsageStorageConsumption|Uso de facturación para las ejecuciones de consumo de almacenamiento|Count|Total|Número de ejecuciones del consumo de almacenamiento que se facturan.|None|

## <a name="microsoftlogicintegrationserviceenvironments"></a>Microsoft.Logic/integrationServiceEnvironments

|Métrica|Nombre de métrica para mostrar|Unidad|Tipo de agregación|Descripción|Dimensions|
|---|---|---|---|---|---|
|RunsStarted|Ejecuciones iniciadas|Count|Total|Número de ejecuciones de flujo de trabajo iniciadas|None|
|RunsCompleted|Ejecuciones completadas|Count|Total|Número de ejecuciones de flujo de trabajo completadas|None|
|RunsSucceeded|Ejecuciones correctas|Count|Total|Número de ejecuciones de flujo de trabajo correctas|None|
|RunsFailed|Ejecuciones con errores|Count|Total|Número de ejecuciones de flujo de trabajo con errores|None|
|RunsCancelled|Ejecuciones canceladas|Count|Total|Número de ejecuciones de flujo de trabajo canceladas|None|
|RunLatency|Latencia de ejecución|Segundos|Average|Latencia de ejecuciones de flujo de trabajo completadas|None|
|RunSuccessLatency|Latencia de ejecuciones correctas|Segundos|Average|Latencia de ejecuciones de flujo de trabajo correctas|None|
|RunThrottledEvents|Ejecución de eventos limitados|Count|Total|Número de eventos limitados de desencadenador o acciones de flujo de trabajo|None|
|RunStartThrottledEvents|Ejecución de eventos limitados de inicio|Count|Total|Número de eventos limitados de inicio de ejecución de flujo de trabajo.|None|
|RunFailurePercentage|Porcentaje de errores de ejecución|Percent|Total|Porcentaje de ejecuciones de flujo de trabajo con errores.|None|
|ActionsStarted|Acciones iniciadas |Count|Total|Número de acciones de flujo de trabajo iniciadas|None|
|ActionsCompleted|Acciones completadas |Count|Total|Número de acciones de flujo de trabajo completadas|None|
|ActionsSucceeded|Acciones correctas |Count|Total|Número de acciones de flujo de trabajo correctas|None|
|ActionsFailed|Acciones con errores |Count|Total|Número de acciones de flujo de trabajo con errores|None|
|ActionsSkipped|Acciones omitidas |Count|Total|Número de acciones de flujo de trabajo omitidas|None|
|ActionLatency|Latencia de acciones |Segundos|Average|Latencia de acciones de flujo de trabajo completadas|None|
|ActionSuccessLatency|Latencia de acciones correctas |Segundos|Average|Latencia de acciones de flujo de trabajo correctas|None|
|ActionThrottledEvents|Eventos limitados de acciones|Count|Total|Número de eventos limitados de acciones de flujo de trabajo|None|
|TriggersStarted|Desencadenadores iniciados |Count|Total|Número de desencadenadores de flujo de trabajo iniciados|None|
|TriggersCompleted|Desencadenadores completados |Count|Total|Número de desencadenadores de flujo de trabajo completados|None|
|TriggersSucceeded|Desencadenadores correctos |Count|Total|Número de desencadenadores de flujo de trabajo correctos|None|
|TriggersFailed|Desencadenadores con errores |Count|Total|Número de desencadenadores de flujo de trabajo con errores|None|
|TriggersSkipped|Desencadenadores omitidos|Count|Total|Número de desencadenadores de flujo de trabajo omitidos|None|
|TriggersFired|Desencadenadores activados |Count|Total|Número de desencadenadores de flujo de trabajo activados|None|
|TriggerLatency|Latencia de desencadenador |Segundos|Average|Latencia de desencadenadores de flujo de trabajo completados|None|
|TriggerFireLatency|Latencia de desencadenador activado |Segundos|Average|Latencia de desencadenadores de flujo de trabajo activados|None|
|TriggerSuccessLatency|Latencia de desencadenadores correctos |Segundos|Average|Latencia de desencadenadores de flujo de trabajo correctos|None|
|TriggerThrottledEvents|Eventos limitados de desencadenadores|Count|Total|Número de eventos limitados de desencadenador de flujo de trabajo|None|
|IntegrationServiceEnvironmentWorkflowProcessorUsage|Uso del procesador de flujo de trabajo para el entorno del servicio de integración|Percent|Average|Uso del procesador de flujo de trabajo para el entorno de servicio de integración.|None|
|IntegrationServiceEnvironmentWorkflowMemoryUsage|Uso del memoria de flujo de trabajo para el entorno del servicio de integración|Percent|Average|Uso del memoria de flujo de trabajo para el entorno del servicio de integración.|None|
|IntegrationServiceEnvironmentConnectorProcessorUsage|Uso del procesador del conector para el entorno del servicio de integración|Percent|Average|Uso del procesador del conector para el entorno del servicio de integración.|None|
|IntegrationServiceEnvironmentConnectorMemoryUsage|Uso de la memoria del conector para el entorno del servicio de integración|Percent|Average|Uso de la memoria del conector para el entorno del servicio de integración.|None|

## <a name="microsoftmachinelearningservicesworkspaces"></a>Microsoft.MachineLearningServices/workspaces

|Métrica|Nombre de métrica para mostrar|Unidad|Tipo de agregación|Descripción|Dimensions|
|---|---|---|---|---|---|
|Ejecuciones canceladas|Ejecuciones canceladas|Count|Total|Número de ejecuciones canceladas para esta área de trabajo|Scenario,RunType,PublishedPipelineId,ComputeType,PipelineStepType|
|Cancelar ejecuciones solicitadas|Cancelar ejecuciones solicitadas|Count|Total|Número de ejecuciones en las que se solicitó la cancelación para esta área de trabajo|Scenario,RunType,PublishedPipelineId,ComputeType,PipelineStepType|
|Ejecuciones finalizadas|Ejecuciones finalizadas|Count|Total|Número de ejecuciones finalizadas correctamente para esta área de trabajo|Scenario,RunType,PublishedPipelineId,ComputeType,PipelineStepType|
|Ejecuciones con error|Ejecuciones con error|Count|Total|Número de ejecuciones con error para esta área de trabajo|Scenario,RunType,PublishedPipelineId,ComputeType,PipelineStepType|
|Finalizando ejecuciones|Finalizando ejecuciones|Count|Total|Número de ejecuciones especificadas para finalizar el estado de esta área de trabajo|Scenario,RunType,PublishedPipelineId,ComputeType,PipelineStepType|
|Ejecuciones que no responden|Ejecuciones que no responden|Count|Total|Número de ejecuciones que no responden para esta área de trabajo|Scenario,RunType,PublishedPipelineId,ComputeType,PipelineStepType|
|Ejecuciones no iniciadas|Ejecuciones no iniciadas|Count|Total|Número de ejecuciones en estado no iniciado para esta área de trabajo|Scenario,RunType,PublishedPipelineId,ComputeType,PipelineStepType|
|Preparando ejecuciones|Preparando ejecuciones|Count|Total|Número de ejecuciones que se están preparando para esta área de trabajo|Scenario,RunType,PublishedPipelineId,ComputeType,PipelineStepType|
|Aprovisionamiento de ejecuciones|Aprovisionamiento de ejecuciones|Count|Total|Número de ejecuciones que se están aprovisionando para esta área de trabajo|Scenario,RunType,PublishedPipelineId,ComputeType,PipelineStepType|
|Ejecuciones en cola|Ejecuciones en cola|Count|Total|Número de ejecuciones en cola para esta área de trabajo|Scenario,RunType,PublishedPipelineId,ComputeType,PipelineStepType|
|Ejecuciones iniciadas|Ejecuciones iniciadas|Count|Total|Número de ejecuciones iniciadas para esta área de trabajo|Scenario,RunType,PublishedPipelineId,ComputeType,PipelineStepType|
|Iniciando ejecuciones|Iniciando ejecuciones|Count|Total|Número de ejecuciones iniciadas para esta área de trabajo|Scenario,RunType,PublishedPipelineId,ComputeType,PipelineStepType|
|Errors|Errors|Count|Total|Número de errores de ejecución en esta área de trabajo|Escenario|
|Advertencias|Advertencias|Count|Total|Número de advertencias de ejecución en esta área de trabajo|Escenario|
|Model Register Succeeded (Registro de modelo realizado correctamente)|Model Register Succeeded (Registro de modelo realizado correctamente)|Count|Total|Número de registros de modelo que se realizaron correctamente en este espacio de trabajo.|Escenario|
|Model Register Failed|Model Register Failed (Error en el registro de modelo)|Count|Total|Número de registros de modelo erróneos de este espacio de trabajo.|Scenario,StatusCode|
|Model Deploy Started|Model Deploy Started (Implementación de modelo iniciada)|Count|Total|Número de implementaciones de modelos que se iniciaron en este espacio de trabajo.|Escenario|
|Model Deploy Succeeded|Model Deploy Succeeded (Implementación de modelo correcta)|Count|Total|Número de implementaciones de modelos que se realizaron correctamente en este espacio de trabajo.|Escenario|
|Error al realizar la implementación de modelo|Error al realizar la implementación de modelo|Count|Total|Número de implementaciones de modelo erróneas de este espacio de trabajo.|Scenario,StatusCode|
|Total Nodes|Total Nodes (Nodos totales)|Count|Average|Número de nodos totales. Este total incluye algunos de los nodos activos, nodos inactivos, nodos inutilizables, nodos con prioridad, nodos de salida.|Scenario,ClusterName|
|Active Nodes|Active Nodes (Nodos activos)|Count|Average|Número de nodos activos. Son los nodos que ejecutan activamente un trabajo.|Scenario,ClusterName|
|Idle Nodes|Idle Nodes (Nodos inactivos)|Count|Average|Número de nodos inactivos. Los nodos inactivos son los nodos que no ejecutan ningún trabajo, pero que pueden aceptar un nuevo trabajo si está disponible.|Scenario,ClusterName|
|Nodos no utilizables|Nodos no utilizables|Count|Average|Número de nodos inutilizables. Los nodos inutilizables no funcionan debido a algún problema que no se puede resolver. Azure reciclará estos nodos.|Scenario,ClusterName|
|Nodos con prioridad|Preempted Nodes (Nodos con prioridad)|Count|Average|Número de nodos con prioridad. Estos nodos son los nodos de prioridad baja que se quitan del grupo de nodos disponible.|Scenario,ClusterName|
|Leaving Nodes|Leaving Nodes (Nodos de salida)|Count|Average|Número de nodos de salida. Los nodos de salida son los nodos que acaban de procesar un trabajo y pasan al estado inactivo.|Scenario,ClusterName|
|Núcleos totales|Total Cores (Núcleos totales)|Count|Average|Número de núcleos totales|Scenario,ClusterName|
|Núcleos activos|Active Cores (Núcleos activos)|Count|Average|Número de núcleos activos|Scenario,ClusterName|
|Núcleos inactivos|Idle Cores (Núcleos inactivos)|Count|Average|Número de núcleos inactivos|Scenario,ClusterName|
|Núcleos no utilizables|Unusable Cores (Núcleos no utilizables)|Count|Average|Número de núcleos no utilizables|Scenario,ClusterName|
|Núcleos con prioridad|Preempted Cores (Núcleos con prioridad)|Count|Average|Número de núcleos con prioridad|Scenario,ClusterName|
|Núcleos de salida|Leaving Cores (Núcleos de salida)|Count|Average|Número de núcleos de salida|Scenario,ClusterName|
|Porcentaje de uso de la cuota|Porcentaje de uso de la cuota|Count|Average|Porcentaje de cuota utilizada|Scenario,ClusterName,VmFamilyName,VmPriority|
|CpuUtilization|CpuUtilization|Count|Average|CPU (versión preliminar)|Scenario,runId,NodeId,CreatedTime|
|GpuUtilization|GpuUtilization|Count|Average|GPU (versión preliminar)|Scenario,runId,NodeId,CreatedTime,DeviceId|


## <a name="microsoftmapsaccounts"></a>Microsoft.Maps/accounts

|Métrica|Nombre de métrica para mostrar|Unidad|Tipo de agregación|Descripción|Dimensions|
|---|---|---|---|---|---|
|Uso|Uso|Count|Count|Recuento de llamadas a la API|ApiCategory,ApiName,ResultType,ResponseCode|
|Disponibilidad|Disponibilidad|Percent|Average|Disponibilidad de las API|ApiCategory,ApiName|

## <a name="microsoftmediamediaservicesstreamingendpoints"></a>Microsoft.Media/mediaservices/streamingEndpoints

|Métrica|Nombre de métrica para mostrar|Unidad|Tipo de agregación|Descripción|Dimensions|
|---|---|---|---|---|---|
|Salida|Salida|Bytes|Total|Cantidad de datos de salida, en bytes.|OutputFormat|
|SuccessE2ELatency|Latencia correcta de extremo a extremo|Milisegundos|Average|Latencia media de las solicitudes correctas en milisegundos.|OutputFormat|
|Requests|Requests|Count|Total|Solicitudes a un punto de conexión de streaming.|OutputFormat,HttpStatusCode,ErrorCode|


## <a name="microsoftmediamediaservices"></a>Microsoft.Media/mediaservices

|Métrica|Nombre de métrica para mostrar|Unidad|Tipo de agregación|Descripción|Dimensions|
|---|---|---|---|---|---|
|AssetQuota|Cuota de recursos|Count|Average|Cantidad de recursos permitidos para la cuenta de servicio multimedia actual|None|
|AssetCount|Número de recursos|Count|Average|Cantidad de recursos creados en la cuenta de servicio multimedia actual|None|
|AssetQuotaUsedPercentage|Porcentaje de cuota de recursos usado|Percent|Average|Porcentaje de recursos usado en la cuenta de servicio multimedia actual|None|
|ContentKeyPolicyCount|Cuota de directiva de clave de contenido|Count|Average|Cantidad de directivas de clave de contenido permitida para la cuenta de servicio multimedia actual|None|
|ContentKeyPolicyCount|Número de directivas de clave de contenido|Count|Average|Cantidad de directivas de clave de contenido creadas en la cuenta de servicio multimedia actual|None|
|ContentKeyPolicyQuotaUsedPercentage|Porcentaje utilizado de la cuota de directivas de clave de contenido|Percent|Average|Porcentaje de directivas de clave de contenido usado en la cuenta de servicio multimedia actual|None|
|StreamingPolicyQuota|Cuota de directivas de streaming|Count|Average|Cantidad de directivas de streaming permitidas para la cuenta de servicio multimedia actual|None|
|StreamingPolicyCount|Número de directivas de streaming|Count|Average|Cantidad de directivas de streaming creadas en la cuenta de servicio multimedia actual|None|
|StreamingPolicyQuotaUsedPercentage|Porcentaje usado de la cuota de directiva de streaming|Percent|Average|Porcentaje de directivas de streaming usado en la cuenta de servicio multimedia actual|None|


## <a name="microsoftmixedrealityremoterenderingaccounts"></a>Microsoft.MixedReality/remoteRenderingAccounts

|Métrica|Nombre de métrica para mostrar|Unidad|Tipo de agregación|Descripción|Dimensions|
|---|---|---|---|---|---|
|AssetsConverted|Activos convertidos|Count|Total|Número total de activos convertidos|AppId,ResourceId,SDKVersion|
|ActiveRenderingSessions|Sesiones de representación activa|Count|Total|Número total de sesiones de representación activas|AppId,ResourceId,SessionType,SDKVersion|

## <a name="microsoftnetappnetappaccountscapacitypoolsvolumes"></a>Microsoft.NetApp/netAppAccounts/capacityPools/volumes

|Métrica|Nombre de métrica para mostrar|Unidad|Tipo de agregación|Descripción|Dimensions|
|---|---|---|---|---|---|
|AverageReadLatency|Latencia de lectura media|MilliSeconds|Average|Latencia de lectura media en milisegundos por operación|None|
|AverageWriteLatency|Latencia de escritura media|MilliSeconds|Average|Latencia de escritura media en milisegundos por operación|None|
|VolumeLogicalSize|Tamaño consumido del volumen|Bytes|Average|Tamaño lógico del volumen (bytes usados)|None|
|VolumeSnapshotSize|Tamaño de instantánea de volumen|Bytes|Average|Tamaño de todas las instantáneas del volumen|None|
|ReadIops|IOPS de lectura|CountPerSecond|Average|Operaciones de entrada/salida de lectura por segundo|None|
|WriteIops|IOPS de escritura|CountPerSecond|Average|Operaciones de entrada/salida de escritura por segundo|None|

## <a name="microsoftnetappnetappaccountscapacitypools"></a>Microsoft.NetApp/netAppAccounts/capacityPools

|Métrica|Nombre de métrica para mostrar|Unidad|Tipo de agregación|Descripción|Dimensions|
|---|---|---|---|---|---|
|VolumePoolAllocatedUsed|Grupo asignado al tamaño del volumen|Bytes|Average|Tamaño usado asignado del grupo|None|
|VolumePoolTotalLogicalSize|Tamaño consumido del grupo|Bytes|Average|Suma del tamaño lógico de todos los volúmenes que pertenecen al grupo|None|

## <a name="microsoftnetworknetworkinterfaces"></a>Microsoft.Network/networkInterfaces

|Métrica|Nombre de métrica para mostrar|Unidad|Tipo de agregación|Descripción|Dimensions|
|---|---|---|---|---|---|
|BytesSentRate|Bytes enviados|Bytes|Total|Número de bytes de la interfaz de red enviados|None|
|BytesReceivedRate|Bytes recibidos|Bytes|Total|Número de bytes de la interfaz de red recibidos|None|
|PacketsSentRate|Paquetes enviados|Count|Total|Número de paquetes de la interfaz de red enviados|None|
|PacketsReceivedRate|Paquetes recibidos|Count|Total|Número de paquetes de la interfaz de red recibidos|None|

## <a name="microsoftnetworkloadbalancers"></a>Microsoft.Network/loadBalancers

|Métrica|Nombre de métrica para mostrar|Unidad|Tipo de agregación|Descripción|Dimensions|
|---|---|---|---|---|---|
|VipAvailability|Disponibilidad de la ruta de acceso de datos|Count|Average|Disponibilidad media de la ruta de acceso de datos de Load Balancer por duración|FrontendIPAddress,FrontendPort|
|DipAvailability|Estado del sondeo de mantenimiento|Count|Average|Estado del sondeo de mantenimiento medio de Load Balancer por duración|ProtocolType,BackendPort,FrontendIPAddress,FrontendPort,BackendIPAddress|
|ByteCount|Recuento de bytes|Count|Total|Número total de bytes transmitidos en un período|FrontendIPAddress,FrontendPort,Direction|
|PacketCount|Recuento de paquetes|Count|Total|Número total de paquetes transmitidos en un período|FrontendIPAddress,FrontendPort,Direction|
|SYNCount|Recuento de SYN|Count|Total|Número total de paquetes SYN transmitidos en un período|FrontendIPAddress,FrontendPort,Direction|
|SnatConnectionCount|Recuento de conexiones SNAT|Count|Total|Número total de conexiones SNAT nuevas creadas en un período|FrontendIPAddress,BackendIPAddress,ConnectionState|
|AllocatedSnatPorts|Puertos SNAT asignados (versión preliminar)|Count|Total|Número total de puertos SNAT asignados dentro del período de tiempo|FrontendIPAddress,BackendIPAddress,ProtocolType,IsAwaitingRemoval|
|UsedSnatPorts|Puertos SNAT usados (versión preliminar)|Count|Total|Número total de puertos SNAT usados dentro del período de tiempo|FrontendIPAddress,BackendIPAddress,ProtocolType,IsAwaitingRemoval|

## <a name="microsoftnetworkdnszones"></a>Microsoft.Network/dnszones

|Métrica|Nombre de métrica para mostrar|Unidad|Tipo de agregación|Descripción|Dimensions|
|---|---|---|---|---|---|
|QueryVolume|Volumen de consulta|Count|Total|Número de consultas que se sirven para una zona DNS|None|
|RecordSetCount|Recuento de conjunto de registros|Count|Máxima|Número de conjuntos de registros de una zona DNS|None|
|RecordSetCapacityUtilization|Uso de capacidad de conjunto de registros|Percent|Máxima|Porcentaje de capacidad de conjunto de registros que usa una zona DNS|None|


## <a name="microsoftnetworkpublicipaddresses"></a>Microsoft.Network/publicIPAddresses

|Métrica|Nombre de métrica para mostrar|Unidad|Tipo de agregación|Descripción|Dimensions|
|---|---|---|---|---|---|
|PacketsInDDoS|Paquetes entrantes de DDoS|CountPerSecond|Máxima|Paquetes entrantes de DDoS|None|
|PacketsDroppedDDoS|Paquetes entrantes de DDoS descartados|CountPerSecond|Máxima|Paquetes entrantes de DDoS descartados|None|
|PacketsForwardedDDoS|Paquetes entrantes de DDoS reenviados|CountPerSecond|Máxima|Paquetes entrantes de DDoS reenviados|None|
|TCPPacketsInDDoS|Paquetes TCP entrantes de DDoS|CountPerSecond|Máxima|Paquetes TCP entrantes de DDoS|None|
|TCPPacketsDroppedDDoS|Paquetes TCP entrantes de DDoS descartados|CountPerSecond|Máxima|Paquetes TCP entrantes de DDoS descartados|None|
|TCPPacketsForwardedDDoS|Paquetes TCP entrantes de DDoS reenviados|CountPerSecond|Máxima|Paquetes TCP entrantes de DDoS reenviados|None|
|UDPPacketsInDDoS|Paquetes UDP entrantes de DDoS|CountPerSecond|Máxima|Paquetes UDP entrantes de DDoS|None|
|UDPPacketsDroppedDDoS|Paquetes UDP entrantes de DDoS descartados|CountPerSecond|Máxima|Paquetes UDP entrantes de DDoS descartados|None|
|UDPPacketsForwardedDDoS|Paquetes UDP entrantes de DDoS reenviados|CountPerSecond|Máxima|Paquetes UDP entrantes de DDoS reenviados|None|
|BytesInDDoS|Bytes de entrada de DDoS|BytesPerSecond|Máxima|Bytes de entrada de DDoS|None|
|BytesDroppedDDoS|Bytes de entrada de DDoS descartados|BytesPerSecond|Máxima|Bytes de entrada de DDoS descartados|None|
|BytesForwardedDDoS|Bytes de entrada de DDoS reenviados|BytesPerSecond|Máxima|Bytes de entrada de DDoS reenviados|None|
|TCPBytesInDDoS|Bytes de entrada TCP de DDoS|BytesPerSecond|Máxima|Bytes de entrada TCP de DDoS|None|
|TCPBytesDroppedDDoS|Bytes de entrada TCP de DDoS descartados|BytesPerSecond|Máxima|Bytes de entrada TCP de DDoS descartados|None|
|TCPBytesForwardedDDoS|Bytes de entrada TCP de DDoS reenviados|BytesPerSecond|Máxima|Bytes de entrada TCP de DDoS reenviados|None|
|UDPBytesInDDoS|Bytes de entrada UDP de DDoS|BytesPerSecond|Máxima|Bytes de entrada UDP de DDoS|None|
|UDPBytesDroppedDDoS|Bytes de entrada UDP de DDoS descartados|BytesPerSecond|Máxima|Bytes de entrada UDP de DDoS descartados|None|
|UDPBytesDroppedDDoS|Bytes de entrada UDP de DDoS reenviados|BytesPerSecond|Máxima|Bytes de entrada UDP de DDoS reenviados|None|
|IfUnderDDoSAttack|Bajo ataque de DDoS o no|Count|Máxima|Bajo ataque de DDoS o no|None|
|DDoSTriggerTCPPackets|Paquetes entrantes TCP para desencadenar la mitigación de DDoS|CountPerSecond|Máxima|Paquetes entrantes TCP para desencadenar la mitigación de DDoS|None|
|DDoSTriggerUDPPackets|Paquetes entrantes UDP para desencadenar la mitigación de DDoS|CountPerSecond|Máxima|Paquetes entrantes UDP para desencadenar la mitigación de DDoS|None|
|DDoSTriggerSYNPackets|Paquetes entrantes SYN para desencadenar la mitigación de DDoS|CountPerSecond|Máxima|Paquetes entrantes SYN para desencadenar la mitigación de DDoS|None|
|VipAvailability|Disponibilidad de la ruta de acceso de datos|Count|Average|Disponibilidad media de la dirección IP por duración|Port|
|ByteCount|Recuento de bytes|Count|Total|Número total de bytes transmitidos en un período|Port,Direction|
|PacketCount|Recuento de paquetes|Count|Total|Número total de paquetes transmitidos en un período|Port,Direction|
|SynCount|Recuento de SYN|Count|Total|Número total de paquetes SYN transmitidos en un período|Port,Direction|



## <a name="microsoftnetworkvirtualnetworks"></a>Microsoft.Network/virtualNetworks

|Métrica|Nombre de métrica para mostrar|Unidad|Tipo de agregación|Descripción|Dimensions|
|---|---|---|---|---|---|
|PingMeshAverageRoundtripMs|Round trip time for Pings to a VM [Tiempo de recorrido de ida y vuelta de los pings a una VM]|MilliSeconds|Average|Tiempo de ida y vuelta de los pings enviados a una VM de destino|SourceCustomerAddress,DestinationCustomerAddress|
|PingMeshProbesFailedPercent|Failed Pings to a VM (Pings a una VM erróneos)|Percent|Average|Porcentaje de pings erróneos del total de pings enviados de una VM de destino|SourceCustomerAddress,DestinationCustomerAddress|


## <a name="microsoftnetworkazurefirewalls"></a>Microsoft.Network/azurefirewalls

|Métrica|Nombre de métrica para mostrar|Unidad|Tipo de agregación|Descripción|Dimensions|
|---|---|---|---|---|---|
|ApplicationRuleHit|Recuento de llamadas de las reglas de aplicación|Count|Total|Número de veces que se llamó a las reglas de aplicación|Status,Reason,Protocol|
|NetworkRuleHit|Número de llamadas de reglas de red|Count|Total|Número de veces que se llamó a las reglas de red|Status,Reason,Protocol|
|FirewallHealth|Firewall health state (Estado de mantenimiento del firewall)|Percent|Average|Estado de mantenimiento del firewall|Status,Reason|
|DataProcessed|Datos procesados|Bytes|Total|Cantidad total de datos procesados por el firewall|None|
|SNATPortUtilization|SNAT port utilization (Uso de puertos SNAT)|Percent|Average|Uso de puertos SNAT|None|


## <a name="microsoftnetworkapplicationgateways"></a>Microsoft.Network/applicationGateways

|Métrica|Nombre de métrica para mostrar|Unidad|Tipo de agregación|Descripción|Dimensions|
|---|---|---|---|---|---|
|Throughput|Throughput|BytesPerSecond|Average|Número de bytes por segundo que ha ofrecido Application Gateway|None|
|UnhealthyHostCount|Recuento de hosts con estado incorrecto|Count|Average|Número de hosts de back-end con estado incorrecto|BackendSettingsPool|
|HealthyHostCount|Recuento de hosts con estado correcto|Count|Average|Número de hosts de back-end con estado correcto|BackendSettingsPool|
|TotalRequests|Total de solicitudes|Count|Total|Recuento de solicitudes correctas que ha servido Application Gateway|BackendSettingsPool|
|AvgRequestCountPerHealthyHost|Requests per minute per Healthy Host (Solicitudes por minuto y host con estado correcto)|Count|Average|Recuento medio de solicitudes por minuto por host de back-end con estado correcto de un grupo|BackendSettingsPool|
|FailedRequests|Solicitudes con error|Count|Total|Recuento de solicitudes con error que ha servido Application Gateway|BackendSettingsPool|
|ResponseStatus|Estado de respuesta|Count|Total|Estado de respuesta HTTP devuelto por Application Gateway|HttpStatusGroup|
|CurrentConnections|Conexiones actuales|Count|Total|Recuento de conexiones actuales establecidas con Application Gateway|None|
|NewConnectionsPerSecond|Nuevas conexiones por segundo|CountPerSecond|Average|Nuevas conexiones por segundo establecidas con Application Gateway|None|
|CpuUtilization|Uso de CPU|Percent|Average|Uso actual de la CPU de Application Gateway|None|
|CapacityUnits|Unidades de capacidad actuales|Count|Average|Unidades de capacidad consumidas|None|
|FixedBillableCapacityUnits|Unidades de capacidad facturables fijas|Count|Average|Unidades de capacidad mínima que se cobrarán|None|
|EstimatedBilledCapacityUnits|Unidades de capacidad facturadas estimadas|Count|Average|Unidades de capacidad estimadas que se cobrarán|None|
|ComputeUnits|Current Compute Units (Unidades de proceso actuales)|Count|Average|Unidades de proceso consumidas|None|
|BackendResponseStatus|Backend Response Status (Estado de respuesta de back-end)|Count|Total|Número de códigos de respuesta HTTP generados por los miembros de back-end No se incluyen los códigos de respuesta generados por Application Gateway.|BackendServer,BackendPool,BackendHttpSetting,HttpStatusGroup|
|TlsProtocol|Client TLS Protocol (Protocolo TLS de cliente)|Count|Total|Número de solicitudes TLS y no TLS iniciadas por el cliente que establecieron la conexión con Application Gateway. Para ver la distribución del protocolo TLS, filtre por el protocolo TLS de la dimensión.|Listener,TlsProtocol|
|BytesSent|Bytes enviados|Bytes|Total|Número total de bytes enviados por Application Gateway a los clientes|Agente de escucha|
|BytesReceived|Bytes recibidos|Bytes|Total|Número total de bytes recibidos por Application Gateway de los clientes|Agente de escucha|
|ClientRtt|Client RTT (RTT del cliente)|MilliSeconds|Average|Tiempo medio de ida y vuelta entre clientes y Application Gateway. Esta métrica indica cuánto tiempo se tarda en establecer conexiones y devolver confirmaciones.|Agente de escucha|
|ApplicationGatewayTotalTime|Application Gateway Total Time (Tiempo total de Application Gateway)|MilliSeconds|Average|Promedio de tiempo que se tarda en procesar una solicitud y en enviar la respuesta. Esto se calcula como promedio del intervalo desde el momento en que Application Gateway recibe el primer byte de una solicitud HTTP hasta el momento en que termina la operación de envío de la respuesta. Es importante tener en cuenta que esto normalmente incluye el tiempo de procesamiento de Application Gateway, el tiempo que los paquetes de solicitud y respuesta viajan por la red y el tiempo que el servidor back-end tardó en responder.|Agente de escucha|
|BackendConnectTime|Backend Connect Time (Tiempo de conexión de back-end)|MilliSeconds|Average|Tiempo empleado en establecer una conexión con un servidor back-end.|Listener,BackendServer,BackendPool,BackendHttpSetting|
|BackendFirstByteResponseTime|Backend First Byte Response Time (Tiempo de respuesta del primer byte de back-end)|MilliSeconds|Average|Intervalo de tiempo entre el inicio del establecimiento de una conexión con el servidor back-end y la recepción del primer byte del encabezado de respuesta, el tiempo de procesamiento aproximado del servidor back-end|Listener,BackendServer,BackendPool,BackendHttpSetting|
|BackendLastByteResponseTime|Backend Last Byte Response Time (Tiempo de respuesta del último byte de back-end)|MilliSeconds|Average|Intervalo de tiempo entre el inicio del establecimiento de una conexión con el servidor back-end y la recepción del último byte del cuerpo de la respuesta|Listener,BackendServer,BackendPool,BackendHttpSetting|
|MatchedCount|Distribución de reglas totales del firewall de aplicaciones web v1|Count|Total|Distribución de reglas totales del firewall de aplicaciones web v1 para el tráfico entrante|RuleGroup,RuleId|
|BlockedCount|Distribución de reglas de solicitudes bloqueadas del firewall de aplicaciones web v1|Count|Total|Distribución de reglas de solicitudes bloqueadas del firewall de aplicaciones web v1|RuleGroup,RuleId|
|BlockedReqCount|Recuento de solicitudes bloqueadas del firewall de aplicaciones web v1|Count|Total|Recuento de solicitudes bloqueadas del firewall de aplicaciones web v1|None|


## <a name="microsoftnetworkvirtualnetworkgateways"></a>Microsoft.Network/virtualNetworkGateways

|Métrica|Nombre de métrica para mostrar|Unidad|Tipo de agregación|Descripción|Dimensions|
|---|---|---|---|---|---|
|AverageBandwidth|Ancho de banda S2S de puerta de enlace|BytesPerSecond|Average|Ancho de banda de sitio a sitio medio de una puerta de enlace en bytes por segundo|None|
|P2SBandwidth|Ancho de banda P2S de puerta de enlace|BytesPerSecond|Average|Ancho de banda de punto a sitio medio de una puerta de enlace en bytes por segundo|None|
|P2SConnectionCount|Recuento de conexiones P2S|Count|Máxima|Recuento de conexiones de punto a sitio de una puerta de enlace|Protocolo|
|TunnelAverageBandwidth|Ancho de banda de túnel|BytesPerSecond|Average|Media de ancho de banda de un túnel en bytes por segundo|ConnectionName,RemoteIP|
|TunnelEgressBytes|Bytes de salida de túnel|Bytes|Total|Bytes de salida de un túnel|ConnectionName,RemoteIP|
|TunnelIngressBytes|Bytes de salida de túnel|Bytes|Total|Bytes de entrada de un túnel|ConnectionName,RemoteIP|
|TunnelEgressPackets|Paquetes de salida de túnel|Count|Total|Recuento de paquetes salientes de un túnel|ConnectionName,RemoteIP|
|TunnelIngressPackets|Paquetes de entrada de túnel|Count|Total|Recuento de paquetes entrantes de un túnel|ConnectionName,RemoteIP|
|TunnelEgressPacketDropTSMismatch|Colocación de paquetes con error de coincidencia del selector de tráfico de túnel de salida|Count|Total|Recuento de paquetes de salida con error de coincidencia del selector de tráfico de un túnel|ConnectionName,RemoteIP|
|TunnelIngressPacketDropTSMismatch|Colocación de paquetes con error de coincidencia del selector de tráfico de túnel de entrada|Count|Total|Recuento de paquetes entrantes con error de coincidencia del selector de tráfico de un túnel|ConnectionName,RemoteIP|


## <a name="microsoftnetworkexpressrouteports"></a>Microsoft.Network/expressRoutePorts

|Métrica|Nombre de métrica para mostrar|Unidad|Tipo de agregación|Descripción|Dimensions|
|---|---|---|---|---|---|
|RxLightLevel|RxLightLevel|Count|Average|Nivel de iluminación de recepción en dBm|Link,Lane|
|TxLightLevel|TxLightLevel|Count|Average|Nivel de iluminación de transmisión en dBm|Link,Lane|
|AdminState|AdminState|Count|Average|Estado de administración del puerto|Vínculo|
|LineProtocol|LineProtocol|Count|Average|Estado del protocolo de línea del puerto|Vínculo|
|PortBitsInPerSecond|BitsInPerSecond|CountPerSecond|Average|Bits que ingresan a Azure por segundo|Vínculo|
|PortBitsOutPerSecond|BitsOutPerSecond|CountPerSecond|Average|Bits que egresan de Azure por segundo|Vínculo|



## <a name="microsoftnetworkexpressroutecircuits"></a>Microsoft.Network/expressRouteCircuits

|Métrica|Nombre de métrica para mostrar|Unidad|Tipo de agregación|Descripción|Dimensions|
|---|---|---|---|---|---|
|BitsInPerSecond|BitsInPerSecond|CountPerSecond|Average|Bits que ingresan a Azure por segundo|PeeringType|
|BitsOutPerSecond|BitsOutPerSecond|CountPerSecond|Average|Bits que egresan de Azure por segundo|PeeringType|
|GlobalReachBitsInPerSecond|GlobalReachBitsInPerSecond|CountPerSecond|Average|Bits que ingresan a Azure por segundo|PeeredCircuitSKey|
|GlobalReachBitsOutPerSecond|GlobalReachBitsOutPerSecond|CountPerSecond|Average|Bits que egresan de Azure por segundo|PeeredCircuitSKey|
|BgpAvailability|Disponibilidad de BGP|Percent|Average|Disponibilidad de BGP de MSEE a todos los elementos del mismo nivel.|PeeringType,Peer|
|ArpAvailability|Disponibilidad de ARP|Percent|Average|Disponibilidad de ARP de MSEE a todos los elementos del mismo nivel.|PeeringType,Peer|
|QosDropBitsInPerSecond|DroppedInBitsPerSecond|CountPerSecond|Average|Bits de entrada de datos descartados por segundo|None|
|QosDropBitsOutPerSecond|DroppedOutBitsPerSecond|CountPerSecond|Average|Bits de salida de datos descartados por segundo|None|

## <a name="microsoftnetworkexpressroutecircuitspeerings"></a>Microsoft.Network/expressRouteCircuits/peerings

|Métrica|Nombre de métrica para mostrar|Unidad|Tipo de agregación|Descripción|Dimensions|
|---|---|---|---|---|---|
|BitsInPerSecond|BitsInPerSecond|CountPerSecond|Average|Bits que ingresan a Azure por segundo|None|
|BitsOutPerSecond|BitsOutPerSecond|CountPerSecond|Average|Bits que egresan de Azure por segundo|None|

## <a name="microsoftnetworkconnections"></a>Microsoft.Network/connections

|Métrica|Nombre de métrica para mostrar|Unidad|Tipo de agregación|Descripción|Dimensions|
|---|---|---|---|---|---|
|BitsInPerSecond|BitsInPerSecond|CountPerSecond|Average|Bits que ingresan a Azure por segundo|None|
|BitsOutPerSecond|BitsOutPerSecond|CountPerSecond|Average|Bits que egresan de Azure por segundo|None|

## <a name="microsoftnetworkexpressroutegateways"></a>Microsoft.Network/expressRouteGateways

|Métrica|Nombre de métrica para mostrar|Unidad|Tipo de agregación|Descripción|Dimensions|
|---|---|---|---|---|---|
|ErGatewayConnectionBitsInPerSecond|BitsInPerSecond|CountPerSecond|Average|Bits que ingresan a Azure por segundo|ConnectionName|
|ErGatewayConnectionBitsOutPerSecond|BitsOutPerSecond|CountPerSecond|Average|Bits que egresan de Azure por segundo|ConnectionName|

## <a name="microsoftnetworktrafficmanagerprofiles"></a>Microsoft.Network/trafficManagerProfiles

|Métrica|Nombre de métrica para mostrar|Unidad|Tipo de agregación|Descripción|Dimensions|
|---|---|---|---|---|---|
|QpsByEndpoint|Consultas por punto de conexión devueltas|Count|Total|Número de veces que se devolvió un punto de conexión de Traffic Manager en el período dado|EndpointName|
|ProbeAgentCurrentEndpointStateByProfileResourceId|Estado de punto de conexión por punto de conexión|Count|Máxima|1 si el estado de sondeo de un punto de conexión es “Habilitado”, 0 en caso contrario.|EndpointName|



## <a name="microsoftnetworknetworkwatchersconnectionmonitors"></a>Microsoft.Network/networkWatchers/connectionMonitors

|Métrica|Nombre de métrica para mostrar|Unidad|Tipo de agregación|Descripción|Dimensions|
|---|---|---|---|---|---|
|ProbesFailedPercent|% de sondeos con error|Percent|Average|% de sondeos de supervisión de conectividad con error|None|
|AverageRoundtripMs|Prom. Tiempo de ida y vuelta (ms)|MilliSeconds|Average|Promedio de tiempo de ida y vuelta de red (ms) para los sondeos de supervisión de conectividad que se envían entre el origen y el destino|None|
|ChecksFailedPercent|Checks Failed Percent (Preview) [Porcentaje de comprobaciones erróneas (versión preliminar)]|Percent|Average|% de comprobaciones de supervisión de conectividad con error|SourceAddress,SourceName,SourceResourceId,SourceType,Protocol,DestinationAddress,DestinationName,DestinationResourceId,DestinationType,DestinationPort,TestGroupName,TestConfigurationName|
|RoundTripTimeMs|Round-Trip Time (ms) (Preview) [Tiempo de ida y vuelta (ms) (versión preliminar)]|MilliSeconds|Average|Tiempo de ida y vuelta en milisegundos para las comprobaciones de supervisión de conectividad|SourceAddress,SourceName,SourceResourceId,SourceType,Protocol,DestinationAddress,DestinationName,DestinationResourceId,DestinationType,DestinationPort,TestGroupName,TestConfigurationName|


## <a name="microsoftnetworkfrontdoors"></a>Microsoft.Network/frontdoors

|Métrica|Nombre de métrica para mostrar|Unidad|Tipo de agregación|Descripción|Dimensions|
|---|---|---|---|---|---|
|RequestCount|Recuento de solicitudes|Count|Total|Número de solicitudes de cliente que atiende el proxy HTTP/S|HttpStatus,HttpStatusGroup,ClientRegion,ClientCountry|
|RequestSize|Tamaño de la solicitud|Bytes|Total|Número de bytes enviados como solicitudes de clientes al proxy HTTP/S|HttpStatus,HttpStatusGroup,ClientRegion,ClientCountry|
|ResponseSize|Tamaño de la respuesta|Bytes|Total|Número de bytes enviados como respuestas del proxy HTTP/S a los clientes|HttpStatus,HttpStatusGroup,ClientRegion,ClientCountry|
|BillableResponseSize|Billable Response Size (Tamaño de respuesta facturable)|Bytes|Total|Número de bytes facturables (mínimo de 2 KB por solicitud) enviados como respuestas del proxy HTTP/S a los clientes.|HttpStatus,HttpStatusGroup,ClientRegion,ClientCountry|
|BackendRequestCount|Recuento de solicitudes de back-end|Count|Total|Número de solicitudes enviadas del proxy HTTP/S a los back-end|HttpStatus,HttpStatusGroup,Backend|
|BackendRequestLatency|Latencia de las solicitudes de back-end|MilliSeconds|Average|Tiempo calculado desde que el proxy HTTP/S ha enviado la solicitud al back-end hasta que el proxy HTTP/S ha recibido el último byte de respuesta del back-end|Back-end|
|TotalLatency|Latencia total|MilliSeconds|Average|Tiempo calculado desde que el proxy HTTP/S ha recibido la solicitud de cliente hasta que el cliente ha reconocido el último byte de respuesta del proxy HTTP/S|HttpStatus,HttpStatusGroup,ClientRegion,ClientCountry|
|BackendHealthPercentage|Porcentaje de estado del back-end|Percent|Average|El porcentaje de sondeos de estado correctos del proxy HTTP/S a los back-ends|Backend,BackendPool|
|WebApplicationFirewallRequestCount|Recuento de solicitudes del firewall de aplicaciones web|Count|Total|Número de solicitudes de cliente procesadas por el firewall de aplicaciones web|PolicyName,RuleName,Action|


## <a name="microsoftnetworkprivatednszones"></a>Microsoft.Network/privateDnsZones

|Métrica|Nombre de métrica para mostrar|Unidad|Tipo de agregación|Descripción|Dimensions|
|---|---|---|---|---|---|
|QueryVolume|Volumen de consulta|Count|Total|Número de consultas que se sirven para una zona de DNS privado|None|
|RecordSetCount|Recuento de conjunto de registros|Count|Máxima|Número de conjuntos de registros de una zona de DNS privado|None|
|RecordSetCapacityUtilization|Uso de capacidad de conjunto de registros|Percent|Máxima|Porcentaje de capacidad de conjunto de registros que usa una zona de DNS privado|None|
|VirtualNetworkLinkCount|Recuento de vínculos de red virtual|Count|Máxima|Número de redes virtuales vinculadas a una zona de DNS privado|None|
|VirtualNetworkLinkCapacityUtilization|Uso de la capacidad del vínculo de red virtual|Percent|Máxima|Porcentaje de capacidad del vínculo de red virtual que usa una zona de DNS privado|None|
|VirtualNetworkWithRegistrationLinkCount|Recuento de vínculo de registro de red virtual|Count|Máxima|Número de redes virtuales vinculadas a una zona de DNS privado con el registro automático habilitado|None|
|VirtualNetworkWithRegistrationCapacityUtilization|Uso de la capacidad del vínculo de registro de la red virtual|Percent|Máxima|Porcentaje de capacidad del vínculo de red virtual con registro automático que usa una zona de DNS privado|None|

## <a name="microsoftnotificationhubsnamespacesnotificationhubs"></a>Microsoft.NotificationHubs/Namespaces/NotificationHubs

|Métrica|Nombre de métrica para mostrar|Unidad|Tipo de agregación|Descripción|Dimensions|
|---|---|---|---|---|---|
|registration.all|Operaciones de registro|Count|Total|El recuento de todas las operaciones de registro correctas (creaciones, actualizaciones, consultas y eliminaciones). |None|
|registration.create|Operaciones de creación de registros|Count|Total|El recuento de todas las creaciones de registros correctas.|None|
|registration.update|Operaciones de actualización de registros|Count|Total|El recuento de todas las actualizaciones de registros correctas.|None|
|registration.get|Operaciones de lectura de registros|Count|Total|El recuento de todas las consultas de registros correctas.|None|
|registration.delete|Operaciones de eliminación de registros|Count|Total|El recuento de todas las eliminaciones de registros correctas.|None|
|incoming|Mensajes entrantes|Count|Total|El recuento de todas las llamadas a la API de envío. |None|
|incoming.scheduled|Notificaciones push programadas enviadas|Count|Total|Notificaciones push programadas canceladas|None|
|incoming.scheduled.cancel|Notificaciones push programadas canceladas|Count|Total|Notificaciones push programadas canceladas|None|
|scheduled.pending|Notificaciones programadas pendientes|Count|Total|Notificaciones programadas pendientes|None|
|installation.all|Operaciones de administración de instalaciones|Count|Total|Operaciones de administración de instalaciones|None|
|installation.get|Obtener operaciones de instalación|Count|Total|Obtener operaciones de instalación|None|
|installation.upsert|Crear o actualizar operaciones de instalación|Count|Total|Crear o actualizar operaciones de instalación|None|
|installation.patch|Aplicar revisión a operaciones de instalación|Count|Total|Aplicar revisión a operaciones de instalación|None|
|installation.delete|Eliminar operaciones de instalación|Count|Total|Eliminar operaciones de instalación|None|
|outgoing.allpns.success|Notificaciones correctas|Count|Total|El recuento de todas las notificaciones correctas.|None|
|outgoing.allpns.invalidpayload|Errores de carga útil|Count|Total|El recuento de las inserciones que provocaron un error porque el PNS devolvió un error de carga útil incorrecta.|None|
|outgoing.allpns.pnserror|Errores del sistema de notificación externo|Count|Total|El recuento de inserciones que provocaron un error porque se produjo un problema al comunicarse con el PNS (excluye los problemas de autenticación).|None|
|outgoing.allpns.channelerror|Errores de canal|Count|Total|El recuento de inserciones que provocaron un error porque el canal no era válido no está asociado a la aplicación correcta limitada o expirada.|None|
|outgoing.allpns.badorexpiredchannel|Errores de canal incorrecto o expirado|Count|Total|El recuento de inserciones que provocaron un error porque el canal/token/registrationId del registro ha expirado o no es válido.|None|
|outgoing.wns.success|Notificaciones de WNS correctas|Count|Total|El recuento de todas las notificaciones correctas.|None|
|outgoing.wns.invalidcredentials|Errores de autorización de WNS (credenciales no válidas)|Count|Total|El recuento de inserciones que provocaron un error porque el PNS no aceptó las credenciales proporcionadas o las credenciales están bloqueadas. (Windows Live no reconoce las credenciales).|None|
|outgoing.wns.badchannel|Error de canal incorrecto de WNS|Count|Total|El recuento de inserciones que provocaron un error porque no se reconoció ChannelURI en el registro (estado de WNS: 404 No encontrado).|None|
|outgoing.wns.expiredchannel|Error de canal expirado de WNS|Count|Total|El recuento de inserciones que provocaron un error porque ChannelURI expiró (estado de WNS: 410 Ya no existe).|None|
|outgoing.wns.throttled|Notificaciones limitadas de WNS|Count|Total|El recuento de inserciones que provocaron un error porque WNS limita esta aplicación (estado de WNS: 406 No aceptable).|None|
|outgoing.wns.tokenproviderunreachable|Errores de autorización de WNS (inaccesible)|Count|Total|No se puede acceder a Windows Live.|None|
|outgoing.wns.invalidtoken|Errores de autorización de WNS (token no válido)|Count|Total|El token que se proporciona a WNS no es válido (estado de WNS: 401 No autorizado).|None|
|outgoing.wns.wrongtoken|Errores de autorización de WNS (token erróneo)|Count|Total|El token que se proporciona a WNS es válido, pero para otra aplicación (estado de WNS: 403 Prohibido). Esto puede ocurrir si la instancia de ChannelURI en el registro está asociada a otra aplicación. Compruebe que la aplicación cliente está asociada a la misma aplicación cuyas credenciales están en el centro de notificaciones.|None|
|outgoing.wns.invalidnotificationformat|Formato de notificación no válido de WNS|Count|Total|El formato de la notificación no es válido (estado de WNS: 400). Tenga en cuenta que WNS no rechaza todas las cargas útiles no válidas.|None|
|outgoing.wns.invalidnotificationsize|Error de tamaño de notificación no válido de WNS|Count|Total|La carga útil de la notificación es demasiado grande (estado de WNS: 413).|None|
|outgoing.wns.channelthrottled|Canal de WNS limitado|Count|Total|La notificación se descartó porque la instancia de ChannelURI del registro está limitada (encabezado de respuesta WNS: X-WNS-NotificationStatus:channelThrottled).|None|
|outgoing.wns.channeldisconnected|Canal de WNS desconectado|Count|Total|La notificación se descartó porque la instancia de ChannelURI del registro está limitada (encabezado de respuesta WNS: X-WNS-DeviceConnectionStatus: desconectado).|None|
|outgoing.wns.dropped|Notificaciones descartadas de WNS|Count|Total|La notificación se descartó porque la instancia de ChannelURI del registro está limitada (X-WNS-NotificationStatus: descartado pero no X-WNS-DeviceConnectionStatus: desconectado).|None|
|outgoing.wns.pnserror|Errores de WNS|Count|Total|La notificación no se entregó debido a errores de comunicación con WNS.|None|
|outgoing.wns.authenticationerror|Errores de autenticación de WNS|Count|Total|La notificación no se entregó debido a errores de comunicación con Windows Live, credenciales no válidas o token erróneo.|None|
|outgoing.apns.success|Notificaciones de APNS correctas|Count|Total|El recuento de todas las notificaciones correctas.|None|
|outgoing.apns.invalidcredentials|Errores de autorización de APNS|Count|Total|El recuento de inserciones que provocaron un error porque el PNS no aceptó las credenciales proporcionadas o las credenciales están bloqueadas.|None|
|outgoing.apns.badchannel|Error de canal incorrecto de APNS|Count|Total|El recuento de inserciones que provocaron un error porque el token no es válido (código de estado de APNS: 8).|None|
|outgoing.apns.expiredchannel|Error de canal expirado de APNS|Count|Total|El recuento de tokens invalidados por el canal de comentarios de APNS.|None|
|outgoing.apns.invalidnotificationsize|Error de tamaño de notificación no válido de APNS|Count|Total|El recuento de inserciones que provocaron un error porque la carga útil era demasiado grande (código de estado de APNS: 7).|None|
|outgoing.apns.pnserror|Errores de APNS|Count|Total|El recuento de inserciones que provocaron un error debido a errores de comunicación con APNS.|None|
|outgoing.gcm.success|Notificaciones de GCM correctas|Count|Total|El recuento de todas las notificaciones correctas.|None|
|outgoing.gcm.invalidcredentials|Errores de autorización de GCM (credenciales no válidas)|Count|Total|El recuento de inserciones que provocaron un error porque el PNS no aceptó las credenciales proporcionadas o las credenciales están bloqueadas.|None|
|outgoing.gcm.badchannel|Error de canal incorrecto de GCM|Count|Total|El recuento de inserciones que provocaron un error porque la instancia de registrationId del registro no se ha reconocido (resultado de GCM: registro no válido).|None|
|outgoing.gcm.expiredchannel|Error de canal expirado de GCM|Count|Total|El recuento de inserciones que provocaron un error porque la instancia de registrationId del registro ha caducado (resultado de GCM: NotRegistered).|None|
|outgoing.gcm.throttled|Notificaciones limitadas de GCM|Count|Total|El recuento de inserciones que provocaron un error porque GCM limitó esta aplicación (código de estado de GCM: 501-599 o resultado: no disponible).|None|
|outgoing.gcm.invalidnotificationformat|Formato de notificación no válido de GCM|Count|Total|El recuento de inserciones que provocaron un error porque la carga útil no tenía el formato correcto (resultado de GCM: InvalidDataKey o InvalidTtl).|None|
|outgoing.gcm.invalidnotificationsize|Error de tamaño de notificación no válido de GCM|Count|Total|El recuento de inserciones que provocaron un error porque la carga útil era demasiado grande (resultado de GCM: MessageTooBig).|None|
|outgoing.gcm.wrongchannel|Error de canal incorrecto de GCM|Count|Total|El recuento de inserciones que provocaron un error porque la instancia de registrationId del registro no está asociado a la aplicación actual (resultado de GCM: InvalidPackageName).|None|
|outgoing.gcm.pnserror|Errores de GCM|Count|Total|El recuento de inserciones que provocaron un error debido a errores de comunicación con GCM.|None|
|outgoing.gcm.authenticationerror|Errores de autenticación de GCM|Count|Total|El recuento de inserciones que provocaron un error debido a que el PNS no aceptó las credenciales proporcionadas, las credenciales están bloqueadas o el Id. del remitente no está configurado correctamente en la aplicación (resultado de GCM: MismatchedSenderId).|None|
|outgoing.mpns.success|Notificaciones de MPNS correctas|Count|Total|El recuento de todas las notificaciones correctas.|None|
|outgoing.mpns.invalidcredentials|Credenciales no válidas de MPNS|Count|Total|El recuento de inserciones que provocaron un error porque el PNS no aceptó las credenciales proporcionadas o las credenciales están bloqueadas.|None|
|outgoing.mpns.badchannel|Error de canal incorrecto de MPNS|Count|Total|El recuento de inserciones que provocaron un error porque no se reconoció ChannelURI en el registro (estado de MPNS: 404 No encontrado).|None|
|outgoing.mpns.throttled|Notificaciones limitadas de MPNS|Count|Total|El recuento de inserciones que provocaron un error porque MPNS limita esta aplicación (MPNS de WNS: 406 No aceptable).|None|
|outgoing.mpns.invalidnotificationformat|Formato de notificación no válido de MPNS|Count|Total|El recuento de inserciones que provocaron un error porque la carga útil de la notificación era demasiado grande.|None|
|outgoing.mpns.channeldisconnected|Canal de MPNS desconectado|Count|Total|El recuento de inserciones que provocaron un error porque se desconectó la instancia ChannelURI en el registro (estado de MPNS: 412 No encontrado).|None|
|outgoing.mpns.dropped|Notificaciones descartadas de MPNS|Count|Total|El recuento de inserciones que eliminó MPNS (encabezado de respuesta MPNS: X-NotificationStatus: QueueFull o Suprimido).|None|
|outgoing.mpns.pnserror|Errores de MPNS|Count|Total|El recuento de inserciones que provocaron un error debido a errores de comunicación con MPNS.|None|
|outgoing.mpns.authenticationerror|Errores de autenticación de MPNS|Count|Total|El recuento de inserciones que provocaron un error porque el PNS no aceptó las credenciales proporcionadas o las credenciales están bloqueadas.|None|
|notificationhub.pushes|Todas las notificaciones salientes|Count|Total|Todas las notificaciones salientes del Centro de notificaciones|None|
|incoming.all.requests|Todas las solicitudes entrantes|Count|Total|Solicitudes entrantes totales de un centro de notificaciones|None|
|incoming.all.failedrequests|Todas las solicitudes entrantes con error|Count|Total|Solicitudes entrantes totales con error de un centro de notificaciones|None|

## <a name="microsoftoperationalinsightsworkspaces"></a>Microsoft.OperationalInsights/workspaces

|Métrica|Nombre de métrica para mostrar|Unidad|Tipo de agregación|Descripción|Dimensions|
|---|---|---|---|---|---|
|Average_% Free Inodes|% de Inodes libres|Count|Average|Average_% Free Inodes|Computer,ObjectName,InstanceName,CounterPath,SourceSystem|
|Average_% Free Space|% de espacio libre|Count|Average|Average_% Free Space|Computer,ObjectName,InstanceName,CounterPath,SourceSystem|
|Average_% Used Inodes|% de Inodes usados|Count|Average|Average_% Used Inodes|Computer,ObjectName,InstanceName,CounterPath,SourceSystem|
|Average_% Used Space|% espacio usado|Count|Average|Average_% Used Space|Computer,ObjectName,InstanceName,CounterPath,SourceSystem|
|Average_Disk Read Bytes/sec|Bytes de lectura de disco/s|Count|Average|Average_Disk Read Bytes/sec|Computer,ObjectName,InstanceName,CounterPath,SourceSystem|
|Average_Disk Reads/sec|Lecturas de disco/s|Count|Average|Average_Disk Reads/sec|Computer,ObjectName,InstanceName,CounterPath,SourceSystem|
|Average_Disk Transfers/sec|Transferencias de disco/s|Count|Average|Average_Disk Transfers/sec|Computer,ObjectName,InstanceName,CounterPath,SourceSystem|
|Average_Disk Write Bytes/sec|Bytes de escritura en disco/s|Count|Average|Average_Disk Write Bytes/sec|Computer,ObjectName,InstanceName,CounterPath,SourceSystem|
|Average_Disk Writes/sec|Escrituras en disco/s|Count|Average|Average_Disk Writes/sec|Computer,ObjectName,InstanceName,CounterPath,SourceSystem|
|Average_Free Megabytes|Megabytes libres|Count|Average|Average_Free Megabytes|Computer,ObjectName,InstanceName,CounterPath,SourceSystem|
|Average_Logical Disk Bytes/sec|Bytes de disco lógico/s|Count|Average|Average_Logical Disk Bytes/sec|Computer,ObjectName,InstanceName,CounterPath,SourceSystem|
|Average_% Available Memory|% de memoria disponible|Count|Average|Average_% Available Memory|Computer,ObjectName,InstanceName,CounterPath,SourceSystem|
|Average_% Available Swap Space|% de espacio de intercambio disponible|Count|Average|Average_% Available Swap Space|Computer,ObjectName,InstanceName,CounterPath,SourceSystem|
|Average_% Used Memory|% de memoria usada|Count|Average|Average_% Used Memory|Computer,ObjectName,InstanceName,CounterPath,SourceSystem|
|Average_% Used Swap Space|% de espacio de intercambio usado|Count|Average|Average_% Used Swap Space|Computer,ObjectName,InstanceName,CounterPath,SourceSystem|
|Average_Available MBytes Memory|MB de memoria disponibles|Count|Average|Average_Available MBytes Memory|Computer,ObjectName,InstanceName,CounterPath,SourceSystem|
|Average_Available MBytes Swap|Intercambio de MB disponibles|Count|Average|Average_Available MBytes Swap|Computer,ObjectName,InstanceName,CounterPath,SourceSystem|
|Average_Page Reads/sec|Lecturas de página/s|Count|Average|Average_Page Reads/sec|Computer,ObjectName,InstanceName,CounterPath,SourceSystem|
|Average_Page Writes/sec|Escrituras de página/s|Count|Average|Average_Page Writes/sec|Computer,ObjectName,InstanceName,CounterPath,SourceSystem|
|Average_Pages/sec|Páginas/s|Count|Average|Average_Pages/sec|Computer,ObjectName,InstanceName,CounterPath,SourceSystem|
|Average_Used MBytes Swap Space|Espacio de intercambio de MB usado|Count|Average|Average_Used MBytes Swap Space|Computer,ObjectName,InstanceName,CounterPath,SourceSystem|
|Average_Used Memory MBytes|MB de memoria usados|Count|Average|Average_Used Memory MBytes|Computer,ObjectName,InstanceName,CounterPath,SourceSystem|
|Average_Total Bytes Transmitted|Número total de bytes transmitidos|Count|Average|Average_Total Bytes Transmitted|Computer,ObjectName,InstanceName,CounterPath,SourceSystem|
|Average_Total Bytes Received|Número total de bytes recibidos|Count|Average|Average_Total Bytes Received|Computer,ObjectName,InstanceName,CounterPath,SourceSystem|
|Average_Total Bytes|Número total de bytes|Count|Average|Average_Total Bytes|Computer,ObjectName,InstanceName,CounterPath,SourceSystem|
|Average_Total Packets Transmitted|Número total de paquetes transmitidos|Count|Average|Average_Total Packets Transmitted|Computer,ObjectName,InstanceName,CounterPath,SourceSystem|
|Average_Total Packets Received|Número total de paquetes recibidos|Count|Average|Average_Total Packets Received|Computer,ObjectName,InstanceName,CounterPath,SourceSystem|
|Average_Total Rx Errors|Errores de Rx totales|Count|Average|Average_Total Rx Errors|Computer,ObjectName,InstanceName,CounterPath,SourceSystem|
|Average_Total Tx Errors|Errores de Tx totales|Count|Average|Average_Total Tx Errors|Computer,ObjectName,InstanceName,CounterPath,SourceSystem|
|Average_Total Collisions|Colisiones totales|Count|Average|Average_Total Collisions|Computer,ObjectName,InstanceName,CounterPath,SourceSystem|
|Average_Avg. Segundos de disco/lecturas|Prom. Segundos de disco/lecturas|Count|Average|Average_Avg. Segundos de disco/lecturas|Computer,ObjectName,InstanceName,CounterPath,SourceSystem|
|Average_Avg. Segundos de disco/transferencias|Prom. Segundos de disco/transferencias|Count|Average|Average_Avg. Segundos de disco/transferencias|Computer,ObjectName,InstanceName,CounterPath,SourceSystem|
|Average_Avg. Segundos de disco/escrituras|Prom. Segundos de disco/escrituras|Count|Average|Average_Avg. Segundos de disco/escrituras|Computer,ObjectName,InstanceName,CounterPath,SourceSystem|
|Average_Physical Disk Bytes/sec|Bytes de disco físico/s|Count|Average|Average_Physical Disk Bytes/sec|Computer,ObjectName,InstanceName,CounterPath,SourceSystem|
|Average_Pct Privileged Time|Porcentaje de tiempo con privilegios|Count|Average|Average_Pct Privileged Time|Computer,ObjectName,InstanceName,CounterPath,SourceSystem|
|Average_Pct User Time|Porcentaje de tiempo de usuario|Count|Average|Average_Pct User Time|Computer,ObjectName,InstanceName,CounterPath,SourceSystem|
|Average_Used Memory kBytes|Kilobytes de memoria usados|Count|Average|Average_Used Memory kBytes|Computer,ObjectName,InstanceName,CounterPath,SourceSystem|
|Average_Virtual Shared Memory|Memoria compartida virtual|Count|Average|Average_Virtual Shared Memory|Computer,ObjectName,InstanceName,CounterPath,SourceSystem|
|Average_% DPC Time|% de tiempo de DPC|Count|Average|Average_% DPC Time|Computer,ObjectName,InstanceName,CounterPath,SourceSystem|
|Average_% Idle Time|% de tiempo de inactividad|Count|Average|Average_% Idle Time|Computer,ObjectName,InstanceName,CounterPath,SourceSystem|
|Average_% Interrupt Time|% de tiempo de interrupción|Count|Average|Average_% Interrupt Time|Computer,ObjectName,InstanceName,CounterPath,SourceSystem|
|Average_% IO Wait Time|% de tiempo de espera de E/S|Count|Average|Average_% IO Wait Time|Computer,ObjectName,InstanceName,CounterPath,SourceSystem|
|Average_% Nice Time|% de tiempo bueno|Count|Average|Average_% Nice Time|Computer,ObjectName,InstanceName,CounterPath,SourceSystem|
|Average_% Privileged Time|% de tiempo con privilegios|Count|Average|Average_% Privileged Time|Computer,ObjectName,InstanceName,CounterPath,SourceSystem|
|Average_% Processor Time|% de tiempo de procesador|Count|Average|Average_% Processor Time|Computer,ObjectName,InstanceName,CounterPath,SourceSystem|
|Average_% User Time|% de tiempo de usuario|Count|Average|Average_% User Time|Computer,ObjectName,InstanceName,CounterPath,SourceSystem|
|Average_Free Physical Memory|Memoria física libre|Count|Average|Average_Free Physical Memory|Computer,ObjectName,InstanceName,CounterPath,SourceSystem|
|Average_Free Space in Paging Files|Espacio libre en archivos de paginación|Count|Average|Average_Free Space in Paging Files|Computer,ObjectName,InstanceName,CounterPath,SourceSystem|
|Average_Free Virtual Memory|Memoria virtual libre|Count|Average|Average_Free Virtual Memory|Computer,ObjectName,InstanceName,CounterPath,SourceSystem|
|Average_Processes|Procesos|Count|Average|Average_Processes|Computer,ObjectName,InstanceName,CounterPath,SourceSystem|
|Average_Size Stored In Paging Files|Tamaño almacenado en archivos de paginación|Count|Average|Average_Size Stored In Paging Files|Computer,ObjectName,InstanceName,CounterPath,SourceSystem|
|Average_Uptime|Tiempo de actividad|Count|Average|Average_Uptime|Computer,ObjectName,InstanceName,CounterPath,SourceSystem|
|Average_Users|Usuarios|Count|Average|Average_Users|Computer,ObjectName,InstanceName,CounterPath,SourceSystem|
|Average_Current Disk Queue Length|Current Disk Queue Length|Count|Average|Average_Current Disk Queue Length|Computer,ObjectName,InstanceName,CounterPath,SourceSystem|
|Average_Available MBytes|MB disponibles|Count|Average|Average_Available MBytes|Computer,ObjectName,InstanceName,CounterPath,SourceSystem|
|Average_% Committed Bytes In Use|% de bytes confirmados en uso|Count|Average|Average_% Committed Bytes In Use|Computer,ObjectName,InstanceName,CounterPath,SourceSystem|
|Average_Bytes Received/sec|Bytes recibidos por segundo|Count|Average|Average_Bytes Received/sec|Computer,ObjectName,InstanceName,CounterPath,SourceSystem|
|Average_Bytes Sent/sec|Bytes enviados por segundo|Count|Average|Average_Bytes Sent/sec|Computer,ObjectName,InstanceName,CounterPath,SourceSystem|
|Average_Bytes Total/sec|Bytes totales por segundo|Count|Average|Average_Bytes Total/sec|Computer,ObjectName,InstanceName,CounterPath,SourceSystem|
|Average_Processor Queue Length|Longitud de cola del procesador|Count|Average|Average_Processor Queue Length|Computer,ObjectName,InstanceName,CounterPath,SourceSystem|
|Latido|Latido|Count|Total|Latido|Computer,OSType,Version,SourceComputerId|
|Actualizar|Actualizar|Count|Average|Actualizar|Computer,Product,Classification,UpdateState,Optional,Approved|
|Evento|Evento|Count|Average|Evento|Source,EventLog,Computer,EventCategory,EventLevel,EventLevelName,EventID|

## <a name="microsoftpeeringpeeringservices"></a>Microsoft.Peering/peeringServices

|Métrica|Nombre de métrica para mostrar|Unidad|Tipo de agregación|Descripción|Dimensions|
|---|---|---|---|---|---|
|PrefixLatency|Latencia de prefijo|Milisegundos|Average|Latencia de prefijo media|PrefixName|

## <a name="microsoftpeeringpeerings"></a>Microsoft.Peering/peerings

|Métrica|Nombre de métrica para mostrar|Unidad|Tipo de agregación|Descripción|Dimensions|
|---|---|---|---|---|---|
|SessionAvailabilityV4|Disponibilidad de la sesión V4|Percent|Average|Disponibilidad de la sesión V4|ConnectionId|
|SessionAvailabilityV6|Disponibilidad de la sesión V6|Percent|Average|Disponibilidad de la sesión V6|ConnectionId|
|IngressTrafficRate|Velocidad de tráfico de entrada|BitsPerSecond|Average|Velocidad de tráfico de entrada en bits por segundo|ConnectionId|
|EgressTrafficRate|Velocidad de tráfico de salida|BitsPerSecond|Average|Velocidad de tráfico de salida en bits por segundo|ConnectionId|


## <a name="microsoftpowerbidedicatedcapacities"></a>Microsoft.PowerBIDedicated/capacities

|Métrica|Nombre de métrica para mostrar|Unidad|Tipo de agregación|Descripción|Dimensions|
|---|---|---|---|---|---|
|QueryDuration|Duración de la consulta|Milisegundos|Average|Duración de la consulta DAX en el último intervalo|Sin dimensiones|
|QueryPoolJobQueueLength|Subprocesos: longitud de cola de trabajos del grupo de consultas|Count|Average|Número de trabajos en la cola del grupo de subprocesos de consultas.|Sin dimensiones|
|qpu_high_utilization_metric|Uso intensivo de QPU|Count|Total|Uso intensivo de QPU en el último minuto: 1 para un uso intensivo de QPU y 0 en caso contrario.|Sin dimensiones|
|memory_metric|Memoria|Bytes|Average|Memoria. Intervalo de 0-3 GB para A1, 0-5 GB para A2, 0-10 GB para A3, 0-25 GB para A4, 0-50 GB para A5 y 0-100 GB para A6|Sin dimensiones|
|memory_thrashing_metric|Paginación excesiva de memoria|Percent|Average|Paginación excesiva media de memoria.|Sin dimensiones|


## <a name="microsoftprojectbabylonaccounts"></a>Microsoft.ProjectBabylon/accounts

|Métrica|Nombre de métrica para mostrar|Unidad|Tipo de agregación|Descripción|Dimensions|
|---|---|---|---|---|---|
|AssetDistributionByClassification|Distribución de los recursos por clasificación|Count|Total|Indica el número de activos con una clasificación determinada asignada, es decir, están clasificados con esa etiqueta.|Classification,Source,ResourceId|
|AssetDistributionByStorageType|Distribución de recursos por tipo de almacenamiento|Count|Total|Indica el número de recursos con un tipo de almacenamiento determinado.|StorageType,ResourceId|
|CatalogActiveUsers|Usuarios activos cada día|Count|Total|Número de usuarios activos cada día|ResourceId|
|CatalogUsage|Distribución de uso por operación|Count|Total|Indica el número de operación que realiza el usuario en el catálogo, es decir, acceso, búsqueda, glosario.|Operation,ResourceId|
|NumberOfAssetsWithClassifications|Número de recursos con al menos una clasificación|Count|Average|Indica el número de recursos con al menos una clasificación de etiqueta.|ResourceId|
|ScanCancelled|Examen cancelado|Count|Total|Indica el número de exámenes cancelados.|ResourceId|
|ScanCompleted|Examen completado|Count|Total|Indica el número de exámenes completados correctamente.|ResourceId|
|ScanFailed|Examen con errores|Count|Total|Indica el número de exámenes con errores.|ResourceId|
|ScanTimeTaken|Tiempo invertido en el examen|Segundos|Total|Indica el tiempo de examen total en segundos.|ResourceId|




## <a name="microsoftrelaynamespaces"></a>Microsoft.Relay/namespaces

|Métrica|Nombre de métrica para mostrar|Unidad|Tipo de agregación|Descripción|Dimensions|
|---|---|---|---|---|---|
|ListenerConnections-Success|ListenerConnections-Success|Count|Total|Valor correcto de ListenerConnections de Microsoft.Relay.|EntityName,OperationResult|
|ListenerConnections-ClientError|ListenerConnections-ClientError|Count|Total|ClientError en ListenerConnections para Microsoft.Relay.|EntityName,OperationResult|
|ListenerConnections-ServerError|ListenerConnections-ServerError|Count|Total|ServerError en ListenerConnections para Microsoft.Relay.|EntityName,OperationResult|
|SenderConnections-Success|SenderConnections-Success|Count|Total|Valor correcto de SenderConnections para Microsoft.Relay.|EntityName,OperationResult|
|SenderConnections-ClientError|SenderConnections-ClientError|Count|Total|ClientError en SenderConnections para Microsoft.Relay.|EntityName,OperationResult|
|SenderConnections-ServerError|SenderConnections-ServerError|Count|Total|ServerError en SenderConnections para Microsoft.Relay.|EntityName,OperationResult|
|ListenerConnections-TotalRequests|ListenerConnections-TotalRequests|Count|Total|Valor total de ListenerConnections para Microsoft.Relay.|EntityName|
|SenderConnections-TotalRequests|SenderConnections-TotalRequests|Count|Total|Solicitudes totales de SenderConnections para Microsoft.Relay.|EntityName|
|ActiveConnections|ActiveConnections|Count|Total|Valor total de ActiveConnections para Microsoft.Relay.|EntityName|
|ActiveListeners|ActiveListeners|Count|Total|Valor total de ActiveListeners para Microsoft.Relay.|EntityName|
|BytesTransferred|BytesTransferred|Count|Total|Valor total de BytesTransferred para Microsoft.Relay.|EntityName|
|ListenerDisconnects|ListenerDisconnects|Count|Total|Valor total de ListenerDisconnects para Microsoft.Relay.|EntityName|
|SenderDisconnects|SenderDisconnects|Count|Total|Valor total de SenderDisconnects para Microsoft.Relay.|EntityName|


## <a name="microsoftsearchsearchservices"></a>Microsoft.Search/searchServices

|Métrica|Nombre de métrica para mostrar|Unidad|Tipo de agregación|Descripción|Dimensions|
|---|---|---|---|---|---|
|SearchLatency|Latencia de búsqueda|Segundos|Average|Promedio de latencia de búsqueda para el servicio de búsqueda|None|
|SearchQueriesPerSecond|Consultas de búsqueda por segundo|CountPerSecond|Average|Consultas de búsqueda por segundo para el servicio de búsqueda|None|
|ThrottledSearchQueriesPercentage|Porcentaje de consultas de búsqueda limitadas|Percent|Average|Porcentaje de consultas de búsqueda limitadas para el servicio de búsqueda|None|


## <a name="microsoftservicebusnamespaces"></a>Microsoft.ServiceBus/namespaces

|Métrica|Nombre de métrica para mostrar|Unidad|Tipo de agregación|Descripción|Dimensions|
|---|---|---|---|---|---|
|SuccessfulRequests|Solicitudes correctas|Count|Total|Número total de solicitudes correctas para un espacio de nombres|EntityName,OperationResult|
|ServerErrors|Errores del servidor.|Count|Total|Errores del servidor para Microsoft.ServiceBus.|EntityName,OperationResult|
|UserErrors|Errores de usuario.|Count|Total|Errores de usuario para Microsoft.ServiceBus.|EntityName,OperationResult|
|ThrottledRequests|Solicitudes limitadas.|Count|Total|Solicitudes limitadas para Microsoft.ServiceBus.|EntityName,OperationResult|
|IncomingRequests|Solicitudes entrantes|Count|Total|Solicitudes entrantes para Microsoft.ServiceBus.|EntityName|
|IncomingMessages|Mensajes entrantes|Count|Total|Mensajes entrantes para Microsoft.ServiceBus.|EntityName|
|OutgoingMessages|Mensajes salientes|Count|Total|Mensajes salientes para Microsoft.ServiceBus.|EntityName|
|ActiveConnections|ActiveConnections|Count|Total|Total de conexiones activas para Microsoft.ServiceBus.|None|
|ConnectionsOpened|Conexiones abiertas.|Count|Average|Conexiones abiertas para Microsoft.ServiceBus.|EntityName|
|ConnectionsClosed|Conexiones cerradas.|Count|Average|Conexiones cerradas para Microsoft.ServiceBus.|EntityName|
|Size|Size|Bytes|Average|Tamaño de cola/tema en bytes.|EntityName|
|error de Hadoop|Recuento de mensajes de una cola/tema.|Count|Average|Recuento de mensajes de una cola/tema.|EntityName|
|ActiveMessages|Recuento de mensajes activos de una cola/tema.|Count|Average|Recuento de mensajes activos de una cola/tema.|EntityName|
|DeadletteredMessages|Recuento de mensajes fallidos de una cola/tema.|Count|Average|Recuento de mensajes fallidos de una cola/tema.|EntityName|
|ScheduledMessages|Recuento de mensajes programados de una cola/tema.|Count|Average|Recuento de mensajes programados de una cola/tema.|EntityName|
|NamespaceCpuUsage|CPU|Percent|Máxima|Métrica de uso de CPU de espacio de nombres prémium de Service Bus.|Réplica|
|NamespaceMemoryUsage|Uso de la memoria|Percent|Máxima|Métrica de uso de memoria de espacio de nombres prémium de Service Bus.|Réplica|
|CPUXNS|CPU (Deprecated) [CPU (en desuso)]|Percent|Máxima|Métrica de uso de CPU de espacio de nombres prémium de Service Bus. Esta métrica está en desuso. Use la métrica de CPU (NamespaceCpuUsage) en su lugar.|Réplica|
|WSXNS|Memory Usage (Deprecated) [Uso de memoria (en desuso)]|Percent|Máxima|Métrica de uso de memoria de espacio de nombres prémium de Service Bus. Esta métrica está en desuso. Utilice la métrica de uso de memoria (NamespaceMemoryUsage) en su lugar.|Réplica|


## <a name="microsoftservicefabricmeshapplications"></a>Microsoft.ServiceFabricMesh/applications

|Métrica|Nombre de métrica para mostrar|Unidad|Tipo de agregación|Descripción|Dimensions|
|---|---|---|---|---|---|
|AllocatedCpu|AllocatedCpu|Count|Average|CPU asignada a este contenedor en millares de núcleos|ApplicationName,ServiceName,CodePackageName,ServiceReplicaName|
|AllocatedMemory|AllocatedMemory|Bytes|Average|Memoria asignada a este contenedor en MB|ApplicationName,ServiceName,CodePackageName,ServiceReplicaName|
|ActualCpu|ActualCpu|Count|Average|Uso de CPU real en millares de núcleos|ApplicationName,ServiceName,CodePackageName,ServiceReplicaName|
|ActualMemory|ActualMemory|Bytes|Average|Uso de memoria real en MB|ApplicationName,ServiceName,CodePackageName,ServiceReplicaName|
|CpuUtilization|CpuUtilization|Percent|Average|Utilización de la CPU para este contenedor como porcentaje de AllocatedCpu|ApplicationName,ServiceName,CodePackageName,ServiceReplicaName|
|MemoryUtilization|MemoryUtilization|Percent|Average|Utilización de la CPU para este contenedor como porcentaje de AllocatedCpu|ApplicationName,ServiceName,CodePackageName,ServiceReplicaName|
|ApplicationStatus|ApplicationStatus|Count|Average|Estado de una aplicación Service Fabric Mesh|ApplicationName,Status|
|ServiceStatus|ServiceStatus|Count|Average|Estado de mantenimiento de un servicio en una aplicación Service Fabric Mesh|ApplicationName,Status,ServiceName|
|ServiceReplicaStatus|ServiceReplicaStatus|Count|Average|Estado de mantenimiento de una réplica de servicio en una aplicación Service Fabric Mesh|ApplicationName,Status,ServiceName,ServiceReplicaName|
|ContainerStatus|ContainerStatus|Count|Average|Estado del contenedor en una aplicación Service Fabric Mesh|ApplicationName,ServiceName,CodePackageName,ServiceReplicaName,Status|
|RestartCount|RestartCount|Count|Average|Número de reinicios de un contenedor en una aplicación Service Fabric Mesh|ApplicationName,Status,ServiceName,ServiceReplicaName,CodePackageName|

## <a name="microsoftsignalrservicesignalr"></a>Microsoft.SignalRService/SignalR

|Métrica|Nombre de métrica para mostrar|Unidad|Tipo de agregación|Descripción|Dimensions|
|---|---|---|---|---|---|
|ConnectionCount|Número de conexiones|Count|Máxima|Cantidad de conexiones de usuario.|Punto de conexión|
|MessageCount|Número de mensajes|Count|Total|Cantidad total de mensajes.|None|
|InboundTraffic|Tráfico entrante|Bytes|Total|Tráfico entrante del servicio|None|
|OutboundTraffic|Tráfico saliente|Bytes|Total|Tráfico saliente del servicio|None|
|UserErrors|Errores de usuario|Percent|Máxima|Porcentaje de errores de usuario|None|
|SystemErrors|Errores de sistema|Percent|Máxima|Porcentaje de errores del sistema|None|



## <a name="microsoftsqlserversdatabases"></a>Microsoft.Sql/servers/databases

|Métrica|Nombre de métrica para mostrar|Unidad|Tipo de agregación|Descripción|Dimensions|
|---|---|---|---|---|---|
|cpu_percent|Porcentaje de CPU|Percent|Average|Porcentaje de CPU|None|
|physical_data_read_percent|Porcentaje de E/S de datos|Percent|Average|Porcentaje de E/S de datos|None|
|log_write_percent|Porcentaje de E/S de registro|Percent|Average|Porcentaje de E/S de registro. No es aplicable a los almacenes de datos.|None|
|dtu_consumption_percent|Porcentaje de DTU|Percent|Average|Porcentaje de DTU. Se aplica a las bases de datos basadas en DTU.|None|
|storage|Espacio de datos usado|Bytes|Máxima|Espacio de datos usado. No es aplicable a los almacenes de datos.|None|
|connection_successful|Conexiones correctas|Count|Total|Conexiones correctas|None|
|connection_failed|Conexiones con errores|Count|Total|Conexiones con errores|None|
|blocked_by_firewall|Bloqueado por el firewall|Count|Total|Bloqueado por el firewall|None|
|deadlock|Interbloqueos|Count|Total|Interbloqueos. No es aplicable a los almacenes de datos.|None|
|storage_percent|Porcentaje de espacio de datos usado|Percent|Máxima|Porcentaje de espacio de datos usado. No se aplica a los almacenamientos de datos ni a las bases de datos de hiperescala.|None|
|xtp_storage_percent|Porcentaje de almacenamiento de OLTP en memoria|Percent|Average|Porcentaje de almacenamiento de OLTP en memoria. No es aplicable a los almacenes de datos.|None|
|workers_percent|Porcentaje de trabajos|Percent|Average|Porcentaje de trabajos. No es aplicable a los almacenes de datos.|None|
|sessions_percent|Porcentaje de sesiones|Percent|Average|Porcentaje de sesiones. No es aplicable a los almacenes de datos.|None|
|dtu_limit|Límite de DTU|Count|Average|Límite de DTU. Se aplica a las bases de datos basadas en DTU.|None|
|dtu_used|DTU utilizada|Count|Average|DTU utilizada. Se aplica a las bases de datos basadas en DTU.|None|
|cpu_limit|Límite de CPU|Count|Average|Límite de CPU. Se aplica a las bases de datos basadas en núcleo virtual.|None|
|cpu_used|CPU utilizada|Count|Average|CPU utilizada. Se aplica a las bases de datos basadas en núcleo virtual.|None|
|dwu_limit|Límite de DWU|Count|Máxima|Límite de DWU. Solo se aplica a los almacenes de datos.|None|
|dwu_consumption_percent|Porcentaje de DWU|Percent|Máxima|Porcentaje de DWU. Solo se aplica a los almacenes de datos.|None|
|dwu_used|DWU utilizada|Count|Máxima|DWU utilizada. Solo se aplica a los almacenes de datos.|None|
|cache_hit_percent|Porcentaje de aciertos de caché|Percent|Máxima|Porcentaje de aciertos de caché. Solo se aplica a los almacenes de datos.|None|
|cache_used_percent|Porcentaje de caché usada|Percent|Máxima|Porcentaje de caché usada. Solo se aplica a los almacenes de datos.|None|
|sqlserver_process_core_percent<sup>1</sup> |Porcentaje de núcleos de proceso de SQL Server|Percent|Máxima|Porcentaje de uso de la CPU para el proceso de SQL Server, medido por el sistema operativo.|None|
|sqlserver_process_memory_percent<sup>1</sup> |Porcentaje de memoria de proceso de SQL Server|Percent|Máxima|Porcentaje de uso de la memoria para el proceso de SQL Server, medido por el sistema operativo.|None|
|tempdb_data_size<sup>1</sup> |Kilobytes de tamaño de archivo de datos Tempdb|Count|Máxima|Kilobytes de tamaño de archivo de datos Tempdb.|None|
|tempdb_log_size<sup>1</sup> |Kilobytes de tamaño de archivo de registro Tempdb|Count|Máxima|Kilobytes de tamaño de archivo de registro Tempdb.|None|
|tempdb_log_used_percent<sup>1</sup> |Porcentaje de registro de tempdb usado|Percent|Máxima|Porcentaje de registro de tempdb usado.|None|
|local_tempdb_usage_percent|Porcentaje de tempdb local|Percent|Average|Porcentaje de tempdb local. Solo se aplica a los almacenes de datos.|None|
|app_cpu_billed|CPU de aplicación facturada|Count|Total|CPU de aplicación facturada. Se aplica a las bases de datos sin servidor.|None|
|app_cpu_percent|Porcentaje de CPU de la aplicación|Percent|Average|Porcentaje de CPU de la aplicación. Se aplica a las bases de datos sin servidor.|None|
|app_memory_percent|App memory percentage (Porcentaje de memoria de la aplicación)|Percent|Average|Porcentaje de memoria de la aplicación. Se aplica a las bases de datos sin servidor.|None|
|allocated_data_storage|Espacio de datos asignado|Bytes|Average|Almacenamiento de datos asignado. No es aplicable a los almacenes de datos.|None|
|memory_usage_percent|Porcentaje de memoria|Percent|Máxima|Porcentaje de memoria. Solo se aplica a los almacenes de datos.|None|
|dw_backup_size_gb|Tamaño del almacenamiento de datos|Count|Total|El tamaño del almacenamiento de datos se compone del tamaño de los datos y del registro de transacciones. La métrica se cuenta para la parte de "almacenamiento" de la factura. Solo se aplica a los almacenes de datos.|None|
|dw_snapshot_size_gb|Tamaño del almacenamiento de instantáneas|Count|Total|El tamaño del almacenamiento de instantáneas es el tamaño de los cambios incrementales que capturan las instantáneas para crear puntos de restauración definidos por el usuario y automáticos. La métrica se cuenta para la parte de "almacenamiento" de la factura. Solo se aplica a los almacenes de datos.|None|
|dw_geosnapshot_size_gb|Tamaño de almacenamiento de recuperación ante desastres|Count|Total|El tamaño de almacenamiento de recuperación ante desastres se refleja como "almacenamiento de recuperación ante desastres" en su factura. Solo se aplica a los almacenes de datos.|None|
|wlg_allocation_relative_to_system_percent|Asignación de grupos de cargas de trabajo por porcentaje del sistema|Percent|Máxima|Porcentaje asignado de recursos en relación con todo el sistema por grupo de cargas de trabajo. Solo se aplica a los almacenes de datos.|WorkloadGroupName,IsUserDefined|
|wlg_allocation_relative_to_wlg_effective_cap_percent|\- Asignación de grupos de cargas de trabajo por porcentaje máximo de recursos|Percent|Máxima|Porcentaje asignado de recursos en relación con los recursos máximos especificados por grupo de cargas de trabajo. Solo se aplica a los almacenes de datos.|WorkloadGroupName,IsUserDefined|
|wlg_active_queries|Consultas activas de grupo de cargas de trabajo|Count|Total|Consultas activas dentro del grupo de cargas de trabajo. Solo se aplica a los almacenes de datos.|WorkloadGroupName,IsUserDefined|
|wlg_queued_queries|Consultas en cola del grupo de cargas de trabajo|Count|Total|Consultas en cola dentro del grupo de cargas de trabajo. Solo se aplica a los almacenes de datos.|WorkloadGroupName,IsUserDefined|
|active_queries|Consultas activas|Count|Total|Consultas activas en todos los grupos de cargas de trabajo. Solo se aplica a los almacenes de datos.|None|
|queued_queries|Consultas en cola|Count|Total|Consultas en cola en todos los grupos de cargas de trabajo. Solo se aplica a los almacenes de datos.|None|
|wlg_active_queries_timeouts|Tiempos de espera de consultas de grupo de cargas de trabajo|Count|Total|Consultas que han agotado el tiempo de espera para el grupo de cargas de trabajo. Solo se aplica a los almacenes de datos.|WorkloadGroupName,IsUserDefined|
|wlg_effective_min_resource_percent|Porcentaje mínimo de recursos efectivo|Percent|Máxima|Porcentaje mínimo de recursos reservados y aislados para el grupo de cargas de trabajo teniendo en cuenta el nivel de servicio mínimo. Solo se aplica a los almacenes de datos.|WorkloadGroupName,IsUserDefined|
|wlg_effective_cap_resource_percent|Porcentaje máximo de recursos efectivo|Percent|Máxima|Límite máximo en el porcentaje de recursos permitidos para el grupo de cargas de trabajo, teniendo en cuenta el porcentaje de recursos mínimo efectivo asignado a otros grupos de cargas de trabajo. Solo se aplica a los almacenes de datos.|WorkloadGroupName,IsUserDefined|
|full_backup_size_bytes|Tamaño de almacenamiento de copia de seguridad completa|Bytes|Máxima|Tamaño de almacenamiento de copia de seguridad completa acumulativa. Se aplica a las bases de datos basadas en núcleo virtual. No aplicable a bases de datos de hiperescala.|None|
|diff_backup_size_bytes|Tamaño de almacenamiento de copia de seguridad diferencial|Bytes|Máxima|Tamaño de almacenamiento de copia de seguridad diferencial acumulativo. Se aplica a las bases de datos basadas en núcleo virtual. No aplicable a bases de datos de hiperescala.|None|
|log_backup_size_bytes|Tamaño de almacenamiento de copia de seguridad de registros|Bytes|Máxima|Tamaño de almacenamiento de copia de seguridad de registros acumulativa. Se aplica a las bases de datos de hiperescala y basadas en núcleo virtual.|None|
|snapshot_backup_size_bytes|Tamaño de almacenamiento de copia de seguridad de instantánea|Bytes|Máxima|Tamaño de almacenamiento de copia de seguridad de instantánea acumulativa. Se aplica a las bases de datos de hiperescala.|None|
|base_blob_size_bytes|Tamaño de almacenamiento de blobs de base|Bytes|Máxima|Tamaño de almacenamiento de blobs de base. Se aplica a las bases de datos de hiperescala.|None|

<sup>1</sup> Esta métrica está disponible para las bases de datos que usan el modelo de compra de núcleos virtuales con 2 núcleos virtuales, o más, o 200 DTU, o más, para el modelo de compra basado en DTU. 

## <a name="microsoftsqlserverselasticpools"></a>Microsoft.Sql/servers/elasticPools

|Métrica|Nombre de métrica para mostrar|Unidad|Tipo de agregación|Descripción|Dimensions|
|---|---|---|---|---|---|
|cpu_percent|Porcentaje de CPU|Percent|Average|Porcentaje de CPU|None|
|database_cpu_percent|Porcentaje de CPU|Percent|Average|Porcentaje de CPU|DatabaseResourceId|
|physical_data_read_percent|Porcentaje de E/S de datos|Percent|Average|Porcentaje de E/S de datos|None|
|database_physical_data_read_percent|Porcentaje de E/S de datos|Percent|Average|Porcentaje de E/S de datos|DatabaseResourceId|
|log_write_percent|Porcentaje de E/S de registro|Percent|Average|Porcentaje de E/S de registro|None|
|database_log_write_percent|Porcentaje de E/S de registro|Percent|Average|Porcentaje de E/S de registro|DatabaseResourceId|
|dtu_consumption_percent|Porcentaje de DTU|Percent|Average|Porcentaje de DTU. Se aplica a los grupos elásticos basados en DTU.|None|
|database_dtu_consumption_percent|Porcentaje de DTU|Percent|Average|Porcentaje de DTU|DatabaseResourceId|
|storage_percent|Porcentaje de espacio de datos usado|Percent|Average|Porcentaje de espacio de datos usado|None|
|workers_percent|Porcentaje de trabajos|Percent|Average|Porcentaje de trabajos|None|
|database_workers_percent|Porcentaje de trabajos|Percent|Average|Porcentaje de trabajos|DatabaseResourceId|
|sessions_percent|Porcentaje de sesiones|Percent|Average|Porcentaje de sesiones|None|
|database_sessions_percent|Porcentaje de sesiones|Percent|Average|Porcentaje de sesiones|DatabaseResourceId|
|eDTU_limit|Límite de eDTU|Count|Average|Límite de eDTU. Se aplica a los grupos elásticos basados en DTU.|None|
|storage_limit|Tamaño máximo de datos|Bytes|Average|Tamaño máximo de datos|None|
|eDTU_used|eDTU utilizada|Count|Average|eDTU utilizada. Se aplica a los grupos elásticos basados en DTU.|None|
|database_eDTU_used|eDTU utilizada|Count|Average|eDTU utilizada|DatabaseResourceId|
|storage_used|Espacio de datos usado|Bytes|Average|Espacio de datos usado|None|
|database_storage_used|Espacio de datos usado|Bytes|Average|Espacio de datos usado|DatabaseResourceId|
|xtp_storage_percent|Porcentaje de almacenamiento de OLTP en memoria|Percent|Average|Porcentaje de almacenamiento de OLTP en memoria|None|
|cpu_limit|Límite de CPU|Count|Average|Límite de CPU. Se aplica a los grupos elásticos basados en núcleo virtual.|None|
|database_cpu_limit|Límite de CPU|Count|Average|Límite de CPU|DatabaseResourceId|
|cpu_used|CPU utilizada|Count|Average|CPU utilizada. Se aplica a los grupos elásticos basados en núcleo virtual.|None|
|database_eDTU_used|CPU utilizada|Count|Average|CPU utilizada|DatabaseResourceId|
|sqlserver_process_core_percent<sup>1</sup>|Porcentaje de núcleos de proceso de SQL Server|Percent|Máxima|Porcentaje de uso de la CPU para el proceso de SQL Server, medido por el sistema operativo. Se aplica a los grupos elásticos. |None|
|sqlserver_process_memory_percent<sup>1</sup>|Porcentaje de memoria de proceso de SQL Server|Percent|Máxima|Porcentaje de uso de la memoria para el proceso de SQL Server, medido por el sistema operativo. Se aplica a los grupos elásticos. |None|
|tempdb_data_size<sup>1</sup>|Kilobytes de tamaño de archivo de datos Tempdb|Count|Máxima|Kilobytes de tamaño de archivo de datos Tempdb.|None|
|tempdb_log_size<sup>1</sup>|Kilobytes de tamaño de archivo de registro Tempdb|Count|Máxima|Kilobytes de tamaño de archivo de registro Tempdb. |None|
|tempdb_log_used_percent<sup>1</sup>|Porcentaje de registro de tempdb usado|Percent|Máxima|Porcentaje de registro de tempdb usado.|None|
|allocated_data_storage|Espacio de datos asignado|Bytes|Average|Espacio de datos asignado|None|
|database_allocated_data_storage|Espacio de datos asignado|Bytes|Average|Espacio de datos asignado|DatabaseResourceId|
|allocated_data_storage_percent|Porcentaje asignado de espacio de datos|Percent|Máxima|Porcentaje asignado de espacio de datos|None|

<sup>1</sup> Esta métrica está disponible para las bases de datos que usan el modelo de compra de núcleos virtuales con 2 núcleos virtuales, o más, o 200 DTU, o más, para el modelo de compra basado en DTU. 

## <a name="microsoftsqlmanagedinstances"></a>Microsoft.Sql/managedInstances

|Métrica|Nombre de métrica para mostrar|Unidad|Tipo de agregación|Descripción|Dimensions|
|---|---|---|---|---|---|
|virtual_core_count|Recuento de núcleos virtuales|Count|Average|Recuento de núcleos virtuales|None|
|avg_cpu_percent|Porcentaje de CPU medio|Percent|Average|Porcentaje de CPU medio|None|
|reserved_storage_mb|Espacio de almacenamiento reservado|Count|Average|Espacio de almacenamiento reservado|None|
|storage_space_used_mb|Espacio de almacenamiento usado|Count|Average|Espacio de almacenamiento usado|None|
|io_requests|Recuento de solicitudes de E/S|Count|Average|Recuento de solicitudes de E/S|None|
|io_bytes_read|Bytes de E/S leídos|Bytes|Average|Bytes de E/S leídos|None|
|io_bytes_written|Bytes de E/S escritos|Bytes|Average|Bytes de E/S escritos|None|



## <a name="microsoftstoragestorageaccounts"></a>Microsoft.Storage/storageAccounts

|Métrica|Nombre de métrica para mostrar|Unidad|Tipo de agregación|Descripción|Dimensions|
|---|---|---|---|---|---|
|UsedCapacity|Capacidad usada|Bytes|Average|Capacidad usada de la cuenta|None|
|Transacciones|Transacciones|Count|Total|El número de solicitudes realizadas a un servicio de almacenamiento o la operación de API especificada. Este número incluye solicitudes correctas y con errores, así como las solicitudes que generaron errores. Use la dimensión ResponseType para el número de diferentes tipos de respuesta.|ResponseType,GeoType,ApiName,Authentication|
|Entrada|Entrada|Bytes|Total|La cantidad de datos de entrada, en bytes. Este número incluye la entradas desde un cliente externo en Azure Storage, así como la entrada dentro de Azure.|GeoType,ApiName,Authentication|
|Salida|Salida|Bytes|Total|La cantidad de datos de salida, en bytes. Este número incluye la salida de un cliente externo en Azure Storage, así como la salida dentro de Azure. En consecuencia, este número no refleja las salidas facturables.|GeoType,ApiName,Authentication|
|SuccessServerLatency|Latencia del servidor correcta|Milisegundos|Average|La latencia media utilizada por Azure Storage para procesar una solicitud correcta, en milisegundos. Este valor no incluye la latencia de red especificada en AverageE2ELatency.|GeoType,ApiName,Authentication|
|SuccessE2ELatency|Latencia de E2E correcta|Milisegundos|Average|La latencia media de un extremo a otro de las solicitudes correctas realizadas a un servicio de almacenamiento o a la operación de API especificada, en milisegundos. Este valor incluye el tiempo de procesamiento requerido dentro de Azure Storage para leer la solicitud, enviar la respuesta y recibir la confirmación de la respuesta.|GeoType,ApiName,Authentication|
|Disponibilidad|Disponibilidad|Percent|Average|El porcentaje de disponibilidad para el servicio de almacenamiento o la operación de API especificada. Para calcular la disponibilidad hay que tomar el valor deTotalBillableRequests y dividirlo por el número de solicitudes aplicables, incluidas aquellas que generaron errores inesperados. Todos los errores inesperados reducen la disponibilidad del servicio de almacenamiento o de la operación de API especificada.|GeoType,ApiName,Authentication|

## <a name="microsoftstoragestorageaccountsblobservices"></a>Microsoft.Storage/storageAccounts/blobServices

|Métrica|Nombre de métrica para mostrar|Unidad|Tipo de agregación|Descripción|Dimensions|
|---|---|---|---|---|---|
|BlobCapacity|Capacidad de Blob|Bytes|Average|Cantidad de almacenamiento que ha usado la instancia de Blob service de la cuenta de almacenamiento, en bytes.|BlobType,Tier|
|BlobCount|Recuento de blobs|Count|Average|Número de blobs en la instancia de Blob service de la cuenta de almacenamiento.|BlobType,Tier|
|ContainerCount|Recuento de contenedores de blobs|Count|Average|Número de contenedores en la instancia de Blob service de la cuenta de almacenamiento.|None|
|IndexCapacity|Capacidad de índice|Bytes|Average|Cantidad de almacenamiento que usa el índice (jerárquico) de ADLS Gen2 en bytes.|None|
|Transacciones|Transacciones|Count|Total|El número de solicitudes realizadas a un servicio de almacenamiento o la operación de API especificada. Este número incluye solicitudes correctas y con errores, así como las solicitudes que generaron errores. Use la dimensión ResponseType para el número de diferentes tipos de respuesta.|ResponseType,GeoType,ApiName,Authentication|
|Entrada|Entrada|Bytes|Total|La cantidad de datos de entrada, en bytes. Este número incluye la entradas desde un cliente externo en Azure Storage, así como la entrada dentro de Azure.|GeoType,ApiName,Authentication|
|Salida|Salida|Bytes|Total|La cantidad de datos de salida, en bytes. Este número incluye la salida de un cliente externo en Azure Storage, así como la salida dentro de Azure. En consecuencia, este número no refleja las salidas facturables.|GeoType,ApiName,Authentication|
|SuccessServerLatency|Latencia del servidor correcta|Milisegundos|Average|La latencia media utilizada por Azure Storage para procesar una solicitud correcta, en milisegundos. Este valor no incluye la latencia de red especificada en AverageE2ELatency.|GeoType,ApiName,Authentication|
|SuccessE2ELatency|Latencia de E2E correcta|Milisegundos|Average|La latencia media de un extremo a otro de las solicitudes correctas realizadas a un servicio de almacenamiento o a la operación de API especificada, en milisegundos. Este valor incluye el tiempo de procesamiento requerido dentro de Azure Storage para leer la solicitud, enviar la respuesta y recibir la confirmación de la respuesta.|GeoType,ApiName,Authentication|
|Disponibilidad|Disponibilidad|Percent|Average|El porcentaje de disponibilidad para el servicio de almacenamiento o la operación de API especificada. Para calcular la disponibilidad hay que tomar el valor deTotalBillableRequests y dividirlo por el número de solicitudes aplicables, incluidas aquellas que generaron errores inesperados. Todos los errores inesperados reducen la disponibilidad del servicio de almacenamiento o de la operación de API especificada.|GeoType,ApiName,Authentication|

## <a name="microsoftstoragestorageaccountstableservices"></a>Microsoft.Storage/storageAccounts/tableServices

|Métrica|Nombre de métrica para mostrar|Unidad|Tipo de agregación|Descripción|Dimensions|
|---|---|---|---|---|---|
|TableCapacity|Capacidad de Table|Bytes|Average|Cantidad de almacenamiento que ha utilizado la instancia de Table service de la cuenta de almacenamiento, en bytes.|None|
|TableCount|Recuento de tablas|Count|Average|Número de tablas en la instancia de Table service de la cuenta de almacenamiento.|None|
|TableEntityCount|Recuento de entidades de Table|Count|Average|Número de entidades de tabla en la instancia de Table service de la cuenta de almacenamiento.|None|
|Transacciones|Transacciones|Count|Total|El número de solicitudes realizadas a un servicio de almacenamiento o la operación de API especificada. Este número incluye solicitudes correctas y con errores, así como las solicitudes que generaron errores. Use la dimensión ResponseType para el número de diferentes tipos de respuesta.|ResponseType,GeoType,ApiName,Authentication|
|Entrada|Entrada|Bytes|Total|La cantidad de datos de entrada, en bytes. Este número incluye la entradas desde un cliente externo en Azure Storage, así como la entrada dentro de Azure.|GeoType,ApiName,Authentication|
|Salida|Salida|Bytes|Total|La cantidad de datos de salida, en bytes. Este número incluye la salida de un cliente externo en Azure Storage, así como la salida dentro de Azure. En consecuencia, este número no refleja las salidas facturables.|GeoType,ApiName,Authentication|
|SuccessServerLatency|Latencia del servidor correcta|Milisegundos|Average|La latencia media utilizada por Azure Storage para procesar una solicitud correcta, en milisegundos. Este valor no incluye la latencia de red especificada en AverageE2ELatency.|GeoType,ApiName,Authentication|
|SuccessE2ELatency|Latencia de E2E correcta|Milisegundos|Average|La latencia media de un extremo a otro de las solicitudes correctas realizadas a un servicio de almacenamiento o a la operación de API especificada, en milisegundos. Este valor incluye el tiempo de procesamiento requerido dentro de Azure Storage para leer la solicitud, enviar la respuesta y recibir la confirmación de la respuesta.|GeoType,ApiName,Authentication|
|Disponibilidad|Disponibilidad|Percent|Average|El porcentaje de disponibilidad para el servicio de almacenamiento o la operación de API especificada. Para calcular la disponibilidad hay que tomar el valor deTotalBillableRequests y dividirlo por el número de solicitudes aplicables, incluidas aquellas que generaron errores inesperados. Todos los errores inesperados reducen la disponibilidad del servicio de almacenamiento o de la operación de API especificada.|GeoType,ApiName,Authentication|

## <a name="microsoftstoragestorageaccountsfileservices"></a>Microsoft.Storage/storageAccounts/fileServices

|Métrica|Nombre de métrica para mostrar|Unidad|Tipo de agregación|Descripción|Dimensions|
|---|---|---|---|---|---|
|FileCapacity|Capacidad de File|Bytes|Average|Cantidad de almacenamiento que ha utilizado el servicio File de la cuenta de almacenamiento, en bytes.|FileShare|
|FileCount|Recuento de archivos|Count|Average|Número de archivos en el servicio File de la cuenta de almacenamiento.|FileShare|
|FileShareCount|Recuento de recursos compartidos de archivos|Count|Average|Número de recursos compartidos de archivos en el servicio File de la cuenta de almacenamiento.|None|
|FileShareSnapshotCount|File share snapshot count (Recuento de instantáneas del recurso compartido de archivos)|Count|Average|Número de instantáneas presentes en el recurso compartido en el servicio Files de la cuenta de almacenamiento.|FileShare|
|FileShareSnapshotSize|File share snapshot size (Tamaño de instantánea del recurso compartido de archivos)|Bytes|Average|Cantidad de almacenamiento que usan las instantáneas del servicio Files de la cuenta de almacenamiento en bytes.|FileShare|
|FileShareCapacityQuota|File share quota size (Tamaño de cuota del recurso compartido de archivos)|Bytes|Average|Límite superior de la cantidad de almacenamiento que puede usar el servicio Azure Files en bytes.|FileShare|
|Transacciones|Transacciones|Count|Total|El número de solicitudes realizadas a un servicio de almacenamiento o la operación de API especificada. Este número incluye solicitudes correctas y con errores, así como las solicitudes que generaron errores. Use la dimensión ResponseType para el número de diferentes tipos de respuesta.|ResponseType,GeoType,ApiName,Authentication,FileShare|
|Entrada|Entrada|Bytes|Total|La cantidad de datos de entrada, en bytes. Este número incluye la entradas desde un cliente externo en Azure Storage, así como la entrada dentro de Azure.|GeoType,ApiName,Authentication,FileShare|
|Salida|Salida|Bytes|Total|La cantidad de datos de salida, en bytes. Este número incluye la salida de un cliente externo en Azure Storage, así como la salida dentro de Azure. En consecuencia, este número no refleja las salidas facturables.|GeoType,ApiName,Authentication,FileShare|
|SuccessServerLatency|Latencia del servidor correcta|Milisegundos|Average|La latencia media utilizada por Azure Storage para procesar una solicitud correcta, en milisegundos. Este valor no incluye la latencia de red especificada en AverageE2ELatency.|GeoType,ApiName,Authentication,FileShare|
|SuccessE2ELatency|Latencia de E2E correcta|Milisegundos|Average|La latencia media de un extremo a otro de las solicitudes correctas realizadas a un servicio de almacenamiento o a la operación de API especificada, en milisegundos. Este valor incluye el tiempo de procesamiento requerido dentro de Azure Storage para leer la solicitud, enviar la respuesta y recibir la confirmación de la respuesta.|GeoType,ApiName,Authentication,FileShare|
|Disponibilidad|Disponibilidad|Percent|Average|El porcentaje de disponibilidad para el servicio de almacenamiento o la operación de API especificada. Para calcular la disponibilidad hay que tomar el valor deTotalBillableRequests y dividirlo por el número de solicitudes aplicables, incluidas aquellas que generaron errores inesperados. Todos los errores inesperados reducen la disponibilidad del servicio de almacenamiento o de la operación de API especificada.|GeoType,ApiName,Authentication,FileShare|

## <a name="microsoftstoragestorageaccountsqueueservices"></a>Microsoft.Storage/storageAccounts/queueServices

|Métrica|Nombre de métrica para mostrar|Unidad|Tipo de agregación|Descripción|Dimensions|
|---|---|---|---|---|---|
|QueueCapacity|Capacidad de Queue|Bytes|Average|Cantidad de almacenamiento que ha utilizado la instancia de Queue service de la cuenta de almacenamiento, en bytes.|None|
|QueueCount|Recuento de colas|Count|Average|Número de colas en la instancia de Queue service de la cuenta de almacenamiento.|None|
|QueueMessageCount|Recuento de mensajes de Queue|Count|Average|Número aproximado de mensajes de cola en la instancia de Queue service de la cuenta de almacenamiento.|None|
|Transacciones|Transacciones|Count|Total|El número de solicitudes realizadas a un servicio de almacenamiento o la operación de API especificada. Este número incluye solicitudes correctas y con errores, así como las solicitudes que generaron errores. Use la dimensión ResponseType para el número de diferentes tipos de respuesta.|ResponseType,GeoType,ApiName,Authentication|
|Entrada|Entrada|Bytes|Total|La cantidad de datos de entrada, en bytes. Este número incluye la entradas desde un cliente externo en Azure Storage, así como la entrada dentro de Azure.|GeoType,ApiName,Authentication|
|Salida|Salida|Bytes|Total|La cantidad de datos de salida, en bytes. Este número incluye la salida de un cliente externo en Azure Storage, así como la salida dentro de Azure. En consecuencia, este número no refleja las salidas facturables.|GeoType,ApiName,Authentication|
|SuccessServerLatency|Latencia del servidor correcta|Milisegundos|Average|La latencia media utilizada por Azure Storage para procesar una solicitud correcta, en milisegundos. Este valor no incluye la latencia de red especificada en AverageE2ELatency.|GeoType,ApiName,Authentication|
|SuccessE2ELatency|Latencia de E2E correcta|Milisegundos|Average|La latencia media de un extremo a otro de las solicitudes correctas realizadas a un servicio de almacenamiento o a la operación de API especificada, en milisegundos. Este valor incluye el tiempo de procesamiento requerido dentro de Azure Storage para leer la solicitud, enviar la respuesta y recibir la confirmación de la respuesta.|GeoType,ApiName,Authentication|
|Disponibilidad|Disponibilidad|Percent|Average|El porcentaje de disponibilidad para el servicio de almacenamiento o la operación de API especificada. Para calcular la disponibilidad hay que tomar el valor deTotalBillableRequests y dividirlo por el número de solicitudes aplicables, incluidas aquellas que generaron errores inesperados. Todos los errores inesperados reducen la disponibilidad del servicio de almacenamiento o de la operación de API especificada.|GeoType,ApiName,Authentication|





## <a name="microsoftstoragecachecaches"></a>Microsoft.StorageCache/caches

|Métrica|Nombre de métrica para mostrar|Unidad|Tipo de agregación|Descripción|Dimensions|
|---|---|---|---|---|---|
|ClientIOPS|Total Client IOPS (Total de IOPS de cliente)|Count|Average|Tasa de operaciones de archivos de cliente que procesa la caché.|None|
|ClientLatency|Average Client Latency (Latencia media de cliente)|Milisegundos|Average|Latencia media de las operaciones de archivos de cliente en la caché de almacenamiento.|None|
|ClientReadIOPS|Client Read IOPS (IOPS de lectura de cliente)|CountPerSecond|Average|Operaciones de lectura de cliente por segundo.|None|
|ClientReadThroughput|Average Cache Read Throughput (Rendimiento medio de lectura de caché)|BytesPerSecond|Average|Velocidad de transferencia de los datos de lectura del cliente.|None|
|ClientWriteIOPS|Client Write IOPS (IOPS de escritura de cliente)|CountPerSecond|Average|Operaciones de escritura de cliente por segundo.|None|
|ClientWriteThroughput|Average Cache Write Throughput (Rendimiento medio de escritura de caché)|BytesPerSecond|Average|Velocidad de transferencia de los datos de escritura del cliente.|None|
|ClientMetadataReadIOPS|Client Metadata Read IOPS (IOPS de lectura de metadatos de cliente)|CountPerSecond|Average|Tasa de operaciones de archivos de cliente enviadas a la memoria caché, sin incluir las lecturas de datos, que no modifican el estado persistente.|None|
|ClientMetadataWriteIOPS|Client Metadata Write IOPS (IOPS de escritura de metadatos de cliente)|CountPerSecond|Average|Tasa de operaciones de archivos de cliente enviadas a la caché, sin incluir las escrituras de datos, que modifican el estado persistente.|None|
|ClientLockIOPS|Client Lock IOPS (IOPS de bloqueo de cliente)|CountPerSecond|Average|Operaciones de bloqueo de archivos de cliente por segundo.|None|
|StorageTargetHealth|Storage Target Health (Estado del destino de almacenamiento)|Count|Average|Resultados booleanos de la prueba de conectividad entre la caché y los destinos de almacenamiento.|None|
|Tiempo de actividad|Tiempo de actividad|Count|Average|Resultados booleanos de la prueba de conectividad entre la caché y el sistema de supervisión.|None|
|StorageTargetIOPS|Total StorageTarget IOPS (IOPS total de StorageTarget)|Count|Average|Tasa de todas las operaciones de archivos que la caché envía a un elemento StorageTarget determinado.|StorageTarget|
|StorageTargetWriteIOPS|StorageTarget Write IOPS (IOPS de escritura de StorageTarget)|Count|Average|Tasa de operaciones de escritura de archivos que la caché envía a un elemento StorageTarget determinado.|StorageTarget|
|StorageTargetAsyncWriteThroughput|StorageTarget Asynchronous Write Throughput (Rendimiento de escritura asincrónica de StorageTarget)|BytesPerSecond|Average|Velocidad con la que la caché escribe datos de forma asincrónica en un elemento StorageTarget determinado. Se trata de escrituras oportunistas que no hacen que los clientes se bloqueen.|StorageTarget|
|StorageTargetSyncWriteThroughput|StorageTarget Synchronous Write Throughput (Rendimiento de escritura sincrónica de StorageTarget)|BytesPerSecond|Average|Velocidad con la que la caché escribe datos de forma sincrónica en un elemento StorageTarget determinado. Se trata de escrituras que hacen que los clientes se bloqueen.|StorageTarget|
|StorageTargetTotalWriteThroughput|StorageTarget Total Write Throughput (Rendimiento de escritura total de StorageTarget)|BytesPerSecond|Average|Velocidad total con la que la caché escribe datos en un elemento StorageTarget determinado.|StorageTarget|
|StorageTargetLatency|StorageTarget Latency (Latencia de StorageTarget)|Milisegundos|Average|Latencia de ida y vuelta media de todas las operaciones de archivos que la caché envía a un elemento StorageTarget determinado.|StorageTarget|
|StorageTargetMetadataReadIOPS|StorageTarget Metadata Read IOPS (IOPS de lectura de metadatos de StorageTarget)|CountPerSecond|Average|Tasa de operaciones de archivos que no modifican el estado persistente y excluyen la operación de lectura, y que la caché envía a un elemento StorageTarget determinado.|StorageTarget|
|StorageTargetMetadataWriteIOPS|StorageTarget Metadata Write IOPS (IOPS de escritura de metadatos de StorageTarget)|CountPerSecond|Average|Tasa de operaciones de archivos que modifican el estado persistente y excluyen la operación de escritura, y que la caché envía a un elemento StorageTarget determinado.|StorageTarget|
|StorageTargetReadIOPS|StorageTarget Read IOPS (IOPS de lectura de StorageTarget)|CountPerSecond|Average|Tasa de operaciones de lectura de archivos que la caché envía a un elemento StorageTarget determinado.|StorageTarget|
|StorageTargetReadAheadThroughput|StorageTarget Read Ahead Throughput (Rendimiento de lectura anticipada de StorageTarget)|BytesPerSecond|Average|Velocidad con la que la caché lee datos de forma oportunista desde el elemento StorageTarget.|StorageTarget|
|StorageTargetFillThroughput|StorageTarget Fill Throughput (Rendimiento de relleno de StorageTarget)|BytesPerSecond|Average|Velocidad con la que la caché lee datos de StorageTarget para controlar un error de caché.|StorageTarget|
|StorageTargetTotalReadThroughput|StorageTarget Total Read Throughput (Rendimiento de lectura total de StorageTarget)|BytesPerSecond|Average|Velocidad total con la que la caché lee datos de un elemento StorageTarget determinado.|StorageTarget|

## <a name="microsoftstoragesyncstoragesyncservices"></a>microsoft.storagesync/storageSyncServices

|Métrica|Nombre de métrica para mostrar|Unidad|Tipo de agregación|Descripción|Dimensions|
|---|---|---|---|---|---|
|ServerSyncSessionResult|Resultado de la sesión de sincronización|Count|Average|Métrica que registra un valor de 1 cada vez que el punto de conexión del servidor completa correctamente una sesión de sincronización con el punto de conexión en la nube|SyncGroupName,ServerEndpointName,SyncDirection|
|StorageSyncSyncSessionAppliedFilesCount|Archivos sincronizados|Count|Total|Número de archivos sincronizados|SyncGroupName,ServerEndpointName,SyncDirection|
|StorageSyncSyncSessionPerItemErrorsCount|Archivos que no se están sincronizando|Count|Total|No se pudo sincronizar el recuento de archivos|SyncGroupName,ServerEndpointName,SyncDirection|
|StorageSyncBatchTransferredFileBytes|Bytes sincronizados|Bytes|Total|Tamaño total del archivo transferido para las sesiones de sincronización|SyncGroupName,ServerEndpointName,SyncDirection|
|StorageSyncServerHeartbeat|Estado en línea del servidor|Count|Máxima|Métrica que registra un valor de 1 cada vez que el servidor registrado registra correctamente un latido con el punto de conexión en la nube.|nombreDeServidor|
|StorageSyncRecallIOTotalSizeBytes|Recuperación de niveles de la nube|Bytes|Total|Tamaño total de datos recuperados por el servidor|nombreDeServidor|
|StorageSyncRecalledTotalNetworkBytes|Tamaño de recuperación de nube por niveles|Bytes|Total|El tamaño de los datos que se recuperan|SyncGroupName,ServerName|
|StorageSyncRecallThroughputBytesPerSecond|Rendimiento de recuperación de nube por niveles|BytesPerSecond|Average|Rendimiento del tamaño de recuperación de datos|SyncGroupName,ServerName|
|StorageSyncRecalledNetworkBytesByApplication|Tamaño de recuperación de nube por niveles por aplicación|Bytes|Total|Tamaño de los datos recuperados por aplicación|SyncGroupName,ServerName,ApplicationName|

## <a name="microsoftstoragesyncstoragesyncservicessyncgroups"></a>microsoft.storagesync/storageSyncServices/syncGroups

|Métrica|Nombre de métrica para mostrar|Unidad|Tipo de agregación|Descripción|Dimensions|
|---|---|---|---|---|---|
|SyncGroupSyncSessionAppliedFilesCount|Archivos sincronizados|Count|Total|Número de archivos sincronizados|SyncGroupName,ServerEndpointName,SyncDirection|
|SyncGroupSyncSessionPerItemErrorsCount|Archivos que no se están sincronizando|Count|Total|No se pudo sincronizar el recuento de archivos|SyncGroupName,ServerEndpointName,SyncDirection|
|SyncGroupBatchTransferredFileBytes|Bytes sincronizados|Bytes|Total|Tamaño total del archivo transferido para las sesiones de sincronización|SyncGroupName,ServerEndpointName,SyncDirection|

## <a name="microsoftstoragesyncstoragesyncservicessyncgroupsserverendpoints"></a>microsoft.storagesync/storageSyncServices/syncGroups/serverEndpoints

|Métrica|Nombre de métrica para mostrar|Unidad|Tipo de agregación|Descripción|Dimensions|
|---|---|---|---|---|---|
|ServerEndpointSyncSessionAppliedFilesCount|Archivos sincronizados|Count|Total|Número de archivos sincronizados|ServerEndpointName,SyncDirection|
|ServerEndpointSyncSessionPerItemErrorsCount|Archivos que no se están sincronizando|Count|Total|No se pudo sincronizar el recuento de archivos|ServerEndpointName,SyncDirection|
|ServerEndpointBatchTransferredFileBytes|Bytes sincronizados|Bytes|Total|Tamaño total del archivo transferido para las sesiones de sincronización|ServerEndpointName,SyncDirection|

## <a name="microsoftstoragesyncstoragesyncservicesregisteredservers"></a>microsoft.storagesync/storageSyncServices/registeredServers

|Métrica|Nombre de métrica para mostrar|Unidad|Tipo de agregación|Descripción|Dimensions|
|---|---|---|---|---|---|
|ServerHeartbeat|Estado en línea del servidor|Count|Máxima|Métrica que registra un valor de 1 cada vez que el servidor registrado registra correctamente un latido con el punto de conexión en la nube.|ServerResourceId,ServerName|
|ServerRecallIOTotalSizeBytes|Recuperación de niveles de la nube|Bytes|Total|Tamaño total de datos recuperados por el servidor|ServerResourceId,ServerName|



## <a name="microsoftstreamanalyticsstreamingjobs"></a>Microsoft.StreamAnalytics/streamingjobs

|Métrica|Nombre de métrica para mostrar|Unidad|Tipo de agregación|Descripción|Dimensions|
|---|---|---|---|---|---|
|ResourceUtilization|SU % uso|Percent|Máxima|SU % uso|LogicalName,PartitionId|
|InputEvents|Eventos de entrada|Count|Total|Eventos de entrada|LogicalName,PartitionId|
|InputEventBytes|Bytes del evento de entrada|Bytes|Total|Bytes del evento de entrada|LogicalName,PartitionId|
|LateInputEvents|Eventos de entrada retrasada|Count|Total|Eventos de entrada retrasada|LogicalName,PartitionId|
|OutputEvents|Eventos de salida|Count|Total|Eventos de salida|LogicalName,PartitionId|
|ConversionErrors|Errores de conversión de datos|Count|Total|Errores de conversión de datos|LogicalName,PartitionId|
|Errors|Errores de tiempo de ejecución|Count|Total|Errores de tiempo de ejecución|LogicalName,PartitionId|
|DroppedOrAdjustedEvents|Eventos de fuera de orden|Count|Total|Eventos de fuera de orden|LogicalName,PartitionId|
|AMLCalloutRequests|Solicitudes de función|Count|Total|Solicitudes de función|LogicalName,PartitionId|
|AMLCalloutFailedRequests|Solicitudes de función con errores|Count|Total|Solicitudes de función con errores|LogicalName,PartitionId|
|AMLCalloutInputEvents|Eventos de función|Count|Total|Eventos de función|LogicalName,PartitionId|
|DeserializationError|Errores de deserialización de entrada|Count|Total|Errores de deserialización de entrada|LogicalName,PartitionId|
|EarlyInputEvents|Primeros eventos de entrada|Count|Total|Primeros eventos de entrada|LogicalName,PartitionId|
|OutputWatermarkDelaySeconds|Retraso de la marca de agua|Segundos|Máxima|Retraso de la marca de agua|LogicalName,PartitionId|
|InputEventsSourcesBacklogged|Eventos de entrada pendientes|Count|Máxima|Eventos de entrada pendientes|LogicalName,PartitionId|
|InputEventsSourcesPerSecond|Orígenes de entrada recibidos|Count|Total|Orígenes de entrada recibidos|LogicalName,PartitionId|

## <a name="microsoftsynapseworkspaces"></a>Microsoft.Synapse/workspaces

|Métrica|Nombre de métrica para mostrar|Unidad|Tipo de agregación|Descripción|Dimensions|
|---|---|---|---|---|---|
|OrchestrationPipelineRunsEnded|Ejecuciones de canalización finalizadas|Count|Total|Recuento de ejecuciones de canalización de orquestación correctas, con error o canceladas|Result,FailureType,Pipeline|
|OrchestrationActivityRunsEnded|Ejecuciones de actividad finalizadas|Count|Total|Recuento de actividades de orquestación correctas, con error o canceladas|Result,FailureType,Activity,ActivityType,Pipeline|
|OrchestrationTriggersEnded|Desencadenadores finalizados|Count|Total|Recuento de desencadenadores de orquestación correctos, con error o cancelados|Result,FailureType,Trigger|
|SQLOnDemandLoginAttempts|Intentos de inicio de sesión|Count|Total|Recuento de intentos de inicio de sesión correctos o con error|Resultado|
|SQLOnDemandQueriesEnded|Consultas finalizadas|Count|Total|Recuento de consultas correctas, con error o canceladas|Resultado|
|SQLOnDemandQueryProcessedBytes|Datos procesados|Bytes|Total|Cantidad de datos procesados por consultas|None|

## <a name="microsoftsynapseworkspacesbigdatapools"></a>Microsoft.Synapse/workspaces/bigDataPools

|Métrica|Nombre de métrica para mostrar|Unidad|Tipo de agregación|Descripción|Dimensions|
|---|---|---|---|---|---|
|SparkJobsEnded|Aplicaciones finalizadas|Count|Total|Recuento de aplicaciones finalizadas|JobType,JobResult|
|CoresCapacity|Capacidad de núcleos|Count|Máxima|Capacidad de núcleos|None|
|MemoryCapacityGB|Capacidad de memoria (GB)|Count|Máxima|Capacidad de memoria (GB)|None|

## <a name="microsoftsynapseworkspacessqlpools"></a>Microsoft.Synapse/workspaces/sqlPools

|Métrica|Nombre de métrica para mostrar|Unidad|Tipo de agregación|Descripción|Dimensions|
|---|---|---|---|---|---|
|DWULimit|Límite de DWU|Count|Máxima|Objetivo de nivel de servicio del grupo de SQL|None|
|DWUUsed|DWU utilizada|Count|Máxima|Representa una representación de alto nivel del uso en el grupo de SQL. Medido por el límite de DWU * porcentaje de DWU|None|
|DWUUsedPercent|Porcentaje usado de DWU|Percent|Máxima|Representa una representación de alto nivel del uso en el grupo de SQL. Medido tomando el máximo entre el porcentaje de CPU y el porcentaje de E/S de datos|None|
|ConnectionsBlockedByFirewall|Conexiones bloqueadas por el firewall|Count|Total|Recuento de conexiones bloqueadas por las reglas de firewall. Vuelva a visitar las directivas de control de acceso para el grupo de SQL y supervise estas conexiones si el recuento es alto.|None|
|AdaptiveCacheHitPercent|Porcentaje de aciertos de caché adaptable|Percent|Máxima|Mide en qué grado las cargas de trabajo usan la caché adaptable. Use esta métrica con la métrica del porcentaje de aciertos de caché para determinar si se debe escalar para aumentar la capacidad o volver a ejecutar cargas de trabajo para hidratar la memoria caché.|None|
|AdaptiveCacheUsedPercent|Porcentaje usado de caché adaptable|Percent|Máxima|Mide en qué grado las cargas de trabajo usan la caché adaptable. Use esta métrica con la métrica del porcentaje usado de caché para determinar si se debe escalar para aumentar la capacidad o volver a ejecutar cargas de trabajo para hidratar la memoria caché.|None|
|LocalTempDBUsedPercent|Porcentaje usado de tempdb local|Percent|Máxima|El uso de tempdb local en todos los nodos de ejecución. Los valores se emiten cada cinco minutos.|None|
|MemoryUsedPercent|Porcentaje utilizado de memoria.|Percent|Máxima|Uso de memoria en todos los nodos del grupo de SQL|None|
|Conexiones|Conexiones|Count|Total|Recuento total de inicios de sesión en el grupo de SQL|Resultado|
|WLGActiveQueries|Consultas activas de grupo de cargas de trabajo|Count|Total|Consultas activas dentro del grupo de cargas de trabajo. El uso de esta métrica sin filtrar y sin dividir muestra todas las consultas activas que se ejecutan en el sistema.|IsUserDefined,WorkloadGroup|
|WLGActiveQueriesTimeouts|Tiempos de espera de consultas de grupo de cargas de trabajo|Count|Total|Consultas para el grupo de cargas de trabajo que han agotado el tiempo de espera. Los tiempos de espera de consulta que indica esta métrica son solo una vez que se ha iniciado la ejecución de la consulta (no incluye el tiempo de espera debido a las esperas de bloqueo o de recursos).|IsUserDefined,WorkloadGroup|
|WLGAllocationBySystemPercent|Asignación de grupos de cargas de trabajo por porcentaje del sistema|Percent|Máxima|El porcentaje de asignación de recursos en relación con todo el sistema.|IsUserDefined,WorkloadGroup|
|WLGAllocationByMaxResourcePercent|Asignación de grupos de cargas de trabajo por porcentaje máximo de recursos|Percent|Máxima|Muestra el porcentaje de asignación de recursos en relación con la métrica Porcentaje máximo de recursos efectivo por grupo de cargas de trabajo. Esta métrica proporciona la utilización efectiva del grupo de cargas de trabajo.|IsUserDefined,WorkloadGroup|
|WLGEffectiveCapResourcePercent|Porcentaje máximo de recursos efectivo|Percent|Máxima|Porcentaje máximo de recursos efectivo para el grupo de cargas de trabajo. Si hay otros grupos de cargas de trabajo con min_percentage_resource > 0, effective_cap_percentage_resource se reduce proporcionalmente|IsUserDefined,WorkloadGroup|
|wlg_effective_min_resource_percent|Porcentaje mínimo de recursos efectivo|Percent|Mínima|La configuración de porcentaje de recursos mínimos efectivos permitida teniendo en cuenta el nivel de servicio y la configuración del grupo de cargas de trabajo. El valor min_percentage_resource efectivo se puede ajustar en niveles de servicio superiores o inferiores.|IsUserDefined,WorkloadGroup|
|WLGQueuedQueries|Consultas en cola del grupo de cargas de trabajo|Count|Total|El recuento acumulado de solicitudes en cola una vez alcanzado el límite de simultaneidad máximo.|IsUserDefined,WorkloadGroup|

## <a name="microsofttimeseriesinsightsenvironments"></a>Microsoft.TimeSeriesInsights/environments

|Métrica|Nombre de métrica para mostrar|Unidad|Tipo de agregación|Descripción|Dimensions|
|---|---|---|---|---|---|
|IngressReceivedMessages|Mensajes de entrada recibidos|Count|Total|Número de mensajes leídos desde todos los orígenes de eventos de Event Hubs o IoT Hub|None|
|IngressReceivedInvalidMessages|Mensajes no válidos de entrada recibidos|Count|Total|Número de mensajes no válidos leídos desde todos los orígenes de eventos de Event Hubs o IoT Hub|None|
|IngressReceivedBytes|Bytes de entrada recibidos|Bytes|Total|Número de bytes leídos desde todos los orígenes de eventos|None|
|IngressStoredBytes|Bytes de entrada almacenados|Bytes|Total|Tamaño total de los eventos almacenados correctamente y disponibles para su consulta|None|
|IngressStoredEvents|Eventos de entrada almacenados|Count|Total|Número de eventos acoplados que se procesaron correctamente y que están disponibles para su consulta|None|
|IngressReceivedMessagesTimeLag|Tiempo de retardo de los mensajes de entrada recibidos|Segundos|Máxima|Diferencia entre el momento en el que el mensaje se pone en la cola en el origen del evento y el momento en que se procesa en la entrada|None|
|IngressReceivedMessagesCountLag|Intervalo de recuento de los mensajes de entrada recibidos|Count|Average|Diferencia entre el número de secuencia del último mensaje en cola en la partición del origen del evento y el del mensaje que se está procesando en la entrada.|None|
|WarmStorageMaxProperties|Propiedades máximas del almacenamiento intermedio|Count|Máxima|Número máximo de propiedades utilizadas que permite el entorno para la SKU S1/S2 y número máximo de propiedades admitido por el almacenamiento intermedio para la SKU de pago por uso|None|
|WarmStorageUsedProperties|Propiedades utilizadas de almacenamiento intermedio |Count|Máxima|Número de propiedades utilizadas por el entorno para la SKU S1/S2 y número de propiedades que utiliza el almacenamiento intermedio para la SKU de pago por uso|None|



## <a name="microsofttimeseriesinsightsenvironmentseventsources"></a>Microsoft.TimeSeriesInsights/environments/eventsources

|Métrica|Nombre de métrica para mostrar|Unidad|Tipo de agregación|Descripción|Dimensions|
|---|---|---|---|---|---|
|IngressReceivedMessages|Mensajes de entrada recibidos|Count|Total|Número de mensajes leídos desde el origen del evento|None|
|IngressReceivedInvalidMessages|Mensajes no válidos de entrada recibidos|Count|Total|Número de mensajes leídos no válidos desde el origen del evento|None|
|IngressReceivedBytes|Bytes de entrada recibidos|Bytes|Total|Número de bytes leídos desde el origen del evento|None|
|IngressStoredBytes|Bytes de entrada almacenados|Bytes|Total|Tamaño total de los eventos almacenados correctamente y disponibles para su consulta|None|
|IngressStoredEvents|Eventos de entrada almacenados|Count|Total|Número de eventos acoplados que se procesaron correctamente y que están disponibles para su consulta|None|
|IngressReceivedMessagesTimeLag|Tiempo de retardo de los mensajes de entrada recibidos|Segundos|Máxima|Diferencia entre el momento en el que el mensaje se pone en la cola en el origen del evento y el momento en que se procesa en la entrada|None|
|IngressReceivedMessagesCountLag|Intervalo de recuento de los mensajes de entrada recibidos|Count|Average|Diferencia entre el número de secuencia del último mensaje en cola en la partición del origen del evento y el del mensaje que se está procesando en la entrada.|None|
|WarmStorageMaxProperties|Propiedades máximas del almacenamiento intermedio|Count|Máxima|Número máximo de propiedades utilizadas que permite el entorno para la SKU S1/S2 y número máximo de propiedades admitido por el almacenamiento intermedio para la SKU de pago por uso|None|
|WarmStorageUsedProperties|Propiedades utilizadas de almacenamiento intermedio |Count|Máxima|Número de propiedades utilizadas por el entorno para la SKU S1/S2 y número de propiedades que utiliza el almacenamiento intermedio para la SKU de pago por uso|None|

## <a name="microsoftvmwarecloudsimplevirtualmachines"></a>Microsoft.VMwareCloudSimple/virtualMachines

|Métrica|Nombre de métrica para mostrar|Unidad|Tipo de agregación|Descripción|Dimensions|
|---|---|---|---|---|---|
|DiskReadBytesPerSecond|Disk Read Bytes/Sec|BytesPerSecond|Average|Rendimiento de disco promedio debido a las operaciones de lectura durante el período de ejemplo.|None|
|DiskWriteBytesPerSecond|Disk Write Bytes/Sec|BytesPerSecond|Average|Rendimiento de disco promedio debido a las operaciones de escritura durante el período de ejemplo.|None|
|Bytes de lectura de disco|Bytes de lectura de disco|Bytes|Total|Rendimiento de disco total debido a las operaciones de lectura durante el período de ejemplo.|None|
|Bytes de escritura de disco|Bytes de escritura de disco|Bytes|Total|Rendimiento de disco total debido a las operaciones de escritura durante el período de ejemplo.|None|
|DiskReadOperations|Operaciones de lectura en disco|Count|Total|El número de operaciones de lectura de E/S en el período de ejemplo anterior. Tenga en cuenta que estas operaciones pueden ser de tamaño variable.|None|
|DiskWriteOperations|Operaciones de escritura en disco|Count|Total|El número de operaciones de escritura de E/S en el período de ejemplo anterior. Tenga en cuenta que estas operaciones pueden ser de tamaño variable.|None|
|Operaciones de lectura de disco por segundo|Operaciones de lectura de disco por segundo|CountPerSecond|Average|El número promedio de operaciones de lectura de E/S en el período de ejemplo anterior. Tenga en cuenta que estas operaciones pueden ser de tamaño variable.|None|
|Operaciones de escritura por segundo en disco|Operaciones de escritura por segundo en disco|CountPerSecond|Average|El número promedio de operaciones de escritura de E/S en el período de ejemplo anterior. Tenga en cuenta que estas operaciones pueden ser de tamaño variable.|None|
|DiskReadLatency|Latencia de lectura en disco|Milisegundos|Average|Latencia de lectura total. La suma de las latencias de lectura del kernel y el dispositivo.|None|
|DiskWriteLatency|Latencia de escritura en disco|Milisegundos|Average|Latencia de escritura total. La suma de las latencias de escritura del kernel y el dispositivo.|None|
|NetworkInBytesPerSecond|Red en bytes/seg|BytesPerSecond|Average|Rendimiento de red promedio para el tráfico recibido.|None|
|NetworkOutBytesPerSecond|Bytes/seg de red externa|BytesPerSecond|Average|Rendimiento de red promedio para el tráfico transmitido.|None|
|Red interna|Red interna|Bytes|Total|Rendimiento de red total para el tráfico recibido.|None|
|Red interna|Red interna|Bytes|Total|Rendimiento de red total para el tráfico transmitido.|None|
|MemoryUsed|Memoria usada|Bytes|Average|La cantidad de memoria de la máquina que está usando la máquina virtual.|None|
|MemoryGranted|Memoria concedida|Bytes|Average|La cantidad de memoria que concedió el host a la máquina virtual. No se concede memoria al host hasta que se toque una vez, y la memoria concedida se puede intercambiar o aumentar en caso de que el kernel de la VM necesite la memoria.|None|
|MemoryActive|Memoria activa|Bytes|Average|La cantidad de memoria utilizada por la máquina virtual en el último período de tiempo reducido. Esta es la cantidad de memoria "verdadera" que requiere la máquina virtual. La memoria adicional que no se utiliza se puede intercambiar o aumentar sin afectar al rendimiento del invitado.|None|
|Porcentaje de CPU|Porcentaje de CPU|Percent|Average|El uso de la CPU. Este valor se notifica con el 100 %, que representa todos los núcleos de procesador del sistema. Por ejemplo, una máquina virtual bidireccional que use el 50 % de un sistema de cuatro núcleos estará usando por completo dos núcleos.|None|
|PercentageCpuReady|Preparación de la CPU en porcentaje|Milisegundos|Total|El tiempo de preparación es el tiempo que se tuvo que esperar a que las CPU estuvieran disponibles en el último intervalo de actualización.|None|

## <a name="microsoftwebserverfarms"></a>Microsoft.Web/serverfarms

|Métrica|Nombre de métrica para mostrar|Unidad|Tipo de agregación|Descripción|Dimensions|
|---|---|---|---|---|---|
|CpuPercentage|Porcentaje de CPU|Percent|Average|Porcentaje de CPU|Instancia|
|MemoryPercentage|Porcentaje de memoria|Percent|Average|Porcentaje de memoria|Instancia|
|DiskQueueLength|Longitud de la cola de disco|Count|Average|Longitud de la cola de disco|Instancia|
|HttpQueueLength|Longitud de la cola HTTP|Count|Average|Longitud de la cola HTTP|Instancia|
|BytesReceived|Entrada de datos|Bytes|Total|Entrada de datos|Instancia|
|BytesSent|Salida de datos|Bytes|Total|Salida de datos|Instancia|
|TcpSynSent|TCP Syn Sent|Count|Average|TCP Syn Sent|Instancia|
|TcpSynReceived|TCP Syn Received|Count|Average|TCP Syn Received|Instancia|
|TcpEstablished|TCP Established|Count|Average|TCP Established|Instancia|
|TcpFinWait1|TCP Fin Wait 1|Count|Average|TCP Fin Wait 1|Instancia|
|TcpFinWait2|TCP Fin Wait 2|Count|Average|TCP Fin Wait 2|Instancia|
|TcpClosing|TCP Closing (Cierre de TCP)|Count|Average|TCP Closing|Instancia|
|TcpCloseWait|TCP Close Wait|Count|Average|TCP Close Wait|Instancia|
|TcpLastAck|TCP Last Ack|Count|Average|TCP Last Ack|Instancia|
|TcpTimeWait|TCP Time Wait|Count|Average|TCP Time Wait|Instancia|

## <a name="microsoftwebsites-excluding-functions"></a>Microsoft.Web/sites (se excluye Functions) 

> [!NOTE]
> **El uso del sistema de archivos** es una nueva métrica que se está implementando globalmente; no se espera ningún dato a menos que se haya incluido en la lista blanca de la versión preliminar privada.

> [!IMPORTANT]
> **Tiempo medio de respuesta** pasará a estar en desuso para evitar confusiones con las agregaciones de métricas. Use **Tiempo de respuesta** a modo de reemplazo.

|Métrica|Nombre de métrica para mostrar|Unidad|Tipo de agregación|Descripción|Dimensions|
|---|---|---|---|---|---|
|CpuTime|Tiempo de CPU|Segundos|Total|Tiempo de CPU|Instancia|
|Requests|Requests|Count|Total|Requests|Instancia|
|BytesReceived|Entrada de datos|Bytes|Total|Entrada de datos|Instancia|
|BytesSent|Salida de datos|Bytes|Total|Salida de datos|Instancia|
|Http101|Http 101|Count|Total|Http 101|Instancia|
|Http2xx|Http 2xx|Count|Total|Http 2xx|Instancia|
|Http3xx|Http 3xx|Count|Total|Http 3xx|Instancia|
|Http401|Http 401|Count|Total|Http 401|Instancia|
|Http403|Http 403|Count|Total|Http 403|Instancia|
|Http404|Http 404|Count|Total|Http 404|Instancia|
|Http406|Http 406|Count|Total|Http 406|Instancia|
|Http4xx|Http 4xx|Count|Total|Http 4xx|Instancia|
|Http5xx|Errores de servidor HTTP|Count|Total|Errores de servidor HTTP|Instancia|
|MemoryWorkingSet|Espacio de trabajo de memoria|Bytes|Average|Espacio de trabajo de memoria|Instancia|
|AverageMemoryWorkingSet|Espacio de trabajo de memoria promedio|Bytes|Average|Espacio de trabajo de memoria promedio|Instancia|
|HttpResponseTime|Tiempo de respuesta|Segundos|Total|Tiempo de respuesta|Instancia|
|AverageResponseTime|Tiempo medio de respuesta (en desuso)|Segundos|Average|Tiempo de respuesta promedio|Instancia|
|AppConnections|Conexiones|Count|Average|Conexiones|Instancia|
|Asas|Recuento de identificadores|Count|Average|Recuento de identificadores|Instancia|
|Subprocesos|Número de subprocesos|Count|Average|Número de subprocesos|Instancia|
|PrivateBytes|Bytes privados|Bytes|Average|Bytes privados|Instancia|
|IoReadBytesPerSecond|Bytes de lectura de E/S por segundo|BytesPerSecond|Total|Bytes de lectura de E/S por segundo|Instancia|
|IoReadBytesPerSecond|Bytes de escritura de E/S por segundo|BytesPerSecond|Total|Bytes de escritura de E/S por segundo|Instancia|
|IoOtherBytesPerSecond|Otros bytes de E/S por segundo|BytesPerSecond|Total|Otros bytes de E/S por segundo|Instancia|
|IoReadOperationsPerSecond|Operaciones de lectura de E/S por segundo|BytesPerSecond|Total|Operaciones de lectura de E/S por segundo|Instancia|
|IoWriteOperationsPerSecond|Operaciones de escritura de E/S por segundo|BytesPerSecond|Total|Operaciones de escritura de E/S por segundo|Instancia|
|IoOtherOperationsPerSecond|Otras operaciones de E/S por segundo|BytesPerSecond|Total|Otras operaciones de E/S por segundo|Instancia|
|RequestsInApplicationQueue|Solicitudes en cola de la aplicación|Count|Average|Solicitudes en cola de la aplicación|Instancia|
|CurrentAssemblies|Ensamblados actuales|Count|Average|Ensamblados actuales|Instancia|
|TotalAppDomains|Dominios de aplicación totales|Count|Average|Dominios de aplicación totales|Instancia|
|TotalAppDomainsUnloaded|Dominios de aplicación totales descargados|Count|Average|Dominios de aplicación totales descargados|Instancia|
|Gen0Collections|Recolección de elementos no utilizados de Gen 0|Count|Total|Recolección de elementos no utilizados de Gen 0|Instancia|
|Gen1Collections|Recolección de elementos no utilizados de Gen 1|Count|Total|Recolección de elementos no utilizados de Gen 1|Instancia|
|Gen2Collections|Recolección de elementos no utilizados de Gen 2|Count|Total|Recolección de elementos no utilizados de Gen 2|Instancia|
|HealthCheckStatus|Estado de comprobación de estado|Count|Average|Estado de comprobación de estado|Instancia|
|FileSystemUsage|Uso del sistema de archivos|Bytes|Average|Uso del sistema de archivos|None|

## <a name="microsoftwebsites-functions"></a>Microsoft.Web/sites (Functions)

> [!NOTE]
> **El uso del sistema de archivos** es una nueva métrica que se está implementando globalmente; no se espera ningún dato a menos que se haya incluido en la lista blanca de la versión preliminar privada.

|Métrica|Nombre de métrica para mostrar|Unidad|Tipo de agregación|Descripción|Dimensions|
|---|---|---|---|---|---|
|BytesReceived|Entrada de datos|Bytes|Total|Entrada de datos|Instancia|
|BytesSent|Salida de datos|Bytes|Total|Salida de datos|Instancia|
|Http5xx|Errores de servidor HTTP|Count|Total|Errores de servidor HTTP|Instancia|
|MemoryWorkingSet|Espacio de trabajo de memoria|Bytes|Average|Espacio de trabajo de memoria|Instancia|
|AverageMemoryWorkingSet|Espacio de trabajo de memoria promedio|Bytes|Average|Espacio de trabajo de memoria promedio|Instancia|
|FunctionExecutionUnits|Unidades de ejecución de función|MB/milisegundos|Total|[Unidades de ejecución de función](https://github.com/Azure/Azure-Functions/wiki/Consumption-Plan-Cost-Billing-FAQ#how-can-i-view-graphs-of-execution-count-and-gb-seconds)|Instancia|
|FunctionExecutionCount|Recuento de ejecución de funciones|Count|Total|Recuento de ejecución de funciones|Instancia|
|PrivateBytes|Bytes privados|Bytes|Average|Bytes privados|Instancia|
|IoReadBytesPerSecond|Bytes de lectura de E/S por segundo|BytesPerSecond|Total|Bytes de lectura de E/S por segundo|Instancia|
|IoReadBytesPerSecond|Bytes de escritura de E/S por segundo|BytesPerSecond|Total|Bytes de escritura de E/S por segundo|Instancia|
|IoOtherBytesPerSecond|Otros bytes de E/S por segundo|BytesPerSecond|Total|Otros bytes de E/S por segundo|Instancia|
|IoReadOperationsPerSecond|Operaciones de lectura de E/S por segundo|BytesPerSecond|Total|Operaciones de lectura de E/S por segundo|Instancia|
|IoWriteOperationsPerSecond|Operaciones de escritura de E/S por segundo|BytesPerSecond|Total|Operaciones de escritura de E/S por segundo|Instancia|
|IoOtherOperationsPerSecond|Otras operaciones de E/S por segundo|BytesPerSecond|Total|Otras operaciones de E/S por segundo|Instancia|
|RequestsInApplicationQueue|Solicitudes en cola de la aplicación|Count|Average|Solicitudes en cola de la aplicación|Instancia|
|CurrentAssemblies|Ensamblados actuales|Count|Average|Ensamblados actuales|Instancia|
|TotalAppDomains|Dominios de aplicación totales|Count|Average|Dominios de aplicación totales|Instancia|
|TotalAppDomainsUnloaded|Dominios de aplicación totales descargados|Count|Average|Dominios de aplicación totales descargados|Instancia|
|Gen0Collections|Recolección de elementos no utilizados de Gen 0|Count|Total|Recolección de elementos no utilizados de Gen 0|Instancia|
|Gen1Collections|Recolección de elementos no utilizados de Gen 1|Count|Total|Recolección de elementos no utilizados de Gen 1|Instancia|
|Gen2Collections|Recolección de elementos no utilizados de Gen 2|Count|Total|Recolección de elementos no utilizados de Gen 2|Instancia|
|HealthCheckStatus|Estado de comprobación de estado|Count|Average|Estado de comprobación de estado|Instancia|
|FileSystemUsage|Uso del sistema de archivos|Bytes|Average|Uso del sistema de archivos|None|

## <a name="microsoftwebsitesslots"></a>Microsoft.Web/sites/slots

|Métrica|Nombre de métrica para mostrar|Unidad|Tipo de agregación|Descripción|Dimensions|
|---|---|---|---|---|---|
|CpuTime|Tiempo de CPU|Segundos|Total|Tiempo de CPU|Instancia|
|Requests|Requests|Count|Total|Requests|Instancia|
|BytesReceived|Entrada de datos|Bytes|Total|Entrada de datos|Instancia|
|BytesSent|Salida de datos|Bytes|Total|Salida de datos|Instancia|
|Http101|Http 101|Count|Total|Http 101|Instancia|
|Http2xx|Http 2xx|Count|Total|Http 2xx|Instancia|
|Http3xx|Http 3xx|Count|Total|Http 3xx|Instancia|
|Http401|Http 401|Count|Total|Http 401|Instancia|
|Http403|Http 403|Count|Total|Http 403|Instancia|
|Http404|Http 404|Count|Total|Http 404|Instancia|
|Http406|Http 406|Count|Total|Http 406|Instancia|
|Http4xx|Http 4xx|Count|Total|Http 4xx|Instancia|
|Http5xx|Errores de servidor HTTP|Count|Total|Errores de servidor HTTP|Instancia|
|MemoryWorkingSet|Espacio de trabajo de memoria|Bytes|Average|Espacio de trabajo de memoria|Instancia|
|AverageMemoryWorkingSet|Espacio de trabajo de memoria promedio|Bytes|Average|Espacio de trabajo de memoria promedio|Instancia|
|AverageResponseTime|Tiempo de respuesta promedio|Segundos|Average|Tiempo de respuesta promedio|Instancia|
|HttpResponseTime|Tiempo de respuesta|Segundos|Average|Tiempo de respuesta|Instancia|
|FunctionExecutionUnits|Unidades de ejecución de función|Count|Total|Unidades de ejecución de función|Instancia|
|FunctionExecutionCount|Recuento de ejecución de funciones|Count|Total|Recuento de ejecución de funciones|Instancia|
|AppConnections|Conexiones|Count|Average|Conexiones|Instancia|
|Asas|Recuento de identificadores|Count|Average|Recuento de identificadores|Instancia|
|Subprocesos|Número de subprocesos|Count|Average|Número de subprocesos|Instancia|
|PrivateBytes|Bytes privados|Bytes|Average|Bytes privados|Instancia|
|IoReadBytesPerSecond|Bytes de lectura de E/S por segundo|BytesPerSecond|Total|Bytes de lectura de E/S por segundo|Instancia|
|IoReadBytesPerSecond|Bytes de escritura de E/S por segundo|BytesPerSecond|Total|Bytes de escritura de E/S por segundo|Instancia|
|IoOtherBytesPerSecond|Otros bytes de E/S por segundo|BytesPerSecond|Total|Otros bytes de E/S por segundo|Instancia|
|IoReadOperationsPerSecond|Operaciones de lectura de E/S por segundo|BytesPerSecond|Total|Operaciones de lectura de E/S por segundo|Instancia|
|IoWriteOperationsPerSecond|Operaciones de escritura de E/S por segundo|BytesPerSecond|Total|Operaciones de escritura de E/S por segundo|Instancia|
|IoOtherOperationsPerSecond|Otras operaciones de E/S por segundo|BytesPerSecond|Total|Otras operaciones de E/S por segundo|Instancia|
|RequestsInApplicationQueue|Solicitudes en cola de la aplicación|Count|Average|Solicitudes en cola de la aplicación|Instancia|
|CurrentAssemblies|Ensamblados actuales|Count|Average|Ensamblados actuales|Instancia|
|TotalAppDomains|Dominios de aplicación totales|Count|Average|Dominios de aplicación totales|Instancia|
|TotalAppDomainsUnloaded|Dominios de aplicación totales descargados|Count|Average|Dominios de aplicación totales descargados|Instancia|
|Gen0Collections|Recolección de elementos no utilizados de Gen 0|Count|Total|Recolección de elementos no utilizados de Gen 0|Instancia|
|Gen1Collections|Recolección de elementos no utilizados de Gen 1|Count|Total|Recolección de elementos no utilizados de Gen 1|Instancia|
|Gen2Collections|Recolección de elementos no utilizados de Gen 2|Count|Total|Recolección de elementos no utilizados de Gen 2|Instancia|
|HealthCheckStatus|Estado de comprobación de estado|Count|Average|Estado de comprobación de estado|Instancia|
|FileSystemUsage|Uso del sistema de archivos|Bytes|Average|Uso del sistema de archivos|None|

## <a name="microsoftwebhostingenvironmentsmultirolepools"></a>Microsoft.Web/hostingEnvironments/multiRolePools

|Métrica|Nombre de métrica para mostrar|Unidad|Tipo de agregación|Descripción|Dimensions|
|---|---|---|---|---|---|
|Requests|Requests|Count|Total|Requests|Instancia|
|BytesReceived|Entrada de datos|Bytes|Total|Entrada de datos|Instancia|
|BytesSent|Salida de datos|Bytes|Total|Salida de datos|Instancia|
|Http101|Http 101|Count|Total|Http 101|Instancia|
|Http2xx|Http 2xx|Count|Total|Http 2xx|Instancia|
|Http3xx|Http 3xx|Count|Total|Http 3xx|Instancia|
|Http401|Http 401|Count|Total|Http 401|Instancia|
|Http403|Http 403|Count|Total|Http 403|Instancia|
|Http404|Http 404|Count|Total|Http 404|Instancia|
|Http406|Http 406|Count|Total|Http 406|Instancia|
|Http4xx|Http 4xx|Count|Total|Http 4xx|Instancia|
|Http5xx|Errores de servidor HTTP|Count|Total|Errores de servidor HTTP|Instancia|
|AverageResponseTime|Tiempo de respuesta promedio|Segundos|Average|Tiempo de respuesta promedio|Instancia|
|CpuPercentage|Porcentaje de CPU|Percent|Average|Porcentaje de CPU|Instancia|
|MemoryPercentage|Porcentaje de memoria|Percent|Average|Porcentaje de memoria|Instancia|
|DiskQueueLength|Longitud de la cola de disco|Count|Average|Longitud de la cola de disco|Instancia|
|HttpQueueLength|Longitud de la cola HTTP|Count|Average|Longitud de la cola HTTP|Instancia|
|ActiveRequests|Solicitudes activas|Count|Total|Solicitudes activas|Instancia|
|TotalFrontEnds|Número total de front-ends|Count|Average|Número total de front-ends|None|
|SmallAppServicePlanInstances|Trabajos pequeños del plan de App Service|Count|Average|Trabajos pequeños del plan de App Service|None|
|MediumAppServicePlanInstances|Trabajos medianos del plan de App Service|Count|Average|Trabajos medianos del plan de App Service|None|
|LargeAppServicePlanInstances|Trabajos grandes del plan de App Service|Count|Average|Trabajos grandes del plan de App Service|None|

## <a name="microsoftwebhostingenvironmentsworkerpools"></a>Microsoft.Web/hostingEnvironments/workerPools

|Métrica|Nombre de métrica para mostrar|Unidad|Tipo de agregación|Descripción|Dimensions|
|---|---|---|---|---|---|
|WorkersTotal|Número total de trabajos|Count|Average|Número total de trabajos|None|
|WorkersAvailable|Trabajos disponibles|Count|Average|Trabajos disponibles|None|
|WorkersUsed|Trabajos usados|Count|Average|Trabajos usados|None|
|CpuPercentage|Porcentaje de CPU|Percent|Average|Porcentaje de CPU|Instancia|
|MemoryPercentage|Porcentaje de memoria|Percent|Average|Porcentaje de memoria|Instancia|
## <a name="next-steps"></a>Pasos siguientes
* [Lea información sobre las métricas en Azure Monitor](data-platform.md)
* [Creación de alertas basadas en métricas](alerts-overview.md)
* [Exportación de métricas a cuentas de almacenamiento, Event Hubs o Log Analytics](platform-logs-overview.md)
