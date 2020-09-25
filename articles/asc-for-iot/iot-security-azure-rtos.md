---
title: Introducción al módulo de seguridad para Azure RTOS
description: Aprenda más sobre la compatibilidad y la implementación del módulo de seguridad para Azure RTOS, parte del servicio Azure Security Center para IoT.
services: asc-for-iot
ms.service: asc-for-iot
documentationcenter: na
author: mlottner
manager: rkarlin
editor: ''
ms.subservice: asc-for-iot
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/07/2020
ms.author: mlottner
ms.openlocfilehash: bcf98f503b0f0c8bf76435d9884297d5f53d65e6
ms.sourcegitcommit: 7f62a228b1eeab399d5a300ddb5305f09b80ee14
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/08/2020
ms.locfileid: "89514482"
---
# <a name="overview-security-module-for-azure-rtos-preview"></a>Introducción: Módulo de seguridad para Azure RTOS (versión preliminar)

El módulo de seguridad RTOS de Azure Security Center para IoT proporciona una solución de seguridad integral para dispositivos Azure RTOS. Azure RTOS ahora se incluye con el módulo de seguridad de Azure IoT integrado y proporciona cobertura para amenazas comunes y posibles actividades malintencionadas en los dispositivos del sistema operativo en tiempo real. 

![Azure RTOS de Azure Security Center para IoT](./media/architecture/azure-rtos-security-monitoring.png)


El módulo de seguridad para Azure RTOS ofrece las siguientes características: 
- Detección de actividad de red malintencionada
- Línea de base del comportamiento de los dispositivos basada en alertas personalizadas
- Mejora de la higiene de seguridad de los dispositivos

### <a name="detection-of-malicious-network-activities"></a>Detección de actividades de red malintencionadas

Se supervisa la actividad de red entrante y saliente de cada dispositivo (protocolos admitidos: TCP, UDP, ICMP en IPv4 e IPv6). Azure Security Center para IoT inspecciona cada una de estas actividades de red en la fuente de inteligencia sobre amenazas de Microsoft. La fuente se actualiza en tiempo real con millones de indicadores de amenazas únicos recopilados en todo el mundo. 

### <a name="device-behavior-baselining-based-on-custom-alerts"></a>Línea de base del comportamiento de los dispositivos basada en alertas personalizadas

La línea de base permite la agrupación en clústeres de dispositivos en grupos de seguridad y la definición del comportamiento esperado de cada grupo. Dado que los dispositivos IoT suelen estar diseñados para funcionar en escenarios bien definidos y limitados, es fácil crear una línea de base que defina su comportamiento esperado mediante un conjunto de parámetros. Cualquier desviación de la línea de base desencadena una alerta. 

### <a name="improve-your-device-security-hygiene"></a>Mejora de la higiene de seguridad de los dispositivos

Al aprovechar la infraestructura recomendada que proporciona Azure Security Center para IoT, adquiere conocimientos e información sobre los problemas de su entorno que afectan y dañan la postura de seguridad de los dispositivos. Una posición de seguridad deficiente con respecto a los dispositivos IoT puede permitir el éxito de posibles ataques si se deja como está, ya que la seguridad siempre se mide por el vínculo más débil dentro de cualquier organización. 

## <a name="get-started-protecting-azure-rtos-devices"></a>Introducción a la protección de dispositivos Azure RTOS

El módulo de seguridad para Azure RTOS se proporciona como una descarga gratuita para los dispositivos. El servicio en la nube Azure Security Center para IoT está disponible con una versión de prueba de 30 días por suscripción de Azure. Para comenzar, descargue el [módulo de seguridad para Azure RTOS](https://github.com/azure-rtos/iot-security-module-preview). 


## <a name="next-steps"></a>Pasos siguientes

En este artículo, aprendió sobre el servicio del módulo de seguridad para Azure RTOS. Para más información sobre el módulo de seguridad y empezar a usarlo, consulte los siguientes artículos:

- [Conceptos del módulo de seguridad de IoT de Azure RTOS](concept-rtos-security-module.md)
- [Inicio rápido: Módulo de seguridad de IoT de Azure RTOS](quickstart-azure-rtos-security-module.md)


