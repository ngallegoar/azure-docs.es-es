---
title: archivo de inclusión
description: archivo de inclusión
services: service-bus-messaging
author: clemensv
ms.service: service-bus-messaging
ms.topic: include
ms.date: 11/24/2020
ms.author: clemensv
ms.custom: include file
ms.openlocfilehash: fe205524c5aef6a43771cc09c810da217678d108
ms.sourcegitcommit: a43a59e44c14d349d597c3d2fd2bc779989c71d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/25/2020
ms.locfileid: "96022154"
---
La opción de protocolo AMQP sobre WebSockets se ejecuta a través del puerto TCP 443 como la API HTTP o de REST, pero, de lo contrario, es funcionalmente idéntica a AMQP sin modificar. Esta opción tiene una latencia de conexión inicial un poco más alta debido a los intercambios de protocolo de enlace adicionales y una sobrecarga ligeramente mayor como compensación para compartir el puerto HTTPS. Si se selecciona este modo, el puerto TCP 443 es suficiente para la comunicación. Las opciones siguientes permiten seleccionar el modo WebSockets AMQP o AMQP:

| Lenguaje | Opción   |
| -------- | ----- |
| .NET     | La propiedad [ServiceBusConnection.TransportType](/dotnet/api/microsoft.azure.servicebus.servicebusconnection.transporttype?view=azure-dotnet) con [TransportType.Amqp](/dotnet/api/microsoft.azure.servicebus.transporttype?view=azure-dotnet) o [TransportType.AmqpWebSockets](/dotnet/api/microsoft.azure.servicebus.transporttype?view=azure-dotnet) |
| Java     | [com.microsoft.azure.servicebus.ClientSettings](/java/api/com.microsoft.azure.servicebus.clientsettings.clientsettings?view=azure-java-stable) con [com.microsoft.azure.servicebus.primitives.TransportType.AMQP](/java/api/com.microsoft.azure.servicebus.primitives.transporttype?view=azure-java-stable) o [com.microsoft.azure.servicebus.primitives.TransportType.AMQP_WEB_SOCKETS](/java/api/com.microsoft.azure.servicebus.primitives.transporttype?view=azure-java-stable) |
| Nodo  | [ServiceBusClientOptions](/javascript/api/@azure/service-bus/servicebusclientoptions?view=azure-node-latest) tiene un argumento de constructor `webSocket`. |
| Python | [ServiceBusClient.transport_type](https://azuresdkdocs.blob.core.windows.net/$web/python/azure-servicebus/latest/azure.servicebus.html#azure.servicebus.ServiceBusClient) con [TransportType.Amqp](https://azuresdkdocs.blob.core.windows.net/$web/python/azure-servicebus/latest/azure.servicebus.html#azure.servicebus.TransportType) o [TransportType.AmqpOverWebSocket](https://azuresdkdocs.blob.core.windows.net/$web/python/azure-servicebus/latest/azure.servicebus.html#azure.servicebus.TransportType) |