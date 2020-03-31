---
title: La importancia de las notificaciones push en las aplicaciones móviles con Visual Studio App Center y Azure Notification Hubs
description: Obtenga información sobre servicios como Visual Studio App Center que le permiten interactuar con los usuarios de la aplicación móvil.
author: codemillmatt
ms.assetid: 12bbb070-9b3c-4faf-8588-ccff02097224
ms.service: vs-appcenter
ms.topic: article
ms.date: 03/24/2020
ms.author: masoucou
ms.openlocfilehash: bc59857cdb843dfff54e69f2226f13cfe70df8f0
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "80235328"
---
# <a name="engage-with-your-application-users-by-sending-push-notifications"></a>Interacción con los usuarios de la aplicación enviando notificaciones push

Tanto si va a compilar aplicaciones empresariales como de consumidor, quiere que los usuarios usen activamente su aplicación. A menudo, hay ocasiones en las que quiere compartir noticias de última hora, actualizaciones de juegos, ofertas emocionantes, actualizaciones de productos o cualquier información relevante con los usuarios. Para aumentar la interacción con los usuarios y hacer que sigan volviendo a su aplicación, necesita una manera de comunicarse con ellos en tiempo real.

Las notificaciones push ofrecen una manera rápida y eficaz de comunicarse con los usuarios de la aplicación. Puede ponerse en contacto con los usuarios en el momento adecuado y notificarles la información que quiera, normalmente en un cuadro de diálogo o un elemento emergente en un dispositivo móvil, independientemente de lo que hagan.

## <a name="value-of-push-notifications"></a>Valor de las notificaciones push
Las notificaciones push proporcionan valor a usuarios y empresas.

Las empresas pueden:
- Comunicarse directamente con sus usuarios enviándoles mensajes en las plataformas compatibles en el momento adecuado.
- Aumentar la implicación del usuario mediante el envío de actualizaciones y recordatorios en tiempo real a los usuarios, animándoles a interactuar con la aplicación de forma periódica.
- Conservar usuarios y volver a interactuar con usuarios inactivos que hayan descargado la aplicación pero no la usen para volver a activarlos.
- Aumentar las tasas de conversión mediante el análisis del comportamiento del usuario, la segmentación de los usuarios y el aprovechamiento de campañas basadas en notificaciones de inserción en el dispositivo móvil.
- Promover productos y servicios mediante el envío de mensajes de inserción y actualizaciones oportunas a los usuarios.
- Dirigirse a los usuarios mediante el envío de mensajes de inserción personalizados.

Para los usuarios de la aplicación, las notificaciones push:
- Ofrecen valor e información en tiempo real.
- Recuerdan a los usuarios que usen la aplicación.

Use los servicios siguientes para habilitar notificaciones push en sus aplicaciones móviles.

## <a name="visual-studio-app-center"></a>Visual Studio App Center
[App Center Push](/appcenter/push/) le permite enviar mensajes dirigidos a los usuarios de iOS, Android y Windows sin tener que administrar el proceso de envío de notificaciones a los dispositivos con Servicios de notificaciones push (PNS). Basado en Azure Notification Hubs, este servicio elimina las complejidades asociadas a la inserción de notificaciones de forma manual, ya que proporciona un panel eficaz.

**Características principales**
- Envíe notificaciones push a dispositivos móviles a través de diversas plataformas.
- Use notificaciones para enviar datos a una aplicación, mostrar un mensaje al usuario o desencadenar una acción a través de la aplicación.
- Con los destinos de notificación: 
    - Difunda mensajes a todos los dispositivos registrados.
    - Envíe notificaciones a audiencias en función de la información del dispositivo y de las propiedades personalizadas.
    - Envíe notificaciones push a usuarios específicos.
    - Envíe notificaciones push a dispositivos específicos.
- Haga uso de la telemetría enriquecida en inserciones, dispositivos y errores que están disponibles en el portal de App Center.
- Obtenga compatibilidad con las plataformas iOS, Android, macOS, Xamarin, React Native, Unity y Cordova.

**Referencias**
- [Registro con Visual Studio App Center](https://appcenter.ms/signup?utm_source=Mobile%20Development%20Docs&utm_medium=Azure&utm_campaign=New%20azure%20docs)
- [Introducción a App Center Push](/appcenter/push/)

## <a name="azure-notification-hubs"></a>Azure Notification Hubs
[Notification Hubs](/azure/notification-hubs/notification-hubs-push-notification-overview) proporciona un motor de inserción fácil de usar y escalado horizontalmente. Puede utilizarlo para enviar notificaciones push a cualquier plataforma y desde cualquier entorno back-end, en la nube o local.

**Características principales**
- Envíe notificaciones push a cualquier plataforma desde cualquier entorno back-end, en la nube o local.
- Rápida difusión de notificaciones push a millones de dispositivos móviles con una única llamada de API.
- Adapte las notificaciones push en función del cliente, el idioma y la ubicación.
- Defina y notifique dinámicamente los segmentos de clientes.
- Escale al momento a millones de dispositivos móviles.
- Obtenga compatibilidad con las plataformas iOS, Android, Windows, Kindle y Baidu.
        
**Referencias**
- [Azure Portal](https://portal.azure.com) 
- [Introducción a Azure Notification Hubs](/azure/notification-hubs/)
- [Guías de inicio rápido](/azure/notification-hubs/create-notification-hub-portal)
- [Muestras](/azure/notification-hubs/samples)
