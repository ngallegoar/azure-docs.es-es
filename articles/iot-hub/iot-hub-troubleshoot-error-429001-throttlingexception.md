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
ms.openlocfilehash: d2f12a6982886eeaa375151c5b8a73acc573aab9
ms.sourcegitcommit: d767156543e16e816fc8a0c3777f033d649ffd3c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/26/2020
ms.locfileid: "92545368"
---
# <a name="429001-throttlingexception"></a>429001 ThrottlingException

En este artículo se describen las causas y las soluciones de los errores **429001 ThrottlingException**.

## <a name="symptoms"></a>Síntomas

Las solicitudes a IoT Hub producen el error **429001 ThrottlingException**.

## <a name="cause"></a>Causa

Los [límites de ancho de banda](./iot-hub-devguide-quotas-throttling.md) de IoT Hub se han superado para la operación solicitada.

## <a name="solution"></a>Solución

Compruebe si está alcanzando el límite; para ello, compare la métrica *Intentos de envío de mensajes de telemetría* con los límites especificados antes. También puede comprobar la métrica *Número de errores de limitación*. Para obtener información sobre estas métricas, vea [Métricas de telemetría de dispositivos](monitor-iot-hub-reference.md#device-telemetry-metrics). Para obtener información sobre cómo usar las métricas para facilitar la supervisión del centro de IoT, vea [Supervisión de IoT Hub](monitor-iot-hub.md).

IoT Hub devuelve 429 ThrottlingException solo después de que se haya infringido el límite durante un período de tiempo demasiado largo. Esto es así para que los mensajes no se eliminen si el centro de IoT recibe ráfagas de tráfico. Mientras tanto, IoT Hub procesa los mensajes en la velocidad limitada de la operación, que podría ser lenta si hay mucho tráfico en el trabajo pendiente. Para más información, consulte [Modelado del tráfico de IoT Hub](./iot-hub-devguide-quotas-throttling.md#traffic-shaping).

## <a name="next-steps"></a>Pasos siguientes

Considere la posibilidad de [escalar verticalmente la instancia de IoT Hub](./iot-hub-scaling.md) si alcanza los límites de cuota u otras limitaciones.