---
title: 'Qué es OPC Publisher: Azure | Microsoft Docs'
description: En este artículo se proporciona información general sobre las características de OPC Publisher. Permite publicar datos de telemetría JSON codificados mediante una carga JSON, en Azure IoT Hub.
author: dominicbetts
ms.author: dobett
ms.date: 06/10/2019
ms.topic: overview
ms.service: industrial-iot
services: iot-industrialiot
manager: philmea
ms.custom:
- amqp
- mqtt
ms.openlocfilehash: c77dff4a4f89a78dc0a0d723fbb45fe691246112
ms.sourcegitcommit: acb82fc770128234f2e9222939826e3ade3a2a28
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/21/2020
ms.locfileid: "81681930"
---
# <a name="what-is-opc-publisher"></a>¿Qué es OPC Publisher?

OPC Publisher es una implementación de referencia que demuestra cómo hacer lo siguiente:

- Conectarse a los servidores de OPC UA existentes.
- Publicar datos de telemetría JSON codificados desde servidores de OPC UA con formato Pub/Sub, mediante una carga JSON, en Azure IoT Hub.

Puede usar cualquiera de los protocolos de transporte que admite el SDK del cliente de Azure IoT Hub: HTTPS, AMQP y MQTT.

La implementación de referencia incluye lo siguiente:

- Un *cliente* de OPC UA para conectarse a los servidores de OPC UA que tiene en la red.
- Un *servidor* de OPC UA en el puerto 62222 que puede usar para administrar lo que se publica y ofrece métodos directos de IoT Hub para hacer lo mismo.

Puede descargar la [implementación de referencia de OPC Publisher](https://github.com/Azure/iot-edge-opc-publisher) desde GitHub.

La aplicación se implementa mediante tecnología .NET Core y se puede ejecutar en las plataformas compatibles con .NET Core.

OPC Publisher implementa la lógica de reintento para establecer conexiones a puntos de conexión que no responden a un determinado número de solicitudes de conexión persistente. Por ejemplo, si un servidor OPC UA deja de responder debido a un corte del suministro eléctrico.

Para cada intervalo de publicación diferente de un servidor de OPC UA, la aplicación crea una suscripción independiente en la que se actualizan todos los nodos con este intervalo de publicación.

OPC Publisher admite el procesamiento por lotes de los datos enviados a IoT Hub para reducir la carga de la red. Este procesamiento por lotes envía un paquete a IoT Hub solo si se alcanza el tamaño de paquete configurado.

Esta aplicación usa la pila de referencia de OPC UA de OPC Foundation como paquetes de NuGet. Consulte [https://opcfoundation.org/license/redistributables/1.3/](https://opcfoundation.org/license/redistributables/1.3/) para ver los términos de la licencia.

### <a name="next-steps"></a>Pasos siguientes

Ahora que ha aprendido qué es OPC Publisher, el siguiente paso sugerido es aprender a [configurar OPC Publisher](howto-opc-publisher-configure.md).
