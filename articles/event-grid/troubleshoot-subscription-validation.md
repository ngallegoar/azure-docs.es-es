---
title: 'Azure Event Grid: solución de problemas de validación de suscripciones'
description: En este artículo se muestra cómo se pueden solucionar los problemas de las validaciones de suscripciones.
services: event-grid
author: spelluru
ms.service: event-grid
ms.topic: conceptual
ms.date: 04/30/2020
ms.author: spelluru
ms.openlocfilehash: a052d4c268fadc60f754630156fe0bc0d33acf3b
ms.sourcegitcommit: 1895459d1c8a592f03326fcb037007b86e2fd22f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/01/2020
ms.locfileid: "82629777"
---
# <a name="troubleshoot-azure-event-grid-errors"></a>Solución de problemas de Azure Event Grid
En esta guía de solución de problemas se proporciona información sobre la solución de problemas de validaciones de las suscripciones a eventos. 


## <a name="troubleshoot-event-subscription-validation"></a>Solución de problemas de validación de suscripciones a eventos

Durante la creación de suscripciones a eventos, si ve un mensaje de error como `The attempt to validate the provided endpoint https://your-endpoint-here failed. For more details, visit https://aka.ms/esvalidation`, indica que hay un error en el protocolo de enlace de la validación. Para resolver este error, compruebe los siguientes aspectos:

- Realice una solicitud HTTP POST a la dirección URL de webhook con un cuerpo de la solicitud [sample SubscriptionValidationEvent](webhook-event-delivery.md#validation-details) mediante Postman, Curl o una herramienta similar.
- Si el webhook está implementando un mecanismo de enlace de validación sincrónica, compruebe que ValidationCode se devuelve como parte de la respuesta.
- Si el webhook está implementando un mecanismo de enlace de validación asincrónica, compruebe que la solicitud HTTP POST devuelve 200 OK.
- Si el webhook devuelve 403 (Prohibido) en la respuesta, compruebe si el webhook está detrás de una puerta de enlace de aplicaciones de Azure o del firewall de aplicaciones web. Si es así, tendrá que deshabilitar estas reglas de firewall y volver a realizar una solicitud HTTP POST:

  920300 [Falta un encabezado de aceptación (Accept) en la solicitud; se puede corregir]

  942430 [Restringe la detección de anomalías de caracteres de SQL (args): número de caracteres especiales que se han excedido (12)]

  920230 (Varias codificaciones de direcciones URL detectadas)

  942130 (Ataque por inyección de código SQL: Tautología de SQL detectada).

  931130 [Posible ataque remoto de inclusión de archivos (RFI) = Referencia o vínculo fuera del dominio]

> [!IMPORTANT]
> Para más información sobre la validación de los puntos de conexión para webhooks, consulte [Entrega de eventos de webhook](webhook-event-delivery.md).

## <a name="validate-event-grid-event-subscription-using-postman"></a>Validación de suscripciones a eventos de Event Grid mediante Postman
Este es un ejemplo del uso de Postman para validar una suscripción de webhook de un evento de Event Grid: 

![Validación de suscripciones a eventos de Event Grid mediante Postman](./media/troubleshoot-subscription-validation/event-subscription-validation-postman.png)

Este es un ejemplo del código JSON de SubscriptionValidationEvent:

```json
[
  {
    "id": "2d1781af-3a4c-4d7c-bd0c-e34b19da4e66",
    "topic": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
    "subject": "",
    "data": {
      "validationCode": "512d38b6-c7b8-40c8-89fe-f46f9e9622b6",
    },
    "eventType": "Microsoft.EventGrid.SubscriptionValidationEvent",
    "eventTime": "2018-01-25T22:12:19.4556811Z",
    "metadataVersion": "1",
    "dataVersion": "1"
  }
]
```

Este es el ejemplo de respuesta correcta:

```json
{
  "validationResponse": "512d38b6-c7b8-40c8-89fe-f46f9e9622b6"
}
```

Para más información sobre la validación de eventos de Event Grid para webhooks, consulte [Validación de puntos de conexión con eventos de Event Grid](webhook-event-delivery.md#endpoint-validation-with-event-grid-events).

## <a name="validate-cloud-event-subscription-using-postman"></a>Validación de suscripciones a eventos de nube mediante Postman
Este es un ejemplo de uso de Postman para validar una suscripción de webhook de un evento de nube: 

![Validación de suscripciones a eventos de nube mediante Postman](./media/troubleshoot-subscription-validation/cloud-event-subscription-validation-postman.png)

Use el método **HTTP OPTIONS** para la validación con eventos de nube. Para más información sobre la validación de eventos de nube para webhooks, consulte [Validación de puntos de conexión con eventos de nube](webhook-event-delivery.md#endpoint-validation-with-event-grid-events).

## <a name="event-grid-event-subscription-validation-using-curl"></a>Validación de suscripciones a eventos de Event Grid mediante Curl 
Este es el comando de Curl de ejemplo para validar una suscripción de webhook de un evento de Event Grid: 

```bash
curl -X POST -d '[{"id": "2d1781af-3a4c-4d7c-bd0c-e34b19da4e66","topic": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx","subject": "","data": {"validationCode": "512d38b6-c7b8-40c8-89fe-f46f9e9622b6"},"eventType": "Microsoft.EventGrid.SubscriptionValidationEvent","eventTime": "2018-01-25T22:12:19.4556811Z", "metadataVersion": "1","dataVersion": "1"}]' -H 'Content-Type: application/json' https://{your-webhook-url.com}
```

## <a name="next-steps"></a>Pasos siguientes
Si necesita más ayuda, publique su problema en el [foro de Stack Overflow](https://stackoverflow.com/questions/tagged/azure-eventgrid) o abra una [incidencia de soporte técnico](https://azure.microsoft.com/support/options/). 
