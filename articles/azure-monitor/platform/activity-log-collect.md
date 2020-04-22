---
title: Recopilar y analizar el registro de actividad de Azure en Azure Monitor
description: Recopile el registro de actividad de Azure en los registros de Azure Monitor y utilice la solución de supervisión para analizar tal registro y hacer búsquedas en él en todas las suscripciones de Azure.
ms.subservice: logs
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 04/14/2020
ms.openlocfilehash: 098aeaa06a26c57744402722aa3eacc51ea85fb7
ms.sourcegitcommit: ea006cd8e62888271b2601d5ed4ec78fb40e8427
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/14/2020
ms.locfileid: "81382866"
---
# <a name="collect-and-analyze-azure-activity-log-in-azure-monitor"></a>Recopilar y analizar el registro de actividad de Azure en Azure Monitor
El [Registro de actividad de Azure](platform-logs-overview.md) es un [registro de plataforma](platform-logs-overview.md) que proporciona información de los eventos de nivel de suscripción que se han producido en Azure. Aunque puede ver el registro de actividad en Azure Portal, debe configurarlo para enviarlo a un área de trabajo de Log Analytics para habilitar características adicionales de Azure Monitor. En este artículo se describe cómo realizar esta configuración y cómo enviar el registro de actividad a Azure Storage y Event Hubs.

Recopilar el registro de actividad en un área de trabajo de Log Analytics proporciona las siguientes ventajas:

- No se generan cargos por ingesta ni retención de datos para los datos del registro de actividad almacenados en un área de trabajo de Log Analytics.
- Correlacionar los datos del registro de actividad con otros datos de supervisión recopilados por Azure Monitor.
- Usar las consultas de registro para realizar un análisis complejo y obtener información detallada sobre las entradas del registro de actividad.
- Usar las alertas de registro con entradas de actividad, lo que permite una lógica de alertas más compleja.
- Almacenar las entradas del registro de actividad durante más de 90 días.
- Consolidar las entradas de registro de varias suscripciones e inquilinos de Azure en una ubicación para su análisis conjunto.

> [!IMPORTANT]
> La recopilación de registros entre inquilinos requiere [Azure Lighthouse](/azure/lighthouse).

## <a name="collecting-activity-log"></a>Recopilación del registro de actividad
El registro de actividad se recopila automáticamente para [verlo en Azure Portal](activity-log-view.md). Para recopilarlo en un área de trabajo de Log Analytics o enviarlo a Azure Storage o Event Hubs, cree una [configuración de diagnóstico](diagnostic-settings.md). Este es el mismo método que usan los registros de recursos, lo que hace que sea coherente para todos los [registros de plataforma](platform-logs-overview.md).  

Para crear una configuración de diagnóstico para el registro de actividad, seleccione **Configuración de diagnóstico** en el menú **Registro de actividad** de Azure Monitor. Consulte [Creación de una configuración de diagnóstico para recopilar registros de plataforma y métricas en Azure](diagnostic-settings.md) para obtener más información sobre cómo crear la configuración. Consulte [Categorías del registro de actividad](activity-log-view.md#categories-in-the-activity-log) para obtener una descripción de las categorías que puede filtrar. Si tiene alguna configuración heredada, asegúrese de deshabilitarla antes de crear una configuración de diagnóstico. Si las dos están habilitadas, se pueden producir datos duplicados.

![Configuración de diagnóstico](media/diagnostic-settings-subscription/diagnostic-settings.png)


> [!NOTE]
> Actualmente solo puede crear una configuración de diagnóstico de nivel de suscripción con Azure Portal y una plantilla de Resource Manager. 


## <a name="legacy-settings"></a>Configuración heredada 
Aunque la configuración de diagnóstico es el método preferido para enviar el registro de actividad a distintos destinos, los métodos heredados seguirán funcionando si no elige reemplazarlos por una configuración de diagnóstico. La configuración de diagnóstico tiene las ventajas siguientes respecto a los métodos heredados y se recomienda que actualice la configuración:

- Método coherente para recopilar todos los registros de la plataforma.
- Recopilación del registro de actividad en varias suscripciones e inquilinos.
- Filtrado de la colección para recopilar solo los registros de determinadas categorías.
- Recopilar todas las categorías de registro de actividad. Algunas categorías no se recopilan con el método heredado.
- Latencia más rápida para la ingesta de registros. El método anterior tiene aproximadamente 15 minutos de latencia mientras que la configuración de diagnóstico solo agrega aproximadamente 1 minuto.



### <a name="log-profiles"></a>Perfiles de registro
Los perfiles de registro son el método heredado para enviar el registro de actividad a Azure Storage o Event Hubs. Use el siguiente procedimiento para seguir trabajando con un perfil de registro o deshabilitarlo como preparación para la migración a una configuración de diagnóstico.

1. En el menú **Azure Monitor** de Azure Portal, seleccione **Registro de actividad**.
3. Haga clic en **Configuración de diagnóstico**.

   ![Configuración de diagnóstico](media/diagnostic-settings-subscription/diagnostic-settings.png)

4. Haga clic en el banner de color púrpura de la experiencia heredada.

    ![Experiencia heredada](media/diagnostic-settings-subscription/legacy-experience.png)

### <a name="log-analytics-workspace"></a>Área de trabajo de Log Analytics
El método heredado para recopilar el registro de actividad en un área de trabajo de Log Analytics está conectando el registro en la configuración del área de trabajo. 

1. Desde el menú **Áreas de trabajo de Log Analytics** en Azure Portal, seleccione el área de trabajo para recopilar el registro de actividad.
1. En la sección **Orígenes de datos del área de trabajo** del menú del área de trabajo, seleccione **Registro de actividad de Azure**.
1. Haga clic en la suscripción que desea conectar.

    ![Áreas de trabajo](media/activity-log-collect/workspaces.png)

1. Haga clic en **Conectar** para conectar el registro de actividad en la suscripción al área de trabajo seleccionada. Si la suscripción ya está conectada a otra área de trabajo, haga clic en **Desconectar** primero para desconectarla.

    ![Conexión de áreas de trabajo](media/activity-log-collect/connect-workspace.png)


Para deshabilitar la configuración, realice el mismo procedimiento y haga clic en **Desconectar** para quitar la suscripción del área de trabajo.


## <a name="analyze-activity-log-in-log-analytics-workspace"></a>Análisis del registro de actividad en el área de trabajo de Log Analytics
Cuando conecta un registro de actividad a una área de trabajo de Log Analytics, las entradas se escribirán en el área de trabajo en una tabla denominada *AzureActivity* que puede recuperar con una [consulta de registro](../log-query/log-query-overview.md). La estructura de esta tabla varía en función de la [categoría de la entrada de registro](activity-log-view.md#categories-in-the-activity-log). Consulte [Esquema de eventos del registro de actividad de Azure](activity-log-schema.md) para obtener una descripción de cada categoría.


### <a name="data-structure-changes"></a>Cambios en la estructura de datos
La configuración de diagnóstico recopila los mismos datos que el método heredado que se usa para recopilar el registro de actividad con algunos cambios en la estructura de la tabla *AzureActivity*.

Las columnas de la tabla siguiente están en desuso en el esquema actualizado. Todavía existen en *AzureActivity*, pero no tienen datos. El reemplazo de estas columnas no es nuevo, pero contiene los mismos datos que la columna en desuso. Tienen un formato distinto, por lo que es posible que tenga que modificar las consultas de registro que las utilizan. 

| Columna en desuso | Columna de reemplazo |
|:---|:---|
| ActivityStatus    | ActivityStatusValue    |
| ActivitySubstatus | ActivitySubstatusValue |
| OperationName     | OperationNameValue     |
| ResourceProvider  | ResourceProviderValue  |

> [!IMPORTANT]
> En algunos casos, los valores de estas columnas pueden estar en mayúsculas. Si tiene una consulta que incluye estas columnas, debe usar el [operador =~](https://docs.microsoft.com/azure/kusto/query/datatypes-string-operators) para realizar una comparación sin distinguir entre mayúsculas y minúsculas.

La columna siguiente se ha agregado a *AzureActivity* en el esquema actualizado:

- Authorization_d
- Claims_d
- Properties_d


## <a name="activity-logs-analytics-monitoring-solution"></a>Solución de supervisión de Activity Log Analytics
La solución de supervisión de Azure Log Analytics entrará en desuso pronto y se reemplazará por un libro que use el esquema actualizado en el área de trabajo de Log Analytics. Puede seguir usando la solución si ya está habilitada, pero solo se puede usar si va a recopilar el registro de actividad mediante la configuración heredada. 



### <a name="use-the-solution"></a>Uso de la solución
A las soluciones de supervisión se accede desde el menú **Supervisar** de Azure Portal. Seleccione **Más** en la sección **Información detallada** para abrir la página **Información general** con los iconos de la solución. El icono **Registros de actividad de Azure** muestra un recuento del número de registros **AzureActivity** del área de trabajo.

![Icono Registros de actividad de Azure](media/collect-activity-logs/azure-activity-logs-tile.png)


Haga clic en el icono **Registros de actividad de Azure** para abrir la vista **Registros de actividad de Azure**. La vista incluye los elementos de visualización de la tabla siguiente. Cada uno de ellos muestra hasta otros diez elementos que coinciden con sus criterios para el intervalo de tiempo especificado. Puede ejecutar una consulta de registros que devuelve todos los registros coincidentes; para ello, haga clic en **Ver todo** en la parte inferior del elemento.

![Panel Registros de actividad de Azure](media/collect-activity-logs/activity-log-dash.png)


### <a name="enable-the-solution-for-new-subscriptions"></a>Habilitación de la solución para nuevas suscripciones
Pronto dejará de poder agregar la solución de Activity Logs Analytics a la suscripción mediante Azure Portal. Puede agregarlo mediante el siguiente procedimiento con una plantilla de Resource Manager. 

1. Copie el siguiente JSON en un archivo denominado *ActivityLogTemplate*

    ```json
    {
    "$schema": "https://schema.management.azure.com/schemas/2014-04-01-preview/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "workspaceName": {
            "type": "String",
            "defaultValue": "my-workspace",
            "metadata": {
              "description": "Specifies the name of the workspace."
            }
        },
        "location": {
            "type": "String",
            "allowedValues": [
              "east us",
              "west us",
              "australia central",
              "west europe"
            ],
            "defaultValue": "australia central",
            "metadata": {
              "description": "Specifies the location in which to create the workspace."
            }
        }
      },
        "resources": [
        {
            "type": "Microsoft.OperationalInsights/workspaces",
            "name": "[parameters('workspaceName')]",
            "apiVersion": "2015-11-01-preview",
            "location": "[parameters('location')]",
            "properties": {
                "features": {
                    "searchVersion": 2
                }
            }
        },
        {
            "type": "Microsoft.OperationsManagement/solutions",
            "apiVersion": "2015-11-01-preview",
            "name": "[concat('AzureActivity(', parameters('workspaceName'),')')]",
            "location": "[parameters('location')]",
            "dependsOn": [
                "[resourceId('microsoft.operationalinsights/workspaces', parameters('workspaceName'))]"
            ],
            "plan": {
                "name": "[concat('AzureActivity(', parameters('workspaceName'),')')]",
                "promotionCode": "",
                "product": "OMSGallery/AzureActivity",
                "publisher": "Microsoft"
            },
            "properties": {
                "workspaceResourceId": "[resourceId('microsoft.operationalinsights/workspaces', parameters('workspaceName'))]",
                "containedResources": [
                    "[concat(resourceId('microsoft.operationalinsights/workspaces', parameters('workspaceName')), '/views/AzureActivity(',parameters('workspaceName'))]"
                ]
            }
        },
        {
          "type": "Microsoft.OperationalInsights/workspaces/datasources",
          "kind": "AzureActivityLog",
          "name": "[concat(parameters('workspaceName'), '/', subscription().subscriptionId)]",
          "apiVersion": "2015-11-01-preview",
          "location": "[parameters('location')]",
          "dependsOn": [
              "[parameters('WorkspaceName')]"
          ],
          "properties": {
              "linkedResourceId": "[concat(subscription().Id, '/providers/microsoft.insights/eventTypes/management')]"
          }
        }
      ]
    }    
    ```

2. Implemente la plantilla con los siguientes comandos de PowerShell:

    ```PowerShell
    Connect-AzAccount
    Select-AzSubscription <SubscriptionName>
    New-AzResourceGroupDeployment -Name activitysolution -ResourceGroupName <ResourceGroup> -TemplateFile <Path to template file>
    ```


## <a name="next-steps"></a>Pasos siguientes

- Obtenga más información sobre el [registro de actividad](platform-logs-overview.md).
- Más información sobre la [plataforma de datos de Azure Monitor](data-platform.md).
- Use las [consultas de registros](../log-query/log-query-overview.md) para ver información detallada de los registros de actividad.
