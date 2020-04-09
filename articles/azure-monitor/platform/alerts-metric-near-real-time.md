---
title: Recursos compatibles para las alertas de métricas de Azure Monitor
description: Referencia sobre métricas y registros de soporte técnico para las alertas de métricas en Azure Monitor
author: harelbr
ms.author: harelbr
services: monitoring
ms.topic: conceptual
ms.date: 3/5/2020
ms.subservice: alerts
ms.openlocfilehash: c036fa3708d718d6199d27989e60b11015a1227e
ms.sourcegitcommit: 3c318f6c2a46e0d062a725d88cc8eb2d3fa2f96a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/02/2020
ms.locfileid: "80585853"
---
# <a name="supported-resources-for-metric-alerts-in-azure-monitor"></a>Recursos compatibles para las alertas de métricas de Azure Monitor

Azure Monitor ahora admite un [nuevo tipo de alerta de métrica](../../azure-monitor/platform/alerts-overview.md) que tiene ventajas considerables sobre las anteriores [alertas de métrica clásicas](../../azure-monitor/platform/alerts-classic.overview.md). Las métricas están disponibles para una [amplia lista de servicios de Azure](../../azure-monitor/platform/metrics-supported.md). Las nuevas alertas admiten un subconjunto (creciente) de los tipos de recurso. En este artículo se muestra ese subconjunto.

También puede utilizar las nuevas alertas de métricas en los datos de registros populares almacenados en un área de trabajo de Log Analytics extraídos como métricas. Para obtener más información, consulte [Alertas de métricas para registros](../../azure-monitor/platform/alerts-metric-logs.md).

## <a name="portal-powershell-cli-rest-support"></a>Portal, PowerShell, CLI, REST support
Actualmente, solo puede crear alertas de métricas nuevas en Azure Portal, la [API de REST](https://docs.microsoft.com/rest/api/monitor/metricalerts/) o [plantillas de Resource Manager](../../azure-monitor/platform/alerts-metric-create-templates.md). La compatibilidad con la configuración de nuevas alertas mediante las versiones 2.0 de PowerShell y la CLI de Azure estará disponible próximamente.

## <a name="metrics-and-dimensions-supported"></a>Métricas y dimensiones compatibles
Las nuevas alertas de métricas admiten alertas de métricas que utilizan dimensiones. Puede usar dimensiones para filtrar las métricas al nivel adecuado. Todas las métricas compatibles, junto con las dimensiones aplicables, se pueden explorar y visualizar en el [Explorador de métricas de Azure Monitor](../../azure-monitor/platform/metrics-charts.md).

Esta es la lista completa de los orígenes de métricas de Azure Monitor que se admiten por las nuevas alertas:

|Tipo de recurso  |Dimensiones compatibles |Alertas de varios recursos| Métricas disponibles|
|---------|---------|-----|----------|
|Microsoft.ApiManagement/service | Sí| No | [API Management](../../azure-monitor/platform/metrics-supported.md#microsoftapimanagementservice)|
|Microsoft.AppPlatform/Spring |No| Sí|
|Microsoft.Automation/automationAccounts | Sí| No | [Cuentas de Automation](../../azure-monitor/platform/metrics-supported.md#microsoftautomationautomationaccounts)|
|Microsoft.Batch/batchAccounts | N/D| No | [Cuentas de Batch](../../azure-monitor/platform/metrics-supported.md#microsoftbatchbatchaccounts)|
|Microsoft.Cache/Redis|Sí| No |[Azure Cache for Redis](../../azure-monitor/platform/metrics-supported.md#microsoftcacheredis)|
|Microsoft.ClassicStorage/storageAccounts/mmxclassic|No|Sí|
|Microsoft.ClassicStorage/storageAccounts/mmxclassic/blobServices|No|Sí|
|Microsoft.ClassicStorage/storageAccounts/mmxclassic/fileServices|No|Sí|
|Microsoft.ClassicStorage/storageAccounts/mmxclassic/queueServices|No|Sí|
|Microsoft.ClassicStorage/storageAccounts/mmxclassic/tableServices|No|Sí| |
|Microsoft.CognitiveServices/accounts| N/D | No | [Cognitive Services](../../azure-monitor/platform/metrics-supported.md#microsoftcognitiveservicesaccounts)|
|Microsoft.Compute/virtualMachines |Sí | Sí | [Máquinas virtuales](../../azure-monitor/platform/metrics-supported.md#microsoftcomputevirtualmachines)|
|Microsoft.Compute/virtualMachineScaleSets |N/D | Sí |[Conjuntos de escalado de máquinas virtuales](../../azure-monitor/platform/metrics-supported.md#microsoftcomputevirtualmachinescalesets)|
|Microsoft.ContainerInstance/containerGroups | Sí| No | [Grupos de contenedores](../../azure-monitor/platform/metrics-supported.md#microsoftcontainerinstancecontainergroups)|
|Microsoft.ContainerService/managedClusters | Sí | No | [Clústeres administrados](../../azure-monitor/platform/metrics-supported.md#microsoftcontainerservicemanagedclusters)|
|Microsoft.DataBoxEdge/dataBoxEdgeDevices | Sí | Sí | |
|Microsoft.DataFactory/datafactories| Sí| No | [Factorías de datos V1](../../azure-monitor/platform/metrics-supported.md#microsoftdatafactorydatafactories)|
|Microsoft.DataFactory/factories |Sí | No |[Factorías de datos V2](../../azure-monitor/platform/metrics-supported.md#microsoftdatafactoryfactories)|
|Microsoft.DataShare/accounts |No| Sí|
|Microsoft.DBforMySQL/servers |N/D| No |[DB para MySQL](../../azure-monitor/platform/metrics-supported.md#microsoftdbformysqlservers)|
|Microsoft.DBforPostgreSQL/servers |N/D | No | [DB para PostgreSQL](../../azure-monitor/platform/metrics-supported.md#microsoftdbforpostgresqlservers)|
|Microsoft.Devices/IotHubs | N/D | No |[Métricas de IoT Hub](../../azure-monitor/platform/metrics-supported.md#microsoftdevicesiothubs)|
|Microsoft.Devices/provisioningServices| Sí | No |[Métricas de DPS](../../azure-monitor/platform/metrics-supported.md#microsoftdevicesprovisioningservices)|
|Microsoft.EventGrid/domains|No|Sí| |
|Microsoft.EventGrid/topics |Sí | No |[Temas de Event Grid](../../azure-monitor/platform/metrics-supported.md#microsofteventgridtopics)|
|Microsoft.EventHub/clusters |Sí| No |[Clústeres de Event Hubs](../../azure-monitor/platform/metrics-supported.md#microsofteventhubclusters)|
|Microsoft.EventHub/namespaces |Sí| No |[Event Hubs](../../azure-monitor/platform/metrics-supported.md#microsofteventhubnamespaces)|
|Microsoft.KeyVault/vaults| No |No |[Almacenes](../../azure-monitor/platform/metrics-supported.md#microsoftkeyvaultvaults)|
|Microsoft.Logic/workflows |N/D | No |[Logic Apps](../../azure-monitor/platform/metrics-supported.md#microsoftlogicworkflows) |
|Microsoft.MachineLearningServices/workspaces|Sí| No | [Machine Learning](../../azure-monitor/platform/metrics-supported.md#microsoftmachinelearningservicesworkspaces) |
|Microsoft.NetApp/netAppAccounts/capacityPools |Sí| No | [Grupos de capacidad de Azure NetApp](../../azure-monitor/platform/metrics-supported.md#microsoftnetappnetappaccountscapacitypools) |
|Microsoft.NetApp/netAppAccounts/capacityPools/volumes |Sí| No | [Volúmenes de Azure NetApp](../../azure-monitor/platform/metrics-supported.md#microsoftnetappnetappaccountscapacitypoolsvolumes) |
|Microsoft.Network/applicationGateways|N/D| No |  |
|Microsoft.Network/dnsZones | N/D| No | [Zonas DNS](../../azure-monitor/platform/metrics-supported.md#microsoftnetworkdnszones) |
|Microsoft.Network/expressRouteCircuits | N/D | No |[Circuitos ExpressRoute](../../azure-monitor/platform/metrics-supported.md#microsoftnetworkexpressroutecircuits) |
|Microsoft.Network/loadBalancers (solo para SKU estándar)| Sí| No | [Equilibradores de carga](../../azure-monitor/platform/metrics-supported.md#microsoftnetworkloadbalancers) |
|Microsoft.Network/natGateways|No|Sí|
|Microsoft.Network/privateEndpoints|No|Sí|
|Microsoft.Network/privateLinkServices|No|Sí|
|Microsoft.Network/publicipaddresses |N/D | No |[Direcciones IP públicas](../../azure-monitor/platform/metrics-supported.md#microsoftnetworkpublicipaddresses)|
|Microsoft.Network/trafficManagerProfiles | Sí | No | [Perfiles de Traffic Manager](../../azure-monitor/platform/metrics-supported.md#microsoftnetworktrafficmanagerprofiles) |
|Microsoft.OperationalInsights/workspaces| Sí | No | [Áreas de trabajo de Log Analytics](../../azure-monitor/platform/metrics-supported.md#microsoftoperationalinsightsworkspaces)|
|Microsoft.Relay/namespaces | Sí | No | [Retransmisiones](../../azure-monitor/platform/metrics-supported.md#microsoftrelaynamespaces)|
|Microsoft.Peering/peeringServices|No|Sí|
|Microsoft.PowerBIDedicated/capacities | N/D | No | [Capacidades](../../azure-monitor/platform/metrics-supported.md#microsoftpowerbidedicatedcapacities)|
|Microsoft.Search/searchServices |N/D|No | [Servicios de búsqueda](../../azure-monitor/platform/metrics-supported.md#microsoftsearchsearchservices)|
|Microsoft.ServiceBus/namespaces |Sí| No |[Service Bus](../../azure-monitor/platform/metrics-supported.md#microsoftservicebusnamespaces)|
|Microsoft.Sql/servers/elasticPools |    No | Sí |
|Microsoft.Sql/servers/databases    | No | Sí |
|Microsoft.Storage/storageAccounts |Sí | No | [Cuentas de almacenamiento](../../azure-monitor/platform/metrics-supported.md#microsoftstoragestorageaccounts)|
|Microsoft.Storage/storageAccounts/services | Sí| No | [Servicios de blob](../../azure-monitor/platform/metrics-supported.md#microsoftstoragestorageaccountsblobservices), [servicios de archivo](../../azure-monitor/platform/metrics-supported.md#microsoftstoragestorageaccountsfileservices), [servicios de cola](../../azure-monitor/platform/metrics-supported.md#microsoftstoragestorageaccountsqueueservices) y [servicios de tabla](../../azure-monitor/platform/metrics-supported.md#microsoftstoragestorageaccountstableservices)|
|Microsoft.StreamAnalytics/streamingjobs |N/D| No | [Stream Analytics](../../azure-monitor/platform/metrics-supported.md#microsoftstreamanalyticsstreamingjobs)|
|Microsoft.Microsoft.VMWareCloudSimple/virtualMachines |Sí|No |[Máquinas virtuales de CloudSimple](../../azure-monitor/platform/metrics-supported.md#microsoftvmwarecloudsimplevirtualmachines)|
|Microsoft.Web/hostingEnvironments/multiRolePools | Sí | No | [Grupos de varios roles de App Service Environment](../../azure-monitor/platform/metrics-supported.md#microsoftwebhostingenvironmentsmultirolepools)|
|Microsoft.Web/hostingEnvironments/workerPools | Sí | No | [Grupos de trabajo de App Service Environment](../../azure-monitor/platform/metrics-supported.md#microsoftwebhostingenvironmentsworkerpools)|
|Microsoft.Web/serverfarms | Sí | No | [Planes de App Service](../../azure-monitor/platform/metrics-supported.md#microsoftwebserverfarms)|
|Microsoft.Web/sites | Sí | No | [App Services](../../azure-monitor/platform/metrics-supported.md#microsoftwebsites-excluding-functions) y [Functions](../../azure-monitor/platform/metrics-supported.md#microsoftwebsites-functions)|
|Microsoft.Web/sites/slots | Sí | No | [Ranuras de App Service](../../azure-monitor/platform/metrics-supported.md#microsoftwebsitesslots)|

## <a name="payload-schema"></a>Esquema de carga

> [!NOTE]
> También puede usar el [esquema de alerta común](https://aka.ms/commonAlertSchemaDocs), que le ofrece la ventaja de tener una carga útil de alerta única y extensible en todos los servicios de alerta Azure Monitor, para las integraciones de su webhook. [Obtenga más información sobre las definiciones de esquemas de alertas comunes.](https://aka.ms/commonAlertSchemaDefinitions)


La operación POST contiene el esquema y la carga útil de JSON siguientes para todas las nuevas métricas cuando se usa un [grupo de acciones](../../azure-monitor/platform/action-groups.md) configurado correctamente:

```json
{
  "schemaId": "AzureMonitorMetricAlert",
  "data": {
    "version": "2.0",
    "status": "Activated",
    "context": {
      "timestamp": "2018-02-28T10:44:10.1714014Z",
      "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/Contoso/providers/microsoft.insights/metricAlerts/StorageCheck",
      "name": "StorageCheck",
      "description": "",
      "conditionType": "SingleResourceMultipleMetricCriteria",
      "severity":"3",
      "condition": {
        "windowSize": "PT5M",
        "allOf": [
          {
            "metricName": "Transactions",
            "metricNamespace":"microsoft.storage/storageAccounts",
            "dimensions": [
              {
                "name": "AccountResourceId",
                "value": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/Contoso/providers/Microsoft.Storage/storageAccounts/diag500"
              },
              {
                "name": "GeoType",
                "value": "Primary"
              }
            ],
            "operator": "GreaterThan",
            "threshold": "0",
            "timeAggregation": "PT5M",
            "metricValue": 1
          }
        ]
      },
      "subscriptionId": "00000000-0000-0000-0000-000000000000",
      "resourceGroupName": "Contoso",
      "resourceName": "diag500",
      "resourceType": "Microsoft.Storage/storageAccounts",
      "resourceId": "/subscriptions/1e3ff1c0-771a-4119-a03b-be82a51e232d/resourceGroups/Contoso/providers/Microsoft.Storage/storageAccounts/diag500",
      "portalLink": "https://portal.azure.com/#resource//subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/Contoso/providers/Microsoft.Storage/storageAccounts/diag500"
    },
    "properties": {
      "key1": "value1",
      "key2": "value2"
    }
  }
}
```

## <a name="next-steps"></a>Pasos siguientes

* Más información sobre la nueva [experiencia de alertas](../../azure-monitor/platform/alerts-overview.md).
* Más información sobre las [alertas de registro en Azure](../../azure-monitor/platform/alerts-unified-log.md).
* Más información sobre las [alertas en Azure](../../azure-monitor/platform/alerts-overview.md).
