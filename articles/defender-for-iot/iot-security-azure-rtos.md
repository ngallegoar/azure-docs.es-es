---
title: Información general del módulo de seguridad para Azure RTOS
description: Obtenga más información sobre la compatibilidad y la implementación del módulo de seguridad para Azure RTOS como parte de Azure Defender para IoT.
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
ms.date: 09/07/2020
ms.author: mlottner
ms.openlocfilehash: ec314a729914fee26d30165c9df1644bde3845aa
ms.sourcegitcommit: 83610f637914f09d2a87b98ae7a6ae92122a02f1
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/13/2020
ms.locfileid: "91995499"
---
# <a name="overview-defender-for-iot-security-module-for-azure-rtos-preview"></a>Introducción: Módulo de seguridad de Defender para IoT para Azure RTOS (versión preliminar)

El módulo de seguridad de Azure Defender para IoT proporciona una solución de seguridad completa para dispositivos que usan Azure RTOS. Proporciona cobertura para amenazas comunes y posibles actividades malintencionadas en dispositivos del sistema operativo en tiempo real (RTOS). Azure RTOS ahora se incluye con el módulo de seguridad de Azure IoT integrado.

![Visualización de Defender para IoT para Azure RTOS.](./media/architecture/azure-rtos-security-monitoring.png)


El módulo de seguridad para Azure RTOS ofrece las siguientes características:

- Detección de actividad de red malintencionada
- Línea de base del comportamiento de los dispositivos basada en alertas personalizadas
- Higiene mejorada de la seguridad de los dispositivos

## <a name="detect-malicious-network-activities"></a>Detección de actividades de red malintencionadas

Se supervisa la actividad de red entrante y saliente de cada dispositivo. Los protocolos admitidos son TCP, UDP e ICMP en IPv4 e IPv6. Defender para IoT inspecciona cada una de estas actividades de red en la fuente de inteligencia sobre amenazas de Microsoft. La fuente se actualiza en tiempo real con millones de indicadores de amenazas únicos recopilados en todo el mundo.

## <a name="device-behavior-baselining-based-on-custom-alerts"></a>Línea de base del comportamiento de los dispositivos basada en alertas personalizadas

La línea de base permite la agrupación en clústeres de dispositivos en grupos de seguridad y la definición del comportamiento esperado de cada grupo. Dado que los dispositivos IoT suelen estar diseñados para funcionar en escenarios bien definidos y limitados, es fácil crear una línea de base que defina su comportamiento esperado mediante un conjunto de parámetros. Cualquier desviación de la línea de base desencadena una alerta.

## <a name="improve-your-device-security-hygiene"></a>Mejora de la higiene de seguridad de los dispositivos

Al usar la infraestructura recomendada que proporciona Defender para IoT, puede adquirir conocimientos e información sobre los problemas de su entorno que afectan y dañan la postura de seguridad de los dispositivos. Una débil postura de seguridad de los dispositivos IoT puede permitir que los posibles ataques surtan efecto si se deja sin cambios. La seguridad siempre se mide por el eslabón más débil dentro de cualquier organización.

## <a name="get-started-protecting-azure-rtos-devices"></a>Introducción a la protección de dispositivos Azure RTOS

El módulo de seguridad para Azure RTOS se proporciona como una descarga gratuita para los dispositivos. El servicio en la nube Defender para IoT está disponible con una versión de prueba de 30 días por suscripción de Azure. Para comenzar, descargue el [módulo de seguridad para Azure RTOS](https://github.com/MicrosoftDocs/azure-docs/blob/master/articles/defender-for-iot/iot-security-azure-rtos.md). 

## <a name="next-steps"></a>Pasos siguientes

En este artículo, aprendió sobre el módulo de seguridad para Azure RTOS. Para más información sobre el módulo de seguridad y empezar a usarlo, consulte los siguientes artículos:

- [Conceptos del módulo de seguridad de IoT de Azure RTOS](concept-rtos-security-module.md)
- [Inicio rápido: Módulo de seguridad de IoT de Azure RTOS](quickstart-azure-rtos-security-module.md)
