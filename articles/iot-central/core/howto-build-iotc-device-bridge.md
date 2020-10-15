---
title: Creación del puente de dispositivos de Azure IoT Central | Microsoft Docs
description: Cree el puente de dispositivos de IoT Central para conectar otras nubes de IoT (Sigfox, Particle, The Things Network, etc.) a la aplicación IoT Central.
services: iot-central
ms.service: iot-central
author: viv-liu
ms.author: viviali
ms.date: 07/09/2019
ms.topic: how-to
manager: peterpr
ms.openlocfilehash: 6499c9c29d10a2056b0af5499b68b5edd67d82cb
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "80158425"
---
# <a name="build-the-iot-central-device-bridge-to-connect-other-iot-clouds-to-iot-central"></a>Creación del puente de dispositivos de IoT Central para conectar otras nubes de IoT a IoT Central

*Este tema se aplica a administradores*.

El puente de dispositivos de IoT Central es una solución de código abierto que se conecta su nube de Sigfox, Particle, The Things Network y otras nubes a la aplicación IoT Central. Si usa dispositivos de seguimiento de activos conectados a la red de área amplia de baja potencia de Sigfox, usa dispositivos de control de la calidad del aire en Device Cloud de Particle o usa dispositivos de supervisión de la humedad de suelo en TTN, puede aprovechar directamente el potencial de IoT Central mediante el puente de dispositivos de IoT Central. El puente de dispositivos conecta otras nubes de IoT con IoT Central mediante el reenvío de los datos que los dispositivos envían a las demás nubes a través a la aplicación IoT Central. En la aplicación IoT Central, puede crear reglas y ejecutar análisis en esos datos, crear flujos de trabajo en aplicaciones de Microsoft Flow y Azure Logic, exportar los datos y mucho más. Obtenga el [puente de dispositivos de IoT Central](https://aka.ms/iotcentralgithubdevicebridge) desde GitHub.

## <a name="what-is-it-and-how-does-it-work"></a>¿Qué es y cómo funciona?
El puente de dispositivos de IoT Central es una solución de código abierto en GitHub. Funciona con un botón "Implementar en Azure" que implementa una plantilla de Azure Resource Manager personalizada con varios recursos de Azure en su suscripción de Azure. Los recursos incluyen:
-    Aplicación de función de Azure
-    Cuenta de Azure Storage
-    Plan de consumo
-    Azure Key Vault

La aplicación de función es la pieza fundamental del puente de dispositivos. Recibe las solicitudes HTTP POST desde otras plataformas de IoT o desde cualquier plataforma personalizada a través de una integración de webhook sencilla. Se incluyen ejemplos que muestran cómo conectarse a las nubes de Sigfox Particle y TTN. Puede ampliar fácilmente esta solución para conectarse a su nube de IoT personalizada si su plataforma puede enviar solicitudes HTTP POST a la aplicación de función.
La aplicación de función transforma los datos en un formato aceptado por IoT Central y los reenvía a través de las API de DPS.

![Captura de pantalla de Azure Functions](media/howto-build-iotc-device-bridge/azfunctions.png)

Si la aplicación de IoT Central reconoce el dispositivo por identificador de dispositivo en el mensaje reenviado, aparecerá una nueva medición para dicho dispositivo. Si la aplicación de IoT Central nunca ha visto el identificador de dispositivo, la aplicación de funciones intentará registrar un dispositivo nuevo con ese identificador de dispositivo y aparecerá como "dispositivo no asociado" en la aplicación de IoT Central. 

## <a name="how-do-i-set-it-up"></a>¿Cómo se configura?
Las instrucciones se muestran en detalle en el archivo LÉAME en el repositorio de GitHub. 

## <a name="pricing"></a>Precios
Los recursos de Azure se hospedarán en su suscripción de Azure. Puede aprender más sobre los precios en el [archivo LÉAME](https://aka.ms/iotcentralgithubdevicebridge).

## <a name="next-steps"></a>Pasos siguientes
Ahora que ha aprendido a crear un puente de dispositivos de IoT Central, este es el paso siguiente sugerido:

> [!div class="nextstepaction"]
> [Administración de dispositivos](howto-manage-devices.md)
