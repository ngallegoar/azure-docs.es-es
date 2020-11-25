---
title: Arquitectura de referencia de la solución de supervisión del consumo de agua creada con Azure IoT Central | Microsoft Docs
description: Conozca los conceptos de una solución de supervisión del consumo de agua creada con Azure IoT Central.
author: miriambrus
ms.author: miriamb
ms.date: 10/23/2019
ms.topic: tutorial
ms.service: iot-central
services: iot-central
ms.openlocfilehash: df402064f2aa26e29ef72c0f73f8f5aa761506e2
ms.sourcegitcommit: 642988f1ac17cfd7a72ad38ce38ed7a5c2926b6c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/18/2020
ms.locfileid: "94873566"
---
# <a name="water-consumption-monitoring-reference-architecture"></a>Arquitectura de referencia de supervisión del consumo de agua 



Las soluciones de supervisión del consumo de agua se pueden crear con la **plantilla de aplicación de Azure IoT Central** como una aplicación de IoT de inicio rápido. En este artículo se proporciona una guía de arquitectura de referencia de alto nivel sobre la creación de una solución de un extremo a otro. 

![Arquitectura de supervisión del consumo de agua](./media/concepts-waterconsumptionmonitoring-architecture/concepts-waterconsumptionmonitoring-architecture1.png)

Conceptos:

1. Dispositivos y conectividad  
1. IoT Central 
2. Extensibilidad e integraciones
3. Aplicaciones empresariales

Echemos un vistazo a los componentes clave que generalmente desempeñan un papel en una solución de supervisión del consumo de agua.

## <a name="devices-and-connectivity"></a>Dispositivos y conectividad 
En esta sección se hace referencia a los dispositivos usados en las soluciones inteligentes de control de agua, como aquellos para la supervisión de la calidad del agua o la supervisión del consumo de agua, generalmente como dispositivos inteligentes de supervisión del agua. Los dispositivos de agua inteligentes pueden ser medidores de flujo, monitores de la calidad del agua, válvulas inteligentes, detectores de fugas, etc.

Los dispositivos que se usan en las soluciones inteligentes para el agua normalmente se conectan mediante redes de área extensa de bajo consumo (LPWAN) mediante un operador de red de terceros. Para estos tipos de dispositivos, puede usar el [Puente de dispositivos de Azure IoT Central](../core/howto-build-iotc-device-bridge.md) para enviar los datos del dispositivo a la aplicación de IoT en Azure IoT Central. Como alternativa, puede tener puertas de enlace de dispositivo que sean compatibles con IP y puedan conectarse directamente a IoT Central.

## <a name="iot-central"></a>IoT Central 
Azure IoT Central es una plataforma de aplicaciones de IoT que le ayuda a empezar a trabajar con su solución IoT rápidamente. Puede utilizar su marca, personalizar e integrar la solución con servicios de terceros.
Después de conectar los dispositivos de agua inteligentes a IoT Central, obtendrá el control, la supervisión y las alertas de los dispositivos, la interfaz de usuario con RBAC integrado, paneles de información configurables y opciones de extensibilidad. 


## <a name="extensibility-and-integrations"></a>Extensibilidad e integraciones
Puede ampliar su aplicación de IoT en IoT Central y, si lo desea:
* Transformar e integrar los datos de IoT para el análisis avanzado, como el entrenamiento de modelos de aprendizaje automático, mediante la exportación continua de datos de la aplicación de IoT Central.
* Automatizar flujos de trabajo en otros sistemas al desencadenar acciones mediante Power Automate o webhooks desde la aplicación de IoT Central.
* Acceder mediante programación a la aplicación IoT de IoT Central mediante las API de IoT Central.

## <a name="business-applications"></a>Aplicaciones empresariales 
Los datos de IoT se pueden usar para potenciar diversos tipos de aplicaciones empresariales en una instalación de aguas. Para obtener información sobre cómo conectar la aplicación de supervisión del consumo de agua de IoT Central con los servicios de campo, siga el tutorial sobre [cómo integrar con los servicios de campo de Dynamics 365](./how-to-configure-connected-field-services.md). 


## <a name="next-steps"></a>Pasos siguientes
* Obtenga información sobre cómo [crear una aplicación de IoT Central para el consumo de agua](./tutorial-water-consumption-monitoring.md).
* Más información acerca de las [plantillas de IoT Central Government](./overview-iot-central-government.md)
* Para más información acerca de IoT Central, consulte [Introducción a IoT Central](../core/overview-iot-central.md).
