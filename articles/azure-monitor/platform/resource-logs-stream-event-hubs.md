---
title: Transmisión de registros de plataforma de Azure a un centro de eventos
description: Aprenda a transmitir los registros de recurso de Azure a un centro de eventos para enviar datos a sistemas externos, como SIEM de terceros y otras soluciones de análisis de registros.
author: bwren
services: azure-monitor
ms.topic: conceptual
ms.date: 12/15/2019
ms.author: bwren
ms.subservice: ''
ms.openlocfilehash: 72341b6da0068ba4b7e3f53b08e6015cafb70f09
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "77658921"
---
# <a name="stream-azure-platform-logs-to-azure-event-hubs"></a>Transmisión de registros de plataforma de Azure a Azure Event Hubs
Los [registros de plataforma](platform-logs-overview.md) en Azure, incluidos los registros de recurso y los registros de actividad de Azure, proporcionan información detallada de diagnóstico y auditoría para los recursos de Azure y la plataforma de Azure de la que dependen.  En este artículo se describe la transmisión de los registros de plataforma a Event Hubs para enviar datos a sistemas externos, como SIEM de terceros y otras soluciones de análisis de registros.


## <a name="what-you-can-do-with-platform-logs-sent-to-an-event-hub"></a>Qué se puede hacer con los registros de plataforma enviados a un centro de eventos
Transmita los registros de plataforma de Azure a centros de eventos para proporcionar la siguiente funcionalidad:

* **Transmisión de registros a sistemas de registro y telemetría de terceros**: transmita todos los registros de plataforma a un centro de eventos único para canalizar datos de registro en una herramienta SIEM o de análisis de registros de terceros.
  
* **Creación de una plataforma de registro y telemetría personalizada**: la naturaleza altamente escalable de publicación y suscripción de los centros de eventos otorga la flexibilidad necesaria para ingerir registros de plataforma en una plataforma de telemetría personalizada. Consulte [Designing and Sizing a Global Scale Telemetry Platform on Azure Event Hubs](https://azure.microsoft.com/documentation/videos/build-2015-designing-and-sizing-a-global-scale-telemetry-platform-on-azure-event-Hubs/) (Diseño y cambio de tamaño de una plataforma de telemetría a escala global en Azure Event Hubs) para más información.

* **Visualización del estado del servicio mediante la transmisión de datos a Power BI**: con Event Hubs, Stream Analytics y Power BI, transforme los datos de diagnóstico en información casi en tiempo real de los servicios de Azure. Consulte [Stream Analytics y Power BI: panel de análisis en tiempo real de flujo de datos](../../stream-analytics/stream-analytics-power-bi-dashboard.md) para detalles sobre esta solución.

    El siguiente código SQL es una consulta de Stream Analytics de ejemplo que puede utilizar para analizar todos los datos de registro en una tabla de Power BI:
    
    ```sql
    SELECT
    records.ArrayValue.[Properties you want to track]
    INTO
    [OutputSourceName – the Power BI source]
    FROM
    [InputSourceName] AS e
    CROSS APPLY GetArrayElements(e.records) AS records
    ```

## <a name="prerequisites"></a>Prerrequisitos
Si no lo tiene, [cree un centro de eventos](../../event-hubs/event-hubs-create.md). Si ya tiene una configuración de diagnóstico que usa este espacio de nombres de Event Hubs, se volverá a usar el centro de eventos.

La directiva de acceso compartido del espacio de nombres define los permisos que tiene el mecanismo de transmisión. Para transmitir a Event Hubs, se necesitan permisos de administración, envío y escucha. Puede crear o modificar directivas de acceso compartido en la pestaña Configurar de Azure Portal para su espacio de nombres de Event Hubs.

Para actualizar la configuración de diagnóstico para incluir la transmisión, debe tener el permiso ListKey sobre esa regla de autorización de Event Hubs. El espacio de nombres de Event Hubs no tiene que estar necesariamente en la misma suscripción que la suscripción que emite los registros, siempre que el usuario que configure el valor tenga el acceso RBAC adecuado a ambas suscripciones y estas se encuentren en el mismo inquilino de AAD.

## <a name="create-a-diagnostic-setting"></a>Creación de una configuración de diagnóstico
Envíe los registros de plataforma a un centro de eventos y otros destinos mediante la creación de una configuración de diagnóstico para un recurso de Azure. Consulte [Creación de una configuración de diagnóstico para recopilar registros y métricas en Azure](diagnostic-settings.md) para más información.

## <a name="collect-data-from-compute-resources"></a>Recopilación de datos desde recursos de proceso
La configuración de diagnóstico recopilará los registros de recursos para los recursos de proceso de Azure, como cualquier otro recurso, pero no su sistema operativo invitado o sus cargas de trabajo. Para recopilar estos datos, instale el [agente de Log Analytics](log-analytics-agent.md). 


## <a name="consuming-log-data-from-event-hubs"></a>Consumo de los datos de registro desde los centros de eventos
Los registros de plataforma de los centros de eventos se consumen en formato JSON con los elementos de la tabla siguiente.

| Nombre del elemento | Descripción |
| --- | --- |
| records |Matriz de todos los eventos de registro de esta carga. |
| time |Hora a la que se produjo el error. |
| category |Categoría de registro para este evento. |
| resourceId |Id. de recurso del recurso que generó este evento. |
| operationName |Nombre de la operación. |
| level |Opcional. Indica el nivel de registro de eventos. |
| properties |Propiedades del evento. Estas varían para cada servicio de Azure, tal y como se describe en [](). |


A continuación se muestran datos de salida de ejemplo de Event Hubs para un registro de recursos:

```json
{
    "records": [
        {
            "time": "2016-07-15T18:00:22.6235064Z",
            "workflowId": "/SUBSCRIPTIONS/DF602C9C-7AA0-407D-A6FB-EB20C8BD1192/RESOURCEGROUPS/JOHNKEMTEST/PROVIDERS/MICROSOFT.LOGIC/WORKFLOWS/JOHNKEMTESTLA",
            "resourceId": "/SUBSCRIPTIONS/DF602C9C-7AA0-407D-A6FB-EB20C8BD1192/RESOURCEGROUPS/JOHNKEMTEST/PROVIDERS/MICROSOFT.LOGIC/WORKFLOWS/JOHNKEMTESTLA/RUNS/08587330013509921957/ACTIONS/SEND_EMAIL",
            "category": "WorkflowRuntime",
            "level": "Error",
            "operationName": "Microsoft.Logic/workflows/workflowActionCompleted",
            "properties": {
                "$schema": "2016-04-01-preview",
                "startTime": "2016-07-15T17:58:55.048482Z",
                "endTime": "2016-07-15T18:00:22.4109204Z",
                "status": "Failed",
                "code": "BadGateway",
                "resource": {
                    "subscriptionId": "df602c9c-7aa0-407d-a6fb-eb20c8bd1192",
                    "resourceGroupName": "JohnKemTest",
                    "workflowId": "243aac67fe904cf195d4a28297803785",
                    "workflowName": "JohnKemTestLA",
                    "runId": "08587330013509921957",
                    "location": "westus",
                    "actionName": "Send_email"
                },
                "correlation": {
                    "actionTrackingId": "29a9862f-969b-4c70-90c4-dfbdc814e413",
                    "clientTrackingId": "08587330013509921958"
                }
            }
        },
        {
            "time": "2016-07-15T18:01:15.7532989Z",
            "workflowId": "/SUBSCRIPTIONS/DF602C9C-7AA0-407D-A6FB-EB20C8BD1192/RESOURCEGROUPS/JOHNKEMTEST/PROVIDERS/MICROSOFT.LOGIC/WORKFLOWS/JOHNKEMTESTLA",
            "resourceId": "/SUBSCRIPTIONS/DF602C9C-7AA0-407D-A6FB-EB20C8BD1192/RESOURCEGROUPS/JOHNKEMTEST/PROVIDERS/MICROSOFT.LOGIC/WORKFLOWS/JOHNKEMTESTLA/RUNS/08587330012106702630/ACTIONS/SEND_EMAIL",
            "category": "WorkflowRuntime",
            "level": "Information",
            "operationName": "Microsoft.Logic/workflows/workflowActionStarted",
            "properties": {
                "$schema": "2016-04-01-preview",
                "startTime": "2016-07-15T18:01:15.5828115Z",
                "status": "Running",
                "resource": {
                    "subscriptionId": "df602c9c-7aa0-407d-a6fb-eb20c8bd1192",
                    "resourceGroupName": "JohnKemTest",
                    "workflowId": "243aac67fe904cf195d4a28297803785",
                    "workflowName": "JohnKemTestLA",
                    "runId": "08587330012106702630",
                    "location": "westus",
                    "actionName": "Send_email"
                },
                "correlation": {
                    "actionTrackingId": "042fb72c-7bd4-439e-89eb-3cf4409d429e",
                    "clientTrackingId": "08587330012106702632"
                }
            }
        }
    ]
}
```



## <a name="next-steps"></a>Pasos siguientes

* [Más información sobre los registros de recurso](platform-logs-overview.md).
* [Creación de una configuración de diagnóstico para recopilar registros y métricas en Azure](diagnostic-settings.md).
* [Transmisión de registros de Azure Active Directory con Azure Monitor](../../active-directory/reports-monitoring/tutorial-azure-monitor-stream-logs-to-event-hub.md).
* [Introducción a Event Hubs](../../event-hubs/event-hubs-dotnet-standard-getstarted-send.md).

