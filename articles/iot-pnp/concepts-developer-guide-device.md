---
title: 'Guía para desarrolladores de dispositivos (C): IoT Plug and Play | Microsoft Docs'
description: Descripción de IoT Plug and Play para desarrolladores de dispositivos de C
author: rido-min
ms.author: rmpablos
ms.date: 11/19/2020
ms.topic: conceptual
ms.service: iot-pnp
services: iot-pnp
zone_pivot_groups: programming-languages-set-twenty-six
ms.openlocfilehash: 8d8da39f038f465030a2dced092ab1b008e30e5e
ms.sourcegitcommit: b8eba4e733ace4eb6d33cc2c59456f550218b234
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/23/2020
ms.locfileid: "95511365"
---
# <a name="iot-plug-and-play-device-developer-guide"></a>Guía para desarrolladores de dispositivos IoT Plug and Play

IoT Plug and Play permite crear dispositivos inteligentes que anuncian sus funcionalidades en aplicaciones de Azure IoT. Los dispositivos IoT Plug and Play no requieren configuración manual cuando un cliente los conecta a aplicaciones habilitadas para IoT Plug and Play.

Un dispositivo inteligente podría implementarse directamente, usar [módulos](../iot-hub/iot-hub-devguide-module-twins.md) o usar [módulos de IoT Edge](../iot-edge/about-iot-edge.md).

En esta guía se describen los pasos básicos necesarios para crear un dispositivo, un módulo o un módulo de IoT Edge que sigan las [convenciones de IoT Plug and Play](../iot-pnp/concepts-convention.md).

Para compilar un dispositivo o un módulo IoT Plug and Play o un módulo de IoT Edge, siga estos pasos:

1. Asegúrese de que el dispositivo usa el protocolo MQTT o MQTT sobre WebSockets para conectarse a Azure IoT Hub.
1. Cree un modelo de [Lenguaje de definición de Digital Twins (DTDL)](https://github.com/Azure/opendigitaltwins-dtdl) para describir el dispositivo. Para más información, consulte [Descripción de los componentes de los modelos de IoT Plug and Play](concepts-components.md).
1. Actualice el dispositivo o el módulo para anunciar el elemento `model-id` como parte de la conexión del dispositivo.
1. Implemente telemetría, propiedades y comandos mediante las [convenciones de IoT Plug and Play](concepts-convention.md).

Una vez que la implementación del dispositivo o del módulo esté lista, use [Azure IoT Explorer](howto-use-iot-explorer.md) para validar que el dispositivo o el módulo sigan las convenciones de IoT Plug and Play.

:::zone pivot="programming-language-ansi-c"

[!INCLUDE [iot-pnp-device-devguide-csharp](../../includes/iot-pnp-device-devguide-c.md)]

:::zone-end

:::zone pivot="programming-language-csharp"

[!INCLUDE [iot-pnp-device-devguide-csharp](../../includes/iot-pnp-device-devguide-csharp.md)]

:::zone-end

:::zone pivot="programming-language-java"

[!INCLUDE [iot-pnp-device-devguide-java](../../includes/iot-pnp-device-devguide-java.md)]

:::zone-end

:::zone pivot="programming-language-javascript"

[!INCLUDE [iot-pnp-device-devguide-node](../../includes/iot-pnp-device-devguide-node.md)]

:::zone-end

:::zone pivot="programming-language-python"

[!INCLUDE [iot-pnp-device-devguide-python](../../includes/iot-pnp-device-devguide-python.md)]

:::zone-end

## <a name="next-steps"></a>Pasos siguientes

Ahora que aprendió sobre el desarrollo de dispositivos IoT Plug and Play, estos son algunos recursos adicionales:

- [Lenguaje de definición de Digital Twins (DTDL)](https://github.com/Azure/opendigitaltwins-dtdl)
- [SDK para dispositivos C](https://docs.microsoft.com/azure/iot-hub/iot-c-sdk-ref/)
- [API REST de IoT](https://docs.microsoft.com/rest/api/iothub/device)
- [Componentes del modelo](concepts-components.md)
- [Instalación y uso de las herramientas de creación de DTDL](howto-use-dtdl-authoring-tools.md)
- [Guía para desarrolladores de dispositivos IoT Plug and Play](concepts-developer-guide-service.md)
