---
title: Arquitectura de la solución
description: Obtenga información sobre el flujo de información en el servicio Azure Security Center para IoT.
services: asc-for-iot
ms.service: asc-for-iot
documentationcenter: na
author: mlottner
manager: rkarlin
editor: ''
ms.assetid: 2cf6a49b-5d35-491f-abc3-63ec24eb4bc2
ms.subservice: asc-for-iot
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/23/2019
ms.author: mlottner
ms.openlocfilehash: 4cc7d1982555f058a4ea23f7d8a6fdc2d83e484d
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/02/2020
ms.locfileid: "81311749"
---
# <a name="azure-security-center-for-iot-architecture"></a>Arquitectura de Azure Security Center para IoT

En este artículo se explica la arquitectura del sistema funcional de la solución Azure Security Center para IoT.

## <a name="azure-security-center-for-iot-components"></a>Componentes de Azure Security Center para IoT

Azure Security Center para IoT consta de los siguientes componentes:

- Integración de IoT Hub
- Agentes de dispositivos (opcional)
- SDK de Send Security Message
- Canalización de Analytics

### <a name="azure-security-center-for-iot-workflows"></a>Flujos de trabajo de Azure Security Center para IoT

Azure Security Center para IoT funciona en uno de dos flujos de trabajo de características: Integrado y mejorado

### <a name="built-in"></a>Integrada

En el modo **Integrado**, Azure Security Center para IoT se habilita al elegir la activación de la opción **Seguridad** en IoT Hub. Con la oferta de alertas, recomendaciones y supervisión en tiempo real, el modo integrado proporciona una seguridad sin precedentes y una visibilidad de los dispositivos en un solo paso. El modo de integración no requiere la instalación del agente en ningún dispositivo y usa el análisis avanzado en las actividades registradas para analizar y proteger el dispositivo de campo.

### <a name="enhanced"></a>Mejorada

En el modo **mejorado**, después de activar la opción **Seguridad** en IOT Hub e instalar los agentes de dispositivos de Azure Security Center para IoT en los dispositivos, los agentes recopilan, agregan y analizan eventos de seguridad sin procesar de los dispositivos. Los eventos de seguridad sin procesar pueden incluir las conexiones de IP, la creación de procesos, los inicios de sesión de los usuarios y otra información relacionada con la seguridad. Los agentes de dispositivos de Azure Security Center para IoT controlan la agregación de eventos para ayudar a evitar una alta tasa de rendimiento de la red. Los agentes son altamente personalizables, lo que le permite usarlos para tareas específicas, como enviar solo información importante a los SLA más rápidos, o para agregar información de seguridad exhaustiva y contexto en segmentos más grandes, lo que evita mayores costos de servicio.

![Arquitectura de Azure Security Center para IoT](./media/architecture/azure-iot-security-architecture.png)

Los agentes de dispositivos y otra aplicaciones usan el **SDK de Send Security Message de Azure** para enviar información de seguridad a Azure IoT Hub. IoT Hub recoge esta información y la reenvía al servicio Azure Security Center para IoT.

Una vez que el servicio Azure Security Center para IoT está habilitado, además de los datos reenviados, IoT Hub envía también todos los datos internos para que Azure Security Center para IoT los analice. Estos datos incluyen los registros de operaciones de dispositivos a la nube, las identidades de los dispositivos la y configuración del Hub. Toda esta información ayuda a crear la canalización de análisis de Azure Security Center para IoT.

La canalización de análisis de Azure Security Center para IoT también recibe flujos adicionales de inteligencia de amenazas desde varios orígenes dentro de Microsoft y asociados de Microsoft. La canalización de análisis completa de Azure Security Center para IoT funciona con cada configuración del cliente realizada en el servicio (como alertas personalizadas y uso del SDK de Send Security Message).

Con la canalización de análisis, Azure Security Center para IoT combina todos los flujos de información para generar alertas y recomendaciones accionables. La canalización contiene reglas personalizadas creadas tanto por investigadores de seguridad como por expertos, así como modelos de Machine Learning en busca de desviaciones del comportamiento estándar del dispositivo y análisis de riesgos.

Las recomendaciones y alertas de Azure Security Center para IoT (salida de la canalización de análisis) se escriben en el área de trabajo de Log Analytics de cada cliente. Incluir los eventos sin procesar en el área de trabajo, así como las alertas y recomendaciones, permite investigaciones y consultas de profundización con los detalles exactos de las actividades sospechosas detectadas.

## <a name="next-steps"></a>Pasos siguientes

En este artículo, ha aprendido sobre la arquitectura y el flujo de trabajo básicos de la solución Azure Security Center para IoT. Para más información sobre los requisitos previos, cómo empezar a trabajar y habilitar la solución de seguridad en IoT Hub, consulte los artículos siguientes:

- [Requisitos previos del servicio](service-prerequisites.md)
- [Introducción](getting-started.md)
- [Configuración de una solución](quickstart-configure-your-solution.md)
- [Habilitar la seguridad en IoT Hub](quickstart-onboard-iot-hub.md)
- [Preguntas más frecuentes de Azure Security Center para IoT](resources-frequently-asked-questions.md)
- [Alertas de seguridad de Azure Security Center para IoT](concept-security-alerts.md)
