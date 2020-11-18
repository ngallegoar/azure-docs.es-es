---
title: Solución del error 400027 ConnectionForcefullyClosedOnNewConnection de Azure IoT Hub
description: Corrección del error 400027 ConnectionForcefullyClosedOnNewConnection
author: jlian
manager: briz
ms.service: iot-hub
services: iot-hub
ms.topic: troubleshooting
ms.date: 01/30/2020
ms.author: jlian
ms.custom:
- mqtt
- fasttrack-edit
- iot
ms.openlocfilehash: e5d1dc345c72d77be6172fb9c3a10eb2f38d186a
ms.sourcegitcommit: 4bee52a3601b226cfc4e6eac71c1cb3b4b0eafe2
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/11/2020
ms.locfileid: "94506335"
---
# <a name="400027-connectionforcefullyclosedonnewconnection"></a>400027 ConnectionForcefullyClosedOnNewConnection

En este artículo se describen las causas y las soluciones de los errores **400027 ConnectionForcefullyClosedOnNewConnection**.

## <a name="symptoms"></a>Síntomas

El dispositivo se desconecta con **Communication_Error** como **ConnectionStatusChangeReason** con el SDK de .NET y el tipo de transporte MQTT.

La operación gemela del dispositivo a la nube (como las propiedades indicadas de lectura o revisión) o la invocación del método directo producen un error con el código **400027**.

## <a name="cause"></a>Causa

Otro cliente creó una conexión nueva para IoT Hub con las mismas credenciales, por lo que IoT Hub cerró la conexión anterior. IoT Hub no permite que más de un cliente se conecte con el mismo conjunto de credenciales.

## <a name="solution"></a>Solución

Asegúrese de que cada cliente se conecta a IoT Hub mediante su propia identidad.