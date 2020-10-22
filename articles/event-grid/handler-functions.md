---
title: Azure Functions como controlador de eventos para eventos de Azure Event Grid
description: Aquí se describe cómo puede usar Azure Functions como un controlador de eventos para eventos de Event Grid.
ms.topic: conceptual
ms.date: 09/18/2020
ms.openlocfilehash: 9e04fd3e04dab7a50940c2a4a799a56d447fbb6e
ms.sourcegitcommit: dbe434f45f9d0f9d298076bf8c08672ceca416c6
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/17/2020
ms.locfileid: "92145761"
---
# <a name="azure-function-as-an-event-handler-for-event-grid-events"></a>Azure Functions como controlador de eventos para eventos de Event Grid

Un controlador de eventos es el lugar al que se envía el evento. El controlador realiza una acción para procesar el evento. Varios servicios de Azure se configuran automáticamente para controlar los eventos y **Azure Functions** es uno de ellos. 


Para usar una función de Azure como controlador de eventos, siga uno de estos métodos: 

-   Use el [desencadenador de Event Grid](../azure-functions/functions-bindings-event-grid-trigger.md).  Seleccione **Función de Azure** como **tipo de punto de conexión**. A continuación, especifique la aplicación de funciones de Azure y la función que controlará los eventos. 
-   Use el [desencadenador de HTTP](../azure-functions/functions-bindings-http-webhook.md).  Seleccione **Webhook** como **tipo de punto de conexión**. A continuación, especifique la URL de la función de Azure que controlará los eventos. 

Se recomienda usar el primer enfoque (desencadenador de Event Grid), ya que presenta las siguientes ventajas respecto al segundo enfoque:
-   Event Grid valida automáticamente los desencadenadores de Event Grid. Con los desencadenadores HTTP genéricos, debe implementar la [respuesta de validación](webhook-event-delivery.md) usted mismo.
-   Event Grid ajusta automáticamente la velocidad a la que se entregan los eventos a una función desencadenada por un evento de Event Grid basándose en la tasa percibida a la que la función puede procesar eventos. Esta característica de coincidencia de velocidad evita los errores de entrega que provienen de la incapacidad de una función para procesar eventos, ya que la tasa de procesamiento de eventos de la función puede variar con el tiempo. Para mejorar la eficacia con un alto rendimiento, habilite el procesamiento por lotes en la suscripción de eventos. Para obtener más información, consulte [Habilitar el procesamiento por lotes](#enable-batching).

    > [!NOTE]
    > Actualmente, no puede usar un desencadenador de Event Grid para una aplicación de Azure Functions cuando el evento se entrega en el esquema de **CloudEvents**. En cambio, debe usar un desencadenador HTTP.

## <a name="tutorials"></a>Tutoriales

|Título  |Descripción  |
|---------|---------|
| [Inicio rápido: Control de eventos con una función](custom-event-to-function.md) | Envía un evento personalizado a una función para su procesamiento. |
| [Tutorial: Automatización del cambio de tamaño de las imágenes cargadas mediante Event Grid](resize-images-on-storage-blob-upload-event.md) | Los usuarios cargan imágenes a través de la aplicación web en la cuenta de almacenamiento. Cuando se crea un blob de almacenamiento, Event Grid envía un evento a la aplicación de función, que cambia el tamaño de la imagen cargada. |
| [Tutorial: transmisión de macrodatos a un almacén de datos](event-grid-event-hubs-integration.md) | Cuando Event Hubs crea un archivo de captura, Event Grid envía un evento a una aplicación de función. La aplicación recupera el archivo de captura y migra los datos a un almacenamiento de datos. |
| [Tutorial: Ejemplos de integración de Azure Service Bus en Azure Event Grid](../service-bus-messaging/service-bus-to-event-grid-integration-example.md?toc=%2fazure%2fevent-grid%2ftoc.json) | Event Grid envía mensajes de temas de Service Bus a la aplicación de funciones y a la aplicación lógica. |

## <a name="rest-example-for-put"></a>Ejemplo de REST (para PUT)

```json
{
    "properties": 
    {
        "destination": 
        {
            "endpointType": "AzureFunction",
            "properties": 
            {
                "resourceId": "/subscriptions/<AZURE SUBSCRIPTION ID>/resourceGroups/<RESOURCE GROUP NAME>/providers/Microsoft.Web/sites/<FUNCTION APP NAME>/functions/<FUNCTION NAME>",
                "maxEventsPerBatch": 10,
                "preferredBatchSizeInKilobytes": 6400
            }
        },
        "eventDeliverySchema": "EventGridSchema"
    }
}
```

## <a name="enable-batching"></a>Habilitar el procesamiento por lotes
Para obtener un rendimiento superior, habilite el procesamiento por lotes en la suscripción. Si usa Azure Portal, puede establecer el número máximo de eventos por lote y el tamaño de lote preferido en kilobytes en el momento de crear una suscripción o después de crearla. 

Puede configurar los valores del lote mediante Azure Portal, PowerShell, CLI o la plantilla de Resource Manager. 

### <a name="azure-portal"></a>Azure portal
En el momento de crear una suscripción en la UI, en la página **Crear suscripción de eventos**, cambie a la pestaña **Características avanzadas** y establezca los valores de **Número máximo de eventos por lote** y **Tamaño de lote preferido en kilobytes**. 
    
:::image type="content" source="./media/custom-event-to-function/enable-batching.png" alt-text="Habilitación del procesamiento por lotes en el momento de crear una suscripción":::

Puede actualizar estos valores para una suscripción existente en la pestaña **Características** de la página **Tema de Event Grid**. 

:::image type="content" source="./media/custom-event-to-function/features-batch-settings.png" alt-text="Habilitación del procesamiento por lotes en el momento de crear una suscripción":::

### <a name="azure-resource-manager-template"></a>Plantilla del Administrador de recursos de Azure
Puede establecer **maxEventsPerBatch** y **preferredBatchSizeInKilobytes** en una plantilla de Azure Resource Manager. Para obtener más información, consulte el artículo sobre la [referencia de las plantillas de Microsoft.EventGrid eventSubscriptions](/azure/templates/microsoft.eventgrid/eventsubscriptions).

### <a name="azure-cli"></a>Azure CLI
Puede usar los comandos [az eventgrid event-subscription create](/cli/azure/eventgrid/event-subscription?view=azure-cli-latest#az_eventgrid_event_subscription_create&preserve-view=true) o [az eventgrid event-subscription update](/cli/azure/eventgrid/event-subscription?view=azure-cli-latest#az_eventgrid_event_subscription_update&preserve-view=true) para configurar las opciones relacionadas con el lote con los parámetros `--max-events-per-batch` o `--preferred-batch-size-in-kilobytes`.

### <a name="azure-powershell"></a>Azure PowerShell
Puede usar los cmdlets [New-AzEventGridSubscription](/powershell/module/az.eventgrid/new-azeventgridsubscription) o [Update-AzEventGridSubscription](/powershell/module/az.eventgrid/update-azeventgridsubscription) para configurar las opciones relacionadas con el lote mediante los parámetros `-MaxEventsPerBatch` o `-PreferredBatchSizeInKiloBytes`.

## <a name="next-steps"></a>Pasos siguientes
Consulte el artículo [Controladores de eventos](event-handlers.md) para obtener una lista de controladores de eventos compatibles.