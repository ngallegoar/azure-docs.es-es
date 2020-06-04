---
title: archivo de inclusión
description: archivo de inclusión
services: iot-suite
author: dominicbetts
ms.service: iot-suite
ms.topic: include
ms.date: 04/24/2018
ms.author: dobett
ms.custom: include file
ms.openlocfilehash: 9b9e28f18208674609d0842b0e3a54e3fc661c9f
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/28/2020
ms.locfileid: "67186749"
---
## <a name="view-device-telemetry"></a>Ver la telemetría de dispositivo

Puede ver la telemetría enviada desde el dispositivo en la página **Explorador de dispositivos** de la solución.

1. Seleccione el dispositivo que ha aprovisionado en la lista de dispositivos de la página **Explorador de dispositivos**. Un panel muestra información sobre el dispositivo incluyendo un trazado de su telemetría:

    ![Ver detalles del dispositivo](media/iot-suite-visualize-connecting/devicesdetail.png)

1. Elija **Presión** para cambiar la presentación de telemetría:

    ![Ver la telemetría de presión](media/iot-suite-visualize-connecting/devicespressure.png)

1. Para ver información de diagnóstico sobre el dispositivo, desplácese hacia abajo hasta **Diagnósticos**:

    ![Ver el diagnóstico de los dispositivos](media/iot-suite-visualize-connecting/devicesdiagnostics.png)

## <a name="act-on-your-device"></a>Actúe en su dispositivo

Para invocar métodos en los dispositivos, use la página **Explorador de dispositivos** de la solución de supervisión remota. Por ejemplo, en los dispositivos **Chiller** de la solución de supervisión remota, implemente un método **Reboot**.

1. Elija **Dispositivos** para ir hasta la página **Explorador de dispositivos** de la solución.

1. Seleccione el dispositivo que ha aprovisionado en la lista de dispositivos de la página **Explorador de dispositivos**:

    ![Selección del dispositivo real](media/iot-suite-visualize-connecting/devicesselect.png)

1. Para mostrar una lista de los métodos que se pueden llamar en el dispositivo, elija **Trabajos** y, luego, **Métodos**. Para programar un trabajo para ejecutarse en varios dispositivos, puede seleccionarlos en la lista. El panel **Trabajos** muestra los tipos de métodos comunes a todos los dispositivos seleccionados.

1. Elija **Arrancar**, establezca el nombre del trabajo en **RebootPhysicalChiller** y posteriormente elija **Aplicar**:

    ![Programación de la actualización de firmware](media/iot-suite-visualize-connecting/deviceschedule.png)

1. Aparece una secuencia de mensajes en la consola donde se ejecuta el código del dispositivo cuando el dispositivo controla el método.

> [!NOTE]
> Para realizar el seguimiento del estado del trabajo en la solución, elija **Ver estado de trabajo**.

## <a name="next-steps"></a>Pasos siguientes

En el artículo [Personalización de la solución preconfigurada de supervisión remota](../articles/iot-accelerators/iot-accelerators-remote-monitoring-customize.md) se describen algunas maneras de personalizar el acelerador de soluciones.