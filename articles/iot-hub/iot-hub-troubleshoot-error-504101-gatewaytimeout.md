---
title: Solución de problemas del error 504101 GatewayTimeout de Azure IoT Hub
description: Corrección del error 504101 GatewayTimeout
author: jlian
manager: briz
ms.service: iot-hub
services: iot-hub
ms.topic: troubleshooting
ms.date: 01/30/2020
ms.author: jlian
ms.openlocfilehash: b74a93e15d533bf9b15797e6371a25230f7a08f7
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "76960257"
---
# <a name="504101-gatewaytimeout"></a>504101 GatewayTimeout

En este artículo se describen las causas y las soluciones de los errores **504101 GatewayTimeout**.

## <a name="symptoms"></a>Síntomas

Al intentar invocar un método directo desde IoT Hub para un dispositivo, se produce un error en la solicitud **504101 GatewayTimeout**.

## <a name="cause"></a>Causa

### <a name="cause-1"></a>Causa 1

IoT Hub detectó un error y no pudo confirmar si el método directo se completó antes de que se agotara el tiempo de espera.

### <a name="cause-2"></a>Causa 2

Cuando se usa una versión anterior del SDK de C# para Azure IoT (<1.19.0), el vínculo AMQP entre el dispositivo e IOT Hub se puede eliminar de forma silenciosa debido a un error.

## <a name="solution"></a>Solución

### <a name="solution-1"></a>Solución 1

Emita un reintento.

### <a name="solution-2"></a>Solución 2

Actualice a la versión más reciente del SDK de C# para Azure IoT.