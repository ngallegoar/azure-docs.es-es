---
title: Introducción a los agentes de seguridad
description: Introducción a la configuración, implementación y uso de los agentes del servicio de seguridad de Azure Security Center para IoT en dispositivos de IoT.
services: asc-for-iot
ms.service: asc-for-iot
documentationcenter: na
author: mlottner
manager: rkarlin
editor: ''
ms.assetid: 55c8d3b6-3126-4246-8d07-ef88fe5ea84f
ms.subservice: asc-for-iot
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/23/2019
ms.author: mlottner
ms.openlocfilehash: 0d4d21db5185e4564666e526b3edb5ca6d3451e0
ms.sourcegitcommit: 7e04a51363de29322de08d2c5024d97506937a60
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/14/2020
ms.locfileid: "81310645"
---
# <a name="get-started-with-azure-security-center-for-iot-device-security-agents"></a>Introducción a los agentes de seguridad de dispositivo de Azure Security Center para IoT

Los agentes de seguridad de Azure Security Center para IoT ofrecen funcionalidades de seguridad mejoradas, como la supervisión de conexiones remotas, aplicaciones activas, eventos de inicio de sesión y procedimientos recomendados de configuración del sistema operativo. Tome el control de la protección contra amenazas del campo de dispositivo y la postura de seguridad con un único servicio.

Se proporciona arquitectura de referencia para los agentes de seguridad de Windows y Linux tanto en C# como en C.

Los agentes de Azure Security Center para IoT controlan la recopilación de eventos sin procesar procedentes del sistema operativo del dispositivo, la agregación de eventos para reducir el costo y la configuración mediante un módulo gemelo del dispositivo. Los mensajes de seguridad se envían mediante IoT Hub a los servicios de análisis de Azure Security Center para IoT.

Use el flujo de trabajo a continuación para implementar y probar los agentes de Azure Security Center para IoT:

1. [Habilite el servicio Azure Security Center para IoT en su instancia de IoT Hub](quickstart-onboard-iot-hub.md)
1. Si IoT Hub no tiene ningún dispositivo registrado, [registre uno nuevo](https://docs.microsoft.com/azure/iot-accelerators/quickstart-device-simulation-deploy).
1. [Cree un módulo de seguridad azureiotsecurity](quickstart-create-security-twin.md) para sus dispositivos.
1. Para instalar el agente en un dispositivo simulado de Azure en lugar de hacerlo en un dispositivo real, [prepare una máquina virtual de Azure nueva](https://docs.microsoft.com/azure/virtual-machines/linux/quick-create-portal) en una zona de disponibilidad.
1. [Implemente un agente de seguridad de Azure Security Center para IoT](how-to-deploy-linux-cs.md) en el dispositivo IoT o en una máquina virtual nueva.
1. Siga las instrucciones de [trigger_events](https://aka.ms/iot-security-github-trigger-events) para llevar a cabo una simulación inofensiva de un ataque.
1. Compruebe las alertas de Azure Security Center para IoT en respuesta al ataque simulado del paso anterior. No las compruebe hasta que hayan transcurrido cinco minutos desde la ejecución del script.
1. Explore las [alertas](concept-security-alerts.md), las [recomendaciones](concept-recommendations.md) y [perfeccione su uso de Log Analytics](how-to-security-data-access.md) mediante IoT Hub.

## <a name="next-steps"></a>Pasos siguientes

- Configurar la [solución](quickstart-configure-your-solution.md)
- [Creación de módulos de seguridad](quickstart-create-security-twin.md)
- Configurar [alertas personalizadas](quickstart-create-custom-alerts.md)
- [Implementación de un agente de seguridad](how-to-deploy-agent.md)
