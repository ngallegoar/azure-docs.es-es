---
title: Enrutamiento de eventos y mensajes con Azure Digital Twins | Microsoft Docs
description: Información general sobre el enrutamiento de eventos y mensajes a puntos de conexión de servicio con Azure Digital Twins
ms.author: alinast
author: alinamstanciu
manager: bertvanhoof
ms.service: digital-twins
services: digital-twins
ms.topic: conceptual
ms.date: 01/03/2020
ms.openlocfilehash: 65b760eaf28d907fab3654ed92f960be7556b0d6
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "75862363"
---
# <a name="routing-iot-events-and-messages"></a>Enrutamiento de eventos y mensajes de IoT

A menudo, las soluciones de Internet de las cosas reúnen varios servicios eficaces, como almacenamiento, análisis, etc. En este artículo se describe cómo conectar aplicaciones de Azure Digital Twins a los servicios de almacenamiento, IA y análisis de Azure para proporcionarles más información y funcionalidades.

## <a name="route-types"></a>Tipos de ruta  

Azure Digital Twins ofrece dos formas para conectar eventos de IoT con otros servicios de Azure o en aplicaciones empresariales:

* **Enrutamiento de eventos de Azure Digital Twins**: al cambiar un objeto en el grafo espacial, al recibirse datos de telemetría, o cuando una función definida por el usuario crea una notificación basada en condiciones predefinidas, pueden desencadenarse eventos de Azure Digital Twins. Los usuarios pueden enviar estos eventos a [Azure Event Hubs](https://azure.microsoft.com/services/event-hubs/), a [temas de Azure Service Bus](https://azure.microsoft.com/services/service-bus/) o a [Azure Event Grid](https://azure.microsoft.com/services/event-grid/) para su posterior procesamiento.

* **Enrutamiento de telemetría de dispositivo**: además de los eventos de enrutamientos, Azure Digital Twins también puede enrutar los mensajes de telemetría de dispositivo sin procesar a las instancias de Event Hubs para obtener más información y análisis. Estos tipos de mensajes no los procesa Azure Digital Twins. Únicamente se envían al centro de eventos.

Los usuarios pueden especificar uno o varios puntos de conexión de salida para enviar eventos o para reenviar mensajes. Los eventos y mensajes se enviarán a los puntos de conexión según estas preferencias de enrutamiento predefinidas. En otras palabras, los usuarios pueden especificar un punto de conexión determinado para recibir eventos de operación de grafos, otro para recibir eventos de telemetría de dispositivo, y así sucesivamente.

[![Enrutamiento de eventos de Azure Digital Twins](media/concepts/digital-twins-events-routing.png)](media/concepts/digital-twins-events-routing.png#lightbox)

El enrutamiento a Event Hubs se realiza en el orden de envío de los mensajes de telemetría. Por esto, llegan al punto de conexión en el mismo orden de recepción. 

Event Grid y Service Bus no garantizan que los puntos de conexión reciban los eventos en el mismo orden en que se produjeron. Sin embargo, el esquema del evento incluye una marca de tiempo que se puede utilizar para identificar el orden después de que los eventos lleguen al punto de conexión.

## <a name="route-implementation"></a>Implementación de rutas

El servicio de Azure Digital Twins actualmente admite los siguientes **EndpointTypes**:

* **EventHub** es el punto de conexión de la cadena de conexión de Event Hubs.
* **ServiceBus** es el punto de conexión de la cadena de conexión de Service Bus.
* **EventGrid** es el punto de conexión de la cadena de conexión de Event Grid.

Azure Digital Twins actualmente admite los siguientes **EventTypes** que se enviarán al punto de conexión elegido:

* **DeviceMessages** son mensajes de telemetría enviados desde los dispositivos de los usuarios y reenviados por el sistema.
* **TopologyOperation** es una operación que cambia el grafo o los metadatos de este. Por ejemplo, la incorporación o la eliminación de una entidad, como un espacio.
* **SpaceChange** es un cambio en el valor calculado de un espacio como resultado de un mensaje de telemetría de dispositivo.
* **SensorChange** es un cambio en el valor calculado de un sensor como resultado de un mensaje de telemetría de dispositivo.
* **UdfCustom** es una notificación personalizada de una función definida por el usuario.

> [!IMPORTANT]  
> No todos los **EndpointTypes** admiten todos los **EventTypes**.
> Revise la tabla siguiente para conocer los **EventTypes** que se permiten para cada **EndpointType**.

|             | DeviceMessages | TopologyOperation | SpaceChange | SensorChange | UdfCustom |
| ----------- | -------------- | ----------------- | ----------- | ------------ | --------- |
| EventHub|     X          |         X         |     X       |      X       |   X       |
| ServiceBus|              |         X         |     X       |      X       |   X       |
| EventGrid|               |         X         |     X       |      X       |   X       |

>[!NOTE]  
>Para más información sobre cómo crear puntos de conexión y acceder a ejemplos de esquema de los eventos, lea [Salidas y puntos de conexión](how-to-egress-endpoints.md).

## <a name="next-steps"></a>Pasos siguientes

- Para información acerca de los límites de la versión preliminar de Azure Digital Twins, lea [Límites de servicio de la versión preliminar pública](concepts-service-limits.md).

- Para probar un ejemplo de Azure Digital Twins, lea el [inicio rápido para buscar salas disponibles](quickstart-view-occupancy-dotnet.md).