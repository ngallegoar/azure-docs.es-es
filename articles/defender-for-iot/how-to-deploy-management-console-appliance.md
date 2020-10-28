---
title: Implementación de la consola de administración en Azure Defender para IoT
description: Obtenga información sobre cómo implementar la consola de administración en Azure Defender para IoT.
services: defender-for-iot
ms.service: defender-for-iot
documentationcenter: na
author: rkarlin
manager: rkarlin
editor: ''
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/14/2020
ms.author: rkarlin
ms.openlocfilehash: 6af943951b2dba0c4a11b08ad1bcd961cf1c0fc2
ms.sourcegitcommit: 30505c01d43ef71dac08138a960903c2b53f2499
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/15/2020
ms.locfileid: "92094193"
---
# <a name="deploy-the-management-console"></a>Implementación de la consola de administración
En este artículo se describe el procedimiento para implementar la consola de administración de Azure Defender para IoT en el entorno local.

## <a name="the-on-premises-management-console"></a>La consola de administración local

La consola de administración local proporciona una vista consolidada de todos los recursos de la red y ofrece visibilidad en tiempo real de las alertas e indicadores de riesgo principales de IoT y OT en todas sus instalaciones. Al estar integrada estrechamente con los runbooks y flujos de trabajo de SOC, facilita la priorización de las actividades de mitigación y la correlación entre sitios de las amenazas.

- Holístico: reduzca la complejidad con una única plataforma unificada para la administración de activos, la administración de riesgos y vulnerabilidades, y la supervisión de amenazas con respuesta a incidentes.

- Agregación y correlación: muestra, agrega y analiza los datos y las alertas recopilados en todos los sitios.

- Control de todos los sensores: configura y supervisa todos los sensores desde una ubicación única.

   ![Vista de la administración de sitios de Azure Defender para IoT](media/updates/image2.png)

## <a name="deploy-the-on-premises-management-console-appliance"></a>Implementación del dispositivo de consola de administración local

Para este proceso, debe adquirir su propio hardware e instalar el software. La solución se ejecuta en dispositivos certificados. Al comprar su aplicación certificada, consulte la [guía de especificaciones de hardware de Azure Defender para IoT](https://aka.ms/AzureDefenderforIoTBareMetalAppliance) como referencia.

Para información sobre la descarga de la imagen ISO y la instalación del software de sensor, consulte la [guía de instalación de Azure Defender para IoT](https://aka.ms/AzureDefenderforIoTInstallSensorISO).

**Para implementar la consola de administración local:**

1. Vaya a Microsoft Azure Defender para IoT.

2. Seleccione **consola de administración local** .

   ![Vista de la consola de administración local en Azure Defender para IoT](media/updates/image15.png)

3. Seleccione la versión 3.1 en el menú **Seleccionar versión** .

4. Seleccione **Descargar** y guarde el archivo.

5. Una vez instalado el software, lleve a cabo las tareas de configuración de la red. Consulte la [guía de configuración de la red de Azure Defender para IoT](https://aka.ms/AzureDefenderForIoTNetworkSetup) para obtener más información.

## <a name="next-steps"></a>Pasos siguientes

Para más información sobre las opciones de configuración, prosiga con la guía paso a paso para configurar el módulo.
> [!div class="nextstepaction"]
> [Guía paso a paso de configuración del módulo](./how-to-agent-configuration.md)
