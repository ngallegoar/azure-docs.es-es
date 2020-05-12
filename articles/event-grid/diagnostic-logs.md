---
title: 'Azure Event Grid: Registros de diagnóstico de temas o dominios'
description: En este artículo se proporciona información conceptual sobre los registros de diagnóstico de un tema o un dominio de Azure Event Grid.
services: event-grid
author: spelluru
ms.service: event-grid
ms.topic: conceptual
ms.date: 04/29/2020
ms.author: spelluru
ms.openlocfilehash: 93e7e47cbcc1ab9542ba333b89f7dd655a412489
ms.sourcegitcommit: 1895459d1c8a592f03326fcb037007b86e2fd22f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/01/2020
ms.locfileid: "82629797"
---
#  <a name="diagnostic-logs-for-azure-event-grid-topicsdomains"></a>Registros de diagnóstico de temas o dominios de Azure Event Grid
La configuración de diagnóstico permite a los usuarios de Event Grid capturar y ver los registros de **errores de publicación y entrega** en una cuenta de almacenamiento, un centro de eventos o un área de trabajo de Log Analytics. En este artículo se proporciona un esquema para los registros y una entrada de registro de ejemplo.


## <a name="schema-for-publishdelivery-failure-logs"></a>Esquema para registros de errores de publicación y entrega

| Nombre de propiedad | Tipo de datos | Descripción |
| ------------- | --------- | ----------- | 
| Time | DateTime | Hora a la que se generó la entrada de registro <p>**Valor de ejemplo:**  01-29-2020 09:52:02.700</p> |
| EventSubscriptionName | String | El nombre de la suscripción de eventos <p>**Valor de ejemplo:** "EVENTSUB1"</p> <p>Esta propiedad solo existe para los registros de errores de entrega.</p>  |
| Category | String | Nombre de la categoría del registro. <p>**Valores de ejemplo:** "DeliveryFailures" o "PublishFailures" | 
| OperationName | String | Nombre de la operación realizada al encontrar el error.<p>**Valores de ejemplo:** "Deliver" para errores de entrega. |
| Message | String | Mensaje de registro para el usuario que explica el motivo del error y otros detalles adicionales. |
| ResourceId | String | El identificador de recurso para el recurso de tema/dominio<p>**Valores de ejemplo:** `/SUBSCRIPTIONS/SAMPLE-SUBSCRIPTION-ID/RESOURCEGROUPS/SAMPLE-RESOURCEGROUP/PROVIDERS/MICROSOFT.EVENTGRID/TOPICS/TOPIC1` |

## <a name="example"></a>Ejemplo

```json
{
    "time": "2019-11-01T00:17:13.4389048Z",
    "resourceId": "/SUBSCRIPTIONS/SAMPLE-SUBSCTIPTION-ID /RESOURCEGROUPS/SAMPLE-RESOURCEGROUP-NAME/PROVIDERS/MICROSOFT.EVENTGRID/TOPICS/SAMPLE-TOPIC-NAME ",
    "eventSubscriptionName": "SAMPLEDESTINATION",
    "category": "DeliveryFailures",
    "operationName": "Deliver",
    "message": "Message:outcome=NotFound, latencyInMs=2635, systemId=17284f7c-0044-46fb-84b7-59fda5776017, state=FilteredFailingDelivery, deliveryTime=11/1/2019 12:17:10 AM, deliveryCount=0, probationCount=0, deliverySchema=EventGridEvent, eventSubscriptionDeliverySchema=EventGridEvent, fields=InputEvent, EventSubscriptionId, DeliveryTime, State, Id, DeliverySchema, LastDeliveryAttemptTime, SystemId, fieldCount=, requestExpiration=1/1/0001 12:00:00 AM, delivered=False publishTime=11/1/2019 12:17:10 AM, eventTime=11/1/2019 12:17:09 AM, eventType=Type, deliveryTime=11/1/2019 12:17:10 AM, filteringState=FilteredWithRpc, inputSchema=EventGridEvent, publisher=DIAGNOSTICLOGSTEST-EASTUS.EASTUS-1.EVENTGRID.AZURE.NET, size=363, fields=Id, PublishTime, SerializedBody, EventType, Topic, Subject, FilteringHashCode, SystemId, Publisher, FilteringTopic, TopicCategory, DataVersion, MetadataVersion, InputSchema, EventTime, fieldCount=15, url=sb://diagnosticlogstesting-eastus.servicebus.windows.net/, deliveryResponse=NotFound: The messaging entity 'sb://diagnosticlogstesting-eastus.servicebus.windows.net/eh-diagnosticlogstest' could not be found. TrackingId:c98c5af6-11f0-400b-8f56-c605662fb849_G14, SystemTracker:diagnosticlogstesting-eastus.servicebus.windows.net:eh-diagnosticlogstest, Timestamp:2019-11-01T00:17:13, referenceId: ac141738a9a54451b12b4cc31a10dedc_G14:"
}
```

## <a name="next-steps"></a>Pasos siguientes
Para aprender a habilitar registros de diagnóstico de temas o dominios, consulte [Habilitación de registros de diagnóstico](enable-diagnostic-logs-topic.md).
