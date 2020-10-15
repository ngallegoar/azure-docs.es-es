---
title: Envío de alertas de Azure Service Health con OpsGenie mediante webhooks
description: Obtenga notificaciones personalizadas sobre los eventos del estado de servicio en la instancia de OpsGenie.
ms.topic: conceptual
ms.date: 06/10/2019
ms.openlocfilehash: 112774cb1f9e16b08225471e8dbc1bb79b1bd37d
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "86529104"
---
# <a name="send-azure-service-health-alerts-with-opsgenie-using-webhooks"></a>Envío de alertas de Azure Service Health con OpsGenie mediante webhooks

Este artículo muestra cómo configurar las alertas de estado del servicio de Azure con OpsGenie mediante un webhook. Mediante el uso de la integración del estado del servicio de Azure de [OpsGenie](https://www.opsgenie.com/) puede reenviar alertas de estado del servicio de Azure a OpsGenie. OpsGenie puede determinar las personas adecuadas a las que enviar la notificación según las programaciones de entrega mediante correo electrónico, mensajes de texto (SMS), llamadas de teléfono, notificaciones de inserción de iOS y Android y la escalación de alertas hasta que la alerta se confirma o se cierra.

## <a name="creating-a-service-health-integration-url-in-opsgenie"></a>Creación de una dirección URL de integración de estado del servicio en OpsGenie
1.  Asegúrese de que se ha registrado y ha iniciado sesión en su cuenta de [OpsGenie](https://www.opsgenie.com/).

1.  Vaya a la sección **Integrations** (Integraciones) en OpsGenie.

    ![La sección "Integrations" (Integraciones) en OpsGenie](./media/webhook-alerts/opsgenie-integrations-section.png)

1.  Seleccione el botón de integración **Azure Service Health** (Estado del servicio de Azure).

    ![El botón "Azure Service Health" (Estado del servicio Azure) en OpsGenie](./media/webhook-alerts/opsgenie-azureservicehealth-button.png)

1.  **Nombre** la alerta y especifique el campo **Asignado al equipo**.

1.  Rellene los demás campos como **Recipients** (Destinatarios), **Enabled** (Habilitado) y **Suppress Notifications** (Suprimir notificaciones).

1.  Copie y guarde la **dirección URL**, ya que debe contener la `apiKey` anexada al final.

    ![La "dirección URL de integración" en OpsGenie](./media/webhook-alerts/opsgenie-integration-url.png)

1.  Seleccione **Save Integration** (Guardar integración).

## <a name="create-an-alert-using-opsgenie-in-the-azure-portal"></a>Creación de una alerta con OpsGenie en Azure Portal
### <a name="for-a-new-action-group"></a>Para un nuevo grupo de acciones:
1. Siga los pasos del 1 al 8 en [Creación de una alerta basada en una notificación de mantenimiento del servicio para un nuevo grupo de acciones con Azure Portal](./alerts-activity-log-service-notifications-portal.md).

1. Defina la lista de **acciones**:

    a. **Tipo de acción:** *Webhook*

    b. **Detalles:** la **dirección URL de integración** de OpsGenie guardada anteriormente.

    c. **Nombre:** el identificador, alias o nombre de webhook.

1. Seleccione **Guardar** cuando termine para crear la alerta.

### <a name="for-an-existing-action-group"></a>Para un grupo de acciones existentes:
1. En [Azure Portal](https://portal.azure.com/), seleccione **Supervisión**.

1. En la sección **Configuración**, seleccione **Grupos de acciones**.

1. Busque el grupo de acciones que desee editar.

1. Defina la lista de **acciones**:

    a. **Tipo de acción:** *Webhook*

    b. **Detalles:** la **dirección URL de integración** de OpsGenie guardada anteriormente.

    c. **Nombre:** el identificador, alias o nombre de webhook.

1. Cuando termine de actualizar el grupo de acciones, seleccione **Guardar**.

## <a name="testing-your-webhook-integration-via-an-http-post-request"></a>Prueba de la integración de webhook a través de una solicitud HTTP POST
1. Cree la carga de estado del servicio que desee enviar. Puede encontrar una carga de webhook de estado del servicio de ejemplo en [Webhooks para alertas del registro de actividad de Azure](../azure-monitor/platform/activity-log-alerts-webhook.md).

1. Cree una solicitud HTTP POST de la siguiente manera:

    ```
    POST        https://api.opsgenie.com/v1/json/azureservicehealth?apiKey=<APIKEY>

    HEADERS     Content-Type: application/json

    BODY        <service health payload>
    ```
1. Debe recibir una respuesta `200 OK` con el mensaje de estado "correcto".

1. Vaya a [OpsGenie](https://www.opsgenie.com/) para confirmar que la integración se ha configurado correctamente.

## <a name="next-steps"></a>Pasos siguientes
- Obtenga información acerca de cómo [configurar notificaciones de webhook para los sistemas de administración de problemas existentes](service-health-alert-webhook-guide.md).
- Revise el [Esquema de webhook de alertas del registro de actividad](../azure-monitor/platform/activity-log-alerts-webhook.md). 
- Más información acerca de las [Notificaciones del estado del servicio](./service-notifications.md).
- Más información sobre los [grupos de acciones](../azure-monitor/platform/action-groups.md).
