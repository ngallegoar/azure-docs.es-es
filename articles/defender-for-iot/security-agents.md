---
title: Introducción a los agentes de seguridad
description: Introducción a la configuración, implementación y uso de los agentes del servicio de seguridad de Azure Defender para IoT en dispositivos de IoT.
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
ms.date: 07/23/2019
ms.author: mlottner
ms.openlocfilehash: e3fe3e910134080f6620023f539913454670d093
ms.sourcegitcommit: 9eda79ea41c60d58a4ceab63d424d6866b38b82d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/30/2020
ms.locfileid: "96339921"
---
# <a name="get-started-with-azure-defender-for-iot-device-security-agents"></a>Introducción a los agentes de seguridad de dispositivo de Azure Defender para IoT

Los agentes de seguridad de Defender para IoT ofrecen funcionalidades de seguridad mejoradas, como la supervisión de conexiones remotas, aplicaciones activas, eventos de inicio de sesión y procedimientos recomendados de configuración del sistema operativo. Tome el control de la protección contra amenazas del campo de dispositivo y la postura de seguridad con un único servicio.

Se proporciona arquitectura de referencia para los agentes de seguridad de Windows y Linux tanto en C# como en C.

Los agentes de Defender para IoT controlan la recopilación de eventos sin procesar procedentes del sistema operativo del dispositivo, la agregación de eventos para reducir el costo y la configuración mediante un módulo gemelo del dispositivo. Los mensajes de seguridad se envían a través del IoT Hub a los servicios de análisis de Defender para IoT.

Use el flujo de trabajo a continuación para implementar y probar los agentes de seguridad de Defender para IoT:

1. [Habilitación del servicio Defender para IoT en IoT Hub](quickstart-onboard-iot-hub.md)
1. Si IoT Hub no tiene ningún dispositivo registrado, [registre uno nuevo](../iot-accelerators/quickstart-device-simulation-deploy.md).
1. [Cree un módulo de seguridad azureiotsecurity](quickstart-create-security-twin.md) para sus dispositivos.
1. Para instalar el agente en un dispositivo simulado de Azure en lugar de hacerlo en un dispositivo real, [prepare una máquina virtual de Azure nueva](../virtual-machines/linux/quick-create-portal.md) en una zona de disponibilidad.
1. [Implemente un agente de seguridad de Defender para IoT](how-to-deploy-linux-cs.md) en el dispositivo IoT o en una máquina virtual nueva.
1. Siga las instrucciones de [trigger_events](https://aka.ms/iot-security-github-trigger-events) para llevar a cabo una simulación inofensiva de un ataque.
1. Compruebe las alertas de Defender para IoT en respuesta al ataque simulado del paso anterior. No las compruebe hasta que hayan transcurrido cinco minutos desde la ejecución del script.
1. Explore las [alertas](concept-security-alerts.md), las [recomendaciones](concept-recommendations.md) y [perfeccione su uso de Log Analytics](how-to-security-data-access.md) mediante IoT Hub.

## <a name="next-steps"></a>Pasos siguientes

- Configurar la [solución](quickstart-configure-your-solution.md)
- [Creación de módulos de seguridad](quickstart-create-security-twin.md)
- Configurar [alertas personalizadas](quickstart-create-custom-alerts.md)
- [Implementación de un agente de seguridad](how-to-deploy-agent.md)