---
title: Cola de Storage como controlador de eventos para eventos de Azure Event Grid
description: Aquí se describe cómo puede usar las colas de Azure Storage como controladores de eventos para eventos de Event Grid.
services: event-grid
author: spelluru
ms.service: event-grid
ms.topic: conceptual
ms.date: 05/11/2020
ms.author: spelluru
ms.openlocfilehash: 9b767caa1041f865d8e15cd57796b186f7a4a6bb
ms.sourcegitcommit: bb0afd0df5563cc53f76a642fd8fc709e366568b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/19/2020
ms.locfileid: "83596274"
---
# <a name="storage-queue-as-an-event-handler-for-azure-event-grid-events"></a>Cola de Storage como controlador de eventos para eventos de Azure Event Grid
Un controlador de eventos es el lugar al que se envía el evento. El controlador realiza alguna acción adicional para procesar el evento. Varios servicios de Azure se configuran automáticamente para controlar eventos y **Azure Queue Storage** es uno de ellos. 

Use **Queue Storage** para recibir los eventos que deben extraerse. Puede usar Queue Storage cuando tenga un proceso de ejecución prolongada que tarde demasiado tiempo en responder. Al enviar eventos a Queue Storage, la aplicación puede extraer y procesar eventos siguiendo su propia programación.

## <a name="tutorials"></a>Tutoriales
Consulte el siguiente tutorial para obtener un ejemplo del uso de Queue Storage como un controlador de eventos. 

|Título  |Descripción  |
|---------|---------|
| [Guía de inicio rápido: Enrutamiento de eventos personalizados a Azure Queue Storage con la CLI de Azure y Event Grid](custom-event-to-queue-storage.md) | Describe cómo enviar eventos personalizados a una instancia de Queue Storage. |

## <a name="next-steps"></a>Pasos siguientes
Consulte el artículo [Controladores de eventos](event-handlers.md) para obtener una lista de controladores de eventos compatibles. 
