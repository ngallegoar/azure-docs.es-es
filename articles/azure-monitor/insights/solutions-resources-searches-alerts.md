---
title: Búsquedas guardadas en soluciones de administración | Microsoft Docs
description: Las soluciones de administración incluyen por lo general consultas de registro guardadas para analizar los datos recopilados por la solución. En este artículo se describe cómo definir búsquedas guardadas de Log Analytics en una plantilla de Resource Manager.
ms.subservice: ''
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 07/29/2019
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 61fc64e140af091b5ff3f631398daf901557791b
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "77663035"
---
# <a name="adding-log-analytics-saved-searches-and-alerts-to-management-solution-preview"></a>Adición de búsquedas y alertas guardadas de Log Analytics en la solución de administración (versión preliminar)

> [!IMPORTANT]
> Como [se anunció anteriormente](https://azure.microsoft.com/updates/switch-api-preference-log-alerts/), las áreas de trabajo de Log Analytics creadas después del *1 de junio de 2019*, podrán administrar reglas de alerta **únicamente** mediante la [API REST](https://docs.microsoft.com/rest/api/monitor/scheduledqueryrules/) Azure scheduledQueryRules, la [plantilla de Azure Resource Manager](../../azure-monitor/platform/alerts-log.md#managing-log-alerts-using-azure-resource-template) y los [cmdlets de PowerShell](../../azure-monitor/platform/alerts-log.md#managing-log-alerts-using-powershell). Los clientes pueden [cambiar fácilmente su forma preferida de administración de reglas de alertas](../../azure-monitor/platform/alerts-log-api-switch.md#process-of-switching-from-legacy-log-alerts-api) para que las áreas de trabajo anteriores aprovechen scheduledQueryRules de Azure Monitor como valor predeterminado y consigan muchas de las [nuevas ventajas](../../azure-monitor/platform/alerts-log-api-switch.md#benefits-of-switching-to-new-azure-api), como la posibilidad de usar cmdlets nativos de PowerShell, el aumento del período de retrospectiva en las reglas, la creación de reglas en un grupo de recursos o una suscripción independientes y mucho más.

> [!NOTE]
> Esta es la documentación preliminar para crear soluciones de administración que se encuentran actualmente en versión preliminar. Cualquier esquema descrito a continuación está sujeto a cambios.

Las [soluciones de administración](solutions.md) suelen incluir [búsquedas guardadas](../../azure-monitor/log-query/log-query-overview.md) en Log Analytics para analizar los datos recopilados por la solución. Pueden definir asimismo [alertas](../../azure-monitor/platform/alerts-overview.md) para notificar al usuario o realizar automáticamente una acción en respuesta a un problema crítico. En este artículo se describe cómo definir las búsquedas y alertas guardadas de Log Analytics en una [plantilla de Resource Management](../../azure-resource-manager/templates/quickstart-create-templates-use-the-portal.md) para que puedan incluirse en [soluciones de administración](solutions-creating.md).

> [!NOTE]
> En los ejemplos de este artículo se usan parámetros y variables que son necesarios o comunes para las soluciones de administración, y se describen en [Diseño y compilación de una solución de administración en Azure](solutions-creating.md).

## <a name="prerequisites"></a>Prerrequisitos
En este artículo se supone que ya está familiarizado con la manera de [crear una solución de administración](solutions-creating.md) y la estructura de una [plantilla de Resource Manager](../../azure-resource-manager/templates/template-syntax.md) y un archivo de solución.


## <a name="log-analytics-workspace"></a>Área de trabajo de Log Analytics
Todos los recursos de Log Analytics están contenidos en un [área de trabajo](../../azure-monitor/platform/manage-access.md). Como se describe en [el área de trabajo de Log Analytics y la cuenta de Automation](solutions.md#log-analytics-workspace-and-automation-account), el área de trabajo no está incluida en la solución de administración, pero debe existir antes de que se instale la solución. Si no está disponible, se producirá un error en la instalación de la solución.

El nombre del área de trabajo es el nombre de cada recurso de Log Analytics. Esto se hace en la solución con el parámetro **workspace** tal como se muestra en el siguiente ejemplo de un recurso de SavedSearch.

    "name": "[concat(parameters('workspaceName'), '/', variables('SavedSearchId'))]"

## <a name="log-analytics-api-version"></a>Versión de la API de Log Analytics
Todos los recursos de Log Analytics definidos en una plantilla de Resource Manager tienen una propiedad **apiVersion** que define la versión de la API que el recurso debe usar.

En la tabla siguiente se muestra la versión de API del recurso usado en este ejemplo.

| Tipo de recurso | Versión de API | Consultar |
|:---|:---|:---|
| savedSearches | 2017-03-15-preview | Event &#124; where EventLevelName == "Error"  |


## <a name="saved-searches"></a>Búsquedas guardadas
Incluya [búsquedas guardadas](../../azure-monitor/log-query/log-query-overview.md) en una solución para permitir a los usuarios consultar los datos recopilados por la solución. Las búsquedas guardadas aparecerán en **Búsquedas guardadas** en Azure Portal. También es necesaria una búsqueda guardada para cada alerta.

Los recursos de [búsquedas guardadas de Log Analytics](../../azure-monitor/log-query/log-query-overview.md) tienen un tipo de `Microsoft.OperationalInsights/workspaces/savedSearches` y presentan la siguiente estructura. Aquí se incluyen las variables y los parámetros habituales para que pueda copiar y pegar este fragmento de código en su archivo de solución y cambiar los nombres de parámetro.

    {
        "name": "[concat(parameters('workspaceName'), '/', variables('SavedSearch').Name)]",
        "type": "Microsoft.OperationalInsights/workspaces/savedSearches",
        "apiVersion": "[variables('LogAnalyticsApiVersion')]",
        "dependsOn": [
        ],
        "tags": { },
        "properties": {
            "etag": "*",
            "query": "[variables('SavedSearch').Query]",
            "displayName": "[variables('SavedSearch').DisplayName]",
            "category": "[variables('SavedSearch').Category]"
        }
    }

En la tabla siguiente se describe cada propiedad de una búsqueda guardada.

| Propiedad | Descripción |
|:--- |:--- |
| category | Categoría de la búsqueda guardada.  Las búsquedas guardadas en la misma solución comparten a menudo una única categoría por lo que están agrupadas juntas en la consola. |
| displayname | Nombre para mostrar de la búsqueda guardada en el portal. |
| Query | La consulta que se ejecutará. |

> [!NOTE]
> Puede que necesite utilizar caracteres de escape en la consulta si incluye caracteres que puedan interpretarse como JSON. Por ejemplo, si la consulta era **AzureActivity | OperationName:"Microsoft.Compute/virtualMachines/write"** , en el archivo de solución debe escribirse como **AzureActivity | OperationName:/\"Microsoft.Compute/virtualMachines/write\"** .

## <a name="alerts"></a>Alertas
Las [alertas de registro de Azure](../../azure-monitor/platform/alerts-unified-log.md) se crean mediante reglas de alerta de Azure que ejecutan consultas de registro especificadas a intervalos regulares. Si los resultados de la consulta coinciden con los criterios especificados, se crea un registro de alertas y se ejecutan una o varias acciones mediante [grupos de acciones](../../azure-monitor/platform/action-groups.md).

En el caso de los usuarios que extienden las alertas a Azure, ahora las acciones se controlan en los grupos de acciones de Azure. Cuando un área de trabajo y sus alertas se extienden a Azure, es posible recuperar o agregar acciones mediante el [grupo de acciones: plantilla de Azure Resource Manager](../../azure-monitor/platform/action-groups-create-resource-manager-template.md).
Las reglas de alertas en una solución de administración heredada se componen de estos tres recursos diferentes.

- **Búsqueda guardada.** Define la búsqueda de registros que se ejecuta. Varias reglas de alerta pueden compartir una única búsqueda guardada.
- **Programación.** Define la frecuencia con la que se ejecuta la búsqueda de registros. Cada regla de alerta tiene una única programación.
- **Acción de alerta.** Cada regla de alerta tiene un único recurso de grupo de acción con un tipo de **alerta** que define los detalles de la alerta, como los criterios de cuándo se crea un registro de alertas y la gravedad de la alerta. El recurso del [grupo de acción](../../azure-monitor/platform/action-groups.md) puede tener una lista de acciones configuradas que se deben realizar cuando se desencadena la alerta, como llamadas de voz, SMS, correo electrónico, webhook, herramienta ITSM, runbook de automatización, aplicación lógica, etc.

Los recursos de búsquedas guardadas se han descrito anteriormente. Los demás recursos se describen a continuación.

### <a name="schedule-resource"></a>Recurso de programación

Una búsqueda guardada puede tener una o más programaciones y cada programación representa una regla de alerta independiente. La programación define la frecuencia con la que se realiza la búsqueda y el intervalo de tiempo durante el que se recuperan los datos. Los recursos de programación tienen un tipo de `Microsoft.OperationalInsights/workspaces/savedSearches/schedules/` y presentan la siguiente estructura. Aquí se incluyen las variables y los parámetros habituales para que pueda copiar y pegar este fragmento de código en su archivo de solución y cambiar los nombres de parámetro.

    {
        "name": "[concat(parameters('workspaceName'), '/', variables('SavedSearch').Name, '/', variables('Schedule').Name)]",
        "type": "Microsoft.OperationalInsights/workspaces/savedSearches/schedules/",
        "apiVersion": "[variables('LogAnalyticsApiVersion')]",
        "dependsOn": [
            "[concat('Microsoft.OperationalInsights/workspaces/', parameters('workspaceName'), '/savedSearches/', variables('SavedSearch').Name)]"
        ],
        "properties": {
            "etag": "*",
            "interval": "[variables('Schedule').Interval]",
            "queryTimeSpan": "[variables('Schedule').TimeSpan]",
            "enabled": "[variables('Schedule').Enabled]"
        }
    }
En la tabla siguiente se describen las propiedades para los recursos de programación.

| Nombre del elemento | Obligatorio | Descripción |
|:--|:--|:--|
| enabled       | Sí | Especifica si la alerta está habilitada cuando se crea. |
| interval      | Sí | Frecuencia con la que se ejecuta la consulta en minutos. |
| queryTimeSpan | Sí | Período de tiempo en minutos en el que se evalúan los resultados. |

El recurso de programación debe depender de la búsqueda guardada para que se cree antes de la programación.
> [!NOTE]
> El nombre de la programación debe ser único en un área de trabajo determinado; dos programaciones no pueden tener el mismo identificador, incluso si están asociadas con diferentes búsquedas guardadas. También, el nombre de todas las búsquedas guardadas, programaciones y acciones creadas con la API de Log Analytics debe estar en minúsculas.

### <a name="actions"></a>Acciones
Una programación puede tener varias acciones. Una acción puede definir uno o varios procesos que se van a realizar (como enviar un correo o iniciar un Runbook), o bien puede definir un umbral que determina si los resultados de una búsqueda coinciden con algunos criterios. Algunas acciones definen ambos aspectos, de forma que los procesos se realizan cuando se alcance el umbral.
Las acciones pueden definirse mediante el recurso [grupo de acciones] o un recurso de acción.

Hay dos tipos de recursos de acción especificados por la propiedad **Type**. Una programación requiere una acción **Alert** que define los detalles de la regla de alerta y las acciones que se realizan cuando se crea una alerta. Los recursos de acción tienen un tipo de `Microsoft.OperationalInsights/workspaces/savedSearches/schedules/actions`.

Las acciones de alerta tienen la siguiente estructura. Aquí se incluyen las variables y los parámetros habituales para que pueda copiar y pegar este fragmento de código en su archivo de solución y cambiar los nombres de parámetro.

```json
{
    "name": "[concat(parameters('workspaceName'), '/', variables('SavedSearch').Name, '/', variables('Schedule').Name, '/', variables('Alert').Name)]",
    "type": "Microsoft.OperationalInsights/workspaces/savedSearches/schedules/actions",
    "apiVersion": "[variables('LogAnalyticsApiVersion')]",
    "dependsOn": [
        "[concat('Microsoft.OperationalInsights/workspaces/', parameters('workspaceName'), '/savedSearches/', variables('SavedSearch').Name, '/schedules/', variables('Schedule').Name)]"
    ],
    "properties": {
        "etag": "*",
        "type": "Alert",
        "name": "[variables('Alert').Name]",
        "description": "[variables('Alert').Description]",
        "severity": "[variables('Alert').Severity]",
        "threshold": {
            "operator": "[variables('Alert').Threshold.Operator]",
            "value": "[variables('Alert').Threshold.Value]",
            "metricsTrigger": {
                "triggerCondition": "[variables('Alert').Threshold.Trigger.Condition]",
                "operator": "[variables('Alert').Trigger.Operator]",
                "value": "[variables('Alert').Trigger.Value]"
            },
        },
        "AzNsNotification": {
            "GroupIds": "[variables('MyAlert').AzNsNotification.GroupIds]",
            "CustomEmailSubject": "[variables('MyAlert').AzNsNotification.CustomEmailSubject]",
            "CustomWebhookPayload": "[variables('MyAlert').AzNsNotification.CustomWebhookPayload]"
        }
    }
}
```

En las tablas siguientes se describen las propiedades para los recursos de acción de alerta.

| Nombre del elemento | Obligatorio | Descripción |
|:--|:--|:--|
| `type` | Sí | Tipo de la acción.  Es **Alert** para las acciones de alerta. |
| `name` | Sí | Nombre para mostrar de la alerta.  Es el nombre que se muestra en la consola para la regla de alerta. |
| `description` | No | Descripción opcional de la alerta. |
| `severity` | Sí | Gravedad del registro de alertas según los siguientes valores:<br><br> **crítica**<br>**advertencia**<br>**informativa**

#### <a name="threshold"></a>Umbral
Esta sección es obligatoria. Define las propiedades para el umbral de alerta.

| Nombre del elemento | Obligatorio | Descripción |
|:--|:--|:--|
| `Operator` | Sí | Operador para la comparación según los valores siguientes:<br><br>**gt = mayor que<br>lt = menor que** |
| `Value` | Sí | Valor para comparar los resultados. |

##### <a name="metricstrigger"></a>MetricsTrigger
Esta sección es opcional. Inclúyala para una alerta de unidades métricas.

| Nombre del elemento | Obligatorio | Descripción |
|:--|:--|:--|
| `TriggerCondition` | Sí | Especifica si el umbral es para el número total de infracciones o para infracciones consecutivas con los siguientes valores:<br><br>**Total<br>Consecutive** (Total, Consecutivos) |
| `Operator` | Sí | Operador para la comparación según los valores siguientes:<br><br>**gt = mayor que<br>lt = menor que** |
| `Value` | Sí | Número de veces que se deben cumplir los criterios para desencadenar la alerta. |


#### <a name="throttling"></a>Limitaciones
Esta sección es opcional. Incluya esta sección si desea suprimir alertas en la misma regla durante cierto tiempo después de crear una alerta.

| Nombre del elemento | Obligatorio | Descripción |
|:--|:--|:--|
| DurationInMinutes | Sí, si hay una limitación de elementos incluida | Número de minutos para suprimir alertas después de crear una en la misma regla de alerta. |

#### <a name="azure-action-group"></a>Grupo de acciones de Azure
Todas las alertas de Azure usan el grupo de acciones como el mecanismo predeterminado para controlar las acciones. Con el grupo de acciones, puede especificar las acciones una vez y, luego, asociar el grupo de acciones a varias alertas en todo Azure, sin la necesidad de declarar repetidamente una y otra vez las mismas acciones. Los grupos de acciones admiten varias acciones, incluido el correo electrónico, SMS, llamadas de voz, conexión ITSM, runbook de Automation, URI de webhook, etc.

Para los usuarios que han extendido sus alertas a Azure, ahora una programación tendrá los detalles del grupo de acciones junto con el umbral, para así poder crear una alerta. Antes de crear una alerta, es necesario definir los detalles de correo electrónico, las direcciones URL de webhooks, los detalles de automatización de runbooks y otras acciones dentro de un grupo de acciones. Es posible crear un [grupo de acciones desde Azure Monitor](../../azure-monitor/platform/action-groups.md) en el portal o usar el [grupo de acciones: plantilla de recursos](../../azure-monitor/platform/action-groups-create-resource-manager-template.md).

| Nombre del elemento | Obligatorio | Descripción |
|:--|:--|:--|
| AzNsNotification | Sí | El identificador de recurso del grupo de acciones de Azure que se asociará con la alerta para realizar las acciones necesarias cuando se cumplan los criterios de alerta. |
| CustomEmailSubject | No | Línea de asunto personalizada del correo enviado a todas las direcciones especificadas en el grupo de acciones asociado. |
| CustomWebhookPayload | No | Carga personalizada para enviarse a todos los puntos de conexión de webhook definidos en el grupo de acciones asociadas. El formato depende de lo que espera el webhook y debe ser un valor JSON serializado válido. |

## <a name="sample"></a>Muestra

Aquí se muestra un ejemplo de una solución que incluye los siguientes recursos:

- Búsqueda guardada
- Programación
- Grupo de acciones

En el ejemplo se utilizan variables de [parámetros de solución estándar]( solutions-solution-file.md#parameters) que se suelen utilizar en una solución en lugar de codificar valores en las definiciones de recursos.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0",
    "parameters": {
        "workspaceName": {
            "type": "string",
            "metadata": {
                "Description": "Name of Log Analytics workspace"
            }
        },
        "workspaceregionId": {
            "type": "string",
            "metadata": {
                "Description": "Region of Log Analytics workspace"
            }
        },
        "actiongroup": {
            "type": "string",
            "metadata": {
                "Description": "List of action groups for alert actions separated by semicolon"
            }
        }
    },
    "variables": {
        "SolutionName": "MySolution",
        "SolutionVersion": "1.0",
        "SolutionPublisher": "Contoso",
        "ProductName": "SampleSolution",
        "LogAnalyticsApiVersion-Search": "2017-03-15-preview",
        "LogAnalyticsApiVersion-Solution": "2015-11-01-preview",
        "MySearch": {
            "displayName": "Error records by hour",
            "query": "MyRecord_CL | summarize AggregatedValue = avg(Rating_d) by Instance_s, bin(TimeGenerated, 60m)",
            "category": "Samples",
            "name": "Samples-Count of data"
        },
        "MyAlert": {
            "Name": "[toLower(concat('myalert-',uniqueString(resourceGroup().id, deployment().name)))]",
            "DisplayName": "My alert rule",
            "Description": "Sample alert. Fires when 3 error records found over hour interval.",
            "Severity": "critical",
            "ThresholdOperator": "gt",
            "ThresholdValue": 3,
            "Schedule": {
                "Name": "[toLower(concat('myschedule-',uniqueString(resourceGroup().id, deployment().name)))]",
                "Interval": 15,
                "TimeSpan": 60
            },
            "MetricsTrigger": {
                "TriggerCondition": "Consecutive",
                "Operator": "gt",
                "Value": 3
            },
            "ThrottleMinutes": 60,
            "AzNsNotification": {
                "GroupIds": [
                    "[parameters('actiongroup')]"
                ],
                "CustomEmailSubject": "Sample alert"
            }
        }
    },
    "resources": [
        {
            "name": "[concat(variables('SolutionName'), '[' ,parameters('workspacename'), ']')]",
            "location": "[parameters('workspaceRegionId')]",
            "tags": { },
            "type": "Microsoft.OperationsManagement/solutions",
            "apiVersion": "[variables('LogAnalyticsApiVersion-Solution')]",
            "dependsOn": [
                "[resourceId('Microsoft.OperationalInsights/workspaces/savedSearches', parameters('workspacename'), variables('MySearch').Name)]",
                "[resourceId('Microsoft.OperationalInsights/workspaces/savedSearches/schedules', parameters('workspacename'), variables('MySearch').Name, variables('MyAlert').Schedule.Name)]",
                "[resourceId('Microsoft.OperationalInsights/workspaces/savedSearches/schedules/actions', parameters('workspacename'), variables('MySearch').Name, variables('MyAlert').Schedule.Name, variables('MyAlert').Name)]"
            ],
            "properties": {
                "workspaceResourceId": "[resourceId('Microsoft.OperationalInsights/workspaces', parameters('workspacename'))]",
                "referencedResources": [
                ],
                "containedResources": [
                    "[resourceId('Microsoft.OperationalInsights/workspaces/savedSearches', parameters('workspacename'), variables('MySearch').Name)]",
                    "[resourceId('Microsoft.OperationalInsights/workspaces/savedSearches/schedules', parameters('workspacename'), variables('MySearch').Name, variables('MyAlert').Schedule.Name)]",
                    "[resourceId('Microsoft.OperationalInsights/workspaces/savedSearches/schedules/actions', parameters('workspacename'), variables('MySearch').Name, variables('MyAlert').Schedule.Name, variables('MyAlert').Name)]"
                ]
            },
            "plan": {
                "name": "[concat(variables('SolutionName'), '[' ,parameters('workspaceName'), ']')]",
                "Version": "[variables('SolutionVersion')]",
                "product": "[variables('ProductName')]",
                "publisher": "[variables('SolutionPublisher')]",
                "promotionCode": ""
            }
        },
        {
            "name": "[concat(parameters('workspaceName'), '/', variables('MySearch').Name)]",
            "type": "Microsoft.OperationalInsights/workspaces/savedSearches",
            "apiVersion": "[variables('LogAnalyticsApiVersion-Search')]",
            "dependsOn": [ ],
            "tags": { },
            "properties": {
                "etag": "*",
                "query": "[variables('MySearch').query]",
                "displayName": "[variables('MySearch').displayName]",
                "category": "[variables('MySearch').category]"
            }
        },
        {
            "name": "[concat(parameters('workspaceName'), '/', variables('MySearch').Name, '/', variables('MyAlert').Schedule.Name)]",
            "type": "Microsoft.OperationalInsights/workspaces/savedSearches/schedules/",
            "apiVersion": "[variables('LogAnalyticsApiVersion-Search')]",
            "dependsOn": [
                "[concat('Microsoft.OperationalInsights/workspaces/', parameters('workspaceName'), '/savedSearches/', variables('MySearch').Name)]"
            ],
            "properties": {
                "etag": "*",
                "interval": "[variables('MyAlert').Schedule.Interval]",
                "queryTimeSpan": "[variables('MyAlert').Schedule.TimeSpan]",
                "enabled": true
            }
        },
        {
            "name": "[concat(parameters('workspaceName'), '/', variables('MySearch').Name, '/', variables('MyAlert').Schedule.Name, '/', variables('MyAlert').Name)]",
            "type": "Microsoft.OperationalInsights/workspaces/savedSearches/schedules/actions",
            "apiVersion": "[variables('LogAnalyticsApiVersion-Search')]",
            "dependsOn": [
                "[concat('Microsoft.OperationalInsights/workspaces/', parameters('workspaceName'), '/savedSearches/', variables('MySearch').Name, '/schedules/', variables('MyAlert').Schedule.Name)]"
            ],
            "properties": {
                "etag": "*",
                "Type": "Alert",
                "Name": "[variables('MyAlert').DisplayName]",
                "Description": "[variables('MyAlert').Description]",
                "Severity": "[variables('MyAlert').Severity]",
                "Threshold": {
                    "Operator": "[variables('MyAlert').ThresholdOperator]",
                    "Value": "[variables('MyAlert').ThresholdValue]",
                    "MetricsTrigger": {
                        "TriggerCondition": "[variables('MyAlert').MetricsTrigger.TriggerCondition]",
                        "Operator": "[variables('MyAlert').MetricsTrigger.Operator]",
                        "Value": "[variables('MyAlert').MetricsTrigger.Value]"
                    }
                },
                "Throttling": {
                    "DurationInMinutes": "[variables('MyAlert').ThrottleMinutes]"
                },
                "AzNsNotification": {
                    "GroupIds": "[variables('MyAlert').AzNsNotification.GroupIds]",
                    "CustomEmailSubject": "[variables('MyAlert').AzNsNotification.CustomEmailSubject]"
                }
            }
        }
    ]
}
```

El siguiente archivo de parámetros proporciona valores de ejemplo para esta solución.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "workspacename": {
            "value": "myWorkspace"
        },
        "accountName": {
            "value": "myAccount"
        },
        "workspaceregionId": {
            "value": "East US"
        },
        "regionId": {
            "value": "East US 2"
        },
        "pricingTier": {
            "value": "Free"
        },
        "actiongroup": {
            "value": "/subscriptions/3b540246-808d-4331-99aa-917b808a9166/resourcegroups/myTestGroup/providers/microsoft.insights/actiongroups/sample"
        }
    }
}
```

## <a name="next-steps"></a>Pasos siguientes
* [Incorporación de vistas](solutions-resources-views.md) a la solución de administración.
* [Incorporación de runbooks de Automation y otros recursos](solutions-resources-automation.md) a la solución de administración.
