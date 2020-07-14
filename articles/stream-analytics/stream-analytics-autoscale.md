---
title: Escalabilidad automática de trabajos de Stream Analytics
description: En este artículo se describe cómo escalar automáticamente un trabajo de Stream Analytics según una programación predefinida o valores de métricas de trabajo.
author: sidramadoss
ms.author: sidram
ms.reviewer: mamccrea
ms.service: stream-analytics
ms.topic: how-to
ms.date: 06/03/2020
ms.openlocfilehash: 07cbb28b98fcbac1932424c1c72f388813ec2400
ms.sourcegitcommit: e132633b9c3a53b3ead101ea2711570e60d67b83
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/07/2020
ms.locfileid: "86037569"
---
# <a name="autoscale-stream-analytics-jobs-using-azure-automation"></a>Escalabilidad automática de trabajos de Stream Analytics mediante Azure Automation

Puede optimizar el costo de los trabajos de Stream Analytics mediante la configuración de la escalabilidad automática. La escalabilidad automática aumenta o disminuye las unidades de streaming (SU) del trabajo para que coincidan con el cambio en la carga de entrada. En lugar de aprovisionar en exceso el trabajo, puede escalar o reducir verticalmente según sea necesario. Hay dos formas de configurar los trabajos para la escalabilidad automática:
1. **Definir previamente una programación** cuando tenga una carga de entrada predecible. Por ejemplo, espera una mayor tasa de eventos de entrada durante el día y quiere que el trabajo se ejecute con más SU.
2. **Desencadenar operaciones de escalado y reducción vertical basadas en métricas de trabajo** cuando no tiene una carga de entrada predecible. Puede cambiar dinámicamente el número de SU en función de las métricas del trabajo, como el número de eventos de entrada o los eventos de entrada pendientes.

## <a name="prerequisites"></a>Requisitos previos
Antes de empezar a configurar la escalabilidad automática para el trabajo, realice los pasos siguientes.
1. El trabajo debe estar optimizado para tener una [topología paralela](https://docs.microsoft.com/azure/stream-analytics/stream-analytics-parallelization). Si puede cambiar la escala del trabajo mientras se está ejecutando, el trabajo tiene una topología paralela y se puede configurar para la escalabilidad automática.
2. [Cree una cuenta de Azure Automation](https://docs.microsoft.com/azure/automation/automation-create-standalone-account) con la opción "RunAsAccount" habilitada. Esta cuenta debe tener permisos para administrar los trabajos de Stream Analytics.

## <a name="set-up-azure-automation"></a>Configuración de Azure Automation
### <a name="configure-variables"></a>Configuración de variables
Agregue las siguientes variables dentro de la cuenta de Azure Automation. Estas variables se usarán en los runbooks que se describen en los pasos siguientes.

| Nombre | Tipo | Value |
| --- | --- | --- |
| **jobName** | String | Nombre del trabajo de Stream Analytics que quiere escalar automáticamente. |
| **resourceGroupName** | String | Nombre del grupo de recursos en el que está presente el trabajo. |
| **subId** | String | Id. de la suscripción en la que está presente el trabajo. |
| **increasedSU** | Entero | El valor de SU superior al que quiere que se escale el trabajo en una programación. Este valor debe ser una de las opciones de SU válidas que se pueden ver en la configuración **Escala** del trabajo mientras se está ejecutando. |
| **decreasedSU** | Entero | El valor de SU inferior al que quiere que se escale el trabajo en una programación. Este valor debe ser una de las opciones de SU válidas que se pueden ver en la configuración **Escala** del trabajo mientras se está ejecutando. |
| **maxSU** | Entero | El valor de SU máximo al que quiere escalar el trabajo en pasos, al escalar automáticamente por carga. Este valor debe ser una de las opciones de SU válidas que se pueden ver en la configuración **Escala** del trabajo mientras se está ejecutando. |
| **minSU** | Entero | El valor de SU mínimo al que quiere escalar el trabajo en pasos, al escalar automáticamente por carga. Este valor debe ser una de las opciones de SU válidas que se pueden ver en la configuración **Escala** del trabajo mientras se está ejecutando. |

![Agregación de variables en Azure Automation](./media/autoscale/variables.png)

### <a name="create-runbooks"></a>Crear runbooks
El siguiente paso consiste en crear dos runbooks de PowerShell. Uno para el escalado vertical y otro para las operaciones de reducción vertical.
1. En la cuenta de Azure Automation, vaya a **Runbooks** en **Automatización de procesos** y seleccione **Create Runbook** (Crear Runbook).
2. Asigne al primer runbook el nombre *ScaleUpRunbook* con el tipo establecido en PowerShell. Use el [script ScaleUpRunbook de PowerShell](https://github.com/Azure/azure-stream-analytics/blob/master/Autoscale/ScaleUpRunbook.ps1) disponible en GitHub. Guárdelo y publíquelo.
3. Cree otro runbook llamado *ScaleDownRunbook* con el tipo PowerShell. Use el [script ScaleDownRunbook de PowerShell](https://github.com/Azure/azure-stream-analytics/blob/master/Autoscale/ScaleDownRunbook.ps1) disponible en GitHub. Guárdelo y publíquelo.

![Escalabilidad automática de runbooks en Azure Automation](./media/autoscale/runbooks.png)

Ahora tiene runbooks que pueden desencadenar automáticamente operaciones de escalado y reducción vertical en el trabajo de Stream Analytics. Estos runbooks se pueden desencadenar mediante una programación predefinida o se pueden establecer de forma dinámica en función de las métricas del trabajo.

## <a name="autoscale-based-on-a-schedule"></a>Escalado automático según una programación
Azure Automation permite configurar una programación para desencadenar los runbooks.
1. En la cuenta de Azure Automation, seleccione **Programaciones** en **Recursos compartidos**. A continuación, seleccione **Agregar una programación**.
2. Por ejemplo, puede crear un dos programaciones. Uno que representa cuándo quiere que el trabajo se escale verticalmente y otro que representa cuándo quiere que el trabajo se reduzca verticalmente. Puede definir una periodicidad para estas programaciones.

   ![Programaciones en Azure Automation](./media/autoscale/schedules.png)

3. Abra **ScaleUpRunbook** y, a continuación, seleccione **Programaciones** en **Recursos**. Después, puede vincular el runbook a una programación que creó en los pasos anteriores. Puede tener varias programaciones vinculadas al mismo runbook, lo que puede resultar útil si quiere ejecutar la misma operación de escalado en distintos momentos del día.

![Programación de runbooks en Azure Automation](./media/autoscale/schedulerunbook.png)

1. Repita el paso anterior para **ScaleDownRunbook**.

## <a name="autoscale-based-on-load"></a>Escalabilidad automática según la carga
Puede haber casos en los que no se pueda predecir la carga de entrada. En tales casos, es mejor escalar o reducir verticalmente en pasos, dentro de un límite mínimo y máximo. Puede configurar reglas de alerta en los trabajos de Stream Analytics para desencadenar runbooks cuando las métricas de trabajo suben por encima o bajan por debajo de un umbral.
1. En la cuenta de Azure Automation, cree dos variables de entero más llamadas **minSU** y **maxSU**. Esto establece los límites dentro de los cuales se escalará el trabajo en pasos.
2. Cree dos nuevos runbooks. Puede usar el [script StepScaleUp de PowerShell](https://github.com/Azure/azure-stream-analytics/blob/master/Autoscale/StepScaleUp.ps1) que aumenta las SU del trabajo en incrementos, hasta **maxSU**. También puede usar el [script StepScaleDown de PowerShell](https://github.com/Azure/azure-stream-analytics/blob/master/Autoscale/StepScaleDown.ps1) que reduce las SU del trabajo en pasos, hasta **minSU**. Como alternativa, puede usar los runbooks de la sección anterior si tiene valores de SU específicos a los que quiere escalar.
3. En su trabajo de Stream Analytics, seleccione **Reglas de alertas** en **Supervisión**. 
4. Cree dos grupos de acciones. Uno que se va a usar para la operación de escalado vertical y otro para la operación de reducción vertical. Seleccione **Manage Actions** (Administrar acciones) y, a continuación, haga clic en **Agregar grupo de acciones**. 
5. Rellene los campos obligatorios. Elija **Runbook de Automation** al seleccionar el **Tipo de acción**. Seleccione el runbook que quiere desencadenar cuando se active la alerta. A continuación, cree el grupo de acciones.

   ![Creación de un grupo de acciones](./media/autoscale/create-actiongroup.png)
6. Cree una [**Nueva regla de alertas**](https://docs.microsoft.com/azure/stream-analytics/stream-analytics-set-up-alerts#set-up-alerts-in-the-azure-portal) en el trabajo. Especifique una condición basada en una métrica de su elección. [*Eventos de entrada*, *SU % Utilización* o *Backlogged Input Events*](https://docs.microsoft.com/azure/stream-analytics/stream-analytics-monitoring#metrics-available-for-stream-analytics) (Eventos de entrada pendientes) son métricas recomendadas que se usarán para definir la lógica de escalabilidad automática. También se recomienda usar un valor de *Aggregation granularity* (Granularidad de agregación) y *Frequency of evaluation* (Frecuencia de evaluación) de 1 minuto al desencadenar operaciones de escala vertical. De este modo, se garantiza que el trabajo tiene amplios recursos para hacer frente a picos grandes en el volumen de entrada.
7. Seleccione el grupo de acciones creado en el último paso y cree la alerta.
8. Repita los pasos del 2 al 4 para las operaciones de escalado adicionales que quiera desencadenar en función de la condición de las métricas de trabajo.

Es un procedimiento recomendado ejecutar pruebas de escala antes de ejecutar el trabajo en producción. Al probar el trabajo con distintas cargas de entrada, se obtiene una idea de cuántas SU necesita el trabajo para el rendimiento de diferentes entrada. Puede informar las condiciones que defina en las reglas de alerta que desencadenan las operaciones de escalado y reducción vertical. 

## <a name="next-steps"></a>Pasos siguientes
* [Creación de consultas que se pueden paralelizar en Azure Stream Analytics](stream-analytics-parallelization.md)
* [Escalar los trabajos de Azure Stream Analytics para incrementar el rendimiento](stream-analytics-scale-jobs.md)
