---
title: Arquitectura de referencia de la solución de supervisión de la calidad del agua creada con Azure IoT Central | Microsoft Docs
description: Conozca los conceptos de una solución de supervisión de la calidad del agua creada con Azure IoT Central.
author: miriambrus
ms.author: miriamb
ms.date: 10/23/2019
ms.topic: tutorial
ms.service: iot-central
services: iot-central
ms.openlocfilehash: 0c4b13c56a68205195bd5ad4b696d9e01786a8dd
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "77016551"
---
# <a name="water-quality-monitoring-reference-architecture"></a>Arquitectura de referencia de supervisión de calidad del agua 



Las soluciones de supervisión de la calidad del agua se pueden crear con la plantilla de aplicación de **Azure IoT Central** como una aplicación de IoT de inicio rápido. En este artículo se proporciona una guía de arquitectura de referencia de alto nivel sobre la creación de una solución de un extremo a otro. 


![Arquitectura de supervisión de la calidad del agua](./media/concepts-waterqualitymonitoring-architecture/concepts-waterqualitymonitoring-architecture1.png)

Conceptos:

1. Dispositivos y conectividad  
1. IoT Central 
2. Extensibilidad e integraciones
3. Aplicaciones empresariales

Echemos un vistazo a los componentes clave que generalmente desempeñan un papel en una solución de supervisión de la calidad del agua.

## <a name="devices-and-connectivity"></a>Dispositivos y conectividad 
En esta sección, nos referiremos a los dispositivos usados en la supervisión de la calidad del agua o la supervisión del consumo de agua generalmente como dispositivos de agua inteligentes. Los dispositivos de agua inteligentes pueden ser medidores de flujo, monitores de la calidad del agua, válvulas inteligentes, detectores de fugas, etc.

Los dispositivos que se usan en las soluciones inteligentes para el agua normalmente se conectan mediante redes de área extensa de bajo consumo (LPWAN) mediante un operador de red de terceros. Para estos tipos de dispositivos, puede aprovechar el [Puente de dispositivos de Azure IoT Central](https://docs.microsoft.com/azure/iot-central/core/howto-build-iotc-device-bridge) para enviar los datos del dispositivo a la aplicación de IoT en Azure IoT Central. Como alternativa, puede tener puertas de enlace de dispositivo que sean compatibles con IP y puedan conectarse directamente a IoT Central.

## <a name="iot-central"></a>IoT Central 
Azure IoT Central es una plataforma de aplicaciones de IoT que le ayuda a empezar a trabajar con su solución IoT rápidamente. Puede utilizar su marca, personalizar e integrar la solución con servicios de terceros.
Después de conectar los dispositivos de agua inteligentes a IoT Central, obtendrá el control, la supervisión y las alertas de los dispositivos, la interfaz de usuario con RBAC integrado, paneles de información configurables y opciones de extensibilidad. 

## <a name="extensibility-and-integrations"></a>Extensibilidad e integraciones 
Puede ampliar su aplicación de IoT en IoT Central y, si lo desea:
* Transformar e integrar los datos de IoT para el análisis avanzado, como el entrenamiento de modelos de aprendizaje automático, mediante la exportación continua de datos de la aplicación de IoT Central.
* Automatizar flujos de trabajo en otros sistemas al desencadenar acciones mediante Microsoft Flow o webhooks desde la aplicación de IoT Central.
* Acceder mediante programación a la aplicación IoT de IoT Central mediante las API de IoT Central.

## <a name="business-applications"></a>Aplicaciones empresariales 
Los datos de IoT se pueden usar para potenciar una variedad de aplicaciones empresariales en una instalación de aguas. Para obtener información sobre cómo conectar la aplicación de supervisión de la calidad del agua de IoT Central con los servicios de campo, siga el artículo sobre [cómo integrar con los servicios de campo de Dynamics 365](./how-to-configure-connected-field-services.md). 


## <a name="next-steps"></a>Pasos siguientes
* Aprenda a [crear una aplicación de IoT Central de supervisión de la calidad del agua](./tutorial-water-quality-monitoring.md).
* Más información acerca de las [plantillas de IoT Central Government](./overview-iot-central-government.md)
* Para más información acerca de IoT Central, consulte [Introducción a IoT Central](https://docs.microsoft.com/azure/iot-central/core/overview-iot-central).

