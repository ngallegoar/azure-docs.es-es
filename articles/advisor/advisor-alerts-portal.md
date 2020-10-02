---
title: Creación de alertas de Azure Advisor para nuevas recomendaciones mediante Azure Portal
description: Creación de alertas de Azure Advisor para nuevas recomendaciones
ms.topic: article
ms.date: 09/09/2019
ms.openlocfilehash: 837f12a12e532902ee76e345afa430f758f79c10
ms.sourcegitcommit: bdd5c76457b0f0504f4f679a316b959dcfabf1ef
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/22/2020
ms.locfileid: "90973651"
---
# <a name="create-azure-advisor-alerts-on-new-recommendations-using-the-azure-portal"></a>Creación de alertas de Azure Advisor para nuevas recomendaciones mediante Azure Portal 

En este artículo se muestra cómo configurar una alerta para nuevas recomendaciones desde Azure Advisor mediante Azure Portal. 

Cada vez que Azure Advisor detecta una nueva recomendación para uno de los recursos, se almacena un evento en el [registro de actividad de Azure](../azure-monitor/platform/platform-logs-overview.md). Puede configurar alertas para estos eventos desde Azure Advisor creando alertas específicas para la recomendación. Puede seleccionar una suscripción y, si lo desea, un grupo de recursos para especificar los recursos sobre los que desea recibir alertas. 

También puede determinar los tipos de recomendaciones mediante el uso de estas propiedades:

* Category
* Nivel de impacto
* Tipo de recomendación

También puede configurar la acción que tendrá lugar cuando se desencadene una alerta si:  

* Selecciona un grupo de acciones existente.
* Crea un nuevo grupo de acciones.

Para más información sobre los grupos de acciones, consulte [Creación y administración de grupos de acciones](../azure-monitor/platform/action-groups.md).

> [!NOTE] 
> Las alertas de Advisor solo están disponibles para las recomendaciones de alta disponibilidad, rendimiento y costo. No se admiten recomendaciones de seguridad. 

## <a name="create-alert-rule"></a>Crear regla de alertas
1. En el **portal**, seleccione **Azure Advisor**.

    ![Azure Advisor en el portal](./media/advisor-alerts/create1.png)

2. En la sección **Supervisión** del menú de la izquierda, seleccione **Alertas**. 

    ![Alertas en Advisor](./media/advisor-alerts/create2.png)

3. Seleccione **Nueva alerta de Advisor**.

    ![Nueva alerta de Advisor](./media/advisor-alerts/create3.png)

4. En la sección **Ámbito**, seleccione la suscripción y, opcionalmente, el grupo de recursos en el que desea recibir alertas. 

    ![Ámbito alerta de Advisor](./media/advisor-alerts/create4.png)

5. En sección **Condición**, seleccione el método que desea usar para configurar la alerta. Si desea alertar sobre todas las recomendaciones para una categoría determinada o un nivel de impacto, seleccione **Categoría y nivel de impacto**. Si desea alertar sobre todas las recomendaciones de un tipo determinado, seleccione **Tipo de recomendación**.

    ![Condición de alerta de Azure Advisor](./media/advisor-alerts/create5.png)

6. En función de la opción Configurar por que seleccione, podrá especificar los criterios. Si desea todas las recomendaciones, deje en blanco los campos restantes. 

    ![Grupo de acciones de alerta de Advisor](./media/advisor-alerts/create6.png)

7. En la sección **grupos de acciones**, seleccione **Agregar existente** para usar un grupo de acciones que ya haya creado o seleccione **Crear nuevo** para configurar un nuevo [grupo de acciones](../azure-monitor/platform/action-groups.md). 

    ![Agregar alerta existente de Advisor](./media/advisor-alerts/create7.png)

8. En la sección Detalles de alerta, asigne un nombre y una descripción breve a la alerta. Si desea que la alerta esté habilitada, deje **Habilitar regla tras la creación** establecida en **Sí**. A continuación, seleccione el grupo de recursos en el que se guardará la alerta. Esto no afectará al ámbito de destino de la recomendación. 

    :::image type="content" source="./media/advisor-alerts/create8.png" alt-text="Captura de pantalla de la sección de detalles de alertas.":::


## <a name="configure-recommendation-alerts-to-use-a-webhook"></a>Configuración de alertas de recomendación para usar un webhook
En este artículo se muestra cómo configurar las alertas de Azure Advisor para que envíen datos de recomendación mediante webhooks a un sistema de notificación existente. 

Puede configurar alertas para recibir notificaciones cuando tenga una nueva recomendación de Advisor en uno de sus recursos. Estas alertas pueden avisarle por correo electrónico o mensaje de texto, pero también pueden usarse para integrarse con los sistemas existentes a través de un webhook. 


### <a name="using-the-advisor-recommendation-alert-payload"></a>Uso de la carga de alerta de recomendación de Advisor
Si desea integrar las alertas de Advisor en sus propios sistemas mediante un webhook, deberá analizar la carga de JSON que se envía desde la notificación. 

Al configurar el grupo de acciones para esta alerta, seleccione si desea usar el esquema de alerta común. Si selecciona el esquema de alerta común, la carga tendrá el siguiente aspecto: 

```json
{  
   "schemaId":"azureMonitorCommonAlertSchema",
   "data":{  
      "essentials":{  
         "alertId":"/subscriptions/<subid>/providers/Microsoft.AlertsManagement/alerts/<alerted>",
         "alertRule":"Webhhook-test",
         "severity":"Sev4",
         "signalType":"Activity Log",
         "monitorCondition":"Fired",
         "monitoringService":"Activity Log - Recommendation",
         "alertTargetIDs":[  
            "/subscriptions/<subid>/resourcegroups/<resource group name>/providers/microsoft.dbformariadb/servers/<resource name>"
         ],
         "originAlertId":"001d8b40-5d41-4310-afd7-d65c9d4428ed",
         "firedDateTime":"2019-07-17T23:00:57.3858656Z",
         "description":"A new recommendation is available.",
         "essentialsVersion":"1.0",
         "alertContextVersion":"1.0"
      },
      "alertContext":{  
         "channels":"Operation",
         "claims":"{\"http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress\":\"Microsoft.Advisor\"}",
         "caller":"Microsoft.Advisor",
         "correlationId":"8554b847-2a72-48ef-9776-600aca3c3aab",
         "eventSource":"Recommendation",
         "eventTimestamp":"2019-07-17T22:28:54.1566942+00:00",
         "httpRequest":"{\"clientIpAddress\":\"0.0.0.0\"}",
         "eventDataId":"001d8b40-5d41-4310-afd7-d65c9d4428ed",
         "level":"Informational",
         "operationName":"Microsoft.Advisor/recommendations/available/action",
         "properties":{  
            "recommendationSchemaVersion":"1.0",
            "recommendationCategory":"Performance",
            "recommendationImpact":"Medium",
            "recommendationName":"Increase the MariaDB server vCores",
            "recommendationResourceLink":"https://portal.azure.com/#blade/Microsoft_Azure_Expert/RecommendationListBlade/source/ActivityLog/recommendationTypeId/a5f888e3-8cf4-4491-b2ba-b120e14eb7ce/resourceId/%2Fsubscriptions%<subscription id>%2FresourceGroups%2<resource group name>%2Fproviders%2FMicrosoft.DBforMariaDB%2Fservers%2F<resource name>",
            "recommendationType":"a5f888e3-8cf4-4491-b2ba-b120e14eb7ce"
         },
         "status":"Active",
         "subStatus":"",
         "submissionTimestamp":"2019-07-17T22:28:54.1566942+00:00"
      }
   }
}
  ```

Si no usa el esquema común, la carga es similar a la siguiente: 

```json
{  
   "schemaId":"Microsoft.Insights/activityLogs",
   "data":{  
      "status":"Activated",
      "context":{  
         "activityLog":{  
            "channels":"Operation",
            "claims":"{\"http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress\":\"Microsoft.Advisor\"}",
            "caller":"Microsoft.Advisor",
            "correlationId":"3ea7320f-c002-4062-adb8-96d3bd92a5f4",
            "description":"A new recommendation is available.",
            "eventSource":"Recommendation",
            "eventTimestamp":"2019-07-17T20:36:39.3966926+00:00",
            "httpRequest":"{\"clientIpAddress\":\"0.0.0.0\"}",
            "eventDataId":"a12b8e59-0b1d-4003-bfdc-3d8152922e59",
            "level":"Informational",
            "operationName":"Microsoft.Advisor/recommendations/available/action",
            "properties":{  
               "recommendationSchemaVersion":"1.0",
               "recommendationCategory":"Performance",
               "recommendationImpact":"Medium",
               "recommendationName":"Increase the MariaDB server vCores",
               "recommendationResourceLink":"https://portal.azure.com/#blade/Microsoft_Azure_Expert/RecommendationListBlade/source/ActivityLog/recommendationTypeId/a5f888e3-8cf4-4491-b2ba-b120e14eb7ce/resourceId/%2Fsubscriptions%2F<subscription id>%2FresourceGroups%2F<resource group name>%2Fproviders%2FMicrosoft.DBforMariaDB%2Fservers%2F<resource name>",
               "recommendationType":"a5f888e3-8cf4-4491-b2ba-b120e14eb7ce"
            },
            "resourceId":"/subscriptions/<subscription id>/resourcegroups/<resource group name>/providers/microsoft.dbformariadb/servers/<resource name>",
            "resourceGroupName":"<resource group name>",
            "resourceProviderName":"MICROSOFT.DBFORMARIADB",
            "status":"Active",
            "subStatus":"",
            "subscriptionId":"<subscription id>",
            "submissionTimestamp":"2019-07-17T20:36:39.3966926+00:00",
            "resourceType":"MICROSOFT.DBFORMARIADB/SERVERS"
         }
      },
      "properties":{  
 
      }
   }
}
```

Para identificar los eventos de recomendación de Advisor en cualquiera de los esquemas, busque si **eventSource** es `Recommendation` y **OperationName**  es `Microsoft.Advisor/recommendations/available/action`.

Algunos de los otros campos importantes que puede querer usar son: 

* *alertTargetIDs* (en el esquema común) o *resourceId* (esquema heredado)
* *recommendationType*
* *recommendationName*
* *recommendationCategory*
* *recommendationImpact*
* *recommendationResourceLink*


## <a name="manage-your-alerts"></a>Administración de alertas 

Desde Azure Advisor, puede editar, eliminar o deshabilitar y habilitar las alertas de recomendaciones. 

1. En el **portal**, seleccione **Azure Advisor**.

    :::image type="content" source="./media/advisor-alerts/create1.png" alt-text="Captura de pantalla de la sección de detalles de alertas.":::

2. En la sección **Supervisión** del menú de la izquierda, seleccione **Alertas**.

    :::image type="content" source="./media/advisor-alerts/create2.png" alt-text="Captura de pantalla de la sección de detalles de alertas.":::

3. Para editar una alerta, haga clic en el nombre para abrirla y edite los campos que desee.

4. Para eliminar, habilitar o deshabilitar una alerta, haga clic en los puntos suspensivos al final de la fila y seleccione la acción que desea realizar.
 

## <a name="next-steps"></a>Pasos siguientes
- Consulte la [introducción a las alertas del registro de actividad](../azure-monitor/platform/alerts-overview.md) y aprenda cómo puede recibir alertas.
- Más información sobre los [grupos de acciones](../azure-monitor/platform/action-groups.md).
