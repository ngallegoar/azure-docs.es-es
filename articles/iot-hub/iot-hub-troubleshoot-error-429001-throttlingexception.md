---
title: Solución de problemas del error 429001 ThrottlingException de Azure IoT Hub
description: Corrección del error 429001 ThrottlingException
author: jlian
manager: briz
ms.service: iot-hub
services: iot-hub
ms.topic: troubleshooting
ms.date: 01/30/2020
ms.author: jlian
ms.openlocfilehash: 3095e398d7e5cfe59085144d5bb4e8dc33618064
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "76960265"
---
# <a name="429001-throttlingexception"></a>429001 ThrottlingException

En este artículo se describen las causas y las soluciones de los errores **429001 ThrottlingException**.

## <a name="symptoms"></a>Síntomas

Las solicitudes a IoT Hub producen el error **429001 ThrottlingException**.

## <a name="cause"></a>Causa

Los [límites de ancho de banda](./iot-hub-devguide-quotas-throttling.md) de IoT Hub se han superado para la operación solicitada.

## <a name="solution"></a>Solución

Compruebe si está alcanzando el límite; para ello, compare la métrica *Intentos de envío de mensajes de telemetría* con los límites especificados antes. También puede comprobar la métrica *Número de errores de limitación*. Para más información acerca de estas y de otras métricas disponibles para IoT Hub, consulte [Métricas de IoT Hub y cómo usarlas](./iot-hub-metrics.md#iot-hub-metrics-and-how-to-use-them).

IoT Hub devuelve 429 ThrottlingException solo después de que se haya infringido el límite durante un período de tiempo demasiado largo. Esto es así para que los mensajes no se eliminen si el centro de IoT recibe ráfagas de tráfico. Mientras tanto, IoT Hub procesa los mensajes en la velocidad limitada de la operación, que podría ser lenta si hay mucho tráfico en el trabajo pendiente. Para más información, consulte [Modelado del tráfico de IoT Hub](./iot-hub-devguide-quotas-throttling.md#traffic-shaping).

## <a name="next-steps"></a>Pasos siguientes

Considere la posibilidad de [escalar verticalmente la instancia de IoT Hub](./iot-hub-scaling.md) si alcanza los límites de cuota u otras limitaciones.