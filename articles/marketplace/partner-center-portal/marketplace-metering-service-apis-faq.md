---
title: Preguntas más frecuentes sobre las API del servicio de medición de Marketplace | Azure Marketplace
description: Emita el uso de una oferta de SaaS en Azure Marketplace.
author: dsindona
ms.author: dsindona
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 07/11/2019
ms.openlocfilehash: 6e5b691a41ef283449f9eeeb90e9d01a91616146
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "80275788"
---
# <a name="marketplace-metering-service-apis---faq"></a>Preguntas más frecuentes sobre las API del servicio de medición de Marketplace

Cuando un usuario de Azure se suscriba a un servicio de SaaS con facturación de uso medido, usted realizará un seguimiento del consumo para cada dimensión de facturación que use el cliente. Si el consumo supera las cantidades establecidas para el período seleccionado por el cliente, el servicio emitirá eventos de uso a Microsoft.

## <a name="emit-usage-events"></a>Emisión de eventos de uso

>[!Note]
>Esta sección solo se aplica a las ofertas de SaaS en las que al menos uno de los planes tiene dimensiones de servicio de medición definidas en el momento de publicar la oferta.

![Emisión de eventos de uso](media/isv-emits-usage-event.png)

Consulte la [API de eventos de uso por lotes de SaaS](./marketplace-metering-service-apis.md#batch-usage-event) para obtener información sobre el contrato de API para emitir eventos de uso.

### <a name="how-often-is-it-expected-to-emit-usage"></a>¿Con qué frecuencia se espera que emita el uso?

Idealmente, se espera que emita el uso cada hora durante la última hora (siempre y cuando haya habido uso en la hora anterior).

### <a name="what-is-the-maximum-delay-between-the-time-an-event-occurs-and-the-time-a-usage-event-is-emitted-to-microsoft"></a>¿Cuál es el retraso máximo entre el momento en que se produce un evento y la hora a la que se emite un evento de uso a Microsoft?

Lo ideal es que el evento de uso se emita cada hora, en el caso de los eventos que se produjeron en la última hora. Aun así, se prevé que se produzcan retrasos. El retraso máximo permitido es de 24 horas; transcurrido este tiempo, no se aceptarán eventos de uso.

Por ejemplo, si se produce un evento de uso a las 13:00, tendrá hasta las 13:00 del día siguiente para emitir un evento de uso asociado a este evento. Esto significa que, en el caso de que el sistema que emite el uso tenga un período de inactividad, se puede recuperar y enviar el evento de uso para el intervalo de una hora en el que se produjo sin que se pierda fidelidad.

### <a name="what-happens-when-you-send-more-than-one-usage-event-on-the-same-hour"></a>¿Qué ocurre si se envía más de un evento de uso en la misma hora?

Solo se acepta un evento de uso durante el intervalo de una hora. El intervalo de una hora empieza en el minuto 0 y termina en el minuto 59.  Si se emite más de un evento de uso durante el mismo intervalo de una hora, los eventos de uso posteriores se eliminarán como duplicados.

### <a name="what-happens-when-you-emit-usage-for-a-saas-subscription-that-has-been-unsubscribed-already"></a>¿Qué ocurre cuando se emite el uso para una suscripción de SaaS que se ha cancelado?

No se aceptará ningún evento de uso emitido a la plataforma de Marketplace después de que se haya eliminado una suscripción de SaaS.

### <a name="can-you-get-a-list-of-all-saas-subscriptions-including-active-and-unsubscribed-subscriptions"></a>¿Se puede obtener una lista de todas las suscripciones de SaaS, incluidas las activas y las canceladas?

Sí, cuando se llama a la API `GET /saas/subscriptions`, se incluye una lista de todas las suscripciones de SaaS. El campo de estado de la respuesta de cada suscripción de SaaS captura si la suscripción está activa o se ha cancelado. La llamada a las lista de suscripciones devuelve un máximo de 100 suscripciones en este momento.

## <a name="next-steps"></a>Pasos siguientes

- Consulte [Marketplace metering service APIs](./marketplace-metering-service-apis.md) (API del servicio de medición de Marketplace) para más información.
