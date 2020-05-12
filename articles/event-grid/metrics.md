---
title: Métricas compatibles con Azure Event Grid
description: En este artículo se proporcionan las métricas de Azure Monitor admitidas por el servicio Azure Event Grid.
services: event-grid
author: spelluru
ms.service: event-grid
ms.topic: conceptual
ms.date: 04/29/2020
ms.author: spelluru
ms.openlocfilehash: 643df2f4cc6347e0fd56f9124b68f1888ab85e26
ms.sourcegitcommit: 1895459d1c8a592f03326fcb037007b86e2fd22f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/01/2020
ms.locfileid: "82629761"
---
# <a name="metrics-supported-by-azure-event-grid"></a>Métricas compatibles con Azure Event Grid
En este artículo se proporcionan listas de métricas de Event Grid clasificadas por espacios de nombres. 

## <a name="microsofteventgriddomains"></a>Microsoft.EventGrid/domains

|Métrica|Nombre de métrica para mostrar|Unidad|Tipo de agregación|Descripción|Dimensions|
|---|---|---|---|---|---|
|PublishSuccessCount|Eventos publicados|Count|Total|Número total de eventos publicados en este tema|Tema|
|PublishFailCount|Eventos no publicados|Count|Total|Número total de eventos que no se han podido publicar en este tema|Topic,ErrorType,Error|
|PublishSuccessLatencyInMs|Latencia de publicación correcta|Milisegundos|Total|Latencia de publicación correcta en milisegundos|None|
|MatchedEventCount|Eventos coincidentes|Count|Total|Número total de eventos que coinciden con esta suscripción de eventos|Topic,EventSubscriptionName,DomainEventSubscriptionName|
|DeliveryAttemptFailCount|Eventos de error en la entrega|Count|Total|Número total de eventos que no se han podido entregar a esta suscripción de eventos|Topic,EventSubscriptionName,DomainEventSubscriptionName,Error,ErrorType|
|DeliverySuccessCount|Eventos entregados|Count|Total|Número total de eventos que se han entregado a esta suscripción de eventos|Topic,EventSubscriptionName,DomainEventSubscriptionName|
|DestinationProcessingDurationInMs|Duración del procesamiento de destino|Milisegundos|Average|Duración del procesamiento de destino en milisegundos|Topic,EventSubscriptionName,DomainEventSubscriptionName|
|DroppedEventCount|Eventos quitados|Count|Total|Número total de eventos quitados que coinciden con esta suscripción de eventos|Topic,EventSubscriptionName,DomainEventSubscriptionName,DropReason|
|DeadLetteredCount|Eventos en la cola de mensajes fallidos|Count|Total|Número total de eventos en la cola de eventos fallidos que coinciden con esta suscripción de eventos|Topic,EventSubscriptionName,DomainEventSubscriptionName,DeadLetterReason|

## <a name="microsofteventgridtopics"></a>Microsoft.EventGrid/topics

|Métrica|Nombre de métrica para mostrar|Unidad|Tipo de agregación|Descripción|Dimensions|
|---|---|---|---|---|---|
|PublishSuccessCount|Eventos publicados|Count|Total|Número total de eventos publicados en este tema|None|
|PublishFailCount|Eventos no publicados|Count|Total|Número total de eventos que no se han podido publicar en este tema|ErrorType,Error|
|UnmatchedEventCount|Eventos no coincidentes|Count|Total|Número total de eventos que no coinciden con ninguna de las suscripciones de eventos de este tema|None|
|PublishSuccessLatencyInMs|Latencia de publicación correcta|Milisegundos|Total|Latencia de publicación correcta en milisegundos|None|
|MatchedEventCount|Eventos coincidentes|Count|Total|Número total de eventos que coinciden con esta suscripción de eventos|EventSubscriptionName|
|DeliveryAttemptFailCount|Eventos de error en la entrega|Count|Total|Número total de eventos que no se han podido entregar a esta suscripción de eventos|Error,ErrorType,EventSubscriptionName|
|DeliverySuccessCount|Eventos entregados|Count|Total|Número total de eventos que se han entregado a esta suscripción de eventos|EventSubscriptionName|
|DestinationProcessingDurationInMs|Duración del procesamiento de destino|Milisegundos|Average|Duración del procesamiento de destino en milisegundos|EventSubscriptionName|
|DroppedEventCount|Eventos quitados|Count|Total|Número total de eventos quitados que coinciden con esta suscripción de eventos|DropReason, EventSubscriptionName|
|DeadLetteredCount|Eventos en la cola de mensajes fallidos|Count|Total|Número total de eventos en la cola de eventos fallidos que coinciden con esta suscripción de eventos|DeadLetterReason, EventSubscriptionName|

## <a name="microsofteventgridsystemtopics"></a>Microsoft.EventGrid/systemTopics

|Métrica|Nombre de métrica para mostrar|Unidad|Tipo de agregación|Descripción|Dimensions|
|---|---|---|---|---|---|
|PublishSuccessCount|Eventos publicados|Count|Total|Número total de eventos publicados en este tema|None|
|PublishFailCount|Eventos no publicados|Count|Total|Número total de eventos que no se han podido publicar en este tema|ErrorType, Error|
|UnmatchedEventCount|Eventos no coincidentes|Count|Total|Número total de eventos que no coinciden con ninguna de las suscripciones de eventos de este tema|None|
|PublishSuccessLatencyInMs|Latencia de publicación correcta|Milisegundos|Total|Latencia de publicación correcta en milisegundos|None|
|MatchedEventCount|Eventos coincidentes|Count|Total|Número total de eventos que coinciden con esta suscripción de eventos|EventSubscriptionName|
|DeliveryAttemptFailCount|Eventos de error en la entrega|Count|Total|Número total de eventos que no se han podido entregar a esta suscripción de eventos|Error,ErrorType,EventSubscriptionName|
|DeliverySuccessCount|Eventos entregados|Count|Total|Número total de eventos que se han entregado a esta suscripción de eventos|EventSubscriptionName|
|DestinationProcessingDurationInMs|Duración del procesamiento de destino|Milisegundos|Average|Duración del procesamiento de destino en milisegundos|EventSubscriptionName|
|DroppedEventCount|Eventos quitados|Count|Total|Número total de eventos quitados que coinciden con esta suscripción de eventos|DropReason, EventSubscriptionName|
|DeadLetteredCount|Eventos en la cola de mensajes fallidos|Count|Total|Número total de eventos en la cola de eventos fallidos que coinciden con esta suscripción de eventos|DeadLetterReason, EventSubscriptionName|

## <a name="microsofteventgrideventsubscriptions"></a>Microsoft.EventGrid/eventSubscriptions

|Métrica|Nombre de métrica para mostrar|Unidad|Tipo de agregación|Descripción|Dimensions|
|---|---|---|---|---|---|
|MatchedEventCount|Eventos coincidentes|Count|Total|Número total de eventos que coinciden con esta suscripción de eventos|None|
|DeliveryAttemptFailCount|Eventos de error en la entrega|Count|Total|Número total de eventos que no se han podido entregar a esta suscripción de eventos|Error, ErrorType|
|DeliverySuccessCount|Eventos entregados|Count|Total|Número total de eventos que se han entregado a esta suscripción de eventos|None|
|DestinationProcessingDurationInMs|Duración del procesamiento de destino|Milisegundos|Average|Duración del procesamiento de destino en milisegundos|None|
|DroppedEventCount|Eventos quitados|Count|Total|Número total de eventos quitados que coinciden con esta suscripción de eventos|DropReason|
|DeadLetteredCount|Eventos en la cola de mensajes fallidos|Count|Total|Número total de eventos en la cola de eventos fallidos que coinciden con esta suscripción de eventos|DeadLetterReason|

## <a name="microsofteventgridextensiontopics"></a>Microsoft.EventGrid/extensionTopics

|Métrica|Nombre de métrica para mostrar|Unidad|Tipo de agregación|Descripción|Dimensions|
|---|---|---|---|---|---|
|PublishSuccessCount|Eventos publicados|Count|Total|Número total de eventos publicados en este tema|None|
|PublishFailCount|Eventos no publicados|Count|Total|Número total de eventos que no se han podido publicar en este tema|ErrorType, Error|
|UnmatchedEventCount|Eventos no coincidentes|Count|Total|Número total de eventos que no coinciden con ninguna de las suscripciones de eventos de este tema|None|
|PublishSuccessLatencyInMs|Latencia de publicación correcta|Milisegundos|Total|Latencia de publicación correcta en milisegundos|None|

## <a name="next-steps"></a>Pasos siguientes
Consulte el artículo siguiente: [Registros de diagnóstico](diagnostic-logs.md)
