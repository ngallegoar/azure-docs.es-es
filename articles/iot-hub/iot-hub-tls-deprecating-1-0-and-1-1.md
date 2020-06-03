---
title: Desuso de TLS 1.0 y 1.1 en IoT Hub | Microsoft Docs
description: Instrucciones sobre la puesta en desuso de TLS 1.0 y 1.1 y los cifrados admitidos en IoT Hub.
author: jlian
ms.author: jlian
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 04/14/2020
ms.openlocfilehash: 5c717a02c2008436617d16f08625a1cecc204340
ms.sourcegitcommit: 1f25aa993c38b37472cf8a0359bc6f0bf97b6784
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/26/2020
ms.locfileid: "83849525"
---
# <a name="deprecation-of-tls-10-and-11-in-iot-hub"></a>Desuso de TLS 1.0 y 1.1 en IoT Hub

Para proporcionar el mejor cifrado de su clase, IoT Hub se va a migrar a Seguridad de la capa de transporte (TLS) 1.2 como mecanismo de cifrado elegido para servicios y dispositivos de IoT. 

## <a name="timeline"></a>Escala de tiempo

IoT Hub continuará admitiendo TLS 1.0/1.1 hasta nuevo aviso. De todas formas, se recomienda que todos los clientes migren a TLS 1.2 lo antes posible.

## <a name="deprecating-tls-11-ciphers"></a>Desuso de cifrados TLS 1.1

* `TLS_ECDHE_RSA_WITH_AES_256_CBC_SH`
* `TLS_ECDHE_RSA_WITH_AES_128_CBC_SHA`
* `TLS_RSA_WITH_AES_256_CBC_SHA`
* `TLS_RSA_WITH_AES_128_CBC_SHA`
* `TLS_RSA_WITH_3DES_EDE_CBC_SHA`

## <a name="deprecating-tls-10-ciphers"></a>Desuso de cifrados TLS 1.0

* `TLS_ECDHE_RSA_WITH_AES_256_CBC_SHA`
* `TLS_ECDHE_RSA_WITH_AES_128_CBC_SHA`
* `TLS_RSA_WITH_AES_256_CBC_SHA`
* `TLS_RSA_WITH_AES_128_CBC_SHA`
* `TLS_RSA_WITH_3DES_EDE_CBC_SHA`

## <a name="tls-12-ciphers"></a>Cifrados TLS 1.2

Consulte [los cifrados IoT Hub TLS 1.2 recomendados](iot-hub-tls-support.md#recommended-ciphers).
 
## <a name="customer-feedback"></a>Comentarios del cliente

Si bien el cumplimiento de TLS 1.2 es una de las mejores opciones de cifrado de todo el sector y se habilitará según lo planeado, nos gustaría seguir conociendo las opiniones de los clientes sobre sus implementaciones específicas y las dificultades para adoptar TLS 1.2. Con esta finalidad, puede enviar sus comentarios a [iot_tls1_deprecation@microsoft.com](mailto:iot_tls1_deprecation@microsoft.com).
