---
title: Colas y temas de Service Bus como controladores de eventos para eventos de Azure Event Grid
description: Aquí se describe cómo puede usar las colas y temas de Service Bus como controladores de eventos para eventos de Azure Event Grid.
ms.topic: conceptual
ms.date: 09/03/2020
ms.openlocfilehash: ab219f0dc6009dc01d5915995fc04094e72a88cf
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "91629512"
---
# <a name="service-bus-queues-and-topics-as-event-handlers-for-azure-event-grid-events"></a>Colas y temas de Service Bus como controladores de eventos para eventos de Azure Event Grid
Un controlador de eventos es el lugar al que se envía el evento. El controlador realiza alguna acción adicional para procesar el evento. Varios servicios de Azure se configuran automáticamente para controlar eventos y **Azure Service Bus** es uno de ellos. 

Puede usar una cola o un tema de Service como un controlador para los eventos de Event Grid. 

## <a name="service-bus-queues"></a>Colas de Service Bus
Puede redirigir los eventos de Event Grid directamente a las colas de Service Bus para usarlos en escenarios de almacenamiento en búfer o comando y control en aplicaciones empresariales.

En Azure Portal, al crear una suscripción de eventos, seleccione **Cola de Service Bus** como tipo de punto de conexión y, a continuación, haga clic en **Seleccionar un punto de conexión** para elegir una cola de Service Bus.

### <a name="using-cli-to-add-a-service-bus-queue-handler"></a>Uso de la CLI para agregar un controlador de colas de Service Bus

Para la CLI de Azure, en el siguiente ejemplo, se suscribe un tema y conecta un tema de Event Grid a una cola de Service Bus:

```azurecli-interactive
az eventgrid event-subscription create \
    --name <my-event-subscription> \
    --source-resource-id /subscriptions/{SubID}/resourceGroups/{RG}/providers/Microsoft.EventGrid/topics/topic1 \
    --endpoint-type servicebusqueue \
    --endpoint /subscriptions/{SubID}/resourceGroups/TestRG/providers/Microsoft.ServiceBus/namespaces/ns1/queues/queue1
```

## <a name="service-bus-topics"></a>Temas de Service Bus

Puede enrutar los eventos de Event Grid directamente a los temas de Service Bus para controlar los eventos del sistema de Azure con temas de Service Bus o para escenarios de mensajería de comando y control.

En Azure Portal, al crear una suscripción de eventos, seleccione **Tema de Service Bus** como tipo de punto de conexión y, a continuación, haga clic en **Seleccionar un punto de conexión** para elegir un tema de Service Bus.

### <a name="using-cli-to-add-a-service-bus-topic-handler"></a>Uso de la CLI para agregar un controlador de temas de Service Bus

Para la CLI de Azure, en el siguiente ejemplo, se suscribe un tema y conecta un tema de Event Grid a una cola de Service Bus:

```azurecli-interactive
az eventgrid event-subscription create \
    --name <my-event-subscription> \
    --source-resource-id /subscriptions/{SubID}/resourceGroups/{RG}/providers/Microsoft.EventGrid/topics/topic1 \
    --endpoint-type servicebustopic \
    --endpoint /subscriptions/{SubID}/resourceGroups/TestRG/providers/Microsoft.ServiceBus/namespaces/ns1/topics/topic1
```

## <a name="message-properties"></a>Propiedades del mensaje
Si usa una **cola o un tema de Service Bus** como controlador de eventos de Event Grid, estas son las propiedades que recibirá en los encabezados del mensaje: 

| Nombre de propiedad | Descripción |
| ------------- | ----------- | 
| aeg-subscription-name | Nombre de la suscripción de eventos. |
| aeg-delivery-count | <p>Número de intentos realizados para el evento.</p> <p>Ejemplo: "1"</p> |
| aeg-event-type | <p>Tipo de evento.</p><p> Ejemplo: "Microsoft.Storage.blobCreated"</p> | 
| aeg-metadata-version | <p>Versión de metadatos del evento.</p> <p>Ejemplo: "1".</p><p> Esta propiedad representa la versión de los metadatos en el **esquema de eventos de Event Grid** y la **versión de la especificación** en el **esquema de eventos en la nube**. </p>|
| aeg-data-version | <p>Versión de datos del evento.</p><p>Ejemplo: "1".</p><p>Esta propiedad representa la versión de los datos en el **esquema de eventos de Event Grid** y la no se aplica en el **esquema de eventos en la nube**.</p> |

## <a name="message-headers"></a>Encabezados de mensaje
Cuando se envía un evento a una cola o tema de Service Bus como un mensaje asincrónico, el valor de `messageid` del mensaje asincrónico es un id. de sistema interno.

El id. de sistema interno del mensaje se mantendrá cuando se vuelva a entregar el evento, para que pueda evitar entregas duplicadas; para ello, active la **detección de duplicados** en la entidad de Service Bus. Le recomendamos que habilite la duración de la detección de duplicados en la entidad Service Bus para que sea el período de vida (TTL) del evento o la duración máxima de reintentos, lo que sea más largo.

## <a name="rest-examples-for-put"></a>Ejemplos de REST (para PUT)

### <a name="service-bus-queue"></a>Cola de Service Bus

```json
{
    "properties": 
    {
        "destination": 
        {
            "endpointType": "ServiceBusQueue",
            "properties": 
            {
                "resourceId": "/subscriptions/<AZURE SUBSCRIPTION ID>/resourceGroups/<RESOURCE GROUP NAME>/providers/Microsoft.ServiceBus/namespaces/<SERVICE BUS NAMESPACE NAME>/queues/<SERVICE BUS QUEUE NAME>"
            }
        },
        "eventDeliverySchema": "EventGridSchema"
    }
}
```

### <a name="service-bus-queue---delivery-with-managed-identity"></a>Cola de Service Bus: entrega con identidad administrada

```json
{
    "properties": {
        "deliveryWithResourceIdentity": 
        {
            "identity": 
            {
                "type": "SystemAssigned"
            },
            "destination": 
            {
                "endpointType": "ServiceBusQueue",
                "properties": 
                {
                    "resourceId": "/subscriptions/<AZURE SUBSCRIPTION ID>/resourceGroups/<RESOURCE GROUP NAME>/providers/Microsoft.ServiceBus/namespaces/<SERVICE BUS NAMESPACE NAME>/queues/<SERVICE BUS QUEUE NAME>"
                }
            }
        },
        "eventDeliverySchema": "EventGridSchema"
    }
}
```

### <a name="service-bus-topic"></a>Tema de Service Bus

```json
{
    "properties": 
    {
        "destination": 
        {
            "endpointType": "ServiceBusTopic",
            "properties": 
            {
                "resourceId": "/subscriptions/<AZURE SUBSCRIPTION ID>/resourceGroups/<RESOURCE GROUP NAME>/providers/Microsoft.ServiceBus/namespaces/<SERVICE BUS NAMESPACE NAME>/topics/<SERVICE BUS TOPIC NAME>"
            }
        },
        "eventDeliverySchema": "EventGridSchema"
    }
}
```

### <a name="service-bus-topic---delivery-with-managed-identity"></a>Tema de Service Bus: entrega con identidad administrada

```json
{
    "properties": 
    {
        "deliveryWithResourceIdentity": 
        {
            "identity": 
            {
                "type": "SystemAssigned"
            },
            "destination": 
            {
                "endpointType": "ServiceBusTopic",
                "properties": 
                {
                    "resourceId": "/subscriptions/<AZURE SUBSCRIPTION ID>/resourceGroups/<RESOURCE GROUP NAME>/providers/Microsoft.ServiceBus/namespaces/<SERVICE BUS NAMESPACE NAME>/topics/<SERVICE BUS TOPIC NAME>"
                }
            }
        },
        "eventDeliverySchema": "EventGridSchema"
    }
}
```

## <a name="next-steps"></a>Pasos siguientes
Consulte el artículo [Controladores de eventos](event-handlers.md) para obtener una lista de controladores de eventos compatibles. 
