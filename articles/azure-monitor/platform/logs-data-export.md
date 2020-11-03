---
title: Exportación de datos del área de trabajo de Log Analytics en Azure Monitor (versión preliminar)
description: La exportación de datos de Log Analytics permite exportar continuamente los datos de las tablas seleccionadas del área de trabajo de Log Analytics en una cuenta de Azure Storage o Azure Event Hubs a medida que se recopilan.
ms.subservice: logs
ms.topic: conceptual
ms.custom: references_regions
author: bwren
ms.author: bwren
ms.date: 10/14/2020
ms.openlocfilehash: 6c0908d2656d9d6464ae1f94d5b0cd68f759530a
ms.sourcegitcommit: fb3c846de147cc2e3515cd8219d8c84790e3a442
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/27/2020
ms.locfileid: "92637350"
---
# <a name="log-analytics-workspace-data-export-in-azure-monitor-preview"></a>Exportación de datos del área de trabajo de Log Analytics en Azure Monitor (versión preliminar)
La exportación de datos del área de trabajo de Log Analytics en Azure Monitor permite exportar continuamente los datos de las tablas seleccionadas del área de trabajo de Log Analytics en una cuenta de Azure Storage o Azure Event Hubs a medida que se recopilan. En este artículo se ofrecen detalles sobre esta característica y pasos para configurar la exportación de datos en las áreas de trabajo.

## <a name="overview"></a>Introducción
Una vez configurada la exportación de datos del área de trabajo de Log Analytics, los nuevos datos que se envíen a las tablas seleccionadas en el área de trabajo se exportarán automáticamente a la cuenta de almacenamiento cada hora o al centro de eventos casi en tiempo real.

![Información general sobre la exportación de datos](media/logs-data-export/data-export-overview.png)

Todos los datos de las tablas incluidas se exportan sin ningún filtro. Por ejemplo, al configurar una regla de exportación de datos para la tabla *SecurityEvent* , todos los datos enviados a la tabla *SecurityEvent* se exportan a partir de la hora de configuración.


## <a name="other-export-options"></a>Otras opciones de exportación
La exportación de datos del área de trabajo de Log Analytics permite exportar datos continuamente de un área de trabajo de Log Analytics. Otras opciones para exportar datos en determinados escenarios son las siguientes:

- Exportación programada desde una consulta de registro con una aplicación lógica. Es similar a la característica de exportación de datos, pero permite enviar datos filtrados o agregados a Azure Storage. Este método está sujeto a los [límites de las consultas de registro](../service-limits.md#log-analytics-workspaces). Consulte [Archivado de datos de un área de trabajo de Log Analytics en Azure Storage mediante Logic Apps](logs-export-logic-app.md).
- Exportación única mediante una aplicación lógica. Consulte [Conector de Azure Monitor Logs para Logic Apps y Power Automate](logicapp-flow-connector.md).
- Exportación única en la máquina local mediante el script de PowerShell. Consulte [Invoke-AzOperationalInsightsQueryExport](https://www.powershellgallery.com/packages/Invoke-AzOperationalInsightsQueryExport).


## <a name="current-limitations"></a>Limitaciones actuales

- Actualmente, la configuración solo se puede realizar mediante solicitudes REST o la CLI. No se puede usar Azure Portal ni PowerShell.
- La opción ```--export-all-tables``` de la CLI y REST no se admite y se quitará. Debe proporcionar la lista de tablas en las reglas de exportación de manera explícita.
- Las tablas admitidas se limitan actualmente a las específicas de la sección [Tablas admitidas](#supported-tables) más adelante. Si la regla de exportación de datos incluye una tabla no admitida, la operación se realizará correctamente, pero no se exportará ningún dato de esa tabla. Si la regla de exportación de datos incluye una tabla que no existe, se producirá un error ```Table <tableName> does not exist in the workspace.```.
- El área de trabajo de Log Analytics puede estar en cualquier región, excepto en las siguientes:
  - Norte de Suiza
  - Oeste de Suiza
  - Regiones de Azure Government
- La cuenta de almacenamiento de destino o el centro de eventos deben estar en la misma región que el área de trabajo de Log Analytics.
- Los nombres de las tablas que se vayan a exportar no pueden tener más de 60 caracteres para una cuenta de almacenamiento, ni más de 47 caracteres en el caso de un centro de eventos. Las tablas con nombres más largos no se exportarán.

> [!NOTE]
> La exportación de datos de Log Analytics escribe los datos como blobs en anexos, lo que actualmente se encuentra en versión preliminar para Azure Data Lake Storage Gen2. Para poder configurar la exportación en este almacenamiento, tendrá que abrir una solicitud de soporte técnico. Use los detalles siguientes para dicha solicitud.
> - Tipo de problema: Requisitos previos técnicos
> - Suscripción: Su suscripción
> - Servicio: Data Lake Storage Gen2
> - Recurso: Nombre del recurso
> - Resumen: Solicitud del registro de la suscripción para aceptar datos de la exportación de datos de Log Analytics.
> - Tipo de problema: Conectividad
> - Subtipo de problema: Incidencia de conectividad

## <a name="data-completeness"></a>Integridad de los datos
La exportación de datos continuará reintentando el envío de datos durante un máximo de 30 minutos, en el caso de que el destino no esté disponible. Si sigue sin estar disponible después de 30 minutos, los datos se descartarán hasta que el destino esté disponible.

## <a name="cost"></a>Coste
Actualmente no se cobran cargos adicionales por la característica de exportación de datos. Los precios de la exportación de datos se anunciarán en el futuro y se mostrará un aviso antes del inicio de la facturación. Si decide seguir usando la exportación de datos después del período de aviso, se le facturará según la tarifa aplicable.

## <a name="export-destinations"></a>Destinos de la exportación

### <a name="storage-account"></a>Cuenta de almacenamiento
Los datos se envían cada hora a las cuentas de almacenamiento. La configuración de la exportación de datos crea un contenedor para cada tabla de la cuenta de almacenamiento con el nombre *am-* seguido del nombre de la tabla. Por ejemplo, la tabla *SecurityEvent* se enviaría a un contenedor denominado *am-SecurityEvent*.

La ruta de acceso a los blobs de la cuenta de almacenamiento es *WorkspaceResourceId=/subscriptions/subscription-id/resourcegroups/\<resource-group\>/providers/microsoft.operationalinsights/workspaces/\<workspace\>/y=\<four-digit numeric year\>/m=\<two-digit numeric month\>/d=\<two-digit numeric day\>/h=\<two-digit 24-hour clock hour\>/m=00/PT1H.json*. Dado que los blobs en anexos están limitados a 50 000 escrituras en el almacenamiento, el número de blobs exportados puede ampliarse si el número de anexos es elevado. El patrón de nomenclatura de los blobs sería PT1H_#.json en este caso, donde # corresponde al incremento del número de blobs.

El formato de los datos de la cuenta de almacenamiento es en [líneas JSON](diagnostic-logs-append-blobs.md). Esto significa que todos los registros se delimitan mediante una nueva línea, sin matrices de registros exteriores y sin comas entre los registros JSON. 

[![Datos de ejemplo de almacenamiento](media/logs-data-export/storage-data.png)](media/logs-data-export/storage-data.png#lightbox)

La exportación de datos de Log Analytics puede escribir blobs en anexos en cuentas de almacenamiento inmutables cuando las directivas de retención con una duración definida tienen habilitada la opción *allowProtectedAppendWrites*. Esto permite escribir nuevos bloques en un blob en anexos al tiempo que se mantienen la protección y el cumplimiento de la inmutabilidad. Consulte [Permitir escrituras de blobs en anexos protegidos](../../storage/blobs/storage-blob-immutable-storage.md#allow-protected-append-blobs-writes).

### <a name="event-hub"></a>Centro de eventos
Los datos se envían al centro de eventos prácticamente en tiempo real a medida que llegan a Azure Monitor. Se crea un centro de eventos para cada tipo de datos que se exporta con el nombre *am-* seguido del nombre de la tabla. Por ejemplo, la tabla *SecurityEvent* se enviaría a un centro de eventos denominado *am-SecurityEvent*. Si quiere que los datos exportados lleguen a un centro de eventos específico, o si tiene una tabla con un nombre que supere el límite de 47 caracteres, puede proporcionar el nombre de su propio centro de eventos y exportar todos los datos para las tablas definidas en él.

El volumen de los datos exportados suele aumentar con el tiempo, y es necesario aumentar la escala del centro de eventos para administrar velocidades de transferencia mayores, así como para evitar escenarios de limitación y latencia de datos. Debe usar la característica de inflado automático de Event Hubs para escalar verticalmente y aumentar el número de unidades de procesamiento de forma automática y, de este modo, satisfacer las necesidades de uso. Consulte [Escalado vertical y automático de las unidades de procesamiento de Azure Event Hubs](../../event-hubs/event-hubs-auto-inflate.md) para obtener más información.


## <a name="prerequisites"></a>Requisitos previos
A continuación se indican los requisitos previos que hay que cumplir para poder configurar la exportación de datos de Log Analytics.

- La cuenta de almacenamiento y el centro de eventos deben haberse creado ya y estar en la misma región que el área de trabajo de Log Analytics. Si tiene que replicar los datos en otras cuentas de almacenamiento, puede utilizar cualquiera de las [opciones de redundancia Azure Storage](../../storage/common/storage-redundancy.md).  
- La cuenta de almacenamiento debe ser StorageV1 o StorageV2. No se admite el almacenamiento clásico.  
- Si ha configurado la cuenta de almacenamiento para permitir el acceso desde las redes seleccionadas, tiene que agregar una excepción en la configuración de la cuenta de almacenamiento para permitir que Azure Monitor escriba en el almacenamiento.

## <a name="enable-data-export"></a>Habilitación de la exportación de datos
Para habilitar la exportación de datos de Log Analytics, hay que seguir los pasos que se detallan a continuación. Consulte las siguientes secciones para obtener más información sobre cada uno de ellos.

- Registre el proveedor de recursos.
- Habilite los servicios de Microsoft de confianza.
- Cree una o varias reglas de exportación de datos que definan las tablas que se van a exportar y su destino.

### <a name="register-resource-provider"></a>Registro del proveedor de recursos
El siguiente proveedor de recursos de Azure debe registrarse en su suscripción para habilitar la exportación de datos de Log Analytics. 

- Microsoft.Insights

Probablemente, este proveedor de recursos ya esté registrado para la mayoría de los usuarios Azure Monitor. Para comprobarlo, vaya a **Suscripciones** en Azure Portal. Seleccione su suscripción y, luego, haga clic en **Proveedores de recursos** , en la sección **Configuración** del menú. Busque **Microsoft.Insights**. Si su estado es **Registrado** , entonces ya está registrado. Si no es así, haga clic en **Registrar** para registrarlo.

También puede usar cualquiera de los métodos disponibles para registrar un proveedor de recursos, tal y como se describe en [Tipos y proveedores de recursos de Azure](../../azure-resource-manager/management/resource-providers-and-types.md). El siguiente es un comando de ejemplo con PowerShell:

```PowerShell
Register-AzResourceProvider -ProviderNamespace Microsoft.insights
```

### <a name="allow-trusted-microsoft-services"></a>Habilitación de los servicios de Microsoft de confianza
Si ha configurado la cuenta de almacenamiento para permitir el acceso desde las redes seleccionadas, tiene que agregar una excepción para permitir que Azure Monitor escriba en la cuenta. En la opción **Firewalls y redes virtuales** de la cuenta de almacenamiento, seleccione **Permitir que los servicios de Microsoft de confianza accedan a esta cuenta de almacenamiento**.

[![Firewalls y redes virtuales de la cuenta de almacenamiento](media/logs-data-export/storage-account-vnet.png)](media/logs-data-export/storage-account-vnet.png#lightbox)


### <a name="create-or-update-data-export-rule"></a>Creación o actualización de una regla de exportación de datos
Una regla de exportación de datos define los datos que se van a exportar para un conjunto de tablas a un único destino. Puede crear una regla para cada destino.

Use el siguiente comando de la CLI para ver las tablas en el área de trabajo. Puede resultar útil copiar las tablas que quiere e incluirlas en la regla de exportación de datos.
```azurecli
az monitor log-analytics workspace table list -resource-group resourceGroupName --workspace-name workspaceName --query [].name --output table
```

Use el siguiente comando para crear una regla de exportación de datos en una cuenta de almacenamiento mediante la CLI.

```azurecli
az monitor log-analytics workspace data-export create --resource-group resourceGroupName --workspace-name workspaceName --name ruleName --tables SecurityEvent Heartbeat --destination $storageAccountId
```

Use el siguiente comando para crear una regla de exportación de datos en un centro de eventos mediante la CLI.

```azurecli
az monitor log-analytics workspace data-export create --resource-group resourceGroupName --workspace-name workspaceName --name ruleName --tables SecurityEvent Heartbeat --destination $eventHubsNamespacesId
```

Use la siguiente solicitud para crear una regla de exportación de datos mediante la API REST. La solicitud debe usar la autorización del token de portador y el tipo de contenido aplicación/json.

```rest
PUT https://management.azure.com/subscriptions/<subscription-id>/resourcegroups/<resource-group-name>/providers/Microsoft.operationalInsights/workspaces/<workspace-name>/dataexports/<data-export-name>?api-version=2020-08-01
```

El cuerpo de la solicitud especifica el destino de las tablas. El siguiente es un cuerpo de ejemplo para la solicitud REST para una cuenta de almacenamiento.

```json
{
    "properties": {
        "destination": {
            "resourceId": "/subscriptions/subscription-id/resourcegroups/resource-group-name/providers/Microsoft.Storage/storageAccounts/storage-account-name"
        },
        "tablenames": [
            "table1",
            "table2" 
        ],
        "enable": true
    }
}
```

El siguiente es un cuerpo de ejemplo para la solicitud REST para un centro de eventos.

```json
{
    "properties": {
        "destination": {
            "resourceId": "/subscriptions/subscription-id/resourcegroups/resource-group-name/providers/Microsoft.EventHub/namespaces/eventhub-namespaces-name"
        },
        "tablenames": [
            "table1",
            "table2"
        ],
        "enable": true
    }
}
```

El siguiente es un cuerpo de ejemplo para la solicitud REST para un centro de eventos, donde se proporciona el nombre del centro de eventos. En este caso, todos los datos exportados se envían a este centro de eventos.

```json
{
    "properties": {
        "destination": {
            "resourceId": "/subscriptions/subscription-id/resourcegroups/resource-group-name/providers/Microsoft.EventHub/namespaces/eventhub-namespaces-name",
            "metaData": {
                "EventHubName": "eventhub-name"
        },
        "tablenames": [
            "table1",
            "table2"
        ],
        "enable": true
    }
}
```

## <a name="view-data-export-configuration"></a>Consulta de la configuración de la exportación de datos
Use el siguiente comando para ver la configuración de una regla de exportación de datos mediante la CLI.

```azurecli
az monitor log-analytics workspace data-export show --resource-group resourceGroupName --workspace-name workspaceName --name ruleName
```

Use la siguiente solicitud para ver la configuración de una regla de exportación de datos mediante la API REST. La solicitud debe utilizar la autorización del token de portador.

```rest
GET https://management.azure.com/subscriptions/<subscription-id>/resourcegroups/<resource-group-name>/providers/Microsoft.operationalInsights/workspaces/<workspace-name>/dataexports/<data-export-name>?api-version=2020-08-01
```

## <a name="disable-an-export-rule"></a>Deshabilitación de una regla de exportación
Las reglas de exportación se pueden deshabilitar para permitir que se detenga la exportación cuando no sea necesario conservar los datos durante un período determinado, como al realizar pruebas. Use el siguiente comando para deshabilitar una regla de exportación de datos mediante la CLI.

```azurecli
az monitor log-analytics workspace data-export update --resource-group resourceGroupName --workspace-name workspaceName --name ruleName --enable false
```

Use la siguiente solicitud para deshabilitar una regla de exportación de datos mediante la API REST. La solicitud debe utilizar la autorización del token de portador.

```rest
PUT https://management.azure.com/subscriptions/<subscription-id>/resourcegroups/<resource-group-name>/providers/Microsoft.operationalInsights/workspaces/<workspace-name>/dataexports/<data-export-name>?api-version=2020-08-01
Authorization: Bearer <token>
Content-type: application/json

{
    "properties": {
        "destination": {
            "resourceId": "/subscriptions/subscription-id/resourcegroups/resource-group-name/providers/Microsoft.Storage/storageAccounts/storage-account-name"
        },
        "tablenames": [
"table1",
    "table2" 
        ],
        "enable": false
    }
}
```

## <a name="delete-an-export-rule"></a>Eliminación de una regla de exportación
Use el siguiente comando para eliminar una regla de exportación de datos mediante la CLI.

```azurecli
az monitor log-analytics workspace data-export delete --resource-group resourceGroupName --workspace-name workspaceName --name ruleName
```

Use la siguiente solicitud para eliminar una regla de exportación de datos mediante la API REST. La solicitud debe utilizar la autorización del token de portador.

```rest
DELETE https://management.azure.com/subscriptions/<subscription-id>/resourcegroups/<resource-group-name>/providers/Microsoft.operationalInsights/workspaces/<workspace-name>/dataexports/<data-export-name>?api-version=2020-08-01
```

## <a name="view-all-data-export-rules-in-a-workspace"></a>Consulta de todas las reglas de exportación de datos en un área de trabajo
Use el siguiente comando para ver todas las reglas de exportación de datos en un área de trabajo mediante la CLI.

```azurecli
az monitor log-analytics workspace data-export list --resource-group resourceGroupName --workspace-name workspaceName
```

Use la siguiente solicitud para ver todas las reglas de exportación de datos en un área de trabajo mediante la API REST. La solicitud debe utilizar la autorización del token de portador.

```rest
GET https://management.azure.com/subscriptions/<subscription-id>/resourcegroups/<resource-group-name>/providers/Microsoft.operationalInsights/workspaces/<workspace-name>/dataexports?api-version=2020-08-01
```

## <a name="unsupported-tables"></a>Tablas no admitidas
Si la regla de exportación de datos incluye una tabla no admitida, la configuración se realizará correctamente, pero no se exportará ningún dato de esa tabla. Si la tabla se admite posteriormente, sus datos se exportarán en ese momento.

Si la regla de exportación de datos incluye una tabla que no existe, se producirá un error ```Table <tableName> does not exist in the workspace.```.


## <a name="supported-tables"></a>Tablas admitidas
Las tablas admitidas se limitan actualmente a las que se especifican a continuación. Todos los datos de la tabla se exportarán a menos que se especifiquen limitaciones. Esta lista se actualizará a medida que se agregue compatibilidad con tablas adicionales.


| Tabla | Limitaciones |
|:---|:---|:---|
| AADDomainServicesAccountLogon | |
| AADDomainServicesAccountManagement | |
| AADDomainServicesDirectoryServiceAccess | |
| AADDomainServicesLogonLogoff | |
| AADDomainServicesPolicyChange | |
| AADDomainServicesPrivilegeUse | |
| AADManagedIdentitySignInLogs | |
| AADNonInteractiveUserSignInLogs | |
| AADProvisioningLogs | |
| AADServicePrincipalSignInLogs | |
| ADAssessmentRecommendation | |
| ADFActivityRun | |
| ADFPipelineRun | |
| ADFTriggerRun | |
| ADReplicationResult | |
| ADSecurityAssessmentRecommendation | |
| ADTDigitalTwinsOperation | |
| ADTEventRoutesOperation | |
| ADTModelsOperation | |
| ADTQueryOperation | |
| ADXCommand | |
| ADXQuery | |
| AegDeliveryFailureLogs | |
| AegPublishFailureLogs | |
| Alerta |La compatibilidad es parcial. Algunos de los datos de esta tabla se ingieren por medio de la cuenta de almacenamiento. Estos datos no se exportan actualmente. |
| Anomalías | |
| ApiManagementGatewayLogs | |
| AppCenterError | |
| AppPlatformSystemLogs | |
| AppServiceAppLogs | |
| AppServiceAuditLogs | |
| AppServiceConsoleLogs | |
| AppServiceFileAuditLogs | |
| AppServiceHTTPLogs | |
| AppServiceIPSecAuditLogs | |
| AppServicePlatformLogs | |
| AuditLogs | |
| AutoscaleEvaluationsLog | |
| AutoscaleScaleActionsLog | |
| AWSCloudTrail | |
| AzureAssessmentRecommendation | |
| AzureDevOpsAuditing | |
| BehaviorAnalytics | |
| BlockchainApplicationLog | |
| BlockchainProxyLog | |
| BlockchainProxyLog | |
| CommonSecurityLog | |
| CommonSecurityLog | |
| ComputerGroup | |
| ConfigurationData | La compatibilidad es parcial. Algunos de los datos se ingieren por medio de servicios internos que no se admiten para la exportación. Estos datos no se exportan actualmente. |
| ContainerImageInventory | |
| ContainerInventory | |
| ContainerLog | |
| ContainerLog | |
| ContainerNodeInventory | |
| ContainerServiceLog | |
| CoreAzureBackup | |
| DatabricksAccounts | |
| DatabricksClusters | |
| DatabricksDBFS | |
| DatabricksInstancePools | |
| DatabricksJobs | |
| DatabricksNotebook | |
| DatabricksSecrets | |
| DatabricksSQLPermissions | |
| DatabricksSSH | |
| DatabricksWorkspace | |
| DnsEvents | |
| DnsInventory | |
| Dynamics365Activity | |
| Evento | La compatibilidad es parcial. Algunos de los datos de esta tabla se ingieren por medio de la cuenta de almacenamiento. Estos datos no se exportan actualmente. |
| ExchangeAssessmentRecommendation | |
| ExchangeAssessmentRecommendation | |
| FailedIngestion | |
| FunctionAppLogs | |
| HDInsightAmbariClusterAlerts | |
| HDInsightAmbariSystemMetrics | |
| HDInsightGatewayAuditLogs | |
| HDInsightHadoopAndYarnLogs | |
| HDInsightHadoopAndYarnMetrics | |
| HDInsightHBaseLogs | |
| HDInsightHBaseMetrics | |
| HDInsightHiveAndLLAPLogsSample | |
| HDInsightKafkaLogs | |
| HDInsightKafkaMetrics | |
| HDInsightOozieLogs | |
| HDInsightSecurityLogs | |
| HDInsightSparkApplicationEvents | |
| HDInsightSparkBlockManagerEvents | |
| HDInsightSparkEnvironmentEvents | |
| HDInsightSparkEventsLog | |
| HDInsightSparkExecutorEvents | |
| HDInsightSparkExtraEvents | |
| HDInsightSparkJobEvents | |
| HDInsightSparkLogs | |
| HDInsightSparkSQLExecutionEvents | |
| HDInsightSparkStageEvents | |
| HDInsightSparkStageTaskAccumulables | |
| HDInsightSparkTaskEvents | |
| HDInsightStormLogs | |
| HDInsightStormMetrics | |
| HDInsightStormTopologyMetrics | |
| Latido | |
| HuntingBookmark | |
| InsightsMetrics | La compatibilidad es parcial. Algunos de los datos se ingieren por medio de servicios internos que no se admiten para la exportación. Esta parte no está disponible en la exportación actualmente. |
| IntuneAuditLogs | |
| IntuneDeviceComplianceOrg | |
| IntuneOperationalLogs | |
| KubeEvents | |
| KubeHealth | |
| KubeMonAgentEvents | |
| KubeNodeInventory | |
| KubePodInventory | |
| KubeServices | |
| KubeServices | |
| LAQueryLogs | |
| McasShadowItReporting | |
| MicrosoftAzureBastionAuditLogs | |
| MicrosoftDataShareReceivedSnapshotLog | |
| MicrosoftDataShareSentSnapshotLog | |
| MicrosoftDataShareShareLog | |
| MicrosoftHealthcareApisAuditLogs | |
| NWConnectionMonitorDestinationListenerResult | |
| NWConnectionMonitorDNSResult | |
| NWConnectionMonitorPathResult | |
| NWConnectionMonitorPathResult | |
| NWConnectionMonitorTestResult | |
| NWConnectionMonitorTestResult | |
| OfficeActivity | La compatibilidad es parcial. Algunos de los datos se ingieren mediante webhooks de Office 365 en Log Analytics. Estos datos no se exportan actualmente. |
| Operación | La compatibilidad es parcial. Algunos de los datos se ingieren por medio de servicios internos que no se admiten para la exportación. Estos datos no se exportan actualmente. |
| Perf | La compatibilidad es parcial. Actualmente solo se admiten los datos de rendimiento de Windows. Los datos de rendimiento de Linux no se exportan actualmente. |
| ProtectionStatus | |
| SCCMAssessmentRecommendation | |
| SCOMAssessmentRecommendation | |
| SecurityAlert | |
| SecurityBaseline | |
| SecurityBaselineSummary | |
| SecurityDetection | |
| SecurityEvent | |
| SecurityIncident | |
| SecurityIoTRawEvent | |
| SecurityNestedRecommendation | |
| SecurityRecommendation | |
| SfBAssessmentRecommendation | |
| SfBOnlineAssessmentRecommendation | |
| SharePointOnlineAssessmentRecommendation | |
| SignalRServiceDiagnosticLogs | |
| SigninLogs | |
| SPAssessmentRecommendation | |
| SQLAssessmentRecommendation | |
| SucceededIngestion | |
| SynapseGatewayEvents | |
| SynapseRBACEvents | |
| syslog | La compatibilidad es parcial. Algunos de los datos de esta tabla se ingieren por medio de la cuenta de almacenamiento. Estos datos no se exportan actualmente. |
| ThreatIntelligenceIndicator | |
| Actualizar | La compatibilidad es parcial. Algunos de los datos se ingieren por medio de servicios internos que no se admiten para la exportación. Estos datos no se exportan actualmente. |
| UpdateRunProgress | |
| UpdateSummary | |
| Uso | |
| UserAccessAnalytics | |
| UserPeerAnalytics | |
| Watchlist | |
| WindowsEvent | |
| WindowsFirewall | |
| WireData | La compatibilidad es parcial. Algunos de los datos se ingieren por medio de servicios internos que no se admiten para la exportación. Estos datos no se exportan actualmente. |
| WorkloadMonitoringPerf | |
| WorkloadMonitoringPerf | |
| WVDAgentHealthStatus | |
| WVDCheckpoints | |
| WVDConnections | |
| WVDErrors | |
| WVDFeeds | |
| WVDManagement | |


## <a name="next-steps"></a>Pasos siguientes

- [Consulta de datos exportados desde Azure Monitor mediante Azure Data Explorer (versión preliminar)](azure-data-explorer-query-storage.md)
