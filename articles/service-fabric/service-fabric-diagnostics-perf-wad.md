---
title: Supervisión del rendimiento con Windows Azure Diagnostics
description: Utilice Windows Azure Diagnostics para recopilar contadores de rendimiento para los clústeres de Azure Service Fabric.
author: srrengar
ms.topic: conceptual
ms.date: 11/21/2018
ms.author: srrengar
ms.openlocfilehash: c2114f5392da788bb440589e69e704a148731e02
ms.sourcegitcommit: dabd9eb9925308d3c2404c3957e5c921408089da
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/11/2020
ms.locfileid: "86258622"
---
# <a name="performance-monitoring-with-the-windows-azure-diagnostics-extension"></a>Supervisión del rendimiento con la extensión de Microsoft Azure Diagnostics

Este documento incluye los pasos necesarios para configurar los contadores de rendimiento a través de la extensión de Microsoft Azure Diagnostics para los clústeres de Windows. En el caso de los clústeres de Linux, configure el [agente de Log Analytics](service-fabric-diagnostics-oms-agent.md) para recopilar contadores de rendimiento para los nodos. 

 > [!NOTE]
> Se debe implementar la extensión de Microsoft Azure Diagnostics en el clúster para que estos pasos funcionen. Si no está configurado, diríjase a [Recopilación y agregación de eventos con Azure Diagnostics de Windows](service-fabric-diagnostics-event-aggregation-wad.md).  


[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="collect-performance-counters-via-the-wadcfg"></a>Recopilación de contadores de rendimiento a través de WadCfg

Para recopilar contadores de rendimiento a través de WAD, debe modificar la configuración correctamente en la plantilla de Resource Manager del clúster. Siga estos pasos para agregar un contador de rendimiento que desee que recopile la plantilla y ejecute una actualización de recursos de Resource Manager.

1. Busque la configuración de Microsoft Azure Diagnostics en la plantilla del clúster: buscar `WadCfg`. Se agregarán contadores de rendimiento que recopilar en `DiagnosticMonitorConfiguration`.

2. Establezca la configuración para recopilar los contadores de rendimiento mediante la adición de la sección siguiente a `DiagnosticMonitorConfiguration`. 

    ```json
    "PerformanceCounters": {
        "scheduledTransferPeriod": "PT1M",
        "PerformanceCounterConfiguration": []
    }
    ```

    `scheduledTransferPeriod` define la frecuencia con la que se transfieren los valores de los contadores que se recopilan a la tabla de almacenamiento de Azure y a cualquier receptor configurado. 

3. Agregue los contadores de rendimiento que desee recopilar a `PerformanceCounterConfiguration`, que se declaró en el paso anterior. Cada contador que desee recopilar se define con `counterSpecifier`, `sampleRate`, `unit`, `annotation` y cualquier `sinks` relevante.

Este es un ejemplo de una configuración con el contador para la *Tiempo total de procesador* (la cantidad de tiempo que la CPU estuvo en uso para las operaciones de procesamiento) y las *Invocaciones por segundo del método de actor de Service Fabric*, uno de los contadores de rendimiento personalizados de Service Fabric. Consulte [Lista de eventos y contadores de rendimiento](service-fabric-reliable-actors-diagnostics.md#list-of-events-and-performance-counters) y [Lista de contadores de rendimiento](service-fabric-reliable-serviceremoting-diagnostics.md#list-of-performance-counters) para obtener una lista completa de los contadores de rendimiento personalizados de Service Fabric.

 ```json
 "WadCfg": {
         "DiagnosticMonitorConfiguration": {
           "overallQuotaInMB": "50000",
           "EtwProviders": {
             "EtwEventSourceProviderConfiguration": [
               {
                 "provider": "Microsoft-ServiceFabric-Actors",
                 "scheduledTransferKeywordFilter": "1",
                 "scheduledTransferPeriod": "PT5M",
                 "DefaultEvents": {
                   "eventDestination": "ServiceFabricReliableActorEventTable"
                 }
               },
               {
                 "provider": "Microsoft-ServiceFabric-Services",
                 "scheduledTransferPeriod": "PT5M",
                 "DefaultEvents": {
                   "eventDestination": "ServiceFabricReliableServiceEventTable"
                 }
               }
             ],
             "EtwManifestProviderConfiguration": [
               {
                 "provider": "cbd93bc2-71e5-4566-b3a7-595d8eeca6e8",
                 "scheduledTransferLogLevelFilter": "Information",
                 "scheduledTransferKeywordFilter": "4611686018427387904",
                 "scheduledTransferPeriod": "PT5M",
                 "DefaultEvents": {
                   "eventDestination": "ServiceFabricSystemEventTable"
                 }
               }
             ]
           },
           "PerformanceCounters": {
                 "scheduledTransferPeriod": "PT1M",
                 "PerformanceCounterConfiguration": [
                     {
                         "counterSpecifier": "\\Processor(_Total)\\% Processor Time",
                         "sampleRate": "PT1M",
                         "unit": "Percent",
                         "annotation": [
                         ],
                         "sinks": ""
                     },
                     {
                         "counterSpecifier": "\\Service Fabric Actor Method(*)\\Invocations/Sec",
                         "sampleRate": "PT1M",
                     }
                 ]
             }
         }
       },
  ```

 La velocidad de muestreo para el contador se puede modificar según sus necesidades. El formato es `PT<time><unit>`, por lo que si desea que el contador realice la recopilación cada segundo, debe establecer `"sampleRate": "PT15S"`.

 También puede usar variables en la plantilla de ARM para recopilar una matriz de contadores de rendimiento, que puede resultar útil al recopilar contadores de rendimiento por proceso. En el ejemplo siguiente, se recopilan el tiempo de procesador y el tiempo del recolector de elementos no utilizados por proceso y, a continuación, 2 contadores de rendimiento en los propios nodos, todo ello usando variables. 

 ```json
"variables": {
  "copy": [
      {
        "name": "processorTimeCounters",
        "count": "[length(parameters('monitoredProcesses'))]",
        "input": {
          "counterSpecifier": "\\Process([parameters('monitoredProcesses')[copyIndex('processorTimeCounters')]])\\% Processor Time",
          "sampleRate": "PT1M",
          "unit": "Percent",
          "sinks": "applicationInsights",
          "annotation": [
            {
              "displayName": "[concat(parameters('monitoredProcesses')[copyIndex('processorTimeCounters')],' Processor Time')]",
              "locale": "en-us"
            }
          ]
        }
      },
      {
        "name": "gcTimeCounters",
        "count": "[length(parameters('monitoredProcesses'))]",
        "input": {
          "counterSpecifier": "\\.NET CLR Memory([parameters('monitoredProcesses')[copyIndex('gcTimeCounters')]])\\% Time in GC",
          "sampleRate": "PT1M",
          "unit": "Percent",
          "sinks": "applicationInsights",
          "annotation": [
            {
              "displayName": "[concat(parameters('monitoredProcesses')[copyIndex('gcTimeCounters')],' Time in GC')]",
              "locale": "en-us"
            }
          ]
        }
      }
    ],
    "machineCounters": [
      {
        "counterSpecifier": "\\Memory\\Available Bytes",
        "sampleRate": "PT1M",
        "unit": "KB",
        "sinks": "applicationInsights",
        "annotation": [
          {
            "displayName": "Memory Available Kb",
            "locale": "en-us"
          }
        ]
      },
      {
        "counterSpecifier": "\\Memory\\% Committed Bytes In Use",
        "sampleRate": "PT15S",
        "unit": "percent",
        "annotation": [
          {
            "displayName": "Memory usage",
            "locale": "en-us"
          }
        ]
      }
    ]
  }
....
"WadCfg": {
    "DiagnosticMonitorConfiguration": {
      "overallQuotaInMB": "50000",
      "Metrics": {
        "metricAggregation": [
          {
            "scheduledTransferPeriod": "PT1M"
          }
        ],
        "resourceId": "[resourceId('Microsoft.Compute/virtualMachineScaleSets', variables('vmNodeTypeApp2Name'))]"
      },
      "PerformanceCounters": {
        "scheduledTransferPeriod": "PT1M",
        "PerformanceCounterConfiguration": "[concat(variables ('processorTimeCounters'), variables('gcTimeCounters'),  variables('machineCounters'))]"
      },
....
```

1. Una vez que haya agregado los contadores de rendimiento adecuados que se deben recopilar, debe actualizar el recurso de clúster de manera que estos cambios se reflejan en el clúster de ejecución. Guarde su `template.json` modificado y abra PowerShell. Puede actualizar el clúster con `New-AzResourceGroupDeployment`. La llamada requiere el nombre del grupo de recursos, el archivo de plantilla actualizado y el archivo de parámetro, y solicita a Resource Manager realizar los cambios adecuados en los recursos que se ha actualizado. Una vez que haya iniciado sesión en su cuenta y esté en la suscripción correcta, utilice el siguiente comando para ejecutar la actualización:

    ```sh
    New-AzResourceGroupDeployment -ResourceGroupName <ResourceGroup> -TemplateFile <PathToTemplateFile> -TemplateParameterFile <PathToParametersFile> -Verbose
    ```

1. Una vez que la actualización finalice la implementación (tarda entre 15 y 45 minutos en función de si se trata de la primera implementación y del tamaño del grupo de recursos), WAD debería recopilar los contadores de rendimiento y enviarlos a una tabla denominada WADPerformanceCountersTable en la cuenta de almacenamiento asociada al clúster. Para ver los contadores de rendimiento en Application Insights, [agregue el receptor de AI a la plantilla de Resource Manager](service-fabric-diagnostics-event-aggregation-wad.md#add-the-application-insights-sink-to-the-resource-manager-template).

## <a name="next-steps"></a>Pasos siguientes
* Recopile más contadores de rendimiento para el clúster. Vea [Métricas de rendimiento](service-fabric-diagnostics-event-generation-perf.md) para obtener una lista de contadores que debe recopilar.
* [Use la supervisión y diagnóstico con una máquina virtual Windows y plantillas de Azure Resource Manager](../virtual-machines/extensions/diagnostics-template.md) para realizar más modificaciones en `WadCfg`, incluida la configuración de cuentas de almacenamiento adicionales a la que enviar datos de diagnóstico.
* Visite el [generador de WadCfg](https://azure.github.io/azure-diagnostics-tools/config-builder/) para compilar una plantilla desde cero y asegúrese de que la sintaxis sea correcta. (https://azure.github.io/azure-diagnostics-tools/config-builder/) para compilar una plantilla desde cero y asegúrese de que la sintaxis sea correcta.
