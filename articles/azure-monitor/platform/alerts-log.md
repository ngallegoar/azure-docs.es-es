---
title: Creación, visualización y administración de alertas de registro mediante Azure Monitor | Microsoft Docs
description: Utilice Azure Monitor para crear, ver y administrar las reglas de alertas de registro en Azure.
author: yanivlavi
ms.author: yalavi
ms.topic: conceptual
ms.date: 07/29/2019
ms.subservice: alerts
ms.openlocfilehash: 96b1bd86576f8cf34428eb60e2d3f476312311c1
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "79226532"
---
# <a name="create-view-and-manage-log-alerts-using-azure-monitor"></a>Creación, visualización y administración de alertas de registro mediante Azure Monitor

## <a name="overview"></a>Información general
En este artículo se muestra cómo configurar las alertas de registro con la interfaz de alertas en Azure Portal. La definición de una regla de alertas consta de tres partes:
- Destino: recurso de Azure específico que se va a supervisar.
- Criterios: condición o lógica específicas que, cuando se señaliza la alerta, deben desencadenar una acción.
- Acción: llamada específica enviada a un receptor de una notificación (correo electrónico, SMS, webhook, etc.).

El término **Alertas de registro** se usa para describir las alertas cuya señal es una consulta de registro basada en un [área de trabajo de Log Analytics](../learn/tutorial-viewdata.md) o en [Application Insights](../app/analytics.md). Obtenga más información acerca de la funcionalidad, la terminología y los tipos de [Alertas de registro: información general](alerts-unified-log.md).

> [!NOTE]
> Ahora los datos de registro populares de [un área de trabajo de Log Analytics](../../azure-monitor/learn/tutorial-viewdata.md) también están disponibles en la plataforma de métricas de Azure Monitor. Para obtener más detalles, consulte [Alerta de métricas de los registros](alerts-metric-logs.md).

## <a name="managing-log-alerts-from-the-azure-portal"></a>Administración de las alertas de registro desde Azure Portal

A continuación, se muestra una guía paso a paso acerca de cómo usar las alertas de registro mediante la interfaz de Azure Portal.

### <a name="create-a-log-alert-rule-with-the-azure-portal"></a>Creación de una regla de alertas de registro en Azure Portal

1. En [Azure Portal](https://portal.azure.com/), seleccione **Supervisar** y, en la sección SUPERVISAR, elija **Alertas**.

    ![Supervisión](media/alerts-log/AlertsPreviewMenu.png)

1. Seleccione el botón **Nueva regla de alertas** para crear una alerta en Azure.

    ![Agregar alerta](media/alerts-log/AlertsPreviewOption.png)

1. La sección Crear alerta se muestra con tres partes, que constan de: *definición de la condición de alerta*, *definición de los detalles de la alerta* y *definición del grupo de acciones*.

    ![Creación de una regla](media/alerts-log/AlertsPreviewAdd.png)

1. Para definir la condición de la alerta, use primero el vínculo **Seleccionar recurso** y especifique el destino mediante la selección de un recurso. Para filtrar, elija la _Suscripción_, el _Tipo de recurso_ y el _Recurso_ necesario.

   > [!NOTE]
   > Para crear una alerta de registro, compruebe la señal de **registro** disponible para el recurso seleccionado antes de continuar.
   >  ![Seleccionar recurso](media/alerts-log/Alert-SelectResourceLog.png)

1. *Alertas de registro*: asegúrese de que la opción **Tipo de recurso** sea un origen de análisis como *Log Analytics* o *Application Insights* y el tipo de señal sea **Registro**; después, una vez elegido el **recurso** apropiado, haga clic en *Listo*. A continuación, use el botón **Agregar criterios** para ver la lista de opciones de señal disponibles para el recurso y, de la lista de señales, la opción **Búsqueda de registros personalizada** para el servicio de supervisión del registro, como *Log Analytics* o *Application Insights*.

   ![Selección de un recurso: búsqueda de recursos personalizada](media/alerts-log/AlertsPreviewResourceSelectionLog.png)

   > [!NOTE]
   > 
   > Las listas de las alertas pueden importar una consulta de análisis como tipo de señal (**Log (Saved Query)** (Registro [consulta guardada])), tal como se muestra en la ilustración anterior. Por tanto, los usuarios pueden perfeccionar la consulta en Analytics y luego guardarla para usarla en alertas en otro momento. Puede encontrar más detalles sobre el uso de consultas guardadas en [Introducción a las consultas de registro en Azure Monitor](../log-query/log-query-overview.md) o [Consulta compartida en Analytics de Application Insights](../app/app-insights-overview.md).

1. *Alertas de registro*: una vez seleccionada esta opción, la consulta de alertas se puede indicar en el campo **Consulta de búsqueda**; si la sintaxis de la consulta es incorrecta, en el campo aparece el error en ROJO. Si la sintaxis de consulta es correcta, como referencia, se muestran los datos históricos de la consulta indicada en formato de gráfico con la opción de retocar la ventana de tiempo desde las últimas seis horas hasta la última semana.

    ![Configuración de la regla de alertas](media/alerts-log/AlertsPreviewAlertLog.png)

   > [!NOTE]
   > 
   > La visualización de los datos históricos solo se muestra si los resultados de la consulta tienen detalles temporales. Si la consulta genera datos resumidos o valores de columna específicos, estos se representan en un gráfico singular.
   > Para el tipo Unidades métricas de Alertas de registro con Application Insights o [que cambiaron a la nueva API](alerts-log-api-switch.md), puede especificar por cuál variable concreta desea agrupar los datos mediante la opción **Agregado en**, tal y como se ilustra a continuación:
   > 
   > ![agregar una opción](media/alerts-log/aggregate-on.png)

1. *Alertas de registro*: Con la visualización activada, se puede seleccionar la **Lógica de alerta** de entre las opciones mostradas de Condición, Agregación y, por último, Umbral. Por último, especifique en la lógica el tiempo para evaluar la condición especificada mediante la opción **Periodo**. Además, especifique la frecuencia con que debe ejecutarse la alerta seleccionando la opción adecuada en el campo **Frecuencia**. Las **alertas de registro** se pueden basar en lo siguiente:
    - [Número de registros](../../azure-monitor/platform/alerts-unified-log.md#number-of-results-alert-rules): se crea una alerta si el número de registros devueltos por la consulta es mayor o menor que el valor que proporcione.
    - [Unidades métricas](../../azure-monitor/platform/alerts-unified-log.md#metric-measurement-alert-rules): se crea una alerta si cada *valor agregado* en los resultados excede el valor de umbral proporcionado y se *agrupa por* el valor elegido. El número de infracciones de una alerta es el número de veces que se supera el umbral en el período de tiempo seleccionado. Puede especificar Infracciones totales para cualquier combinación de infracciones en el conjunto de resultados o Infracciones consecutivas para que las infracciones deban tener lugar en muestras consecutivas.


1. En segundo lugar, asigne un nombre a la alerta en el campo **Nombre de la regla de alertas** junto con una **Descripción**, en la que debe proporcionar información específica sobre la alerta, y debe indicar también un valor de **Gravedad** entre las opciones proporcionadas. Estos detalles se reutilizan en todos los correos electrónicos, las notificaciones o las notificaciones push de alerta enviados por Azure Monitor. Además, el usuario puede elegir activar inmediatamente la regla de alertas al crearla cambiando la opción **Habilitar regla tras la creación** según corresponda.

    Solo para las **alertas de registro**, se encuentra disponible alguna funcionalidad adicional en los detalles de la alerta:

    - **Suprimir alertas**: Al activar la supresión de la regla de alerta, las acciones de la regla se deshabilitan durante un período de tiempo definido después de crear una nueva alerta. La regla se sigue ejecutando y crea registros de alerta si se cumplen los criterios. De esta forma, dispone de tiempo para corregir el problema sin ejecutar acciones duplicadas.

        ![Desactivar las alertas de registro](media/alerts-log/AlertsPreviewSuppress.png)

        > [!TIP]
        > Especifique un valor de desactivar las alertas mayor que la frecuencia de alertas para garantizar que las notificaciones se detengan sin superposición

1. Como tercer y último paso, especifique si es necesario desencadenar algún **grupo de acciones** para la regla de alertas si se cumple la condición de alerta. Puede elegir cualquier grupo de acciones existente con alerta o crear uno. Según el grupo de acciones seleccionado, cuando se desencadena la alerta, Azure: envía correos electrónicos, envía SMS, llama a webhooks, la soluciona con runbooks de Azure, envía notificaciones push a la herramienta de ITSM, etc. Obtenga más información sobre los [grupos de acciones](action-groups.md).

    > [!NOTE]
    > Consulte los [límites de servicio de suscripción de Azure](../../azure-resource-manager/management/azure-subscription-service-limits.md) para conocer los límites relacionados con las cargas de runbook desencadenadas para las alertas de registro a través de grupos de acciones de Azure.

    Para las **alertas de registro**, se encuentra disponible alguna funcionalidad adicional para reemplazar las acciones predeterminadas:

    - **Notificación por correo electrónico**: invalida el *asunto del correo electrónico* en el correo electrónico, enviado a través del grupo de acciones, si existen una o más acciones de correo electrónico en dicho grupo de acciones. No se puede modificar el cuerpo del mensaje de correo y este campo **no** es para la dirección de correo electrónico.
    - **Incluir carga de JSON personalizada**: invalida el webhook JSON usado por los grupos de acciones si una o varias acciones de webhook existen en el grupo de acciones mencionado. El usuario puede especificar el formato JSON que se usará para todos los webhooks configurados en el grupo de acciones asociado; para más información sobre los formatos de webhook, consulte [Acciones de webhook para alertas de registro](../../azure-monitor/platform/alerts-log-webhook.md). La opción de vista de webhook se proporciona para comprobar el formato con datos JSON de ejemplo.

        ![Invalidaciones de acciones para alertas de registro](media/alerts-log/AlertsPreviewOverrideLog.png)


1. Si todos los campos son válidos y tienen una marca verde, se puede hacer clic en el botón **Crear regla de alertas** y se crea la alerta en Azure Monitor: Alertas. Todas las alertas pueden verse en el panel de las alertas.

     ![Creación de reglas](media/alerts-log/AlertsPreviewCreate.png)

     En cuestión de minutos, se activa la alerta y se desencadena tal como se describió anteriormente.

Los usuarios también pueden finalizar la consulta de análisis en [Log Analytics](../log-query/portals.md) e insertarla para crear una alerta mediante el botón "Establecer alerta" y, a continuación, seguir las instrucciones del paso 6 en adelante del tutorial anterior.

 ![Log Analytics: Establecer alerta](media/alerts-log/AlertsAnalyticsCreate.png)

### <a name="view--manage-log-alerts-in-azure-portal"></a>Visualización y administración de las alertas de registro en Azure Portal

1. En [Azure Portal](https://portal.azure.com/), seleccione **Supervisar** y, en la sección SUPERVISAR, elija **Alertas**.

1. Se muestra el **panel de alertas**, de forma que aparecen todas las alertas de Azure (incluidas las alertas de registro) en un único panel, incluidas todas las instancias de cuando se ha activado la regla de la alerta de registro. Para obtener más información, consulte [Alert Management](https://aka.ms/managealertinstances).
    > [!NOTE]
    > Las reglas de la alerta de registro se componen de una lógica personalizada basada en la consulta proporcionada por los usuarios y, por lo tanto, sin un estado resuelto. Debido a esto, cada vez que se cumplen las condiciones especificadas en la regla de la alerta de registro, se activa.

1. Seleccione el botón **Administrar reglas** situado en la barra superior para navegar hasta la sección de administración de reglas, donde se enumeran todas las reglas de alerta creadas, incluidas las alertas que se han deshabilitado.
    ![Administrar reglas de alertas](media/alerts-log/manage-alert-rules.png)

## <a name="managing-log-alerts-using-azure-resource-template"></a>Administración de alertas de registro mediante la plantilla de recursos de Azure

Las alertas del registro de Azure Monitor están asociadas con el tipo de recurso `Microsoft.Insights/scheduledQueryRules/`. Para más información sobre este tipo de recurso, vea [Azure Monitor: referencia de la API de reglas de consulta programada](https://docs.microsoft.com/rest/api/monitor/scheduledqueryrules/). Las alertas de registro de Application Insights o Log Analytics se pueden crear mediante la [API de Reglas de consulta programadas](https://docs.microsoft.com/rest/api/monitor/scheduledqueryrules/).

> [!NOTE]
> Las alertas de registro de Log Analytics también se puede administrar mediante [Alert API de Log Analytics](api-alerts.md) y las plantillas heredadas de [búsquedas y alertas guardadas de Log Analytics](../insights/solutions-resources-searches-alerts.md) también. Para más información acerca del uso de la nueva API de Reglas de consulta programadas detallada aquí de forma predeterminada, consulte [Switch to new API for Log Analytics Alerts](alerts-log-api-switch.md) (Cambio a una API nueva de alertas de Log Analytics).


### <a name="sample-log-alert-creation-using-azure-resource-template"></a>Creación de una alerta de registro de ejemplo mediante la plantilla de recursos de Azure

Esta es la estructura de la plantilla de recursos basada en la [creación de reglas de consulta programada](https://docs.microsoft.com/rest/api/monitor/scheduledqueryrules/createorupdate) mediante una consulta de búsqueda de registros estándar de una [alerta de registro del tipo número de resultados](alerts-unified-log.md#number-of-results-alert-rules), con datos de ejemplo establecidos como variables.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
    },
    "variables": {
        "alertLocation": "southcentralus",
        "alertName": "samplelogalert",
        "alertDescription": "Sample log search alert",
        "alertStatus": "true",
        "alertSource":{
            "Query":"requests",
            "SourceId": "/subscriptions/a123d7efg-123c-1234-5678-a12bc3defgh4/resourceGroups/myRG/providers/microsoft.insights/components/sampleAIapplication",
            "Type":"ResultCount"
        },
        "alertSchedule":{
            "Frequency": 15,
            "Time": 60
        },
        "alertActions":{
            "SeverityLevel": "4"
        },
        "alertTrigger":{
            "Operator":"GreaterThan",
            "Threshold":"1"
        },
        "actionGrp":{
            "ActionGroup": "/subscriptions/a123d7efg-123c-1234-5678-a12bc3defgh4/resourceGroups/myRG/providers/microsoft.insights/actiongroups/sampleAG",
            "Subject": "Customized Email Header",
            "Webhook": "{ \"alertname\":\"#alertrulename\", \"IncludeSearchResults\":true }"
        }
    },
    "resources":[ {
        "name":"[variables('alertName')]",
        "type":"Microsoft.Insights/scheduledQueryRules",
        "apiVersion": "2018-04-16",
        "location": "[variables('alertLocation')]",
        "properties":{
            "description": "[variables('alertDescription')]",
            "enabled": "[variables('alertStatus')]",
            "source": {
                "query": "[variables('alertSource').Query]",
                "dataSourceId": "[variables('alertSource').SourceId]",
                "queryType":"[variables('alertSource').Type]"
            },
            "schedule":{
                "frequencyInMinutes": "[variables('alertSchedule').Frequency]",
                "timeWindowInMinutes": "[variables('alertSchedule').Time]"
            },
            "action":{
                "odata.type": "Microsoft.WindowsAzure.Management.Monitoring.Alerts.Models.Microsoft.AppInsights.Nexus.DataContracts.Resources.ScheduledQueryRules.AlertingAction",
                "severity":"[variables('alertActions').SeverityLevel]",
                "aznsAction":{
                    "actionGroup":"[array(variables('actionGrp').ActionGroup)]",
                    "emailSubject":"[variables('actionGrp').Subject]",
                    "customWebhookPayload":"[variables('actionGrp').Webhook]"
                },
                "trigger":{
                    "thresholdOperator":"[variables('alertTrigger').Operator]",
                    "threshold":"[variables('alertTrigger').Threshold]"
                }
            }
        }
    } ]
}

```

El JSON del ejemplo anterior puede guardarse como (digamos) sampleScheduledQueryRule.json a efectos de este tutorial y puede implementarse mediante [Azure Resource Manager en Azure Portal](../../azure-resource-manager/templates/deploy-portal.md#deploy-resources-from-custom-template).


### <a name="log-alert-with-cross-resource-query-using-azure-resource-template"></a>Alerta de registro con consulta entre registros mediante la plantilla de recursos de Azure

Esta es la estructura de la plantilla de recursos basada en la [creación de reglas de consulta programada](https://docs.microsoft.com/rest/api/monitor/scheduledqueryrules/createorupdate) mediante una [consulta de búsqueda de registros entre recursos](../../azure-monitor/log-query/cross-workspace-query.md) de una [alerta de registro del tipo unidad métrica](../../azure-monitor/platform/alerts-unified-log.md#metric-measurement-alert-rules), con datos de ejemplo establecidos como variables.

```json

{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
    },
    "variables": {
        "alertLocation": "Region Name for your Application Insights App or Log Analytics Workspace",
        "alertName": "sample log alert",
        "alertDescr": "Sample log search alert",
        "alertStatus": "true",
        "alertSource":{
            "Query":"union workspace(\"servicews\").Update, app('serviceapp').requests | summarize AggregatedValue = count() by bin(TimeGenerated,1h), Classification",
            "Resource1": "/subscriptions/a123d7efg-123c-1234-5678-a12bc3defgh4/resourceGroups/contosoRG/providers/microsoft.OperationalInsights/workspaces/servicews",
            "Resource2": "/subscriptions/a123d7efg-123c-1234-5678-a12bc3defgh4/resourceGroups/contosoRG/providers/microsoft.insights/components/serviceapp",
            "SourceId": "/subscriptions/a123d7efg-123c-1234-5678-a12bc3defgh4/resourceGroups/contosoRG/providers/microsoft.OperationalInsights/workspaces/servicews",
            "Type":"ResultCount"
        },
        "alertSchedule":{
            "Frequency": 15,
            "Time": 60
        },
        "alertActions":{
            "SeverityLevel": "4",
            "SuppressTimeinMin": 20
        },
        "alertTrigger":{
            "Operator":"GreaterThan",
            "Threshold":"1"
        },
        "metricMeasurement": {
            "thresholdOperator": "Equal",
            "threshold": "1",
            "metricTriggerType": "Consecutive",
            "metricColumn": "Classification"
        },
        "actionGrp":{
            "ActionGroup": "/subscriptions/a123d7efg-123c-1234-5678-a12bc3defgh4/resourceGroups/contosoRG/providers/microsoft.insights/actiongroups/sampleAG",
            "Subject": "Customized Email Header",
            "Webhook": "{ \"alertname\":\"#alertrulename\", \"IncludeSearchResults\":true }"
        }
    },
    "resources":[ {
        "name":"[variables('alertName')]",
        "type":"Microsoft.Insights/scheduledQueryRules",
        "apiVersion": "2018-04-16",
        "location": "[variables('alertLocation')]",
        "properties":{
            "description": "[variables('alertDescr')]",
            "enabled": "[variables('alertStatus')]",
            "source": {
                "query": "[variables('alertSource').Query]",
                "authorizedResources": "[concat(array(variables('alertSource').Resource1), array(variables('alertSource').Resource2))]",
                "dataSourceId": "[variables('alertSource').SourceId]",
                "queryType":"[variables('alertSource').Type]"
            },
            "schedule":{
                "frequencyInMinutes": "[variables('alertSchedule').Frequency]",
                "timeWindowInMinutes": "[variables('alertSchedule').Time]"
            },
            "action":{
                "odata.type": "Microsoft.WindowsAzure.Management.Monitoring.Alerts.Models.Microsoft.AppInsights.Nexus.DataContracts.Resources.ScheduledQueryRules.AlertingAction",
                "severity":"[variables('alertActions').SeverityLevel]",
                "throttlingInMin": "[variables('alertActions').SuppressTimeinMin]",
                "aznsAction":{
                    "actionGroup": "[array(variables('actionGrp').ActionGroup)]",
                    "emailSubject":"[variables('actionGrp').Subject]",
                    "customWebhookPayload":"[variables('actionGrp').Webhook]"
                },
                "trigger":{
                    "thresholdOperator":"[variables('alertTrigger').Operator]",
                    "threshold":"[variables('alertTrigger').Threshold]",
                    "metricTrigger":{
                        "thresholdOperator": "[variables('metricMeasurement').thresholdOperator]",
                        "threshold": "[variables('metricMeasurement').threshold]",
                        "metricColumn": "[variables('metricMeasurement').metricColumn]",
                        "metricTriggerType": "[variables('metricMeasurement').metricTriggerType]"
                    }
                }
            }
        }
    } ]
}

```

> [!IMPORTANT]
> Si usa una consulta entre recursos en la alerta de registros, el uso de [authorizedResources](https://docs.microsoft.com/rest/api/monitor/scheduledqueryrules/createorupdate#source) es obligatorio y el usuario debe acceder a la lista de los recursos indicados.

El JSON del ejemplo anterior puede guardarse como (digamos) sampleScheduledQueryRule.json a efectos de este tutorial y puede implementarse mediante [Azure Resource Manager en Azure Portal](../../azure-resource-manager/templates/deploy-portal.md#deploy-resources-from-custom-template).

## <a name="managing-log-alerts-using-powershell"></a>Administración de alertas de registro con PowerShell

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

Azure Monitor: [API de Reglas de consulta programadas](https://docs.microsoft.com/rest/api/monitor/scheduledqueryrules/) es una API REST totalmente compatible con la API REST de Azure Resource Manager. Y los cmdlets de PowerShell que se enumeran a continuación están disponibles para aprovechar la [API de Reglas de consulta programadas](https://docs.microsoft.com/rest/api/monitor/scheduledqueryrules/).

1. [New-AzScheduledQueryRule](https://docs.microsoft.com/powershell/module/az.monitor/new-azscheduledqueryrule): cmdlet de PowerShell para crear una nueva regla de alerta de registro.
1. [Set-AzScheduledQueryRule](https://docs.microsoft.com/powershell/module/az.monitor/set-azscheduledqueryrule): cmdlet de PowerShell para actualizar una regla de alerta de registro.
1. [New-AzScheduledQueryRuleSource](https://docs.microsoft.com/powershell/module/az.monitor/new-azscheduledqueryrulesource): cmdlet de PowerShell para crear o actualizar el objeto que especifica los parámetros de origen para una alerta de registro. Se usa como entrada en los cmdlets [New-AzScheduledQueryRule](https://docs.microsoft.com/powershell/module/az.monitor/new-azscheduledqueryrule) y [Set-AzScheduledQueryRule](https://docs.microsoft.com/powershell/module/az.monitor/set-azscheduledqueryrule).
1. [New-AzScheduledQueryRuleSchedule](https://docs.microsoft.com/powershell/module/az.monitor/New-AzScheduledQueryRuleSchedule): cmdlet de PowerShell para crear o actualizar el objeto que especifica los parámetros de programación para una alerta de registro. Se usa como entrada en los cmdlets [New-AzScheduledQueryRule](https://docs.microsoft.com/powershell/module/az.monitor/new-azscheduledqueryrule) y [Set-AzScheduledQueryRule](https://docs.microsoft.com/powershell/module/az.monitor/set-azscheduledqueryrule).
1. [New-AzScheduledQueryRuleAlertingAction](https://docs.microsoft.com/powershell/module/az.monitor/New-AzScheduledQueryRuleAlertingAction): cmdlet de PowerShell para crear o actualizar el objeto que especifica los parámetros de acción para una alerta de registro. Se usa como entrada en los cmdlets [New-AzScheduledQueryRule](https://docs.microsoft.com/powershell/module/az.monitor/new-azscheduledqueryrule) y [Set-AzScheduledQueryRule](https://docs.microsoft.com/powershell/module/az.monitor/set-azscheduledqueryrule).
1. [New-AzScheduledQueryRuleAznsActionGroup](https://docs.microsoft.com/powershell/module/az.monitor/new-azscheduledqueryruleaznsactiongroup): cmdlet de PowerShell para crear o actualizar el objeto que especifica los parámetros de grupos para una alerta de registro. Se usa como entrada en el cmdlet [New-AzScheduledQueryRuleAlertingAction](https://docs.microsoft.com/powershell/module/az.monitor/New-AzScheduledQueryRuleAlertingAction).
1. [New-AzScheduledQueryRuleTriggerCondition](https://docs.microsoft.com/powershell/module/az.monitor/new-azscheduledqueryruletriggercondition): cmdlet de PowerShell para crear o actualizar el objeto que especifica los parámetros de condición de desencadenador para una alerta de registro. Se usa como entrada en el cmdlet [New-AzScheduledQueryRuleAlertingAction](https://docs.microsoft.com/powershell/module/az.monitor/New-AzScheduledQueryRuleAlertingAction).
1. [New-AzScheduledQueryRuleLogMetricTrigger](https://docs.microsoft.com/powershell/module/az.monitor/new-azscheduledqueryrulelogmetrictrigger): cmdlet de PowerShell para crear o actualizar el objeto que especifica los parámetros de condición de desencadenador para una [alerta de registro de tipo de medida de métrica](../../azure-monitor/platform/alerts-unified-log.md#metric-measurement-alert-rules). Se usa como entrada en el cmdlet [New-AzScheduledQueryRuleTriggerCondition](https://docs.microsoft.com/powershell/module/az.monitor/new-azscheduledqueryruletriggercondition).
1. [Get-AzScheduledQueryRule](https://docs.microsoft.com/powershell/module/az.monitor/get-azscheduledqueryrule): cmdlet de PowerShell para enumerar las reglas de alerta de registro o una regla de alerta de registro específica
1. [Update-AzScheduledQueryRule](https://docs.microsoft.com/powershell/module/az.monitor/update-azscheduledqueryrule): cmdlet de PowerShell para habilitar o deshabilitar la regla de alerta de registro
1. [Remove-AzScheduledQueryRule](https://docs.microsoft.com/powershell/module/az.monitor/remove-azscheduledqueryrule): cmdlet de PowerShell para eliminar una regla de alerta de registro

> [!NOTE]
> Los cmdlets de PowerShell ScheduledQueryRules solo pueden administrar las reglas creadas por el propio cmdlet o mediante Azure Monitor: [API de Reglas de consulta programadas](https://docs.microsoft.com/rest/api/monitor/scheduledqueryrules/). Las reglas de alerta de registro creadas con la [API de alertas de Log Analytics](api-alerts.md) antiguas y las plantillas antiguas de [alertas y búsquedas guardadas de Log Analytics](../insights/solutions-resources-searches-alerts.md) pueden administrarse mediante los cmdlets de PowerShell ScheduledQueryRules solo después de que el usuario [cambie la preferencia de API a las alertas de Log Analytics](alerts-log-api-switch.md).

A continuación, se muestran los pasos para crear una regla de alertas de registro de ejemplo con los cmdlets scheduledQueryRules de PowerShell.
```powershell
$source = New-AzScheduledQueryRuleSource -Query 'Heartbeat | summarize AggregatedValue = count() by bin(TimeGenerated, 5m), _ResourceId' -DataSourceId "/subscriptions/a123d7efg-123c-1234-5678-a12bc3defgh4/resourceGroups/contosoRG/providers/microsoft.OperationalInsights/workspaces/servicews"

$schedule = New-AzScheduledQueryRuleSchedule -FrequencyInMinutes 15 -TimeWindowInMinutes 30

$metricTrigger = New-AzScheduledQueryRuleLogMetricTrigger -ThresholdOperator "GreaterThan" -Threshold 2 -MetricTriggerType "Consecutive" -MetricColumn "_ResourceId"

$triggerCondition = New-AzScheduledQueryRuleTriggerCondition -ThresholdOperator "LessThan" -Threshold 5 -MetricTrigger $metricTrigger

$aznsActionGroup = New-AzScheduledQueryRuleAznsActionGroup -ActionGroup "/subscriptions/a123d7efg-123c-1234-5678-a12bc3defgh4/resourceGroups/contosoRG/providers/microsoft.insights/actiongroups/sampleAG" -EmailSubject "Custom email subject" -CustomWebhookPayload "{ `"alert`":`"#alertrulename`", `"IncludeSearchResults`":true }"

$alertingAction = New-AzScheduledQueryRuleAlertingAction -AznsAction $aznsActionGroup -Severity "3" -Trigger $triggerCondition

New-AzScheduledQueryRule -ResourceGroupName "contosoRG" -Location "Region Name for your Application Insights App or Log Analytics Workspace" -Action $alertingAction -Enabled $true -Description "Alert description" -Schedule $schedule -Source $source -Name "Alert Name"
```

## <a name="managing-log-alerts-using-cli-or-api"></a>Administración de alertas de registro con la CLI o la API

Azure Monitor: [API de Reglas de consulta programadas](https://docs.microsoft.com/rest/api/monitor/scheduledqueryrules/) es una API REST totalmente compatible con la API REST de Azure Resource Manager. Por lo tanto, se puede utilizar a través de Powershell con los comandos de Resource Manager para la CLI de Azure.


> [!NOTE]
> Las alertas de registro de Log Analytics también se puede administrar mediante [Alert API de Log Analytics](api-alerts.md) y las plantillas heredadas de [búsquedas y alertas guardadas de Log Analytics](../insights/solutions-resources-searches-alerts.md) también. Para más información acerca del uso de la nueva API de Reglas de consulta programadas detallada aquí de forma predeterminada, consulte [Switch to new API for Log Analytics Alerts](alerts-log-api-switch.md) (Cambio a una API nueva de alertas de Log Analytics).

Actualmente, las alertas de registro no tienen comandos de la CLI dedicados; pero, como se muestra a continuación, se pueden usar mediante el cmdlet de la CLI de Azure Resource Manager para la plantilla de recursos de muestra que se mostró anteriormente (sampleScheduledQueryRule.json) en la sección Plantilla de recursos:

```azurecli
az group deployment create --resource-group contosoRG --template-file sampleScheduledQueryRule.json
```

Si la operación se realiza correctamente, se devolverá 201 para indicar que se ha creado la regla de alertas o 200 si se ha modificado una regla de alerta existente.

## <a name="next-steps"></a>Pasos siguientes

* Más información sobre las [alertas de registro en las alertas de Azure](../../azure-monitor/platform/alerts-unified-log.md).
* Conocer las [acciones de webhook para alertas de registro](../../azure-monitor/platform/alerts-log-webhook.md)
* Más información sobre [Application Insights](../../azure-monitor/app/analytics.md)
* Obtenga más información sobre las [consultas de registro](../log-query/log-query-overview.md).
