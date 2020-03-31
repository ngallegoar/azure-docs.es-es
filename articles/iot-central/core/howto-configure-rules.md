---
title: Configuración de reglas y acciones en Azure IoT Central | Microsoft Docs
description: En este artículo paso a paso se muestra cómo puede, como generador, configurar las reglas y las acciones basadas en la telemetría en la aplicación de Azure IoT Central.
author: vavilla
ms.author: vavilla
ms.date: 11/27/2019
ms.topic: how-to
ms.service: iot-central
services: iot-central
manager: philmea
ms.openlocfilehash: 509f9557a8128df12353ad02a7c7db02b7b42631
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "80158487"
---
# <a name="configure-rules"></a>Configuración de reglas



*Este artículo se aplica a los administradores, operadores y compiladores.*

Las reglas de IoT Central sirven como herramienta de respuesta personalizable que se desencadena en eventos supervisados activamente desde dispositivos conectados. En las secciones siguientes se describe cómo se evalúan las reglas.

## <a name="select-target-devices"></a>Seleccionar dispositivos de destino

Use la sección de dispositivos de destino para seleccionar el tipo de dispositivos que se aplicará a esta regla. Asimismo, los filtros le permiten restringir aún más los dispositivos que deben incluirse. Tenga en cuenta que los filtros usan propiedades en la plantilla de dispositivos para así poder filtrar el conjunto de dispositivos. Los filtros no desencadenan una acción. En la siguiente captura de pantalla, los dispositivos de destino son el tipo de plantilla de dispositivo **Frigorífico**. El filtro indica que la regla solo debe incluir **Frigoríficos** en los que la propiedad **Estado de fabricación** sea igual a **Washington**.

![Condiciones](media/howto-configure-rules/filters.png)

## <a name="use-multiple-conditions"></a>Uso de varias condiciones

Las reglas se activan según las condiciones. Actualmente, cuando se agregan varias condiciones a una regla, se agrupan lógicamente mediante AND. En otras palabras, se deben cumplir todas las condiciones para que la regla se evalúe como "true".  

En la siguiente captura de pantalla, las condiciones comprueban si la temperatura es mayor que 70 &deg;F y la humedad es menor que 10. Cuando ambas instrucciones son "true", la regla se evalúa como "true" y se desencadena una acción.

![Condiciones](media/howto-configure-rules/conditions.png)

## <a name="use-aggregate-windowing"></a>Uso de la opción para agregar funciones de ventana

Las reglas evalúan ventanas de tiempo de agregado como, como las ventanas de saltos de tamaño constante. En la captura de pantalla siguiente, la ventana de tiempo es de cinco minutos. Cada cinco minutos, la regla se evalúa en función los últimos cinco minutos de datos. Los datos solo se evalúan una vez en la ventana a la que correspondan.

![Ventana de saltos de tamaño constante](media/howto-configure-rules/tumbling-window.png)

## <a name="use-rules-with-iot-edge-modules"></a>Usar reglas con módulos de IoT Edge

Una restricción se aplica a las reglas que se aplican a los módulos de IoT Edge. Las reglas en la telemetría de módulos diferentes no se evalúan como reglas válidas. Intente lo siguiente como ejemplo: La primera condición de la regla se encuentra en la telemetría de la temperatura del módulo A. La segunda condición de la regla se encuentra en la telemetría de la humedad en el módulo B. Dado que las dos condiciones provienen de módulos diferentes, se trata de un conjunto no válido de condiciones. Esta regla no es válida y producirá un error al intentar guardarla.

## <a name="next-steps"></a>Pasos siguientes

Ahora que ha aprendido a configurar una regla en una aplicación de Azure IoT Central puede:

> [!div class="nextstepaction"]
> [Analizar los datos sobre la marcha](howto-create-analytics.md)
