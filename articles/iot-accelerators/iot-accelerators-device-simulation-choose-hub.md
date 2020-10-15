---
title: Uso de un centro de IoT existente con la solución Simulación de dispositivo en Azure | Microsoft Docs
description: En este artículo se describe cómo configurar un acelerador de soluciones Simulación de dispositivo para usar una instancia de IoT Hub existente.
author: dominicbetts
manager: timlt
ms.author: dobett
ms.service: iot-accelerators
services: iot-accelerators
ms.date: 10/25/2018
ms.topic: conceptual
ms.openlocfilehash: 1f89e23d7bb279e7cce5c104060cc7898517f8b7
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "73889192"
---
# <a name="use-an-existing-iot-hub-with-the-device-simulation-solution-accelerator"></a>Uso de una instancia de IoT Hub existente con el acelerador de soluciones Simulación de dispositivo

Cuando implemente Simulación del dispositivo, también puede implementar un centro de IoT para utilizarlo en la simulación. Esta opción implementa un [centro de IoT del nivel S2 con una sola unidad de escalado](../iot-hub/iot-hub-scaling.md). Aunque implemente este centro de IoT opcional, podrá seguir utilizando otro centro de IoT como destino en la ejecución de la simulación.

Si decide no implementar el centro de IoT opcional, tendrá que utilizar su propio centro con cualquier simulación que ejecute.

Si no dispone de ninguna instancia de IoT, siempre puede crear una nueva desde [Azure Portal](https://portal.azure.com).

Si desea utilizar un centro de IoT existente, necesita una cadena de conexión para la directiva de acceso compartido **iothubowner**. Puede obtener la cadena de conexión de [Azure Portal](https://portal.azure.com):

1. En la página de configuración del centro en el portal, haga clic en **Directivas de acceso compartido**.

1. Haga clic en **iothubowner**.

1. Copie la cadena de conexión principal o la secundaria.

[![Obtención de la cadena de conexión](./media/iot-accelerators-device-simulation-choose-hub/connectionstring-inline.png)](./media/iot-accelerators-device-simulation-choose-hub/connectionstring-expanded.png#lightbox)

Utilice la cadena de conexión que copió al configurar la simulación:

![Configuración de la simulación](./media/iot-accelerators-device-simulation-choose-hub/configuresimulation.png)

### <a name="next-steps"></a>Pasos siguientes

En esta guía, ha aprendido cómo usar una instancia de IoT Hub existente en una simulación. Es posible que ahora desee aprender a [crear un modelo de dispositivo avanzado](iot-accelerators-device-simulation-advanced-device.md) para utilizarlo en una simulación.
