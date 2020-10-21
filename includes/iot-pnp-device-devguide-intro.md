---
author: dominicbetts
ms.author: dobett
ms.service: iot-pnp
ms.topic: include
ms.date: 09/24/2020
ms.openlocfilehash: 706e936590f5f47ee2989bf77b3caad67a8f28fc
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "91579390"
---
IoT Plug and Play permite crear dispositivos inteligentes que anuncian sus funcionalidades en aplicaciones de Azure IoT. Los dispositivos IoT Plug and Play no requieren configuración manual cuando un cliente los conecta a aplicaciones habilitadas para IoT Plug and Play.

Un dispositivo inteligente podría implementarse directamente, usar [módulos](../articles/iot-hub/iot-hub-devguide-module-twins.md) o usar [módulos de IoT Edge](../articles/iot-edge/about-iot-edge.md).

En esta guía se describen los pasos básicos necesarios para crear un dispositivo, un módulo o un módulo de IoT Edge que sigan las [convenciones de IoT Plug and Play](../articles/iot-pnp/concepts-convention.md).

Para compilar un dispositivo o un módulo IoT Plug and Play o un módulo de IoT Edge, siga estos pasos:

1. Asegúrese de que el dispositivo usa el protocolo MQTT o MQTT sobre WebSockets para conectarse a Azure IoT Hub.
1. Cree un modelo de [Lenguaje de definición de Digital Twins (DTDL)](https://github.com/Azure/opendigitaltwins-dtdl) para describir el dispositivo. Para más información, consulte [Descripción de los componentes de los modelos de IoT Plug and Play](../articles/iot-pnp/concepts-components.md).
1. Actualice el dispositivo o el módulo para anunciar el elemento `model-id` como parte de la conexión del dispositivo.
1. Implemente telemetría, propiedades y comandos mediante las [convenciones de IoT Plug and Play](../articles/iot-pnp/concepts-convention.md).

Una vez que la implementación del dispositivo o del módulo esté lista, use [Azure IoT Explorer](../articles/iot-pnp/howto-use-iot-explorer.md) para validar que el dispositivo o el módulo sigan las convenciones de IoT Plug and Play.
