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
ms.openlocfilehash: f4949816f516c6a6b60cfda0602f458256370d40
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "76960213"
---
# <a name="400027-connectionforcefullyclosedonnewconnection"></a>400027 ConnectionForcefullyClosedOnNewConnection

En este artículo se describen las causas y las soluciones de los errores **400027 ConnectionForcefullyClosedOnNewConnection**.

## <a name="symptoms"></a>Síntomas

La operación gemela del dispositivo a la nube (como las propiedades indicadas de lectura o revisión) o la invocación del método directo producen un error con el código **400027**.

## <a name="cause"></a>Causa

Otro cliente creó una conexión nueva para IoT Hub con las mismas credenciales, por lo que IoT Hub cerró la conexión anterior. IoT Hub no permite que más de un cliente se conecte con el mismo conjunto de credenciales.

## <a name="solution"></a>Solución

Asegúrese de que cada cliente se conecta a IoT Hub mediante su propia identidad.