---
title: Event Hub como controlador de eventos para eventos de Azure Event Grid
description: Aquí se describe cómo puede usar los centros de eventos como controladores de eventos para eventos de Event Grid.
ms.topic: conceptual
ms.date: 07/07/2020
ms.openlocfilehash: 4fb8027290831dbfed4b6d202d61d2d689274828
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "91629631"
---
# <a name="event-hub-as-an-event-handler-for-azure-event-grid-events"></a>Centro de eventos como controlador de eventos para eventos de Azure Event Grid
Un controlador de eventos es el lugar al que se envía el evento. El controlador realiza una acción para procesar el evento. Varios servicios de Azure se configuran automáticamente para controlar los eventos y **Azure Event Hubs** es uno de ellos. 

Use **Event Hubs** cuando la solución obtenga eventos de Event Grid más rápido de lo que puede procesarlos. Una vez que los eventos se encuentren en un centro de eventos, la aplicación puede procesar los eventos del centro de eventos en su propia programación. Puede escalar el procesamiento de eventos para controlar los eventos entrantes.

## <a name="tutorials"></a>Tutoriales
Consulte los siguientes ejemplos: 

|Título  |Descripción  |
|---------|---------|
| [Inicio rápido: enrutamiento de eventos personalizados a Azure Event Hubs con la CLI de Azure](custom-event-to-eventhub.md) | Envía un evento personalizado a un centro de eventos para que lo procese una aplicación. |
| [Plantilla de Resource Manager: creación de un tema personalizado Event Grid y envío de eventos a un centro de eventos](https://github.com/Azure/azure-quickstart-templates/tree/master/101-event-grid-event-hubs-handler)| Plantilla de Resource Manager que crea una suscripción a un tema personalizado. Envía eventos a una instancia de Azure Event Hubs. |

## <a name="message-properties"></a>Propiedades del mensaje
Si usa un **centro de eventos** como un controlador de eventos para eventos de Event Grid, estas son las propiedades que recibirá en los encabezados del mensaje: 

| Nombre de propiedad | Descripción |
| ------------- | ----------- | 
| aeg-subscription-name | Nombre de la suscripción de eventos. |
| aeg-delivery-count | <p>Número de intentos realizados para el evento.</p> <p>Ejemplo: "1"</p> |
| aeg-event-type | <p>Tipo de evento.</p><p> Ejemplo: "Microsoft.Storage.blobCreated"</p> | 
| aeg-metadata-version | <p>Versión de metadatos del evento.</p> <p>Ejemplo: "1".</p><p> Esta propiedad representa la versión de los metadatos en el **esquema de eventos de Event Grid** y la **versión de la especificación** en el **esquema de eventos en la nube**. </p>|
| aeg-data-version | <p>Versión de datos del evento.</p><p>Ejemplo: "1".</p><p>Esta propiedad representa la versión de los datos en el **esquema de eventos de Event Grid** y la no se aplica en el **esquema de eventos en la nube**.</p> |
| aeg-output-event-id | Id. del evento de Event Grid. |

## <a name="rest-examples-for-put"></a>Ejemplos de REST (para PUT)


### <a name="event-hub"></a>Centro de eventos

```json
{
    "properties": 
    {
        "destination": 
        {
            "endpointType": "EventHub",
            "properties": 
            {
                "resourceId": "/subscriptions/<AZURE SUBSCRIPTION ID>/resourceGroups/<RESOURCE GROUP NAME>/providers/Microsoft.EventHub/namespaces/<EVENT HUBS NAMESPACE NAME>/eventhubs/<EVENT HUB NAME>"
            }
        },
        "eventDeliverySchema": "EventGridSchema"
    }
}
```

### <a name="event-hub---delivery-with-managed-identity"></a>Centro de eventos: con identidad administrada

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
                "endpointType": "EventHub",
                "properties": 
                {
                    "resourceId": "/subscriptions/<AZURE SUBSCRIPTION ID>/resourceGroups/<RESOURCE GROUP NAME>/providers/Microsoft.EventHub/namespaces/<EVENT HUBS NAMESPACE NAME>/eventhubs/<EVENT HUB NAME>"
                }
            }
        },
        "eventDeliverySchema": "EventGridSchema"
    }
}
```

## <a name="next-steps"></a>Pasos siguientes
Consulte el artículo [Controladores de eventos](event-handlers.md) para obtener una lista de controladores de eventos compatibles. 
