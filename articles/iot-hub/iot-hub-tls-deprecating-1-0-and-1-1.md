---
title: Desuso de TLS 1.0 y 1.1 en IoT Hub y Device Provisioning Service (DPS) | Microsoft Docs
description: Instrucciones sobre el desuso de TLS 1.0 y 1.1 y los cifrados admitidos en IoT Hub y DPS.
author: rezasherafat
ms.author: rezas
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 12/16/2019
ms.openlocfilehash: d61ca8fe7c6f5e7cc400714d7c31a0a7e50b8a88
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "78402791"
---
# <a name="deprecation-of-tls-10-and-11-in-iot-hub-and-device-provisioning-service"></a>Desuso de TLS 1.0 y 1.1 en IoT Hub y en Device Provisioning Service

Para proporcionar el mejor cifrado, IoT Hub y Device Provisioning Service (DPS) se van a migrar a Seguridad de la capa de transporte (TLS) 1.2 como el mecanismo de cifrado elegido para servicios y dispositivos de IoT. 

## <a name="supported-ciphers"></a>Cifrados compatibles

La escala de tiempo para la disponibilidad de varios cifrados utilizados en el protocolo de enlace TLS es la siguiente:

* TLS_ECDHE_RSA_WITH_AES_128_CBC_SHA256 (actualmente compatible)
* TLS_ECDHE_RSA_WITH_AES_256_CBC_SHA384 (será compatible durante la segunda mitad de 2020)
* TLS_ECDHE_RSA_WITH_AES_128_GCM_SHA256 (será compatible durante la segunda mitad de 2020)
* TLS_ECDHE_RSA_WITH_AES_256_GCM_SHA384 (será compatible durante la segunda mitad de 2020)


## <a name="customer-feedback"></a>Comentarios del cliente

Si bien el cumplimiento de TLS 1.2 es una de las mejores opciones de cifrado de todo el sector y se habilitará según lo planeado, nos gustaría seguir conociendo las opiniones de los clientes sobre sus implementaciones específicas y las dificultades para adoptar TLS 1.2. Con esta finalidad, puede enviar sus comentarios a [iot_tls1_deprecation@microsoft.com](mailto:iot_tls1_deprecation@microsoft.com).
