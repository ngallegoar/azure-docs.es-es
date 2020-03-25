---
title: 'Supervisión de dispositivos en la solución de supervisión remota: Azure | Microsoft Docs'
description: En este tutorial aprenderá a supervisar los dispositivos de IoT mediante el acelerador de solución de supervisión remota.
author: dominicbetts
manager: timlt
ms.author: dobett
ms.service: iot-accelerators
services: iot-accelerators
ms.date: 03/08/2019
ms.topic: tutorial
ms.custom: mvc
ms.openlocfilehash: 400a71b11fde210b889d938041e88c5ebe73c1dc
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/24/2020
ms.locfileid: "73890864"
---
# <a name="tutorial-monitor-your-iot-devices"></a>Tutorial: Supervisión de los dispositivos de IoT

En este tutorial, usará el acelerador de soluciones de supervisión remota para supervisar los dispositivos de IoT conectados. Utilice el panel de soluciones para ver la telemetría, la información del dispositivo, las alertas y los KPI.

El tutorial utiliza dos dispositivos de camión simulados que envían telemetría de ubicación, velocidad y temperatura de la carga. Los camiones son administrados por una organización denominada Contoso y están conectados al acelerador de soluciones de supervisión remota. Como operador de Contoso, debe supervisar la ubicación y el comportamiento de uno de los camiones (truck-02) sobre el terreno.

En este tutorial, hizo lo siguiente:

>[!div class="checklist"]
> * Filtrar los dispositivos en el panel
> * Ver la telemetría en tiempo real
> * Ver los detalles de dispositivo
> * Ver las alertas de los dispositivos
> * Ver los KPI del sistema

Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de empezar.

[!INCLUDE [iot-accelerators-tutorial-prereqs](../../includes/iot-accelerators-tutorial-prereqs.md)]

## <a name="choose-the-devices-to-display"></a>Elección de los dispositivos que se van a mostrar

Para seleccionar los dispositivos conectados que se muestran en la página **Panel**, use los filtros. Para mostrar solo los dispositivos **Camión**, elija el filtro **Camiones** integrado en el menú desplegable de filtros:

[![Filtro de los camiones en el panel](./media/iot-accelerators-remote-monitoring-monitor/dashboardtruckfilter-inline.png)](./media/iot-accelerators-remote-monitoring-monitor/dashboardtruckfilter-expanded.png#lightbox)

Cuando aplica un filtro, solo los dispositivos que coinciden con las condiciones de filtro aparecen en el mapa y en el panel de telemetría. Puede ver que hay dos camiones conectados al acelerador de soluciones, incluido truck-02:

[![Solo los camiones se muestran en el mapa](./media/iot-accelerators-remote-monitoring-monitor/dashboardtruckmap-inline.png)](./media/iot-accelerators-remote-monitoring-monitor/dashboardtruckmap-expanded.png#lightbox)

Para crear, editar y eliminar filtros, haga clic en **Manage device groups** (Administrar grupos de dispositivos).

## <a name="view-real-time-telemetry"></a>Ver la telemetría en tiempo real

El acelerador de la solución traza los datos de telemetría en tiempo real en el gráfico de la página **Panel**. La parte superior del gráfico de telemetría muestra los tipos de telemetría disponibles para los dispositivos (incluido truck-02) seleccionados con el filtro actual. De forma predeterminada, el gráfico muestra la latitud de los camiones y truck-02 parece estar estacionado:

[![Tipos de la telemetría del camión](./media/iot-accelerators-remote-monitoring-monitor/dashboardtelemetryview-inline.png)](./media/iot-accelerators-remote-monitoring-monitor/dashboardtelemetryview-expanded.png#lightbox)

Para ver la telemetría de temperatura de los camiones, haga clic en **Temperatura**. Puede ver cómo ha variado la temperatura de truck-02 durante la última hora:

[![Trazado de la telemetría de temperatura de los camiones](./media/iot-accelerators-remote-monitoring-monitor/dashboardselecttelemetry-inline.png)](./media/iot-accelerators-remote-monitoring-monitor/dashboardselecttelemetry-expanded.png#lightbox)

## <a name="view-the-map"></a>Visualización del mapa

El mapa muestra información sobre los camiones simulados seleccionados con el filtro actual. Puede hacer zoom y desplazar el mapa para mostrar las ubicaciones con mayor o menor detalle. El color del icono de un dispositivo que aparece en el mapa indica si hay alguna **alerta** (azul oscuro) o **advertencia** (rojo) activa para el dispositivo. A la izquierda del mapa, se muestra un resumen del número de **alertas** y **advertencias**.

Para ver los detalles de truck-02, desplácese lateralmente en el mapa y haga zoom para ubicar el dispositivo y, luego, selecciónelo en el mapa. A continuación, haga clic en la etiqueta de dispositivo para abrir el panel **Detalles del dispositivo**. Estos detalles incluyen:

* Valores recientes de telemetría
* Métodos que admite el dispositivo
* Propiedades del dispositivo

[![Vista de los detalles del dispositivo en el panel](./media/iot-accelerators-remote-monitoring-monitor/dashboarddevicedetail-inline.png)](./media/iot-accelerators-remote-monitoring-monitor/dashboarddevicedetail-expanded.png#lightbox)

## <a name="view-alerts"></a>Visualización de alertas

El panel **Alertas** muestra información detallada sobre las alertas más recientes de los dispositivos. Las alertas de truck-02 indican una temperatura de la carga superior a la normal:

[![Vista de las alertas de dispositivo en el panel](./media/iot-accelerators-remote-monitoring-monitor/dashboardsystemalarms-inline.png)](./media/iot-accelerators-remote-monitoring-monitor/dashboardsystemalarms-expanded.png#lightbox)

Puede usar un filtro para ajustar el intervalo de tiempo de las alertas recientes. De manera predeterminada, el panel muestra las alertas de la última hora.

## <a name="view-the-system-kpis"></a>Ver los KPI del sistema

La página **Panel** muestra los KPI del sistema calculados por el acelerador de soluciones en el panel **Análisis**:

[![KPI del panel](./media/iot-accelerators-remote-monitoring-monitor/dashboardkpis-inline.png)](./media/iot-accelerators-remote-monitoring-monitor/dashboardkpis-expanded.png#lightbox)

El panel muestra tres KPI para las alertas seleccionados por los filtros de dispositivo y el intervalo de tiempo actuales:

* El número de alertas activas para las reglas que han desencadenado la mayoría de las alertas.
* La proporción de alertas por tipo de dispositivo.
* El porcentaje de alertas que son críticas.

Para truck-02, todas las alertas son advertencias de una temperatura de la carga superior a la normal.

Los mismos filtros que establecen el intervalo de tiempo para las alertas y controlan qué dispositivos se muestran, determinan cómo se agregan los KPI. De manera predeterminada, el panel muestra los KPI agregados durante la última hora.

[!INCLUDE [iot-accelerators-tutorial-cleanup](../../includes/iot-accelerators-tutorial-cleanup.md)]

## <a name="next-steps"></a>Pasos siguientes

En este tutorial se ha demostrado cómo usar la página **Panel** en el acelerador de soluciones de supervisión remota para filtrar y supervisar los camiones simulados. Para obtener información sobre cómo usar el acelerador de soluciones para detectar problemas con los dispositivos conectados, continúe con el siguiente tutorial.

> [!div class="nextstepaction"]
> [Detección de problemas de dispositivos conectados a la solución de supervisión](iot-accelerators-remote-monitoring-automate.md)