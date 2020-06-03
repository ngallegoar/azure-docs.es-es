---
title: Métricas de plataforma de Azure Monitor que pueden exportarse con la configuración de diagnóstico
description: Lista de métricas disponibles para cada tipo de recurso con Azure Monitor.
services: azure-monitor
ms.topic: reference
ms.date: 03/30/2020
ms.subservice: metrics
ms.openlocfilehash: 91fc2c4525ee622064520b0098087d54158bbe9e
ms.sourcegitcommit: 50673ecc5bf8b443491b763b5f287dde046fdd31
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/20/2020
ms.locfileid: "83680691"
---
# <a name="azure-monitor-platform-metrics-exportable-via-diagnostic-settings"></a>Métricas de plataforma de Azure Monitor que pueden exportarse con la configuración de diagnóstico

De forma predeterminada, Azure Monitor proporciona [métricas de plataforma](data-platform-metrics.md) sin ninguna configuración. Existen diferentes métodos para interactuar con las métricas de plataforma; por ejemplo, se pueden representar en gráficos en el portal, se puede acceder a ellas utilizando la API REST o se pueden consultar con PowerShell o la CLI. Consulte en [Métricas compatibles con Azure Monitor](metrics-supported.md) una lista completa de las métricas de plataforma que actualmente están disponibles con la canalización de métricas consolidada de Azure Monitor. Para consultar estas métricas y acceder a ellas, use la [versión de la API 01-01-2018](https://docs.microsoft.com/rest/api/monitor/metricdefinitions). Otras métricas pueden estar disponibles en el portal o mediante las API heredadas.

Existen dos formas de exportar las métricas de plataforma de la canalización de Azure Monitor a otras ubicaciones:
1. Utilizando la [configuración de diagnóstico](diagnostic-settings.md) para hacerlas llegar a Log Analytics, Event Hubs o Azure Storage.
2. Utilizando la [API REST de métricas](https://docs.microsoft.com/rest/api/monitor/metrics/list).

Debido a las complejidades del back-end de Azure Monitor, no todas las métricas pueden exportarse con la configuración de diagnóstico. En la tabla siguiente, se indican las métricas que pueden y que no pueden exportarse con la configuración de diagnóstico.

## <a name="change-to-behavior-for-nulls-and-zero-values"></a>Cambio en el comportamiento de valores NULL y cero 
 
En el caso de las métricas de la plataforma que se pueden exportar a través de la configuración de diagnóstico, hay algunas métricas para las que Azure Monitor interpreta los 0 como valores Null. Esto ha provocado cierta confusión entre los 0 reales (emitidos por el recurso) y los 0 interpretados (valores NULL). Próximamente se producirá un cambio y las métricas de la plataforma exportadas a través de la configuración de diagnóstico ya no exportarán los 0, a menos que el recurso subyacente los haya emitido realmente. 

> [!CAUTION]
> El cambio en el comportamiento descrito anteriormente está programado para que se produzca el 1 de junio de 2020.

Tenga en cuenta lo siguiente:

1.  Si elimina un grupo de recursos o un recurso específico, los datos de métricas de los recursos afectados ya no se enviarán a los destinos de exportación de la configuración de diagnóstico. Es decir, ya no aparecerán en Event Hubs, cuentas de almacenamiento y áreas de trabajo de Log Analytics.
2.  Esta mejora estará disponible en todas las nubes públicas y privadas.
3.  Este cambio no afectará al comportamiento de ninguna de las experiencias siguientes: 
   - Registros de recursos de la plataforma exportados a través de la configuración de diagnóstico
   - Creación de gráficos de métricas en el Explorador de métricas
   - Alertas en las métricas de la plataforma
 
## <a name="metrics-exportable-table"></a>Tabla exportable de métricas 

La tabla contiene las columnas siguientes. 
- ¿Se puede exportar con la configuración de diagnóstico? 
- Efectuado mediante NULL/0 
- ResourceType 
- Métrica 
- MetricDisplayName
- Unidad 
- AggregationType


> [!NOTE]
> La tabla siguiente puede tener una barra de desplazamiento horizontal en la parte inferior. Si cree que falta información, compruebe que la barra de desplazamiento esté a la izquierda.  


| ¿Se puede exportar con la configuración de diagnóstico?  | Ya se emiten valores NULL |  ResourceType  |  Métrica  |  MetricDisplayName  |  Unidad  |  AggregationType | 
|---|---| ---- | ----- | ------ | ---- | ---- | 
| ****Sí****  | No |  Microsoft.AnalysisServices/servers  |  CleanerCurrentPrice  |  Memoria: precio actual de limpieza  |  Count  |  Average | 
| ****Sí****  | No |  Microsoft.AnalysisServices/servers  |  CleanerMemoryNonshrinkable  |  Memoria: memoria de limpieza no reducible  |  Bytes  |  Average | 
| ****Sí****  | No |  Microsoft.AnalysisServices/servers  |  CleanerMemoryShrinkable  |  Memoria: memoria de limpieza reducible  |  Bytes  |  Average | 
| ****Sí****  | No |  Microsoft.AnalysisServices/servers  |  CommandPoolBusyThreads  |  Subprocesos: subprocesos ocupados del grupo de comandos  |  Count  |  Average | 
| ****Sí****  | No |  Microsoft.AnalysisServices/servers  |  CommandPoolIdleThreads  |  Subprocesos: subprocesos inactivos del grupo de comandos  |  Count  |  Average | 
| ****Sí****  | No |  Microsoft.AnalysisServices/servers  |  CommandPoolJobQueueLength  |  Longitud de cola de trabajos de grupo de comandos  |  Count  |  Average | 
| ****Sí****  | No |  Microsoft.AnalysisServices/servers  |  CurrentConnections  |  Conexión: Conexiones actuales  |  Count  |  Average | 
| ****Sí****  | No |  Microsoft.AnalysisServices/servers  |  CurrentUserSessions  |  Sesiones de usuario actuales  |  Count  |  Average | 
| ****Sí****  | No |  Microsoft.AnalysisServices/servers  |  LongParsingBusyThreads  |  Subprocesos: subprocesos ocupados en análisis largos  |  Count  |  Average | 
| **Sí**  | No |  Microsoft.AnalysisServices/servers  |  LongParsingIdleThreads  |  Subprocesos: subprocesos inactivos en análisis largos  |  Count  |  Average | 
| **Sí**  | No |  Microsoft.AnalysisServices/servers  |  LongParsingJobQueueLength  |  Subprocesos: longitud de cola de trabajos en análisis largos  |  Count  |  Average | 
| **Sí**  | No |  Microsoft.AnalysisServices/servers  |  mashup_engine_memory_metric  |  Memoria del motor M  |  Bytes  |  Average | 
| **Sí**  | No |  Microsoft.AnalysisServices/servers  |  mashup_engine_private_bytes_metric  |  Bytes privados del motor M  |  Bytes  |  Average | 
| **Sí**  | No |  Microsoft.AnalysisServices/servers  |  mashup_engine_qpu_metric  |  QPU de motor M  |  Count  |  Average | 
| **Sí**  | No |  Microsoft.AnalysisServices/servers  |  mashup_engine_virtual_bytes_metric  |  Bytes virtuales del motor M  |  Bytes  |  Average | 
| **Sí**  | No |  Microsoft.AnalysisServices/servers  |  memory_metric  |  Memoria  |  Bytes  |  Average | 
| **Sí**  | No |  Microsoft.AnalysisServices/servers  |  memory_thrashing_metric  |  Paginación excesiva de memoria  |  Percent  |  Average | 
| **Sí**  | No |  Microsoft.AnalysisServices/servers  |  MemoryLimitHard  |  Memoria: límite de memoria física  |  Bytes  |  Average | 
| **Sí**  | No |  Microsoft.AnalysisServices/servers  |  MemoryLimitHigh  |  Memoria: límite alto de memoria  |  Bytes  |  Average | 
| **Sí**  | No |  Microsoft.AnalysisServices/servers  |  MemoryLimitLow  |  Memoria: límite bajo de memoria  |  Bytes  |  Average | 
| **Sí**  | No |  Microsoft.AnalysisServices/servers  |  MemoryLimitVertiPaq  |  Memoria: VertiPaq de límite de memoria  |  Bytes  |  Average | 
| **Sí**  | No |  Microsoft.AnalysisServices/servers  |  MemoryUsage  |  Memoria: Uso de la memoria  |  Bytes  |  Average | 
| **Sí**  | No |  Microsoft.AnalysisServices/servers  |  private_bytes_metric  |  Bytes privados  |  Bytes  |  Average | 
| **Sí**  | No |  Microsoft.AnalysisServices/servers  |  ProcessingPoolBusyIOJobThreads  |  Subprocesos: subprocesos de trabajo de E/S ocupados del grupo de procesamiento  |  Count  |  Average | 
| **Sí**  | No |  Microsoft.AnalysisServices/servers  |  ProcessingPoolBusyNonIOThreads  |  Subprocesos: subprocesos de trabajo ocupados que no son de E/S del grupo de procesamiento  |  Count  |  Average | 
| **Sí**  | No |  Microsoft.AnalysisServices/servers  |  ProcessingPoolIdleIOJobThreads  |  Subprocesos: subprocesos de trabajo de E/S inactivos del grupo de procesamiento  |  Count  |  Average | 
| **Sí**  | No |  Microsoft.AnalysisServices/servers  |  ProcessingPoolIdleNonIOThreads  |  Subprocesos: subprocesos de trabajo inactivos que no son de E/S del grupo de procesamiento  |  Count  |  Average | 
| **Sí**  | No |  Microsoft.AnalysisServices/servers  |  ProcessingPoolIOJobQueueLength  |  Subprocesos: Longitud de cola de trabajos de E/S de grupo de procesamiento  |  Count  |  Average | 
| **Sí**  | No |  Microsoft.AnalysisServices/servers  |  ProcessingPoolJobQueueLength  |  Longitud de cola de trabajos de grupo de procesamiento  |  Count  |  Average | 
| **Sí**  | No |  Microsoft.AnalysisServices/servers  |  qpu_metric  |  QPU  |  Count  |  Average | 
| **Sí**  | No |  Microsoft.AnalysisServices/servers  |  QueryPoolBusyThreads  |  Subprocesos ocupados de grupo de consultas  |  Count  |  Average | 
| **Sí**  | No |  Microsoft.AnalysisServices/servers  |  QueryPoolIdleThreads  |  Subprocesos: subprocesos inactivos del grupo de consultas  |  Count  |  Average | 
| **Sí**  | No |  Microsoft.AnalysisServices/servers  |  QueryPoolJobQueueLength  |  Subprocesos: longitud de cola de trabajos del grupo de consultas  |  Count  |  Average | 
| **Sí**  | No |  Microsoft.AnalysisServices/servers  |  Quota  |  Memoria: Quota  |  Bytes  |  Average | 
| **Sí**  | No |  Microsoft.AnalysisServices/servers  |  QuotaBlocked  |  Memoria: cuota bloqueada  |  Count  |  Average | 
| **Sí**  | No |  Microsoft.AnalysisServices/servers  |  RowsConvertedPerSec  |  Procesamiento: filas convertidas por segundo  |  CountPerSecond  |  Average | 
| **Sí**  | No |  Microsoft.AnalysisServices/servers  |  RowsReadPerSec  |  Procesamiento: filas leídas por segundo  |  CountPerSecond  |  Average | 
| **Sí**  | No |  Microsoft.AnalysisServices/servers  |  RowsWrittenPerSec  |  Procesamiento: filas escritas por segundo  |  CountPerSecond  |  Average | 
| **Sí**  | No |  Microsoft.AnalysisServices/servers  |  ShortParsingBusyThreads  |  Subprocesos: subprocesos ocupados en análisis cortos  |  Count  |  Average | 
| **Sí**  | No |  Microsoft.AnalysisServices/servers  |  ShortParsingIdleThreads  |  Subprocesos: subprocesos inactivos en análisis cortos  |  Count  |  Average | 
| **Sí**  | No |  Microsoft.AnalysisServices/servers  |  ShortParsingJobQueueLength  |  Subprocesos: longitud de cola de trabajos en análisis cortos  |  Count  |  Average | 
| **Sí**  | No |  Microsoft.AnalysisServices/servers  |  SuccessfullConnectionsPerSec  |  Conexiones correctas por segundo  |  CountPerSecond  |  Average | 
| **Sí**  | No |  Microsoft.AnalysisServices/servers  |  TotalConnectionFailures  |  Número total de errores de conexión  |  Count  |  Average | 
| **Sí**  | No |  Microsoft.AnalysisServices/servers  |  TotalConnectionRequests  |  Número total de solicitudes de conexión  |  Count  |  Average | 
| **Sí**  | No |  Microsoft.AnalysisServices/servers  |  VertiPaqNonpaged  |  Memoria: VertiPaq no paginado  |  Bytes  |  Average | 
| **Sí**  | No |  Microsoft.AnalysisServices/servers  |  VertiPaqPaged  |  Memoria: VertiPaq paginado  |  Bytes  |  Average | 
| **Sí**  | No |  Microsoft.AnalysisServices/servers  |  virtual_bytes_metric  |  Bytes virtuales  |  Bytes  |  Average | 
| **Sí**  | **Sí** |  Microsoft.ApiManagement/service  |  BackendDuration  |  Duration of Backend Requests (Duración de las solicitudes de back-end)  |  Milisegundos  |  Average | 
| **Sí**  | No |  Microsoft.ApiManagement/service  |  Capacity  |  Capacity  |  Percent  |  Average | 
| **Sí**  | **Sí** |  Microsoft.ApiManagement/service  |  Duration  |  Duración total de las solicitudes de puerta de enlace  |  Milisegundos  |  Average | 
| **Sí**  | **Sí** |  Microsoft.ApiManagement/service  |  EventHubDroppedEvents  |  Eventos EventHub quitados  |  Count  |  Total | 
| **Sí**  | **Sí** |  Microsoft.ApiManagement/service  |  EventHubRejectedEvents  |  Eventos EventHub rechazados  |  Count  |  Total | 
| **Sí**  | **Sí** |  Microsoft.ApiManagement/service  |  EventHubSuccessfulEvents  |  Eventos EventHub correctos  |  Count  |  Total | 
| **Sí**  | **Sí** |  Microsoft.ApiManagement/service  |  EventHubThrottledEvents  |  Eventos EventHub limitados  |  Count  |  Total | 
| **Sí**  | **Sí** |  Microsoft.ApiManagement/service  |  EventHubTimedoutEvents  |  Eventos EventHub con tiempo de espera agotado  |  Count  |  Total | 
| **Sí**  | **Sí** |  Microsoft.ApiManagement/service  |  EventHubTotalBytesSent  |  Tamaño de los eventos EventHub  |  Bytes  |  Total | 
| **Sí**  | **Sí** |  Microsoft.ApiManagement/service  |  EventHubTotalEvents  |  Eventos totales de EventHub  |  Count  |  Total | 
| **Sí**  | **Sí** |  Microsoft.ApiManagement/service  |  EventHubTotalFailedEvents  |  Eventos EventHub con errores  |  Count  |  Total | 
| **Sí**  | **Sí** |  Microsoft.ApiManagement/service  |  FailedRequests  |  Failed Gateway Requests (Deprecated) [Solicitudes de puerta de enlace erróneas (en desuso)]  |  Count  |  Total | 
| **Sí**  | **Sí** |  Microsoft.ApiManagement/service  |  OtherRequests  |  Other Gateway Requests (Deprecated) [Otras solicitudes de puerta de enlace (en desuso)]  |  Count  |  Total | 
| **Sí**  | **Sí** |  Microsoft.ApiManagement/service  |  Requests  |  Requests  |  Count  |  Total | 
| **Sí**  | **Sí** |  Microsoft.ApiManagement/service  |  SuccessfulRequests  |  Successful Gateway Requests (Deprecated) [Solicitudes de puerta de enlace correctas (en desuso)]  |  Count  |  Total | 
| **Sí**  | **Sí** |  Microsoft.ApiManagement/service  |  TotalRequests  |  Total Gateway Requests (Deprecated) [Solicitudes de puerta de enlace en total (en desuso)]  |  Count  |  Total | 
| **Sí**  | **Sí** |  Microsoft.ApiManagement/service  |  UnauthorizedRequests  |  Unauthorized Gateway Requests (Deprecated) [Solicitudes de puerta de enlace no autorizadas (en desuso)]  |  Count  |  Total | 
| **Sí**  | No |  Microsoft.AppPlatform/Spring  |  AppCpuUsagePercentage  |  App CPU Usage Percentage (Porcentaje de uso de CPU de aplicaciones)  |  Percent  |  Average | 
| **Sí**  | No |  Microsoft.AppPlatform/Spring  |  AppMemoryCommitted  |  App Memory Assigned (Memoria de la aplicación asignada)  |  Bytes  |  Average | 
| **Sí**  | No |  Microsoft.AppPlatform/Spring  |  AppMemoryMax  |  App Memory Max (Memoria máxima de la aplicación)  |  Bytes  |  Máxima | 
| **Sí**  | No |  Microsoft.AppPlatform/Spring  |  AppMemoryUsed  |  App Memory Used (Memoria de la aplicación utilizada)  |  Bytes  |  Average | 
| **Sí**  | No |  Microsoft.AppPlatform/Spring  |  GCPauseTotalCount  |  GC Pause Count (Recuento de pausas de la GC)  |  Count  |  Total | 
| **Sí**  | No |  Microsoft.AppPlatform/Spring  |  GCPauseTotalTime  |  GC Pause Total Time (Tiempo total de pausas de la GC)  |  Milisegundos  |  Total | 
| **Sí**  | No |  Microsoft.AppPlatform/Spring  |  MaxOldGenMemoryPoolBytes  |  Max Available Old Generation Data Size (Tamaño máximo de datos de generación anterior disponible)  |  Bytes  |  Average | 
| **Sí**  | No |  Microsoft.AppPlatform/Spring  |  OldGenMemoryPoolBytes  |  Old Generation Data Size (Tamaño de datos de generación anterior)  |  Bytes  |  Average | 
| **Sí**  | No |  Microsoft.AppPlatform/Spring  |  OldGenPromotedBytes  |  Promote to Old Generation Data Size (Promoción al tamaño de datos de generación anterior)  |  Bytes  |  Máxima | 
| **Sí**  | No |  Microsoft.AppPlatform/Spring  |  SystemCpuUsagePercentage  |  System CPU Usage Percentage (Porcentaje de uso de CPU del sistema)  |  Percent  |  Average | 
| **Sí**  | No |  Microsoft.AppPlatform/Spring  |  TomcatErrorCount  |  Tomcat Global Error (Error global de Tomcat)  |  Count  |  Total | 
| **Sí**  | No |  Microsoft.AppPlatform/Spring  |  TomcatReceivedBytes  |  Tomcat Total Received Bytes (Total de bytes recibidos de Tomcat)  |  Bytes  |  Total | 
| **Sí**  | No |  Microsoft.AppPlatform/Spring  |  TomcatRequestMaxTime  |  Tomcat Request Max Time (Tiempo máximo de las solicitudes de Tomcat)  |  Milisegundos  |  Máxima | 
| **Sí**  | No |  Microsoft.AppPlatform/Spring  |  TomcatRequestTotalCount  |  Tomcat Request Total Count (Recuento total de las solicitudes de Tomcat)  |  Count  |  Total | 
| **Sí**  | No |  Microsoft.AppPlatform/Spring  |  TomcatRequestTotalTime  |  Tomcat Request Total Times (Tiempo total de la solicitud de Tomcat)  |  Milisegundos  |  Total | 
| **Sí**  | No |  Microsoft.AppPlatform/Spring  |  TomcatResponseAvgTime  |  Tomcat Request Average Time (Tiempo medio de las solicitudes de Tomcat)  |  Milisegundos  |  Average | 
| **Sí**  | No |  Microsoft.AppPlatform/Spring  |  TomcatSentBytes  |  Tomcat Total Sent Bytes (Total de bytes enviados de Tomcat)  |  Bytes  |  Total | 
| **Sí**  | No |  Microsoft.AppPlatform/Spring  |  TomcatSessionActiveCurrentCount  |  Tomcat Session Alive Count (Recuento activo de las sesiones de Tomcat)  |  Count  |  Total | 
| **Sí**  | No |  Microsoft.AppPlatform/Spring  |  TomcatSessionActiveMaxCount  |  Tomcat Session Max Active Count (Recuento activo máximo de las sesiones de Tomcat)  |  Count  |  Total | 
| **Sí**  | No |  Microsoft.AppPlatform/Spring  |  TomcatSessionAliveMaxTime  |  Tomcat Session Max Alive Time (Tempo activo máximo de las sesiones de Tomcat)  |  Milisegundos  |  Máxima | 
| **Sí**  | No |  Microsoft.AppPlatform/Spring  |  TomcatSessionCreatedCount  |  Tomcat Session Created Count (Recuento de sesiones creadas de Tomcat)  |  Count  |  Total | 
| **Sí**  | No |  Microsoft.AppPlatform/Spring  |  TomcatSessionExpiredCount  |  Tomcat Session Expired Count (Recuento de sesiones expiradas de Tomcat)  |  Count  |  Total | 
| **Sí**  | No |  Microsoft.AppPlatform/Spring  |  TomcatSessionRejectedCount  |  Tomcat Session Rejected Count (Recuento de sesiones rechazadas de Tomcat)  |  Count  |  Total | 
| **Sí**  | No |  Microsoft.AppPlatform/Spring  |  YoungGenPromotedBytes  |  Promote to Young Generation Data Size (Promoción al tamaño de datos de nueva generación)  |  Bytes  |  Máxima | 
| **Sí**  | No |  Microsoft.Automation/automationAccounts  |  TotalJob  |  Número total de trabajos  |  Count  |  Total | 
| **Sí**  | No |  Microsoft.Automation/automationAccounts  |  TotalUpdateDeploymentMachineRuns  |  Ejecuciones de máquina de implementación de actualizaciones totales  |  Count  |  Total | 
| **Sí**  | No |  Microsoft.Automation/automationAccounts  |  TotalUpdateDeploymentRuns  |  Ejecuciones de implementación de actualizaciones totales  |  Count  |  Total | 
| No  | No |  Microsoft.Batch/batchAccounts  |  CoreCount  |  Recuento de núcleos dedicados  |  Count  |  Total | 
| No  | No |  Microsoft.Batch/batchAccounts  |  CreatingNodeCount  |  Recuento de nodos creados  |  Count  |  Total | 
| No  | No |  Microsoft.Batch/batchAccounts  |  IdleNodeCount  |  Recuento de nodos inactivos  |  Count  |  Total | 
| **Sí**  | No |  Microsoft.Batch/batchAccounts  |  JobDeleteCompleteEvent  |  Eventos de finalización de eliminaciones de trabajo  |  Count  |  Total | 
| **Sí**  | No |  Microsoft.Batch/batchAccounts  |  JobDeleteStartEvent  |  Eventos de inicio de eliminaciones de trabajo  |  Count  |  Total | 
| **Sí**  | No |  Microsoft.Batch/batchAccounts  |  JobDisableCompleteEvent  |  Eventos de finalización de deshabilitación de trabajo  |  Count  |  Total | 
| **Sí**  | No |  Microsoft.Batch/batchAccounts  |  JobDisableStartEvent  |  Eventos de inicio de deshabilitación de trabajo  |  Count  |  Total | 
| **Sí**  | No |  Microsoft.Batch/batchAccounts  |  JobStartEvent  |  Eventos de inicio de trabajo  |  Count  |  Total | 
| **Sí**  | No |  Microsoft.Batch/batchAccounts  |  JobTerminateCompleteEvent  |  Eventos de finalización de terminaciones de trabajo  |  Count  |  Total | 
| **Sí**  | No |  Microsoft.Batch/batchAccounts  |  JobTerminateStartEvent  |  Eventos de inicio de terminaciones de trabajo  |  Count  |  Total | 
| No  | No |  Microsoft.Batch/batchAccounts  |  LeavingPoolNodeCount  |  Recuento de nodos que salen del grupo  |  Count  |  Total | 
| No  | No |  Microsoft.Batch/batchAccounts  |  LowPriorityCoreCount  |  Recuento de núcleos de baja prioridad  |  Count  |  Total | 
| No  | No |  Microsoft.Batch/batchAccounts  |  OfflineNodeCount  |  Recuento de nodos sin conexión  |  Count  |  Total | 
| **Sí**  | No |  Microsoft.Batch/batchAccounts  |  PoolCreateEvent  |  Eventos de creación de grupo  |  Count  |  Total | 
| **Sí**  | No |  Microsoft.Batch/batchAccounts  |  PoolDeleteCompleteEvent  |  Eventos de finalización de eliminaciones de grupo  |  Count  |  Total | 
| **Sí**  | No |  Microsoft.Batch/batchAccounts  |  PoolDeleteStartEvent  |  Eventos de inicio de eliminación de grupo  |  Count  |  Total | 
| **Sí**  | No |  Microsoft.Batch/batchAccounts  |  PoolResizeCompleteEvent  |  Eventos de finalización de cambio de tamaño de grupo  |  Count  |  Total | 
| **Sí**  | No |  Microsoft.Batch/batchAccounts  |  PoolResizeStartEvent  |  Eventos de inicio de cambio de tamaño de grupo  |  Count  |  Total | 
| No  | No |  Microsoft.Batch/batchAccounts  |  PreemptedNodeCount  |  Recuento de nodos con prioridad  |  Count  |  Total | 
| No  | No |  Microsoft.Batch/batchAccounts  |  RebootingNodeCount  |  Recuento de nodos de reinicio  |  Count  |  Total | 
| No  | No |  Microsoft.Batch/batchAccounts  |  ReimagingNodeCount  |  Recuento de nodos de restablecimiento de imagen inicial  |  Count  |  Total | 
| No  | No |  Microsoft.Batch/batchAccounts  |  RunningNodeCount  |  Recuento de nodos en ejecución  |  Count  |  Total | 
| No  | No |  Microsoft.Batch/batchAccounts  |  StartingNodeCount  |  Recuento de nodos de inicio  |  Count  |  Total | 
| No  | No |  Microsoft.Batch/batchAccounts  |  StartTaskFailedNodeCount  |  Recuento de nodos con error de la tarea de inicio  |  Count  |  Total | 
| **Sí**  | No |  Microsoft.Batch/batchAccounts  |  TaskCompleteEvent  |  Eventos de tarea completada  |  Count  |  Total | 
| **Sí**  | No |  Microsoft.Batch/batchAccounts  |  TaskFailEvent  |  Eventos de error en tareas  |  Count  |  Total | 
| **Sí**  | No |  Microsoft.Batch/batchAccounts  |  TaskStartEvent  |  Eventos de inicio de tarea  |  Count  |  Total | 
| No  | No |  Microsoft.Batch/batchAccounts  |  TotalLowPriorityNodeCount  |  Recuento de nodos de baja prioridad  |  Count  |  Total | 
| No  | No |  Microsoft.Batch/batchAccounts  |  TotalNodeCount  |  Recuento de nodos dedicados  |  Count  |  Total | 
| No  | No |  Microsoft.Batch/batchAccounts  |  UnusableNodeCount  |  Recuento de nodos no utilizables  |  Count  |  Total | 
| No  | No |  Microsoft.Batch/batchAccounts  |  WaitingForStartTaskNodeCount  |  Recuento de nodos esperando a la tarea de inicio  |  Count  |  Total | 
| **Sí**  | No |  Microsoft.BatchAI/workspaces  |  Active Cores  |  Active Cores (Núcleos activos)  |  Count  |  Average | 
| **Sí**  | No |  Microsoft.BatchAI/workspaces  |  Active Nodes  |  Active Nodes (Nodos activos)  |  Count  |  Average | 
| **Sí**  | No |  Microsoft.BatchAI/workspaces  |  Idle Cores  |  Idle Cores (Núcleos inactivos)  |  Count  |  Average | 
| **Sí**  | No |  Microsoft.BatchAI/workspaces  |  Idle Nodes  |  Idle Nodes (Nodos inactivos)  |  Count  |  Average | 
| **Sí**  | No |  Microsoft.BatchAI/workspaces  |  Job Completed  |  Job Completed (Trabajo completado)  |  Count  |  Total | 
| **Sí**  | No |  Microsoft.BatchAI/workspaces  |  Job Submitted  |  Job Submitted (Trabajo enviado)  |  Count  |  Total | 
| **Sí**  | No |  Microsoft.BatchAI/workspaces  |  Leaving Cores  |  Leaving Cores (Núcleos de salida)  |  Count  |  Average | 
| **Sí**  | No |  Microsoft.BatchAI/workspaces  |  Leaving Nodes  |  Leaving Nodes (Nodos de salida)  |  Count  |  Average | 
| **Sí**  | No |  Microsoft.BatchAI/workspaces  |  Preempted Cores  |  Preempted Cores (Núcleos con prioridad)  |  Count  |  Average | 
| **Sí**  | No |  Microsoft.BatchAI/workspaces  |  Preempted Nodes  |  Preempted Nodes (Nodos con prioridad)  |  Count  |  Average | 
| **Sí**  | No |  Microsoft.BatchAI/workspaces  |  Porcentaje de uso de la cuota  |  Porcentaje de uso de la cuota  |  Count  |  Average | 
| **Sí**  | No |  Microsoft.BatchAI/workspaces  |  Total Cores  |  Total Cores (Núcleos totales)  |  Count  |  Average | 
| **Sí**  | No |  Microsoft.BatchAI/workspaces  |  Total Nodes  |  Total Nodes (Nodos totales)  |  Count  |  Average | 
| **Sí**  | No |  Microsoft.BatchAI/workspaces  |  Unusable Cores  |  Unusable Cores (Núcleos no utilizables)  |  Count  |  Average | 
| **Sí**  | No |  Microsoft.BatchAI/workspaces  |  Nodos no utilizables  |  Nodos no utilizables  |  Count  |  Average | 
| **Sí**  | No |  Microsoft.Blockchain/blockchainMembers  |  ConnectionAccepted  |  Accepted Connections (Conexiones aceptadas)  |  Count  |  Total | 
| **Sí**  | No |  Microsoft.Blockchain/blockchainMembers  |  ConnectionActive  |  Conexiones activas  |  Count  |  Average | 
| **Sí**  | No |  Microsoft.Blockchain/blockchainMembers  |  ConnectionHandled  |  Handled Connections (Conexiones controladas)  |  Count  |  Total | 
| **Sí**  | No |  Microsoft.Blockchain/blockchainMembers  |  CpuUsagePercentageInDouble  |  CPU Usage Percentage (Porcentaje de uso de CPU)  |  Percent  |  Máxima | 
| **Sí**  | No |  Microsoft.Blockchain/blockchainMembers  |  IOReadBytes  |  IO Read Bytes (Bytes de lectura de E/S)  |  Bytes  |  Total | 
| **Sí**  | No |  Microsoft.Blockchain/blockchainMembers  |  IOWriteBytes  |  IO Write Bytes (Bytes de escritura de E/S)  |  Bytes  |  Total | 
| **Sí**  | No |  Microsoft.Blockchain/blockchainMembers  |  MemoryLimit  |  Memory Limit (Límite de memoria)  |  Bytes  |  Average | 
| **Sí**  | No |  Microsoft.Blockchain/blockchainMembers  |  MemoryUsage  |  Uso de la memoria  |  Bytes  |  Average | 
| **Sí**  | No |  Microsoft.Blockchain/blockchainMembers  |  MemoryUsagePercentageInDouble  |  Memory Usage Percentage (Porcentaje de uso de memoria)  |  Percent  |  Average | 
| **Sí**  | No |  Microsoft.Blockchain/blockchainMembers  |  PendingTransactions  |  Pending Transactions (Transacciones pendientes)  |  Count  |  Average | 
| **Sí**  | No |  Microsoft.Blockchain/blockchainMembers  |  ProcessedBlocks  |  Processed Blocks (Bloques procesados)  |  Count  |  Total | 
| **Sí**  | No |  Microsoft.Blockchain/blockchainMembers  |  ProcessedTransactions  |  Processed Transactions (Transacciones procesadas)  |  Count  |  Total | 
| **Sí**  | No |  Microsoft.Blockchain/blockchainMembers  |  QueuedTransactions  |  Queued Transactions (Transacciones en cola)  |  Count  |  Average | 
| **Sí**  | No |  Microsoft.Blockchain/blockchainMembers  |  RequestHandled  |  Handled Requests (Solicitudes controladas)  |  Count  |  Total | 
| **Sí**  | No |  Microsoft.Blockchain/blockchainMembers  |  StorageUsage  |  Storage Usage (Uso del almacenamiento)  |  Bytes  |  Average | 
| **Sí**  | No |  Microsoft.Cache/redis  |  cachehits  |  Aciertos de caché  |  Count  |  Total | 
| **Sí**  | No |  Microsoft.Cache/redis  |  cachehits0  |  Aciertos de caché (partición 0)  |  Count  |  Total | 
| **Sí**  | No |  Microsoft.Cache/redis  |  cachehits1  |  Aciertos de caché (partición 1)  |  Count  |  Total | 
| **Sí**  | No |  Microsoft.Cache/redis  |  cachehits2  |  Aciertos de caché (partición 2)  |  Count  |  Total | 
| **Sí**  | No |  Microsoft.Cache/redis  |  cachehits3  |  Aciertos de caché (partición 3)  |  Count  |  Total | 
| **Sí**  | No |  Microsoft.Cache/redis  |  cachehits4  |  Aciertos de caché (partición 4)  |  Count  |  Total | 
| **Sí**  | No |  Microsoft.Cache/redis  |  cachehits5  |  Aciertos de caché (partición 5)  |  Count  |  Total | 
| **Sí**  | No |  Microsoft.Cache/redis  |  cachehits6  |  Aciertos de caché (partición 6)  |  Count  |  Total | 
| **Sí**  | No |  Microsoft.Cache/redis  |  cachehits7  |  Aciertos de caché (partición 7)  |  Count  |  Total | 
| **Sí**  | No |  Microsoft.Cache/redis  |  cachehits8  |  Aciertos de caché (partición 8)  |  Count  |  Total | 
| **Sí**  | No |  Microsoft.Cache/redis  |  cachehits9  |  Aciertos de caché (partición 9)  |  Count  |  Total | 
| **Sí**  | No |  Microsoft.Cache/redis  |  cacheLatency  |  Microsegundos de latencia de la memoria caché (versión preliminar)  |  Count  |  Average | 
| **Sí**  | No |  Microsoft.Cache/redis  |  cachemisses  |  Errores de caché  |  Count  |  Total | 
| **Sí**  | No |  Microsoft.Cache/redis  |  cachemisses0  |  Errores de caché (partición 0)  |  Count  |  Total | 
| **Sí**  | No |  Microsoft.Cache/redis  |  cachemisses1  |  Errores de caché (partición 1)  |  Count  |  Total | 
| **Sí**  | No |  Microsoft.Cache/redis  |  cachemisses2  |  Errores de caché (partición 2)  |  Count  |  Total | 
| **Sí**  | No |  Microsoft.Cache/redis  |  cachemisses3  |  Errores de caché (partición 3)  |  Count  |  Total | 
| **Sí**  | No |  Microsoft.Cache/redis  |  cachemisses4  |  Errores de caché (partición 4)  |  Count  |  Total | 
| **Sí**  | No |  Microsoft.Cache/redis  |  cachemisses5  |  Errores de caché (partición 5)  |  Count  |  Total | 
| **Sí**  | No |  Microsoft.Cache/redis  |  cachemisses6  |  Errores de caché (partición 6)  |  Count  |  Total | 
| **Sí**  | No |  Microsoft.Cache/redis  |  cachemisses7  |  Errores de caché (partición 7)  |  Count  |  Total | 
| **Sí**  | No |  Microsoft.Cache/redis  |  cachemisses8  |  Errores de caché (partición 8)  |  Count  |  Total | 
| **Sí**  | No |  Microsoft.Cache/redis  |  cachemisses9  |  Errores de caché (partición 9)  |  Count  |  Total | 
| **Sí**  | No |  Microsoft.Cache/redis  |  cacheRead  |  Lectura de caché  |  BytesPerSecond  |  Máxima | 
| **Sí**  | No |  Microsoft.Cache/redis  |  cacheRead0  |  Lectura de caché (partición 0)  |  BytesPerSecond  |  Máxima | 
| **Sí**  | No |  Microsoft.Cache/redis  |  cacheRead1  |  Lectura de caché (partición 1)  |  BytesPerSecond  |  Máxima | 
| **Sí**  | No |  Microsoft.Cache/redis  |  cacheRead2  |  Lectura de caché (partición 2)  |  BytesPerSecond  |  Máxima | 
| **Sí**  | No |  Microsoft.Cache/redis  |  cacheRead3  |  Lectura de caché (partición 3)  |  BytesPerSecond  |  Máxima | 
| **Sí**  | No |  Microsoft.Cache/redis  |  cacheRead4  |  Lectura de caché (partición 4)  |  BytesPerSecond  |  Máxima | 
| **Sí**  | No |  Microsoft.Cache/redis  |  cacheRead5  |  Lectura de caché (partición 5)  |  BytesPerSecond  |  Máxima | 
| **Sí**  | No |  Microsoft.Cache/redis  |  cacheRead6  |  Lectura de caché (partición 6)  |  BytesPerSecond  |  Máxima | 
| **Sí**  | No |  Microsoft.Cache/redis  |  cacheRead7  |  Lectura de caché (partición 7)  |  BytesPerSecond  |  Máxima | 
| **Sí**  | No |  Microsoft.Cache/redis  |  cacheRead8  |  Lectura de caché (partición 8)  |  BytesPerSecond  |  Máxima | 
| **Sí**  | No |  Microsoft.Cache/redis  |  cacheRead9  |  Lectura de caché (partición 9)  |  BytesPerSecond  |  Máxima | 
| **Sí**  | No |  Microsoft.Cache/redis  |  cacheWrite  |  Escritura de caché  |  BytesPerSecond  |  Máxima | 
| **Sí**  | No |  Microsoft.Cache/redis  |  cacheWrite0  |  Escritura de caché (partición 0)  |  BytesPerSecond  |  Máxima | 
| **Sí**  | No |  Microsoft.Cache/redis  |  cacheWrite1  |  Escritura de caché (partición 1)  |  BytesPerSecond  |  Máxima | 
| **Sí**  | No |  Microsoft.Cache/redis  |  cacheWrite2  |  Escritura de caché (partición 2)  |  BytesPerSecond  |  Máxima | 
| **Sí**  | No |  Microsoft.Cache/redis  |  cacheWrite3  |  Escritura de caché (partición 3)  |  BytesPerSecond  |  Máxima | 
| **Sí**  | No |  Microsoft.Cache/redis  |  cacheWrite4  |  Escritura de caché (partición 4)  |  BytesPerSecond  |  Máxima | 
| **Sí**  | No |  Microsoft.Cache/redis  |  cacheWrite5  |  Escritura de caché (partición 5)  |  BytesPerSecond  |  Máxima | 
| **Sí**  | No |  Microsoft.Cache/redis  |  cacheWrite6  |  Escritura de caché (partición 6)  |  BytesPerSecond  |  Máxima | 
| **Sí**  | No |  Microsoft.Cache/redis  |  cacheWrite7  |  Escritura de caché (partición 7)  |  BytesPerSecond  |  Máxima | 
| **Sí**  | No |  Microsoft.Cache/redis  |  cacheWrite8  |  Escritura de caché (partición 8)  |  BytesPerSecond  |  Máxima | 
| **Sí**  | No |  Microsoft.Cache/redis  |  cacheWrite9  |  Escritura de caché (partición 9)  |  BytesPerSecond  |  Máxima | 
| **Sí**  | No |  Microsoft.Cache/redis  |  connectedclients  |  Clientes conectados  |  Count  |  Máxima | 
| **Sí**  | No |  Microsoft.Cache/redis  |  connectedclients0  |  Clientes conectados (partición 0)  |  Count  |  Máxima | 
| **Sí**  | No |  Microsoft.Cache/redis  |  connectedclients1  |  Clientes conectados (partición 1)  |  Count  |  Máxima | 
| **Sí**  | No |  Microsoft.Cache/redis  |  connectedclients2  |  Clientes conectados (partición 2)  |  Count  |  Máxima | 
| **Sí**  | No |  Microsoft.Cache/redis  |  connectedclients3  |  Clientes conectados (partición 3)  |  Count  |  Máxima | 
| **Sí**  | No |  Microsoft.Cache/redis  |  connectedclients4  |  Clientes conectados (partición 4)  |  Count  |  Máxima | 
| **Sí**  | No |  Microsoft.Cache/redis  |  connectedclients5  |  Clientes conectados (partición 5)  |  Count  |  Máxima | 
| **Sí**  | No |  Microsoft.Cache/redis  |  connectedclients6  |  Clientes conectados (partición 6)  |  Count  |  Máxima | 
| **Sí**  | No |  Microsoft.Cache/redis  |  connectedclients7  |  Clientes conectados (partición 7)  |  Count  |  Máxima | 
| **Sí**  | No |  Microsoft.Cache/redis  |  connectedclients8  |  Clientes conectados (partición 8)  |  Count  |  Máxima | 
| **Sí**  | No |  Microsoft.Cache/redis  |  connectedclients9  |  Clientes conectados (partición 9)  |  Count  |  Máxima | 
| **Sí**  | No |  Microsoft.Cache/redis  |  errors  |  Errors  |  Count  |  Máxima | 
| **Sí**  | No |  Microsoft.Cache/redis  |  evictedkeys  |  Claves expulsadas  |  Count  |  Total | 
| **Sí**  | No |  Microsoft.Cache/redis  |  evictedkeys0  |  Claves expulsadas (partición 0)  |  Count  |  Total | 
| **Sí**  | No |  Microsoft.Cache/redis  |  evictedkeys1  |  Claves expulsadas (partición 1)  |  Count  |  Total | 
| **Sí**  | No |  Microsoft.Cache/redis  |  evictedkeys2  |  Claves expulsadas (partición 2)  |  Count  |  Total | 
| **Sí**  | No |  Microsoft.Cache/redis  |  evictedkeys3  |  Claves expulsadas (partición 3)  |  Count  |  Total | 
| **Sí**  | No |  Microsoft.Cache/redis  |  evictedkeys4  |  Claves expulsadas (partición 4)  |  Count  |  Total | 
| **Sí**  | No |  Microsoft.Cache/redis  |  evictedkeys5  |  Claves expulsadas (partición 5)  |  Count  |  Total | 
| **Sí**  | No |  Microsoft.Cache/redis  |  evictedkeys6  |  Claves expulsadas (partición 6)  |  Count  |  Total | 
| **Sí**  | No |  Microsoft.Cache/redis  |  evictedkeys7  |  Claves expulsadas (partición 7)  |  Count  |  Total | 
| **Sí**  | No |  Microsoft.Cache/redis  |  evictedkeys8  |  Claves expulsadas (partición 8)  |  Count  |  Total | 
| **Sí**  | No |  Microsoft.Cache/redis  |  evictedkeys9  |  Claves expulsadas (partición 9)  |  Count  |  Total | 
| **Sí**  | No |  Microsoft.Cache/redis  |  expiredkeys  |  Claves expiradas  |  Count  |  Total | 
| **Sí**  | No |  Microsoft.Cache/redis  |  expiredkeys0  |  Claves expiradas (partición 0)  |  Count  |  Total | 
| **Sí**  | No |  Microsoft.Cache/redis  |  expiredkeys1  |  Claves expiradas (partición 1)  |  Count  |  Total | 
| **Sí**  | No |  Microsoft.Cache/redis  |  expiredkeys2  |  Claves expiradas (partición 2)  |  Count  |  Total | 
| **Sí**  | No |  Microsoft.Cache/redis  |  expiredkeys3  |  Claves expiradas (partición 3)  |  Count  |  Total | 
| **Sí**  | No |  Microsoft.Cache/redis  |  expiredkeys4  |  Claves expiradas (partición 4)  |  Count  |  Total | 
| **Sí**  | No |  Microsoft.Cache/redis  |  expiredkeys5  |  Claves expiradas (partición 5)  |  Count  |  Total | 
| **Sí**  | No |  Microsoft.Cache/redis  |  expiredkeys6  |  Claves expiradas (partición 6)  |  Count  |  Total | 
| **Sí**  | No |  Microsoft.Cache/redis  |  expiredkeys7  |  Claves expiradas (partición 7)  |  Count  |  Total | 
| **Sí**  | No |  Microsoft.Cache/redis  |  expiredkeys8  |  Claves expiradas (partición 8)  |  Count  |  Total | 
| **Sí**  | No |  Microsoft.Cache/redis  |  expiredkeys9  |  Claves expiradas (partición 9)  |  Count  |  Total | 
| **Sí**  | No |  Microsoft.Cache/redis  |  getcommands  |  Gets  |  Count  |  Total | 
| **Sí**  | No |  Microsoft.Cache/redis  |  getcommands0  |  Obtenciones (partición 0)  |  Count  |  Total | 
| **Sí**  | No |  Microsoft.Cache/redis  |  getcommands1  |  Obtenciones (partición 1)  |  Count  |  Total | 
| **Sí**  | No |  Microsoft.Cache/redis  |  getcommands2  |  Obtenciones (partición 2)  |  Count  |  Total | 
| **Sí**  | No |  Microsoft.Cache/redis  |  getcommands3  |  Obtenciones (partición 3)  |  Count  |  Total | 
| **Sí**  | No |  Microsoft.Cache/redis  |  getcommands4  |  Obtenciones (partición 4)  |  Count  |  Total | 
| **Sí**  | No |  Microsoft.Cache/redis  |  getcommands5  |  Obtenciones (partición 5)  |  Count  |  Total | 
| **Sí**  | No |  Microsoft.Cache/redis  |  getcommands6  |  Obtenciones (partición 6)  |  Count  |  Total | 
| **Sí**  | No |  Microsoft.Cache/redis  |  getcommands7  |  Obtenciones (partición 7)  |  Count  |  Total | 
| **Sí**  | No |  Microsoft.Cache/redis  |  getcommands8  |  Obtenciones (partición 8)  |  Count  |  Total | 
| **Sí**  | No |  Microsoft.Cache/redis  |  getcommands9  |  Obtenciones (partición 9)  |  Count  |  Total | 
| **Sí**  | No |  Microsoft.Cache/redis  |  operationsPerSecond  |  Operaciones por segundo  |  Count  |  Máxima | 
| **Sí**  | No |  Microsoft.Cache/redis  |  operationsPerSecond0  |  Operaciones por segundo (partición 0)  |  Count  |  Máxima | 
| **Sí**  | No |  Microsoft.Cache/redis  |  operationsPerSecond1  |  Operaciones por segundo (partición 1)  |  Count  |  Máxima | 
| **Sí**  | No |  Microsoft.Cache/redis  |  operationsPerSecond2  |  Operaciones por segundo (partición 2)  |  Count  |  Máxima | 
| **Sí**  | No |  Microsoft.Cache/redis  |  operationsPerSecond3  |  Operaciones por segundo (partición 3)  |  Count  |  Máxima | 
| **Sí**  | No |  Microsoft.Cache/redis  |  operationsPerSecond4  |  Operaciones por segundo (partición 4)  |  Count  |  Máxima | 
| **Sí**  | No |  Microsoft.Cache/redis  |  operationsPerSecond5  |  Operaciones por segundo (partición 5)  |  Count  |  Máxima | 
| **Sí**  | No |  Microsoft.Cache/redis  |  operationsPerSecond6  |  Operaciones por segundo (partición 6)  |  Count  |  Máxima | 
| **Sí**  | No |  Microsoft.Cache/redis  |  operationsPerSecond7  |  Operaciones por segundo (partición 7)  |  Count  |  Máxima | 
| **Sí**  | No |  Microsoft.Cache/redis  |  operationsPerSecond8  |  Operaciones por segundo (partición 8)  |  Count  |  Máxima | 
| **Sí**  | No |  Microsoft.Cache/redis  |  operationsPerSecond9  |  Operaciones por segundo (partición 9)  |  Count  |  Máxima | 
| **Sí**  | No |  Microsoft.Cache/redis  |  percentProcessorTime  |  CPU  |  Percent  |  Máxima | 
| **Sí**  | No |  Microsoft.Cache/redis  |  percentProcessorTime0  |  CPU (partición 0)  |  Percent  |  Máxima | 
| **Sí**  | No |  Microsoft.Cache/redis  |  percentProcessorTime1  |  CPU (partición 1)  |  Percent  |  Máxima | 
| **Sí**  | No |  Microsoft.Cache/redis  |  percentProcessorTime2  |  CPU (partición 2)  |  Percent  |  Máxima | 
| **Sí**  | No |  Microsoft.Cache/redis  |  percentProcessorTime3  |  CPU (partición 3)  |  Percent  |  Máxima | 
| **Sí**  | No |  Microsoft.Cache/redis  |  percentProcessorTime4  |  CPU (partición 4)  |  Percent  |  Máxima | 
| **Sí**  | No |  Microsoft.Cache/redis  |  percentProcessorTime5  |  CPU (partición 5)  |  Percent  |  Máxima | 
| **Sí**  | No |  Microsoft.Cache/redis  |  percentProcessorTime6  |  CPU (partición 6)  |  Percent  |  Máxima | 
| **Sí**  | No |  Microsoft.Cache/redis  |  percentProcessorTime7  |  CPU (partición 7)  |  Percent  |  Máxima | 
| **Sí**  | No |  Microsoft.Cache/redis  |  percentProcessorTime8  |  CPU (partición 8)  |  Percent  |  Máxima | 
| **Sí**  | No |  Microsoft.Cache/redis  |  percentProcessorTime9  |  CPU (partición 9)  |  Percent  |  Máxima | 
| **Sí**  | No |  Microsoft.Cache/redis  |  serverLoad  |  Carga de servidor  |  Percent  |  Máxima | 
| **Sí**  | No |  Microsoft.Cache/redis  |  serverLoad0  |  Carga de servidor (partición 0)  |  Percent  |  Máxima | 
| **Sí**  | No |  Microsoft.Cache/redis  |  serverLoad1  |  Carga de servidor (partición 1)  |  Percent  |  Máxima | 
| **Sí**  | No |  Microsoft.Cache/redis  |  serverLoad2  |  Carga de servidor (partición 2)  |  Percent  |  Máxima | 
| **Sí**  | No |  Microsoft.Cache/redis  |  serverLoad3  |  Carga de servidor (partición 3)  |  Percent  |  Máxima | 
| **Sí**  | No |  Microsoft.Cache/redis  |  serverLoad4  |  Carga de servidor (partición 4)  |  Percent  |  Máxima | 
| **Sí**  | No |  Microsoft.Cache/redis  |  serverLoad5  |  Carga de servidor (partición 5)  |  Percent  |  Máxima | 
| **Sí**  | No |  Microsoft.Cache/redis  |  serverLoad6  |  Carga de servidor (partición 6)  |  Percent  |  Máxima | 
| **Sí**  | No |  Microsoft.Cache/redis  |  serverLoad7  |  Carga de servidor (partición 7)  |  Percent  |  Máxima | 
| **Sí**  | No |  Microsoft.Cache/redis  |  serverLoad8  |  Carga de servidor (partición 8)  |  Percent  |  Máxima | 
| **Sí**  | No |  Microsoft.Cache/redis  |  serverLoad9  |  Carga de servidor (partición 9)  |  Percent  |  Máxima | 
| **Sí**  | No |  Microsoft.Cache/redis  |  setcommands  |  Conjuntos  |  Count  |  Total | 
| **Sí**  | No |  Microsoft.Cache/redis  |  setcommands0  |  Definiciones (partición 0)  |  Count  |  Total | 
| **Sí**  | No |  Microsoft.Cache/redis  |  setcommands1  |  Definiciones (partición 1)  |  Count  |  Total | 
| **Sí**  | No |  Microsoft.Cache/redis  |  setcommands2  |  Definiciones (partición 2)  |  Count  |  Total | 
| **Sí**  | No |  Microsoft.Cache/redis  |  setcommands3  |  Definiciones (partición 3)  |  Count  |  Total | 
| **Sí**  | No |  Microsoft.Cache/redis  |  setcommands4  |  Definiciones (partición 4)  |  Count  |  Total | 
| **Sí**  | No |  Microsoft.Cache/redis  |  setcommands5  |  Definiciones (partición 5)  |  Count  |  Total | 
| **Sí**  | No |  Microsoft.Cache/redis  |  setcommands6  |  Definiciones (partición 6)  |  Count  |  Total | 
| **Sí**  | No |  Microsoft.Cache/redis  |  setcommands7  |  Definiciones (partición 7)  |  Count  |  Total | 
| **Sí**  | No |  Microsoft.Cache/redis  |  setcommands8  |  Definiciones (partición 8)  |  Count  |  Total | 
| **Sí**  | No |  Microsoft.Cache/redis  |  setcommands9  |  Definiciones (partición 9)  |  Count  |  Total | 
| **Sí**  | No |  Microsoft.Cache/redis  |  totalcommandsprocessed  |  Total de operaciones  |  Count  |  Total | 
| **Sí**  | No |  Microsoft.Cache/redis  |  totalcommandsprocessed0  |  Total de operaciones (partición 0)  |  Count  |  Total | 
| **Sí**  | No |  Microsoft.Cache/redis  |  totalcommandsprocessed1  |  Total de operaciones (partición 1)  |  Count  |  Total | 
| **Sí**  | No |  Microsoft.Cache/redis  |  totalcommandsprocessed2  |  Total de operaciones (partición 2)  |  Count  |  Total | 
| **Sí**  | No |  Microsoft.Cache/redis  |  totalcommandsprocessed3  |  Total de operaciones (partición 3)  |  Count  |  Total | 
| **Sí**  | No |  Microsoft.Cache/redis  |  totalcommandsprocessed4  |  Total de operaciones (partición 4)  |  Count  |  Total | 
| **Sí**  | No |  Microsoft.Cache/redis  |  totalcommandsprocessed5  |  Total de operaciones (partición 5)  |  Count  |  Total | 
| **Sí**  | No |  Microsoft.Cache/redis  |  totalcommandsprocessed6  |  Total de operaciones (partición 6)  |  Count  |  Total | 
| **Sí**  | No |  Microsoft.Cache/redis  |  totalcommandsprocessed7  |  Total de operaciones (partición 7)  |  Count  |  Total | 
| **Sí**  | No |  Microsoft.Cache/redis  |  totalcommandsprocessed8  |  Total de operaciones (partición 8)  |  Count  |  Total | 
| **Sí**  | No |  Microsoft.Cache/redis  |  totalcommandsprocessed9  |  Total de operaciones (partición 9)  |  Count  |  Total | 
| **Sí**  | No |  Microsoft.Cache/redis  |  totalkeys  |  Total de claves  |  Count  |  Máxima | 
| **Sí**  | No |  Microsoft.Cache/redis  |  totalkeys0  |  Total de claves (partición 0)  |  Count  |  Máxima | 
| **Sí**  | No |  Microsoft.Cache/redis  |  totalkeys1  |  Total de claves (partición 1)  |  Count  |  Máxima | 
| **Sí**  | No |  Microsoft.Cache/redis  |  totalkeys2  |  Total de claves (partición 2)  |  Count  |  Máxima | 
| **Sí**  | No |  Microsoft.Cache/redis  |  totalkeys3  |  Total de claves (partición 3)  |  Count  |  Máxima | 
| **Sí**  | No |  Microsoft.Cache/redis  |  totalkeys4  |  Total de claves (partición 4)  |  Count  |  Máxima | 
| **Sí**  | No |  Microsoft.Cache/redis  |  totalkeys5  |  Total de claves (partición 5)  |  Count  |  Máxima | 
| **Sí**  | No |  Microsoft.Cache/redis  |  totalkeys6  |  Total de claves (partición 6)  |  Count  |  Máxima | 
| **Sí**  | No |  Microsoft.Cache/redis  |  totalkeys7  |  Total de claves (partición 7)  |  Count  |  Máxima | 
| **Sí**  | No |  Microsoft.Cache/redis  |  totalkeys8  |  Total de claves (partición 8)  |  Count  |  Máxima | 
| **Sí**  | No |  Microsoft.Cache/redis  |  totalkeys9  |  Total de claves (partición 9)  |  Count  |  Máxima | 
| **Sí**  | No |  Microsoft.Cache/redis  |  usedmemory  |  Memoria usada  |  Bytes  |  Máxima | 
| **Sí**  | No |  Microsoft.Cache/redis  |  usedmemory0  |  Memoria usada (partición 0)  |  Bytes  |  Máxima | 
| **Sí**  | No |  Microsoft.Cache/redis  |  usedmemory1  |  Memoria usada (partición 1)  |  Bytes  |  Máxima | 
| **Sí**  | No |  Microsoft.Cache/redis  |  usedmemory2  |  Memoria usada (partición 2)  |  Bytes  |  Máxima | 
| **Sí**  | No |  Microsoft.Cache/redis  |  usedmemory3  |  Memoria usada (partición 3)  |  Bytes  |  Máxima | 
| **Sí**  | No |  Microsoft.Cache/redis  |  usedmemory4  |  Memoria usada (partición 4)  |  Bytes  |  Máxima | 
| **Sí**  | No |  Microsoft.Cache/redis  |  usedmemory5  |  Memoria usada (partición 5)  |  Bytes  |  Máxima | 
| **Sí**  | No |  Microsoft.Cache/redis  |  usedmemory6  |  Memoria usada (partición 6)  |  Bytes  |  Máxima | 
| **Sí**  | No |  Microsoft.Cache/redis  |  usedmemory7  |  Memoria usada (partición 7)  |  Bytes  |  Máxima | 
| **Sí**  | No |  Microsoft.Cache/redis  |  usedmemory8  |  Memoria usada (partición 8)  |  Bytes  |  Máxima | 
| **Sí**  | No |  Microsoft.Cache/redis  |  usedmemory9  |  Memoria usada (partición 9)  |  Bytes  |  Máxima | 
| **Sí**  | No |  Microsoft.Cache/redis  |  usedmemorypercentage  |  Porcentaje de memoria usado  |  Percent  |  Máxima | 
| **Sí**  | No |  Microsoft.Cache/redis  |  usedmemoryRss  |  Memoria RSS usada  |  Bytes  |  Máxima | 
| **Sí**  | No |  Microsoft.Cache/redis  |  usedmemoryRss0  |  Memoria RSS usada (partición 0)  |  Bytes  |  Máxima | 
| **Sí**  | No |  Microsoft.Cache/redis  |  usedmemoryRss1  |  Memoria RSS usada (partición 1)  |  Bytes  |  Máxima | 
| **Sí**  | No |  Microsoft.Cache/redis  |  usedmemoryRss2  |  Memoria RSS usada (partición 2)  |  Bytes  |  Máxima | 
| **Sí**  | No |  Microsoft.Cache/redis  |  usedmemoryRss3  |  Memoria RSS usada (partición 3)  |  Bytes  |  Máxima | 
| **Sí**  | No |  Microsoft.Cache/redis  |  usedmemoryRss4  |  Memoria RSS usada (partición 4)  |  Bytes  |  Máxima | 
| **Sí**  | No |  Microsoft.Cache/redis  |  usedmemoryRss5  |  Memoria RSS usada (partición 5)  |  Bytes  |  Máxima | 
| **Sí**  | No |  Microsoft.Cache/redis  |  usedmemoryRss6  |  Memoria RSS usada (partición 6)  |  Bytes  |  Máxima | 
| **Sí**  | No |  Microsoft.Cache/redis  |  usedmemoryRss7  |  Memoria RSS usada (partición 7)  |  Bytes  |  Máxima | 
| **Sí**  | No |  Microsoft.Cache/redis  |  usedmemoryRss8  |  Memoria RSS usada (partición 8)  |  Bytes  |  Máxima | 
| **Sí**  | No |  Microsoft.Cache/redis  |  usedmemoryRss9  |  Memoria RSS usada (partición 9)  |  Bytes  |  Máxima | 
| No  | No |  Microsoft.ClassicCompute/domainNames/slots/roles  |  Disk Read Bytes/Sec  |  Lectura de disco  |  BytesPerSecond  |  Average | 
| **Sí**  | No |  Microsoft.ClassicCompute/domainNames/slots/roles  |  Operaciones de lectura de disco por segundo  |  Operaciones de lectura de disco por segundo  |  CountPerSecond  |  Average | 
| No  | No |  Microsoft.ClassicCompute/domainNames/slots/roles  |  Disk Write Bytes/Sec  |  Escritura de disco  |  BytesPerSecond  |  Average | 
| **Sí**  | No |  Microsoft.ClassicCompute/domainNames/slots/roles  |  Operaciones de escritura por segundo en disco  |  Operaciones de escritura por segundo en disco  |  CountPerSecond  |  Average | 
| **Sí**  | No |  Microsoft.ClassicCompute/domainNames/slots/roles  |  Red interna  |  Red interna  |  Bytes  |  Total | 
| **Sí**  | No |  Microsoft.ClassicCompute/domainNames/slots/roles  |  Red interna  |  Red interna  |  Bytes  |  Total | 
| **Sí**  | No |  Microsoft.ClassicCompute/domainNames/slots/roles  |  Porcentaje de CPU  |  Porcentaje de CPU  |  Percent  |  Average | 
| No  | No |  Microsoft.ClassicCompute/virtualMachines  |  Disk Read Bytes/Sec  |  Lectura de disco  |  BytesPerSecond  |  Average | 
| **Sí**  | No |  Microsoft.ClassicCompute/virtualMachines  |  Operaciones de lectura de disco por segundo  |  Operaciones de lectura de disco por segundo  |  CountPerSecond  |  Average | 
| No  | No |  Microsoft.ClassicCompute/virtualMachines  |  Disk Write Bytes/Sec  |  Escritura de disco  |  BytesPerSecond  |  Average | 
| **Sí**  | No |  Microsoft.ClassicCompute/virtualMachines  |  Operaciones de escritura por segundo en disco  |  Operaciones de escritura por segundo en disco  |  CountPerSecond  |  Average | 
| **Sí**  | No |  Microsoft.ClassicCompute/virtualMachines  |  Red interna  |  Red interna  |  Bytes  |  Total | 
| **Sí**  | No |  Microsoft.ClassicCompute/virtualMachines  |  Red interna  |  Red interna  |  Bytes  |  Total | 
| **Sí**  | No |  Microsoft.ClassicCompute/virtualMachines  |  Porcentaje de CPU  |  Porcentaje de CPU  |  Percent  |  Average | 
| **Sí**  | No |  Microsoft.ClassicStorage/storageAccounts  |  Disponibilidad  |  Disponibilidad  |  Percent  |  Average | 
| **Sí**  | No |  Microsoft.ClassicStorage/storageAccounts  |  Salida  |  Salida  |  Bytes  |  Total | 
| **Sí**  | No |  Microsoft.ClassicStorage/storageAccounts  |  Entrada  |  Entrada  |  Bytes  |  Total | 
| **Sí**  | No |  Microsoft.ClassicStorage/storageAccounts  |  SuccessE2ELatency  |  Latencia de E2E correcta  |  Milisegundos  |  Average | 
| **Sí**  | No |  Microsoft.ClassicStorage/storageAccounts  |  SuccessServerLatency  |  Latencia del servidor correcta  |  Milisegundos  |  Average | 
| **Sí**  | No |  Microsoft.ClassicStorage/storageAccounts  |  Transacciones  |  Transacciones  |  Count  |  Total | 
| No  | No |  Microsoft.ClassicStorage/storageAccounts  |  UsedCapacity  |  Capacidad usada  |  Bytes  |  Average | 
| **Sí**  | No |  Microsoft.ClassicStorage/storageAccounts/blobServices  |  Disponibilidad  |  Disponibilidad  |  Percent  |  Average | 
| No  | No |  Microsoft.ClassicStorage/storageAccounts/blobServices  |  BlobCapacity  |  Capacidad de Blob  |  Bytes  |  Average | 
| No  | No |  Microsoft.ClassicStorage/storageAccounts/blobServices  |  BlobCount  |  Recuento de blobs  |  Count  |  Average | 
| **Sí**  | No |  Microsoft.ClassicStorage/storageAccounts/blobServices  |  ContainerCount  |  Recuento de contenedores de blobs  |  Count  |  Average | 
| **Sí**  | No |  Microsoft.ClassicStorage/storageAccounts/blobServices  |  Salida  |  Salida  |  Bytes  |  Total | 
| No  | No |  Microsoft.ClassicStorage/storageAccounts/blobServices  |  IndexCapacity  |  Capacidad de índice  |  Bytes  |  Average | 
| **Sí**  | No |  Microsoft.ClassicStorage/storageAccounts/blobServices  |  Entrada  |  Entrada  |  Bytes  |  Total | 
| **Sí**  | No |  Microsoft.ClassicStorage/storageAccounts/blobServices  |  SuccessE2ELatency  |  Latencia de E2E correcta  |  Milisegundos  |  Average | 
| **Sí**  | No |  Microsoft.ClassicStorage/storageAccounts/blobServices  |  SuccessServerLatency  |  Latencia del servidor correcta  |  Milisegundos  |  Average | 
| **Sí**  | No |  Microsoft.ClassicStorage/storageAccounts/blobServices  |  Transacciones  |  Transacciones  |  Count  |  Total | 
| **Sí**  | No |  Microsoft.ClassicStorage/storageAccounts/fileServices  |  Disponibilidad  |  Disponibilidad  |  Percent  |  Average | 
| **Sí**  | No |  Microsoft.ClassicStorage/storageAccounts/fileServices  |  Salida  |  Salida  |  Bytes  |  Total | 
| No  | No |  Microsoft.ClassicStorage/storageAccounts/fileServices  |  FileCapacity  |  Capacidad de File  |  Bytes  |  Average | 
| No  | No |  Microsoft.ClassicStorage/storageAccounts/fileServices  |  FileCount  |  Recuento de archivos  |  Count  |  Average | 
| No  | No |  Microsoft.ClassicStorage/storageAccounts/fileServices  |  FileShareCount  |  Recuento de recursos compartidos de archivos  |  Count  |  Average | 
| No  | No |  Microsoft.ClassicStorage/storageAccounts/fileServices  |  FileShareQuota  |  File share quota size (Tamaño de cuota del recurso compartido de archivos)  |  Bytes  |  Average | 
| No  | No |  Microsoft.ClassicStorage/storageAccounts/fileServices  |  FileShareSnapshotCount  |  File Share Snapshot Count (Recuento de instantáneas del recurso compartido de archivos)  |  Count  |  Average | 
| No  | No |  Microsoft.ClassicStorage/storageAccounts/fileServices  |  FileShareSnapshotSize  |  File Share Snapshot Size (Tamaño de instantánea del recurso compartido de archivos)  |  Bytes  |  Average | 
| **Sí**  | No |  Microsoft.ClassicStorage/storageAccounts/fileServices  |  Entrada  |  Entrada  |  Bytes  |  Total | 
| **Sí**  | No |  Microsoft.ClassicStorage/storageAccounts/fileServices  |  SuccessE2ELatency  |  Latencia de E2E correcta  |  Milisegundos  |  Average | 
| **Sí**  | No |  Microsoft.ClassicStorage/storageAccounts/fileServices  |  SuccessServerLatency  |  Latencia del servidor correcta  |  Milisegundos  |  Average | 
| **Sí**  | No |  Microsoft.ClassicStorage/storageAccounts/fileServices  |  Transacciones  |  Transacciones  |  Count  |  Total | 
| **Sí**  | No |  Microsoft.ClassicStorage/storageAccounts/queueServices  |  Disponibilidad  |  Disponibilidad  |  Percent  |  Average | 
| **Sí**  | No |  Microsoft.ClassicStorage/storageAccounts/queueServices  |  Salida  |  Salida  |  Bytes  |  Total | 
| **Sí**  | No |  Microsoft.ClassicStorage/storageAccounts/queueServices  |  Entrada  |  Entrada  |  Bytes  |  Total | 
| **Sí**  | No |  Microsoft.ClassicStorage/storageAccounts/queueServices  |  QueueCapacity  |  Capacidad de Queue  |  Bytes  |  Average | 
| **Sí**  | No |  Microsoft.ClassicStorage/storageAccounts/queueServices  |  QueueCount  |  Recuento de colas  |  Count  |  Average | 
| **Sí**  | No |  Microsoft.ClassicStorage/storageAccounts/queueServices  |  QueueMessageCount  |  Recuento de mensajes de Queue  |  Count  |  Average | 
| **Sí**  | No |  Microsoft.ClassicStorage/storageAccounts/queueServices  |  SuccessE2ELatency  |  Latencia de E2E correcta  |  Milisegundos  |  Average | 
| **Sí**  | No |  Microsoft.ClassicStorage/storageAccounts/queueServices  |  SuccessServerLatency  |  Latencia del servidor correcta  |  Milisegundos  |  Average | 
| **Sí**  | No |  Microsoft.ClassicStorage/storageAccounts/queueServices  |  Transacciones  |  Transacciones  |  Count  |  Total | 
| **Sí**  | No |  Microsoft.ClassicStorage/storageAccounts/tableServices  |  Disponibilidad  |  Disponibilidad  |  Percent  |  Average | 
| **Sí**  | No |  Microsoft.ClassicStorage/storageAccounts/tableServices  |  Salida  |  Salida  |  Bytes  |  Total | 
| **Sí**  | No |  Microsoft.ClassicStorage/storageAccounts/tableServices  |  Entrada  |  Entrada  |  Bytes  |  Total | 
| **Sí**  | No |  Microsoft.ClassicStorage/storageAccounts/tableServices  |  SuccessE2ELatency  |  Latencia de E2E correcta  |  Milisegundos  |  Average | 
| **Sí**  | No |  Microsoft.ClassicStorage/storageAccounts/tableServices  |  SuccessServerLatency  |  Latencia del servidor correcta  |  Milisegundos  |  Average | 
| **Sí**  | No |  Microsoft.ClassicStorage/storageAccounts/tableServices  |  TableCapacity  |  Capacidad de Table  |  Bytes  |  Average | 
| **Sí**  | No |  Microsoft.ClassicStorage/storageAccounts/tableServices  |  TableCount  |  Recuento de tablas  |  Count  |  Average | 
| **Sí**  | No |  Microsoft.ClassicStorage/storageAccounts/tableServices  |  TableEntityCount  |  Recuento de entidades de Table  |  Count  |  Average | 
| **Sí**  | No |  Microsoft.ClassicStorage/storageAccounts/tableServices  |  Transacciones  |  Transacciones  |  Count  |  Total | 
| **Sí**  | No |  Microsoft.CognitiveServices/accounts  |  BlockedCalls  |  Llamadas bloqueadas  |  Count  |  Total | 
| **Sí**  | No |  Microsoft.CognitiveServices/accounts  |  CharactersTrained  |  Caracteres entrenados  |  Count  |  Total | 
| **Sí**  | No |  Microsoft.CognitiveServices/accounts  |  CharactersTranslated  |  Caracteres traducidos  |  Count  |  Total | 
| **Sí**  | No |  Microsoft.CognitiveServices/accounts  |  ClientErrors  |  Errores de cliente  |  Count  |  Total | 
| **Sí**  | No |  Microsoft.CognitiveServices/accounts  |  DataIn  |  Entrada de datos  |  Bytes  |  Total | 
| **Sí**  | No |  Microsoft.CognitiveServices/accounts  |  DataOut  |  Salida de datos  |  Bytes  |  Total | 
| **Sí**  | No |  Microsoft.CognitiveServices/accounts  |  Latencia  |  Latencia  |  MilliSeconds  |  Average | 
| **Sí**  | No |  Microsoft.CognitiveServices/accounts  |  ServerErrors  |  Errores del servidor  |  Count  |  Total | 
| **Sí**  | No |  Microsoft.CognitiveServices/accounts  |  SpeechSessionDuration  |  Duración de la sesión de voz  |  Segundos  |  Total | 
| **Sí**  | No |  Microsoft.CognitiveServices/accounts  |  SuccessfulCalls  |  Llamadas correctas  |  Count  |  Total | 
| **Sí**  | No |  Microsoft.CognitiveServices/accounts  |  TotalCalls  |  Total de llamadas  |  Count  |  Total | 
| **Sí**  | No |  Microsoft.CognitiveServices/accounts  |  TotalErrors  |  Total de errores  |  Count  |  Total | 
| **Sí**  | No |  Microsoft.CognitiveServices/accounts  |  TotalTokenCalls  |  Llamadas de token totales  |  Count  |  Total | 
| **Sí**  | No |  Microsoft.CognitiveServices/accounts  |  TotalTransactions  |  Transacciones totales  |  Count  |  Total | 
| **Sí**  | No |  Microsoft.Compute/virtualMachines  |  Créditos de CPU consumidos  |  Créditos de CPU consumidos  |  Count  |  Average | 
| **Sí**  | No |  Microsoft.Compute/virtualMachines  |  Créditos de CPU restantes  |  Créditos de CPU restantes  |  Count  |  Average | 
| **Sí**  | No |  Microsoft.Compute/virtualMachines  |  Profundidad de cola de discos de datos  |  Profundidad de cola de discos de datos (versión preliminar)  |  Count  |  Average | 
| **Sí**  | No |  Microsoft.Compute/virtualMachines  |  Bytes de lectura de discos de datos por segundo  |  Bytes de lectura de discos de datos por segundo (versión preliminar)  |  CountPerSecond  |  Average | 
| **Sí**  | No |  Microsoft.Compute/virtualMachines  |  Operaciones de lectura de discos de datos por segundo  |  Operaciones de lectura de discos de datos por segundo (versión preliminar)  |  CountPerSecond  |  Average | 
| **Sí**  | No |  Microsoft.Compute/virtualMachines  |  Bytes de escritura de discos de datos por segundo  |  Bytes de escritura de discos de datos por segundo (versión preliminar)  |  CountPerSecond  |  Average | 
| **Sí**  | No |  Microsoft.Compute/virtualMachines  |  Operaciones de escritura de discos de datos por segundo  |  Operaciones de lectura de discos de datos por segundo (versión preliminar)  |  CountPerSecond  |  Average | 
| **Sí**  | No |  Microsoft.Compute/virtualMachines  |  Bytes de lectura de disco  |  Bytes de lectura de disco  |  Bytes  |  Total | 
| **Sí**  | No |  Microsoft.Compute/virtualMachines  |  Operaciones de lectura de disco por segundo  |  Operaciones de lectura de disco por segundo  |  CountPerSecond  |  Average | 
| **Sí**  | No |  Microsoft.Compute/virtualMachines  |  Bytes de escritura de disco  |  Bytes de escritura de disco  |  Bytes  |  Total | 
| **Sí**  | No |  Microsoft.Compute/virtualMachines  |  Operaciones de escritura por segundo en disco  |  Operaciones de escritura por segundo en disco  |  CountPerSecond  |  Average | 
| **Sí**  | No |  Microsoft.Compute/virtualMachines  |  Flujos de entrada  |  Flujos de entrada  |  Count  |  Average | 
| **Sí**  | No |  Microsoft.Compute/virtualMachines  |  Tasa de creación máxima de flujos de entrada  |  Tasa de creación máxima de flujos de entrada (versión preliminar)  |  CountPerSecond  |  Average | 
| **Sí**  | No |  Microsoft.Compute/virtualMachines  |  Red interna  |  Network In Billable (Deprecated) [Entrada de red facturable (en desuso)]  |  Bytes  |  Total | 
| **Sí**  | No |  Microsoft.Compute/virtualMachines  |  Red entrante total  |  Red entrante total  |  Bytes  |  Total | 
| **Sí**  | No |  Microsoft.Compute/virtualMachines  |  Red interna  |  Network Out Billable (Deprecated) [Salida de red facturable (en desuso)]  |  Bytes  |  Total | 
| **Sí**  | No |  Microsoft.Compute/virtualMachines  |  Red saliente total  |  Red saliente total  |  Bytes  |  Total | 
| **Sí**  | No |  Microsoft.Compute/virtualMachines  |  Profundidad de cola de discos de SO  |  Profundidad de cola de discos de SO (versión preliminar)  |  Count  |  Average | 
| **Sí**  | No |  Microsoft.Compute/virtualMachines  |  Bytes de lectura de discos del sistema operativo por segundo  |  Bytes de lectura de discos del sistema operativo por segundo (versión preliminar)  |  CountPerSecond  |  Average | 
| **Sí**  | No |  Microsoft.Compute/virtualMachines  |  Operaciones de lectura de discos de SO por segundo  |  Operaciones de lectura de discos del sistema operativo por segundo (versión preliminar)  |  CountPerSecond  |  Average | 
| **Sí**  | No |  Microsoft.Compute/virtualMachines  |  Bytes de escritura en discos del SO por segundo  |  Bytes de escritura de discos del sistema operativo por segundo (versión preliminar)  |  CountPerSecond  |  Average | 
| **Sí**  | No |  Microsoft.Compute/virtualMachines  |  Operaciones de escritura en discos de SO por segundo  |  Operaciones de escritura de discos del sistema operativo por segundo (versión preliminar)  |  CountPerSecond  |  Average | 
| **Sí**  | No |  Microsoft.Compute/virtualMachines  |  Sistema operativo por disco QD  |  Profundidad de la cola de disco de SO (en desuso)  |  Count  |  Average | 
| **Sí**  | No |  Microsoft.Compute/virtualMachines  |  Sistema operativo por bytes de lectura de discos por segundo  |  Bytes de lectura de discos de SO por segundo (en desuso)  |  CountPerSecond  |  Average | 
| **Sí**  | No |  Microsoft.Compute/virtualMachines  |  Sistema operativo por operaciones de lectura de discos por segundo  |  Operaciones de lectura de discos de SO por segundo (en desuso)  |  CountPerSecond  |  Average | 
| **Sí**  | No |  Microsoft.Compute/virtualMachines  |  Sistema operativo por bytes de escritura de disco por segundo  |  Bytes de escritura de discos de SO por segundo (en desuso)  |  CountPerSecond  |  Average | 
| **Sí**  | No |  Microsoft.Compute/virtualMachines  |  Sistema operativo operaciones de escritura de discos por segundo  |  Operaciones de escritura de discos de SO por segundo (en desuso)  |  CountPerSecond  |  Average | 
| **Sí**  | No |  Microsoft.Compute/virtualMachines  |  Flujos de salida  |  Flujos de salida  |  Count  |  Average | 
| **Sí**  | No |  Microsoft.Compute/virtualMachines  |  Tasa de creación máxima de flujos de salida  |  Tasa de creación máxima de flujos de salida (versión preliminar)  |  CountPerSecond  |  Average | 
| **Sí**  | No |  Microsoft.Compute/virtualMachines  |  Por disco QD  |  Profundidad de la cola de disco de datos (en desuso)  |  Count  |  Average | 
| **Sí**  | No |  Microsoft.Compute/virtualMachines  |  Bytes de lectura de discos por segundo  |  Bytes de lectura de discos de datos por segundo (en desuso)  |  CountPerSecond  |  Average | 
| **Sí**  | No |  Microsoft.Compute/virtualMachines  |  Operaciones de lectura de discos por segundo  |  Operaciones de lectura de discos de datos por segundo (en desuso)  |  CountPerSecond  |  Average | 
| **Sí**  | No |  Microsoft.Compute/virtualMachines  |  Bytes de escritura de discos por segundo  |  Bytes de escritura de discos de datos por segundo (en desuso)  |  CountPerSecond  |  Average | 
| **Sí**  | No |  Microsoft.Compute/virtualMachines  |  Operaciones de escritura de discos por segundo  |  Operaciones de escritura de discos de datos por segundo (en desuso)  |  CountPerSecond  |  Average | 
| **Sí**  | No |  Microsoft.Compute/virtualMachines  |  Porcentaje de CPU  |  Porcentaje de CPU  |  Percent  |  Average | 
| **Sí**  | No |  Microsoft.Compute/virtualMachines  |  Acierto de lectura de la caché de discos de datos premium  |  Acierto de lectura de la caché de discos de datos premium (versión preliminar)  |  Percent  |  Average | 
| **Sí**  | No |  Microsoft.Compute/virtualMachines  |  Error de lectura de la caché de discos de datos premium  |  Error de lectura de la caché de discos de datos premium (versión preliminar)  |  Percent  |  Average | 
| **Sí**  | No |  Microsoft.Compute/virtualMachines  |  Acierto de lectura de la caché de discos de SO premium  |  Acierto de lectura de la caché de discos de SO premium (versión preliminar)  |  Percent  |  Average | 
| **Sí**  | No |  Microsoft.Compute/virtualMachines  |  Error de lectura de la caché de discos de SO premium  |  Error de lectura de la caché de discos de SO premium (versión preliminar)  |  Percent  |  Average | 
| **Sí**  | No |  Microsoft.Compute/virtualMachineScaleSets  |  Créditos de CPU consumidos  |  Créditos de CPU consumidos  |  Count  |  Average | 
| **Sí**  | No |  Microsoft.Compute/virtualMachineScaleSets  |  Créditos de CPU restantes  |  Créditos de CPU restantes  |  Count  |  Average | 
| **Sí**  | No |  Microsoft.Compute/virtualMachineScaleSets  |  Profundidad de cola de discos de datos  |  Profundidad de cola de discos de datos (versión preliminar)  |  Count  |  Average | 
| **Sí**  | No |  Microsoft.Compute/virtualMachineScaleSets  |  Bytes de lectura de discos de datos por segundo  |  Bytes de lectura de discos de datos por segundo (versión preliminar)  |  CountPerSecond  |  Average | 
| **Sí**  | No |  Microsoft.Compute/virtualMachineScaleSets  |  Operaciones de lectura de discos de datos por segundo  |  Operaciones de lectura de discos de datos por segundo (versión preliminar)  |  CountPerSecond  |  Average | 
| **Sí**  | No |  Microsoft.Compute/virtualMachineScaleSets  |  Bytes de escritura de discos de datos por segundo  |  Bytes de escritura de discos de datos por segundo (versión preliminar)  |  CountPerSecond  |  Average | 
| **Sí**  | No |  Microsoft.Compute/virtualMachineScaleSets  |  Operaciones de escritura de discos de datos por segundo  |  Operaciones de lectura de discos de datos por segundo (versión preliminar)  |  CountPerSecond  |  Average | 
| **Sí**  | No |  Microsoft.Compute/virtualMachineScaleSets  |  Bytes de lectura de disco  |  Bytes de lectura de disco  |  Bytes  |  Total | 
| **Sí**  | No |  Microsoft.Compute/virtualMachineScaleSets  |  Operaciones de lectura de disco por segundo  |  Operaciones de lectura de disco por segundo  |  CountPerSecond  |  Average | 
| **Sí**  | No |  Microsoft.Compute/virtualMachineScaleSets  |  Bytes de escritura de disco  |  Bytes de escritura de disco  |  Bytes  |  Total | 
| **Sí**  | No |  Microsoft.Compute/virtualMachineScaleSets  |  Operaciones de escritura por segundo en disco  |  Operaciones de escritura por segundo en disco  |  CountPerSecond  |  Average | 
| **Sí**  | No |  Microsoft.Compute/virtualMachineScaleSets  |  Flujos de entrada  |  Flujos de entrada  |  Count  |  Average | 
| **Sí**  | No |  Microsoft.Compute/virtualMachineScaleSets  |  Tasa de creación máxima de flujos de entrada  |  Tasa de creación máxima de flujos de entrada (versión preliminar)  |  CountPerSecond  |  Average | 
| **Sí**  | No |  Microsoft.Compute/virtualMachineScaleSets  |  Red interna  |  Network In Billable (Deprecated) [Entrada de red facturable (en desuso)]  |  Bytes  |  Total | 
| **Sí**  | No |  Microsoft.Compute/virtualMachineScaleSets  |  Red entrante total  |  Red entrante total  |  Bytes  |  Total | 
| **Sí**  | No |  Microsoft.Compute/virtualMachineScaleSets  |  Red interna  |  Network Out Billable (Deprecated) [Salida de red facturable (en desuso)]  |  Bytes  |  Total | 
| **Sí**  | No |  Microsoft.Compute/virtualMachineScaleSets  |  Red saliente total  |  Red saliente total  |  Bytes  |  Total | 
| **Sí**  | No |  Microsoft.Compute/virtualMachineScaleSets  |  Profundidad de cola de discos de SO  |  Profundidad de cola de discos de SO (versión preliminar)  |  Count  |  Average | 
| **Sí**  | No |  Microsoft.Compute/virtualMachineScaleSets  |  Bytes de lectura de discos del sistema operativo por segundo  |  Bytes de lectura de discos del sistema operativo por segundo (versión preliminar)  |  CountPerSecond  |  Average | 
| **Sí**  | No |  Microsoft.Compute/virtualMachineScaleSets  |  Operaciones de lectura de discos de SO por segundo  |  Operaciones de lectura de discos del sistema operativo por segundo (versión preliminar)  |  CountPerSecond  |  Average | 
| **Sí**  | No |  Microsoft.Compute/virtualMachineScaleSets  |  Bytes de escritura en discos del SO por segundo  |  Bytes de escritura de discos del sistema operativo por segundo (versión preliminar)  |  CountPerSecond  |  Average | 
| **Sí**  | No |  Microsoft.Compute/virtualMachineScaleSets  |  Operaciones de escritura en discos de SO por segundo  |  Operaciones de escritura de discos del sistema operativo por segundo (versión preliminar)  |  CountPerSecond  |  Average | 
| **Sí**  | No |  Microsoft.Compute/virtualMachineScaleSets  |  Sistema operativo por disco QD  |  Profundidad de la cola de disco de SO (en desuso)  |  Count  |  Average | 
| **Sí**  | No |  Microsoft.Compute/virtualMachineScaleSets  |  Sistema operativo por bytes de lectura de discos por segundo  |  Bytes de lectura de discos de SO por segundo (en desuso)  |  CountPerSecond  |  Average | 
| **Sí**  | No |  Microsoft.Compute/virtualMachineScaleSets  |  Sistema operativo por operaciones de lectura de discos por segundo  |  Operaciones de lectura de discos de SO por segundo (en desuso)  |  CountPerSecond  |  Average | 
| **Sí**  | No |  Microsoft.Compute/virtualMachineScaleSets  |  Sistema operativo por bytes de escritura de disco por segundo  |  Bytes de escritura de discos de SO por segundo (en desuso)  |  CountPerSecond  |  Average | 
| **Sí**  | No |  Microsoft.Compute/virtualMachineScaleSets  |  Sistema operativo operaciones de escritura de discos por segundo  |  Operaciones de escritura de discos de SO por segundo (en desuso)  |  CountPerSecond  |  Average | 
| **Sí**  | No |  Microsoft.Compute/virtualMachineScaleSets  |  Flujos de salida  |  Flujos de salida  |  Count  |  Average | 
| **Sí**  | No |  Microsoft.Compute/virtualMachineScaleSets  |  Tasa de creación máxima de flujos de salida  |  Tasa de creación máxima de flujos de salida (versión preliminar)  |  CountPerSecond  |  Average | 
| **Sí**  | No |  Microsoft.Compute/virtualMachineScaleSets  |  Por disco QD  |  Profundidad de la cola de disco de datos (en desuso)  |  Count  |  Average | 
| **Sí**  | No |  Microsoft.Compute/virtualMachineScaleSets  |  Bytes de lectura de discos por segundo  |  Bytes de lectura de discos de datos por segundo (en desuso)  |  CountPerSecond  |  Average | 
| **Sí**  | No |  Microsoft.Compute/virtualMachineScaleSets  |  Operaciones de lectura de discos por segundo  |  Operaciones de lectura de discos de datos por segundo (en desuso)  |  CountPerSecond  |  Average | 
| **Sí**  | No |  Microsoft.Compute/virtualMachineScaleSets  |  Bytes de escritura de discos por segundo  |  Bytes de escritura de discos de datos por segundo (en desuso)  |  CountPerSecond  |  Average | 
| **Sí**  | No |  Microsoft.Compute/virtualMachineScaleSets  |  Operaciones de escritura de discos por segundo  |  Operaciones de escritura de discos de datos por segundo (en desuso)  |  CountPerSecond  |  Average | 
| **Sí**  | No |  Microsoft.Compute/virtualMachineScaleSets  |  Porcentaje de CPU  |  Porcentaje de CPU  |  Percent  |  Average | 
| **Sí**  | No |  Microsoft.Compute/virtualMachineScaleSets  |  Acierto de lectura de la caché de discos de datos premium  |  Acierto de lectura de la caché de discos de datos premium (versión preliminar)  |  Percent  |  Average | 
| **Sí**  | No |  Microsoft.Compute/virtualMachineScaleSets  |  Error de lectura de la caché de discos de datos premium  |  Error de lectura de la caché de discos de datos premium (versión preliminar)  |  Percent  |  Average | 
| **Sí**  | No |  Microsoft.Compute/virtualMachineScaleSets  |  Acierto de lectura de la caché de discos de SO premium  |  Acierto de lectura de la caché de discos de SO premium (versión preliminar)  |  Percent  |  Average | 
| **Sí**  | No |  Microsoft.Compute/virtualMachineScaleSets  |  Error de lectura de la caché de discos de SO premium  |  Error de lectura de la caché de discos de SO premium (versión preliminar)  |  Percent  |  Average | 
| **Sí**  | No |  Microsoft.Compute/virtualMachineScaleSets/virtualMachines  |  Créditos de CPU consumidos  |  Créditos de CPU consumidos  |  Count  |  Average | 
| **Sí**  | No |  Microsoft.Compute/virtualMachineScaleSets/virtualMachines  |  Créditos de CPU restantes  |  Créditos de CPU restantes  |  Count  |  Average | 
| **Sí**  | No |  Microsoft.Compute/virtualMachineScaleSets/virtualMachines  |  Profundidad de cola de discos de datos  |  Profundidad de cola de discos de datos (versión preliminar)  |  Count  |  Average | 
| **Sí**  | No |  Microsoft.Compute/virtualMachineScaleSets/virtualMachines  |  Bytes de lectura de discos de datos por segundo  |  Bytes de lectura de discos de datos por segundo (versión preliminar)  |  CountPerSecond  |  Average | 
| **Sí**  | No |  Microsoft.Compute/virtualMachineScaleSets/virtualMachines  |  Operaciones de lectura de discos de datos por segundo  |  Operaciones de lectura de discos de datos por segundo (versión preliminar)  |  CountPerSecond  |  Average | 
| **Sí**  | No |  Microsoft.Compute/virtualMachineScaleSets/virtualMachines  |  Bytes de escritura de discos de datos por segundo  |  Bytes de escritura de discos de datos por segundo (versión preliminar)  |  CountPerSecond  |  Average | 
| **Sí**  | No |  Microsoft.Compute/virtualMachineScaleSets/virtualMachines  |  Operaciones de escritura de discos de datos por segundo  |  Operaciones de lectura de discos de datos por segundo (versión preliminar)  |  CountPerSecond  |  Average | 
| **Sí**  | No |  Microsoft.Compute/virtualMachineScaleSets/virtualMachines  |  Bytes de lectura de disco  |  Bytes de lectura de disco  |  Bytes  |  Total | 
| **Sí**  | No |  Microsoft.Compute/virtualMachineScaleSets/virtualMachines  |  Operaciones de lectura de disco por segundo  |  Operaciones de lectura de disco por segundo  |  CountPerSecond  |  Average | 
| **Sí**  | No |  Microsoft.Compute/virtualMachineScaleSets/virtualMachines  |  Bytes de escritura de disco  |  Bytes de escritura de disco  |  Bytes  |  Total | 
| **Sí**  | No |  Microsoft.Compute/virtualMachineScaleSets/virtualMachines  |  Operaciones de escritura por segundo en disco  |  Operaciones de escritura por segundo en disco  |  CountPerSecond  |  Average | 
| **Sí**  | No |  Microsoft.Compute/virtualMachineScaleSets/virtualMachines  |  Flujos de entrada  |  Flujos de entrada  |  Count  |  Average | 
| **Sí**  | No |  Microsoft.Compute/virtualMachineScaleSets/virtualMachines  |  Tasa de creación máxima de flujos de entrada  |  Tasa de creación máxima de flujos de entrada (versión preliminar)  |  CountPerSecond  |  Average | 
| **Sí**  | No |  Microsoft.Compute/virtualMachineScaleSets/virtualMachines  |  Red interna  |  Network In Billable (Deprecated) [Entrada de red facturable (en desuso)]  |  Bytes  |  Total | 
| **Sí**  | No |  Microsoft.Compute/virtualMachineScaleSets/virtualMachines  |  Red entrante total  |  Red entrante total  |  Bytes  |  Total | 
| **Sí**  | No |  Microsoft.Compute/virtualMachineScaleSets/virtualMachines  |  Red interna  |  Network Out Billable (Deprecated) [Salida de red facturable (en desuso)]  |  Bytes  |  Total | 
| **Sí**  | No |  Microsoft.Compute/virtualMachineScaleSets/virtualMachines  |  Red saliente total  |  Red saliente total  |  Bytes  |  Total | 
| **Sí**  | No |  Microsoft.Compute/virtualMachineScaleSets/virtualMachines  |  Profundidad de cola de discos de SO  |  Profundidad de cola de discos de SO (versión preliminar)  |  Count  |  Average | 
| **Sí**  | No |  Microsoft.Compute/virtualMachineScaleSets/virtualMachines  |  Bytes de lectura de discos del sistema operativo por segundo  |  Bytes de lectura de discos del sistema operativo por segundo (versión preliminar)  |  CountPerSecond  |  Average | 
| **Sí**  | No |  Microsoft.Compute/virtualMachineScaleSets/virtualMachines  |  Operaciones de lectura de discos de SO por segundo  |  Operaciones de lectura de discos del sistema operativo por segundo (versión preliminar)  |  CountPerSecond  |  Average | 
| **Sí**  | No |  Microsoft.Compute/virtualMachineScaleSets/virtualMachines  |  Bytes de escritura en discos del SO por segundo  |  Bytes de escritura de discos del sistema operativo por segundo (versión preliminar)  |  CountPerSecond  |  Average | 
| **Sí**  | No |  Microsoft.Compute/virtualMachineScaleSets/virtualMachines  |  Operaciones de escritura en discos de SO por segundo  |  Operaciones de escritura de discos del sistema operativo por segundo (versión preliminar)  |  CountPerSecond  |  Average | 
| **Sí**  | No |  Microsoft.Compute/virtualMachineScaleSets/virtualMachines  |  Sistema operativo por disco QD  |  Profundidad de la cola de disco de SO (en desuso)  |  Count  |  Average | 
| **Sí**  | No |  Microsoft.Compute/virtualMachineScaleSets/virtualMachines  |  Sistema operativo por bytes de lectura de discos por segundo  |  Bytes de lectura de discos de SO por segundo (en desuso)  |  CountPerSecond  |  Average | 
| **Sí**  | No |  Microsoft.Compute/virtualMachineScaleSets/virtualMachines  |  Sistema operativo por operaciones de lectura de discos por segundo  |  Operaciones de lectura de discos de SO por segundo (en desuso)  |  CountPerSecond  |  Average | 
| **Sí**  | No |  Microsoft.Compute/virtualMachineScaleSets/virtualMachines  |  Sistema operativo por bytes de escritura de disco por segundo  |  Bytes de escritura de discos de SO por segundo (en desuso)  |  CountPerSecond  |  Average | 
| **Sí**  | No |  Microsoft.Compute/virtualMachineScaleSets/virtualMachines  |  Sistema operativo operaciones de escritura de discos por segundo  |  Operaciones de escritura de discos de SO por segundo (en desuso)  |  CountPerSecond  |  Average | 
| **Sí**  | No |  Microsoft.Compute/virtualMachineScaleSets/virtualMachines  |  Flujos de salida  |  Flujos de salida  |  Count  |  Average | 
| **Sí**  | No |  Microsoft.Compute/virtualMachineScaleSets/virtualMachines  |  Tasa de creación máxima de flujos de salida  |  Tasa de creación máxima de flujos de salida (versión preliminar)  |  CountPerSecond  |  Average | 
| **Sí**  | No |  Microsoft.Compute/virtualMachineScaleSets/virtualMachines  |  Por disco QD  |  Profundidad de la cola de disco de datos (en desuso)  |  Count  |  Average | 
| **Sí**  | No |  Microsoft.Compute/virtualMachineScaleSets/virtualMachines  |  Bytes de lectura de discos por segundo  |  Bytes de lectura de discos de datos por segundo (en desuso)  |  CountPerSecond  |  Average | 
| **Sí**  | No |  Microsoft.Compute/virtualMachineScaleSets/virtualMachines  |  Operaciones de lectura de discos por segundo  |  Operaciones de lectura de discos de datos por segundo (en desuso)  |  CountPerSecond  |  Average | 
| **Sí**  | No |  Microsoft.Compute/virtualMachineScaleSets/virtualMachines  |  Bytes de escritura de discos por segundo  |  Bytes de escritura de discos de datos por segundo (en desuso)  |  CountPerSecond  |  Average | 
| **Sí**  | No |  Microsoft.Compute/virtualMachineScaleSets/virtualMachines  |  Operaciones de escritura de discos por segundo  |  Operaciones de escritura de discos de datos por segundo (en desuso)  |  CountPerSecond  |  Average | 
| **Sí**  | No |  Microsoft.Compute/virtualMachineScaleSets/virtualMachines  |  Porcentaje de CPU  |  Porcentaje de CPU  |  Percent  |  Average | 
| **Sí**  | No |  Microsoft.Compute/virtualMachineScaleSets/virtualMachines  |  Acierto de lectura de la caché de discos de datos premium  |  Acierto de lectura de la caché de discos de datos premium (versión preliminar)  |  Percent  |  Average | 
| **Sí**  | No |  Microsoft.Compute/virtualMachineScaleSets/virtualMachines  |  Error de lectura de la caché de discos de datos premium  |  Error de lectura de la caché de discos de datos premium (versión preliminar)  |  Percent  |  Average | 
| **Sí**  | No |  Microsoft.Compute/virtualMachineScaleSets/virtualMachines  |  Acierto de lectura de la caché de discos de SO premium  |  Acierto de lectura de la caché de discos de SO premium (versión preliminar)  |  Percent  |  Average | 
| **Sí**  | No |  Microsoft.Compute/virtualMachineScaleSets/virtualMachines  |  Error de lectura de la caché de discos de SO premium  |  Error de lectura de la caché de discos de SO premium (versión preliminar)  |  Percent  |  Average | 
| **Sí**  | No |  Microsoft.ContainerInstance/containerGroups  |  CpuUsage  |  Uso de CPU  |  Count  |  Average | 
| **Sí**  | No |  Microsoft.ContainerInstance/containerGroups  |  MemoryUsage  |  Uso de la memoria  |  Bytes  |  Average | 
| **Sí**  | No |  Microsoft.ContainerInstance/containerGroups  |  NetworkBytesReceivedPerSecond  |  Bytes de red recibidos por segundo  |  Bytes  |  Average | 
| **Sí**  | No |  Microsoft.ContainerInstance/containerGroups  |  NetworkBytesTransmittedPerSecond  |  Bytes de red transmitidos por segundo  |  Bytes  |  Average | 
| **Sí**  | No |  Microsoft.ContainerRegistry/registries  |  RunDuration  |  Duración de la ejecución  |  Milisegundos  |  Total | 
| **Sí**  | No |  Microsoft.ContainerRegistry/registries  |  SuccessfulPullCount  |  Recuento de extracciones correctas  |  Count  |  Average | 
| **Sí**  | No |  Microsoft.ContainerRegistry/registries  |  SuccessfulPushCount  |  Recuento de inserciones correctas  |  Count  |  Average | 
| **Sí**  | No |  Microsoft.ContainerRegistry/registries  |  TotalPullCount  |  Recuento total de extracciones  |  Count  |  Average | 
| **Sí**  | No |  Microsoft.ContainerRegistry/registries  |  TotalPushCount  |  Recuento total de inserciones  |  Count  |  Average | 
| No  | No |  Microsoft.ContainerService/managedClusters  |  kube_node_status_allocatable_cpu_cores  |  Número total de núcleos de CPU disponibles en un clúster administrado  |  Count  |  Average | 
| No  | No |  Microsoft.ContainerService/managedClusters  |  kube_node_status_allocatable_memory_bytes  |  Cantidad total de memoria disponible en un clúster administrado  |  Bytes  |  Average | 
| No  | No |  Microsoft.ContainerService/managedClusters  |  kube_node_status_condition  |  Estados de diversas condiciones de nodo  |  Count  |  Average | 
| No  | No |  Microsoft.ContainerService/managedClusters  |  kube_pod_status_phase  |  Número de pods por fase  |  Count  |  Average | 
| No  | No |  Microsoft.ContainerService/managedClusters  |  kube_pod_status_ready  |  Número de pods con estado Listo  |  Count  |  Average | 
| **Sí**  | No |  Microsoft.DataBoxEdge/dataBoxEdgeDevices  |  AvailableCapacity  |  Capacidad disponible  |  Bytes  |  Average | 
| **Sí**  | No |  Microsoft.DataBoxEdge/dataBoxEdgeDevices  |  BytesUploadedToCloud  |  Bytes cargados en la nube (dispositivo)  |  Bytes  |  Average | 
| **Sí**  | No |  Microsoft.DataBoxEdge/dataBoxEdgeDevices  |  BytesUploadedToCloudPerShare  |  Bytes cargados en la nube (recurso compartido)  |  Bytes  |  Average | 
| **Sí**  | No |  Microsoft.DataBoxEdge/dataBoxEdgeDevices  |  CloudReadThroughput  |  Rendimiento de descarga en la nube  |  BytesPerSecond  |  Average | 
| **Sí**  | No |  Microsoft.DataBoxEdge/dataBoxEdgeDevices  |  CloudReadThroughputPerShare  |  Rendimiento de descarga en la nube (recurso compartido)  |  BytesPerSecond  |  Average | 
| **Sí**  | No |  Microsoft.DataBoxEdge/dataBoxEdgeDevices  |  CloudUploadThroughput  |  Rendimiento de carga en la nube  |  BytesPerSecond  |  Average | 
| **Sí**  | No |  Microsoft.DataBoxEdge/dataBoxEdgeDevices  |  CloudUploadThroughputPerShare  |  Rendimiento de carga en la nube (recurso compartido)  |  BytesPerSecond  |  Average | 
| **Sí**  | No |  Microsoft.DataBoxEdge/dataBoxEdgeDevices  |  HyperVMemoryUtilization  |  Proceso perimetral: uso de memoria  |  Percent  |  Average | 
| **Sí**  | No |  Microsoft.DataBoxEdge/dataBoxEdgeDevices  |  HyperVVirtualProcessorUtilization  |  Proceso perimetral: porcentaje de CPU  |  Percent  |  Average | 
| **Sí**  | No |  Microsoft.DataBoxEdge/dataBoxEdgeDevices  |  NICReadThroughput  |  Rendimiento de lectura (red)  |  BytesPerSecond  |  Average | 
| **Sí**  | No |  Microsoft.DataBoxEdge/dataBoxEdgeDevices  |  NICWriteThroughput  |  Rendimiento de escritura (red)  |  BytesPerSecond  |  Average | 
| **Sí**  | No |  Microsoft.DataBoxEdge/dataBoxEdgeDevices  |  TotalCapacity  |  Capacidad total  |  Bytes  |  Average | 
| **Sí**  | No |  Microsoft.DataFactory/datafactories  |  FailedRuns  |  Ejecuciones con error  |  Count  |  Total | 
| **Sí**  | No |  Microsoft.DataFactory/datafactories  |  SuccessfulRuns  |  Ejecuciones correctas  |  Count  |  Total | 
| **Sí**  | No |  Microsoft.DataFactory/factories  |  ActivityCancelledRuns  |  Cancelled activity runs metrics (Métricas de ejecuciones de actividad canceladas)  |  Count  |  Total | 
| **Sí**  | No |  Microsoft.DataFactory/factories  |  ActivityFailedRuns  |  Métricas de ejecuciones de actividad erróneas  |  Count  |  Total | 
| **Sí**  | No |  Microsoft.DataFactory/factories  |  ActivitySucceededRuns  |  Métricas de ejecución de actividad realizadas correctamente  |  Count  |  Total | 
| **Sí**  | No |  Microsoft.DataFactory/factories  |  FactorySizeInGbUnits  |  Tamaño total de fábrica (unidad de GB)  |  Count  |  Máxima | 
| **Sí**  | No |  Microsoft.DataFactory/factories  |  IntegrationRuntimeAvailableMemory  |  Memoria disponible de entorno de ejecución de integración  |  Bytes  |  Average | 
| **Sí**  | No |  Microsoft.DataFactory/factories  |  IntegrationRuntimeAverageTaskPickupDelay  |  Integration runtime queue duration (Duración de cola de entorno de ejecución de integración)  |  Segundos  |  Average | 
| **Sí**  | No |  Microsoft.DataFactory/factories  |  IntegrationRuntimeCpuPercentage  |  Uso de la CPU de entorno de ejecución de integración  |  Percent  |  Average | 
| **Sí**  | No |  Microsoft.DataFactory/factories  |  IntegrationRuntimeQueueLength  |  Integration runtime queue length (Longitud de cola de entorno de ejecución de integración)  |  Count  |  Average | 
| **Sí**  | No |  Microsoft.DataFactory/factories  |  MaxAllowedFactorySizeInGbUnits  |  Tamaño de fábrica máximo permitido (unidad de GB)  |  Count  |  Máxima | 
| **Sí**  | No |  Microsoft.DataFactory/factories  |  MaxAllowedResourceCount  |  Recuento del máximo de entidades permitidas  |  Count  |  Máxima | 
| **Sí**  | No |  Microsoft.DataFactory/factories  |  PipelineCancelledRuns  |  Cancelled pipeline runs metrics (Métricas de ejecuciones de canalización canceladas)  |  Count  |  Total | 
| **Sí**  | No |  Microsoft.DataFactory/factories  |  PipelineFailedRuns  |  Métricas de ejecuciones de canalización erróneas  |  Count  |  Total | 
| **Sí**  | No |  Microsoft.DataFactory/factories  |  PipelineSucceededRuns  |  Las métricas de ejecuciones de canalización se realizaron correctamente  |  Count  |  Total | 
| **Sí**  | No |  Microsoft.DataFactory/factories  |  ResourceCount  |  Recuento total de entidades  |  Count  |  Máxima | 
| **Sí**  | No |  Microsoft.DataFactory/factories  |  TriggerCancelledRuns  |  Cancelled trigger runs metrics (Métricas de ejecuciones de desencadenador canceladas)  |  Count  |  Total | 
| **Sí**  | No |  Microsoft.DataFactory/factories  |  TriggerFailedRuns  |  Métricas de ejecuciones de desencadenador erróneas  |  Count  |  Total | 
| **Sí**  | No |  Microsoft.DataFactory/factories  |  TriggerSucceededRuns  |  Métricas de ejecuciones de desencadenador realizadas correctamente  |  Count  |  Total | 
| **Sí**  | No |  Microsoft.DataLakeAnalytics/accounts  |  JobAUEndedCancelled  |  Tiempo AU para cancelados  |  Segundos  |  Total | 
| **Sí**  | No |  Microsoft.DataLakeAnalytics/accounts  |  JobAUEndedFailure  |  Error de tiempo AU  |  Segundos  |  Total | 
| **Sí**  | No |  Microsoft.DataLakeAnalytics/accounts  |  JobAUEndedSuccess  |  Tiempo AU correcto  |  Segundos  |  Total | 
| **Sí**  | No |  Microsoft.DataLakeAnalytics/accounts  |  JobEndedCancelled  |  Trabajos cancelados  |  Count  |  Total | 
| **Sí**  | No |  Microsoft.DataLakeAnalytics/accounts  |  JobEndedFailure  |  Trabajos con error  |  Count  |  Total | 
| **Sí**  | No |  Microsoft.DataLakeAnalytics/accounts  |  JobEndedSuccess  |  Trabajos correctos  |  Count  |  Total | 
| **Sí**  | No |  Microsoft.DataLakeStore/accounts  |  DataRead  |  Datos leídos  |  Bytes  |  Total | 
| **Sí**  | No |  Microsoft.DataLakeStore/accounts  |  DataWritten  |  Datos escritos  |  Bytes  |  Total | 
| **Sí**  | No |  Microsoft.DataLakeStore/accounts  |  ReadRequests  |  Solicitudes de lectura  |  Count  |  Total | 
| **Sí**  | No |  Microsoft.DataLakeStore/accounts  |  TotalStorage  |  Almacenamiento total  |  Bytes  |  Máxima | 
| **Sí**  | No |  Microsoft.DataLakeStore/accounts  |  WriteRequests  |  Solicitudes de escritura  |  Count  |  Total | 
| **Sí**  | **Sí** |  Microsoft.DBforMariaDB/servers  |  active_connections  |  Conexiones activas  |  Count  |  Average | 
| **Sí**  | **Sí** |  Microsoft.DBforMariaDB/servers  |  backup_storage_used  |  Almacenamiento de copia de seguridad utilizado  |  Bytes  |  Average | 
| **Sí**  | **Sí** |  Microsoft.DBforMariaDB/servers  |  connections_failed  |  Conexiones con errores  |  Count  |  Total | 
| **Sí**  | **Sí** |  Microsoft.DBforMariaDB/servers  |  cpu_percent  |  Porcentaje de CPU  |  Percent  |  Average | 
| **Sí**  | **Sí** |  Microsoft.DBforMariaDB/servers  |  io_consumption_percent  |  Porcentaje de E/S  |  Percent  |  Average | 
| **Sí**  | **Sí** |  Microsoft.DBforMariaDB/servers  |  memory_percent  |  Porcentaje de memoria  |  Percent  |  Average | 
| **Sí**  | **Sí** |  Microsoft.DBforMariaDB/servers  |  network_bytes_egress  |  Red interna  |  Bytes  |  Total | 
| **Sí**  | **Sí** |  Microsoft.DBforMariaDB/servers  |  network_bytes_ingress  |  Red interna  |  Bytes  |  Total | 
| **Sí**  | **Sí** |  Microsoft.DBforMariaDB/servers  |  seconds_behind_master  |  Intervalo de replicación en segundos  |  Count  |  Máxima | 
| **Sí**  | **Sí** |  Microsoft.DBforMariaDB/servers  |  serverlog_storage_limit  |  Límite de almacenamiento del registro del servidor  |  Bytes  |  Average | 
| **Sí**  | **Sí** |  Microsoft.DBforMariaDB/servers  |  serverlog_storage_percent  |  Porcentaje de almacenamiento del registro del servidor  |  Percent  |  Average | 
| **Sí**  | **Sí** |  Microsoft.DBforMariaDB/servers  |  serverlog_storage_usage  |  Almacenamiento del registro del servidor usado  |  Bytes  |  Average | 
| **Sí**  | **Sí** |  Microsoft.DBforMariaDB/servers  |  storage_limit  |  Límite de almacenamiento  |  Bytes  |  Máxima | 
| **Sí**  | **Sí** |  Microsoft.DBforMariaDB/servers  |  storage_percent  |  Porcentaje de almacenamiento  |  Percent  |  Average | 
| **Sí**  | **Sí** |  Microsoft.DBforMariaDB/servers  |  storage_used  |  Almacenamiento utilizado  |  Bytes  |  Average | 
| **Sí**  | **Sí** |  Microsoft.DBforMySQL/servers  |  active_connections  |  Conexiones activas  |  Count  |  Average | 
| **Sí**  | **Sí** |  Microsoft.DBforMySQL/servers  |  backup_storage_used  |  Almacenamiento de copia de seguridad utilizado  |  Bytes  |  Average | 
| **Sí**  | **Sí** |  Microsoft.DBforMySQL/servers  |  connections_failed  |  Conexiones con errores  |  Count  |  Total | 
| **Sí**  | **Sí** |  Microsoft.DBforMySQL/servers  |  cpu_percent  |  Porcentaje de CPU  |  Percent  |  Average | 
| **Sí**  | **Sí** |  Microsoft.DBforMySQL/servers  |  io_consumption_percent  |  Porcentaje de E/S  |  Percent  |  Average | 
| **Sí**  | **Sí** |  Microsoft.DBforMySQL/servers  |  memory_percent  |  Porcentaje de memoria  |  Percent  |  Average | 
| **Sí**  | **Sí** |  Microsoft.DBforMySQL/servers  |  network_bytes_egress  |  Red interna  |  Bytes  |  Total | 
| **Sí**  | **Sí** |  Microsoft.DBforMySQL/servers  |  network_bytes_ingress  |  Red interna  |  Bytes  |  Total | 
| **Sí**  | **Sí** |  Microsoft.DBforMySQL/servers  |  seconds_behind_master  |  Intervalo de replicación en segundos  |  Count  |  Máxima | 
| **Sí**  | **Sí** |  Microsoft.DBforMySQL/servers  |  serverlog_storage_limit  |  Límite de almacenamiento del registro del servidor  |  Bytes  |  Máxima | 
| **Sí**  | **Sí** |  Microsoft.DBforMySQL/servers  |  serverlog_storage_percent  |  Porcentaje de almacenamiento del registro del servidor  |  Percent  |  Average | 
| **Sí**  | **Sí** |  Microsoft.DBforMySQL/servers  |  serverlog_storage_usage  |  Almacenamiento del registro del servidor usado  |  Bytes  |  Average | 
| **Sí**  | **Sí** |  Microsoft.DBforMySQL/servers  |  storage_limit  |  Límite de almacenamiento  |  Bytes  |  Máxima | 
| **Sí**  | **Sí** |  Microsoft.DBforMySQL/servers  |  storage_percent  |  Porcentaje de almacenamiento  |  Percent  |  Average | 
| **Sí**  | **Sí** |  Microsoft.DBforMySQL/servers  |  storage_used  |  Almacenamiento utilizado  |  Bytes  |  Average | 
| **Sí**  | No |  Microsoft.DBforPostgreSQL/servers  |  active_connections  |  Conexiones activas  |  Count  |  Average | 
| **Sí**  | No |  Microsoft.DBforPostgreSQL/servers  |  backup_storage_used  |  Almacenamiento de copia de seguridad utilizado  |  Bytes  |  Average | 
| **Sí**  | No |  Microsoft.DBforPostgreSQL/servers  |  connections_failed  |  Conexiones con errores  |  Count  |  Total | 
| **Sí**  | No |  Microsoft.DBforPostgreSQL/servers  |  cpu_percent  |  Porcentaje de CPU  |  Percent  |  Average | 
| **Sí**  | No |  Microsoft.DBforPostgreSQL/servers  |  io_consumption_percent  |  Porcentaje de E/S  |  Percent  |  Average | 
| **Sí**  | No |  Microsoft.DBforPostgreSQL/servers  |  memory_percent  |  Porcentaje de memoria  |  Percent  |  Average | 
| **Sí**  | No |  Microsoft.DBforPostgreSQL/servers  |  network_bytes_egress  |  Red interna  |  Bytes  |  Total | 
| **Sí**  | No |  Microsoft.DBforPostgreSQL/servers  |  network_bytes_ingress  |  Red interna  |  Bytes  |  Total | 
| **Sí**  | No |  Microsoft.DBforPostgreSQL/servers  |  pg_replica_log_delay_in_bytes  |  Retraso máximo entre réplicas  |  Bytes  |  Máxima | 
| **Sí**  | No |  Microsoft.DBforPostgreSQL/servers  |  pg_replica_log_delay_in_seconds  |  Retraso entre réplicas  |  Segundos  |  Máxima | 
| **Sí**  | No |  Microsoft.DBforPostgreSQL/servers  |  serverlog_storage_limit  |  Límite de almacenamiento del registro del servidor  |  Bytes  |  Máxima | 
| **Sí**  | No |  Microsoft.DBforPostgreSQL/servers  |  serverlog_storage_percent  |  Porcentaje de almacenamiento del registro del servidor  |  Percent  |  Average | 
| **Sí**  | No |  Microsoft.DBforPostgreSQL/servers  |  serverlog_storage_usage  |  Almacenamiento del registro del servidor usado  |  Bytes  |  Average | 
| **Sí**  | No |  Microsoft.DBforPostgreSQL/servers  |  storage_limit  |  Límite de almacenamiento  |  Bytes  |  Máxima | 
| **Sí**  | No |  Microsoft.DBforPostgreSQL/servers  |  storage_percent  |  Porcentaje de almacenamiento  |  Percent  |  Average | 
| **Sí**  | No |  Microsoft.DBforPostgreSQL/servers  |  storage_used  |  Almacenamiento utilizado  |  Bytes  |  Average | 
| **Sí**  | No |  Microsoft.DBforPostgreSQL/serversv2  |  active_connections  |  Conexiones activas  |  Count  |  Average | 
| **Sí**  | No |  Microsoft.DBforPostgreSQL/serversv2  |  cpu_percent  |  Porcentaje de CPU  |  Percent  |  Average | 
| **Sí**  | No |  Microsoft.DBforPostgreSQL/serversv2  |  iops  |  E/S  |  Count  |  Average | 
| **Sí**  | No |  Microsoft.DBforPostgreSQL/serversv2  |  memory_percent  |  Porcentaje de memoria  |  Percent  |  Average | 
| **Sí**  | No |  Microsoft.DBforPostgreSQL/serversv2  |  network_bytes_egress  |  Red interna  |  Bytes  |  Total | 
| **Sí**  | No |  Microsoft.DBforPostgreSQL/serversv2  |  network_bytes_ingress  |  Red interna  |  Bytes  |  Total | 
| **Sí**  | No |  Microsoft.DBforPostgreSQL/serversv2  |  storage_percent  |  Porcentaje de almacenamiento  |  Percent  |  Average | 
| **Sí**  | No |  Microsoft.DBforPostgreSQL/serversv2  |  storage_used  |  Almacenamiento utilizado  |  Bytes  |  Average | 
| **Sí**  | **Sí** |  Microsoft.Devices/Account  |  digitaltwins.telemetry.nodes  |  Digital Twins Node Telemetry Placeholder (Marcador de posición de telemetría de nodos de Digital Twins)  |  Count  |  Total | 
| **Sí**  | **Sí** |  Microsoft.Devices/IotHubs  |  c2d.commands.egress.abandon.success  |  C2D messages abandoned (Mensajes de C2D abandonados)  |  Count  |  Total | 
| **Sí**  | **Sí** |  Microsoft.Devices/IotHubs  |  c2d.commands.egress.complete.success  |  C2D message deliveries completed (Entregas de mensajes de C2D completadas)  |  Count  |  Total | 
| **Sí**  | **Sí** |  Microsoft.Devices/IotHubs  |  c2d.commands.egress.reject.success  |  C2D messages rejected (Mensajes de C2D rechazados)  |  Count  |  Total | 
| **Sí**  | **Sí** |  Microsoft.Devices/IotHubs  |  c2d.methods.failure  |  Invocaciones al método directo con error  |  Count  |  Total | 
| **Sí**  | **Sí** |  Microsoft.Devices/IotHubs  |  c2d.methods.requestSize  |  Tamaño de la solicitud de las invocaciones a métodos directos  |  Bytes  |  Average | 
| **Sí**  | **Sí** |  Microsoft.Devices/IotHubs  |  c2d.methods.responseSize  |  Tamaño de la respuesta de las invocaciones a métodos directos  |  Bytes  |  Average | 
| **Sí**  | **Sí** |  Microsoft.Devices/IotHubs  |  c2d.methods.success  |  Invocaciones correctas al método directo  |  Count  |  Total | 
| **Sí**  | **Sí** |  Microsoft.Devices/IotHubs  |  c2d.twin.read.failure  |  Lecturas gemelas con error del back-end  |  Count  |  Total | 
| **Sí**  | **Sí** |  Microsoft.Devices/IotHubs  |  c2d.twin.read.size  |  Tamaño de la respuesta de las lecturas gemelas del back-end  |  Bytes  |  Average | 
| **Sí**  | **Sí** |  Microsoft.Devices/IotHubs  |  c2d.twin.read.success  |  Lecturas gemelas correctas del back-end  |  Count  |  Total | 
| **Sí**  | **Sí** |  Microsoft.Devices/IotHubs  |  c2d.twin.update.failure  |  Actualizaciones gemelas con error del back-end  |  Count  |  Total | 
| **Sí**  | **Sí** |  Microsoft.Devices/IotHubs  |  c2d.twin.update.size  |  Tamaño de las actualizaciones gemelas del back-end  |  Bytes  |  Average | 
| **Sí**  | **Sí** |  Microsoft.Devices/IotHubs  |  c2d.twin.update.success  |  Actualizaciones gemelas correctas del back-end  |  Count  |  Total | 
| **Sí**  | **Sí** |  Microsoft.Devices/IotHubs  |  C2DMessagesExpired  |  C2D Messages Expired (preview) [Mensajes de C2D expirados (versión preliminar)]  |  Count  |  Total | 
| **Sí**  | **Sí** |  Microsoft.Devices/IotHubs  |  configuraciones  |  Métricas de configuración  |  Count  |  Total | 
| No  | No |  Microsoft.Devices/IotHubs  |  connectedDeviceCount  |  Dispositivos conectados (versión preliminar)  |  Count  |  Average | 
| **Sí**  | **Sí** |  Microsoft.Devices/IotHubs  |  d2c.endpoints.egress.builtIn.events  |  Enrutamiento: mensajes entregados a mensajes/eventos  |  Count  |  Total | 
| **Sí**  | **Sí** |  Microsoft.Devices/IotHubs  |  d2c.endpoints.egress.eventHubs  |  Enrutamiento: mensajes entregados al centro de eventos  |  Count  |  Total | 
| **Sí**  | **Sí** |  Microsoft.Devices/IotHubs  |  d2c.endpoints.egress.serviceBusQueues  |  Enrutamiento: mensajes entregados a la cola de Service Bus  |  Count  |  Total | 
| **Sí**  | **Sí** |  Microsoft.Devices/IotHubs  |  d2c.endpoints.egress.serviceBusTopics  |  Enrutamiento: mensajes entregados al tema de Service Bus  |  Count  |  Total | 
| **Sí**  | **Sí** |  Microsoft.Devices/IotHubs  |  d2c.endpoints.egress.storage  |  Enrutamiento: mensajes entregados al almacenamiento  |  Count  |  Total | 
| **Sí**  | **Sí** |  Microsoft.Devices/IotHubs  |  d2c.endpoints.egress.storage.blobs  |  Enrutamiento: blobs entregados al almacenamiento  |  Count  |  Total | 
| **Sí**  | **Sí** |  Microsoft.Devices/IotHubs  |  d2c.endpoints.egress.storage.bytes  |  Enrutamiento: datos entregados al almacenamiento  |  Bytes  |  Total | 
| **Sí**  | **Sí** |  Microsoft.Devices/IotHubs  |  d2c.endpoints.latency.builtIn.events  |  Enrutamiento: latencia de mensajes para mensajes/eventos  |  Milisegundos  |  Average | 
| **Sí**  | **Sí** |  Microsoft.Devices/IotHubs  |  d2c.endpoints.latency.eventHubs  |  Enrutamiento: latencia de mensajes para el centro de eventos  |  Milisegundos  |  Average | 
| **Sí**  | **Sí** |  Microsoft.Devices/IotHubs  |  d2c.endpoints.latency.serviceBusQueues  |  Enrutamiento: latencia de mensajes de la cola de Service Bus  |  Milisegundos  |  Average | 
| **Sí**  | **Sí** |  Microsoft.Devices/IotHubs  |  d2c.endpoints.latency.serviceBusTopics  |  Enrutamiento: latencia de mensajes del tema de Service Bus  |  Milisegundos  |  Average | 
| **Sí**  | **Sí** |  Microsoft.Devices/IotHubs  |  d2c.endpoints.latency.storage  |  Enrutamiento: latencia de mensajes para almacenamiento  |  Milisegundos  |  Average | 
| **Sí**  | **Sí** |  Microsoft.Devices/IotHubs  |  d2c.telemetry.egress.dropped  |  Enrutamiento: mensajes de telemetría quitados   |  Count  |  Total | 
| **Sí**  | **Sí** |  Microsoft.Devices/IotHubs  |  d2c.telemetry.egress.fallback  |  Enrutamiento: mensajes entregados a la reserva  |  Count  |  Total | 
| **Sí**  | **Sí** |  Microsoft.Devices/IotHubs  |  d2c.telemetry.egress.invalid  |  Enrutamiento: mensajes de telemetría incompatibles  |  Count  |  Total | 
| **Sí**  | **Sí** |  Microsoft.Devices/IotHubs  |  d2c.telemetry.egress.orphaned  |  Enrutamiento: mensajes de telemetría huérfanos   |  Count  |  Total | 
| **Sí**  | **Sí** |  Microsoft.Devices/IotHubs  |  d2c.telemetry.egress.success  |  Enrutamiento: mensajes de telemetría entregados  |  Count  |  Total | 
| **Sí**  | **Sí** |  Microsoft.Devices/IotHubs  |  d2c.telemetry.ingress.allProtocol  |  Intentos de envío de mensajes de telemetría  |  Count  |  Total | 
| **Sí**  | **Sí** |  Microsoft.Devices/IotHubs  |  d2c.telemetry.ingress.sendThrottle  |  Número de errores de limitación  |  Count  |  Total | 
| **Sí**  | **Sí** |  Microsoft.Devices/IotHubs  |  d2c.telemetry.ingress.success  |  Mensajes de telemetría enviados  |  Count  |  Total | 
| **Sí**  | **Sí** |  Microsoft.Devices/IotHubs  |  d2c.twin.read.failure  |  Lecturas gemelas con error de los dispositivos  |  Count  |  Total | 
| **Sí**  | **Sí** |  Microsoft.Devices/IotHubs  |  d2c.twin.read.size  |  Tamaño de la respuesta de las lecturas gemelas de dispositivos  |  Bytes  |  Average | 
| **Sí**  | **Sí** |  Microsoft.Devices/IotHubs  |  d2c.twin.read.success  |  Lecturas gemelas correctas de los dispositivos  |  Count  |  Total | 
| **Sí**  | **Sí** |  Microsoft.Devices/IotHubs  |  d2c.twin.update.failure  |  Actualizaciones gemelas con error de los dispositivos  |  Count  |  Total | 
| **Sí**  | **Sí** |  Microsoft.Devices/IotHubs  |  d2c.twin.update.size  |  Tamaño de las actualizaciones gemelas de los dispositivos  |  Bytes  |  Average | 
| **Sí**  | **Sí** |  Microsoft.Devices/IotHubs  |  d2c.twin.update.success  |  Actualizaciones gemelas correctas de los dispositivos  |  Count  |  Total | 
| **Sí**  | **Sí** |  Microsoft.Devices/IotHubs  |  dailyMessageQuotaUsed  |  Número total de mensajes usados  |  Count  |  Average | 
| **Sí**  | **Sí** |  Microsoft.Devices/IotHubs  |  deviceDataUsage  |  Uso total de datos del dispositivo  |  Bytes  |  Total | 
| **Sí**  | **Sí** |  Microsoft.Devices/IotHubs  |  deviceDataUsageV2  |  Uso total de datos del dispositivo (versión preliminar)  |  Bytes  |  Total | 
| **Sí**  | **Sí** |  Microsoft.Devices/IotHubs  |  devices.connectedDevices.allProtocol  |  Dispositivos conectados (en desuso)   |  Count  |  Total | 
| **Sí**  | **Sí** |  Microsoft.Devices/IotHubs  |  devices.totalDevices  |  Total de dispositivos (en desuso)  |  Count  |  Total | 
| **Sí**  | **Sí** |  Microsoft.Devices/IotHubs  |  EventGridDeliveries  |  Event Grid deliveries(preview) [Entregas de Event Grid (versión preliminar)]  |  Count  |  Total | 
| **Sí**  | **Sí** |  Microsoft.Devices/IotHubs  |  EventGridLatency  |  Latencia de Event Grid (versión preliminar)  |  Milisegundos  |  Average | 
| **Sí**  | **Sí** |  Microsoft.Devices/IotHubs  |  jobs.cancelJob.failure  |  Cancelaciones de trabajos con error  |  Count  |  Total | 
| **Sí**  | **Sí** |  Microsoft.Devices/IotHubs  |  jobs.cancelJob.success  |  Cancelaciones de trabajos correctas  |  Count  |  Total | 
| **Sí**  | **Sí** |  Microsoft.Devices/IotHubs  |  jobs.completed  |  Trabajos completados  |  Count  |  Total | 
| **Sí**  | **Sí** |  Microsoft.Devices/IotHubs  |  jobs.createDirectMethodJob.failure  |  Creaciones con error de trabajos de invocación de método  |  Count  |  Total | 
| **Sí**  | **Sí** |  Microsoft.Devices/IotHubs  |  jobs.createDirectMethodJob.success  |  Creaciones correctas de trabajos de invocación de método  |  Count  |  Total | 
| **Sí**  | **Sí** |  Microsoft.Devices/IotHubs  |  jobs.createTwinUpdateJob.failure  |  Creaciones con error de trabajos de actualización gemelos  |  Count  |  Total | 
| **Sí**  | **Sí** |  Microsoft.Devices/IotHubs  |  jobs.createTwinUpdateJob.success  |  Creaciones correctas de trabajos de actualización gemelos  |  Count  |  Total | 
| **Sí**  | **Sí** |  Microsoft.Devices/IotHubs  |  jobs.failed  |  Trabajos con error  |  Count  |  Total | 
| **Sí**  | **Sí** |  Microsoft.Devices/IotHubs  |  jobs.listJobs.failure  |  Llamadas con error para enumerar trabajos  |  Count  |  Total | 
| **Sí**  | **Sí** |  Microsoft.Devices/IotHubs  |  jobs.listJobs.success  |  Llamadas correctas para enumerar trabajos  |  Count  |  Total | 
| **Sí**  | **Sí** |  Microsoft.Devices/IotHubs  |  jobs.queryJobs.failure  |  Consultas de trabajo con error  |  Count  |  Total | 
| **Sí**  | **Sí** |  Microsoft.Devices/IotHubs  |  jobs.queryJobs.success  |  Consultas de trabajo correctas  |  Count  |  Total | 
| No  | No |  Microsoft.Devices/IotHubs  |  totalDeviceCount  |  Total de dispositivos (versión preliminar)  |  Count  |  Average | 
| **Sí**  | **Sí** |  Microsoft.Devices/IotHubs  |  twinQueries.failure  |  Consultas gemelas con error  |  Count  |  Total | 
| **Sí**  | **Sí** |  Microsoft.Devices/IotHubs  |  twinQueries.resultSize  |  Tamaño de resultado de consultas gemelas  |  Bytes  |  Average | 
| **Sí**  | **Sí** |  Microsoft.Devices/IotHubs  |  twinQueries.success  |  Consultas gemelas correctas  |  Count  |  Total | 
| **Sí**  | **Sí** |  Microsoft.Devices/provisioningServices  |  AttestationAttempts  |  Intentos de atestación  |  Count  |  Total | 
| **Sí**  | **Sí** |  Microsoft.Devices/provisioningServices  |  DeviceAssignments  |  Dispositivos asignados  |  Count  |  Total | 
| **Sí**  | **Sí** |  Microsoft.Devices/provisioningServices  |  RegistrationAttempts  |  Intentos de registro  |  Count  |  Total | 
| No  | No |  Microsoft.DocumentDB/databaseAccounts  |  AvailableStorage  |  Almacenamiento disponible  |  Bytes  |  Total | 
| No  | No |  Microsoft.DocumentDB/databaseAccounts  |  CassandraConnectionClosures  |  Cierres de conexión de Cassandra  |  Count  |  Total | 
| No  | No |  Microsoft.DocumentDB/databaseAccounts  |  CassandraRequestCharges  |  Cargos de solicitud de Cassandra  |  Count  |  Total | 
| No  | No |  Microsoft.DocumentDB/databaseAccounts  |  CassandraRequests  |  Solicitudes de Cassandra  |  Count  |  Count | 
| No  | No |  Microsoft.DocumentDB/databaseAccounts  |  DataUsage  |  Uso de datos  |  Bytes  |  Total | 
| **Sí**  | No |  Microsoft.DocumentDB/databaseAccounts  |  DeleteVirtualNetwork  |  DeleteVirtualNetwork  |  Count  |  Count | 
| No  | No |  Microsoft.DocumentDB/databaseAccounts  |  DocumentCount  |  Recuento de documentos  |  Count  |  Total | 
| No  | No |  Microsoft.DocumentDB/databaseAccounts  |  DocumentQuota  |  Cuota de documentos  |  Bytes  |  Total | 
| No  | No |  Microsoft.DocumentDB/databaseAccounts  |  IndexUsage  |  Uso de índice  |  Bytes  |  Total | 
| No  | No |  Microsoft.DocumentDB/databaseAccounts  |  MetadataRequests  |  Solicitudes de metadatos  |  Count  |  Count | 
| **Sí**  | **Sí** |  Microsoft.DocumentDB/databaseAccounts  |  MongoRequestCharge  |  Cargo de la solicitud de Mongo  |  Count  |  Total | 
| **Sí**  | **Sí** |  Microsoft.DocumentDB/databaseAccounts  |  MongoRequests  |  Solicitudes de Mongo  |  Count  |  Count | 
| No  | No |  Microsoft.DocumentDB/databaseAccounts  |  MongoRequestsCount  |  Mongo Request Rate (Velocidad de solicitudes de Mongo)  |  CountPerSecond  |  Average | 
| No  | No |  Microsoft.DocumentDB/databaseAccounts  |  MongoRequestsDelete  |  Velocidad de la solicitud de eliminación de Mongo  |  CountPerSecond  |  Average | 
| No  | No |  Microsoft.DocumentDB/databaseAccounts  |  MongoRequestsInsert  |  Velocidad de la solicitud de inserción de Mongo  |  CountPerSecond  |  Average | 
| No  | No |  Microsoft.DocumentDB/databaseAccounts  |  MongoRequestsQuery  |  Velocidad de la solicitud de consulta de Mongo  |  CountPerSecond  |  Average | 
| No  | No |  Microsoft.DocumentDB/databaseAccounts  |  MongoRequestsUpdate  |  Mongo Update Request Rate (Velocidad de solicitudes de actualización de Mongo)  |  CountPerSecond  |  Average | 
| No  | No |  Microsoft.DocumentDB/databaseAccounts  |  ProvisionedThroughput  |  Rendimiento aprovisionado  |  Count  |  Máxima | 
| **Sí**  | No |  Microsoft.DocumentDB/databaseAccounts  |  ReplicationLatency  |  Latencia de replicación P99  |  MilliSeconds  |  Average | 
| No  | No |  Microsoft.DocumentDB/databaseAccounts  |  ServiceAvailability  |  Disponibilidad del servicio  |  Percent  |  Average | 
| **Sí**  | **Sí** |  Microsoft.DocumentDB/databaseAccounts  |  TotalRequests  |  Total de solicitudes  |  Count  |  Count | 
| **Sí**  | **Sí** |  Microsoft.DocumentDB/databaseAccounts  |  TotalRequestUnits  |  Unidades de solicitud totales  |  Count  |  Total | 
| No  | No |  Microsoft.EnterpriseKnowledgeGraph/services  |  FailureCount  |  Failure Count (Recuento de errores)  |  Count  |  Count | 
| No  | No |  Microsoft.EnterpriseKnowledgeGraph/services  |  SuccessCount  |  Success Count (Recuento de transacciones correctas)  |  Count  |  Count | 
| No  | No |  Microsoft.EnterpriseKnowledgeGraph/services  |  SuccessLatency  |  Success Latency (Latencia de transacciones correctas)  |  MilliSeconds  |  Average | 
| No  | No |  Microsoft.EnterpriseKnowledgeGraph/services  |  TransactionCount  |  Transaction Count (Recuento de transacciones)  |  Count  |  Count | 
| **Sí**  | **Sí** |  Microsoft.EventGrid/domains  |  DeadLetteredCount  |  Eventos en la cola de mensajes fallidos  |  Count  |  Total | 
| No  | No |  Microsoft.EventGrid/domains  |  DeliveryAttemptFailCount  |  Eventos de error en la entrega  |  Count  |  Total | 
| **Sí**  | **Sí** |  Microsoft.EventGrid/domains  |  DeliverySuccessCount  |  Eventos entregados  |  Count  |  Total | 
| No  | No |  Microsoft.EventGrid/domains  |  DestinationProcessingDurationInMs  |  Duración del procesamiento de destino  |  Milisegundos  |  Average | 
| **Sí**  | **Sí** |  Microsoft.EventGrid/domains  |  DroppedEventCount  |  Eventos quitados  |  Count  |  Total | 
| **Sí**  | **Sí** |  Microsoft.EventGrid/domains  |  MatchedEventCount  |  Eventos coincidentes  |  Count  |  Total | 
| **Sí**  | **Sí** |  Microsoft.EventGrid/domains  |  PublishFailCount  |  Eventos no publicados  |  Count  |  Total | 
| **Sí**  | **Sí** |  Microsoft.EventGrid/domains  |  PublishSuccessCount  |  Eventos publicados  |  Count  |  Total | 
| **Sí**  | **Sí** |  Microsoft.EventGrid/domains  |  PublishSuccessLatencyInMs  |  Latencia de publicación correcta  |  Count  |  Total | 
| **Sí**  | **Sí** |  Microsoft.EventGrid/eventSubscriptions  |  DeadLetteredCount  |  Eventos en la cola de mensajes fallidos  |  Count  |  Total | 
| No  | No |  Microsoft.EventGrid/eventSubscriptions  |  DeliveryAttemptFailCount  |  Eventos de error en la entrega  |  Count  |  Total | 
| **Sí**  | **Sí** |  Microsoft.EventGrid/eventSubscriptions  |  DeliverySuccessCount  |  Eventos entregados  |  Count  |  Total | 
| No  | No |  Microsoft.EventGrid/eventSubscriptions  |  DestinationProcessingDurationInMs  |  Duración del procesamiento de destino  |  Milisegundos  |  Average | 
| **Sí**  | **Sí** |  Microsoft.EventGrid/eventSubscriptions  |  DroppedEventCount  |  Eventos quitados  |  Count  |  Total | 
| **Sí**  | **Sí** |  Microsoft.EventGrid/eventSubscriptions  |  MatchedEventCount  |  Eventos coincidentes  |  Count  |  Total | 
| **Sí**  | **Sí** |  Microsoft.EventGrid/extensionTopics  |  PublishFailCount  |  Eventos no publicados  |  Count  |  Total | 
| **Sí**  | **Sí** |  Microsoft.EventGrid/extensionTopics  |  PublishSuccessCount  |  Eventos publicados  |  Count  |  Total | 
| **Sí**  | **Sí** |  Microsoft.EventGrid/extensionTopics  |  PublishSuccessLatencyInMs  |  Latencia de publicación correcta  |  Count  |  Total | 
| **Sí**  | **Sí** |  Microsoft.EventGrid/extensionTopics  |  UnmatchedEventCount  |  Eventos no coincidentes  |  Count  |  Total | 
| **Sí**  | **Sí** |  Microsoft.EventGrid/topics  |  PublishFailCount  |  Eventos no publicados  |  Count  |  Total | 
| **Sí**  | **Sí** |  Microsoft.EventGrid/topics  |  PublishSuccessCount  |  Eventos publicados  |  Count  |  Total | 
| **Sí**  | **Sí** |  Microsoft.EventGrid/topics  |  PublishSuccessLatencyInMs  |  Latencia de publicación correcta  |  Count  |  Total | 
| **Sí**  | **Sí** |  Microsoft.EventGrid/topics  |  UnmatchedEventCount  |  Eventos no coincidentes  |  Count  |  Total | 
| No  | No |  Microsoft.EventHub/clusters  |  ActiveConnections  |  ActiveConnections  |  Count  |  Average | 
| No  | No |  Microsoft.EventHub/clusters  |  AvailableMemory  |  Memoria disponible  |  Percent  |  Máxima | 
| No  | No |  Microsoft.EventHub/clusters  |  CaptureBacklog  |  Capturar el trabajo pendiente.  |  Count  |  Total | 
| No  | No |  Microsoft.EventHub/clusters  |  CapturedBytes  |  Bytes capturados.  |  Bytes  |  Total | 
| No  | No |  Microsoft.EventHub/clusters  |  CapturedMessages  |  Mensajes capturados.  |  Count  |  Total | 
| No  | No |  Microsoft.EventHub/clusters  |  ConnectionsClosed  |  Conexiones cerradas.  |  Count  |  Average | 
| No  | No |  Microsoft.EventHub/clusters  |  ConnectionsOpened  |  Conexiones abiertas.  |  Count  |  Average | 
| No  | No |  Microsoft.EventHub/clusters  |  CPU  |  CPU  |  Percent  |  Máxima | 
| **Sí**  | **Sí** |  Microsoft.EventHub/clusters  |  IncomingBytes  |  Bytes entrantes.  |  Bytes  |  Total | 
| **Sí**  | **Sí** |  Microsoft.EventHub/clusters  |  IncomingMessages  |  Mensajes entrantes  |  Count  |  Total | 
| **Sí**  | **Sí** |  Microsoft.EventHub/clusters  |  IncomingRequests  |  Solicitudes entrantes  |  Count  |  Total | 
| **Sí**  | **Sí** |  Microsoft.EventHub/clusters  |  OutgoingBytes  |  Bytes salientes.  |  Bytes  |  Total | 
| **Sí**  | **Sí** |  Microsoft.EventHub/clusters  |  OutgoingMessages  |  Mensajes salientes  |  Count  |  Total | 
| No  | No |  Microsoft.EventHub/clusters  |  QuotaExceededErrors  |  Cuota de errores superada.  |  Count  |  Total | 
| No  | No |  Microsoft.EventHub/clusters  |  ServerErrors  |  Errores del servidor.  |  Count  |  Total | 
| No  | No |  Microsoft.EventHub/clusters  |  SuccessfulRequests  |  Solicitudes correctas  |  Count  |  Total | 
| No  | No |  Microsoft.EventHub/clusters  |  ThrottledRequests  |  Solicitudes limitadas.  |  Count  |  Total | 
| No  | No |  Microsoft.EventHub/clusters  |  UserErrors  |  Errores de usuario.  |  Count  |  Total | 
| No  | No |  Microsoft.EventHub/namespaces  |  ActiveConnections  |  ActiveConnections  |  Count  |  Average | 
| No  | No |  Microsoft.EventHub/namespaces  |  CaptureBacklog  |  Capturar el trabajo pendiente.  |  Count  |  Total | 
| No  | No |  Microsoft.EventHub/namespaces  |  CapturedBytes  |  Bytes capturados.  |  Bytes  |  Total | 
| No  | No |  Microsoft.EventHub/namespaces  |  CapturedMessages  |  Mensajes capturados.  |  Count  |  Total | 
| No  | No |  Microsoft.EventHub/namespaces  |  ConnectionsClosed  |  Conexiones cerradas.  |  Count  |  Average | 
| No  | No |  Microsoft.EventHub/namespaces  |  ConnectionsOpened  |  Conexiones abiertas.  |  Count  |  Average | 
| **Sí**  | **Sí** |  Microsoft.EventHub/namespaces  |  EHABL  |  Mensajes de trabajo pendiente de archivado (en desuso)  |  Count  |  Total | 
| **Sí**  | **Sí** |  Microsoft.EventHub/namespaces  |  EHAMBS  |  Rendimiento de mensajes de archivado (en desuso)  |  Bytes  |  Total | 
| **Sí**  | **Sí** |  Microsoft.EventHub/namespaces  |  EHAMSGS  |  Mensajes de archivado (en desuso)  |  Count  |  Total | 
| **Sí**  | **Sí** |  Microsoft.EventHub/namespaces  |  EHINBYTES  |  Bytes de entrada (en desuso)  |  Bytes  |  Total | 
| **Sí**  | **Sí** |  Microsoft.EventHub/namespaces  |  EHINMBS  |  Incoming bytes (obsolete) (Deprecated) [Bytes entrantes (obsoletos) (en desuso)]  |  Bytes  |  Total | 
| **Sí**  | **Sí** |  Microsoft.EventHub/namespaces  |  EHINMSGS  |  Mensajes entrantes (en desuso)  |  Count  |  Total | 
| **Sí**  | **Sí** |  Microsoft.EventHub/namespaces  |  EHOUTBYTES  |  Bytes de salida (en desuso)  |  Bytes  |  Total | 
| **Sí**  | **Sí** |  Microsoft.EventHub/namespaces  |  EHOUTMBS  |  Outgoing bytes (obsolete) (Deprecated) [Bytes salientes (obsoletos) (en desuso)]  |  Bytes  |  Total | 
| **Sí**  | **Sí** |  Microsoft.EventHub/namespaces  |  EHOUTMSGS  |  Mensajes salientes (en desuso)  |  Count  |  Total | 
| **Sí**  | **Sí** |  Microsoft.EventHub/namespaces  |  FAILREQ  |  Solicitudes con error (en desuso)  |  Count  |  Total | 
| **Sí**  | **Sí** |  Microsoft.EventHub/namespaces  |  IncomingBytes  |  Bytes entrantes.  |  Bytes  |  Total | 
| **Sí**  | **Sí** |  Microsoft.EventHub/namespaces  |  IncomingMessages  |  Mensajes entrantes  |  Count  |  Total | 
| **Sí**  | **Sí** |  Microsoft.EventHub/namespaces  |  IncomingRequests  |  Solicitudes entrantes  |  Count  |  Total | 
| **Sí**  | **Sí** |  Microsoft.EventHub/namespaces  |  INMSGS  |  Incoming Messages (obsolete) (Deprecated) [Mensajes entrantes (obsoletos) (en desuso)]  |  Count  |  Total | 
| **Sí**  | **Sí** |  Microsoft.EventHub/namespaces  |  INREQS  |  Solicitudes entrantes (en desuso)  |  Count  |  Total | 
| **Sí**  | **Sí** |  Microsoft.EventHub/namespaces  |  INTERR  |  Errores internos del servidor (en desuso)  |  Count  |  Total | 
| **Sí**  | **Sí** |  Microsoft.EventHub/namespaces  |  MISCERR  |  Otros errores (en desuso)  |  Count  |  Total | 
| **Sí**  | **Sí** |  Microsoft.EventHub/namespaces  |  OutgoingBytes  |  Bytes salientes.  |  Bytes  |  Total | 
| **Sí**  | **Sí** |  Microsoft.EventHub/namespaces  |  OutgoingMessages  |  Mensajes salientes  |  Count  |  Total | 
| **Sí**  | **Sí** |  Microsoft.EventHub/namespaces  |  OUTMSGS  |  Outgoing Messages (obsolete) (Deprecated) [Mensajes salientes (obsoletos) (en desuso)]  |  Count  |  Total | 
| No  | No |  Microsoft.EventHub/namespaces  |  QuotaExceededErrors  |  Cuota de errores superada.  |  Count  |  Total | 
| No  | No |  Microsoft.EventHub/namespaces  |  ServerErrors  |  Errores del servidor.  |  Count  |  Total | 
| No  | No |  Microsoft.EventHub/namespaces  |  Size  |  Size  |  Bytes  |  Average | 
| No  | No |  Microsoft.EventHub/namespaces  |  SuccessfulRequests  |  Solicitudes correctas  |  Count  |  Total | 
| **Sí**  | **Sí** |  Microsoft.EventHub/namespaces  |  SUCCREQ  |  Solicitudes correctas (en desuso)  |  Count  |  Total | 
| **Sí**  | **Sí** |  Microsoft.EventHub/namespaces  |  SVRBSY  |  Errores de servidor ocupado (en desuso)  |  Count  |  Total | 
| No  | No |  Microsoft.EventHub/namespaces  |  ThrottledRequests  |  Solicitudes limitadas.  |  Count  |  Total | 
| No  | No |  Microsoft.EventHub/namespaces  |  UserErrors  |  Errores de usuario.  |  Count  |  Total | 
| **Sí**  | **Sí** |  Microsoft.HDInsight/clusters  |  CategorizedGatewayRequests  |  Solicitudes de puerta de enlace categorizadas  |  Count  |  Total | 
| **Sí**  | **Sí** |  Microsoft.HDInsight/clusters  |  GatewayRequests  |  Solicitudes de puerta de enlace  |  Count  |  Total | 
| **Sí**  | **Sí** |  Microsoft.HDInsight/clusters  |  NumActiveWorkers  |  Número de trabajos activos  |  Count  |  Máxima | 
| **Sí**  | No |  Microsoft.HDInsight/clusters  |  ScalingRequests  |  Scaling requests (Solicitudes de escalado)  |  Count  |  Máxima | 
| **Sí**  | No |  Microsoft.Insights/AutoscaleSettings  |  MetricThreshold  |  Umbral de métrica  |  Count  |  Average | 
| **Sí**  | No |  Microsoft.Insights/AutoscaleSettings  |  ObservedCapacity  |  Capacidad observada  |  Count  |  Average | 
| **Sí**  | No |  Microsoft.Insights/AutoscaleSettings  |  ObservedMetricValue  |  Valor de métrica observado  |  Count  |  Average | 
| **Sí**  | **Sí** |  Microsoft.Insights/AutoscaleSettings  |  ScaleActionsInitiated  |  Acciones de escalado iniciadas  |  Count  |  Total | 
| **Sí**  | No |  Microsoft.Insights/Components  |  availabilityResults/availabilityPercentage  |  Disponibilidad  |  Percent  |  Average | 
| No  | No |  Microsoft.Insights/Components  |  availabilityResults/count  |  Pruebas de disponibilidad  |  Count  |  Count | 
| **Sí**  | No |  Microsoft.Insights/Components  |  availabilityResults/duration  |  Duración de la prueba de disponibilidad  |  MilliSeconds  |  Average | 
| **Sí**  | No |  Microsoft.Insights/Components  |  browserTimings/networkDuration  |  Tiempo de conexión de red de carga de página  |  MilliSeconds  |  Average | 
| **Sí**  | No |  Microsoft.Insights/Components  |  browserTimings/processingDuration  |  Tiempo de procesamiento del cliente  |  MilliSeconds  |  Average | 
| **Sí**  | No |  Microsoft.Insights/Components  |  browserTimings/receiveDuration  |  Tiempo de recepción de respuesta  |  MilliSeconds  |  Average | 
| **Sí**  | No |  Microsoft.Insights/Components  |  browserTimings/sendDuration  |  Tiempo de solicitud de envío  |  MilliSeconds  |  Average | 
| **Sí**  | No |  Microsoft.Insights/Components  |  browserTimings/totalDuration  |  Tiempo de carga de página del explorador  |  MilliSeconds  |  Average | 
| No  | No |  Microsoft.Insights/Components  |  dependencies/count  |  Llamadas de dependencia  |  Count  |  Count | 
| **Sí**  | No |  Microsoft.Insights/Components  |  dependencies/duration  |  Duración de la dependencia  |  MilliSeconds  |  Average | 
| No  | No |  Microsoft.Insights/Components  |  dependencies/failed  |  Errores de llamadas de dependencia  |  Count  |  Count | 
| No  | No |  Microsoft.Insights/Components  |  exceptions/browser  |  Excepciones de explorador  |  Count  |  Count | 
| **Sí**  | **Sí** |  Microsoft.Insights/Components  |  exceptions/count  |  Excepciones  |  Count  |  Count | 
| No  | No |  Microsoft.Insights/Components  |  exceptions/server  |  Excepciones de servidor  |  Count  |  Count | 
| **Sí**  | **Sí** |  Microsoft.Insights/Components  |  pageViews/count  |  Vistas de página  |  Count  |  Count | 
| **Sí**  | No |  Microsoft.Insights/Components  |  pageViews/duration  |  Tiempo de carga de la vista de página  |  MilliSeconds  |  Average | 
| **Sí**  | **Sí** |  Microsoft.Insights/Components  |  performanceCounters/exceptionsPerSecond  |  Velocidad de excepciones  |  CountPerSecond  |  Average | 
| **Sí**  | No |  Microsoft.Insights/Components  |  performanceCounters/memoryAvailableBytes  |  Memoria disponible  |  Bytes  |  Average | 
| **Sí**  | No |  Microsoft.Insights/Components  |  performanceCounters/processCpuPercentage  |  CPU de procesos  |  Percent  |  Average | 
| **Sí**  | No |  Microsoft.Insights/Components  |  performanceCounters/processIOBytesPerSecond  |  Velocidad de E/S del proceso  |  BytesPerSecond  |  Average | 
| **Sí**  | No |  Microsoft.Insights/Components  |  performanceCounters/processorCpuPercentage  |  Tiempo de procesador  |  Percent  |  Average | 
| **Sí**  | No |  Microsoft.Insights/Components  |  performanceCounters/processPrivateBytes  |  Bytes privados del proceso  |  Bytes  |  Average | 
| **Sí**  | No |  Microsoft.Insights/Components  |  performanceCounters/requestExecutionTime  |  Tiempo de ejecución de solicitud HTTP  |  MilliSeconds  |  Average | 
| **Sí**  | **Sí** |  Microsoft.Insights/Components  |  performanceCounters/requestsInQueue  |  Solicitudes HTTP en la cola de la aplicación  |  Count  |  Average | 
| **Sí**  | **Sí** |  Microsoft.Insights/Components  |  performanceCounters/requestsPerSecond  |  Tasa de solicitudes HTTP  |  CountPerSecond  |  Average | 
| No  | No |  Microsoft.Insights/Components  |  requests/count  |  Solicitudes de servidor  |  Count  |  Count | 
| **Sí**  | No |  Microsoft.Insights/Components  |  requests/duration  |  Tiempo de respuesta del servidor  |  MilliSeconds  |  Average | 
| No  | No |  Microsoft.Insights/Components  |  requests/failed  |  Error en las solicitudes  |  Count  |  Count | 
| No  | No |  Microsoft.Insights/Components  |  requests/rate  |  Tasa de solicitudes del servidor  |  CountPerSecond  |  Average | 
| **Sí**  | **Sí** |  Microsoft.Insights/Components  |  traces/count  |  Traces  |  Count  |  Count | 
| **Sí**  | No |  Microsoft.KeyVault/vaults  |  ServiceApiHit  |  Número total de visitas de la API de servicio  |  Count  |  Count | 
| **Sí**  | No |  Microsoft.KeyVault/vaults  |  ServiceApiLatency  |  Latencia general de la API de servicio  |  Milisegundos  |  Average | 
| **Sí**  | No |  Microsoft.KeyVault/vaults  |  ServiceApiResult  |  Número total de resultados de la API de servicio  |  Count  |  Count | 
| **Sí**  | **Sí** |  Microsoft.Kusto/Clusters  |  CacheUtilization  |  Uso de la caché  |  Percent  |  Average | 
| **Sí**  | **Sí** |  Microsoft.Kusto/Clusters  |  ContinuousExportMaxLatenessMinutes  |  Minutos de retraso máximos de exportación continua  |  Count  |  Máxima | 
| **Sí**  | **Sí** |  Microsoft.Kusto/Clusters  |  ContinuousExportNumOfRecordsExported  |  Continuous export - num of exported records (Exportación continua: número de registros exportados)  |  Count  |  Total | 
| **Sí**  | **Sí** |  Microsoft.Kusto/Clusters  |  ContinuousExportPendingCount  |  Recuento pendiente de exportación continua  |  Count  |  Máxima | 
| **Sí**  | **Sí** |  Microsoft.Kusto/Clusters  |  ContinuousExportResult  |  Continuous Export Result (Resultado de la exportación continua)  |  Count  |  Count | 
| **Sí**  | **Sí** |  Microsoft.Kusto/Clusters  |  CPU  |  CPU  |  Percent  |  Average | 
| **Sí**  | **Sí** |  Microsoft.Kusto/Clusters  |  EventsProcessedForEventHubs  |  Events processed (for Event/IoT Hubs) [Eventos procesados (para instancias de Event Hubs o IoT Hub)]  |  Count  |  Total | 
| **Sí**  | **Sí** |  Microsoft.Kusto/Clusters  |  ExportUtilization  |  Utilización de la exportación  |  Percent  |  Máxima | 
| **Sí**  | **Sí** |  Microsoft.Kusto/Clusters  |  IngestionLatencyInSeconds  |  Ingestion latency (in seconds) [Latencia de la ingesta (en segundos)]  |  Segundos  |  Average | 
| **Sí**  | **Sí** |  Microsoft.Kusto/Clusters  |  IngestionResult  |  Resultado de la ingesta  |  Count  |  Count | 
| **Sí**  | **Sí** |  Microsoft.Kusto/Clusters  |  IngestionUtilization  |  Uso de la ingesta  |  Percent  |  Average | 
| **Sí**  | **Sí** |  Microsoft.Kusto/Clusters  |  IngestionVolumeInMB  |  Volumen de ingesta (en MB)  |  Count  |  Total | 
| **Sí**  | **Sí** |  Microsoft.Kusto/Clusters  |  KeepAlive  |  Mantener conexión  |  Count  |  Average | 
| **Sí**  | **Sí** |  Microsoft.Kusto/Clusters  |  QueryDuration  |  Duración de la consulta  |  Milisegundos  |  Average | 
| **Sí**  | **Sí** |  Microsoft.Kusto/Clusters  |  SteamingIngestRequestRate  |  Streaming Ingest Request Rate [Velocidad de solicitudes de ingesta de streaming]  |  Count  |  RateRequestsPerSecond | 
| **Sí**  | **Sí** |  Microsoft.Kusto/Clusters  |  StreamingIngestDataRate  |  Streaming Ingest Data Rate (Velocidad de datos de la ingesta de streaming)  |  Count  |  Average | 
| **Sí**  | **Sí** |  Microsoft.Kusto/Clusters  |  StreamingIngestDuration  |  Streaming Ingest Duration (Duración de la ingesta de streaming)  |  Milisegundos  |  Average | 
| **Sí**  | **Sí** |  Microsoft.Kusto/Clusters  |  StreamingIngestResults  |  Streaming Ingest Result (Resultado de la ingesta de streaming)  |  Count  |  Average | 
| **Sí**  | No |  Microsoft.Logic/integrationServiceEnvironments  |  ActionLatency  |  Latencia de acciones   |  Segundos  |  Average | 
| **Sí**  | **Sí** |  Microsoft.Logic/integrationServiceEnvironments  |  ActionsCompleted  |  Acciones completadas   |  Count  |  Total | 
| **Sí**  | **Sí** |  Microsoft.Logic/integrationServiceEnvironments  |  ActionsFailed  |  Acciones con errores   |  Count  |  Total | 
| **Sí**  | **Sí** |  Microsoft.Logic/integrationServiceEnvironments  |  ActionsSkipped  |  Acciones omitidas   |  Count  |  Total | 
| **Sí**  | **Sí** |  Microsoft.Logic/integrationServiceEnvironments  |  ActionsStarted  |  Acciones iniciadas   |  Count  |  Total | 
| **Sí**  | **Sí** |  Microsoft.Logic/integrationServiceEnvironments  |  ActionsSucceeded  |  Acciones correctas   |  Count  |  Total | 
| **Sí**  | No |  Microsoft.Logic/integrationServiceEnvironments  |  ActionSuccessLatency  |  Latencia de acciones correctas   |  Segundos  |  Average | 
| **Sí**  | **Sí** |  Microsoft.Logic/integrationServiceEnvironments  |  ActionThrottledEvents  |  Eventos limitados de acciones  |  Count  |  Total | 
| **Sí**  | No |  Microsoft.Logic/integrationServiceEnvironments  |  IntegrationServiceEnvironmentConnectorMemoryUsage  |  Uso de la memoria del conector para el entorno del servicio de integración  |  Percent  |  Average | 
| **Sí**  | No |  Microsoft.Logic/integrationServiceEnvironments  |  IntegrationServiceEnvironmentConnectorProcessorUsage  |  Uso del procesador del conector para el entorno del servicio de integración  |  Percent  |  Average | 
| **Sí**  | No |  Microsoft.Logic/integrationServiceEnvironments  |  IntegrationServiceEnvironmentWorkflowMemoryUsage  |  Uso del memoria de flujo de trabajo para el entorno del servicio de integración  |  Percent  |  Average | 
| **Sí**  | No |  Microsoft.Logic/integrationServiceEnvironments  |  IntegrationServiceEnvironmentWorkflowProcessorUsage  |  Uso del procesador de flujo de trabajo para el entorno del servicio de integración  |  Percent  |  Average | 
| **Sí**  | **Sí** |  Microsoft.Logic/integrationServiceEnvironments  |  RunFailurePercentage  |  Porcentaje de errores de ejecución  |  Percent  |  Total | 
| **Sí**  | No |  Microsoft.Logic/integrationServiceEnvironments  |  RunLatency  |  Latencia de ejecución  |  Segundos  |  Average | 
| **Sí**  | **Sí** |  Microsoft.Logic/integrationServiceEnvironments  |  RunsCancelled  |  Ejecuciones canceladas  |  Count  |  Total | 
| **Sí**  | **Sí** |  Microsoft.Logic/integrationServiceEnvironments  |  RunsCompleted  |  Ejecuciones completadas  |  Count  |  Total | 
| **Sí**  | **Sí** |  Microsoft.Logic/integrationServiceEnvironments  |  RunsFailed  |  Ejecuciones con errores  |  Count  |  Total | 
| **Sí**  | **Sí** |  Microsoft.Logic/integrationServiceEnvironments  |  RunsStarted  |  Ejecuciones iniciadas  |  Count  |  Total | 
| **Sí**  | **Sí** |  Microsoft.Logic/integrationServiceEnvironments  |  RunsSucceeded  |  Ejecuciones correctas  |  Count  |  Total | 
| **Sí**  | **Sí** |  Microsoft.Logic/integrationServiceEnvironments  |  RunStartThrottledEvents  |  Ejecución de eventos limitados de inicio  |  Count  |  Total | 
| **Sí**  | No |  Microsoft.Logic/integrationServiceEnvironments  |  RunSuccessLatency  |  Latencia de ejecuciones correctas  |  Segundos  |  Average | 
| **Sí**  | **Sí** |  Microsoft.Logic/integrationServiceEnvironments  |  RunThrottledEvents  |  Ejecución de eventos limitados  |  Count  |  Total | 
| **Sí**  | No |  Microsoft.Logic/integrationServiceEnvironments  |  TriggerFireLatency  |  Latencia de desencadenador activado   |  Segundos  |  Average | 
| **Sí**  | No |  Microsoft.Logic/integrationServiceEnvironments  |  TriggerLatency  |  Latencia de desencadenador   |  Segundos  |  Average | 
| **Sí**  | **Sí** |  Microsoft.Logic/integrationServiceEnvironments  |  TriggersCompleted  |  Desencadenadores completados   |  Count  |  Total | 
| **Sí**  | **Sí** |  Microsoft.Logic/integrationServiceEnvironments  |  TriggersFailed  |  Desencadenadores con errores   |  Count  |  Total | 
| **Sí**  | **Sí** |  Microsoft.Logic/integrationServiceEnvironments  |  TriggersFired  |  Desencadenadores activados   |  Count  |  Total | 
| **Sí**  | **Sí** |  Microsoft.Logic/integrationServiceEnvironments  |  TriggersSkipped  |  Desencadenadores omitidos  |  Count  |  Total | 
| **Sí**  | **Sí** |  Microsoft.Logic/integrationServiceEnvironments  |  TriggersStarted  |  Desencadenadores iniciados   |  Count  |  Total | 
| **Sí**  | **Sí** |  Microsoft.Logic/integrationServiceEnvironments  |  TriggersSucceeded  |  Desencadenadores correctos   |  Count  |  Total | 
| **Sí**  | No |  Microsoft.Logic/integrationServiceEnvironments  |  TriggerSuccessLatency  |  Latencia de desencadenadores correctos   |  Segundos  |  Average | 
| **Sí**  | **Sí** |  Microsoft.Logic/integrationServiceEnvironments  |  TriggerThrottledEvents  |  Eventos limitados de desencadenadores  |  Count  |  Total | 
| **Sí**  | No |  Microsoft.Logic/workflows  |  ActionLatency  |  Latencia de acciones   |  Segundos  |  Average | 
| **Sí**  | **Sí** |  Microsoft.Logic/workflows  |  ActionsCompleted  |  Acciones completadas   |  Count  |  Total | 
| **Sí**  | **Sí** |  Microsoft.Logic/workflows  |  ActionsFailed  |  Acciones con errores   |  Count  |  Total | 
| **Sí**  | **Sí** |  Microsoft.Logic/workflows  |  ActionsSkipped  |  Acciones omitidas   |  Count  |  Total | 
| **Sí**  | **Sí** |  Microsoft.Logic/workflows  |  ActionsStarted  |  Acciones iniciadas   |  Count  |  Total | 
| **Sí**  | **Sí** |  Microsoft.Logic/workflows  |  ActionsSucceeded  |  Acciones correctas   |  Count  |  Total | 
| **Sí**  | No |  Microsoft.Logic/workflows  |  ActionSuccessLatency  |  Latencia de acciones correctas   |  Segundos  |  Average | 
| **Sí**  | **Sí** |  Microsoft.Logic/workflows  |  ActionThrottledEvents  |  Eventos limitados de acciones  |  Count  |  Total | 
| **Sí**  | **Sí** |  Microsoft.Logic/workflows  |  BillableActionExecutions  |  Ejecuciones de acciones facturables  |  Count  |  Total | 
| **Sí**  | **Sí** |  Microsoft.Logic/workflows  |  BillableTriggerExecutions  |  Ejecuciones del desencadenador facturable  |  Count  |  Total | 
| **Sí**  | **Sí** |  Microsoft.Logic/workflows  |  BillingUsageNativeOperation  |  Uso de facturación para las ejecuciones de la operación nativa  |  Count  |  Total | 
| **Sí**  | **Sí** |  Microsoft.Logic/workflows  |  BillingUsageNativeOperation  |  Uso de facturación para las ejecuciones de la operación nativa  |  Count  |  Total | 
| **Sí**  | **Sí** |  Microsoft.Logic/workflows  |  BillingUsageStandardConnector  |  Uso de facturación para las ejecuciones del conector estándar  |  Count  |  Total | 
| **Sí**  | **Sí** |  Microsoft.Logic/workflows  |  BillingUsageStandardConnector  |  Uso de facturación para las ejecuciones del conector estándar  |  Count  |  Total | 
| **Sí**  | **Sí** |  Microsoft.Logic/workflows  |  BillingUsageStorageConsumption  |  Uso de facturación para las ejecuciones de consumo de almacenamiento  |  Count  |  Total | 
| **Sí**  | **Sí** |  Microsoft.Logic/workflows  |  BillingUsageStorageConsumption  |  Uso de facturación para las ejecuciones de consumo de almacenamiento  |  Count  |  Total | 
| **Sí**  | **Sí** |  Microsoft.Logic/workflows  |  RunFailurePercentage  |  Porcentaje de errores de ejecución  |  Percent  |  Total | 
| **Sí**  | No |  Microsoft.Logic/workflows  |  RunLatency  |  Latencia de ejecución  |  Segundos  |  Average | 
| **Sí**  | **Sí** |  Microsoft.Logic/workflows  |  RunsCancelled  |  Ejecuciones canceladas  |  Count  |  Total | 
| **Sí**  | **Sí** |  Microsoft.Logic/workflows  |  RunsCompleted  |  Ejecuciones completadas  |  Count  |  Total | 
| **Sí**  | **Sí** |  Microsoft.Logic/workflows  |  RunsFailed  |  Ejecuciones con errores  |  Count  |  Total | 
| **Sí**  | **Sí** |  Microsoft.Logic/workflows  |  RunsStarted  |  Ejecuciones iniciadas  |  Count  |  Total | 
| **Sí**  | **Sí** |  Microsoft.Logic/workflows  |  RunsSucceeded  |  Ejecuciones correctas  |  Count  |  Total | 
| **Sí**  | **Sí** |  Microsoft.Logic/workflows  |  RunStartThrottledEvents  |  Ejecución de eventos limitados de inicio  |  Count  |  Total | 
| **Sí**  | No |  Microsoft.Logic/workflows  |  RunSuccessLatency  |  Latencia de ejecuciones correctas  |  Segundos  |  Average | 
| **Sí**  | **Sí** |  Microsoft.Logic/workflows  |  RunThrottledEvents  |  Ejecución de eventos limitados  |  Count  |  Total | 
| **Sí**  | **Sí** |  Microsoft.Logic/workflows  |  TotalBillableExecutions  |  Total de ejecuciones facturables  |  Count  |  Total | 
| **Sí**  | No |  Microsoft.Logic/workflows  |  TriggerFireLatency  |  Latencia de desencadenador activado   |  Segundos  |  Average | 
| **Sí**  | No |  Microsoft.Logic/workflows  |  TriggerLatency  |  Latencia de desencadenador   |  Segundos  |  Average | 
| **Sí**  | **Sí** |  Microsoft.Logic/workflows  |  TriggersCompleted  |  Desencadenadores completados   |  Count  |  Total | 
| **Sí**  | **Sí** |  Microsoft.Logic/workflows  |  TriggersFailed  |  Desencadenadores con errores   |  Count  |  Total | 
| **Sí**  | **Sí** |  Microsoft.Logic/workflows  |  TriggersFired  |  Desencadenadores activados   |  Count  |  Total | 
| **Sí**  | **Sí** |  Microsoft.Logic/workflows  |  TriggersSkipped  |  Desencadenadores omitidos  |  Count  |  Total | 
| **Sí**  | **Sí** |  Microsoft.Logic/workflows  |  TriggersStarted  |  Desencadenadores iniciados   |  Count  |  Total | 
| **Sí**  | **Sí** |  Microsoft.Logic/workflows  |  TriggersSucceeded  |  Desencadenadores correctos   |  Count  |  Total | 
| **Sí**  | No |  Microsoft.Logic/workflows  |  TriggerSuccessLatency  |  Latencia de desencadenadores correctos   |  Segundos  |  Average | 
| **Sí**  | **Sí** |  Microsoft.Logic/workflows  |  TriggerThrottledEvents  |  Eventos limitados de desencadenadores  |  Count  |  Total | 
| **Sí**  | No |  Microsoft.MachineLearningServices/workspaces  |  Active Cores  |  Active Cores (Núcleos activos)  |  Count  |  Average | 
| **Sí**  | No |  Microsoft.MachineLearningServices/workspaces  |  Active Nodes  |  Active Nodes (Nodos activos)  |  Count  |  Average | 
| **Sí**  | No |  Microsoft.MachineLearningServices/workspaces  |  Ejecuciones finalizadas  |  Ejecuciones finalizadas  |  Count  |  Total | 
| **Sí**  | No |  Microsoft.MachineLearningServices/workspaces  |  Ejecuciones con error  |  Ejecuciones con error  |  Count  |  Total | 
| **Sí**  | No |  Microsoft.MachineLearningServices/workspaces  |  Idle Cores  |  Idle Cores (Núcleos inactivos)  |  Count  |  Average | 
| **Sí**  | No |  Microsoft.MachineLearningServices/workspaces  |  Idle Nodes  |  Idle Nodes (Nodos inactivos)  |  Count  |  Average | 
| **Sí**  | No |  Microsoft.MachineLearningServices/workspaces  |  Leaving Cores  |  Leaving Cores (Núcleos de salida)  |  Count  |  Average | 
| **Sí**  | No |  Microsoft.MachineLearningServices/workspaces  |  Leaving Nodes  |  Leaving Nodes (Nodos de salida)  |  Count  |  Average | 
| **Sí**  | No |  Microsoft.MachineLearningServices/workspaces  |  Error al realizar la implementación de modelo  |  Error al realizar la implementación de modelo  |  Count  |  Total | 
| **Sí**  | No |  Microsoft.MachineLearningServices/workspaces  |  Model Deploy Started  |  Model Deploy Started (Implementación de modelo iniciada)  |  Count  |  Total | 
| **Sí**  | No |  Microsoft.MachineLearningServices/workspaces  |  Model Deploy Succeeded  |  Model Deploy Succeeded (Implementación de modelo correcta)  |  Count  |  Total | 
| **Sí**  | No |  Microsoft.MachineLearningServices/workspaces  |  Model Register Failed  |  Model Register Failed (Error en el registro de modelo)  |  Count  |  Total | 
| **Sí**  | No |  Microsoft.MachineLearningServices/workspaces  |  Model Register Succeeded  |  Model Register Succeeded (Registro de modelo realizado correctamente)  |  Count  |  Total | 
| **Sí**  | No |  Microsoft.MachineLearningServices/workspaces  |  Preempted Cores  |  Preempted Cores (Núcleos con prioridad)  |  Count  |  Average | 
| **Sí**  | No |  Microsoft.MachineLearningServices/workspaces  |  Preempted Nodes  |  Preempted Nodes (Nodos con prioridad)  |  Count  |  Average | 
| **Sí**  | No |  Microsoft.MachineLearningServices/workspaces  |  Porcentaje de uso de la cuota  |  Porcentaje de uso de la cuota  |  Count  |  Average | 
| **Sí**  | No |  Microsoft.MachineLearningServices/workspaces  |  Ejecuciones iniciadas  |  Ejecuciones iniciadas  |  Count  |  Total | 
| **Sí**  | No |  Microsoft.MachineLearningServices/workspaces  |  Total Cores  |  Total Cores (Núcleos totales)  |  Count  |  Average | 
| **Sí**  | No |  Microsoft.MachineLearningServices/workspaces  |  Total Nodes  |  Total Nodes (Nodos totales)  |  Count  |  Average | 
| **Sí**  | No |  Microsoft.MachineLearningServices/workspaces  |  Unusable Cores  |  Unusable Cores (Núcleos no utilizables)  |  Count  |  Average | 
| **Sí**  | No |  Microsoft.MachineLearningServices/workspaces  |  Nodos no utilizables  |  Nodos no utilizables  |  Count  |  Average | 
| **Sí**  | No |  Microsoft.Maps/accounts  |  Disponibilidad  |  Disponibilidad  |  Percent  |  Average | 
| No  | No |  Microsoft.Maps/accounts  |  Uso  |  Uso  |  Count  |  Count | 
| **Sí**  | No |  Microsoft.Media/mediaservices  |  AssetCount  |  Número de recursos  |  Count  |  Average | 
| **Sí**  | No |  Microsoft.Media/mediaservices  |  AssetQuota  |  Cuota de recursos  |  Count  |  Average | 
| **Sí**  | No |  Microsoft.Media/mediaservices  |  AssetQuotaUsedPercentage  |  Porcentaje de cuota de recursos usado  |  Percent  |  Average | 
| **Sí**  | No |  Microsoft.Media/mediaservices  |  ContentKeyPolicyCount  |  Número de directivas de clave de contenido  |  Count  |  Average | 
| **Sí**  | No |  Microsoft.Media/mediaservices  |  ContentKeyPolicyCount  |  Cuota de directiva de clave de contenido  |  Count  |  Average | 
| **Sí**  | No |  Microsoft.Media/mediaservices  |  ContentKeyPolicyQuotaUsedPercentage  |  Porcentaje utilizado de la cuota de directivas de clave de contenido  |  Percent  |  Average | 
| **Sí**  | No |  Microsoft.Media/mediaservices  |  StreamingPolicyCount  |  Número de directivas de streaming  |  Count  |  Average | 
| **Sí**  | No |  Microsoft.Media/mediaservices  |  StreamingPolicyQuota  |  Cuota de directivas de streaming  |  Count  |  Average | 
| **Sí**  | No |  Microsoft.Media/mediaservices  |  StreamingPolicyQuotaUsedPercentage  |  Porcentaje usado de la cuota de directiva de streaming  |  Percent  |  Average | 
| **Sí**  | No |  Microsoft.Media/mediaservices/streamingEndpoints  |  Salida  |  Salida  |  Bytes  |  Total | 
| **Sí**  | No |  Microsoft.Media/mediaservices/streamingEndpoints  |  Requests  |  Requests  |  Count  |  Total | 
| **Sí**  | No |  Microsoft.Media/mediaservices/streamingEndpoints  |  SuccessE2ELatency  |  Latencia correcta de extremo a extremo  |  Milisegundos  |  Average | 
| **Sí**  | No |  Microsoft.Microservices4Spring/appClusters  |  GCPauseTotalCount  |  GC Pause Count (Recuento de pausas de la GC)  |  Count  |  Total | 
| **Sí**  | No |  Microsoft.Microservices4Spring/appClusters  |  GCPauseTotalTime  |  GC Pause Total Time (Tiempo total de pausas de la GC)  |  Milisegundos  |  Total | 
| **Sí**  | No |  Microsoft.Microservices4Spring/appClusters  |  MaxOldGenMemoryPoolBytes  |  Max Available Old Generation Data Size (Tamaño máximo de datos de generación anterior disponible)  |  Bytes  |  Average | 
| **Sí**  | No |  Microsoft.Microservices4Spring/appClusters  |  OldGenMemoryPoolBytes  |  Old Generation Data Size (Tamaño de datos de generación anterior)  |  Bytes  |  Average | 
| **Sí**  | No |  Microsoft.Microservices4Spring/appClusters  |  OldGenPromotedBytes  |  Promote to Old Generation Data Size (Promoción al tamaño de datos de generación anterior)  |  Bytes  |  Máxima | 
| **Sí**  | No |  Microsoft.Microservices4Spring/appClusters  |  ServiceCpuUsagePercentage  |  Service CPU Usage Percentage (Porcentaje de uso de CPU de servicio)  |  Percent  |  Average | 
| **Sí**  | No |  Microsoft.Microservices4Spring/appClusters  |  ServiceMemoryCommitted  |  Service Memory Assigned (Memoria de servicio asignada)  |  Bytes  |  Average | 
| **Sí**  | No |  Microsoft.Microservices4Spring/appClusters  |  ServiceMemoryMax  |  Service Memory Max (Memoria de servicio máxima)  |  Bytes  |  Máxima | 
| **Sí**  | No |  Microsoft.Microservices4Spring/appClusters  |  ServiceMemoryUsed  |  Service Memory Used (Memoria de servicio usada)  |  Bytes  |  Average | 
| **Sí**  | No |  Microsoft.Microservices4Spring/appClusters  |  SystemCpuUsagePercentage  |  System CPU Usage Percentage (Porcentaje de uso de CPU del sistema)  |  Percent  |  Average | 
| **Sí**  | No |  Microsoft.Microservices4Spring/appClusters  |  TomcatErrorCount  |  Tomcat Global Error (Error global de Tomcat)  |  Count  |  Total | 
| **Sí**  | No |  Microsoft.Microservices4Spring/appClusters  |  TomcatReceivedBytes  |  Tomcat Total Received Bytes (Total de bytes recibidos de Tomcat)  |  Bytes  |  Total | 
| **Sí**  | No |  Microsoft.Microservices4Spring/appClusters  |  TomcatRequestMaxTime  |  Tomcat Request Max Time (Tiempo máximo de las solicitudes de Tomcat)  |  Milisegundos  |  Máxima | 
| **Sí**  | No |  Microsoft.Microservices4Spring/appClusters  |  TomcatRequestTotalCount  |  Tomcat Request Total Count (Recuento total de las solicitudes de Tomcat)  |  Count  |  Total | 
| **Sí**  | No |  Microsoft.Microservices4Spring/appClusters  |  TomcatRequestTotalTime  |  Tomcat Request Total Times (Tiempo total de la solicitud de Tomcat)  |  Milisegundos  |  Total | 
| **Sí**  | No |  Microsoft.Microservices4Spring/appClusters  |  TomcatResponseAvgTime  |  Tomcat Request Average Time (Tiempo medio de las solicitudes de Tomcat)  |  Milisegundos  |  Average | 
| **Sí**  | No |  Microsoft.Microservices4Spring/appClusters  |  TomcatSentBytes  |  Tomcat Total Sent Bytes (Total de bytes enviados de Tomcat)  |  Bytes  |  Total | 
| **Sí**  | No |  Microsoft.Microservices4Spring/appClusters  |  TomcatSessionActiveCurrentCount  |  Tomcat Session Alive Count (Recuento activo de las sesiones de Tomcat)  |  Count  |  Total | 
| **Sí**  | No |  Microsoft.Microservices4Spring/appClusters  |  TomcatSessionActiveMaxCount  |  Tomcat Session Max Active Count (Recuento activo máximo de las sesiones de Tomcat)  |  Count  |  Total | 
| **Sí**  | No |  Microsoft.Microservices4Spring/appClusters  |  TomcatSessionAliveMaxTime  |  Tomcat Session Max Alive Time (Tempo activo máximo de las sesiones de Tomcat)  |  Milisegundos  |  Máxima | 
| **Sí**  | No |  Microsoft.Microservices4Spring/appClusters  |  TomcatSessionCreatedCount  |  Tomcat Session Created Count (Recuento de sesiones creadas de Tomcat)  |  Count  |  Total | 
| **Sí**  | No |  Microsoft.Microservices4Spring/appClusters  |  TomcatSessionExpiredCount  |  Tomcat Session Expired Count (Recuento de sesiones expiradas de Tomcat)  |  Count  |  Total | 
| **Sí**  | No |  Microsoft.Microservices4Spring/appClusters  |  TomcatSessionRejectedCount  |  Tomcat Session Rejected Count (Recuento de sesiones rechazadas de Tomcat)  |  Count  |  Total | 
| **Sí**  | No |  Microsoft.Microservices4Spring/appClusters  |  YoungGenPromotedBytes  |  Promote to Young Generation Data Size (Promoción al tamaño de datos de nueva generación)  |  Bytes  |  Máxima | 
| **Sí**  | No |  Microsoft.NetApp/netAppAccounts/capacityPools  |  VolumePoolAllocatedUsed  |  Uso asignado del grupo de volumen  |  Bytes  |  Average | 
| **Sí**  | No |  Microsoft.NetApp/netAppAccounts/capacityPools  |  VolumePoolTotalLogicalSize  |  Tamaño lógico total del grupo de volumen  |  Bytes  |  Average | 
| **Sí**  | No |  Microsoft.NetApp/netAppAccounts/capacityPools/volumes  |  AverageReadLatency  |  Latencia de lectura media  |  MilliSeconds  |  Average | 
| **Sí**  | No |  Microsoft.NetApp/netAppAccounts/capacityPools/volumes  |  AverageWriteLatency  |  Latencia de escritura media  |  MilliSeconds  |  Average | 
| **Sí**  | No |  Microsoft.NetApp/netAppAccounts/capacityPools/volumes  |  ReadIops  |  IOPS de lectura  |  CountPerSecond  |  Average | 
| **Sí**  | No |  Microsoft.NetApp/netAppAccounts/capacityPools/volumes  |  VolumeLogicalSize  |  Tamaño lógico del volumen  |  Bytes  |  Average | 
| **Sí**  | No |  Microsoft.NetApp/netAppAccounts/capacityPools/volumes  |  VolumeSnapshotSize  |  Tamaño de instantánea de volumen  |  Bytes  |  Average | 
| **Sí**  | No |  Microsoft.NetApp/netAppAccounts/capacityPools/volumes  |  WriteIops  |  IOPS de escritura  |  CountPerSecond  |  Average | 
| No  | No |  Microsoft.Network/applicationGateways  |  ApplicationGatewayTotalTime  |  Application Gateway Total Time (Tiempo total de Application Gateway)  |  MilliSeconds  |  Average | 
| No  | No |  Microsoft.Network/applicationGateways  |  AvgRequestCountPerHealthyHost  |  Requests per minute per Healthy Host (Solicitudes por minuto y host con estado correcto)  |  Count  |  Average | 
| No  | No |  Microsoft.Network/applicationGateways  |  BackendConnectTime  |  Backend Connect Time (Tiempo de conexión de back-end)  |  MilliSeconds  |  Average | 
| No  | No |  Microsoft.Network/applicationGateways  |  BackendFirstByteResponseTime  |  Backend First Byte Response Time (Tiempo de respuesta del primer byte de back-end)  |  MilliSeconds  |  Average | 
| No  | No |  Microsoft.Network/applicationGateways  |  BackendLastByteResponseTime  |  Backend Last Byte Response Time (Tiempo de respuesta del último byte de back-end)  |  MilliSeconds  |  Average | 
| **Sí**  | No |  Microsoft.Network/applicationGateways  |  BackendResponseStatus  |  Backend Response Status (Estado de respuesta de back-end)  |  Count  |  Total | 
| **Sí**  | No |  Microsoft.Network/applicationGateways  |  BlockedCount  |  Web Application Firewall Blocked Requests Rule Distribution (Distribución de reglas de solicitudes bloqueadas del firewall de aplicaciones web)  |  Count  |  Total | 
| **Sí**  | No |  Microsoft.Network/applicationGateways  |  BlockedReqCount  |  Web Application Firewall Blocked Requests Count (Recuento de solicitudes bloqueadas del firewall de aplicaciones web)  |  Count  |  Total | 
| **Sí**  | No |  Microsoft.Network/applicationGateways  |  BytesReceived  |  Bytes recibidos  |  Bytes  |  Total | 
| **Sí**  | No |  Microsoft.Network/applicationGateways  |  BytesSent  |  Bytes enviados  |  Bytes  |  Total | 
| No  | No |  Microsoft.Network/applicationGateways  |  CapacityUnits  |  Unidades de capacidad actuales  |  Count  |  Average | 
| No  | No |  Microsoft.Network/applicationGateways  |  ClientRtt  |  Client RTT (RTT del cliente)  |  MilliSeconds  |  Average | 
| No  | No |  Microsoft.Network/applicationGateways  |  ComputeUnits  |  Current Compute Units (Unidades de proceso actuales)  |  Count  |  Average | 
| **Sí**  | No |  Microsoft.Network/applicationGateways  |  CurrentConnections  |  Conexiones actuales  |  Count  |  Total | 
| **Sí**  | No |  Microsoft.Network/applicationGateways  |  FailedRequests  |  Solicitudes con error  |  Count  |  Total | 
| **Sí**  | No |  Microsoft.Network/applicationGateways  |  HealthyHostCount  |  Recuento de hosts con estado correcto  |  Count  |  Average | 
| **Sí**  | No |  Microsoft.Network/applicationGateways  |  MatchedCount  |  Web Application Firewall Total Rule Distribution (Distribución de reglas totales del firewall de aplicaciones web)  |  Count  |  Total | 
| **Sí**  | No |  Microsoft.Network/applicationGateways  |  ResponseStatus  |  Estado de respuesta  |  Count  |  Total | 
| No  | No |  Microsoft.Network/applicationGateways  |  Throughput  |  Throughput  |  BytesPerSecond  |  Average | 
| **Sí**  | No |  Microsoft.Network/applicationGateways  |  TlsProtocol  |  Client TLS Protocol (Protocolo TLS de cliente)  |  Count  |  Total | 
| **Sí**  | No |  Microsoft.Network/applicationGateways  |  TotalRequests  |  Total de solicitudes  |  Count  |  Total | 
| **Sí**  | No |  Microsoft.Network/applicationGateways  |  UnhealthyHostCount  |  Recuento de hosts con estado incorrecto  |  Count  |  Average | 
| **Sí**  | No |  Microsoft.Network/azurefirewalls  |  ApplicationRuleHit  |  Recuento de llamadas de las reglas de aplicación  |  Count  |  Total | 
| **Sí**  | No |  Microsoft.Network/azurefirewalls  |  DataProcessed  |  Datos procesados  |  Bytes  |  Total | 
| **Sí**  | No |  Microsoft.Network/azurefirewalls  |  FirewallHealth  |  Firewall health state (Estado de mantenimiento del firewall)  |  Percent  |  Average | 
| **Sí**  | No |  Microsoft.Network/azurefirewalls  |  NetworkRuleHit  |  Número de llamadas de reglas de red  |  Count  |  Total | 
| **Sí**  | No |  Microsoft.Network/azurefirewalls  |  SNATPortUtilization  |  SNAT port utilization (Uso de puertos SNAT)  |  Percent  |  Average | 
| **Sí**  | No |  Microsoft.Network/connections  |  BitsInPerSecond  |  BitsInPerSecond  |  CountPerSecond  |  Average | 
| **Sí**  | No |  Microsoft.Network/connections  |  BitsOutPerSecond  |  BitsOutPerSecond  |  CountPerSecond  |  Average | 
| **Sí**  | No |  Microsoft.Network/dnszones  |  QueryVolume  |  Volumen de consulta  |  Count  |  Total | 
| No  | No |  Microsoft.Network/dnszones  |  RecordSetCapacityUtilization  |  Uso de capacidad de conjunto de registros  |  Percent  |  Máxima | 
| **Sí**  | No |  Microsoft.Network/dnszones  |  RecordSetCount  |  Recuento de conjunto de registros  |  Count  |  Máxima | 
| **Sí**  | No |  Microsoft.Network/expressRouteCircuits  |  ArpAvailability  |  Disponibilidad de ARP  |  Percent  |  Average | 
| **Sí**  | No |  Microsoft.Network/expressRouteCircuits  |  BgpAvailability  |  Disponibilidad de BGP  |  Percent  |  Average | 
| No  | No |  Microsoft.Network/expressRouteCircuits  |  BitsInPerSecond  |  BitsInPerSecond  |  CountPerSecond  |  Average | 
| No  | No |  Microsoft.Network/expressRouteCircuits  |  BitsOutPerSecond  |  BitsOutPerSecond  |  CountPerSecond  |  Average | 
| No  | No |  Microsoft.Network/expressRouteCircuits  |  GlobalReachBitsInPerSecond  |  GlobalReachBitsInPerSecond  |  CountPerSecond  |  Average | 
| No  | No |  Microsoft.Network/expressRouteCircuits  |  GlobalReachBitsOutPerSecond  |  GlobalReachBitsOutPerSecond  |  CountPerSecond  |  Average | 
| No  | No |  Microsoft.Network/expressRouteCircuits  |  QosDropBitsInPerSecond  |  DroppedInBitsPerSecond  |  CountPerSecond  |  Average | 
| No  | No |  Microsoft.Network/expressRouteCircuits  |  QosDropBitsOutPerSecond  |  DroppedOutBitsPerSecond  |  CountPerSecond  |  Average | 
| **Sí**  | No |  Microsoft.Network/expressRouteCircuits/peerings  |  BitsInPerSecond  |  BitsInPerSecond  |  CountPerSecond  |  Average | 
| **Sí**  | No |  Microsoft.Network/expressRouteCircuits/peerings  |  BitsOutPerSecond  |  BitsOutPerSecond  |  CountPerSecond  |  Average | 
| No  | No |  Microsoft.Network/expressRouteGateways  |  ErGatewayConnectionBitsInPerSecond  |  BitsInPerSecond  |  CountPerSecond  |  Average | 
| No  | No |  Microsoft.Network/expressRouteGateways  |  ErGatewayConnectionBitsOutPerSecond  |  BitsOutPerSecond  |  CountPerSecond  |  Average | 
| **Sí**  | No |  Microsoft.Network/expressRoutePorts  |  AdminState  |  AdminState  |  Count  |  Average | 
| **Sí**  | No |  Microsoft.Network/expressRoutePorts  |  LineProtocol  |  LineProtocol  |  Count  |  Average | 
| **Sí**  | No |  Microsoft.Network/expressRoutePorts  |  PortBitsInPerSecond  |  BitsInPerSecond  |  CountPerSecond  |  Average | 
| **Sí**  | No |  Microsoft.Network/expressRoutePorts  |  PortBitsOutPerSecond  |  BitsOutPerSecond  |  CountPerSecond  |  Average | 
| **Sí**  | No |  Microsoft.Network/expressRoutePorts  |  RxLightLevel  |  RxLightLevel  |  Count  |  Average | 
| **Sí**  | No |  Microsoft.Network/expressRoutePorts  |  TxLightLevel  |  TxLightLevel  |  Count  |  Average | 
| **Sí**  | **Sí** |  Microsoft.Network/frontdoors  |  BackendHealthPercentage  |  Porcentaje de estado del back-end  |  Percent  |  Average | 
| **Sí**  | **Sí** |  Microsoft.Network/frontdoors  |  BackendRequestCount  |  Recuento de solicitudes de back-end  |  Count  |  Total | 
| **Sí**  | **Sí** |  Microsoft.Network/frontdoors  |  BackendRequestLatency  |  Latencia de las solicitudes de back-end  |  MilliSeconds  |  Average | 
| **Sí**  | **Sí** |  Microsoft.Network/frontdoors  |  BillableResponseSize  |  Billable Response Size (Tamaño de respuesta facturable)  |  Bytes  |  Total | 
| **Sí**  | **Sí** |  Microsoft.Network/frontdoors  |  RequestCount  |  Recuento de solicitudes  |  Count  |  Total | 
| **Sí**  | **Sí** |  Microsoft.Network/frontdoors  |  RequestSize  |  Tamaño de la solicitud  |  Bytes  |  Total | 
| **Sí**  | **Sí** |  Microsoft.Network/frontdoors  |  ResponseSize  |  Tamaño de la respuesta  |  Bytes  |  Total | 
| **Sí**  | **Sí** |  Microsoft.Network/frontdoors  |  TotalLatency  |  Latencia total  |  MilliSeconds  |  Average | 
| **Sí**  | **Sí** |  Microsoft.Network/frontdoors  |  WebApplicationFirewallRequestCount  |  Recuento de solicitudes del firewall de aplicaciones web  |  Count  |  Total | 
| No  | No |  Microsoft.Network/loadBalancers  |  AllocatedSnatPorts  |  Puertos SNAT asignados (versión preliminar)  |  Count  |  Total | 
| **Sí**  | No |  Microsoft.Network/loadBalancers  |  ByteCount  |  Recuento de bytes  |  Count  |  Total | 
| **Sí**  | No |  Microsoft.Network/loadBalancers  |  DipAvailability  |  Estado del sondeo de mantenimiento  |  Count  |  Average | 
| **Sí**  | No |  Microsoft.Network/loadBalancers  |  PacketCount  |  Recuento de paquetes  |  Count  |  Total | 
| **Sí**  | No |  Microsoft.Network/loadBalancers  |  SnatConnectionCount  |  Recuento de conexiones SNAT  |  Count  |  Total | 
| **Sí**  | No |  Microsoft.Network/loadBalancers  |  SYNCount  |  Recuento de SYN  |  Count  |  Total | 
| No  | No |  Microsoft.Network/loadBalancers  |  UsedSnatPorts  |  Puertos SNAT usados (versión preliminar)  |  Count  |  Total | 
| **Sí**  | No |  Microsoft.Network/loadBalancers  |  VipAvailability  |  Disponibilidad de la ruta de acceso de datos  |  Count  |  Average | 
| **Sí**  | No |  Microsoft.Network/networkInterfaces  |  BytesReceivedRate  |  Bytes recibidos  |  Bytes  |  Total | 
| **Sí**  | No |  Microsoft.Network/networkInterfaces  |  BytesSentRate  |  Bytes enviados  |  Bytes  |  Total | 
| **Sí**  | No |  Microsoft.Network/networkInterfaces  |  PacketsReceivedRate  |  Paquetes recibidos  |  Count  |  Total | 
| **Sí**  | No |  Microsoft.Network/networkInterfaces  |  PacketsSentRate  |  Paquetes enviados  |  Count  |  Total | 
| **Sí**  | No |  Microsoft.Network/networkWatchers/connectionMonitors  |  AverageRoundtripMs  |  Prom. Tiempo de ida y vuelta (ms)  |  MilliSeconds  |  Average | 
| **Sí**  | No |  Microsoft.Network/networkWatchers/connectionMonitors  |  ChecksFailedPercent  |  Checks Failed Percent (Preview) [Porcentaje de comprobaciones erróneas (versión preliminar)]  |  Percent  |  Average | 
| **Sí**  | No |  Microsoft.Network/networkWatchers/connectionMonitors  |  ProbesFailedPercent  |  % de sondeos con error  |  Percent  |  Average | 
| **Sí**  | No |  Microsoft.Network/networkWatchers/connectionMonitors  |  RoundTripTimeMs  |  Round-Trip Time (ms) (Preview) [Tiempo de ida y vuelta (ms) (versión preliminar)]  |  MilliSeconds  |  Average | 
| **Sí**  | No |  Microsoft.Network/publicIPAddresses  |  ByteCount  |  Recuento de bytes  |  Count  |  Total | 
| **Sí**  | No |  Microsoft.Network/publicIPAddresses  |  BytesDroppedDDoS  |  Bytes de entrada de DDoS descartados  |  BytesPerSecond  |  Máxima | 
| **Sí**  | No |  Microsoft.Network/publicIPAddresses  |  BytesForwardedDDoS  |  Bytes de entrada de DDoS reenviados  |  BytesPerSecond  |  Máxima | 
| **Sí**  | No |  Microsoft.Network/publicIPAddresses  |  BytesInDDoS  |  Bytes de entrada de DDoS  |  BytesPerSecond  |  Máxima | 
| **Sí**  | No |  Microsoft.Network/publicIPAddresses  |  DDoSTriggerSYNPackets  |  Paquetes entrantes SYN para desencadenar la mitigación de DDoS  |  CountPerSecond  |  Máxima | 
| **Sí**  | No |  Microsoft.Network/publicIPAddresses  |  DDoSTriggerTCPPackets  |  Paquetes entrantes TCP para desencadenar la mitigación de DDoS  |  CountPerSecond  |  Máxima | 
| **Sí**  | No |  Microsoft.Network/publicIPAddresses  |  DDoSTriggerUDPPackets  |  Paquetes entrantes UDP para desencadenar la mitigación de DDoS  |  CountPerSecond  |  Máxima | 
| **Sí**  | No |  Microsoft.Network/publicIPAddresses  |  IfUnderDDoSAttack  |  Bajo ataque de DDoS o no  |  Count  |  Máxima | 
| **Sí**  | No |  Microsoft.Network/publicIPAddresses  |  PacketCount  |  Recuento de paquetes  |  Count  |  Total | 
| **Sí**  | No |  Microsoft.Network/publicIPAddresses  |  PacketsDroppedDDoS  |  Paquetes entrantes de DDoS descartados  |  CountPerSecond  |  Máxima | 
| **Sí**  | No |  Microsoft.Network/publicIPAddresses  |  PacketsForwardedDDoS  |  Paquetes entrantes de DDoS reenviados  |  CountPerSecond  |  Máxima | 
| **Sí**  | No |  Microsoft.Network/publicIPAddresses  |  PacketsInDDoS  |  Paquetes entrantes de DDoS  |  CountPerSecond  |  Máxima | 
| **Sí**  | No |  Microsoft.Network/publicIPAddresses  |  SynCount  |  Recuento de SYN  |  Count  |  Total | 
| **Sí**  | No |  Microsoft.Network/publicIPAddresses  |  TCPBytesDroppedDDoS  |  Bytes de entrada TCP de DDoS descartados  |  BytesPerSecond  |  Máxima | 
| **Sí**  | No |  Microsoft.Network/publicIPAddresses  |  TCPBytesForwardedDDoS  |  Bytes de entrada TCP de DDoS reenviados  |  BytesPerSecond  |  Máxima | 
| **Sí**  | No |  Microsoft.Network/publicIPAddresses  |  TCPBytesInDDoS  |  Bytes de entrada TCP de DDoS  |  BytesPerSecond  |  Máxima | 
| **Sí**  | No |  Microsoft.Network/publicIPAddresses  |  TCPPacketsDroppedDDoS  |  Paquetes TCP entrantes de DDoS descartados  |  CountPerSecond  |  Máxima | 
| **Sí**  | No |  Microsoft.Network/publicIPAddresses  |  TCPPacketsForwardedDDoS  |  Paquetes TCP entrantes de DDoS reenviados  |  CountPerSecond  |  Máxima | 
| **Sí**  | No |  Microsoft.Network/publicIPAddresses  |  TCPPacketsInDDoS  |  Paquetes TCP entrantes de DDoS  |  CountPerSecond  |  Máxima | 
| **Sí**  | No |  Microsoft.Network/publicIPAddresses  |  UDPBytesDroppedDDoS  |  Bytes de entrada UDP de DDoS descartados  |  BytesPerSecond  |  Máxima | 
| **Sí**  | No |  Microsoft.Network/publicIPAddresses  |  UDPBytesDroppedDDoS  |  Bytes de entrada UDP de DDoS reenviados  |  BytesPerSecond  |  Máxima | 
| **Sí**  | No |  Microsoft.Network/publicIPAddresses  |  UDPBytesInDDoS  |  Bytes de entrada UDP de DDoS  |  BytesPerSecond  |  Máxima | 
| **Sí**  | No |  Microsoft.Network/publicIPAddresses  |  UDPPacketsDroppedDDoS  |  Paquetes UDP entrantes de DDoS descartados  |  CountPerSecond  |  Máxima | 
| **Sí**  | No |  Microsoft.Network/publicIPAddresses  |  UDPPacketsForwardedDDoS  |  Paquetes UDP entrantes de DDoS reenviados  |  CountPerSecond  |  Máxima | 
| **Sí**  | No |  Microsoft.Network/publicIPAddresses  |  UDPPacketsInDDoS  |  Paquetes UDP entrantes de DDoS  |  CountPerSecond  |  Máxima | 
| **Sí**  | No |  Microsoft.Network/publicIPAddresses  |  VipAvailability  |  Disponibilidad de la ruta de acceso de datos  |  Count  |  Average | 
| **Sí**  | No |  Microsoft.Network/trafficManagerProfiles  |  ProbeAgentCurrentEndpointStateByProfileResourceId  |  Estado de punto de conexión por punto de conexión  |  Count  |  Máxima | 
| **Sí**  | **Sí** |  Microsoft.Network/trafficManagerProfiles  |  QpsByEndpoint  |  Consultas por punto de conexión devueltas  |  Count  |  Total | 
| **Sí**  | No |  Microsoft.Network/virtualNetworkGateways  |  AverageBandwidth  |  Ancho de banda S2S de puerta de enlace  |  BytesPerSecond  |  Average | 
| **Sí**  | No |  Microsoft.Network/virtualNetworkGateways  |  P2SBandwidth  |  Ancho de banda P2S de puerta de enlace  |  BytesPerSecond  |  Average | 
| **Sí**  | No |  Microsoft.Network/virtualNetworkGateways  |  P2SConnectionCount  |  Recuento de conexiones P2S  |  Count  |  Máxima | 
| **Sí**  | No |  Microsoft.Network/virtualNetworkGateways  |  TunnelAverageBandwidth  |  Ancho de banda de túnel  |  BytesPerSecond  |  Average | 
| **Sí**  | No |  Microsoft.Network/virtualNetworkGateways  |  TunnelEgressBytes  |  Bytes de salida de túnel  |  Bytes  |  Total | 
| **Sí**  | No |  Microsoft.Network/virtualNetworkGateways  |  TunnelEgressPacketDropTSMismatch  |  Colocación de paquetes con error de coincidencia del selector de tráfico de túnel de salida  |  Count  |  Total | 
| **Sí**  | No |  Microsoft.Network/virtualNetworkGateways  |  TunnelEgressPackets  |  Paquetes de salida de túnel  |  Count  |  Total | 
| **Sí**  | No |  Microsoft.Network/virtualNetworkGateways  |  TunnelIngressBytes  |  Bytes de salida de túnel  |  Bytes  |  Total | 
| **Sí**  | No |  Microsoft.Network/virtualNetworkGateways  |  TunnelIngressPacketDropTSMismatch  |  Colocación de paquetes con error de coincidencia del selector de tráfico de túnel de entrada  |  Count  |  Total | 
| **Sí**  | No |  Microsoft.Network/virtualNetworkGateways  |  TunnelIngressPackets  |  Paquetes de entrada de túnel  |  Count  |  Total | 
| **Sí**  | No |  Microsoft.Network/virtualNetworks  |  PingMeshAverageRoundtripMs  |  Round trip time for Pings to a VM [Tiempo de recorrido de ida y vuelta de los pings a una VM]  |  MilliSeconds  |  Average | 
| **Sí**  | No |  Microsoft.Network/virtualNetworks  |  PingMeshProbesFailedPercent  |  Failed Pings to a VM (Pings a una VM erróneos)  |  Percent  |  Average | 
| **Sí**  | **Sí** |  Microsoft.NotificationHubs/Namespaces/NotificationHubs  |  incoming  |  Mensajes entrantes  |  Count  |  Total | 
| **Sí**  | **Sí** |  Microsoft.NotificationHubs/Namespaces/NotificationHubs  |  incoming.all.failedrequests  |  Todas las solicitudes entrantes con error  |  Count  |  Total | 
| **Sí**  | **Sí** |  Microsoft.NotificationHubs/Namespaces/NotificationHubs  |  incoming.all.requests  |  Todas las solicitudes entrantes  |  Count  |  Total | 
| **Sí**  | **Sí** |  Microsoft.NotificationHubs/Namespaces/NotificationHubs  |  incoming.scheduled  |  Notificaciones push programadas enviadas  |  Count  |  Total | 
| **Sí**  | **Sí** |  Microsoft.NotificationHubs/Namespaces/NotificationHubs  |  incoming.scheduled.cancel  |  Notificaciones push programadas canceladas  |  Count  |  Total | 
| **Sí**  | **Sí** |  Microsoft.NotificationHubs/Namespaces/NotificationHubs  |  installation.all  |  Operaciones de administración de instalaciones  |  Count  |  Total | 
| **Sí**  | **Sí** |  Microsoft.NotificationHubs/Namespaces/NotificationHubs  |  installation.delete  |  Eliminar operaciones de instalación  |  Count  |  Total | 
| **Sí**  | **Sí** |  Microsoft.NotificationHubs/Namespaces/NotificationHubs  |  installation.get  |  Obtener operaciones de instalación  |  Count  |  Total | 
| **Sí**  | **Sí** |  Microsoft.NotificationHubs/Namespaces/NotificationHubs  |  installation.patch  |  Aplicar revisión a operaciones de instalación  |  Count  |  Total | 
| **Sí**  | **Sí** |  Microsoft.NotificationHubs/Namespaces/NotificationHubs  |  installation.upsert  |  Crear o actualizar operaciones de instalación  |  Count  |  Total | 
| **Sí**  | **Sí** |  Microsoft.NotificationHubs/Namespaces/NotificationHubs  |  notificationhub.pushes  |  Todas las notificaciones salientes  |  Count  |  Total | 
| **Sí**  | No |  Microsoft.NotificationHubs/Namespaces/NotificationHubs  |  outgoing.allpns.badorexpiredchannel  |  Errores de canal incorrecto o expirado  |  Count  |  Total | 
| **Sí**  | **Sí** |  Microsoft.NotificationHubs/Namespaces/NotificationHubs  |  outgoing.allpns.channelerror  |  Errores de canal  |  Count  |  Total | 
| **Sí**  | **Sí** |  Microsoft.NotificationHubs/Namespaces/NotificationHubs  |  outgoing.allpns.invalidpayload  |  Errores de carga útil  |  Count  |  Total | 
| **Sí**  | **Sí** |  Microsoft.NotificationHubs/Namespaces/NotificationHubs  |  outgoing.allpns.pnserror  |  Errores del sistema de notificación externo  |  Count  |  Total | 
| **Sí**  | **Sí** |  Microsoft.NotificationHubs/Namespaces/NotificationHubs  |  outgoing.allpns.success  |  Notificaciones correctas  |  Count  |  Total | 
| **Sí**  | **Sí** |  Microsoft.NotificationHubs/Namespaces/NotificationHubs  |  outgoing.apns.badchannel  |  Error de canal incorrecto de APNS  |  Count  |  Total | 
| **Sí**  | **Sí** |  Microsoft.NotificationHubs/Namespaces/NotificationHubs  |  outgoing.apns.expiredchannel  |  Error de canal expirado de APNS  |  Count  |  Total | 
| **Sí**  | **Sí** |  Microsoft.NotificationHubs/Namespaces/NotificationHubs  |  outgoing.apns.invalidcredentials  |  Errores de autorización de APNS  |  Count  |  Total | 
| **Sí**  | **Sí** |  Microsoft.NotificationHubs/Namespaces/NotificationHubs  |  outgoing.apns.invalidnotificationsize  |  Error de tamaño de notificación no válido de APNS  |  Count  |  Total | 
| **Sí**  | **Sí** |  Microsoft.NotificationHubs/Namespaces/NotificationHubs  |  outgoing.apns.pnserror  |  Errores de APNS  |  Count  |  Total | 
| **Sí**  | **Sí** |  Microsoft.NotificationHubs/Namespaces/NotificationHubs  |  outgoing.apns.success  |  Notificaciones de APNS correctas  |  Count  |  Total | 
| **Sí**  | **Sí** |  Microsoft.NotificationHubs/Namespaces/NotificationHubs  |  outgoing.gcm.authenticationerror  |  Errores de autenticación de GCM  |  Count  |  Total | 
| **Sí**  | **Sí** |  Microsoft.NotificationHubs/Namespaces/NotificationHubs  |  outgoing.gcm.badchannel  |  Error de canal incorrecto de GCM  |  Count  |  Total | 
| **Sí**  | **Sí** |  Microsoft.NotificationHubs/Namespaces/NotificationHubs  |  outgoing.gcm.expiredchannel  |  Error de canal expirado de GCM  |  Count  |  Total | 
| **Sí**  | **Sí** |  Microsoft.NotificationHubs/Namespaces/NotificationHubs  |  outgoing.gcm.invalidcredentials  |  Errores de autorización de GCM (credenciales no válidas)  |  Count  |  Total | 
| **Sí**  | **Sí** |  Microsoft.NotificationHubs/Namespaces/NotificationHubs  |  outgoing.gcm.invalidnotificationformat  |  Formato de notificación no válido de GCM  |  Count  |  Total | 
| **Sí**  | **Sí** |  Microsoft.NotificationHubs/Namespaces/NotificationHubs  |  outgoing.gcm.invalidnotificationsize  |  Error de tamaño de notificación no válido de GCM  |  Count  |  Total | 
| **Sí**  | **Sí** |  Microsoft.NotificationHubs/Namespaces/NotificationHubs  |  outgoing.gcm.pnserror  |  Errores de GCM  |  Count  |  Total | 
| **Sí**  | **Sí** |  Microsoft.NotificationHubs/Namespaces/NotificationHubs  |  outgoing.gcm.success  |  Notificaciones de GCM correctas  |  Count  |  Total | 
| **Sí**  | **Sí** |  Microsoft.NotificationHubs/Namespaces/NotificationHubs  |  outgoing.gcm.throttled  |  Notificaciones limitadas de GCM  |  Count  |  Total | 
| **Sí**  | **Sí** |  Microsoft.NotificationHubs/Namespaces/NotificationHubs  |  outgoing.gcm.wrongchannel  |  Error de canal incorrecto de GCM  |  Count  |  Total | 
| **Sí**  | **Sí** |  Microsoft.NotificationHubs/Namespaces/NotificationHubs  |  outgoing.mpns.authenticationerror  |  Errores de autenticación de MPNS  |  Count  |  Total | 
| **Sí**  | **Sí** |  Microsoft.NotificationHubs/Namespaces/NotificationHubs  |  outgoing.mpns.badchannel  |  Error de canal incorrecto de MPNS  |  Count  |  Total | 
| **Sí**  | **Sí** |  Microsoft.NotificationHubs/Namespaces/NotificationHubs  |  outgoing.mpns.channeldisconnected  |  Canal de MPNS desconectado  |  Count  |  Total | 
| **Sí**  | **Sí** |  Microsoft.NotificationHubs/Namespaces/NotificationHubs  |  outgoing.mpns.dropped  |  Notificaciones descartadas de MPNS  |  Count  |  Total | 
| **Sí**  | **Sí** |  Microsoft.NotificationHubs/Namespaces/NotificationHubs  |  outgoing.mpns.invalidcredentials  |  Credenciales no válidas de MPNS  |  Count  |  Total | 
| **Sí**  | **Sí** |  Microsoft.NotificationHubs/Namespaces/NotificationHubs  |  outgoing.mpns.invalidnotificationformat  |  Formato de notificación no válido de MPNS  |  Count  |  Total | 
| **Sí**  | **Sí** |  Microsoft.NotificationHubs/Namespaces/NotificationHubs  |  outgoing.mpns.pnserror  |  Errores de MPNS  |  Count  |  Total | 
| **Sí**  | **Sí** |  Microsoft.NotificationHubs/Namespaces/NotificationHubs  |  outgoing.mpns.success  |  Notificaciones de MPNS correctas  |  Count  |  Total | 
| **Sí**  | **Sí** |  Microsoft.NotificationHubs/Namespaces/NotificationHubs  |  outgoing.mpns.throttled  |  Notificaciones limitadas de MPNS  |  Count  |  Total | 
| **Sí**  | **Sí** |  Microsoft.NotificationHubs/Namespaces/NotificationHubs  |  outgoing.wns.authenticationerror  |  Errores de autenticación de WNS  |  Count  |  Total | 
| **Sí**  | **Sí** |  Microsoft.NotificationHubs/Namespaces/NotificationHubs  |  outgoing.wns.badchannel  |  Error de canal incorrecto de WNS  |  Count  |  Total | 
| **Sí**  | **Sí** |  Microsoft.NotificationHubs/Namespaces/NotificationHubs  |  outgoing.wns.channeldisconnected  |  Canal de WNS desconectado  |  Count  |  Total | 
| **Sí**  | **Sí** |  Microsoft.NotificationHubs/Namespaces/NotificationHubs  |  outgoing.wns.channelthrottled  |  Canal de WNS limitado  |  Count  |  Total | 
| **Sí**  | **Sí** |  Microsoft.NotificationHubs/Namespaces/NotificationHubs  |  outgoing.wns.dropped  |  Notificaciones descartadas de WNS  |  Count  |  Total | 
| **Sí**  | **Sí** |  Microsoft.NotificationHubs/Namespaces/NotificationHubs  |  outgoing.wns.expiredchannel  |  Error de canal expirado de WNS  |  Count  |  Total | 
| **Sí**  | **Sí** |  Microsoft.NotificationHubs/Namespaces/NotificationHubs  |  outgoing.wns.invalidcredentials  |  Errores de autorización de WNS (credenciales no válidas)  |  Count  |  Total | 
| **Sí**  | **Sí** |  Microsoft.NotificationHubs/Namespaces/NotificationHubs  |  outgoing.wns.invalidnotificationformat  |  Formato de notificación no válido de WNS  |  Count  |  Total | 
| **Sí**  | **Sí** |  Microsoft.NotificationHubs/Namespaces/NotificationHubs  |  outgoing.wns.invalidnotificationsize  |  Error de tamaño de notificación no válido de WNS  |  Count  |  Total | 
| **Sí**  | **Sí** |  Microsoft.NotificationHubs/Namespaces/NotificationHubs  |  outgoing.wns.invalidtoken  |  Errores de autorización de WNS (token no válido)  |  Count  |  Total | 
| **Sí**  | **Sí** |  Microsoft.NotificationHubs/Namespaces/NotificationHubs  |  outgoing.wns.pnserror  |  Errores de WNS  |  Count  |  Total | 
| **Sí**  | **Sí** |  Microsoft.NotificationHubs/Namespaces/NotificationHubs  |  outgoing.wns.success  |  Notificaciones de WNS correctas  |  Count  |  Total | 
| **Sí**  | **Sí** |  Microsoft.NotificationHubs/Namespaces/NotificationHubs  |  outgoing.wns.throttled  |  Notificaciones limitadas de WNS  |  Count  |  Total | 
| **Sí**  | **Sí** |  Microsoft.NotificationHubs/Namespaces/NotificationHubs  |  outgoing.wns.tokenproviderunreachable  |  Errores de autorización de WNS (inaccesible)  |  Count  |  Total | 
| **Sí**  | **Sí** |  Microsoft.NotificationHubs/Namespaces/NotificationHubs  |  outgoing.wns.wrongtoken  |  Errores de autorización de WNS (token erróneo)  |  Count  |  Total | 
| **Sí**  | **Sí** |  Microsoft.NotificationHubs/Namespaces/NotificationHubs  |  registration.all  |  Operaciones de registro  |  Count  |  Total | 
| **Sí**  | **Sí** |  Microsoft.NotificationHubs/Namespaces/NotificationHubs  |  registration.create  |  Operaciones de creación de registros  |  Count  |  Total | 
| **Sí**  | **Sí** |  Microsoft.NotificationHubs/Namespaces/NotificationHubs  |  registration.delete  |  Operaciones de eliminación de registros  |  Count  |  Total | 
| **Sí**  | **Sí** |  Microsoft.NotificationHubs/Namespaces/NotificationHubs  |  registration.get  |  Operaciones de lectura de registros  |  Count  |  Total | 
| **Sí**  | **Sí** |  Microsoft.NotificationHubs/Namespaces/NotificationHubs  |  registration.update  |  Operaciones de actualización de registros  |  Count  |  Total | 
| **Sí**  | **Sí** |  Microsoft.NotificationHubs/Namespaces/NotificationHubs  |  scheduled.pending  |  Notificaciones programadas pendientes  |  Count  |  Total | 
| **Sí**  | No |  Microsoft.OperationalInsights/workspaces  |  Average_% Available Memory  |  % de memoria disponible  |  Count  |  Average | 
| **Sí**  | No |  Microsoft.OperationalInsights/workspaces  |  Average_% Available Swap Space  |  % de espacio de intercambio disponible  |  Count  |  Average | 
| **Sí**  | No |  Microsoft.OperationalInsights/workspaces  |  Average_% Committed Bytes In Use  |  % de bytes confirmados en uso  |  Count  |  Average | 
| **Sí**  | No |  Microsoft.OperationalInsights/workspaces  |  Average_% DPC Time  |  % de tiempo de DPC  |  Count  |  Average | 
| **Sí**  | No |  Microsoft.OperationalInsights/workspaces  |  Average_% Free Inodes  |  % de Inodes libres  |  Count  |  Average | 
| **Sí**  | No |  Microsoft.OperationalInsights/workspaces  |  Average_% Free Space  |  % de espacio libre  |  Count  |  Average | 
| **Sí**  | No |  Microsoft.OperationalInsights/workspaces  |  Average_% Free Space  |  % de espacio libre  |  Count  |  Average | 
| **Sí**  | No |  Microsoft.OperationalInsights/workspaces  |  Average_% Idle Time  |  % de tiempo de inactividad  |  Count  |  Average | 
| **Sí**  | No |  Microsoft.OperationalInsights/workspaces  |  Average_% Interrupt Time  |  % de tiempo de interrupción  |  Count  |  Average | 
| **Sí**  | No |  Microsoft.OperationalInsights/workspaces  |  Average_% IO Wait Time  |  % de tiempo de espera de E/S  |  Count  |  Average | 
| **Sí**  | No |  Microsoft.OperationalInsights/workspaces  |  Average_% Nice Time  |  % de tiempo bueno  |  Count  |  Average | 
| **Sí**  | No |  Microsoft.OperationalInsights/workspaces  |  Average_% Privileged Time  |  % de tiempo con privilegios  |  Count  |  Average | 
| **Sí**  | No |  Microsoft.OperationalInsights/workspaces  |  Average_% Processor Time  |  % de tiempo de procesador  |  Count  |  Average | 
| **Sí**  | No |  Microsoft.OperationalInsights/workspaces  |  Average_% Processor Time  |  % de tiempo de procesador  |  Count  |  Average | 
| **Sí**  | No |  Microsoft.OperationalInsights/workspaces  |  Average_% Used Inodes  |  % de Inodes usados  |  Count  |  Average | 
| **Sí**  | No |  Microsoft.OperationalInsights/workspaces  |  Average_% Used Memory  |  % de memoria usada  |  Count  |  Average | 
| **Sí**  | No |  Microsoft.OperationalInsights/workspaces  |  Average_% Used Space  |  % espacio usado  |  Count  |  Average | 
| **Sí**  | No |  Microsoft.OperationalInsights/workspaces  |  Average_% Used Swap Space  |  % de espacio de intercambio usado  |  Count  |  Average | 
| **Sí**  | No |  Microsoft.OperationalInsights/workspaces  |  Average_% User Time  |  % de tiempo de usuario  |  Count  |  Average | 
| **Sí**  | No |  Microsoft.OperationalInsights/workspaces  |  Average_Available MBytes  |  MB disponibles  |  Count  |  Average | 
| **Sí**  | No |  Microsoft.OperationalInsights/workspaces  |  Average_Available MBytes Memory  |  MB de memoria disponibles  |  Count  |  Average | 
| **Sí**  | No |  Microsoft.OperationalInsights/workspaces  |  Average_Available MBytes Swap  |  Intercambio de MB disponibles  |  Count  |  Average | 
| **Sí**  | No |  Microsoft.OperationalInsights/workspaces  |  Average_Avg. Segundos de disco/lecturas  |  Prom. Segundos de disco/lecturas  |  Count  |  Average | 
| **Sí**  | No |  Microsoft.OperationalInsights/workspaces  |  Average_Avg. Segundos de disco/lecturas  |  Prom. Segundos de disco/lecturas  |  Count  |  Average | 
| **Sí**  | No |  Microsoft.OperationalInsights/workspaces  |  Average_Avg. Segundos de disco/transferencias  |  Prom. Segundos de disco/transferencias  |  Count  |  Average | 
| **Sí**  | No |  Microsoft.OperationalInsights/workspaces  |  Average_Avg. Segundos de disco/escrituras  |  Prom. Segundos de disco/escrituras  |  Count  |  Average | 
| **Sí**  | No |  Microsoft.OperationalInsights/workspaces  |  Average_Avg. Segundos de disco/escrituras  |  Prom. Segundos de disco/escrituras  |  Count  |  Average | 
| **Sí**  | No |  Microsoft.OperationalInsights/workspaces  |  Average_Bytes Received/sec  |  Bytes recibidos por segundo  |  Count  |  Average | 
| **Sí**  | No |  Microsoft.OperationalInsights/workspaces  |  Average_Bytes Sent/sec  |  Bytes enviados por segundo  |  Count  |  Average | 
| **Sí**  | No |  Microsoft.OperationalInsights/workspaces  |  Average_Bytes Total/sec  |  Bytes totales por segundo  |  Count  |  Average | 
| **Sí**  | No |  Microsoft.OperationalInsights/workspaces  |  Average_Current Disk Queue Length  |  Current Disk Queue Length  |  Count  |  Average | 
| **Sí**  | No |  Microsoft.OperationalInsights/workspaces  |  Average_Disk Read Bytes/sec  |  Bytes de lectura de disco/s  |  Count  |  Average | 
| **Sí**  | No |  Microsoft.OperationalInsights/workspaces  |  Average_Disk Reads/sec  |  Lecturas de disco/s  |  Count  |  Average | 
| **Sí**  | No |  Microsoft.OperationalInsights/workspaces  |  Average_Disk Reads/sec  |  Lecturas de disco/s  |  Count  |  Average | 
| **Sí**  | No |  Microsoft.OperationalInsights/workspaces  |  Average_Disk Transfers/sec  |  Transferencias de disco/s  |  Count  |  Average | 
| **Sí**  | No |  Microsoft.OperationalInsights/workspaces  |  Average_Disk Transfers/sec  |  Transferencias de disco/s  |  Count  |  Average | 
| **Sí**  | No |  Microsoft.OperationalInsights/workspaces  |  Average_Disk Write Bytes/sec  |  Bytes de escritura en disco/s  |  Count  |  Average | 
| **Sí**  | No |  Microsoft.OperationalInsights/workspaces  |  Average_Disk Writes/sec  |  Escrituras en disco/s  |  Count  |  Average | 
| **Sí**  | No |  Microsoft.OperationalInsights/workspaces  |  Average_Disk Writes/sec  |  Escrituras en disco/s  |  Count  |  Average | 
| **Sí**  | No |  Microsoft.OperationalInsights/workspaces  |  Average_Free Megabytes  |  Megabytes libres  |  Count  |  Average | 
| **Sí**  | No |  Microsoft.OperationalInsights/workspaces  |  Average_Free Megabytes  |  Megabytes libres  |  Count  |  Average | 
| **Sí**  | No |  Microsoft.OperationalInsights/workspaces  |  Average_Free Physical Memory  |  Memoria física libre  |  Count  |  Average | 
| **Sí**  | No |  Microsoft.OperationalInsights/workspaces  |  Average_Free Space in Paging Files  |  Espacio libre en archivos de paginación  |  Count  |  Average | 
| **Sí**  | No |  Microsoft.OperationalInsights/workspaces  |  Average_Free Virtual Memory  |  Memoria virtual libre  |  Count  |  Average | 
| **Sí**  | No |  Microsoft.OperationalInsights/workspaces  |  Average_Logical Disk Bytes/sec  |  Bytes de disco lógico/s  |  Count  |  Average | 
| **Sí**  | No |  Microsoft.OperationalInsights/workspaces  |  Average_Page Reads/sec  |  Lecturas de página/s  |  Count  |  Average | 
| **Sí**  | No |  Microsoft.OperationalInsights/workspaces  |  Average_Page Writes/sec  |  Escrituras de página/s  |  Count  |  Average | 
| **Sí**  | No |  Microsoft.OperationalInsights/workspaces  |  Average_Pages/sec  |  Páginas/s  |  Count  |  Average | 
| **Sí**  | No |  Microsoft.OperationalInsights/workspaces  |  Average_Pct Privileged Time  |  Porcentaje de tiempo con privilegios  |  Count  |  Average | 
| **Sí**  | No |  Microsoft.OperationalInsights/workspaces  |  Average_Pct User Time  |  Porcentaje de tiempo de usuario  |  Count  |  Average | 
| **Sí**  | No |  Microsoft.OperationalInsights/workspaces  |  Average_Physical Disk Bytes/sec  |  Bytes de disco físico/s  |  Count  |  Average | 
| **Sí**  | No |  Microsoft.OperationalInsights/workspaces  |  Average_Processes  |  Procesos  |  Count  |  Average | 
| **Sí**  | No |  Microsoft.OperationalInsights/workspaces  |  Average_Processor Queue Length  |  Longitud de cola del procesador  |  Count  |  Average | 
| **Sí**  | No |  Microsoft.OperationalInsights/workspaces  |  Average_Size Stored In Paging Files  |  Tamaño almacenado en archivos de paginación  |  Count  |  Average | 
| **Sí**  | No |  Microsoft.OperationalInsights/workspaces  |  Average_Total Bytes  |  Número total de bytes  |  Count  |  Average | 
| **Sí**  | No |  Microsoft.OperationalInsights/workspaces  |  Average_Total Bytes Received  |  Número total de bytes recibidos  |  Count  |  Average | 
| **Sí**  | No |  Microsoft.OperationalInsights/workspaces  |  Average_Total Bytes Transmitted  |  Número total de bytes transmitidos  |  Count  |  Average | 
| **Sí**  | No |  Microsoft.OperationalInsights/workspaces  |  Average_Total Collisions  |  Colisiones totales  |  Count  |  Average | 
| **Sí**  | No |  Microsoft.OperationalInsights/workspaces  |  Average_Total Packets Received  |  Número total de paquetes recibidos  |  Count  |  Average | 
| **Sí**  | No |  Microsoft.OperationalInsights/workspaces  |  Average_Total Packets Transmitted  |  Número total de paquetes transmitidos  |  Count  |  Average | 
| **Sí**  | No |  Microsoft.OperationalInsights/workspaces  |  Average_Total Rx Errors  |  Errores de Rx totales  |  Count  |  Average | 
| **Sí**  | No |  Microsoft.OperationalInsights/workspaces  |  Average_Total Tx Errors  |  Errores de Tx totales  |  Count  |  Average | 
| **Sí**  | No |  Microsoft.OperationalInsights/workspaces  |  Average_Uptime  |  Tiempo de actividad  |  Count  |  Average | 
| **Sí**  | No |  Microsoft.OperationalInsights/workspaces  |  Average_Used MBytes Swap Space  |  Espacio de intercambio de MB usado  |  Count  |  Average | 
| **Sí**  | No |  Microsoft.OperationalInsights/workspaces  |  Average_Used Memory kBytes  |  Kilobytes de memoria usados  |  Count  |  Average | 
| **Sí**  | No |  Microsoft.OperationalInsights/workspaces  |  Average_Used Memory MBytes  |  MB de memoria usados  |  Count  |  Average | 
| **Sí**  | No |  Microsoft.OperationalInsights/workspaces  |  Average_Users  |  Usuarios  |  Count  |  Average | 
| **Sí**  | No |  Microsoft.OperationalInsights/workspaces  |  Average_Virtual Shared Memory  |  Memoria compartida virtual  |  Count  |  Average | 
| **Sí**  | No |  Microsoft.OperationalInsights/workspaces  |  Evento  |  Evento  |  Count  |  Average | 
| **Sí**  | **Sí** |  Microsoft.OperationalInsights/workspaces  |  Latido  |  Latido  |  Count  |  Total | 
| **Sí**  | No |  Microsoft.OperationalInsights/workspaces  |  Actualizar  |  Actualizar  |  Count  |  Average | 
| **Sí**  | No |  Microsoft.PowerBIDedicated/capacities  |  memory_metric  |  Memoria  |  Bytes  |  Average | 
| **Sí**  | No |  Microsoft.PowerBIDedicated/capacities  |  memory_thrashing_metric  |  Memory Thrashing (Datasets) [Paginación excesiva de memoria (conjuntos de datos)]  |  Percent  |  Average | 
| **Sí**  | No |  Microsoft.PowerBIDedicated/capacities  |  qpu_high_utilization_metric  |  Uso intensivo de QPU  |  Count  |  Total | 
| **Sí**  | No |  Microsoft.PowerBIDedicated/capacities  |  QueryDuration  |  Query Duration (Datasets) [Duración de la consulta (conjuntos de datos)]  |  Milisegundos  |  Average | 
| **Sí**  | No |  Microsoft.PowerBIDedicated/capacities  |  QueryPoolJobQueueLength  |  Query Pool Job Queue Length (Datasets) [longitud de cola de trabajos del grupo de consultas (conjuntos de datos)]  |  Count  |  Average | 
| No  | No |  Microsoft.Relay/namespaces  |  ActiveConnections  |  ActiveConnections  |  Count  |  Total | 
| No  | No |  Microsoft.Relay/namespaces  |  ActiveListeners  |  ActiveListeners  |  Count  |  Total | 
| **Sí**  | **Sí** |  Microsoft.Relay/namespaces  |  BytesTransferred  |  BytesTransferred  |  Count  |  Total | 
| No  | No |  Microsoft.Relay/namespaces  |  ListenerConnections-ClientError  |  ListenerConnections-ClientError  |  Count  |  Total | 
| No  | No |  Microsoft.Relay/namespaces  |  ListenerConnections-ServerError  |  ListenerConnections-ServerError  |  Count  |  Total | 
| No  | No |  Microsoft.Relay/namespaces  |  ListenerConnections-Success  |  ListenerConnections-Success  |  Count  |  Total | 
| No  | No |  Microsoft.Relay/namespaces  |  ListenerConnections-TotalRequests  |  ListenerConnections-TotalRequests  |  Count  |  Total | 
| No  | No |  Microsoft.Relay/namespaces  |  ListenerDisconnects  |  ListenerDisconnects  |  Count  |  Total | 
| No  | No |  Microsoft.Relay/namespaces  |  SenderConnections-ClientError  |  SenderConnections-ClientError  |  Count  |  Total | 
| No  | No |  Microsoft.Relay/namespaces  |  SenderConnections-ServerError  |  SenderConnections-ServerError  |  Count  |  Total | 
| No  | No |  Microsoft.Relay/namespaces  |  SenderConnections-Success  |  SenderConnections-Success  |  Count  |  Total | 
| No  | No |  Microsoft.Relay/namespaces  |  SenderConnections-TotalRequests  |  SenderConnections-TotalRequests  |  Count  |  Total | 
| No  | No |  Microsoft.Relay/namespaces  |  SenderDisconnects  |  SenderDisconnects  |  Count  |  Total | 
| **Sí**  | No |  Microsoft.Search/searchServices  |  SearchLatency  |  Latencia de búsqueda  |  Segundos  |  Average | 
| **Sí**  | No |  Microsoft.Search/searchServices  |  SearchQueriesPerSecond  |  Consultas de búsqueda por segundo  |  CountPerSecond  |  Average | 
| **Sí**  | No |  Microsoft.Search/searchServices  |  ThrottledSearchQueriesPercentage  |  Porcentaje de consultas de búsqueda limitadas  |  Percent  |  Average | 
| No  | No |  Microsoft.ServiceBus/namespaces  |  ActiveConnections  |  ActiveConnections  |  Count  |  Total | 
| No  | No |  Microsoft.ServiceBus/namespaces  |  ActiveMessages  |  Recuento de mensajes activos de una cola/tema.  |  Count  |  Average | 
| No  | No |  Microsoft.ServiceBus/namespaces  |  ConnectionsClosed  |  Conexiones cerradas.  |  Count  |  Average | 
| No  | No |  Microsoft.ServiceBus/namespaces  |  ConnectionsOpened  |  Conexiones abiertas.  |  Count  |  Average | 
| No  | No |  Microsoft.ServiceBus/namespaces  |  CPUXNS  |  CPU (Deprecated) [CPU (en desuso)]  |  Percent  |  Máxima | 
| No  | No |  Microsoft.ServiceBus/namespaces  |  DeadletteredMessages  |  Recuento de mensajes fallidos de una cola/tema.  |  Count  |  Average | 
| **Sí**  | **Sí** |  Microsoft.ServiceBus/namespaces  |  IncomingMessages  |  Mensajes entrantes  |  Count  |  Total | 
| **Sí**  | **Sí** |  Microsoft.ServiceBus/namespaces  |  IncomingRequests  |  Solicitudes entrantes  |  Count  |  Total | 
| No  | No |  Microsoft.ServiceBus/namespaces  |  error de Hadoop  |  Recuento de mensajes de una cola/tema.  |  Count  |  Average | 
| No  | No |  Microsoft.ServiceBus/namespaces  |  NamespaceCpuUsage  |  CPU  |  Percent  |  Máxima | 
| No  | No |  Microsoft.ServiceBus/namespaces  |  NamespaceMemoryUsage  |  Uso de la memoria  |  Percent  |  Máxima | 
| **Sí**  | **Sí** |  Microsoft.ServiceBus/namespaces  |  OutgoingMessages  |  Mensajes salientes  |  Count  |  Total | 
| No  | No |  Microsoft.ServiceBus/namespaces  |  ScheduledMessages  |  Recuento de mensajes programados de una cola/tema.  |  Count  |  Average | 
| No  | No |  Microsoft.ServiceBus/namespaces  |  ServerErrors  |  Errores del servidor.  |  Count  |  Total | 
| No  | No |  Microsoft.ServiceBus/namespaces  |  Size  |  Size  |  Bytes  |  Average | 
| No  | No |  Microsoft.ServiceBus/namespaces  |  SuccessfulRequests  |  Solicitudes correctas  |  Count  |  Total | 
| No  | No |  Microsoft.ServiceBus/namespaces  |  ThrottledRequests  |  Solicitudes limitadas.  |  Count  |  Total | 
| No  | No |  Microsoft.ServiceBus/namespaces  |  UserErrors  |  Errores de usuario.  |  Count  |  Total | 
| No  | No |  Microsoft.ServiceBus/namespaces  |  WSXNS  |  Memory Usage (Deprecated) [Uso de memoria (en desuso)]  |  Percent  |  Máxima | 
| No  | No |  Microsoft.ServiceFabricMesh/applications  |  ActualCpu  |  ActualCpu  |  Count  |  Average | 
| No  | No |  Microsoft.ServiceFabricMesh/applications  |  ActualMemory  |  ActualMemory  |  Bytes  |  Average | 
| No  | No |  Microsoft.ServiceFabricMesh/applications  |  AllocatedCpu  |  AllocatedCpu  |  Count  |  Average | 
| No  | No |  Microsoft.ServiceFabricMesh/applications  |  AllocatedMemory  |  AllocatedMemory  |  Bytes  |  Average | 
| No  | No |  Microsoft.ServiceFabricMesh/applications  |  ApplicationStatus  |  ApplicationStatus  |  Count  |  Average | 
| No  | No |  Microsoft.ServiceFabricMesh/applications  |  ContainerStatus  |  ContainerStatus  |  Count  |  Average | 
| No  | No |  Microsoft.ServiceFabricMesh/applications  |  CpuUtilization  |  CpuUtilization  |  Percent  |  Average | 
| No  | No |  Microsoft.ServiceFabricMesh/applications  |  MemoryUtilization  |  MemoryUtilization  |  Percent  |  Average | 
| No  | No |  Microsoft.ServiceFabricMesh/applications  |  RestartCount  |  RestartCount  |  Count  |  Average | 
| No  | No |  Microsoft.ServiceFabricMesh/applications  |  ServiceReplicaStatus  |  ServiceReplicaStatus  |  Count  |  Average | 
| No  | No |  Microsoft.ServiceFabricMesh/applications  |  ServiceStatus  |  ServiceStatus  |  Count  |  Average | 
| **Sí**  | **Sí** |  Microsoft.SignalRService/SignalR  |  ConnectionCount  |  Número de conexiones  |  Count  |  Máxima | 
| **Sí**  | **Sí** |  Microsoft.SignalRService/SignalR  |  InboundTraffic  |  Tráfico entrante  |  Bytes  |  Total | 
| **Sí**  | **Sí** |  Microsoft.SignalRService/SignalR  |  MessageCount  |  Número de mensajes  |  Count  |  Total | 
| **Sí**  | **Sí** |  Microsoft.SignalRService/SignalR  |  OutboundTraffic  |  Tráfico saliente  |  Bytes  |  Total | 
| **Sí**  | **Sí** |  Microsoft.SignalRService/SignalR  |  SystemErrors  |  Errores de sistema  |  Percent  |  Máxima | 
| **Sí**  | **Sí** |  Microsoft.SignalRService/SignalR  |  UserErrors  |  Errores de usuario  |  Percent  |  Máxima | 
| **Sí**  | **Sí** |  Microsoft.Sql/managedInstances  |  avg_cpu_percent  |  Porcentaje de CPU medio  |  Percent  |  Average | 
| **Sí**  | No |  Microsoft.Sql/managedInstances  |  io_bytes_read  |  Bytes de E/S leídos  |  Bytes  |  Average | 
| **Sí**  | No |  Microsoft.Sql/managedInstances  |  io_bytes_written  |  Bytes de E/S escritos  |  Bytes  |  Average | 
| **Sí**  | No |  Microsoft.Sql/managedInstances  |  io_requests  |  Recuento de solicitudes de E/S  |  Count  |  Average | 
| **Sí**  | No |  Microsoft.Sql/managedInstances  |  reserved_storage_mb  |  Espacio de almacenamiento reservado  |  Count  |  Average | 
| **Sí**  | No |  Microsoft.Sql/managedInstances  |  storage_space_used_mb  |  Espacio de almacenamiento usado  |  Count  |  Average | 
| **Sí**  | No |  Microsoft.Sql/managedInstances  |  virtual_core_count  |  Recuento de núcleos virtuales  |  Count  |  Average | 
| No  | No |  Microsoft.Sql/servers  |  database_dtu_consumption_percent  |  Porcentaje de DTU  |  Percent  |  Average | 
| No  | No |  Microsoft.Sql/servers  |  database_storage_used  |  Espacio de datos usado  |  Bytes  |  Average | 
| **Sí**  | **Sí** |  Microsoft.Sql/servers  |  dtu_consumption_percent  |  Porcentaje de DTU  |  Percent  |  Average | 
| **Sí**  | **Sí** |  Microsoft.Sql/servers  |  dtu_used  |  DTU utilizada  |  Count  |  Average | 
| **Sí**  | No |  Microsoft.Sql/servers  |  storage_used  |  Espacio de datos usado  |  Bytes  |  Average | 
| **Sí**  | No |  Microsoft.Sql/servers/databases  |  allocated_data_storage  |  Espacio de datos asignado  |  Bytes  |  Average | 
| **Sí**  | No |  Microsoft.Sql/servers/databases  |  app_cpu_billed  |  CPU de aplicación facturada  |  Count  |  Total | 
| **Sí**  | No |  Microsoft.Sql/servers/databases  |  app_cpu_percent  |  Porcentaje de CPU de la aplicación  |  Percent  |  Average | 
| **Sí**  | No |  Microsoft.Sql/servers/databases  |  app_memory_percent  |  App memory percentage (Porcentaje de memoria de la aplicación)  |  Percent  |  Average | 
| **Sí**  | **Sí** |  Microsoft.Sql/servers/databases  |  blocked_by_firewall  |  Bloqueado por el firewall  |  Count  |  Total | 
| **Sí**  | **Sí** |  Microsoft.Sql/servers/databases  |  cache_hit_percent  |  Porcentaje de aciertos de caché  |  Percent  |  Máxima | 
| **Sí**  | No |  Microsoft.Sql/servers/databases  |  cache_used_percent  |  Porcentaje de caché usada  |  Percent  |  Máxima | 
| **Sí**  | **Sí** |  Microsoft.Sql/servers/databases  |  connection_failed  |  Conexiones con errores  |  Count  |  Total | 
| **Sí**  | **Sí** |  Microsoft.Sql/servers/databases  |  connection_successful  |  Conexiones correctas  |  Count  |  Total | 
| **Sí**  | No |  Microsoft.Sql/servers/databases  |  cpu_limit  |  Límite de CPU  |  Count  |  Average | 
| **Sí**  | **Sí** |  Microsoft.Sql/servers/databases  |  cpu_percent  |  Porcentaje de CPU  |  Percent  |  Average | 
| **Sí**  | **Sí** |  Microsoft.Sql/servers/databases  |  cpu_used  |  CPU utilizada  |  Count  |  Average | 
| **Sí**  | **Sí** |  Microsoft.Sql/servers/databases  |  deadlock  |  Interbloqueos  |  Count  |  Total | 
| **Sí**  | **Sí** |  Microsoft.Sql/servers/databases  |  dtu_consumption_percent  |  Porcentaje de DTU  |  Percent  |  Average | 
| **Sí**  | No |  Microsoft.Sql/servers/databases  |  dtu_limit  |  Límite de DTU  |  Count  |  Average | 
| **Sí**  | **Sí** |  Microsoft.Sql/servers/databases  |  dtu_used  |  DTU utilizada  |  Count  |  Average | 
| **Sí**  | **Sí** |  Microsoft.Sql/servers/databases  |  dwu_consumption_percent  |  Porcentaje de DWU  |  Percent  |  Máxima | 
| **Sí**  | No |  Microsoft.Sql/servers/databases  |  dwu_limit  |  Límite de DWU  |  Count  |  Máxima | 
| **Sí**  | **Sí** |  Microsoft.Sql/servers/databases  |  dwu_used  |  DWU utilizada  |  Count  |  Máxima | 
| **Sí**  | No |  Microsoft.Sql/servers/databases  |  local_tempdb_usage_percent  |  Porcentaje de tempdb local  |  Percent  |  Average | 
| **Sí**  | **Sí** |  Microsoft.Sql/servers/databases  |  log_write_percent  |  Porcentaje de E/S de registro  |  Percent  |  Average | 
| **Sí**  | No |  Microsoft.Sql/servers/databases  |  memory_usage_percent  |  Porcentaje de memoria  |  Percent  |  Máxima | 
| **Sí**  | **Sí** |  Microsoft.Sql/servers/databases  |  physical_data_read_percent  |  Porcentaje de E/S de datos  |  Percent  |  Average | 
| **Sí**  | **Sí** |  Microsoft.Sql/servers/databases  |  sessions_percent  |  Porcentaje de sesiones  |  Percent  |  Average | 
| **Sí**  | No |  Microsoft.Sql/servers/databases  |  storage  |  Espacio de datos usado  |  Bytes  |  Máxima | 
| **Sí**  | No |  Microsoft.Sql/servers/databases  |  storage_percent  |  Porcentaje de espacio de datos usado  |  Percent  |  Máxima | 
| **Sí**  | No |  Microsoft.Sql/servers/databases  |  tempdb_data_size  |  Kilobytes de tamaño de archivo de datos Tempdb  |  Count  |  Máxima | 
| **Sí**  | No |  Microsoft.Sql/servers/databases  |  tempdb_log_size  |  Kilobytes de tamaño de archivo de registro Tempdb  |  Count  |  Máxima | 
| **Sí**  | No |  Microsoft.Sql/servers/databases  |  tempdb_log_used_percent  |  Porcentaje de registro de tempdb usado  |  Percent  |  Máxima | 
| **Sí**  | **Sí** |  Microsoft.Sql/servers/databases  |  workers_percent  |  Porcentaje de trabajos  |  Percent  |  Average | 
| **Sí**  | **Sí** |  Microsoft.Sql/servers/databases  |  xtp_storage_percent  |  Porcentaje de almacenamiento de OLTP en memoria  |  Percent  |  Average | 
| **Sí**  | No |  Microsoft.Sql/servers/elasticPools  |  allocated_data_storage  |  Espacio de datos asignado  |  Bytes  |  Average | 
| **Sí**  | No |  Microsoft.Sql/servers/elasticPools  |  allocated_data_storage_percent  |  Porcentaje asignado de espacio de datos  |  Percent  |  Máxima | 
| **Sí**  | No |  Microsoft.Sql/servers/elasticPools  |  cpu_limit  |  Límite de CPU  |  Count  |  Average | 
| **Sí**  | **Sí** |  Microsoft.Sql/servers/elasticPools  |  cpu_percent  |  Porcentaje de CPU  |  Percent  |  Average | 
| **Sí**  | No |  Microsoft.Sql/servers/elasticPools  |  cpu_used  |  CPU utilizada  |  Count  |  Average | 
| No  | No |  Microsoft.Sql/servers/elasticPools  |  database_allocated_data_storage  |  Espacio de datos asignado  |  Bytes  |  Average | 
| No  | No |  Microsoft.Sql/servers/elasticPools  |  database_cpu_limit  |  Límite de CPU  |  Count  |  Average | 
| No  | No |  Microsoft.Sql/servers/elasticPools  |  database_cpu_percent  |  Porcentaje de CPU  |  Percent  |  Average | 
| No  | No |  Microsoft.Sql/servers/elasticPools  |  database_cpu_used  |  CPU utilizada  |  Count  |  Average | 
| No  | No |  Microsoft.Sql/servers/elasticPools  |  database_dtu_consumption_percent  |  Porcentaje de DTU  |  Percent  |  Average | 
| No  | No |  Microsoft.Sql/servers/elasticPools  |  database_eDTU_used  |  eDTU utilizada  |  Count  |  Average | 
| No  | No |  Microsoft.Sql/servers/elasticPools  |  database_log_write_percent  |  Porcentaje de E/S de registro  |  Percent  |  Average | 
| No  | No |  Microsoft.Sql/servers/elasticPools  |  database_physical_data_read_percent  |  Porcentaje de E/S de datos  |  Percent  |  Average | 
| No  | No |  Microsoft.Sql/servers/elasticPools  |  database_sessions_percent  |  Porcentaje de sesiones  |  Percent  |  Average | 
| No  | No |  Microsoft.Sql/servers/elasticPools  |  database_storage_used  |  Espacio de datos usado  |  Bytes  |  Average | 
| No  | No |  Microsoft.Sql/servers/elasticPools  |  database_workers_percent  |  Porcentaje de trabajos  |  Percent  |  Average | 
| **Sí**  | **Sí** |  Microsoft.Sql/servers/elasticPools  |  dtu_consumption_percent  |  Porcentaje de DTU  |  Percent  |  Average | 
| **Sí**  | No |  Microsoft.Sql/servers/elasticPools  |  eDTU_limit  |  Límite de eDTU  |  Count  |  Average | 
| **Sí**  | **Sí** |  Microsoft.Sql/servers/elasticPools  |  eDTU_used  |  eDTU utilizada  |  Count  |  Average | 
| **Sí**  | **Sí** |  Microsoft.Sql/servers/elasticPools  |  log_write_percent  |  Porcentaje de E/S de registro  |  Percent  |  Average | 
| **Sí**  | **Sí** |  Microsoft.Sql/servers/elasticPools  |  physical_data_read_percent  |  Porcentaje de E/S de datos  |  Percent  |  Average | 
| **Sí**  | **Sí** |  Microsoft.Sql/servers/elasticPools  |  sessions_percent  |  Porcentaje de sesiones  |  Percent  |  Average | 
| **Sí**  | No |  Microsoft.Sql/servers/elasticPools  |  storage_limit  |  Tamaño máximo de datos  |  Bytes  |  Average | 
| **Sí**  | No |  Microsoft.Sql/servers/elasticPools  |  storage_percent  |  Porcentaje de espacio de datos usado  |  Percent  |  Average | 
| **Sí**  | No |  Microsoft.Sql/servers/elasticPools  |  storage_used  |  Espacio de datos usado  |  Bytes  |  Average | 
| **Sí**  | No |  Microsoft.Sql/servers/elasticPools  |  tempdb_data_size  |  Kilobytes de tamaño de archivo de datos Tempdb  |  Count  |  Máxima | 
| **Sí**  | No |  Microsoft.Sql/servers/elasticPools  |  tempdb_log_size  |  Kilobytes de tamaño de archivo de registro Tempdb  |  Count  |  Máxima | 
| **Sí**  | No |  Microsoft.Sql/servers/elasticPools  |  tempdb_log_used_percent  |  Porcentaje de registro de tempdb usado  |  Percent  |  Máxima | 
| **Sí**  | **Sí** |  Microsoft.Sql/servers/elasticPools  |  workers_percent  |  Porcentaje de trabajos  |  Percent  |  Average | 
| **Sí**  | **Sí** |  Microsoft.Sql/servers/elasticPools  |  xtp_storage_percent  |  Porcentaje de almacenamiento de OLTP en memoria  |  Percent  |  Average | 
| **Sí**  | No |  Microsoft.Storage/storageAccounts  |  Disponibilidad  |  Disponibilidad  |  Percent  |  Average | 
| **Sí**  | No |  Microsoft.Storage/storageAccounts  |  Salida  |  Salida  |  Bytes  |  Total | 
| **Sí**  | No |  Microsoft.Storage/storageAccounts  |  Entrada  |  Entrada  |  Bytes  |  Total | 
| **Sí**  | No |  Microsoft.Storage/storageAccounts  |  SuccessE2ELatency  |  Latencia de E2E correcta  |  Milisegundos  |  Average | 
| **Sí**  | No |  Microsoft.Storage/storageAccounts  |  SuccessServerLatency  |  Latencia del servidor correcta  |  Milisegundos  |  Average | 
| **Sí**  | No |  Microsoft.Storage/storageAccounts  |  Transacciones  |  Transacciones  |  Count  |  Total | 
| No  | No |  Microsoft.Storage/storageAccounts  |  UsedCapacity  |  Capacidad usada  |  Bytes  |  Average | 
| **Sí**  | No |  Microsoft.Storage/storageAccounts/blobServices  |  Disponibilidad  |  Disponibilidad  |  Percent  |  Average | 
| No  | No |  Microsoft.Storage/storageAccounts/blobServices  |  BlobCapacity  |  Capacidad de Blob  |  Bytes  |  Average | 
| No  | No |  Microsoft.Storage/storageAccounts/blobServices  |  BlobCount  |  Recuento de blobs  |  Count  |  Average | 
| **Sí**  | No |  Microsoft.Storage/storageAccounts/blobServices  |  ContainerCount  |  Recuento de contenedores de blobs  |  Count  |  Average | 
| **Sí**  | No |  Microsoft.Storage/storageAccounts/blobServices  |  Salida  |  Salida  |  Bytes  |  Total | 
| No  | No |  Microsoft.Storage/storageAccounts/blobServices  |  IndexCapacity  |  Capacidad de índice  |  Bytes  |  Average | 
| **Sí**  | No |  Microsoft.Storage/storageAccounts/blobServices  |  Entrada  |  Entrada  |  Bytes  |  Total | 
| **Sí**  | No |  Microsoft.Storage/storageAccounts/blobServices  |  SuccessE2ELatency  |  Latencia de E2E correcta  |  Milisegundos  |  Average | 
| **Sí**  | No |  Microsoft.Storage/storageAccounts/blobServices  |  SuccessServerLatency  |  Latencia del servidor correcta  |  Milisegundos  |  Average | 
| **Sí**  | No |  Microsoft.Storage/storageAccounts/blobServices  |  Transacciones  |  Transacciones  |  Count  |  Total | 
| **Sí**  | No |  Microsoft.Storage/storageAccounts/fileServices  |  Disponibilidad  |  Disponibilidad  |  Percent  |  Average | 
| **Sí**  | No |  Microsoft.Storage/storageAccounts/fileServices  |  Salida  |  Salida  |  Bytes  |  Total | 
| No  | No |  Microsoft.Storage/storageAccounts/fileServices  |  FileCapacity  |  Capacidad de File  |  Bytes  |  Average | 
| No  | No |  Microsoft.Storage/storageAccounts/fileServices  |  FileCount  |  Recuento de archivos  |  Count  |  Average | 
| No  | No |  Microsoft.Storage/storageAccounts/fileServices  |  FileShareCount  |  Recuento de recursos compartidos de archivos  |  Count  |  Average | 
| No  | No |  Microsoft.Storage/storageAccounts/fileServices  |  FileShareQuota  |  File share quota size (Tamaño de cuota del recurso compartido de archivos)  |  Bytes  |  Average | 
| No  | No |  Microsoft.Storage/storageAccounts/fileServices  |  FileShareSnapshotCount  |  File share snapshot count (Recuento de instantáneas del recurso compartido de archivos)  |  Count  |  Average | 
| No  | No |  Microsoft.Storage/storageAccounts/fileServices  |  FileShareSnapshotSize  |  File share snapshot size (Tamaño de instantánea del recurso compartido de archivos)  |  Bytes  |  Average | 
| **Sí**  | No |  Microsoft.Storage/storageAccounts/fileServices  |  Entrada  |  Entrada  |  Bytes  |  Total | 
| **Sí**  | No |  Microsoft.Storage/storageAccounts/fileServices  |  SuccessE2ELatency  |  Latencia de E2E correcta  |  Milisegundos  |  Average | 
| **Sí**  | No |  Microsoft.Storage/storageAccounts/fileServices  |  SuccessServerLatency  |  Latencia del servidor correcta  |  Milisegundos  |  Average | 
| **Sí**  | No |  Microsoft.Storage/storageAccounts/fileServices  |  Transacciones  |  Transacciones  |  Count  |  Total | 
| **Sí**  | No |  Microsoft.Storage/storageAccounts/queueServices  |  Disponibilidad  |  Disponibilidad  |  Percent  |  Average | 
| **Sí**  | No |  Microsoft.Storage/storageAccounts/queueServices  |  Salida  |  Salida  |  Bytes  |  Total | 
| **Sí**  | No |  Microsoft.Storage/storageAccounts/queueServices  |  Entrada  |  Entrada  |  Bytes  |  Total | 
| **Sí**  | No |  Microsoft.Storage/storageAccounts/queueServices  |  QueueCapacity  |  Capacidad de Queue  |  Bytes  |  Average | 
| **Sí**  | No |  Microsoft.Storage/storageAccounts/queueServices  |  QueueCount  |  Recuento de colas  |  Count  |  Average | 
| **Sí**  | No |  Microsoft.Storage/storageAccounts/queueServices  |  QueueMessageCount  |  Recuento de mensajes de Queue  |  Count  |  Average | 
| **Sí**  | No |  Microsoft.Storage/storageAccounts/queueServices  |  SuccessE2ELatency  |  Latencia de E2E correcta  |  Milisegundos  |  Average | 
| **Sí**  | No |  Microsoft.Storage/storageAccounts/queueServices  |  SuccessServerLatency  |  Latencia del servidor correcta  |  Milisegundos  |  Average | 
| **Sí**  | No |  Microsoft.Storage/storageAccounts/queueServices  |  Transacciones  |  Transacciones  |  Count  |  Total | 
| **Sí**  | No |  Microsoft.Storage/storageAccounts/tableServices  |  Disponibilidad  |  Disponibilidad  |  Percent  |  Average | 
| **Sí**  | No |  Microsoft.Storage/storageAccounts/tableServices  |  Salida  |  Salida  |  Bytes  |  Total | 
| **Sí**  | No |  Microsoft.Storage/storageAccounts/tableServices  |  Entrada  |  Entrada  |  Bytes  |  Total | 
| **Sí**  | No |  Microsoft.Storage/storageAccounts/tableServices  |  SuccessE2ELatency  |  Latencia de E2E correcta  |  Milisegundos  |  Average | 
| **Sí**  | No |  Microsoft.Storage/storageAccounts/tableServices  |  SuccessServerLatency  |  Latencia del servidor correcta  |  Milisegundos  |  Average | 
| **Sí**  | No |  Microsoft.Storage/storageAccounts/tableServices  |  TableCapacity  |  Capacidad de Table  |  Bytes  |  Average | 
| **Sí**  | No |  Microsoft.Storage/storageAccounts/tableServices  |  TableCount  |  Recuento de tablas  |  Count  |  Average | 
| **Sí**  | No |  Microsoft.Storage/storageAccounts/tableServices  |  TableEntityCount  |  Recuento de entidades de Table  |  Count  |  Average | 
| **Sí**  | No |  Microsoft.Storage/storageAccounts/tableServices  |  Transacciones  |  Transacciones  |  Count  |  Total | 
| **Sí**  | No |  Microsoft.StorageCache/caches  |  ClientIOPS  |  Total Client IOPS (Total de IOPS de cliente)  |  Count  |  Average | 
| **Sí**  | No |  Microsoft.StorageCache/caches  |  ClientLatency  |  Average Client Latency (Latencia media de cliente)  |  Milisegundos  |  Average | 
| **Sí**  | No |  Microsoft.StorageCache/caches  |  ClientLockIOPS  |  Client Lock IOPS (IOPS de bloqueo de cliente)  |  CountPerSecond  |  Average | 
| **Sí**  | No |  Microsoft.StorageCache/caches  |  ClientMetadataReadIOPS  |  Client Metadata Read IOPS (IOPS de lectura de metadatos de cliente)  |  CountPerSecond  |  Average | 
| **Sí**  | No |  Microsoft.StorageCache/caches  |  ClientMetadataWriteIOPS  |  Client Metadata Write IOPS (IOPS de escritura de metadatos de cliente)  |  CountPerSecond  |  Average | 
| **Sí**  | No |  Microsoft.StorageCache/caches  |  ClientReadIOPS  |  Client Read IOPS (IOPS de lectura de cliente)  |  CountPerSecond  |  Average | 
| **Sí**  | No |  Microsoft.StorageCache/caches  |  ClientReadThroughput  |  Average Cache Read Throughput (Rendimiento medio de lectura de caché)  |  BytesPerSecond  |  Average | 
| **Sí**  | No |  Microsoft.StorageCache/caches  |  ClientWriteIOPS  |  Client Write IOPS (IOPS de escritura de cliente)  |  CountPerSecond  |  Average | 
| **Sí**  | No |  Microsoft.StorageCache/caches  |  ClientWriteThroughput  |  Average Cache Write Throughput (Rendimiento medio de escritura de caché)  |  BytesPerSecond  |  Average | 
| **Sí**  | No |  Microsoft.StorageCache/caches  |  StorageTargetAsyncWriteThroughput  |  StorageTarget Asynchronous Write Throughput (Rendimiento de escritura asincrónica de StorageTarget)  |  BytesPerSecond  |  Average | 
| **Sí**  | No |  Microsoft.StorageCache/caches  |  StorageTargetFillThroughput  |  StorageTarget Fill Throughput (Rendimiento de relleno de StorageTarget)  |  BytesPerSecond  |  Average | 
| **Sí**  | No |  Microsoft.StorageCache/caches  |  StorageTargetHealth  |  Storage Target Health (Estado del destino de almacenamiento)  |  Count  |  Average | 
| **Sí**  | No |  Microsoft.StorageCache/caches  |  StorageTargetIOPS  |  Total StorageTarget IOPS (IOPS total de StorageTarget)  |  Count  |  Average | 
| **Sí**  | No |  Microsoft.StorageCache/caches  |  StorageTargetLatency  |  StorageTarget Latency (Latencia de StorageTarget)  |  Milisegundos  |  Average | 
| **Sí**  | No |  Microsoft.StorageCache/caches  |  StorageTargetMetadataReadIOPS  |  StorageTarget Metadata Read IOPS (IOPS de lectura de metadatos de StorageTarget)  |  CountPerSecond  |  Average | 
| **Sí**  | No |  Microsoft.StorageCache/caches  |  StorageTargetMetadataWriteIOPS  |  StorageTarget Metadata Write IOPS (IOPS de escritura de metadatos de StorageTarget)  |  CountPerSecond  |  Average | 
| **Sí**  | No |  Microsoft.StorageCache/caches  |  StorageTargetReadAheadThroughput  |  StorageTarget Read Ahead Throughput (Rendimiento de lectura anticipada de StorageTarget)  |  BytesPerSecond  |  Average | 
| **Sí**  | No |  Microsoft.StorageCache/caches  |  StorageTargetReadIOPS  |  StorageTarget Read IOPS (IOPS de lectura de StorageTarget)  |  CountPerSecond  |  Average | 
| **Sí**  | No |  Microsoft.StorageCache/caches  |  StorageTargetSyncWriteThroughput  |  StorageTarget Synchronous Write Throughput (Rendimiento de escritura sincrónica de StorageTarget)  |  BytesPerSecond  |  Average | 
| **Sí**  | No |  Microsoft.StorageCache/caches  |  StorageTargetTotalReadThroughput  |  StorageTarget Total Read Throughput (Rendimiento de lectura total de StorageTarget)  |  BytesPerSecond  |  Average | 
| **Sí**  | No |  Microsoft.StorageCache/caches  |  StorageTargetTotalWriteThroughput  |  StorageTarget Total Write Throughput (Rendimiento de escritura total de StorageTarget)  |  BytesPerSecond  |  Average | 
| **Sí**  | No |  Microsoft.StorageCache/caches  |  StorageTargetWriteIOPS  |  StorageTarget Write IOPS (IOPS de escritura de StorageTarget)  |  Count  |  Average | 
| **Sí**  | No |  Microsoft.StorageCache/caches  |  Tiempo de actividad  |  Tiempo de actividad  |  Count  |  Average | 
| **Sí**  | No |  microsoft.storagesync/storageSyncServices  |  ServerSyncSessionResult  |  Resultado de la sesión de sincronización  |  Count  |  Average | 
| **Sí**  | No |  microsoft.storagesync/storageSyncServices  |  StorageSyncBatchTransferredFileBytes  |  Bytes sincronizados  |  Bytes  |  Total | 
| **Sí**  | No |  microsoft.storagesync/storageSyncServices  |  StorageSyncRecalledNetworkBytesByApplication  |  Tamaño de recuperación de nube por niveles por aplicación  |  Bytes  |  Total | 
| **Sí**  | No |  microsoft.storagesync/storageSyncServices  |  StorageSyncRecalledTotalNetworkBytes  |  Tamaño de recuperación de nube por niveles  |  Bytes  |  Total | 
| **Sí**  | No |  microsoft.storagesync/storageSyncServices  |  StorageSyncRecallIOTotalSizeBytes  |  Recuperación de niveles de la nube  |  Bytes  |  Total | 
| **Sí**  | No |  microsoft.storagesync/storageSyncServices  |  StorageSyncRecallThroughputBytesPerSecond  |  Rendimiento de recuperación de nube por niveles  |  BytesPerSecond  |  Average | 
| **Sí**  | No |  microsoft.storagesync/storageSyncServices  |  StorageSyncServerHeartbeat  |  Estado en línea del servidor  |  Count  |  Máxima | 
| **Sí**  | No |  microsoft.storagesync/storageSyncServices  |  StorageSyncSyncSessionAppliedFilesCount  |  Archivos sincronizados  |  Count  |  Total | 
| **Sí**  | No |  microsoft.storagesync/storageSyncServices  |  StorageSyncSyncSessionPerItemErrorsCount  |  Archivos que no se están sincronizando  |  Count  |  Total | 
| **Sí**  | No |  microsoft.storagesync/storageSyncServices/registeredServers  |  ServerHeartbeat  |  Estado en línea del servidor  |  Count  |  Máxima | 
| **Sí**  | No |  microsoft.storagesync/storageSyncServices/registeredServers  |  ServerRecallIOTotalSizeBytes  |  Recuperación de niveles de la nube  |  Bytes  |  Total | 
| **Sí**  | No |  microsoft.storagesync/storageSyncServices/syncGroups  |  SyncGroupBatchTransferredFileBytes  |  Bytes sincronizados  |  Bytes  |  Total | 
| **Sí**  | No |  microsoft.storagesync/storageSyncServices/syncGroups  |  SyncGroupSyncSessionAppliedFilesCount  |  Archivos sincronizados  |  Count  |  Total | 
| **Sí**  | No |  microsoft.storagesync/storageSyncServices/syncGroups  |  SyncGroupSyncSessionPerItemErrorsCount  |  Archivos que no se están sincronizando  |  Count  |  Total | 
| **Sí**  | No |  microsoft.storagesync/storageSyncServices/syncGroups/serverEndpoints  |  ServerEndpointBatchTransferredFileBytes  |  Bytes sincronizados  |  Bytes  |  Total | 
| **Sí**  | No |  microsoft.storagesync/storageSyncServices/syncGroups/serverEndpoints  |  ServerEndpointSyncSessionAppliedFilesCount  |  Archivos sincronizados  |  Count  |  Total | 
| **Sí**  | No |  microsoft.storagesync/storageSyncServices/syncGroups/serverEndpoints  |  ServerEndpointSyncSessionPerItemErrorsCount  |  Archivos que no se están sincronizando  |  Count  |  Total | 
| **Sí**  | No |  Microsoft.StreamAnalytics/streamingjobs  |  AMLCalloutFailedRequests  |  Solicitudes de función con errores  |  Count  |  Total | 
| **Sí**  | No |  Microsoft.StreamAnalytics/streamingjobs  |  AMLCalloutInputEvents  |  Eventos de función  |  Count  |  Total | 
| **Sí**  | No |  Microsoft.StreamAnalytics/streamingjobs  |  AMLCalloutRequests  |  Solicitudes de función  |  Count  |  Total | 
| **Sí**  | No |  Microsoft.StreamAnalytics/streamingjobs  |  ConversionErrors  |  Errores de conversión de datos  |  Count  |  Total | 
| **Sí**  | No |  Microsoft.StreamAnalytics/streamingjobs  |  DeserializationError  |  Errores de deserialización de entrada  |  Count  |  Total | 
| **Sí**  | No |  Microsoft.StreamAnalytics/streamingjobs  |  DroppedOrAdjustedEvents  |  Eventos de fuera de orden  |  Count  |  Total | 
| **Sí**  | No |  Microsoft.StreamAnalytics/streamingjobs  |  EarlyInputEvents  |  Primeros eventos de entrada  |  Count  |  Total | 
| **Sí**  | No |  Microsoft.StreamAnalytics/streamingjobs  |  Errors  |  Errores de tiempo de ejecución  |  Count  |  Total | 
| **Sí**  | No |  Microsoft.StreamAnalytics/streamingjobs  |  InputEventBytes  |  Bytes del evento de entrada  |  Bytes  |  Total | 
| **Sí**  | No |  Microsoft.StreamAnalytics/streamingjobs  |  InputEvents  |  Eventos de entrada  |  Count  |  Total | 
| **Sí**  | No |  Microsoft.StreamAnalytics/streamingjobs  |  InputEventsSourcesBacklogged  |  Eventos de entrada pendientes  |  Count  |  Máxima | 
| **Sí**  | No |  Microsoft.StreamAnalytics/streamingjobs  |  InputEventsSourcesPerSecond  |  Orígenes de entrada recibidos  |  Count  |  Total | 
| **Sí**  | No |  Microsoft.StreamAnalytics/streamingjobs  |  LateInputEvents  |  Eventos de entrada retrasada  |  Count  |  Total | 
| **Sí**  | No |  Microsoft.StreamAnalytics/streamingjobs  |  OutputEvents  |  Eventos de salida  |  Count  |  Total | 
| **Sí**  | No |  Microsoft.StreamAnalytics/streamingjobs  |  OutputWatermarkDelaySeconds  |  Retraso de la marca de agua  |  Segundos  |  Máxima | 
| **Sí**  | No |  Microsoft.StreamAnalytics/streamingjobs  |  ResourceUtilization  |  SU % uso  |  Percent  |  Máxima | 
| **Sí**  | No |  Microsoft.VMwareCloudSimple/virtualMachines  |  Bytes de lectura de disco  |  Bytes de lectura de disco  |  Bytes  |  Total | 
| **Sí**  | No |  Microsoft.VMwareCloudSimple/virtualMachines  |  Operaciones de lectura de disco por segundo  |  Operaciones de lectura de disco por segundo  |  CountPerSecond  |  Average | 
| **Sí**  | No |  Microsoft.VMwareCloudSimple/virtualMachines  |  Bytes de escritura de disco  |  Bytes de escritura de disco  |  Bytes  |  Total | 
| **Sí**  | No |  Microsoft.VMwareCloudSimple/virtualMachines  |  Operaciones de escritura por segundo en disco  |  Operaciones de escritura por segundo en disco  |  CountPerSecond  |  Average | 
| **Sí**  | No |  Microsoft.VMwareCloudSimple/virtualMachines  |  DiskReadBytesPerSecond  |  Disk Read Bytes/Sec  |  BytesPerSecond  |  Average | 
| **Sí**  | No |  Microsoft.VMwareCloudSimple/virtualMachines  |  DiskReadLatency  |  Latencia de lectura en disco  |  Milisegundos  |  Average | 
| **Sí**  | No |  Microsoft.VMwareCloudSimple/virtualMachines  |  DiskReadOperations  |  Operaciones de lectura en disco  |  Count  |  Total | 
| **Sí**  | No |  Microsoft.VMwareCloudSimple/virtualMachines  |  DiskWriteBytesPerSecond  |  Disk Write Bytes/Sec  |  BytesPerSecond  |  Average | 
| **Sí**  | No |  Microsoft.VMwareCloudSimple/virtualMachines  |  DiskWriteLatency  |  Latencia de escritura en disco  |  Milisegundos  |  Average | 
| **Sí**  | No |  Microsoft.VMwareCloudSimple/virtualMachines  |  DiskWriteOperations  |  Operaciones de escritura en disco  |  Count  |  Total | 
| **Sí**  | No |  Microsoft.VMwareCloudSimple/virtualMachines  |  MemoryActive  |  Memoria activa  |  Bytes  |  Average | 
| **Sí**  | No |  Microsoft.VMwareCloudSimple/virtualMachines  |  MemoryGranted  |  Memoria concedida  |  Bytes  |  Average | 
| **Sí**  | No |  Microsoft.VMwareCloudSimple/virtualMachines  |  MemoryUsed  |  Memoria usada  |  Bytes  |  Average | 
| **Sí**  | No |  Microsoft.VMwareCloudSimple/virtualMachines  |  Red interna  |  Red interna  |  Bytes  |  Total | 
| **Sí**  | No |  Microsoft.VMwareCloudSimple/virtualMachines  |  Red interna  |  Red interna  |  Bytes  |  Total | 
| **Sí**  | No |  Microsoft.VMwareCloudSimple/virtualMachines  |  NetworkInBytesPerSecond  |  Red en bytes/seg  |  BytesPerSecond  |  Average | 
| **Sí**  | No |  Microsoft.VMwareCloudSimple/virtualMachines  |  NetworkOutBytesPerSecond  |  Bytes/seg de red externa  |  BytesPerSecond  |  Average | 
| **Sí**  | No |  Microsoft.VMwareCloudSimple/virtualMachines  |  Porcentaje de CPU  |  Porcentaje de CPU  |  Percent  |  Average | 
| **Sí**  | No |  Microsoft.VMwareCloudSimple/virtualMachines  |  PercentageCpuReady  |  Preparación de la CPU en porcentaje  |  Milisegundos  |  Total | 
| **Sí**  | **Sí** |  Microsoft.Web/hostingEnvironments/multiRolePools  |  ActiveRequests  |  Solicitudes activas  |  Count  |  Total | 
| **Sí**  | **Sí** |  Microsoft.Web/hostingEnvironments/multiRolePools  |  AverageResponseTime  |  Tiempo de respuesta promedio  |  Segundos  |  Average | 
| **Sí**  | **Sí** |  Microsoft.Web/hostingEnvironments/multiRolePools  |  BytesReceived  |  Entrada de datos  |  Bytes  |  Total | 
| **Sí**  | **Sí** |  Microsoft.Web/hostingEnvironments/multiRolePools  |  BytesSent  |  Salida de datos  |  Bytes  |  Total | 
| **Sí**  | **Sí** |  Microsoft.Web/hostingEnvironments/multiRolePools  |  CpuPercentage  |  Porcentaje de CPU  |  Percent  |  Average | 
| **Sí**  | **Sí** |  Microsoft.Web/hostingEnvironments/multiRolePools  |  DiskQueueLength  |  Longitud de la cola de disco  |  Count  |  Average | 
| **Sí**  | **Sí** |  Microsoft.Web/hostingEnvironments/multiRolePools  |  Http101  |  Http 101  |  Count  |  Total | 
| **Sí**  | **Sí** |  Microsoft.Web/hostingEnvironments/multiRolePools  |  Http2xx  |  Http 2xx  |  Count  |  Total | 
| **Sí**  | **Sí** |  Microsoft.Web/hostingEnvironments/multiRolePools  |  Http3xx  |  Http 3xx  |  Count  |  Total | 
| **Sí**  | **Sí** |  Microsoft.Web/hostingEnvironments/multiRolePools  |  Http401  |  Http 401  |  Count  |  Total | 
| **Sí**  | **Sí** |  Microsoft.Web/hostingEnvironments/multiRolePools  |  Http403  |  Http 403  |  Count  |  Total | 
| **Sí**  | **Sí** |  Microsoft.Web/hostingEnvironments/multiRolePools  |  Http404  |  Http 404  |  Count  |  Total | 
| **Sí**  | **Sí** |  Microsoft.Web/hostingEnvironments/multiRolePools  |  Http406  |  Http 406  |  Count  |  Total | 
| **Sí**  | **Sí** |  Microsoft.Web/hostingEnvironments/multiRolePools  |  Http4xx  |  Http 4xx  |  Count  |  Total | 
| **Sí**  | **Sí** |  Microsoft.Web/hostingEnvironments/multiRolePools  |  Http5xx  |  Errores de servidor HTTP  |  Count  |  Total | 
| **Sí**  | **Sí** |  Microsoft.Web/hostingEnvironments/multiRolePools  |  HttpQueueLength  |  Longitud de la cola HTTP  |  Count  |  Average | 
| **Sí**  | **Sí** |  Microsoft.Web/hostingEnvironments/multiRolePools  |  LargeAppServicePlanInstances  |  Trabajos grandes del plan de App Service  |  Count  |  Average | 
| **Sí**  | **Sí** |  Microsoft.Web/hostingEnvironments/multiRolePools  |  MediumAppServicePlanInstances  |  Trabajos medianos del plan de App Service  |  Count  |  Average | 
| **Sí**  | **Sí** |  Microsoft.Web/hostingEnvironments/multiRolePools  |  MemoryPercentage  |  Porcentaje de memoria  |  Percent  |  Average | 
| **Sí**  | **Sí** |  Microsoft.Web/hostingEnvironments/multiRolePools  |  Requests  |  Requests  |  Count  |  Total | 
| **Sí**  | **Sí** |  Microsoft.Web/hostingEnvironments/multiRolePools  |  SmallAppServicePlanInstances  |  Trabajos pequeños del plan de App Service  |  Count  |  Average | 
| **Sí**  | **Sí** |  Microsoft.Web/hostingEnvironments/multiRolePools  |  TotalFrontEnds  |  Número total de front-ends  |  Count  |  Average | 
| **Sí**  | **Sí** |  Microsoft.Web/hostingEnvironments/workerPools  |  CpuPercentage  |  Porcentaje de CPU  |  Percent  |  Average | 
| **Sí**  | **Sí** |  Microsoft.Web/hostingEnvironments/workerPools  |  MemoryPercentage  |  Porcentaje de memoria  |  Percent  |  Average | 
| **Sí**  | **Sí** |  Microsoft.Web/hostingEnvironments/workerPools  |  WorkersAvailable  |  Trabajos disponibles  |  Count  |  Average | 
| **Sí**  | **Sí** |  Microsoft.Web/hostingEnvironments/workerPools  |  WorkersTotal  |  Número total de trabajos  |  Count  |  Average | 
| **Sí**  | **Sí** |  Microsoft.Web/hostingEnvironments/workerPools  |  WorkersUsed  |  Trabajos usados  |  Count  |  Average | 
| **Sí**  | **Sí** |  Microsoft.Web/serverfarms  |  BytesReceived  |  Entrada de datos  |  Bytes  |  Total | 
| **Sí**  | **Sí** |  Microsoft.Web/serverfarms  |  BytesSent  |  Salida de datos  |  Bytes  |  Total | 
| **Sí**  | **Sí** |  Microsoft.Web/serverfarms  |  CpuPercentage  |  Porcentaje de CPU  |  Percent  |  Average | 
| **Sí**  | **Sí** |  Microsoft.Web/serverfarms  |  DiskQueueLength  |  Longitud de la cola de disco  |  Count  |  Average | 
| **Sí**  | **Sí** |  Microsoft.Web/serverfarms  |  HttpQueueLength  |  Longitud de la cola HTTP  |  Count  |  Average | 
| **Sí**  | **Sí** |  Microsoft.Web/serverfarms  |  MemoryPercentage  |  Porcentaje de memoria  |  Percent  |  Average | 
| **Sí**  | **Sí** |  Microsoft.Web/serverfarms  |  TcpCloseWait  |  TCP Close Wait  |  Count  |  Average | 
| **Sí**  | **Sí** |  Microsoft.Web/serverfarms  |  TcpClosing  |  TCP Closing (Cierre de TCP)  |  Count  |  Average | 
| **Sí**  | **Sí** |  Microsoft.Web/serverfarms  |  TcpEstablished  |  TCP Established  |  Count  |  Average | 
| **Sí**  | **Sí** |  Microsoft.Web/serverfarms  |  TcpFinWait1  |  TCP Fin Wait 1  |  Count  |  Average | 
| **Sí**  | **Sí** |  Microsoft.Web/serverfarms  |  TcpFinWait2  |  TCP Fin Wait 2  |  Count  |  Average | 
| **Sí**  | **Sí** |  Microsoft.Web/serverfarms  |  TcpLastAck  |  TCP Last Ack  |  Count  |  Average | 
| **Sí**  | **Sí** |  Microsoft.Web/serverfarms  |  TcpSynReceived  |  TCP Syn Received  |  Count  |  Average | 
| **Sí**  | **Sí** |  Microsoft.Web/serverfarms  |  TcpSynSent  |  TCP Syn Sent  |  Count  |  Average | 
| **Sí**  | **Sí** |  Microsoft.Web/serverfarms  |  TcpTimeWait  |  TCP Time Wait  |  Count  |  Average | 
| **Sí**  | **Sí** |  Microsoft.Web/sites  |  AppConnections  |  Conexiones  |  Count  |  Average | 
| **Sí**  | **Sí** |  Microsoft.Web/sites  |  AverageMemoryWorkingSet  |  Espacio de trabajo de memoria promedio  |  Bytes  |  Average | 
| **Sí**  | **Sí** |  Microsoft.Web/sites  |  AverageResponseTime  |  Tiempo de respuesta promedio  |  Segundos  |  Average | 
| **Sí**  | **Sí** |  Microsoft.Web/sites  |  BytesReceived  |  Entrada de datos  |  Bytes  |  Total | 
| **Sí**  | **Sí** |  Microsoft.Web/sites  |  BytesSent  |  Salida de datos  |  Bytes  |  Total | 
| **Sí**  | **Sí** |  Microsoft.Web/sites  |  CpuTime  |  Tiempo de CPU  |  Segundos  |  Total | 
| **Sí**  | **Sí** |  Microsoft.Web/sites  |  CurrentAssemblies  |  Ensamblados actuales  |  Count  |  Average | 
| **Sí**  | **Sí** |  Microsoft.Web/sites  |  FunctionExecutionCount  |  Recuento de ejecución de funciones  |  Count  |  Total | 
| **Sí**  | **Sí** |  Microsoft.Web/sites  |  FunctionExecutionUnits  |  Unidades de ejecución de función  |  Count  |  Total | 
| **Sí**  | **Sí** |  Microsoft.Web/sites  |  Gen0Collections  |  Recolección de elementos no utilizados de Gen 0  |  Count  |  Total | 
| **Sí**  | **Sí** |  Microsoft.Web/sites  |  Gen1Collections  |  Recolección de elementos no utilizados de Gen 1  |  Count  |  Total | 
| **Sí**  | **Sí** |  Microsoft.Web/sites  |  Gen2Collections  |  Recolección de elementos no utilizados de Gen 2  |  Count  |  Total | 
| **Sí**  | **Sí** |  Microsoft.Web/sites  |  Asas  |  Recuento de identificadores  |  Count  |  Average | 
| **Sí**  | **Sí** |  Microsoft.Web/sites  |  HealthCheckStatus  |  Estado de comprobación de estado  |  Count  |  Average | 
| **Sí**  | **Sí** |  Microsoft.Web/sites  |  Http101  |  Http 101  |  Count  |  Total | 
| **Sí**  | **Sí** |  Microsoft.Web/sites  |  Http2xx  |  Http 2xx  |  Count  |  Total | 
| **Sí**  | **Sí** |  Microsoft.Web/sites  |  Http3xx  |  Http 3xx  |  Count  |  Total | 
| **Sí**  | **Sí** |  Microsoft.Web/sites  |  Http401  |  Http 401  |  Count  |  Total | 
| **Sí**  | **Sí** |  Microsoft.Web/sites  |  Http403  |  Http 403  |  Count  |  Total | 
| **Sí**  | **Sí** |  Microsoft.Web/sites  |  Http404  |  Http 404  |  Count  |  Total | 
| **Sí**  | **Sí** |  Microsoft.Web/sites  |  Http406  |  Http 406  |  Count  |  Total | 
| **Sí**  | **Sí** |  Microsoft.Web/sites  |  Http4xx  |  Http 4xx  |  Count  |  Total | 
| **Sí**  | **Sí** |  Microsoft.Web/sites  |  Http5xx  |  Errores de servidor HTTP  |  Count  |  Total | 
| **Sí**  | **Sí** |  Microsoft.Web/sites  |  HttpResponseTime  |  Tiempo de respuesta  |  Segundos  |  Average | 
| **Sí**  | **Sí** |  Microsoft.Web/sites  |  IoOtherBytesPerSecond  |  Otros bytes de E/S por segundo  |  BytesPerSecond  |  Total | 
| **Sí**  | **Sí** |  Microsoft.Web/sites  |  IoOtherOperationsPerSecond  |  Otras operaciones de E/S por segundo  |  BytesPerSecond  |  Total | 
| **Sí**  | **Sí** |  Microsoft.Web/sites  |  IoReadBytesPerSecond  |  Bytes de lectura de E/S por segundo  |  BytesPerSecond  |  Total | 
| **Sí**  | **Sí** |  Microsoft.Web/sites  |  IoReadOperationsPerSecond  |  Operaciones de lectura de E/S por segundo  |  BytesPerSecond  |  Total | 
| **Sí**  | **Sí** |  Microsoft.Web/sites  |  IoReadBytesPerSecond  |  Bytes de escritura de E/S por segundo  |  BytesPerSecond  |  Total | 
| **Sí**  | **Sí** |  Microsoft.Web/sites  |  IoWriteOperationsPerSecond  |  Operaciones de escritura de E/S por segundo  |  BytesPerSecond  |  Total | 
| **Sí**  | **Sí** |  Microsoft.Web/sites  |  MemoryWorkingSet  |  Espacio de trabajo de memoria  |  Bytes  |  Average | 
| **Sí**  | **Sí** |  Microsoft.Web/sites  |  PrivateBytes  |  Bytes privados  |  Bytes  |  Average | 
| **Sí**  | **Sí** |  Microsoft.Web/sites  |  Requests  |  Requests  |  Count  |  Total | 
| **Sí**  | **Sí** |  Microsoft.Web/sites  |  RequestsInApplicationQueue  |  Solicitudes en cola de la aplicación  |  Count  |  Average | 
| **Sí**  | **Sí** |  Microsoft.Web/sites  |  Subprocesos  |  Número de subprocesos  |  Count  |  Average | 
| **Sí**  | **Sí** |  Microsoft.Web/sites  |  TotalAppDomains  |  Dominios de aplicación totales  |  Count  |  Average | 
| **Sí**  | **Sí** |  Microsoft.Web/sites  |  TotalAppDomainsUnloaded  |  Dominios de aplicación totales descargados  |  Count  |  Average | 
| **Sí**  | **Sí** |  Microsoft.Web/sites/slots  |  AppConnections  |  Conexiones  |  Count  |  Average | 
| **Sí**  | **Sí** |  Microsoft.Web/sites/slots  |  AverageMemoryWorkingSet  |  Espacio de trabajo de memoria promedio  |  Bytes  |  Average | 
| **Sí**  | **Sí** |  Microsoft.Web/sites/slots  |  AverageResponseTime  |  Tiempo de respuesta promedio  |  Segundos  |  Average | 
| **Sí**  | **Sí** |  Microsoft.Web/sites/slots  |  BytesReceived  |  Entrada de datos  |  Bytes  |  Total | 
| **Sí**  | **Sí** |  Microsoft.Web/sites/slots  |  BytesSent  |  Salida de datos  |  Bytes  |  Total | 
| **Sí**  | **Sí** |  Microsoft.Web/sites/slots  |  CpuTime  |  Tiempo de CPU  |  Segundos  |  Total | 
| **Sí**  | **Sí** |  Microsoft.Web/sites/slots  |  CurrentAssemblies  |  Ensamblados actuales  |  Count  |  Average | 
| **Sí**  | **Sí** |  Microsoft.Web/sites/slots  |  FunctionExecutionCount  |  Recuento de ejecución de funciones  |  Count  |  Total | 
| **Sí**  | **Sí** |  Microsoft.Web/sites/slots  |  FunctionExecutionUnits  |  Unidades de ejecución de función  |  Count  |  Total | 
| **Sí**  | **Sí** |  Microsoft.Web/sites/slots  |  Gen0Collections  |  Recolección de elementos no utilizados de Gen 0  |  Count  |  Total | 
| **Sí**  | **Sí** |  Microsoft.Web/sites/slots  |  Gen1Collections  |  Recolección de elementos no utilizados de Gen 1  |  Count  |  Total | 
| **Sí**  | **Sí** |  Microsoft.Web/sites/slots  |  Gen2Collections  |  Recolección de elementos no utilizados de Gen 2  |  Count  |  Total | 
| **Sí**  | **Sí** |  Microsoft.Web/sites/slots  |  Asas  |  Recuento de identificadores  |  Count  |  Average | 
| **Sí**  | **Sí** |  Microsoft.Web/sites/slots  |  HealthCheckStatus  |  Estado de comprobación de estado  |  Count  |  Average | 
| **Sí**  | **Sí** |  Microsoft.Web/sites/slots  |  Http101  |  Http 101  |  Count  |  Total | 
| **Sí**  | **Sí** |  Microsoft.Web/sites/slots  |  Http2xx  |  Http 2xx  |  Count  |  Total | 
| **Sí**  | **Sí** |  Microsoft.Web/sites/slots  |  Http3xx  |  Http 3xx  |  Count  |  Total | 
| **Sí**  | **Sí** |  Microsoft.Web/sites/slots  |  Http401  |  Http 401  |  Count  |  Total | 
| **Sí**  | **Sí** |  Microsoft.Web/sites/slots  |  Http403  |  Http 403  |  Count  |  Total | 
| **Sí**  | **Sí** |  Microsoft.Web/sites/slots  |  Http404  |  Http 404  |  Count  |  Total | 
| **Sí**  | **Sí** |  Microsoft.Web/sites/slots  |  Http406  |  Http 406  |  Count  |  Total | 
| **Sí**  | **Sí** |  Microsoft.Web/sites/slots  |  Http4xx  |  Http 4xx  |  Count  |  Total | 
| **Sí**  | **Sí** |  Microsoft.Web/sites/slots  |  Http5xx  |  Errores de servidor HTTP  |  Count  |  Total | 
| **Sí**  | **Sí** |  Microsoft.Web/sites/slots  |  HttpResponseTime  |  Tiempo de respuesta  |  Segundos  |  Average | 
| **Sí**  | **Sí** |  Microsoft.Web/sites/slots  |  IoOtherBytesPerSecond  |  Otros bytes de E/S por segundo  |  BytesPerSecond  |  Total | 
| **Sí**  | **Sí** |  Microsoft.Web/sites/slots  |  IoOtherOperationsPerSecond  |  Otras operaciones de E/S por segundo  |  BytesPerSecond  |  Total | 
| **Sí**  | **Sí** |  Microsoft.Web/sites/slots  |  IoReadBytesPerSecond  |  Bytes de lectura de E/S por segundo  |  BytesPerSecond  |  Total | 
| **Sí**  | **Sí** |  Microsoft.Web/sites/slots  |  IoReadOperationsPerSecond  |  Operaciones de lectura de E/S por segundo  |  BytesPerSecond  |  Total | 
| **Sí**  | **Sí** |  Microsoft.Web/sites/slots  |  IoReadBytesPerSecond  |  Bytes de escritura de E/S por segundo  |  BytesPerSecond  |  Total | 
| **Sí**  | **Sí** |  Microsoft.Web/sites/slots  |  IoWriteOperationsPerSecond  |  Operaciones de escritura de E/S por segundo  |  BytesPerSecond  |  Total | 
| **Sí**  | **Sí** |  Microsoft.Web/sites/slots  |  MemoryWorkingSet  |  Espacio de trabajo de memoria  |  Bytes  |  Average | 
| **Sí**  | **Sí** |  Microsoft.Web/sites/slots  |  PrivateBytes  |  Bytes privados  |  Bytes  |  Average | 
| **Sí**  | **Sí** |  Microsoft.Web/sites/slots  |  Requests  |  Requests  |  Count  |  Total | 
| **Sí**  | **Sí** |  Microsoft.Web/sites/slots  |  RequestsInApplicationQueue  |  Solicitudes en cola de la aplicación  |  Count  |  Average | 
| **Sí**  | **Sí** |  Microsoft.Web/sites/slots  |  Subprocesos  |  Número de subprocesos  |  Count  |  Average | 
| **Sí**  | **Sí** |  Microsoft.Web/sites/slots  |  TotalAppDomains  |  Dominios de aplicación totales  |  Count  |  Average | 
| **Sí**  | **Sí** |  Microsoft.Web/sites/slots  |  TotalAppDomainsUnloaded  |  Dominios de aplicación totales descargados  |  Count  |  Average | 
