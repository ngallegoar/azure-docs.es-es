---
title: 'Preguntas frecuentes sobre el acelerador de soluciones de supervisión remota: Azure | Microsoft Docs'
description: Este artículo responde a las preguntas más frecuentes sobre los aceleradores de la solución de supervisión remota.
author: dominicbetts
manager: timlt
ms.service: iot-accelerators
services: iot-accelerators
ms.topic: conceptual
ms.date: 02/15/2018
ms.author: dobett
ms.openlocfilehash: c410ac7f41adb623a7198320a1edced097778569
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "73826249"
---
# <a name="frequently-asked-questions-for-remote-monitoring-solution-accelerator"></a>Preguntas frecuentes sobre el acelerador de la solución de supervisión remota

Consulte, además, las [preguntas más frecuentes](iot-accelerators-faq.md) generales.

### <a name="how-much-does-it-cost-to-provision-the-new-remote-monitoring-solution"></a>¿Cuánto cuesta aprovisionar la nueva solución de supervisión remota?

El nuevo acelerador de la solución ofrece dos opciones de implementación:

* Una opción *básica* diseñada para desarrolladores que buscan el menor costo de desarrollo o para clientes que desean crear una demostración o una prueba de concepto.
* Una opción *estándar* diseñada para empresas que desean implementar una infraestructura para entornos de producción.

### <a name="how-can-i-ensure-i-keep-my-costs-down-while-i-develop-my-solution"></a>¿Cómo me aseguro de mantener mis costos mientras desarrollo la solución?

Además de proporcionar dos implementaciones diferenciadas, la nueva solución de supervisión remota tiene una configuración para habilitar o deshabilitar todos los dispositivos simulados a petición. Deshabilitar la simulación reduce la ingestión de datos en la solución y, por lo tanto, el costo total de los datos.

### <a name="what-is-the-difference-between-the-basic-and-standard-deployment-options-how-do-i-decide-between-the-two-deployment-options"></a>¿Cuál es la diferencia entre los niveles básico y estándar de las opciones de implementación estándar? ¿Cómo decido entre las dos opciones de implementación?

Cada opción de implementación responde a necesidades distintas. La implementación básica está diseñada para empezar a trabajar y desarrollar pequeños proyectos piloto y pruebas de concepto. Proporciona una arquitectura simplificada con los recursos mínimos necesarios y un costo menor. La implementación estándar está diseñada para crear y personalizar una solución para entornos de producción y dispone de los elementos necesarios para materializarla. En cuanto a la confiabilidad y el escalado, los microservicios de aplicación se crean como contenedores de Docker y se implementan con un orquestador (Kubernetes, de forma predeterminada). El orquestador es responsable de la implementación, el escalado y la administración de la aplicación. Debe elegir una opción en función de sus necesidades actuales. Puede usar una u otra, o bien una combinación de ambas, según la fase del proyecto.

### <a name="how-do-i-configure-a-dynamic-map-on-the-dashboard"></a>¿Cómo configuro un mapa dinámico en el panel?

Para obtener más información, vea [Upgrade map key to see devices on a dynamic map](https://github.com/Azure/azure-iot-pcs-remote-monitoring-dotnet/wiki/Developer-Reference-Guide#upgrade-map-key-to-see-devices-on-a-dynamic-map) (Actualización de la clave de mapa para ver dispositivos en un mapa dinámico).

### <a name="where-can-i-find-information-about-the-previous-version-of-the-remote-monitoring-solution"></a>¿Dónde puedo encontrar información acerca de la versión anterior de la solución de supervisión remota?

La versión anterior del acelerador de la solución de supervisión remota se conoce como la solución preconfigurada de supervisión remota de IoT Suite. Puede encontrar la documentación archivada en [https://docs.microsoft.com/previous-versions/azure/iot-suite/](https://docs.microsoft.com/previous-versions/azure/iot-suite/).

### <a name="next-steps"></a>Pasos siguientes

También puede explorar algunas de las demás características y funcionalidades de los aceleradores de soluciones de IoT:

* [Exploración de las funcionalidades del acelerador de soluciones de supervisión remota](quickstart-remote-monitoring-deploy.md)
* [Introducción al acelerador de la solución de mantenimiento predictivo](iot-accelerators-predictive-overview.md)
* [Implementación del acelerador de soluciones de Connected Factory](quickstart-connected-factory-deploy.md)
* [Seguridad de Internet de las cosas desde el principio](/azure/iot-fundamentals/iot-security-ground-up)
