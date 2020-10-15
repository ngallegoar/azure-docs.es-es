---
title: Envío de notificaciones de Azure Service Health mediante webhooks
description: Envíe notificaciones personalizadas de eventos de Service Health a un sistema de administración de problemas existente.
ms.topic: conceptual
ms.service: service-health
ms.date: 3/27/2018
ms.openlocfilehash: 05b0572c89a29fddc881f9977ee437d1319e6254
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "86518935"
---
# <a name="use-a-webhook-to-configure-health-notifications-for-problem-management-systems"></a>Uso de un webhook para configurar las notificaciones relativas al estado en sistemas de administración de problemas

En este artículo se muestra cómo configurar las alertas de Azure Service Health para que envíen datos a través de webhooks a un sistema de notificación existente.

Puede configurar las alertas de Service Health para que reciba una notificación, mediante mensaje de texto o correo electrónico, cuando se produzca algún incidente que le afecte en algún servicio de Azure.

Pero también es posible que tenga implementado un sistema de notificación externo y prefiera utilizarlo. En este artículo se identifican las partes más importantes de la carga del webhook. También se describe cómo crear alertas personalizadas que envíen una notificación en cuanto se produzca algún problema significativo en un servicio.

Si desea usar una integración preconfigurada, consulte:
* [Configure alerts with ServiceNow](service-health-alert-webhook-servicenow.md) (Configuración de alertas con ServiceNow)
* [Configure alerts with PagerDuty](service-health-alert-webhook-pagerduty.md) (Configuración de alertas con PagerDuty)
* [Configure alerts with OpsGenie](service-health-alert-webhook-opsgenie.md) (Configuración de alertas con OpsGenie)

**Vea un vídeo introductorio:**

>[!VIDEO https://www.microsoft.com/en-us/videoplayer/embed/RE2OtUV]

## <a name="configure-a-custom-notification-by-using-the-service-health-webhook-payload"></a>Configuración de una notificación personalizada mediante la carga del webhook de Service Health
Para configurar su propia integración de webhook personalizada, es preciso que analice la carga JSON que se envía en las notificaciones de Service Health.

Consulte [un ejemplo](../azure-monitor/platform/activity-log-alerts-webhook.md) de la carga del webhook de `ServiceHealth`.

Para confirmar que se trata de una alerta de Service Health examine `context.eventSource == "ServiceHealth"`. Las siguientes propiedades son las más importantes:
- **data.context.activityLog.status**
- **data.context.activityLog.level**
- **data.context.activityLog.subscriptionId**
- **data.context.activityLog.properties.title**
- **data.context.activityLog.properties.impactStartTime**
- **data.context.activityLog.properties.communication**
- **data.context.activityLog.properties.impactedServices**
- **data.context.activityLog.properties.trackingId**

## <a name="create-a-link-to-the-service-health-dashboard-for-an-incident"></a>Creación de un vínculo al panel de Service Health en caso de algún incidente
Para crear un vínculo directo al panel de Service Health en cualquier escritorio o dispositivo móvil, genere una dirección URL especializada. Use *trackingId* y los tres primeros y últimos dígitos de *subscriptionId* con este formato:

https<i></i>://app.azure.com/h/ *&lt;trackingId&gt;* / *&lt;tres primeros y tres últimos dígitos de subscriptionId&gt;*

Por ejemplo, si *subscriptionId* es bba14129-e895-429b-8809-278e836ecdb3 y *trackingId* es 0DET-URB, la dirección URL del Service Health es:

https<i></i>://app.azure.com/h/0DET-URB/bbadb3

## <a name="use-the-level-to-detect-the-severity-of-the-issue"></a>Uso de level para detectar la gravedad del problema
De menor a mayor gravedad, la propiedad**level** de la carga puede tener los siguientes valores: *Informational* (Información), *Warning (Advertencia)* , *Error* o *Critical* (Crítico).

## <a name="parse-the-impacted-services-to-determine-the-incident-scope"></a>Análisis de los servicios afectados para determinar el ámbito del incidente
Las alertas de Service Health pueden informar acerca de los problemas que aparecen en varias regiones y servicios. Para obtener todos los detalles, es preciso analizar el valor de `impactedServices`.

El contenido interior es una cadena con [caracteres de escape JSON](https://json.org/) que, cuando no tiene caracteres de escape, contiene otro objeto JSON que puede analizarse regularmente. Por ejemplo:

```json
{"data.context.activityLog.properties.impactedServices": "[{\"ImpactedRegions\":[{\"RegionName\":\"Australia East\"},{\"RegionName\":\"Australia Southeast\"}],\"ServiceName\":\"Alerts & Metrics\"},{\"ImpactedRegions\":[{\"RegionName\":\"Australia Southeast\"}],\"ServiceName\":\"App Service\"}]"}
```

se convierte en:

```json
[
   {
      "ImpactedRegions":[
         {
            "RegionName":"Australia East"
         },
         {
            "RegionName":"Australia Southeast"
         }
      ],
      "ServiceName":"Alerts & Metrics"
   },
   {
      "ImpactedRegions":[
         {
            "RegionName":"Australia Southeast"
         }
      ],
      "ServiceName":"App Service"
   }
]
```

En este ejemplo se muestran los problemas de:
- "Alertas & métricas" en las regiones Este de Australia y Sudeste de Australia.
- "App Service" en la región Sudeste de Australia.

## <a name="test-your-webhook-integration-via-an-http-post-request"></a>Prueba de la integración de un webhook a través de una solicitud HTTP POST

Siga estos pasos:

1. Cree la carga de Service Health que desee enviar. Puede ver una carga del webhook de Service Health de ejemplo en [Webhooks para alertas del registro de actividad de Azure](../azure-monitor/platform/activity-log-alerts-webhook.md).

1. Cree una solicitud HTTP POST de la siguiente manera:

    ```
    POST        https://your.webhook.endpoint

    HEADERS     Content-Type: application/json

    BODY        <service health payload>
    ```
   Debería recibir la respuesta "2XX - Successful".

1. Vaya a [PagerDuty](https://www.pagerduty.com/) para confirmar que la integración se ha configurado correctamente.

## <a name="next-steps"></a>Pasos siguientes
- Revise el [Esquema de webhook de alertas del registro de actividad](../azure-monitor/platform/activity-log-alerts-webhook.md). 
- Más información acerca de las [Notificaciones del estado del servicio](./service-notifications.md).
- Más información sobre los [grupos de acciones](../azure-monitor/platform/action-groups.md).
