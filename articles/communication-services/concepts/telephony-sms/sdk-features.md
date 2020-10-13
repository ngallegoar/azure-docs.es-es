---
title: Información general de la biblioteca cliente de SMS para Azure Communication Services
titleSuffix: An Azure Communication Services concept document
description: Proporciona información general sobre la biblioteca cliente de SMS y sus ofertas.
author: mikben
manager: jken
services: azure-communication-services
ms.author: mikben
ms.date: 09/30/2020
ms.topic: overview
ms.service: azure-communication-services
ms.openlocfilehash: d40637b05a5d398124ee52dbca49d55b3112ec95
ms.sourcegitcommit: 6a4687b86b7aabaeb6aacdfa6c2a1229073254de
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/06/2020
ms.locfileid: "91758411"
---
# <a name="sms-client-library-overview"></a>Información general de la biblioteca cliente de SMS

[!INCLUDE [Public Preview Notice](../../includes/public-preview-include.md)]

Las bibliotecas cliente de SMS de Azure Communication Services se pueden usar para agregar mensajería SMS a las aplicaciones.

## <a name="sms-client-library-capabilities"></a>Funcionalidades de la biblioteca cliente de SMS

En la lista siguiente se presenta el conjunto de características que están disponibles actualmente en nuestras bibliotecas cliente.

| Grupo de características | Capacidad                                                                            | JS  | Java | .NET | Python |
| ----------------- | ------------------------------------------------------------------------------------- | --- | ---- | ---- | ------ |
| Funcionalidades principales | Enviar y recibir mensajes SMS </br> *Se admiten los emojis Unicode*                        | ✔️   | ✔️    | ✔️    | ✔️      |
|                   | Recibir informes de entrega para los mensajes enviados                                            | ✔️   | ✔️    | ✔️    | ✔️      |
|                   | Todos los juegos de caracteres (compatibilidad con lenguaje/Unicode)                                         | ✔️   | ✔️    | ✔️    | ✔️      |
|                   | Compatibilidad con mensajes largos (hasta 2048 caracteres)                                           | ✔️   | ✔️    | ✔️    | ✔️      |
|                   | Concatenación automática de mensajes largos                                                   | ✔️   | ✔️    | ✔️    | ✔️      |
| Events            | Usar Event Grid para configurar webhooks para recibir mensajes entrantes e informes de entrega | ✔️   | ✔️    | ✔️    | ✔️      |
| Número de teléfono      | Números de teléfono gratuitos                                                                     | ✔️   | ✔️    | ✔️    | ✔️      |
| Reglamentario        | Control de deshabilitación de envío                                                                      | ✔️   | ✔️    | ✔️    | ✔️      |
| Supervisión        | Supervisar el uso de los mensajes enviados y recibidos                                          | ✔️   | ✔️    | ✔️    | ✔️      |
| Llamadas RTC      | Agregar funcionalidades de llamadas RTC al número de teléfono gratuito habilitado para SMS                    | ✔️   | ✔️    | ✔️    | ✔️      |

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Introducción al envío de SMS](../../quickstarts/telephony-sms/send.md)

Puede que los siguientes documentos le resulten interesantes:

- Familiarización con los [conceptos de SMS](../telephony-sms/concepts.md) generales
- Obtención de un [número de teléfono](../../quickstarts/telephony-sms/get-phone-number.md) compatible con SMS
- [Planeación de una solución de SMS](../telephony-sms/plan-solution.md)
