---
title: Módulo de seguridad y dispositivo gemelos
description: Sepa en qué consiste el concepto de módulos gemelos de seguridad y cómo se usan en Defender para IoT.
services: defender-for-iot
ms.service: defender-for-iot
documentationcenter: na
author: mlottner
manager: rkarlin
editor: ''
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/24/2019
ms.author: mlottner
ms.openlocfilehash: b33c456d47426a3721e8582f24ffd603db0429c9
ms.sourcegitcommit: 9eda79ea41c60d58a4ceab63d424d6866b38b82d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/30/2020
ms.locfileid: "96340040"
---
# <a name="security-module"></a>Módulo de seguridad

En este artículo se explica el modo en que Defender para IoT usa módulos y dispositivos gemelos.

## <a name="device-twins"></a>Dispositivos gemelos

En el caso de las soluciones de IoT integradas en Azure, los dispositivos gemelos desempeñan un rol clave tanto en la administración de dispositivos como en la automatización de procesos.

Defender para IoT ofrece integración completa con la plataforma de administración de dispositivos de IoT existente, lo que permite no solo administrar el estado de seguridad de los dispositivos, sino también hacer uso de las funcionalidades de control de dispositivos existentes. La integración se consigue usando el mecanismo gemelo de IoT Hub.

Obtenga más información sobre el concepto de [dispositivos gemelos](../iot-hub/iot-hub-devguide-device-twins.md) en Azure IoT Hub.

## <a name="security-module-twins"></a>Módulos de seguridad gemelos

Defender para IoT conserva un módulo gemelo de seguridad por cada dispositivo que haya en el servicio.
Este módulo contiene toda la información relevante relativa a la seguridad de cada dispositivo específico de la solución.
En un módulo de seguridad gemelo dedicado se conservan las propiedades de seguridad del dispositivo para que la comunicación sea más segura y para permitir unas actualizaciones y un mantenimiento que requiere menos recursos.

Vea [Creación de un módulo gemelo](quickstart-create-security-twin.md) y [Configurar agentes de seguridad](how-to-agent-configuration.md) para obtener información sobre cómo crear, personalizar y configurar el módulo gemelo. Vea [Descripción de módulos gemelos](../iot-hub/iot-hub-devguide-module-twins.md) para obtener más información sobre el concepto de módulo gemelo en IoT Hub.

## <a name="see-also"></a>Consulte también

- [Información general de Defender para IoT](overview.md)
- [Implementación de agentes de seguridad](how-to-deploy-agent.md)
- [Métodos de autenticación del agente de seguridad](concept-security-agent-authentication-methods.md)