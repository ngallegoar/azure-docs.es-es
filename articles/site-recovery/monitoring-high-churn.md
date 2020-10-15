---
title: Supervisión de patrones de renovación en máquinas virtuales
description: Aprenda a supervisar los patrones de renovación en máquinas virtuales con la protección de Azure Site Recovery
author: Sharmistha-Rai
manager: gaggupta
ms.topic: how-to
ms.date: 09/09/2020
ms.author: sharrai
ms.openlocfilehash: 51f45b5c0b8a2729f64eb376e06875719474cbfc
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "89663380"
---
# <a name="monitoring-churn-patterns-on-virtual-machines"></a>Supervisión de patrones de renovación en máquinas virtuales

En este artículo se proporciona información general sobre las distintas herramientas que se pueden usar para supervisar los patrones de renovación en una máquina virtual. Mediante el uso de las herramientas adecuadas, es fácil averiguar exactamente qué aplicación está causando una renovación elevada y después llevar a cabo acciones adicionales para esa aplicación.

## <a name="for-azure-virtual-machines-windows-or-linux"></a>Para máquinas virtuales de Azure (Windows o Linux)

Si el equipo está hospedado en Azure y usa un disco administrado o no administrado para el almacenamiento, puede realizar fácilmente un seguimiento del rendimiento por medio de las métricas del disco. Gracias a ello, puede tener información de primera mano y hacer la selección correcta del disco para adaptarse al patrón de uso de la aplicación. También puede usarlo para crear alertas, diagnósticos y automatización de compilaciones. [Más información](https://azure.microsoft.com/blog/per-disk-metrics-managed-disks/).

Una vez que haya protegido las máquinas con Azure Site Recovery, puede supervisarlas mediante registros de Azure Monitor y Log Analytics. [Más información](https://docs.microsoft.com/azure/site-recovery/monitor-log-analytics).

También hay algunas herramientas específicas del sistema operativo que puede usar.

## <a name="for-windows-machines"></a>Para máquinas Windows

En caso de que tenga una máquina, ya sea local o no, que ejecute el sistema operativo Windows, tiene a su disposición algunas herramientas más.

Además de comprobar el uso de disco en el administrador de tareas, consulte siempre el **monitor de recursos** y el **monitor de rendimiento**. Estas herramientas ya están presentes en las máquinas Windows.

### <a name="resource-monitor"></a>Monitor de recursos

El **monitor de recursos** muestra información sobre el uso de recursos de hardware y software en tiempo real. Para ejecutar el monitor de recursos en una máquina Windows, siga estos pasos:

1. Presione Win + R y escriba _resmon_.
1. Una vez que se abre la ventana de resmon (es decir, el monitor de recursos), cambie a la pestaña Disco. Obtendrá la siguiente vista:

    ![Pestaña Disco del monitor de recursos](./media/monitoring-high-churn/resmon-disk-tab.png)

1. Esta pestaña debe supervisarse continuamente durante algún tiempo para obtener una imagen clara. En el ejemplo anterior, vemos que _wmiprv.exe_ tiene una alta tasa de renovación.

Una vez que haya identificado las aplicaciones que causan una gran actividad en el equipo, puede llevar a cabo las acciones necesarias para abordar la actividad relacionada con esas aplicaciones.

### <a name="performance-monitor"></a>Monitor de rendimiento

El **monitor de rendimiento** supervisa varias actividades de un equipo, como el uso de la CPU o la memoria. Para ejecutar el monitor de rendimiento en una máquina Windows, siga estos pasos:

1. Presione Win + R y escriba _perfmon_.
1. Una vez que se abre la ventana perfmon (es decir, el monitor de rendimiento), se muestra la siguiente vista:

    ![Paso 1 del monitor de rendimiento](./media/monitoring-high-churn/perfmon-step1.png)

1. Expanda la carpeta **Herramientas de supervisión** a la derecha y haga clic en Monitor de rendimiento. Se abrirá la vista siguiente, que le proporcionará información en tiempo real sobre el rendimiento actual:

    ![Paso 2 del monitor de rendimiento](./media/monitoring-high-churn/perfmon-step1.png)

1. Este gráfico está supervisando actualmente un solo monitor, es decir, el "% de tiempo de procesador", como se indica en la tabla que se encuentra debajo del gráfico. Para agregar más elementos para la supervisión, haga clic en **"+"** sobre la herramienta.
1. A continuación se muestra el aspecto visual del monitor de rendimiento una vez que se agregan más contadores:

    ![Paso 3 del monitor de rendimiento](./media/monitoring-high-churn/perfmon-step3.png)

Obtenga más información sobre del monitor de rendimiento [aquí](https://docs.microsoft.com/dynamics365/business-central/dev-itpro/administration/monitor-use-performance-monitor-collect-event-trace-data).

## <a name="for-linux-machines"></a>Para máquinas Linux

En caso de que tenga una máquina, ya sea local o no, que ejecute el sistema operativo Linux, tiene a su disposición algunas herramientas más para supervisar los patrones de renovación.

### <a name="iotop"></a>Iotop

Una de las herramientas más usadas es _iotop_. Se trata de una utilidad para mostrar la actividad del disco en tiempo real. Puede enumerar los procesos que realizan operaciones de E/S, junto con el ancho de banda de disco que están usando.

Abra el símbolo del sistema y ejecute este comando: `iotop`.

### <a name="iostat"></a>IoStat

IoStat es una herramienta sencilla que recopila y muestra las estadísticas del dispositivo de almacenamiento de entrada y salida del sistema. Esta herramienta se usa a menudo para realizar un seguimiento de los problemas de rendimiento del dispositivo de almacenamiento, incluidos los dispositivos, los discos locales y los discos remotos.

Abra el símbolo del sistema y ejecute el comando `iostat`.

## <a name="next-steps"></a>Pasos siguientes

Obtenga información sobre cómo supervisar con [Azure Monitor](monitor-log-analytics.md).
