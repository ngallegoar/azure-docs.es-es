---
title: Mensajes y conexiones de Azure SignalR Service
description: Introducción a los conceptos claves de mensajes y conexiones de Azure SignalR Service.
author: sffamily
ms.service: signalr
ms.topic: conceptual
ms.date: 03/01/2019
ms.author: zhshang
ms.openlocfilehash: 5f6428231a3639738e8fb52e7dc3f2f2a3d2a26e
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "75392809"
---
# <a name="messages-and-connections-in-azure-signalr-service"></a>Mensajes y conexiones de Azure SignalR Service

Azure SignalR Service tiene un modelo de facturación basado en el número de conexiones y el número de mensajes. Este artículo explica cómo se definen y cuentan los mensajes y las conexiones para la facturación.


## <a name="message-formats"></a>Formatos de mensajes 

Azure SignalR Service admite los mismos formatos que admite ASP.NET Core SignalR: [JSON](https://www.json.org/) y [MessagePack](/aspnet/core/signalr/messagepackhubprotocol).

## <a name="message-size"></a>Tamaño del mensaje

Azure SignalR Service no tiene límite de tamaño para los mensajes.

Los mensajes grandes se dividen en mensajes más pequeños de no más de 2 KB cada uno que se transmiten por separado. Los SDK controlan la división y el ensamblado de los mensajes. El desarrollador no debe hacer nada.

Los mensajes grandes afectan negativamente al rendimiento de la mensajería. Use mensajes de menor tamaño siempre que sea posible y pruebe para determinar el tamaño de mensaje óptimo para cada escenario de caso de uso.

## <a name="how-messages-are-counted-for-billing"></a>Cómo se cuentan los mensajes para la facturación

Para la facturación, solo se cuentan mensajes salientes de Azure SignalR Service. Los mensajes de ping entre clientes y servidores se omiten.

Los mensajes mayores de 2 KB se cuentan como mensajes múltiples de 2 KB cada uno. El gráfico de recuento de mensajes de Azure Portal se actualiza cada 100 mensajes por centro.

Por ejemplo, suponga que tiene tres clientes y un servidor de aplicaciones. Un cliente envía un mensaje de 4 KB para permitir que el servidor difunda a todos los clientes. El número de mensajes es ocho: un mensaje desde el servicio al servidor de aplicaciones y tres mensajes desde el servicio a los clientes. Cada mensaje se cuenta como dos mensajes de 2 KB.

## <a name="how-connections-are-counted"></a>Cómo se cuentan las conexiones

Existen conexiones de servidores y conexiones de cliente con Azure SignalR Service. De manera predeterminada, cada servidor de aplicaciones comienza con cinco conexiones iniciales por centro de conectividad y cada cliente tiene una conexión de cliente.

El recuento de conexiones que se muestra en Azure Portal incluye conexiones de servidor y conexiones de cliente.

Por ejemplo, suponga que tiene dos servidores de aplicaciones y define cinco centros en el código. El número de conexiones del servidor será 50: 2 servidores de aplicaciones * 5 centros * 5 conexiones por centro.

ASP.NET SignalR calcula las conexiones de los servidores de otra forma. Incluye un centro predeterminado además de los centros que defina. De forma predeterminada, cada servidor de aplicaciones necesita cinco conexiones de los servidores. El número de conexiones iniciales del centro predeterminado se mantiene igual que el de los restantes centros.

Durante la vigencia del servidor de aplicaciones, tanto el servicio como el servidor de aplicaciones mantienen el estado de la conexión de sincronización y realizan el ajuste en las conexiones del servidor para mejorar el rendimiento y la estabilidad del servicio. Por lo tanto, puede ver que el número de conexiones con el servidor cambia de vez en cuando.

## <a name="how-inboundoutbound-traffic-is-counted"></a>Cómo se cuenta el tráfico entrante y saliente

La distinción entre el tráfico entrante y el tráfico saliente se basa en la perspectiva de Azure SignalR Service. El tráfico se calcula en bytes.

## <a name="related-resources"></a>Recursos relacionados

- [Tipos de agregación en Azure Monitor](/azure/azure-monitor/platform/metrics-supported#microsoftsignalrservicesignalr )
- [Configuración de ASP.NET Core SignalR](/aspnet/core/signalr/configuration)
- [JSON](https://www.json.org/)
- [MessagePack](/aspnet/core/signalr/messagepackhubprotocol)
