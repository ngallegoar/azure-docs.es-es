---
title: Recepción de alertas de registro de actividad en las notificaciones del servicio de Azure
description: Reciba notificaciones por SMS, correo electrónico o webhook cuando se produzcan eventos en el servicio de Azure.
ms.topic: conceptual
ms.date: 06/27/2019
ms.openlocfilehash: d318adc76959ac24f4be9946167965a83053f632
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "75749309"
---
# <a name="create-activity-log-alerts-on-service-notifications"></a>Creación de alertas del registro de actividad en notificaciones del servicio
## <a name="overview"></a>Información general

En este artículo se explica cómo configurar las alertas del registro de actividad para las notificaciones de mantenimiento de un servicio mediante Azure Portal.  

Las notificaciones de mantenimiento del servicio se almacenan en el [registro de actividad de Azure](../azure-monitor/platform/platform-logs-overview.md). Debido al volumen posiblemente grande de la información almacenada en el registro de actividad, hay una interfaz de usuario independiente que facilita la visualización y la configuración de alertas en las notificaciones de estado del servicio. 

Puede recibir una alerta cuando Azure envía notificaciones de estado del servicio a la suscripción de Azure. Puede configurar la alerta en función de:

- La clase de notificación de estado del servicio (problemas de servicio, mantenimiento planificado y avisos de estado).
- La suscripción afectada.
- Los servicios afectados.
- Las regiones afectadas.

> [!NOTE]
> Las notificaciones de estado del servicio no envían una alerta relativa a los eventos de estado de recursos.

También puede configurar a quién se debe enviar la alerta:

- Seleccione un grupo de acciones existente.
- Cree un nuevo grupo de acciones (que puede usarse para futuras alertas).

Para más información sobre los grupos de acciones, consulte [Creación y administración de grupos de acciones](../azure-monitor/platform/action-groups.md).

Para obtener información sobre cómo configurar las alertas de notificación de mantenimiento del servicio mediante plantillas de Azure Resource Manager, consulte [Plantillas de Resource Manager](../azure-monitor/platform/alerts-activity-log.md).

### <a name="watch-a-video-on-setting-up-your-first-azure-service-health-alert"></a>Ver un vídeo sobre cómo configurar su primera alerta de Azure Service Health

>[!VIDEO https://www.microsoft.com/en-us/videoplayer/embed/RE2OaXt]

## <a name="alert-and-new-action-group-using-azure-portal"></a>Alerta y nuevo grupo de acciones mediante Azure Portal
1. En el [portal](https://portal.azure.com), seleccione **Estado del servicio**.

    ![El servicio "Estado del servicio"](media/alerts-activity-log-service-notifications/home-servicehealth.png)

1. En la sección **Alertas**, seleccione **Alertas de estado**.

    ![La pestaña "Alertas de estado"](media/alerts-activity-log-service-notifications/alerts-blades-sh.png)

1. Seleccione **Crear alerta de estado de servicio** y rellene los campos.

    ![El comando "Crear alerta de estado de servicio"](media/alerts-activity-log-service-notifications/service-health-alert.png)

1. Seleccione la **suscripción**, los **servicios** y las **regiones** para los que desea recibir alertas.

    ![Cuadro de diálogo "Agregar alerta de registro de actividad"](media/alerts-activity-log-service-notifications/activity-log-alert-new-ux.png)

    > [!NOTE]
    > Esta suscripción se usa para guardar la alerta del registro de actividad. El recurso de la alerta se implementa en esta suscripción y supervisa los eventos en el registro de actividad para dicho recurso.

1. Elija los **Tipos de evento** para los que quiere recibir alertas: *Problema de servicio*, *Mantenimiento planeado* y *Avisos de estado* 

1. Para definir los detalles de la alerta, escriba un **nombre de regla de alertas** y una **descripción**.

1. Seleccione el **grupo de recursos** donde quiere guardar la alerta.

1. Para crear un nuevo grupo de acciones, seleccione **Nuevo grupo de acciones**. Escriba un nombre en el cuadro de texto **Nombre del grupo de acciones** y especifique un nombre en el cuadro de texto **Nombre corto**. Se hace referencia al nombre corto en las notificaciones que se envían cuando se desencadena esta alerta.

    ![Creación de un nuevo grupo de acciones](media/alerts-activity-log-service-notifications/action-group-creation.png)

1. A continuación, defina una lista de destinatarios proporcionando:

    a. **Name**: escriba el nombre, alias o identificador del destinatario.

    b. **Tipo de acción**: seleccione SMS, correo electrónico, webhook, aplicación de Azure, etc.

    c. **Detalles**: según el tipo de acción elegido, proporcione un número de teléfono, una dirección de correo electrónico, un identificador URI de webhook, etc.

1. Seleccione **Aceptar** para crear el grupo de acciones y luego en **Crear regla de alertas** para completar la alerta.

En unos minutos, la alerta está activa y comienza a desencadenarse en función de las condiciones especificadas durante la creación.

Obtenga información acerca de cómo [configurar notificaciones de webhook para los sistemas de administración de problemas existentes](service-health-alert-webhook-guide.md). Para obtener información sobre el esquema de webhook para las alertas del registro de actividad, consulte [Webhooks para alertas del registro de actividad de Azure](../azure-monitor/platform/activity-log-alerts-webhook.md).

>[!NOTE]
>El grupo de acciones definido en estos pasos es reutilizable, como grupo de acciones existente, en todas las futuras definiciones de alertas.
>

## <a name="alert-with-existing-action-group-using-azure-portal"></a>Alerta con un grupo de acciones existente mediante Azure Portal

1. Siga los pasos del 1 al 6 de la sección anterior para crear la notificación de mantenimiento del servicio. 

1. En **Definir grupo de acciones**, haga clic en el botón **Seleccionar grupo de acciones**. Seleccione el grupo adecuado.

1. Seleccione **Agregar** para agregar el grupo de acciones y luego **Crear regla de alertas** para completar la alerta.

En unos minutos, la alerta está activa y comienza a desencadenarse en función de las condiciones especificadas durante la creación.

## <a name="alert-and-new-action-group-using-the-azure-resource-manager-templates"></a>Alerta y nuevo grupo de acciones mediante las plantillas de Azure Resource Manager

En el siguiente ejemplo se crea un grupo de acciones con un destino de correo electrónico y habilita todas las notificaciones de estado de servicio para la suscripción de destino.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "actionGroups_name": {
            "defaultValue": "SubHealth",
            "type": "String"
        },
        "activityLogAlerts_name": {
            "defaultValue": "ServiceHealthActivityLogAlert",
            "type": "String"
        },
        "emailAddress":{
            "type":"string"
        }
    },
    "variables": {
        "alertScope":"[concat('/','subscriptions','/',subscription().subscriptionId)]"
    },
    "resources": [
        {
            "comments": "Action Group",
            "type": "microsoft.insights/actionGroups",
            "name": "[parameters('actionGroups_name')]",
            "apiVersion": "2017-04-01",
            "location": "Global",
            "tags": {},
            "scale": null,
            "properties": {
                "groupShortName": "[parameters('actionGroups_name')]",
                "enabled": true,
                "emailReceivers": [
                    {
                        "name": "[parameters('actionGroups_name')]",
                        "emailAddress": "[parameters('emailAddress')]"
                    }
                ],
                "smsReceivers": [],
                "webhookReceivers": []
            },
            "dependsOn": []
        },
        {
            "comments": "Service Health Activity Log Alert",
            "type": "microsoft.insights/activityLogAlerts",
            "name": "[parameters('activityLogAlerts_name')]",
            "apiVersion": "2017-04-01",
            "location": "Global",
            "tags": {},
            "scale": null,
            "properties": {
                "scopes": [
                    "[variables('alertScope')]"
                ],
                "condition": {
                    "allOf": [
                        {
                            "field": "category",
                            "equals": "ServiceHealth"
                        },
                        {
                            "field": "properties.incidentType",
                            "equals": "Incident"
                        }
                    ]
                },
                "actions": {
                    "actionGroups": [
                        {
                            "actionGroupId": "[resourceId('microsoft.insights/actionGroups', parameters('actionGroups_name'))]",
                            "webhookProperties": {}
                        }
                    ]
                },
                "enabled": true,
                "description": ""
            },
            "dependsOn": [
                "[resourceId('microsoft.insights/actionGroups', parameters('actionGroups_name'))]"
            ]
        }
    ]
}
```

## <a name="manage-your-alerts"></a>Administración de alertas

Después de crear una alerta, es visible en la sección **Alertas** de **Supervisión**. Seleccione la alerta que desea administrar para:

* Editarla.
* Eliminarla.
* Deshabilitarla o habilitarla, si desea detener temporalmente o reanudar la recepción de notificaciones de la alerta.

## <a name="next-steps"></a>Pasos siguientes
- Obtenga información sobre los [procedimientos recomendados para la configuración de alertas de Azure Service Health](https://www.microsoft.com/en-us/videoplayer/embed/RE2OtUa).
- Obtenga información sobre cómo [configurar notificaciones de inserción móviles de Azure Service Health](https://www.microsoft.com/en-us/videoplayer/embed/RE2OtUw).
- Obtenga información acerca de cómo [configurar notificaciones de webhook para los sistemas de administración de problemas existentes](service-health-alert-webhook-guide.md).
- Más información acerca de las [Notificaciones del estado del servicio](service-notifications.md).
- Más información sobre la [Limitación del número de notificaciones](../azure-monitor/platform/alerts-rate-limiting.md).
- Revise el [Esquema de webhook de alertas del registro de actividad](../azure-monitor/platform/activity-log-alerts-webhook.md).
- Consulte la [introducción a las alertas del registro de actividad](../azure-monitor/platform/alerts-overview.md) y aprenda cómo puede recibir alertas.
- Más información sobre los [grupos de acciones](../azure-monitor/platform/action-groups.md).
