---
title: Supervisión de factorías de datos mediante Azure Monitor | Microsoft Docs
description: Aprenda a utilizar Azure Monitor para supervisar las canalizaciones de Azure Data Factory mediante la habilitación de registros de diagnóstico con la información de Data Factory.
services: data-factory
documentationcenter: ''
author: djpmsft
ms.author: daperlov
manager: jroth
ms.reviewer: maghan
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.date: 12/11/2018
ms.openlocfilehash: 5753336eeef115038de4eb0b5ade0651b1fa293e
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/16/2020
ms.locfileid: "81419466"
---
# <a name="alert-and-monitor-data-factories-by-using-azure-monitor"></a>Alerta y supervisión de factorías de datos mediante Azure Monitor

[!INCLUDE[appliesto-adf-xxx-md](includes/appliesto-adf-xxx-md.md)]

Las aplicaciones en la nube son complejas y tienen muchas partes en movimiento. Los supervisores proporcionan datos para garantizar que las aplicaciones permanecen en funcionamiento en un estado correcto. Los supervisores también ayudan a evitar posibles problemas y a solucionar los problemas anteriores.

Puede usar datos de supervisión para extraer conclusiones detalladas sobre las aplicaciones. Este conocimiento le ayuda a mejorar el rendimiento y el mantenimiento de las aplicaciones. También le ayuda a automatizar acciones que, de otro modo, requieren intervención manual.

Azure Monitor ofrece métricas y registros de las infraestructuras a nivel básico para la mayoría de los servicios de Azure. Un recurso emite registros de diagnóstico de Azure que proporcionan datos exhaustivos y frecuentes acerca del funcionamiento de ese recurso. Y Azure Data Factory escribe registros de diagnóstico en Monitor.

Para más información, consulte [Introducción a Azure Monitor](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-azure-monitor).

## <a name="keeping-azure-data-factory-data"></a>Mantenimiento de datos de Azure Data Factory

Data Factory solo almacena los datos de ejecución de canalización durante 45 días. Use Monitor si desea conservar los datos durante más tiempo. Con Monitor, puede enrutar los registros de diagnóstico para su análisis. También puede mantenerlos en una cuenta de almacenamiento para que tenga información de la factoría para la duración seleccionada.

## <a name="diagnostic-logs"></a>Registros de diagnóstico

* Guarde los registro de diagnóstico en una cuenta de almacenamiento para auditarlos o para inspeccionarlos manualmente. Puede usar la configuración de diagnóstico para especificar el tiempo de retención en días.
* Transmita los registros a Azure Event Hubs. Los registros se convierten en la entrada de un servicio del asociado o en una solución de análisis personalizada como Power BI.
* Analice los registros con Log Analytics.

Puede usar una cuenta de almacenamiento o un espacio de nombres de centro de eventos que no esté en la misma suscripción que el recurso que emite los registros. El usuario que configura los ajustes debe tener el control de acceso basado en rol (RBAC) adecuado a ambas suscripciones.

## <a name="set-up-diagnostic-logs"></a>Configuración de registros de diagnósticos

### <a name="diagnostic-settings"></a>Configuración de diagnóstico

Use la configuración de diagnóstico para configurar los registros de diagnóstico de los recursos que no son de proceso. La configuración de un control de recursos tiene las siguientes características:

* Especifican dónde se envían los registros de diagnóstico. Entre los ejemplos se incluyen una cuenta de almacenamiento de Azure, un centro de eventos de Azure o registros de Monitor.
* Especifican las categorías de registro que se envían.
* Especifican cuánto tiempo se debe conservar cada categoría de registro en una cuenta de almacenamiento.
* Una retención de cero días significa que los registros se conservan de forma indefinida. De lo contrario, el valor puede ser cualquier número de días comprendido entre 1 y 2 147 483 647.
* Si se establecen directivas de retención, pero el almacenamiento de registros en una cuenta de almacenamiento está deshabilitado, las directivas de retención no surten ningún efecto. Por ejemplo, esta condición puede ocurrir cuando solo se seleccionan las opciones de registros de Event Hubs o de Monitor.
* Las directivas de retención se aplican por día. El límite entre los días se produce a la medianoche de la hora universal coordinada (UTC). Al final del día, se eliminan los registros de los días que superan la directiva de retención. Por ejemplo, si tuviera una directiva de retención de un día, se eliminarían los registros de anteayer al principio del día de hoy.

### <a name="enable-diagnostic-logs-via-the-azure-monitor-rest-api"></a>Habilitación de registros de diagnóstico mediante la API REST de Azure Monitor

#### <a name="create-or-update-a-diagnostics-setting-in-the-monitor-rest-api"></a>Creación o actualización de una configuración de diagnóstico en la API REST de Monitor

##### <a name="request"></a>Solicitud

```
PUT
https://management.azure.com/{resource-id}/providers/microsoft.insights/diagnosticSettings/service?api-version={api-version}
```

##### <a name="headers"></a>encabezados

* Reemplace `{api-version}` por `2016-09-01`.
* Reemplace `{resource-id}` por el identificador del recurso para el que quiere editar la configuración de diagnóstico. Para más información, consulte el artículo sobre el [uso de grupos de recursos para administrar los recursos de Azure](../azure-resource-manager/management/manage-resource-groups-portal.md).
* Establezca el encabezado `Content-Type` en `application/json`.
* Establezca el encabezado de autorización en un token web de JSON que se obtiene de Azure Active Directory (AAD). Par más información, consulte [Solicitudes de autenticación](../active-directory/develop/authentication-scenarios.md).

##### <a name="body"></a>Body

```json
{
    "properties": {
        "storageAccountId": "/subscriptions/<subID>/resourceGroups/<resourceGroupName>/providers/Microsoft.Storage/storageAccounts/<storageAccountName>",
        "serviceBusRuleId": "/subscriptions/<subID>/resourceGroups/<resourceGroupName>/providers/Microsoft.EventHub/namespaces/<eventHubName>/authorizationrules/RootManageSharedAccessKey",
        "workspaceId": "/subscriptions/<subID>/resourceGroups/<resourceGroupName>/providers/Microsoft.OperationalInsights/workspaces/<LogAnalyticsName>",
        "metrics": [
        ],
        "logs": [
                {
                    "category": "PipelineRuns",
                    "enabled": true,
                    "retentionPolicy": {
                        "enabled": false,
                        "days": 0
                    }
                },
                {
                    "category": "TriggerRuns",
                    "enabled": true,
                    "retentionPolicy": {
                        "enabled": false,
                        "days": 0
                    }
                },
                {
                    "category": "ActivityRuns",
                    "enabled": true,
                    "retentionPolicy": {
                        "enabled": false,
                        "days": 0
                    }
                }
            ]
    },
    "location": ""
}
```

| Propiedad | Tipo | Descripción |
| --- | --- | --- |
| **storageAccountId** |String | El identificador de recurso de la cuenta de almacenamiento en la que quiere enviar los registros de diagnóstico. |
| **serviceBusRuleId** |String | El identificador de regla de Service Bus para el espacio de nombres de Service Bus donde quiere tener los centros de eventos creados para la transmisión de los registros de diagnóstico. El identificador de la regla tiene el formato `{service bus resource ID}/authorizationrules/{key name}`.|
| **workspaceId** | Tipo complejo | Una matriz de intervalos de agregación de métricas y sus directivas de retención. El valor de esta propiedad está vacío. |
|**métricas**| Valores de parámetros de la ejecución de canalización que se pasan a la canalización invocada| Objeto JSON que asigna nombres de parámetro a los valores de argumento. |
| **logs**| Tipo complejo| El nombre de una categoría de registro de diagnóstico para un tipo de recurso. Para obtener la lista de categorías de registro de diagnóstico para un recurso, realice una operación de configuración de diagnóstico GET. |
| **category**| String| Una matriz de las categorías de registro y sus directivas de retención. |
| **timeGrain** | String | La granularidad de las métricas que se capturan en formato de duración ISO 8601. El valor de propiedad debe ser `PT1M`, que especifica un minuto. |
| **enabled**| Boolean | Especifica si la colección de la categoría de métrica o registro está habilitada para este recurso. |
| **retentionPolicy**| Tipo complejo| Describe la directiva de retención para una categoría de métrica o registro. Esta propiedad se usa solo para las cuentas de almacenamiento. |
|**days**| Int| El número de días para retener las métricas o registros. Si el valor de propiedad es 0, los registros se mantienen indefinidamente. Esta propiedad se usa solo para las cuentas de almacenamiento. |

##### <a name="response"></a>Response

200 CORRECTO.


```json
{
    "id": "/subscriptions/<subID>/resourcegroups/adf/providers/microsoft.datafactory/factories/shloadobetest2/providers/microsoft.insights/diagnosticSettings/service",
    "type": null,
    "name": "service",
    "location": null,
    "kind": null,
    "tags": null,
    "properties": {
        "storageAccountId": "/subscriptions/<subID>/resourceGroups/<resourceGroupName>//providers/Microsoft.Storage/storageAccounts/<storageAccountName>",
        "serviceBusRuleId": "/subscriptions/<subID>/resourceGroups/<resourceGroupName>//providers/Microsoft.EventHub/namespaces/<eventHubName>/authorizationrules/RootManageSharedAccessKey",
        "workspaceId": "/subscriptions/<subID>/resourceGroups/<resourceGroupName>//providers/Microsoft.OperationalInsights/workspaces/<LogAnalyticsName>",
        "eventHubAuthorizationRuleId": null,
        "eventHubName": null,
        "metrics": [],
        "logs": [
            {
                "category": "PipelineRuns",
                "enabled": true,
                "retentionPolicy": {
                    "enabled": false,
                    "days": 0
                }
            },
            {
                "category": "TriggerRuns",
                "enabled": true,
                "retentionPolicy": {
                    "enabled": false,
                    "days": 0
                }
            },
            {
                "category": "ActivityRuns",
                "enabled": true,
                "retentionPolicy": {
                    "enabled": false,
                    "days": 0
                }
            }
        ]
    },
    "identity": null
}
```

#### <a name="get-information-about-diagnostics-settings-in-the-monitor-rest-api"></a>Permite obtener información sobre la configuración de diagnóstico en la API REST de Monitor.

##### <a name="request"></a>Solicitud

```
GET
https://management.azure.com/{resource-id}/providers/microsoft.insights/diagnosticSettings/service?api-version={api-version}
```

##### <a name="headers"></a>encabezados

* Reemplace `{api-version}` por `2016-09-01`.
* Reemplace `{resource-id}` por el identificador del recurso para el que quiere editar la configuración de diagnóstico. Para más información, consulte el artículo sobre el [uso de grupos de recursos para administrar los recursos de Azure](../azure-resource-manager/management/manage-resource-groups-portal.md).
* Establezca el encabezado `Content-Type` en `application/json`.
* Establezca el encabezado de autorización en un token web de JSON que obtuvo de Azure AD. Par más información, consulte [Solicitudes de autenticación](../active-directory/develop/authentication-scenarios.md).

##### <a name="response"></a>Response

200 CORRECTO.

```json
{
    "id": "/subscriptions/<subID>/resourcegroups/adf/providers/microsoft.datafactory/factories/shloadobetest2/providers/microsoft.insights/diagnosticSettings/service",
    "type": null,
    "name": "service",
    "location": null,
    "kind": null,
    "tags": null,
    "properties": {
        "storageAccountId": "/subscriptions/<subID>/resourceGroups/shloprivate/providers/Microsoft.Storage/storageAccounts/azmonlogs",
        "serviceBusRuleId": "/subscriptions/<subID>/resourceGroups/shloprivate/providers/Microsoft.EventHub/namespaces/shloeventhub/authorizationrules/RootManageSharedAccessKey",
        "workspaceId": "/subscriptions/<subID>/resourceGroups/ADF/providers/Microsoft.OperationalInsights/workspaces/mihaipie",
        "eventHubAuthorizationRuleId": null,
        "eventHubName": null,
        "metrics": [],
        "logs": [
            {
                "category": "PipelineRuns",
                "enabled": true,
                "retentionPolicy": {
                    "enabled": false,
                    "days": 0
                }
            },
            {
                "category": "TriggerRuns",
                "enabled": true,
                "retentionPolicy": {
                    "enabled": false,
                    "days": 0
                }
            },
            {
                "category": "ActivityRuns",
                "enabled": true,
                "retentionPolicy": {
                    "enabled": false,
                    "days": 0
                }
            }
        ]
    },
    "identity": null
}

```
Para más información, consulte [Configuración de diagnóstico](https://docs.microsoft.com/rest/api/monitor/diagnosticsettings).

## <a name="schema-of-logs-and-events"></a>Esquema de registros y eventos

### <a name="monitor-schema"></a>Esquema de Monitor

#### <a name="activity-run-log-attributes"></a>Atributos de registro de la ejecución de actividad

```json
{
   "Level": "",
   "correlationId":"",
   "time":"",
   "activityRunId":"",
   "pipelineRunId":"",
   "resourceId":"",
   "category":"ActivityRuns",
   "level":"Informational",
   "operationName":"",
   "pipelineName":"",
   "activityName":"",
   "start":"",
   "end":"",
   "properties":
       {
          "Input": "{
              "source": {
                "type": "BlobSource"
              },
              "sink": {
                "type": "BlobSink"
              }
           }",
          "Output": "{"dataRead":121,"dataWritten":121,"copyDuration":5,
               "throughput":0.0236328132,"errors":[]}",
          "Error": "{
              "errorCode": "null",
              "message": "null",
              "failureType": "null",
              "target": "CopyBlobtoBlob"
        }
    }
}
```

| Propiedad | Tipo | Descripción | Ejemplo |
| --- | --- | --- | --- |
| **Level** |String | El nivel de los registros de diagnóstico. En el caso de los registros de ejecución de actividad, establezca el valor de la propiedad en 4. | `4` |
| **correlationId** |String | El identificador único para realizar el seguimiento de una solicitud determinada. | `319dc6b4-f348-405e-b8d7-aafc77b73e77` |
| **time** | String | La hora del evento en formato UTC de intervalo de tiempo`YYYY-MM-DDTHH:MM:SS.00000Z`. | `2017-06-28T21:00:27.3534352Z` |
|**activityRunId**| String| El identificador de la ejecución de actividad. | `3a171e1f-b36e-4b80-8a54-5625394f4354` |
|**pipelineRunId**| String| El identificador de la ejecución de canalización. | `9f6069d6-e522-4608-9f99-21807bfc3c70` |
|**resourceId**| String | El identificador asociado con el recurso de la fábrica de datos. | `/SUBSCRIPTIONS/<subID>/RESOURCEGROUPS/<resourceGroupName>/PROVIDERS/MICROSOFT.DATAFACTORY/FACTORIES/<dataFactoryName>` |
|**category**| String | La categoría de los registros de diagnóstico. Establezca el valor de propiedad en `ActivityRuns`. | `ActivityRuns` |
|**level**| String | El nivel de los registros de diagnóstico. Establezca el valor de propiedad en `Informational`. | `Informational` |
|**operationName**| String | El nombre de la actividad con su estado. Si la actividad es el latido de inicio, el valor de propiedad es `MyActivity -`. Si la actividad es el latido final, el valor de propiedad es `MyActivity - Succeeded`. | `MyActivity - Succeeded` |
|**pipelineName**| String | El nombre de la canalización. | `MyPipeline` |
|**activityName**| String | El nombre de la actividad. | `MyActivity` |
|**start**| String | La hora de inicio de las ejecuciones de actividad en formato UTC del intervalo de tiempo. | `2017-06-26T20:55:29.5007959Z`|
|**end**| String | La hora de finalización de las ejecuciones de actividad en formato UTC del intervalo de tiempo. Si el registro de diagnóstico muestra que una actividad se ha iniciado pero aún no ha finalizado, el valor de propiedad es `1601-01-01T00:00:00Z`. | `2017-06-26T20:55:29.5007959Z` |

#### <a name="pipeline-run-log-attributes"></a>Atributos de registro de la ejecución de la canalización

```json
{
   "Level": "",
   "correlationId":"",
   "time":"",
   "runId":"",
   "resourceId":"",
   "category":"PipelineRuns",
   "level":"Informational",
   "operationName":"",
   "pipelineName":"",
   "start":"",
   "end":"",
   "status":"",
   "properties":
    {
      "Parameters": {
        "<parameter1Name>": "<parameter1Value>"
      },
      "SystemParameters": {
        "ExecutionStart": "",
        "TriggerId": "",
        "SubscriptionId": ""
      }
    }
}
```

| Propiedad | Tipo | Descripción | Ejemplo |
| --- | --- | --- | --- |
| **Level** |String | El nivel de los registros de diagnóstico. En el caso de los registros de ejecución de actividad, establezca el valor de la propiedad en 4. | `4` |
| **correlationId** |String | El identificador único para realizar el seguimiento de una solicitud determinada. | `319dc6b4-f348-405e-b8d7-aafc77b73e77` |
| **time** | String | La hora del evento en formato UTC de intervalo de tiempo`YYYY-MM-DDTHH:MM:SS.00000Z`. | `2017-06-28T21:00:27.3534352Z` |
|**runId**| String| El identificador de la ejecución de canalización. | `9f6069d6-e522-4608-9f99-21807bfc3c70` |
|**resourceId**| String | El identificador asociado con el recurso de la fábrica de datos. | `/SUBSCRIPTIONS/<subID>/RESOURCEGROUPS/<resourceGroupName>/PROVIDERS/MICROSOFT.DATAFACTORY/FACTORIES/<dataFactoryName>` |
|**category**| String | La categoría de los registros de diagnóstico. Establezca el valor de propiedad en `PipelineRuns`. | `PipelineRuns` |
|**level**| String | El nivel de los registros de diagnóstico. Establezca el valor de propiedad en `Informational`. | `Informational` |
|**operationName**| String | El nombre de la canalización junto con su estado. Una vez finalizada la ejecución de la canalización, el valor de propiedad es `Pipeline - Succeeded`. | `MyPipeline - Succeeded`. |
|**pipelineName**| String | El nombre de la canalización. | `MyPipeline` |
|**start**| String | La hora de inicio de las ejecuciones de actividad en formato UTC del intervalo de tiempo. | `2017-06-26T20:55:29.5007959Z`. |
|**end**| String | La hora de finalización de las ejecuciones de actividad en formato UTC del intervalo de tiempo. Si el registro de diagnóstico muestra que una actividad se ha iniciado pero aún no ha finalizado, el valor de propiedad es `1601-01-01T00:00:00Z`.  | `2017-06-26T20:55:29.5007959Z` |
|**status**| String | El estado final de la ejecución de canalización. Los valores de propiedad posibles son `Succeeded` y `Failed`. | `Succeeded`|

#### <a name="trigger-run-log-attributes"></a>Atributos de registro de la ejecución del desencadenador

```json
{
   "Level": "",
   "correlationId":"",
   "time":"",
   "triggerId":"",
   "resourceId":"",
   "category":"TriggerRuns",
   "level":"Informational",
   "operationName":"",
   "triggerName":"",
   "triggerType":"",
   "triggerEvent":"",
   "start":"",
   "status":"",
   "properties":
   {
      "Parameters": {
        "TriggerTime": "",
       "ScheduleTime": ""
      },
      "SystemParameters": {}
    }
}

```

| Propiedad | Tipo | Descripción | Ejemplo |
| --- | --- | --- | --- |
| **Level** |String | El nivel de los registros de diagnóstico. En el caso de los registros de ejecución de actividad, establezca el valor de la propiedad en 4. | `4` |
| **correlationId** |String | El identificador único para realizar el seguimiento de una solicitud determinada. | `319dc6b4-f348-405e-b8d7-aafc77b73e77` |
| **time** | String | La hora del evento en formato UTC de intervalo de tiempo`YYYY-MM-DDTHH:MM:SS.00000Z`. | `2017-06-28T21:00:27.3534352Z` |
|**triggerId**| String| El identificador de la ejecución del desencadenador. | `08587023010602533858661257311` |
|**resourceId**| String | El identificador asociado con el recurso de la fábrica de datos. | `/SUBSCRIPTIONS/<subID>/RESOURCEGROUPS/<resourceGroupName>/PROVIDERS/MICROSOFT.DATAFACTORY/FACTORIES/<dataFactoryName>` |
|**category**| String | La categoría de los registros de diagnóstico. Establezca el valor de propiedad en `PipelineRuns`. | `PipelineRuns` |
|**level**| String | El nivel de los registros de diagnóstico. Establezca el valor de propiedad en `Informational`. | `Informational` |
|**operationName**| String | El nombre del desencadenador con su estado final, que indica si el desencadenador se activó correctamente. Si el latido se realizó correctamente, el valor de la propiedad es `MyTrigger - Succeeded`. | `MyTrigger - Succeeded` |
|**triggerName**| String | El nombre del desencadenador. | `MyTrigger` |
|**triggerType**| String | El tipo del desencadenador. Los valores de propiedad posibles son `Manual Trigger` y `Schedule Trigger`. | `ScheduleTrigger` |
|**triggerEvent**| String | El evento del desencadenador. | `ScheduleTime - 2017-07-06T01:50:25Z` |
|**start**| String | La hora de inicio del desencadenador que se activa en formato UTC del intervalo de tiempo. | `2017-06-26T20:55:29.5007959Z`|
|**status**| String | El estado final que indica si el desencadenador se activó correctamente. Los valores de propiedad posibles son `Succeeded` y `Failed`. | `Succeeded`|

### <a name="log-analytics-schema"></a>Esquema de Log Analytics

Log Analytics hereda el esquema de Monitor con las excepciones siguientes:

* La primera letra de cada nombre de columna se escribe en mayúsculas. Por ejemplo, el nombre de columna "correlationId" en Monitor es "CorrelationId" en Log Analytics.
* No hay ninguna columna "Level".
* La columna de "propiedades" dinámicas se conservará como el tipo de blob de JSON dinámico siguiente.

    | Columna de Azure Monitor | Columna de Log Analytics | Tipo |
    | --- | --- | --- |
    | $.properties.UserProperties | UserProperties | Dinámica |
    | $.properties.Annotations | anotaciones | Dinámica |
    | $.properties.Input | Entrada | Dinámica |
    | $.properties.Output | Output | Dinámica |
    | $.properties.Error.errorCode | ErrorCode | int |
    | $.properties.Error.message | ErrorMessage | string |
    | $.properties.Error | Error | Dinámica |
    | $.properties.Predecessors | Predecessors | Dinámica |
    | $.properties.Parameters | Parámetros | Dinámica |
    | $.properties.SystemParameters | SystemParameters | Dinámica |
    | $.properties.Tags | Etiquetas | Dinámica |
    
## <a name="metrics"></a>Métricas

Con Monitor, puede obtener visibilidad sobre el rendimiento y el estado de las cargas de trabajo de Azure. El tipo más importante de datos de Monitor es la métrica, que también se denomina contador de rendimiento. La mayoría de los recursos de Azure emiten métricas. Monitor proporciona varias maneras de configurar y consumir estas métricas para supervisar y solucionar problemas.

La versión 2 de Azure Data Factory emite las siguientes métricas.

| **Métrica**           | **Nombre de métrica para mostrar**         | **Unidad** | **Tipo de agregación** | **Descripción**                                       |
|----------------------|---------------------------------|----------|----------------------|-------------------------------------------------------|
| PipelineSucceededRuns | Las métricas de ejecuciones de canalización se realizaron correctamente | Count    | Total                | El número total de ejecuciones de canalización realizadas correctamente dentro de un período de minutos. |
| PipelineFailedRuns   | Métricas de ejecuciones de canalización erróneas    | Count    | Total                | El número total de ejecuciones de canalización con error dentro de una período de minutos.    |
| ActivitySucceededRuns | Métricas de ejecución de actividad realizadas correctamente | Count    | Total                | El número total de ejecuciones de actividad realizadas correctamente dentro de un período de minutos.  |
| ActivityFailedRuns   | Métricas de ejecuciones de actividad erróneas    | Count    | Total                | El número total de ejecuciones de actividad en las que se produjo un error en período de minutos.     |
| TriggerSucceededRuns | Métricas de ejecuciones de desencadenador realizadas correctamente  | Count    | Total                | El número total de ejecuciones de desencadenador realizadas correctamente dentro de un período de minutos.   |
| TriggerFailedRuns    | Métricas de ejecuciones de desencadenador erróneas     | Count    | Total                | El número total de ejecuciones de desencadenador en las que se produjo un error dentro de un período de minutos.      |

Para acceder a las métricas, complete las instrucciones que aparecen en la [plataforma de datos de Azure Monitor](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-metrics).

> [!NOTE]
> Solo se emiten eventos de ejecución de canalización y actividad desencadenada. Las ejecuciones de espacio aislado y depuración que están en curso **no** se emiten. 

## <a name="monitor-data-factory-metrics-with-azure-monitor"></a>Supervisión de las métricas de Data Factory con Azure Monitor

Puede usar la integración de Data Factory con Monitor para enrutar datos a Monitor. Esta integración resulta útil en los escenarios siguientes:

* Quiere escribir consultas complejas en un amplio conjunto de métricas que se publican mediante Data Factory en Monitor. Puede crear alertas personalizadas sobre estas consultas mediante Monitor.

* Quiere realizar la supervisión entre fábricas de datos. Puede enrutar datos desde varias factorías de datos a una única área de trabajo de Monitor.

Si desea una demostración y una introducción de siete minutos de esta característica, vea el siguiente vídeo:

> [!VIDEO https://channel9.msdn.com/Shows/Azure-Friday/Monitor-Data-Factory-pipelines-using-Operations-Management-Suite-OMS/player]

### <a name="configure-diagnostic-settings-and-workspace"></a>Configuración de las opciones de diagnóstico y el área de trabajo

Cree o agregue la configuración de diagnóstico de su factoría de datos.

1. En el portal, vaya a Monitor. Seleccione **Configuración** > **Configuración de diagnóstico**.

1. Seleccione la factoría de datos para la que quiere establecer una configuración de diagnóstico.

1. Si no existe ninguna configuración en la factoría de datos seleccionada, se le pedirá que cree una. Seleccione **Activar diagnósticos**.

   ![Creación de una configuración de diagnóstico si no existe ninguna configuración](media/data-factory-monitor-oms/monitor-oms-image1.png)

   Si hay una configuración existente en la factoría de datos, verá una lista de configuraciones ya establecidas en la factoría de datos. Seleccione **Agregar configuración de diagnóstico**.

   ![Adición de una configuración de diagnóstico si existe ninguna configuración](media/data-factory-monitor-oms/add-diagnostic-setting.png)

1. Asigne un nombre de la configuración, seleccione **Enviar a Log Analytics** y, después, seleccione un área de trabajo en **Área de trabajo de Log Analytics**.

    ![Asignación de un nombre a la configuración y selección de un área de trabajo de Log Analytics](media/data-factory-monitor-oms/monitor-oms-image2.png)

1. Seleccione **Guardar**.

Transcurridos unos instantes, el nuevo valor de configuración aparece en la lista de configuraciones para esta factoría de datos. Los registros de diagnóstico se transmiten en esa área de trabajo en cuanto se generan nuevos datos de evento. Pueden pasar hasta 15 minutos entre el momento en que se emite un evento y cuando aparece en Log Analytics.

* En el modo _específico del recurso_, los registros de diagnóstico de Azure Data Factory fluyen a las tablas _ADFPipelineRun_, _ADFTriggerRun_ y _ADFActivityRun_
* En el modo _Azure Diagnostics_, los registros de diagnóstico fluyen a la tabla _AzureDiagnostics_.

> [!NOTE]
> Como una tabla de registro de Azure no puede tener más de 500 columnas, se recomienda encarecidamente seleccionar el modo específico de recursos. Para más información, consulte las [limitaciones conocidas de Log Analytics](../azure-monitor/platform/resource-logs-collect-workspace.md#column-limit-in-azurediagnostics).

### <a name="install-azure-data-factory-analytics-from-azure-marketplace"></a>Instale Azure Data Factory Analytics desde Azure Marketplace

![Vaya a "Azure Marketplace", escriba "Filtro de Analytics" y seleccione "Azure Data Factory Analytics (versión preliminar)".](media/data-factory-monitor-oms/monitor-oms-image3.png)

![Detalles sobre "Azure Data Factory Analytics (versión preliminar)"](media/data-factory-monitor-oms/monitor-oms-image4.png)

Seleccione **Crear** y, después, seleccione **Área de trabajo de OMS** y **Configuración del área de trabajo de OMS**.

![Creación de una solución](media/data-factory-monitor-oms/monitor-oms-image5.png)

### <a name="monitor-data-factory-metrics"></a>Supervisión de métricas de Data Factory

La instalación de Azure Data Factory Analytics crea un conjunto de vistas predeterminado para que se habiliten las métricas siguientes:

- Ejecuciones de ADF: 1) Ejecuciones de canalización por Data Factory
 
- Ejecuciones de ADF: 2) Ejecuciones de actividad por Data Factory

- Ejecuciones de ADF: 3) Ejecuciones de desencadenador por Data Factory

- Errores de ADF: 1) 10 errores de canalización principales por Data Factory

- Errores de ADF: 2) 10 ejecuciones de actividad principales por Data Factory

- Errores de ADF: 3) 10 errores de desencadenador principales por Data Factory

- Estadísticas de ADF: 1) Ejecuciones de actividad por tipo

- Estadísticas de ADF: 2) Ejecuciones de desencadenador por tipo

- Estadísticas de ADF: 3) Duración máxima de las ejecuciones de canalización

![Ventana con "Libros (vista previa)" y "AzureDataFactoryAnalytics" resaltados](media/data-factory-monitor-oms/monitor-oms-image6.png)

Puede visualizar las métricas anteriores, examinar las consultas detrás de estas métricas, editar las consultas, crear alertas y realizar otras acciones.

![Representación gráfica de las ejecuciones de canalización por factoría de datos"](media/data-factory-monitor-oms/monitor-oms-image8.png)

> [!NOTE]
> Azure Data Factory Analytics (versión preliminar) envía los registros de diagnóstico a tablas de destino _específicas del recurso_. Puede escribir consultas en las tablas siguientes: _ADFPipelineRun_, _ADFTriggerRun_ y _ADFActivityRun_.

## <a name="alerts"></a>Alertas

Inicie sesión en Azure Portal y haga clic en **Monitor** > **Alertas** para crear alertas.

![Alertas en el menú del portal](media/monitor-using-azure-monitor/alerts_image3.png)

### <a name="create-alerts"></a>Creación de alertas

1. Haga clic en **+Nueva regla de alertas** para crear una nueva alerta.

    ![Nueva alerta de reglas](media/monitor-using-azure-monitor/alerts_image4.png)

1. Defina la condición de la alerta.

    > [!NOTE]
    > Asegúrese de seleccionar **Todo** en **Filtrar por tipo de recurso**.

    !["Definir condición de la alerta" > "Seleccionar destino", que abre el panel "Seleccionar un recurso" ](media/monitor-using-azure-monitor/alerts_image5.png)

    !["Definir condición de la alerta" > "Agregar criterios", que abre el panel "Configurar lógica de señal"](media/monitor-using-azure-monitor/alerts_image6.png)

    ![Panel "Configurar tipo de señal"](media/monitor-using-azure-monitor/alerts_image7.png)

1. Defina los detalles de la alerta.

    ![Detalles de alertas](media/monitor-using-azure-monitor/alerts_image8.png)

1. Defina el grupo de acciones.

    ![Creación de una regla, con "Nuevo grupo de acciones" resaltado](media/monitor-using-azure-monitor/alerts_image9.png)

    ![Creación de un nuevo grupo de acciones](media/monitor-using-azure-monitor/alerts_image10.png)

    ![Configuración del correo electrónico, SMS, inserciones y voz](media/monitor-using-azure-monitor/alerts_image11.png)

    ![Definición de un grupo de acciones](media/monitor-using-azure-monitor/alerts_image12.png)

## <a name="next-steps"></a>Pasos siguientes
[Supervisión y administración de canalizaciones mediante programación](monitor-programmatically.md)
