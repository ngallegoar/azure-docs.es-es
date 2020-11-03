---
title: archivo de inclusión
description: archivo de inclusión
author: robinsh
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 04/28/2019
ms.author: robinsh
ms.custom: include file
ms.openlocfilehash: 36c05badb3b2292a29b8227c7f03b841474c97ad
ms.sourcegitcommit: d767156543e16e816fc8a0c3777f033d649ffd3c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/26/2020
ms.locfileid: "92548224"
---
Puede usar la API REST [Get Endpoint Health](https://docs.microsoft.com/rest/api/iothub/iothubresource/getendpointhealth#iothubresource_getendpointhealth) para obtener el estado de mantenimiento de los puntos de conexión. Se recomienda usar las [métricas de enrutamiento de IoT Hub](../articles/iot-hub/monitor-iot-hub-reference.md#routing-metrics) relativas a la latencia de mensajes de enrutamiento para identificar y depurar errores cuando el estado del punto de conexión no responda o esté en mal estado, ya que se espera que la latencia sea mayor con el punto de conexión en uno de esos estados. Para obtener más información sobre el uso de métricas de IoT Hub, consulte [Supervisión de IoT Hub](../articles/iot-hub/monitor-iot-hub.md).

|Estado de mantenimiento|Descripción|
|---|---|
|healthy|El punto de conexión acepta los mensajes según lo previsto.|
|unhealthy|El punto de conexión no acepta mensajes e IoT Hub está intentando volver a enviarlos.|
|unknown|IoT Hub no intentó enviar mensajes a este punto de conexión.|
|degraded|El punto de conexión está aceptando mensajes más lentamente de lo esperado o se está recuperando de un estado incorrecto.|
|dead|IoT Hub ya no entrega mensajes a este punto de conexión. No se pudieron enviar los mensajes a este punto de conexión.|
