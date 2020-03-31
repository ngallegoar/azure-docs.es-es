---
title: Solución de problemas del error 403002 de Azure IoT Hub IoTHubQuotaExceeded
description: Corrección del error 403002 IoTHubQuotaExceeded
author: jlian
manager: briz
ms.service: iot-hub
services: iot-hub
ms.topic: troubleshooting
ms.date: 01/30/2020
ms.author: jlian
ms.openlocfilehash: 8312c3267e826088f34b3bffe1520703eec00bdc
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "76960393"
---
# <a name="403002-iothubquotaexceeded"></a>403002 IoTHubQuotaExceeded

En este artículo se describen las causas y las soluciones de los errores **403002 IoTHubQuotaExceeded**.

## <a name="symptoms"></a>Síntomas

Todas las solicitudes a IoT Hub producen el error **403002 IoTHubQuotaExceeded**. En Azure Portal, la lista de dispositivos de IoT Hub no se carga.

## <a name="cause"></a>Causa

Se superó la cuota de mensajes diaria para el centro de IoT. 

## <a name="solution"></a>Solución

[Actualice o aumente el número de unidades en el centro de IoT](iot-hub-upgrade.md) o espere a que se actualice la cuota diaria del siguiente día en horario UTC.

## <a name="next-steps"></a>Pasos siguientes

* Para entender cómo se cuentan las operaciones en la cuota, como las consultas gemelas y los métodos directos, consulte [Precios de IoT Hub](iot-hub-devguide-pricing.md#charges-per-operation).
* Para configurar la supervisión del uso diario de cuota, configure una alerta con la métrica *Total number of messages used* (Número total de mensajes utilizados). Para instrucciones paso a paso, consulte el artículo de [Configuración de métricas y alertas con IoT Hub](tutorial-use-metrics-and-diags.md#set-up-metrics).