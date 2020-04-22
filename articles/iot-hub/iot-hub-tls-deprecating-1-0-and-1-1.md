---
title: Desuso de TLS 1.0 y 1.1 en IoT Hub | Microsoft Docs
description: Instrucciones sobre la puesta en desuso de TLS 1.0 y 1.1 y los cifrados admitidos en IoT Hub.
author: jlian
ms.author: jlian
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 04/14/2020
ms.openlocfilehash: a887dd4df44ba58b0e6646ffb1c10eb21edf3e69
ms.sourcegitcommit: ea006cd8e62888271b2601d5ed4ec78fb40e8427
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/14/2020
ms.locfileid: "81381293"
---
# <a name="deprecation-of-tls-10-and-11-in-iot-hub"></a>Desuso de TLS 1.0 y 1.1 en IoT Hub

Para proporcionar el mejor cifrado de su clase, IoT Hub se va a migrar a Seguridad de la capa de transporte (TLS) 1.2 como mecanismo de cifrado elegido para servicios y dispositivos de IoT. 

## <a name="timeline"></a>Escala de tiempo

IoT Hub continuará admitiendo TLS 1.0/1.1 hasta nuevo aviso. De todas formas, se recomienda que todos los clientes migren a TLS 1.2 lo antes posible.

## <a name="supported-ciphers"></a>Cifrados compatibles

La escala de tiempo para la disponibilidad de varios cifrados utilizados en el protocolo de enlace TLS es la siguiente:

* TLS_ECDHE_RSA_WITH_AES_128_CBC_SHA256 (actualmente compatible)
* TLS_ECDHE_RSA_WITH_AES_256_CBC_SHA384 (será compatible durante la segunda mitad de 2020)
* TLS_ECDHE_RSA_WITH_AES_128_GCM_SHA256 (será compatible durante la segunda mitad de 2020)
* TLS_ECDHE_RSA_WITH_AES_256_GCM_SHA384 (será compatible durante la segunda mitad de 2020)

## <a name="customer-feedback"></a>Comentarios del cliente

Si bien el cumplimiento de TLS 1.2 es una de las mejores opciones de cifrado de todo el sector y se habilitará según lo planeado, nos gustaría seguir conociendo las opiniones de los clientes sobre sus implementaciones específicas y las dificultades para adoptar TLS 1.2. Con esta finalidad, puede enviar sus comentarios a [iot_tls1_deprecation@microsoft.com](mailto:iot_tls1_deprecation@microsoft.com).
