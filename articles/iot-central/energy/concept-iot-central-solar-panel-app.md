---
title: 'Conceptos de arquitectura en Azure IoT Central: panel solar | Microsoft Docs'
description: En este artículo se presentan conceptos clave relacionados con la arquitectura de una aplicación de supervisión de paneles solares de Azure IoT Central.
author: op-ravi
ms.author: omravi
ms.date: 10/23/2019
ms.topic: overview
ms.service: iot-central
services: iot-central
manager: abjork
ms.openlocfilehash: 00ef6cf6d4149f139876cb0c2d845133ba00157c
ms.sourcegitcommit: 7dacbf3b9ae0652931762bd5c8192a1a3989e701
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/16/2020
ms.locfileid: "92127513"
---
# <a name="azure-iot-central---solar-panel-app-architecture"></a>Azure IoT Central: arquitectura de aplicaciones de panel solar




En este artículo se proporciona información general sobre la arquitectura de la plantilla de aplicaciones de supervisión de paneles solares. En el diagrama siguiente se muestra una arquitectura de uso común de aplicaciones de paneles solares en Azure que usa la plataforma de IoT Central.

> [!div class="mx-imgBorder"]
> ![arquitectura de medidores inteligentes](media/concept-iot-central-solar-panel/solar-panel-app-architecture.png)

Esta arquitectura consta de los siguientes componentes. Puede que algunas aplicaciones no requieran todos los componentes que se mencionan aquí.

## <a name="solar-panels-and-connectivity"></a>Paneles solares y conectividad 

Los paneles solares son una fuente significativa de energía renovable. En función del tipo de panel solar y de la configuración, puede conectarlo mediante puertas de enlace u otros dispositivos intermedios y sistemas de propiedad. Puede que necesite crear un dispositivo de puente de IoT Central para conectar los dispositivos que no pueden hacerlo directamente. El puente de dispositivo de IoT Central es una solución de código abierto y puede encontrar los detalles completos [aquí](../core/howto-build-iotc-device-bridge.md). 



## <a name="iot-central-platform"></a>Plataforma de IoT Central
Azure IoT Central es una plataforma que simplifica la compilación de su solución de IoT y permite reducir la carga y los costos de la administración, las operaciones y el desarrollo de IoT. Con IoT Central, puede conectar, supervisar y administrar fácilmente sus recursos de Internet de las cosas (IoT) a escala. Después de conectar los paneles solares a IoT Central, la plantilla de aplicación usa características integradas, como modelos de dispositivos, comandos y paneles. La plantilla de la aplicación también usa el almacenamiento de IoT Central para escenarios de ruta de acceso activa, como la supervisión, el análisis, las reglas y la visualización de datos de medición casi en tiempo real.


## <a name="extensibility-options-to-build-with-iot-central"></a>Opciones de extensibilidad para la compilación con IoT Central
La plataforma de IoT Central proporciona dos opciones de extensibilidad: Exportación de datos continua (CDE) y API. Los clientes y asociados pueden elegir entre estas opciones en función de la personalización de sus soluciones para necesidades específicas. Por ejemplo, uno de nuestros asociados configuró CDE con Azure Data Lake Storage (ADLS). Usa ADLS para la retención de datos a largo plazo y otros escenarios de almacenamiento de la ruta de acceso pasiva, como el procesamiento por lotes, la auditoría e informes. 

## <a name="next-steps"></a>Pasos siguientes

* Ahora que ya conoce la arquitectura, [cree una aplicación de panel solar de forma gratuita](https://apps.azureiotcentral.com/build/new/solar-panel-monitoring).
* Para más información acerca de IoT Central, consulte [Introducción a IoT Central](../index.yml).