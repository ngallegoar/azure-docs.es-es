---
author: spelluru
ms.service: service-bus-messaging
ms.topic: include
ms.date: 11/25/2018
ms.author: spelluru
ms.openlocfilehash: d3d33c87dc1adf65a53b71cc4c833e7f4a191670
ms.sourcegitcommit: eb6bef1274b9e6390c7a77ff69bf6a3b94e827fc
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/05/2020
ms.locfileid: "67186555"
---
## <a name="what-are-service-bus-queues"></a>¿Qué son las colas de Service Bus?
Las colas de Service Bus son compatibles con el modelo de comunicación de **mensajería asíncrona** . Cuando se usan colas, los componentes de una aplicación distribuida no se comunican directamente entre sí, sino que intercambian mensajes a través de una cola, que actúa como un intermediario (agente). El productor del mensaje (remitente) manda un mensaje a la cola y, a continuación sigue con su procesamiento. De forma asíncrona, el destinatario del mensaje (receptor) extrae el mensaje de la cola y lo procesa. El productor no tiene que esperar una respuesta del destinatario para continuar el proceso y el envío de más mensajes. Las colas ofrecen una entrega de mensajes según el modelo **El primero en entrar es el primero en salir (FIFO)** a uno o más destinatarios de la competencia. Es decir, normalmente los receptores reciben y procesan los mensajes en el orden en el que se agregaron a la cola y solo un destinatario del mensaje recibe y procesa cada uno de los mensajes.

![QueueConcepts](./media/howto-service-bus-queues/sb-queues-08.png)

Las colas de Service Bus son una tecnología de uso general que puede utilizarse en una variedad de escenarios:

* Comunicación entre los roles de trabajo y web en una aplicación de Azure de niveles múltiples.
* Comunicación entre aplicaciones locales y aplicaciones hospedadas de Azure en una solución híbrida.
* Comunicación entre componentes de una aplicación distribuida que se ejecuta en local en distintas organizaciones o departamentos de una organización.

El uso de las colas le permite escalar sus aplicaciones más fácilmente y dotar de más resistencia a su arquitectura.


