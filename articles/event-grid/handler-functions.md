---
title: Azure Functions como controlador de eventos para eventos de Azure Event Grid
description: Aquí se describe cómo puede usar Azure Functions como un controlador de eventos para eventos de Event Grid.
services: event-grid
author: spelluru
ms.service: event-grid
ms.topic: conceptual
ms.date: 05/11/2020
ms.author: spelluru
ms.openlocfilehash: 3ff3c0013cb7a373461b997b9922612763461b8d
ms.sourcegitcommit: bb0afd0df5563cc53f76a642fd8fc709e366568b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/19/2020
ms.locfileid: "83595654"
---
# <a name="azure-function-as-an-event-handler-for-event-grid-events"></a>Azure Functions como controlador de eventos para eventos de Event Grid

Un controlador de eventos es el lugar al que se envía el evento. El controlador realiza una acción para procesar el evento. Varios servicios de Azure se configuran automáticamente para controlar los eventos y **Azure Functions** es uno de ellos. 

Use **Azure Functions** en una arquitectura sin servidor para responder a eventos de Event Grid. Cuando use Azure Functions como controlador, utilice el desencadenador de Event Grid en lugar del desencadenador HTTP genérico. Event Grid valida automáticamente los desencadenadores de Event Grid. Con los desencadenadores HTTP genéricos, debe implementar la [respuesta de validación](webhook-event-delivery.md) usted mismo.

Para obtener más información, consulte [Desencadenador de Event Grid para Azure Functions](../azure-functions/functions-bindings-event-grid.md) para obtener información general sobre el uso del desencadenador Event Grid en la instancia de Functions.

## <a name="tutorials"></a>Tutoriales

|Título  |Descripción  |
|---------|---------|
| [Inicio rápido: Control de eventos con una función](custom-event-to-function.md) | Envía un evento personalizado a una función para su procesamiento. |
| [Tutorial: Automatización del cambio de tamaño de las imágenes cargadas mediante Event Grid](resize-images-on-storage-blob-upload-event.md) | Los usuarios cargan imágenes a través de la aplicación web en la cuenta de almacenamiento. Cuando se crea un blob de almacenamiento, Event Grid envía un evento a la aplicación de función, que cambia el tamaño de la imagen cargada. |
| [Tutorial: transmisión de macrodatos a un almacén de datos](event-grid-event-hubs-integration.md) | Cuando Event Hubs crea un archivo de captura, Event Grid envía un evento a una aplicación de función. La aplicación recupera el archivo de captura y migra los datos a un almacenamiento de datos. |
| [Tutorial: Ejemplos de integración de Azure Service Bus en Azure Event Grid](../service-bus-messaging/service-bus-to-event-grid-integration-example.md?toc=%2fazure%2fevent-grid%2ftoc.json) | Event Grid envía mensajes de temas de Service Bus a la aplicación de funciones y a la aplicación lógica. |


## <a name="next-steps"></a>Pasos siguientes
Consulte el artículo [Controladores de eventos](event-handlers.md) para obtener una lista de controladores de eventos compatibles. 
