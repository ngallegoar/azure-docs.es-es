---
title: Introducción a los aceleradores de soluciones de Azure IoT en Azure | Microsoft Docs
description: Información acerca de los aceleradores de soluciones de IoT de Azure. Los aceleradores de soluciones de IoT son elementos completos y preparados para implementar soluciones de IoT.
author: dominicbetts
ms.author: dobett
ms.date: 03/09/2019
ms.topic: overview
ms.custom: mvc
ms.service: iot-accelerators
services: iot-accelerators
manager: timlt
ms.openlocfilehash: a8314fed43c8282c536fc6e95e6c3aa877408b09
ms.sourcegitcommit: f5580dd1d1799de15646e195f0120b9f9255617b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/29/2020
ms.locfileid: "91542502"
---
# <a name="what-are-azure-iot-solution-accelerators"></a>¿Qué son los aceleradores de soluciones de Azure IoT?

Una solución IoT basada en la nube normalmente usa código personalizado y servicios en la nube para administrar la conectividad de los dispositivos, el procesamiento y el análisis de los datos y su presentación.

Los aceleradores de soluciones IoT son soluciones IoT completas y listas para implementar escenarios comunes de IoT. Los escenarios incluyen supervisión remota, fábrica conectada, mantenimiento predictivo y simulación de dispositivo. Al implementar un acelerador de soluciones, la implementación incluye todos los servicios basados en la nube necesarios, junto con el código de aplicación requerido.

Los aceleradores de soluciones son puntos de partida para sus propias soluciones de IoT. El código fuente para todos los aceleradores de soluciones es código abierto y está disponible en GitHub. Le recomendamos que descargue y personalice los aceleradores de soluciones para cumplir sus requisitos.

También puede utilizarlos como herramientas de aprendizaje antes de crear una solución de IoT personalizada desde cero. Implementan prácticas probadas para soluciones de IoT basadas en la nube de modo que pueda realizar su seguimiento.

El código de la aplicación de cada acelerador de soluciones incluye una aplicación web que le permite administrar dicho acelerador.

## <a name="supported-iot-scenarios"></a>Escenarios de IoT admitidos

Actualmente, hay cuatro aceleradores de soluciones disponibles para implementar:

### <a name="remote-monitoring"></a>Supervisión remota

Use el [acelerador de soluciones de supervisión remota](iot-accelerators-remote-monitoring-sample-walkthrough.md) para recopilar datos de telemetría procedentes de dispositivos remotos y para controlarlos. Algunos dispositivos de ejemplo serían los sistemas de refrigeración instalados en las instalaciones de los clientes o las válvulas instaladas en estaciones de bombeo remotas.

Puede usar el panel de supervisión remota para ver los datos de telemetría de los dispositivos conectados, aprovisionar nuevos dispositivos o actualizar el firmware de los dispositivos conectados:

[![Panel de la solución de supervisión remota](./media/about-iot-accelerators/rm-dashboard-inline.png)](./media/about-iot-accelerators/rm-dashboard-expanded.png#lightbox)

### <a name="connected-factory"></a>Fábrica conectada

Use el [acelerador de soluciones Factoría conectada](iot-accelerators-connected-factory-features.md) para recopilar datos de telemetría de recursos industriales con una interfaz de [OPC Unified Architecture](https://opcfoundation.org/about/opc-technologies/opc-ua/) y para controlarlos. Los recursos industriales podrían incluir estaciones de prueba y ensamblado en una línea de producción de una fábrica.

Puede utilizar el panel de fábrica conectada para supervisar y administrar sus dispositivos industriales:

:::image type="content" source="./media/about-iot-accelerators/cf-dashboard-inline.png" alt-text="Captura de pantalla que muestra el panel de la solución de factoría conectada." lightbox="./media/about-iot-accelerators/cf-dashboard-expanded.png":::

### <a name="predictive-maintenance"></a>Mantenimiento predictivo

Use el [acelerador de soluciones de mantenimiento predictivo](iot-accelerators-predictive-walkthrough.md) para predecir cuándo se espera que un dispositivo remoto sufra un error, de modo que pueda realizar tareas de mantenimiento antes de que ocurra. Este acelerador de soluciones utiliza algoritmos de aprendizaje automático para predecir los errores de la telemetría de los dispositivos. Algunos dispositivos de ejemplo podrían ser los motores de un avión o los ascensores.

Puede utilizar el panel de mantenimiento predictivo para ver el análisis de mantenimiento predictivo:

:::image type="content" source="./media/about-iot-accelerators/pm-dashboard-inline.png" alt-text="Captura de pantalla que muestra el panel de la solución de factoría conectada." lightbox="./media/about-iot-accelerators/pm-dashboard-expanded.png":::

### <a name="device-simulation"></a>Simulación de dispositivo

Use el [acelerador de soluciones Simulación de dispositivos](iot-accelerators-device-simulation-overview.md) para ejecutar dispositivos simulados que generan datos de telemetría realistas. Puede utilizar este acelerador de soluciones para probar el comportamiento de los demás aceleradores de soluciones o para probar sus propias soluciones personalizadas de IoT.

Puede usar la aplicación web de simulación de dispositivos para configurar y ejecutar simulaciones:

[![Panel de la solución de fábrica conectada](./media/about-iot-accelerators/ds-dashboard-inline.png)](./media/about-iot-accelerators/ds-dashboard-expanded.png#lightbox)

## <a name="design-principles"></a>Principios de diseño

Todos los aceleradores de soluciones siguen los mismos principios de diseño y objetivos. Están diseñados para ser:

* **Escalables**, lo que le permite conectar y administrar millones de dispositivos conectados.
* **Extensibles**, lo que le permite personalizarlos para satisfacer sus requisitos.
* **Comprensibles**, lo que le permite comprender cómo funcionan y cómo se implementan.
* **Modulares**, lo que le permite intercambiar servicios alternativos.
* **Seguros**, combinando la seguridad de Azure con características de seguridad integradas de dispositivos y de conectividad.

## <a name="architectures-and-languages"></a>Arquitecturas y lenguajes

Los aceleradores de soluciones originales se escribieron con .NET usando una arquitectura Model-View-Controller (MVC). Microsoft está actualizando los aceleradores de soluciones con una nueva arquitectura basada en microservicios. En la siguiente tabla se muestra el estado actual de los aceleradores de soluciones con vínculos a los repositorios de GitHub:

| Acelerador de soluciones   | Architecture  | Lenguajes     |
| ---------------------- | ------------- | ------------- |
| Supervisión remota      | Microservicios | [Java](https://github.com/Azure/azure-iot-pcs-remote-monitoring-java) y [.NET](https://github.com/Azure/azure-iot-pcs-remote-monitoring-dotnet) |
| Mantenimiento predictivo | MVC           | [.NET](https://github.com/Azure/azure-iot-predictive-maintenance)          |
| Fábrica conectada      | MVC           | [.NET](https://github.com/Azure/azure-iot-connected-factory)          |
| Simulación de dispositivo      | Microservicios | [.NET](https://github.com/Azure/device-simulation-dotnet)          |

Para más información sobre la arquitectura de microservicios, consulte [Introducción a la arquitectura de referencia de Azure IoT](https://docs.microsoft.com/azure/architecture/reference-architectures/iot/).

## <a name="deployment-options"></a>Opciones de implementación

Puede implementar los aceleradores de soluciones desde el sitio [Aceleradores de soluciones de Microsoft Azure IoT](https://www.azureiotsolutions.com/Accelerators#) o mediante la línea de comandos.

Puede implementar el acelerador de la solución de supervisión remota en las siguientes configuraciones:

* **Estándar:** implementación ampliada de la infraestructura para desarrollar una implementación de producción. Azure Container Service implementa los microservicios en varias máquinas virtuales de Azure. Kubernetes orquesta los contenedores de Docker que hospedan los microservicios individuales.
* **Básica:** versión de menor costo para ver una demostración o probar una implementación. Todos los microservicios se implementan en una máquina virtual de Azure.
* **Local:** implementación de la máquina local para desarrollo y pruebas. Este enfoque implementa los microservicios en un contenedor Docker local y se conecta a IoT Hub, Azure Cosmos DB y los servicios de almacenamiento de Azure en la nube.

El costo que supone ejecutar un acelerador de soluciones es una combinación del [costo de ejecutar los servicios de Azure subyacentes](https://azure.microsoft.com/pricing). Consulte los detalles de los servicios de Azure utilizados al elegir las opciones de implementación.

## <a name="next-steps"></a>Pasos siguientes

Para probar uno de los aceleradores de soluciones de IoT, consulte las siguientes guías de inicio rápido:

* [Prueba de una solución de supervisión remota](quickstart-remote-monitoring-deploy.md)
* [Prueba de una solución de fábrica conectada](quickstart-connected-factory-deploy.md)
* [Prueba de una solución de mantenimiento predictivo](quickstart-predictive-maintenance-deploy.md)
* [Prueba de una solución de simulación de dispositivos](quickstart-device-simulation-deploy.md)
