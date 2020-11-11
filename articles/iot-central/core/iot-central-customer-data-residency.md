---
title: Residencia de datos del cliente en Azure IoT Central | Microsoft Docs
description: En este artículo se describe la residencia de datos del cliente en las aplicaciones de Azure IoT Central.
author: dominicbetts
ms.author: dobett
ms.date: 11/02/2020
ms.topic: conceptual
ms.service: iot-central
services: iot-central
ms.openlocfilehash: 9cf803ddd5a3c9329cadefffc35ae6d6999323a1
ms.sourcegitcommit: 58f12c358a1358aa363ec1792f97dae4ac96cc4b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/03/2020
ms.locfileid: "93280663"
---
# <a name="azure-iot-central-customer-data-residency"></a>Residencia de datos del cliente de Azure IoT Central

IoT Central no almacena datos de clientes fuera de la geografía especificada por el cliente, excepto en los siguientes escenarios:

- Cuando se agrega un nuevo usuario a una aplicación de IoT Central existente, el identificador de correo electrónico del usuario se puede almacenar fuera de la geografía hasta que el usuario invitado accede a la aplicación por primera vez.

- Los mosaicos de mapa del panel de IoT Central usan [Azure Maps](../../azure-maps/about-azure-maps.md). Cuando se agrega un mosaico de mapa a una aplicación de IoT Central existente, los datos de ubicación se pueden procesar o almacenar de acuerdo con las reglas de geolocalización del servicio Azure Maps.
