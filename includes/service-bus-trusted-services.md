---
title: archivo de inclusión
description: archivo de inclusión
services: service-bus-messaging
author: spelluru
ms.service: service-bus-messaging
ms.topic: include
ms.date: 11/05/2020
ms.author: spelluru
ms.custom: include file
ms.openlocfilehash: bb6f2281cc115904db6babcf31f1af387b254d48
ms.sourcegitcommit: 0dcafc8436a0fe3ba12cb82384d6b69c9a6b9536
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/10/2020
ms.locfileid: "94426575"
---
## <a name="trusted-microsoft-services"></a>Servicios de Microsoft de confianza
Al activar la opción **¿Quiere permitir que los servicios de confianza de Microsoft puedan omitir este firewall?** , los siguientes servicios adquieren acceso a los recursos de Service Bus.

| Servicio de confianza | Escenarios de uso admitidos | 
| --------------- | ------------------------- | 
| Azure Event Grid | Permite a Azure Event Grid enviar eventos a colas o temas en el espacio de nombres de Service Bus. También debe seguir estos pasos: <ul><li>Habilite la identidad asignada por el sistema para un tema o un dominio</li><li>Agregue la identidad al rol Remitente de datos de Azure Service Bus en el espacio de nombres de Service Bus</li><li>A continuación, configure la suscripción de eventos que emplea una cola o tema de Service Bus como punto de conexión para usar la identidad asignada por el sistema.</li></ul> <p>Para obtener más información, vea [Entrega de evento con una identidad administrada](../articles/event-grid/managed-service-identity.md)</p>|
