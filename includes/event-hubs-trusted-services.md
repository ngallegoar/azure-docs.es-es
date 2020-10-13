---
title: archivo de inclusión
description: archivo de inclusión
services: event-hubs
author: spelluru
ms.service: event-hubs
ms.topic: include
ms.date: 08/07/2020
ms.author: spelluru
ms.custom: include file
ms.openlocfilehash: 7cef6252a99430f0d62d8f976510f0a723badc1f
ms.sourcegitcommit: b4f303f59bb04e3bae0739761a0eb7e974745bb7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/02/2020
ms.locfileid: "91654401"
---
## <a name="trusted-microsoft-services"></a>Servicios de Microsoft de confianza
Al activar la opción **¿Quiere permitir que los servicios de confianza de Microsoft puedan omitir este firewall?** , los siguientes servicios adquieren acceso a los recursos de Event Hubs.

| Servicio de confianza | Escenarios de uso admitidos | 
| --------------- | ------------------------- | 
| Azure Event Grid | Permite que Azure Event Grid envíe eventos a Event Hubs en el espacio de nombres de Event Hubs. También debe seguir estos pasos: <ul><li>Habilite la identidad asignada por el sistema para un tema o un dominio</li><li>Agregue la identidad al rol Remitente de datos de Azure Event Hubs en el espacio de nombres de Event Hubs</li><li>A continuación, configure la suscripción de eventos que emplea un centro de eventos como punto de conexión para usar la identidad asignada por el sistema.</li></ul> <p>Para obtener más información, vea [Entrega de evento con una identidad administrada](../articles/event-grid/managed-service-identity.md)</p>|
| Azure Monitor (configuración de diagnósticos) | Permite que Azure Monitor envíe información de diagnóstico a Event Hubs en el espacio de nombres de Event Hubs. |