---
title: 'Configuración de dispositivos en la solución de supervisión remota: Azure | Microsoft Docs'
description: En este tutorial se muestra cómo configurar los dispositivos conectados al acelerador de soluciones de supervisión remota.
author: dominicbetts
manager: timlt
ms.author: dobett
ms.service: iot-accelerators
services: iot-accelerators
ms.date: 03/08/2019
ms.topic: tutorial
ms.custom: mvc
ms.openlocfilehash: e4236952bd41c4955e337813ff6d706263b8ef47
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/24/2020
ms.locfileid: "73890897"
---
# <a name="tutorial-configure-devices-connected-to-your-monitoring-solution"></a>Tutorial: Configuración de dispositivos conectados a la solución de supervisión

En este tutorial, usará el acelerador de soluciones de supervisión remota para configurar y administrar los dispositivos de IoT conectados. Agregará un nuevo dispositivo al acelerador de soluciones y lo configurará.

Contoso solicitó maquinaria nueva para expandir una de sus instalaciones. Mientras espera la entrega de la maquinaria nueva, desea ejecutar una simulación para probar el comportamiento de la solución. Para ejecutar la simulación, agregue un nuevo dispositivo de motor simulado al acelerador de soluciones de supervisión remota y compruebe que este dispositivo simulado responde correctamente a las actualizaciones de configuración. Aunque este tutorial utiliza dispositivos simulados, un desarrollador de dispositivos puede implementar métodos directos en un [dispositivo real conectado al acelerador de soluciones de supervisión remota](iot-accelerators-connecting-devices.md).

En este tutorial, hizo lo siguiente:

>[!div class="checklist"]
> * Aprovisionar un dispositivo simulado.
> * Probar un dispositivo simulado.
> * Volver a configurar un dispositivo.
> * Organizar los dispositivos.

Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de empezar.

[!INCLUDE [iot-accelerators-tutorial-prereqs](../../includes/iot-accelerators-tutorial-prereqs.md)]

## <a name="add-a-simulated-device"></a>Adición de un dispositivo simulado

Vaya a la página **Device Explorer** en la solución y haga clic en **+ Nuevo dispositivo**:

[![Aprovisionamiento de un dispositivo simulado](./media/iot-accelerators-remote-monitoring-manage/devicesprovision-inline.png)](./media/iot-accelerators-remote-monitoring-manage/devicesprovision-expanded.png#lightbox)

En el panel **Nuevo dispositivo**, elija **Simulado**, deje el número de dispositivos que aprovisionar como **1**, elija el modelo de dispositivo **Faulty Engine** (Motor defectuoso) y, a continuación, elija **Aplicar** para crear el dispositivo simulado:

[![Aprovisionamiento de un dispositivo de motor simulado](./media/iot-accelerators-remote-monitoring-manage/devicesprovisionengine-inline.png)](./media/iot-accelerators-remote-monitoring-manage/devicesprovisionengine-expanded.png#lightbox)

## <a name="test-the-simulated-device"></a>Prueba del dispositivo simulado

Para probar que el dispositivo de motor simulado envía los valores de propiedad de informes y telemetría, selecciónelo en la lista de dispositivos de la página **Device Explorer**. La información sobre el motor se muestra en el panel **Detalles del dispositivo**:

[![Vista del nuevo dispositivo de motor simulado](./media/iot-accelerators-remote-monitoring-manage/devicesviewnew-inline.png)](./media/iot-accelerators-remote-monitoring-manage/devicesviewnew-expanded.png#lightbox)

En **Detalles del dispositivo**, compruebe que el dispositivo nuevo está enviando datos de telemetría. Para ver el flujo de telemetría de vibración desde el dispositivo, haga clic en **Vibración**:

[![Selección de un flujo de telemetría para observar](./media/iot-accelerators-remote-monitoring-manage/devicesvibration-inline.png)](./media/iot-accelerators-remote-monitoring-manage/devicesvibration-expanded.png#lightbox)

El panel **Detalles del dispositivo** muestra otra información sobre el dispositivo, como los valores de etiqueta, los métodos que admite y las propiedades de que informa.

Para ver los diagnósticos detallados, desplácese hacia abajo en el panel **Detalles del dispositivo** para ver la sección **Diagnósticos**.

## <a name="reconfigure-a-device"></a>Nueva configuración de un dispositivo

Para comprobar si puede actualizar las propiedades de configuración del motor, selecciónelo en la lista de dispositivos de la página **Device Explorer**. A continuación, haga clic en **Trabajos** y, a continuación, elija **Propiedades**. El panel de trabajos muestra los valores de propiedad del dispositivo seleccionado que se pueden actualizar:

[![Nueva configuración de un dispositivo](./media/iot-accelerators-remote-monitoring-manage/devicesreconfigure-inline.png)](./media/iot-accelerators-remote-monitoring-manage/devicesreconfigure-expanded.png#lightbox)

Para actualizar la ubicación del motor, establezca el nombre del trabajo en **UpdateEngineLocation**, establezca la longitud en **-122.15**, establezca la ubicación en **Factory 2**, establezca la latitud en **47.62** y haga clic en **Aplicar**:

[![Actualización de un valor de propiedad de dispositivo](./media/iot-accelerators-remote-monitoring-manage/devicesreconfigurephysical-inline.png)](./media/iot-accelerators-remote-monitoring-manage/devicesreconfigurephysical-expanded.png#lightbox)

Para realizar un seguimiento del estado del trabajo, haga clic en **View job status** (Ver estado del trabajo):

[![Actualización de un valor de propiedad de dispositivo](./media/iot-accelerators-remote-monitoring-manage/locationjobstatus-inline.png)](./media/iot-accelerators-remote-monitoring-manage/locationjobstatus-expanded.png#lightbox)

Una vez completado el trabajo, vaya a la página **Panel**. El dispositivo de motor se muestra en el mapa en su nueva ubicación:

[![Visualización de la ubicación del motor](./media/iot-accelerators-remote-monitoring-manage/enginelocation-inline.png)](./media/iot-accelerators-remote-monitoring-manage/enginelocation-expanded.png#lightbox)

## <a name="organize-your-devices"></a>Organización de los dispositivos

Para facilitar que los operadores organicen y administren los dispositivos, es conveniente etiquetarlos con el nombre del equipo correspondiente. Contoso tiene dos equipos distintos para las actividades de servicio de campo:

* El equipo de Vehículo inteligente administra los camiones y los dispositivos de creación de prototipos.
* El equipo de Edificio inteligente administra los refrigeradores, los ascensores y los motores.

Para mostrar todos los dispositivos, vaya a la página **Device Explorer** y elija el filtro **Todos los dispositivos**:

[![Mostrar todos los dispositivos](./media/iot-accelerators-remote-monitoring-manage/devicesalldevices-inline.png)](./media/iot-accelerators-remote-monitoring-manage/devicesalldevices-expanded.png#lightbox)

### <a name="add-tags"></a>Incorporación de etiquetas

Seleccione todos los dispositivos de **camiones** y de **creación de prototipos**. A continuación, haga clic en **Trabajos**.

En el panel **Trabajos**, seleccione **Etiqueta**, establezca el nombre del trabajo en **AddConnectedVehicleTag** y, a continuación, agregue una etiqueta de texto llamada **FieldService** con el valor **ConnectedVehicle**. A continuación, haga clic en **Aplicar**:

[![Incorporación de una etiqueta a los dispositivos de prototipo y camión](./media/iot-accelerators-remote-monitoring-manage/devicesaddtag-inline.png)](./media/iot-accelerators-remote-monitoring-manage/devicesaddtag-expanded.png#lightbox)

En la página de dispositivos, seleccione todos los dispositivos **Refrigerador**, **Ascensor** y **Motor**. A continuación, haga clic en **Trabajos**.

En el panel **Trabajos**, seleccione **Etiqueta**, establezca el nombre del trabajo en **AddSmartBuildingTag** y, a continuación, agregue una etiqueta de texto llamada **FieldService** con el valor **SmartBuilding**. A continuación, haga clic en **Aplicar**:

[![Incorporación de una etiqueta a los dispositivos de refrigerador, ascensor y motor](./media/iot-accelerators-remote-monitoring-manage/devicesaddtag2-inline.png)](./media/iot-accelerators-remote-monitoring-manage/devicesaddtag2-expanded.png#lightbox)

### <a name="create-filters"></a>Creación de filtros

Ahora puede usar los valores de etiqueta para crear filtros. En la página **Device Explorer**, haga clic en **Administrar grupos de dispositivos**:

[![Administrar grupos de dispositivos](./media/iot-accelerators-remote-monitoring-manage/devicesmanagefilters-inline.png)](./media/iot-accelerators-remote-monitoring-manage/devicesmanagefilters-expanded.png#lightbox)

Cree un filtro de texto que use el nombre de etiqueta **FieldService** y el valor **SmartBuilding** en la condición. Guarde el filtro como **Edificio inteligente**:

[![Creación del filtro de edificio inteligente](./media/iot-accelerators-remote-monitoring-manage/smartbuildingfilter-inline.png)](./media/iot-accelerators-remote-monitoring-manage/smartbuildingfilter-expanded.png#lightbox)

Cree un filtro de texto que use el nombre de etiqueta **FieldService** y el valor **ConnectedVehicle** en la condición. Guarde el filtro como **Vehículo conectado**.

[![Creación del filtro de vehículo conectado](./media/iot-accelerators-remote-monitoring-manage/connectedvehiclefilter-inline.png)](./media/iot-accelerators-remote-monitoring-manage/connectedvehiclefilter-expanded.png#lightbox)

Ahora el operador de Contoso puede consultar los dispositivos en función del equipo operativo:

[![Creación del filtro de vehículo conectado](./media/iot-accelerators-remote-monitoring-manage/filterinaction-inline.png)](./media/iot-accelerators-remote-monitoring-manage/filterinaction-expanded.png#lightbox)

[!INCLUDE [iot-accelerators-tutorial-cleanup](../../includes/iot-accelerators-tutorial-cleanup.md)]

## <a name="next-steps"></a>Pasos siguientes

En este tutorial se muestra cómo configurar y administrar los dispositivos conectados a un acelerador de soluciones de supervisión remota. Para aprender a usar el acelerador de soluciones para el análisis de causas principales en una alerta inesperada, continúe con el siguiente tutorial.

> [!div class="nextstepaction"]
> [Realización de un análisis de causas principales en una alerta](iot-accelerators-remote-monitoring-root-cause-analysis.md)
