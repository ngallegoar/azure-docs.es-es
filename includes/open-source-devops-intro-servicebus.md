---
author: tomarchermsft
ms.service: ansible
ms.topic: include
ms.date: 04/22/2019
ms.author: tarcher
ms.openlocfilehash: eb96027351cf244e9cd4404f702544411130db5e
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/24/2020
ms.locfileid: "67187005"
---
[Azure Service Bus](/azure/service-bus-messaging/service-bus-messaging-overview) es un agente de mensajes de [integración](https://azure.microsoft.com/product-categories/integration/) empresarial. Service Bus admite dos tipos de comunicación: colas y temas. 

Las colas admiten comunicaciones asincrónicas entre aplicaciones. Una aplicación envía mensajes a una cola, que almacena dichos mensajes. Luego, la aplicación receptora se conecta y lee los mensajes de la cola.

Los temas admiten el patrón publicación-suscripción, que permite una relación de uno a varios entre el origen del mensaje y sus destinatarios.