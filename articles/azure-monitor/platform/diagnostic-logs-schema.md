---
title: Servicios y esquemas admitidos de los registros de recursos de Azure
description: Conozca el esquema de los eventos y servicios admitidos para los registros de recurso de Azure.
ms.subservice: logs
ms.topic: reference
ms.date: 10/22/2019
ms.openlocfilehash: de102c5dc4104aafc44b87b14aeea0b30cb7c083
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "79226344"
---
# <a name="supported-services-schemas-and-categories-for-azure-resource-logs"></a>Servicios, esquemas y categorías admitidos en los registros de recursos de Azure

> [!NOTE]
> Los registros de recurso se conocían anteriormente como registros de diagnóstico.

Los [registros de recursos de Azure Monitor](../../azure-monitor/platform/platform-logs-overview.md) son los registros emitidos por los servicios de Azure que describen el funcionamiento de dichos servicios o recursos. Todos los registros de recursos disponibles a través de Azure Monitor comparten un esquema común de nivel superior, con flexibilidad para que cada servicio emita propiedades únicas para sus propios eventos.

Una combinación del tipo de recurso (disponible en la propiedad `resourceId`) y la `category` identifica un esquema de forma única. En este artículo se describe el esquema de nivel superior para los registros de recursos y los vínculos a los esquemas para cada servicio.

## <a name="top-level-resource-logs-schema"></a>Esquema de registros de recursos de nivel superior

| Nombre | Obligatorio/opcional | Descripción |
|---|---|---|
| time | Obligatorio | Marca de tiempo (UTC) del evento. |
| resourceId | Obligatorio | Identificador del recurso que ha emitido el evento. Para los servicios de inquilino, presenta la forma /tenants/tenant-id/providers/provider-name. |
| tenantId | Necesario para los registros de inquilinos | El identificador del inquilino de Active Directory al que está asociado este evento. Esta propiedad solo se usa para los registros de nivel de inquilino; no aparece en los registros de nivel de recurso. |
| operationName | Obligatorio | Nombre de la operación representada por este evento. Si el evento representa una operación de RBAC, este es el nombre de la operación de RBAC (p. ej. Microsoft.Storage/storageAccounts/blobServices/blobs/Read). Se suele modelar con la forma de una operación de Resource Manager, incluso si no son operaciones de Resource Manager documentadas reales (`Microsoft.<providerName>/<resourceType>/<subtype>/<Write/Read/Delete/Action>`) |
| operationVersion | Opcional | La versión de API asociada con la operación, si operationName se ha realizado mediante una API (p. ej. `http://myservice.windowsazure.net/object?api-version=2016-06-01`). Si no hay ninguna API que se corresponde con esta operación, la versión representa la versión de esa operación en caso de que las propiedades asociadas con la operación cambien en el futuro. |
| category | Obligatorio | Categoría de registro del evento. La categoría es la granularidad con la que se pueden habilitar o deshabilitar los registros en un recurso determinado. Las propiedades que aparecen en el blob de propiedades de un evento son las mismas dentro de una categoría de registro y un tipo de recurso concretos. Las categorías de registro típicas son "Audit", "Operational" "Execution" y "Request". |
| resultType | Opcional | Estado del evento. Entre los valores habituales, se incluyen Started, In Progress, Succeeded, Failed, Active y Resolved. |
| resultSignature | Opcional | Subestado del evento. Si esta operación se corresponde con una llamada API REST, este es el código de estado HTTP de la llamada REST correspondiente. |
| resultDescription | Opcional | Descripción de texto estático de esta operación, por ejemplo. "Obtener el archivo de almacenamiento". |
| durationMs | Opcional | Duración de la operación en milisegundos. |
| callerIpAddress | Opcional | Dirección IP del autor de la llamada, si la operación se corresponde con una llamada API que podría proceder de una entidad con una dirección IP disponible públicamente. |
| correlationId | Opcional | GUID que se usa para agrupar un conjunto de eventos relacionados. Normalmente, si dos eventos tienen el mismo valor operationName pero dos estados diferentes (p. ej. "Started" y "Succeeded"), comparten el mismo identificador de correlación. Esto también puede representar otras relaciones entre los eventos. |
| identity | Opcional | Blob JSON que describe la identidad del usuario o la aplicación que realizó la operación. Normalmente esto incluirá la autorización y las notificaciones o el token JWT de Active Directory. |
| Nivel | Opcional | Nivel de gravedad del evento. Debe ser uno de entre Informativo, Advertencia, Error o Crítico. |
| ubicación | Opcional | Región del recurso que emite el evento, por ejemplo, "Este de EE. UU." o "Sur de Francia". |
| properties | Opcional | Todas las propiedades extendidas relacionadas con esta categoría de eventos determinada. Todas las propiedades personalizadas o únicas se deben colocar dentro de esta "Parte B" del esquema. |

## <a name="service-specific-schemas-for-resource-logs"></a>Esquemas específicos del servicio para los registros de recursos
El esquema para los registros de diagnóstico de recursos varía según la categoría de registro y el recurso. En esta lista se muestran todos los servicios que hacen que los registros de recursos y los vínculos estén disponibles para el servicio y el esquema específico de la categoría cuando sea posible.

| Servicio | Esquema y documentos |
| --- | --- |
| Azure Active Directory | [Información general](../../active-directory/reports-monitoring/concept-activity-logs-azure-monitor.md), [Esquema de registros de auditoría](../../active-directory/reports-monitoring/reference-azure-monitor-audit-log-schema.md) y [Esquema de inicios de sesión](../../active-directory/reports-monitoring/reference-azure-monitor-sign-ins-log-schema.md) |
| Analysis Services | https://azure.microsoft.com/blog/azure-analysis-services-integration-with-azure-diagnostic-logs/ |
| API Management | [Registros de recursos de API Management](../../api-management/api-management-howto-use-azure-monitor.md#diagnostic-logs) |
| Puertas de enlace de aplicaciones |[Registro para Application Gateway](../../application-gateway/application-gateway-diagnostics.md) |
| Azure Automation |[Log Analytics para Azure Automation](../../automation/automation-manage-send-joblogs-log-analytics.md) |
| Azure Batch |[Registro de Azure Batch](../../batch/batch-diagnostics.md) |
| Azure Database for MySQL | [Registros de diagnóstico de Azure Database for MySQL](../../mysql/concepts-server-logs.md#diagnostic-logs) |
| Azure Database for PostgreSQL | [Registros de Azure Database for PostgreSQL](../../postgresql/concepts-server-logs.md#diagnostic-logs) |
| Explorador de datos de Azure | [Registros de Azure Data Explorer](../../data-explorer/using-diagnostic-logs.md) |
| Cognitive Services | [Registro para Azure Cognitive Services](../../cognitive-services/diagnostic-logging.md) |
| Container Registry | [Registro para Azure Container Registry](../../container-registry/container-registry-diagnostics-audit-logs.md) |
| Content Delivery Network | [Registros de Azure para CDN](../../cdn/cdn-azure-diagnostic-logs.md) |
| CosmosDB | [Registro de Azure Cosmos DB](../../cosmos-db/logging.md) |
| Data Factory | [Supervisión de factorías de datos mediante Azure Monitor](../../data-factory/monitor-using-azure-monitor.md) |
| Data Lake Analytics |[Acceso a los registros para Azure Data Lake Analytics](../../data-lake-analytics/data-lake-analytics-diagnostic-logs.md) |
| Data Lake Store |[Acceso a los registros para Azure Data Lake Store](../../data-lake-store/data-lake-store-diagnostic-logs.md) |
| Event Hubs |[Registros de Azure Event Hubs](../../event-hubs/event-hubs-diagnostic-logs.md) |
| ExpressRoute | Esquema no disponible. |
| Azure Firewall | Esquema no disponible. |
| IoT Hub | [Operaciones de IoT Hub](../../iot-hub/iot-hub-monitor-resource-health.md#use-azure-monitor) |
| Key Vault |[Registro de Azure Key Vault](../../key-vault/key-vault-logging.md) |
| Kubernetes Service |[Registro de Kubernetes de Azure](../../aks/view-master-logs.md#log-event-schema) |
| Load Balancer |[Log Analytics para Azure Load Balancer](../../load-balancer/load-balancer-monitor-log.md) |
| Logic Apps |[Esquema de seguimiento personalizado de Logic Apps B2B](../../logic-apps/logic-apps-track-integration-account-custom-tracking-schema.md) |
| Grupos de seguridad de red |[Análisis del registro para grupos de seguridad de red (NSG)](../../virtual-network/virtual-network-nsg-manage-log.md) |
| DDOS Protection | [Administración de Azure DDoS Protection estándar](../../virtual-network/manage-ddos-protection.md) |
| Power BI dedicado | [Registro para Power BI Embedded en Azure](https://docs.microsoft.com/power-bi/developer/azure-pbie-diag-logs) |
| Recovery Services | [Modelo de datos para Azure Backup](../../backup/backup-azure-reports-data-model.md)|
| Search |[Habilitación y uso de Análisis de tráfico de búsqueda](../../search/search-traffic-analytics.md) |
| Azure Service Bus |[Registros de Azure Service Bus](../../service-bus-messaging/service-bus-diagnostic-logs.md) |
| SQL Database | [Registro de Azure SQL Database](../../sql-database/sql-database-metrics-diag-logging.md) |
| Stream Analytics |[Registros de trabajo](../../stream-analytics/stream-analytics-job-diagnostic-logs.md) |
| Traffic Manager | [Esquema de registro de Traffic Manager](../../traffic-manager/traffic-manager-diagnostic-logs.md) |
| Virtual Networks | Esquema no disponible. |
| Puertas de enlace de red virtual | Esquema no disponible. |

## <a name="supported-log-categories-per-resource-type"></a>Categorías de registro admitidas por tipo de recurso

Es posible que algunas categorías solo se admitan para tipos específicos de recursos. Esta es una lista de todos los que están disponibles de algún modo.  Por ejemplo, las categorías Microsoft.Sql/servers/databases no están disponibles para todos los tipos de bases de datos. Para más información, consulte la [información sobre el registro de diagnóstico de SQL Database](../../sql-database/sql-database-metrics-diag-logging.md). 

|Tipo de recurso|Category|Nombre para mostrar de categoría|
|---|---|---|
|Microsoft.AAD/domainServices|SystemSecurity|SystemSecurity|
|Microsoft.AAD/domainServices|AccountManagement|AccountManagement|
|Microsoft.AAD/domainServices|LogonLogoff|LogonLogoff|
|Microsoft.AAD/domainServices|ObjectAccess|ObjectAccess|
|Microsoft.AAD/domainServices|PolicyChange|PolicyChange|
|Microsoft.AAD/domainServices|PrivilegeUse|PrivilegeUse|
|Microsoft.AAD/domainServices|DetailTracking|DetailTracking|
|Microsoft.AAD/domainServices|DirectoryServiceAccess|DirectoryServiceAccess|
|Microsoft.AAD/domainServices|AccountLogon|AccountLogon|
|microsoft.aadiam/tenants|Inicio de sesión|Inicio de sesión|
|Microsoft.AnalysisServices/servers|Motor|Motor|
|Microsoft.AnalysisServices/servers|Servicio|Servicio|
|Microsoft.ApiManagement/service|GatewayLogs|Registros relacionados con la puerta de enlace de ApiManagement|
|Microsoft.AppPlatform/Spring|ApplicationConsole|Consola de aplicación|
|Microsoft.Automation/automationAccounts|JobLogs|Registros de trabajo|
|Microsoft.Automation/automationAccounts|JobStreams|Flujos de trabajo|
|Microsoft.Automation/automationAccounts|DscNodeStatus|Estado del nodo de DSC|
|Microsoft.Batch/batchAccounts|ServiceLog|Registros de servicios|
|Microsoft.BatchAI/workspaces|BaiClusterEvent|BaiClusterEvent|
|Microsoft.BatchAI/workspaces|BaiClusterNodeEvent|BaiClusterNodeEvent|
|Microsoft.BatchAI/workspaces|BaiJobEvent|BaiJobEvent|
|Microsoft.Blockchain/blockchainMembers|BlockchainApplication|Aplicación de cadena de bloques|
|Microsoft.Blockchain/blockchainMembers|Proxy|Proxy|
|Microsoft.Cdn/profiles/endpoints|CoreAnalytics|Obtiene las métricas del punto de conexión; por ejemplo, ancho de banda, salida, etc.|
|Microsoft.ClassicNetwork/networkSecurityGroups|Evento de flujo de reglas de grupo de seguridad de red|Evento de flujo de reglas de grupo de seguridad de red|
|Microsoft.CognitiveServices/accounts|Auditoría|Registros de auditoría|
|Microsoft.CognitiveServices/accounts|RequestResponse|Registros de solicitud y respuesta|
|Microsoft.ContainerRegistry/registries|ContainerRegistryRepositoryEvents|Registros de RepositoryEvent (versión preliminar)|
|Microsoft.ContainerRegistry/registries|ContainerRegistryLoginEvents|Eventos de inicio de sesión (versión preliminar)|
|Microsoft.ContainerService/managedClusters|kube-apiserver|Servidor de la API de Kubernetes|
|Microsoft.ContainerService/managedClusters|kube-controller-manager|Administrador del controlador de Kubernetes|
|Microsoft.ContainerService/managedClusters|kube-scheduler|Programador de Kubernetes|
|Microsoft.ContainerService/managedClusters|kube-audit|Auditoría de Kubernetes|
|Microsoft.ContainerService/managedClusters|cluster-autoscaler|Cluster Autoscaler de Kubernetes|
|Microsoft.Databricks/workspaces|dbfs|Sistema de archivos de Databricks|
|Microsoft.Databricks/workspaces|clusters|Clústeres de Databricks|
|Microsoft.Databricks/workspaces|accounts|Cuentas de Databricks|
|Microsoft.Databricks/workspaces|jobs|Trabajos de Databricks|
|Microsoft.Databricks/workspaces|notebook|Cuaderno de Databricks|
|Microsoft.Databricks/workspaces|ssh|SSH de Databricks|
|Microsoft.Databricks/workspaces|área de trabajo|Área de trabajo de Databricks|
|Microsoft.Databricks/workspaces|secrets|Secretos de Databricks|
|Microsoft.Databricks/workspaces|sqlPermissions|Permisos SQL de Databricks|
|Microsoft.Databricks/workspaces|instancePools|Grupos de instancias|
|Microsoft.DataCatalog/datacatalogs|ScanStatusLogEvent|ScanStatus|
|Microsoft.DataFactory/factories|ActivityRuns|Registro de ejecuciones de actividad de canalización|
|Microsoft.DataFactory/factories|PipelineRuns|Registro de ejecuciones de canalización|
|Microsoft.DataFactory/factories|TriggerRuns|Registro de ejecuciones de desencadenador|
|Microsoft.DataLakeAnalytics/accounts|Auditoría|Registros de auditoría|
|Microsoft.DataLakeAnalytics/accounts|Requests|Registros de solicitud|
|Microsoft.DataLakeStore/accounts|Auditoría|Registros de auditoría|
|Microsoft.DataLakeStore/accounts|Requests|Registros de solicitud|
|Microsoft.DataShare/accounts|Recursos compartidos|Recursos compartidos|
|Microsoft.DataShare/accounts|ShareSubscriptions|Suscripciones a recursos compartidos|
|Microsoft.DataShare/accounts|SentShareSnapshots|Instantáneas de recursos compartidos enviadas|
|Microsoft.DataShare/accounts|ReceivedShareSnapshots|Instantáneas de recursos compartidos recibidas|
|Microsoft.DBforMySQL/servers|MySqlSlowLogs|Registros de MySQL Server|
|Microsoft.DBforMySQL/servers|MySqlAuditLogs|Registros de auditoría de MySQL|
|Microsoft.DBforPostgreSQL/servers|PostgreSQLLogs|Registros del servidor PostgreSQL|
|Microsoft.DBforPostgreSQL/servers|QueryStoreRuntimeStatistics|Estadísticas de tiempo de ejecución del almacén de consultas de PostgreSQL|
|Microsoft.DBforPostgreSQL/servers|QueryStoreWaitStatistics|Estadísticas de espera del almacén de consultas de PostgreSQL|
|Microsoft.DBforPostgreSQL/serversv2|PostgreSQLLogs|Registros del servidor PostgreSQL|
|Microsoft.DBforPostgreSQL/serversv2|QueryStoreRuntimeStatistics|Estadísticas de tiempo de ejecución del almacén de consultas de PostgreSQL|
|Microsoft.DBforPostgreSQL/serversv2|QueryStoreWaitStatistics|Estadísticas de espera del almacén de consultas de PostgreSQL|
|Microsoft.DesktopVirtualization/workspaces|Punto de control|Punto de control|
|Microsoft.DesktopVirtualization/workspaces|Error|Error|
|Microsoft.DesktopVirtualization/workspaces|Administración|Administración|
|Microsoft.DesktopVirtualization/workspaces|Fuente|Fuente|
|Microsoft.DesktopVirtualization/applicationGroups|Punto de control|Punto de control|
|Microsoft.DesktopVirtualization/applicationGroups|Error|Error|
|Microsoft.DesktopVirtualization/applicationGroups|Administración|Administración|
|Microsoft.DesktopVirtualization/hostPools|Punto de control|Punto de control|
|Microsoft.DesktopVirtualization/hostPools|Error|Error|
|Microsoft.DesktopVirtualization/hostPools|Administración|Administración|
|Microsoft.DesktopVirtualization/hostPools|Conexión|Conexión|
|Microsoft.DesktopVirtualization/hostPools|HostRegistration|HostRegistration|
|Microsoft.Devices/IotHubs|Conexiones|Conexiones|
|Microsoft.Devices/IotHubs|DeviceTelemetry|Telemetría de dispositivo|
|Microsoft.Devices/IotHubs|C2DCommands|Comandos C2D|
|Microsoft.Devices/IotHubs|DeviceIdentityOperations|Operaciones de identidad de dispositivos|
|Microsoft.Devices/IotHubs|FileUploadOperations|Operaciones de carga de archivos|
|Microsoft.Devices/IotHubs|Rutas|Rutas|
|Microsoft.Devices/IotHubs|D2CTwinOperations|D2CTwinOperations|
|Microsoft.Devices/IotHubs|C2DTwinOperations|Operaciones gemelas C2D|
|Microsoft.Devices/IotHubs|TwinQueries|Consultas gemelas|
|Microsoft.Devices/IotHubs|JobsOperations|Operaciones de trabajos|
|Microsoft.Devices/IotHubs|DirectMethods|Métodos directos|
|Microsoft.Devices/IotHubs|DistributedTracing|Seguimiento distribuido (versión preliminar)|
|Microsoft.Devices/IotHubs|Configurations|Configurations|
|Microsoft.Devices/IotHubs|DeviceStreams|Flujos de dispositivos (versión preliminar)|
|Microsoft.Devices/provisioningServices|DeviceOperations|Operaciones de dispositivo|
|Microsoft.Devices/provisioningServices|ServiceOperations|Operaciones del servicio|
|Microsoft.DocumentDB/databaseAccounts|DataPlaneRequests|DataPlaneRequests|
|Microsoft.DocumentDB/databaseAccounts|MongoRequests|MongoRequests|
|Microsoft.DocumentDB/databaseAccounts|QueryRuntimeStatistics|QueryRuntimeStatistics|
|Microsoft.DocumentDB/databaseAccounts|PartitionKeyStatistics|PartitionKeyStatistics|
|Microsoft.DocumentDB/databaseAccounts|ControlPlaneRequests|ControlPlaneRequests|
|Microsoft.EnterpriseKnowledgeGraph/services|AuditEvent|Registro de AuditEvent|
|Microsoft.EnterpriseKnowledgeGraph/services|DataIssue|Registro de DataIssue|
|Microsoft.EnterpriseKnowledgeGraph/services|Requests|Registro de configuración|
|Microsoft.EventHub/namespaces|ArchiveLogs|Registros de archivo|
|Microsoft.EventHub/namespaces|OperationalLogs|Registros operativos|
|Microsoft.EventHub/namespaces|AutoScaleLogs|Registros de escalado automático|
|Microsoft.EventHub/namespaces|KafkaCoordinatorLogs|Registros de coordinador de Kafka|
|Microsoft.EventHub/namespaces|KafkaUserErrorLogs|Registros de error de usuario de Kafka|
|Microsoft.EventHub/namespaces|EventHubVNetConnectionEvent|Registros de conexión de filtrado de red virtual/IP|
|Microsoft.EventHub/namespaces|CustomerManagedKeyUserLogs|Registros de clave administrada de cliente|
|Microsoft.HealthcareApis/services|AuditLogs|Registros de auditoría|
|Microsoft.Insights/AutoscaleSettings|AutoscaleEvaluations|Evaluaciones de escalabilidad automática|
|Microsoft.Insights/AutoscaleSettings|AutoscaleScaleActions|Acciones de escalabilidad automática|
|Microsoft.IoTSpaces/Graph|Seguimiento|Seguimiento|
|Microsoft.IoTSpaces/Graph|Operativos|Operativos|
|Microsoft.IoTSpaces/Graph|Auditoría|Auditoría|
|Microsoft.IoTSpaces/Graph|UserDefinedFunction|UserDefinedFunction|
|Microsoft.IoTSpaces/Graph|Entrada|Entrada|
|Microsoft.IoTSpaces/Graph|Salida|Salida|
|Microsoft.KeyVault/vaults|AuditEvent|Registros de auditoría|
|Microsoft.Kusto/Clusters|SucceededIngestion|Operaciones de ingesta correctas|
|Microsoft.Kusto/Clusters|FailedIngestion|Operaciones de ingesta con error|
|Microsoft.Logic/workflows|WorkflowRuntime|Eventos de diagnóstico en tiempo de ejecución de flujo de trabajo|
|Microsoft.Logic/integrationAccounts|IntegrationAccountTrackingEvents|Eventos de seguimiento de la cuenta de integración|
|Microsoft.MachineLearningServices/workspaces|AmlComputeClusterEvent|AmlComputeClusterEvent|
|Microsoft.MachineLearningServices/workspaces|AmlComputeClusterNodeEvent|AmlComputeClusterNodeEvent|
|Microsoft.MachineLearningServices/workspaces|AmlComputeJobEvent|AmlComputeJobEvent|
|Microsoft.Media/mediaservices|KeyDeliveryRequests|Solicitudes de entrega de clave|
|Microsoft.Network/networksecuritygroups|NetworkSecurityGroupEvent|Evento de grupo de seguridad de red|
|Microsoft.Network/networksecuritygroups|NetworkSecurityGroupRuleCounter|Contador de reglas de grupo de seguridad de red|
|Microsoft.Network/networksecuritygroups|NetworkSecurityGroupFlowEvent|Evento de flujo de reglas de grupo de seguridad de red|
|Microsoft.Network/publicIPAddresses|DDoSProtectionNotifications|Notificaciones de protección contra DDoS|
|Microsoft.Network/publicIPAddresses|DDoSMitigationFlowLogs|Registros de flujos de las decisiones de mitigación de DDoS|
|Microsoft.Network/publicIPAddresses|DDoSMitigationReports|Informes de las mitigaciones de DDoS|
|Microsoft.Network/virtualNetworks|VMProtectionAlerts|Alertas de protección de máquina virtual|
|Microsoft.Network/applicationGateways|ApplicationGatewayAccessLog|Registro de acceso de Application Gateway|
|Microsoft.Network/applicationGateways|ApplicationGatewayPerformanceLog|Registro de rendimiento de Application Gateway|
|Microsoft.Network/applicationGateways|ApplicationGatewayFirewallLog|Registro de Firewall de Application Gateway|
|Microsoft.Network/azurefirewalls|AzureFirewallApplicationRule|Regla de aplicación de Azure Firewall|
|Microsoft.Network/azurefirewalls|AzureFirewallNetworkRule|Regla de red de Azure Firewall|
|Microsoft.Network/virtualNetworkGateways|GatewayDiagnosticLog|Registros de diagnóstico de puerta de enlace|
|Microsoft.Network/virtualNetworkGateways|TunnelDiagnosticLog|Registros de diagnóstico de túnel|
|Microsoft.Network/virtualNetworkGateways|RouteDiagnosticLog|Registros de diagnóstico de ruta|
|Microsoft.Network/virtualNetworkGateways|IKEDiagnosticLog|Registros de IKE Diagnostic|
|Microsoft.Network/virtualNetworkGateways|P2SDiagnosticLog|Registros de P2S Diagnostic|
|Microsoft.Network/trafficManagerProfiles|ProbeHealthStatusEvents|Evento de resultados de estado de sondeo de Traffic Manager|
|Microsoft.Network/expressRouteCircuits|PeeringRouteLog|Registros de tablas de rutas de emparejamiento|
|Microsoft.Network/vpnGateways|GatewayDiagnosticLog|Registros de diagnóstico de puerta de enlace|
|Microsoft.Network/vpnGateways|TunnelDiagnosticLog|Registros de diagnóstico de túnel|
|Microsoft.Network/vpnGateways|RouteDiagnosticLog|Registros de diagnóstico de ruta|
|Microsoft.Network/vpnGateways|IKEDiagnosticLog|Registros de IKE Diagnostic|
|Microsoft.Network/frontdoors|FrontdoorAccessLog|Registro de acceso de Front Door|
|Microsoft.Network/frontdoors|FrontdoorWebApplicationFirewallLog|Registro de firewall de aplicaciones web de Front Door|
|Microsoft.Network/p2sVpnGateways|GatewayDiagnosticLog|Registros de diagnóstico de puerta de enlace|
|Microsoft.Network/p2sVpnGateways|IKEDiagnosticLog|Registros de IKE Diagnostic|
|Microsoft.Network/p2sVpnGateways|P2SDiagnosticLog|Registros de P2S Diagnostic|
|Microsoft.Network/bastionHosts|BastionAuditLogs|Registros de auditoría de Bastion|
|Microsoft.Network/loadBalancers|LoadBalancerAlertEvent|Eventos de alerta de equilibrador de carga|
|Microsoft.Network/loadBalancers|LoadBalancerProbeHealthStatus|Estado de mantenimiento de sondeo de equilibrador de carga|
|Microsoft.PowerBIDedicated/capacities|Motor|Motor|
|Microsoft.RecoveryServices/Vaults|AzureBackupReport|Datos de informes de Azure Backup|
|Microsoft.RecoveryServices/Vaults|CoreAzureBackup|Datos principales de Azure Backup|
|Microsoft.RecoveryServices/Vaults|AddonAzureBackupJobs|Datos de trabajo de Azure Backup del complemento|
|Microsoft.RecoveryServices/Vaults|AddonAzureBackupAlerts|Datos de alerta de Azure Backup del complemento|
|Microsoft.RecoveryServices/Vaults|AddonAzureBackupPolicy|Datos de directiva de Azure Backup del complemento|
|Microsoft.RecoveryServices/Vaults|AddonAzureBackupStorage|Datos de almacenamiento de Azure Backup del complemento|
|Microsoft.RecoveryServices/Vaults|AddonAzureBackupProtectedInstance|Datos de instancia protegida de Azure Backup del complemento|
|Microsoft.RecoveryServices/Vaults|AzureSiteRecoveryJobs|Trabajos de Azure Site Recovery|
|Microsoft.RecoveryServices/Vaults|AzureSiteRecoveryEvents|Eventos de Azure Site Recovery|
|Microsoft.RecoveryServices/Vaults|AzureSiteRecoveryReplicatedItems|Elementos replicados de Azure Site Recovery|
|Microsoft.RecoveryServices/Vaults|AzureSiteRecoveryReplicationStats|Estadísticas de replicación de Azure Site Recovery|
|Microsoft.RecoveryServices/Vaults|AzureSiteRecoveryRecoveryPoints|Puntos de recuperación de Azure Site Recovery|
|Microsoft.RecoveryServices/Vaults|AzureSiteRecoveryReplicationDataUploadRate|Velocidad de carga de datos de replicación de Azure Site Recovery|
|Microsoft.RecoveryServices/Vaults|AzureSiteRecoveryProtectedDiskDataChurn|Actividad de los datos del disco protegido de Azure Site Recovery|
|Microsoft.Search/searchServices|OperationLogs|Registros de operaciones|
|Microsoft.ServiceBus/namespaces|OperationalLogs|Registros operativos|
|Microsoft.Sql/servers/databases|SQLInsights|SQL Insights|
|Microsoft.Sql/servers/databases|AutomaticTuning|Ajuste automático|
|Microsoft.Sql/servers/databases|QueryStoreRuntimeStatistics|Estadísticas de tiempo de ejecución del Almacén de consultas|
|Microsoft.Sql/servers/databases|QueryStoreWaitStatistics|Estadísticas de espera del Almacén de consultas|
|Microsoft.Sql/servers/databases|Errors|Errors|
|Microsoft.Sql/servers/databases|DatabaseWaitStatistics|Estadísticas de espera de base de datos|
|Microsoft.Sql/servers/databases|Tiempos de expiración|Tiempos de expiración|
|Microsoft.Sql/servers/databases|Blocks|Blocks|
|Microsoft.Sql/servers/databases|Interbloqueos|Interbloqueos|
|Microsoft.Sql/servers/databases|Auditoría|Registros de auditoría|
|Microsoft.Sql/servers/databases|SQLSecurityAuditEvents|Evento de auditoría de seguridad de SQL|
|Microsoft.Sql/servers/databases|DmsWorkers|Trabajo de DMS|
|Microsoft.Sql/servers/databases|ExecRequests|Solicitudes de ejecución|
|Microsoft.Sql/servers/databases|RequestSteps|Pasos de la solicitud|
|Microsoft.Sql/servers/databases|SqlRequests|Solicitudes de SQL|
|Microsoft.Sql/servers/databases|Esperas|Esperas|
|Microsoft.Sql/managedInstances|ResourceUsageStats|Estadísticas de uso de recursos|
|Microsoft.Sql/managedInstances|SQLSecurityAuditEvents|Evento de auditoría de seguridad de SQL|
|Microsoft.Sql/managedInstances/databases|SQLInsights|SQL Insights|
|Microsoft.Sql/managedInstances/databases|QueryStoreRuntimeStatistics|Estadísticas de tiempo de ejecución del Almacén de consultas|
|Microsoft.Sql/managedInstances/databases|QueryStoreWaitStatistics|Estadísticas de espera del Almacén de consultas|
|Microsoft.Sql/managedInstances/databases|Errors|Errors|
|Microsoft.Storage/storageAccounts/tableServices|StorageRead|StorageRead|
|Microsoft.Storage/storageAccounts/tableServices|StorageWrite|StorageWrite|
|Microsoft.Storage/storageAccounts/tableServices|StorageDelete|StorageDelete|
|Microsoft.Storage/storageAccounts/blobServices|StorageRead|StorageRead|
|Microsoft.Storage/storageAccounts/blobServices|StorageWrite|StorageWrite|
|Microsoft.Storage/storageAccounts/blobServices|StorageDelete|StorageDelete|
|Microsoft.Storage/storageAccounts/fileServices|StorageRead|StorageRead|
|Microsoft.Storage/storageAccounts/fileServices|StorageWrite|StorageWrite|
|Microsoft.Storage/storageAccounts/fileServices|StorageDelete|StorageDelete|
|Microsoft.Storage/storageAccounts/queueServices|StorageRead|StorageRead|
|Microsoft.Storage/storageAccounts/queueServices|StorageWrite|StorageWrite|
|Microsoft.Storage/storageAccounts/queueServices|StorageDelete|StorageDelete|
|Microsoft.StreamAnalytics/streamingjobs|Ejecución|Ejecución|
|Microsoft.StreamAnalytics/streamingjobs|Creación|Creación|
|microsoft.web/hostingenvironments|AppServiceEnvironmentPlatformLogs|Registros de plataforma de App Service Environment|
|microsoft.web/sites|FunctionAppLogs|Registros de aplicaciones de función|
|microsoft.web/sites|AppServiceHTTPLogs|Registros de HTTP|
|microsoft.web/sites|AppServiceConsoleLogs|Registros de consola de App Service|
|microsoft.web/sites|AppServiceAppLogs|Registros de aplicaciones de App Service|
|microsoft.web/sites|AppServiceFileAuditLogs|Registros de auditoría de cambios de contenido de sitio|
|microsoft.web/sites|AppServiceAuditLogs|Acceso a registros de auditoría|
|microsoft.web/sites/slots|FunctionAppLogs|Registros de aplicaciones de función|
|microsoft.web/sites/slots|AppServiceHTTPLogs|Registros de HTTP|
|microsoft.web/sites/slots|AppServiceConsoleLogs|Registros de la consola|
|microsoft.web/sites/slots|AppServiceAppLogs|Registros de aplicación|
|microsoft.web/sites/slots|AppServiceFileAuditLogs|Registros de auditoría de cambios de contenido de sitio|
|microsoft.web/sites/slots|AppServiceAuditLogs|Acceso a registros de auditoría|

## <a name="next-steps"></a>Pasos siguientes

* [Más información sobre los registros de recurso](../../azure-monitor/platform/platform-logs-overview.md)
* [Transmisión de registros de recursos a **Event Hubs**](../../azure-monitor/platform/resource-logs-stream-event-hubs.md)
* [Cambio de la configuración de diagnóstico del registro de recursos con la API REST de Azure Monitor](https://docs.microsoft.com/rest/api/monitor/diagnosticsettings)
* [Análisis de registros desde Azure Storage con Log Analytics](../../azure-monitor/platform/collect-azure-metrics-logs.md)
