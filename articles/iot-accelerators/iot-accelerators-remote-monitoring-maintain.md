---
title: 'Uso de alertas en la solución de supervisión remota: Azure | Microsoft Docs'
description: En este tutorial, se explica cómo puede utilizar las alertas para identificar y solucionar problemas relacionados con los dispositivos conectados al acelerador de soluciones de supervisión remota.
author: dominicbetts
manager: timlt
ms.author: dobett
ms.service: iot-accelerators
services: iot-accelerators
ms.date: 11/08/2018
ms.topic: tutorial
ms.custom: mvc
ms.openlocfilehash: 853fa2b80e04dd8d9225d023db8030fed044ed7f
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/24/2020
ms.locfileid: "73890921"
---
# <a name="tutorial-troubleshoot-and-fix-device-issues"></a>Tutorial: Solución y corrección de problemas de los dispositivos

En este tutorial, utilizará el acelerador de soluciones de supervisión remota para identificar y solucionar problemas relacionados con los dispositivos de IoT conectados. Utilizará las alertas del panel del acelerador de soluciones para identificar los problemas y después ejecutará trabajos remotos para resolverlos.

Contoso está probando un tipo nuevo de **prototipo** en el campo. Como operador de Contoso, durante las pruebas observa que el dispositivo **Prototipo** desencadena inesperadamente una alerta en el panel. Ahora, debe investigar el comportamiento de este dispositivo **prototipo** defectuoso y resolver el problema.

En este tutorial, hizo lo siguiente:

>[!div class="checklist"]
> * Investigación de una alerta procedente de un dispositivo
> * Resolución del problema relacionado con el dispositivo

Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de empezar.

[!INCLUDE [iot-accelerators-tutorial-prereqs](../../includes/iot-accelerators-tutorial-prereqs.md)]

## <a name="investigate-an-alert"></a>Investigación de una alerta

En la página **Panel** observará que hay alertas de temperatura inesperadas que provienen de la regla asociada con los dispositivos **Prototipo**:

[![Alertas que aparecen en el panel](./media/iot-accelerators-remote-monitoring-maintain/dashboardalarm-inline.png)](./media/iot-accelerators-remote-monitoring-maintain/dashboardalarm-expanded.png#lightbox)

Para investigar aún más el problema, elija la opción **Explorar alerta** junto a la alerta:

[![Exploración de la alerta desde el panel](./media/iot-accelerators-remote-monitoring-maintain/dashboardexplorealarm-inline.png)](./media/iot-accelerators-remote-monitoring-maintain/dashboardexplorealarm-expanded.png#lightbox)

Aparece la vista de detalles de la alerta:

* Cuándo se desencadenó la alerta
* La información de estado sobre los dispositivos asociados con la alerta
* Telemetría desde los dispositivos asociados con la alerta

[![Detalles de alertas](./media/iot-accelerators-remote-monitoring-maintain/maintenancealarmdetail-inline.png)](./media/iot-accelerators-remote-monitoring-maintain/maintenancealarmdetail-expanded.png#lightbox)

Para confirmar la alerta, seleccione todas las opciones de **Alert occurrences** (Repeticiones de alertas) y elija **Acknowledge** (Confirmación). Esta acción permite que otros operadores sepan que ha visto la alerta y que está trabajando en ella:

[![Confirmación de alertas](./media/iot-accelerators-remote-monitoring-maintain/maintenanceacknowledge-inline.png)](./media/iot-accelerators-remote-monitoring-maintain/maintenanceacknowledge-expanded.png#lightbox)

Al confirmar la alerta, el estado de la repetición cambia a **Confirmado**.

En la lista de dispositivos con alerta, puede ver el dispositivo **Prototipo** responsable de activar la alerta de temperatura:

[![Lista de los dispositivos que causan la alerta](./media/iot-accelerators-remote-monitoring-maintain/maintenanceresponsibledevice-inline.png)](./media/iot-accelerators-remote-monitoring-maintain/maintenanceresponsibledevice-expanded.png#lightbox)

## <a name="resolve-the-issue"></a>Resolución del problema

Para solucionar el problema con el dispositivo **prototipo**, debe llamar al método **DecreaseTemperature** del dispositivo.

Para realizar alguna acción en un dispositivo, selecciónelo en la lista de dispositivos con alerta y, luego, elija **Trabajos**. El modelo de dispositivo **Prototipo** admite seis métodos:

[![Vista de los métodos que admite el dispositivo](./media/iot-accelerators-remote-monitoring-maintain/maintenancemethods-inline.png)](./media/iot-accelerators-remote-monitoring-maintain/maintenancemethods-expanded.png#lightbox)

Elija **DecreaseTemperature** y establezca el nombre del trabajo en **DecreaseTemperature**. A continuación, haga clic en **Aplicar**:

[![Creación del trabajo para disminuir la temperatura](./media/iot-accelerators-remote-monitoring-maintain/maintenancecreatejob-inline.png)](./media/iot-accelerators-remote-monitoring-maintain/maintenancecreatejob-expanded.png#lightbox)

Para realizar un seguimiento del estado del trabajo, haga clic en **View job status** (Ver estado del trabajo). Use la vista **Trabajos** para realizar un seguimiento de todos los trabajos y las llamadas de método en la solución:

[![Supervisión del trabajo para disminuir la temperatura](./media/iot-accelerators-remote-monitoring-maintain/maintenancerunningjob-inline.png)](./media/iot-accelerators-remote-monitoring-maintain/maintenancerunningjob-expanded.png#lightbox)

Puede comprobar si la temperatura del dispositivo ha caído consultando los datos de telemetría de la página **Dashboard** (Panel):

[![Vista de la disminución de temperatura](./media/iot-accelerators-remote-monitoring-maintain/jobresult-inline.png)](./media/iot-accelerators-remote-monitoring-maintain/jobresult-expanded.png#lightbox)

[!INCLUDE [iot-accelerators-tutorial-cleanup](../../includes/iot-accelerators-tutorial-cleanup.md)]

## <a name="next-steps"></a>Pasos siguientes

En este tutorial, ha aprendido a utilizar alertas para identificar problemas con los dispositivos y a solucionarlos realizando las acciones oportunas. Para aprender a conectar un dispositivo real a un acelerador de soluciones, consulte los artículos de procedimientos.

Ahora que sabe cómo solucionar los problemas con los dispositivos, le sugerimos que aprenda a [conectar un dispositivo al acelerador de soluciones de supervisión remota](iot-accelerators-connecting-devices.md).
