---
title: Opciones de las soluciones de Internet de las cosas (IoT) de Azure
description: Guía para elegir entre los enfoques de servicios de plataforma o plataforma de aplicaciones administradas para crear una solución de IoT. El primero de estos enfoques utiliza servicios como IoT Hub y Digital Twins como bloques de creación. El segundo usa IoT Central para empezar a trabajar rápidamente.
author: dominicbetts
ms.service: iot-fundamentals
services: iot-fundamentals
ms.topic: overview
ms.date: 02/05/2020
ms.author: dobett
ms.openlocfilehash: 046116addf643d6c0eb0faf2f821b6ef78d440c6
ms.sourcegitcommit: eb6bef1274b9e6390c7a77ff69bf6a3b94e827fc
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/05/2020
ms.locfileid: "77050632"
---
# <a name="choose-the-right-iot-solution"></a>Elección de la solución de IoT adecuada

Para compilar una solución de IoT para su empresa, lo habitual es que elija el enfoque de *servicios de plataforma* o el de *plataforma de aplicaciones administradas*.

Los servicios de la plataforma proporcionan los bloques de creación necesarios para obtener aplicaciones de IoT personalizadas y flexibles. Tiene más opciones entre las que elegir y programar tanto al conectar dispositivos como al ingerir, almacenar y analizar los datos. Los servicios de la plataforma de IoT de Azure incluyen los productos Azure IoT Hub y Azure Digital Twins.

Las plataformas de aplicaciones administradas permiten empezar a compilar aplicaciones de forma más rápida que los servicios de plataforma, ya que reducen el número de decisiones necesarias para lograr resultados. La plataforma de aplicaciones administradas se encarga de la mayoría de los elementos de la solución, por lo que el usuario puede centrarse en agregar conocimientos del sector y escalar y conectar dispositivos. Azure IoT Central es una plataforma de aplicaciones administradas.

Para elegir entre estos dos enfoques, es preciso tener en cuenta lo siguiente:

- Cómo se desea administrar la solución.
- Qué nivel de personalización y control se desea sobre la solución.
- Qué estructura de precios se desea.

## <a name="management"></a>Administración

¿Dónde desea gastar los recursos y el tiempo de administración del sistema? 

- Elija el enfoque de servicios de plataforma para tener un control total sobre los servicios subyacentes de la solución. Por ejemplo, desea:

    - Administrar el escalado y la protección de los servicios de forma que se ajusten a sus necesidades.
    - Hacer uso de los conocimientos del personal interno o de los asociados para incorporar dispositivos y aprovisionar servicios.

- Elija el enfoque de plataforma de aplicaciones administradas si lo que desea es aprovechar una plataforma que controla la escala, seguridad y administración de las aplicaciones y dispositivos IoT.

## <a name="control"></a>Control

¿Qué elementos de la solución desea personalizar?

- Elija el método de servicios de plataforma si lo que desea es una personalización y control totales sobre la arquitectura de la solución.

- Elija el enfoque de plataforma de aplicaciones administradas si desea personalizar la marca, los paneles, los roles de usuario, los dispositivos y la telemetría, pero no desea controlar la sobrecarga que supone la administración del sistema de IoT subyacente.

## <a name="pricing"></a>Precios

¿Qué estructura de precios se ajusta mejor a sus necesidades?

- Elija el enfoque de servicios de plataforma si desea ajustar los servicios y controlar los costos generales.

- Elija el enfoque de la plataforma de aplicaciones administradas si desea una estructura de precios sencilla y predecible.

## <a name="summary"></a>Resumen

El enfoque de servicios de plataforma es el más adecuado para todas aquellas empresas con una solución en la nube y conocimientos de dispositivos que deseen:

- Ajustar los servicios en la solución.
- Tener mucho control sobre los servicios en la solución.
- Personalizar completamente la solución.

El enfoque de plataforma de aplicaciones administradas es adecuado para aquellas empresas que:

- No desean dedicar muchos recursos al diseño, desarrollo y administración del sistema.
- Desean una estructura de precios predecible.
- Desean tener funcionalidades de administración.

## <a name="next-steps"></a>Pasos siguientes

Para obtener una explicación más completa de los distintos servicios y plataformas, así como de su uso, consulte [Tecnologías y servicios de Azure para crear soluciones de IoT](iot-services-and-technologies.md).

Para más información sobre los principales atributos de las soluciones de IoT que funcionan perfectamente, consulte el documento [Ocho atributos de soluciones de IoT que han resultado un éxito](https://aka.ms/8attributes).

Para ver un análisis detallado de la arquitectura de IoT, consulte [Microsoft Azure IoT Reference Architecture](https://aka.ms/iotrefarchitecture) (Arquitectura de referencia de IoT de Microsoft Azure).
