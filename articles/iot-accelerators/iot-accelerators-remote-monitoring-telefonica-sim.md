---
title: Integración de datos de la SIM en la solución de supervisión remota (Azure) | Microsoft Azure
description: En este artículo se describe cómo integrar los datos de la SIM de Telefónica en la solución de supervisión remota.
author: hegate
manager: ''
ms.author: hegate
ms.service: iot-accelerators
services: iot-accelerators
ms.date: 05/15/2018
ms.topic: conceptual
ms.openlocfilehash: b07e21131d9560a49d99644525835ac5ee3bac9e
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "61442246"
---
# <a name="integrate-sim-data-in-the-remote-monitoring-solution"></a>Integración de datos de la SIM en la solución de supervisión remota

A menudo, los dispositivos IoT se conectan a la nube con una tarjeta SIM que les permite enviar flujos de datos desde cualquier lugar. La solución de supervisión remota de Azure IoT permite la integración de los datos de la conectividad administrada de IoT para que los operadores también puedan realizar un seguimiento del mantenimiento del dispositivo mediante los datos proporcionados por la SIM de IoT.

La supervisión remota proporciona integración lista para usar con conectividad de IoT de Telefónica, lo que permite a los clientes usar la plataforma de conectividad de IoT para sincronizar los datos de conectividad de las SIM de dispositivo con las soluciones. Esta solución puede ampliarse para ofrecer compatibilidad con otros proveedores de conectividad de IoT mediante el [repositorio](https://github.com/Azure/azure-iot-pcs-remote-monitoring-dotnet) de GitHub.

En este tutorial, aprenderá a:

* Integración de datos de la SIM de IoT de Telefónica en la solución de supervisión remota
* Ver la telemetría en tiempo real
* Visualización de datos de la SIM

## <a name="telefnica-iot-integration-setup"></a>Configuración de la integración de IoT de Telefónica

### <a name="prerequisites"></a>Prerequisites

Esta característica de supervisión remota adicional se encuentra actualmente en versión preliminar. Para sincronizar los datos de conectividad en la solución de supervisión remota de Azure, siga estos pasos:

1. Rellene una solicitud en el [sitio de Telefónica](https://iot.telefonica.com/contact), seleccione la opción **Supervisión remota de Azure** e incluya sus datos de contacto.
2. Telefónica activará la cuenta.
3. Si aún no es cliente de Telefónica y quiere disfrutar de esta ventaja o de otros servicios preparados para la nube con conectividad de IoT, visite el [sitio de Telefónica](https://iot.telefonica.com/) y seleccione la opción **Conectividad**.

### <a name="telefnica-sim-setup"></a>Configuración de la SIM de Telefónica
La asociación entre la SIM de Telefónica y el identificador del dispositivo gemelo de Azure se basa en la propiedad del "alias" de la SIM de IoT de Telefónica. 

Navegue al [Portal de la plataforma de conectividad de IoT de Telefónica](https://m2m-movistar-es.telefonica.com/) > Inventario de SIM > Seleccione su SIM y actualice el "alias" de cada SIM con el identificador del dispositivo gemelo que quiera. Esta tarea también puede realizarse de modo masivo (consulte los manuales de usuario de la plataforma de conectividad de IoT de Telefónica).

Esta tarea también puede realizarse de modo masivo (consulte los manuales de usuario de la plataforma de conectividad de IoT de Telefónica).

![Actualización de Telefónica](./media/iot-accelerators-remote-monitoring-telefonica-sim/telefonica_site.png)

Para conectar el dispositivo a la solución de supervisión remota, puede seguir estos tutoriales con [C](iot-accelerators-connecting-devices-linux.md) o [Nodo](iot-accelerators-connecting-devices-node.md). 

## <a name="view-device-telemetry-and-sim-properties"></a>Visualización de las propiedades de la SIM y de la telemetría del dispositivo

Una vez que la cuenta de Telefónica está correctamente configurada y el dispositivo está conectado, puede visualizar los detalles del dispositivo y los datos de la SIM.

Se publican los siguientes parámetros de conectividad:

* ICCID
* IP
* Presencia de red
* Estado de SIM
* Ubicación basada en la red
* Tráfico de datos consumido

![Panel](./media/iot-accelerators-remote-monitoring-telefonica-sim/dashboard.png)

## <a name="next-steps"></a>Pasos siguientes

Ahora que dispone de información general sobre cómo integrar los datos de la SIM en la solución de supervisión remota de Azure IoT, a continuación se recomiendan los pasos siguientes para los aceleradores de soluciones:

* [Operar con la solución de supervisión remota de Azure IoT](quickstart-remote-monitoring-deploy.md)
* [Supervisión avanzada](iot-accelerators-remote-monitoring-monitor.md)
* [Administración de dispositivos](iot-accelerators-remote-monitoring-manage.md)
* [Solución de problemas de dispositivo](iot-accelerators-remote-monitoring-maintain.md)

