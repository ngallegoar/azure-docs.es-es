---
title: Configuración de la solución Start/Stop VMs during off-hours en Azure Automation
description: En este artículo se describe cómo configurar la solución Start/Stop VMs during off-hours para admitir diferentes casos de uso o escenarios.
services: automation
ms.subservice: process-automation
ms.date: 04/01/2020
ms.topic: conceptual
ms.openlocfilehash: 241866129aa36d67af18fab86a26d9cbf7ce42d6
ms.sourcegitcommit: b9d4b8ace55818fcb8e3aa58d193c03c7f6aa4f1
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/29/2020
ms.locfileid: "82582458"
---
# <a name="how-to-configure-startstop-vms-during-off-hours-solution"></a>Configuración de la solución Start/Stop VMs during off-hours

Con la solución **Start/Stop VMs during off-hours**, puede:

- [Programar el inicio y la detención de las máquinas virtuales](#schedule).
- Programar el inicio y la detención de las máquinas virtuales [mediante etiquetas de Azure](#tags) (no se admite en las máquinas virtuales clásicas).
- Detener automáticamente las máquinas virtuales si se detecta un [bajo uso de la CPU](#cpuutil).

En este artículo se describe cómo configurar correctamente la solución para admitir estos escenarios. También puede obtener información sobre cómo realizar otras opciones de configuración comunes para la solución, como:

* [Configurar notificaciones por correo electrónico](#configure-email-notifications)

* [Agregar una VM](#add-a-vm)

* [Excluir una VM](#exclude-a-vm)

* [Modificar las programaciones de inicio y apagado](#modify-the-startup-and-shutdown-schedules)

## <a name="scenario-1-startstop-vms-on-a-schedule"></a><a name="schedule"></a>Escenario 1: Iniciar o detener las máquinas virtuales según una programación

Este escenario es la configuración predeterminada cuando se implementa la solución por primera vez. Por ejemplo, puede configurarla para que detenga todas las máquinas virtuales de una suscripción por la noche, cuando salga del trabajo, e iniciarlas por la mañana cuando vuelva a la oficina. Al configurar las programaciones **Scheduled-StartVM** y **Scheduled-StopVM** durante la implementación, inician y detienen las máquinas virtuales de destino. Se admite la configuración de esta solución para simplemente detener las máquinas virtuales. Consulte [Modificación de las programaciones de inicio y apagado](#modify-the-startup-and-shutdown-schedules) para información sobre cómo configurar una programación personalizada.

> [!NOTE]
> La zona horaria es su zona horaria actual cuando configura el parámetro de hora de la programación. Sin embargo, en Azure Automation se almacena en formato UTC. No es necesario realizar ninguna conversión de zona horaria ya que se controla durante la implementación.

Mediante la configuración de las siguientes variables, controlará qué máquinas virtuales están en el ámbito: **External_Start_ResourceGroupNames**, **External_Stop_ResourceGroupNames** y **External_ExcludeVMNames**.

Puede habilitar que el destino de la acción sea una suscripción y un grupo de recursos, o una lista específica de máquinas virtuales, pero no ambas cosas.

### <a name="target-the-start-and-stop-actions-against-a-subscription-and-resource-group"></a>Destino de las acciones de inicio y detención a un grupo de recursos y una suscripción

1. Configure las variables `External_Stop_ResourceGroupNames` y `External_ExcludeVMNames` para especificar las VM de destino.

2. Habilite y actualice las programaciones **Scheduled-StartVM** y **Scheduled-StopVM**.

3. Ejecute el runbook **ScheduledStartStop_Parent** con el campo de parámetro **ACTION** establecido en **start** y el campo de parámetro **WHATIF** establecido en True para obtener una vista previa de los cambios.

### <a name="target-the-start-and-stop-action-by-vm-list"></a>Destino de las acciones de inicio y detención por lista de máquinas virtuales

1. Ejecute el runbook **ScheduledStartStop_Parent** con **ACTION** establecido en **start**, agregue una lista separada por comas de VM en el campo de parámetro **VMList** y establezca el parámetro **WHATIF** en True. Obtenga una vista previa de los cambios.

2. Configure la variable `External_ExcludeVMNames` con una lista separada por comas de VM (VM1, VM2, VM3).

3. En este escenario no se respetan las variables `External_Start_ResourceGroupNames` y `External_Stop_ResourceGroupnames`. Para este escenario, es preciso que cree su propia programación de Automation. Para más información, consulte [Programación de un runbook en Azure Automation](../automation/automation-schedules.md).

    > [!NOTE]
    > El valor de **Target ResourceGroup Names** (Nombres de ResourceGroup de destino) se almacena como valor tanto para `External_Start_ResourceGroupNames` como para `External_Stop_ResourceGroupNames`. A fin de lograr un mayor detalle, puede modificar cada una de estas variables para distintos grupos de recursos de destino. Para la acción de inicio, use `External_Start_ResourceGroupNames`; y para la acción de detención, use `External_Stop_ResourceGroupNames`. Las máquinas virtuales se agregan automáticamente a las programaciones de inicio y detención.

## <a name="scenario-2-startstop-vms-in-sequence-by-using-tags"></a><a name="tags"></a>Escenario 2: Iniciar o detener las máquinas virtuales en secuencia mediante el uso de etiquetas

En un entorno que incluya dos, o más, componentes de varias máquinas virtuales que admitan una carga de trabajo distribuida, es importante que se pueda decidir la secuencia en que los componentes se inician o detienen en orden. Para realizar este escenario, lleve a cabo los siguientes pasos:

### <a name="target-the-start-and-stop-actions-against-a-subscription-and-resource-group"></a>Destino de las acciones de inicio y detención a un grupo de recursos y una suscripción

1. Agregue etiquetas `sequencestart` y `sequencestop` con un valor entero positivo a las VM destinadas a las variables `External_Start_ResourceGroupNames` y `External_Stop_ResourceGroupNames`. Las acciones de inicio y detención se realizan en orden ascendente. Para aprender a etiquetar una máquina virtual, consulte [Etiquetado de una máquina virtual en Azure](../virtual-machines/windows/tag.md) y [Etiquetado de una máquina virtual Linux en Azure](../virtual-machines/linux/tag.md).

2. Modifique las programaciones de **Sequenced-StartVM** y **Sequenced-StopVM** a una fecha y hora que cumplan sus requisitos, y habilite la programación.

3. Ejecute el runbook **SequencedStartStop_Parent** con **ACTION** establecido en **start** y **WHATIF** establecido en True para obtener una vista previa de los cambios.

4. Obtenga una vista previa de la acción y realice los cambios necesarios antes de implementarla en las máquinas virtuales de producción. Cuando esté preparado, ejecute manualmente el runbook con el parámetro establecido en **False** o deje que la programación de Automation `Sequenced-StartVM` y `Sequenced-StopVM` se ejecuten automáticamente siguiendo su programación prescrita.

### <a name="target-the-start-and-stop-action-by-vm-list"></a>Destino de las acciones de inicio y detención por lista de máquinas virtuales

1. Agregue etiquetas `sequencestart` y `sequencestop` con un valor entero positivo para las VM que pretende agregar al parámetro `VMList`.

2. Ejecute el runbook **SequencedStartStop_Parent** con **ACTION** establecido en **start**, agregue una lista separada por comas de VM en el campo de parámetro **VMList** y establezca **WHATIF** en True. Obtenga una vista previa de los cambios.

3. Configure la variable `External_ExcludeVMNames` con una lista separada por comas de VM (VM1, VM2, VM3).

4. En este escenario no se respetan las variables `External_Start_ResourceGroupNames` y `External_Stop_ResourceGroupnames`. Para este escenario, es preciso que cree su propia programación de Automation. Para más información, consulte [Programación de un runbook en Azure Automation](../automation/automation-schedules.md).

5. Obtenga una vista previa de la acción y realice los cambios necesarios antes de implementarla en las máquinas virtuales de producción. Cuando esté listo, ejecute manualmente **monitoring-and-diagnostics/monitoring-action-groupsrunbook** con el parámetro establecido en **False**. Como alternativa, deje que las programaciones de Automation `Sequenced-StartVM` y `Sequenced-StopVM` se ejecuten automáticamente siguiendo la programación prescrita.

## <a name="scenario-3-startstop-automatically-based-on-cpu-utilization"></a><a name="cpuutil"></a>Escenario 3: Iniciar o detener automáticamente según el uso de CPU

Esta solución puede ayudarle a administrar el costo de la ejecución de máquinas virtuales de Azure Resource Manager y clásicas en su suscripción mediante la evaluación de las máquinas virtuales de Azure que no se utilizan durante los períodos de poca actividad, por ejemplo, las horas no laborables, y apagarlas automáticamente si el uso del procesador es menor de un porcentaje especificado.

De forma predeterminada, la solución está preconfigurada para evaluar la métrica de CPU en porcentaje, con el fin de ver si la utilización media es del 5 %, o menos. Este escenario se controla mediante las siguientes variables, y puede modificarse si los valores predeterminados no satisfacen sus requisitos:

* `External_AutoStop_MetricName`
* `External_AutoStop_Threshold`
* `External_AutoStop_TimeAggregationOperator`
* `External_AutoStop_TimeWindow`
* `External_AutoStop_Frequency`
* `External_AutoStop_Severity`

Puede habilitar que el destino de la acción sea una suscripción y un grupo de recursos, o una lista específica de máquinas virtuales.

Cuando se ejecuta el runbook **AutoStop_CreateAlert_Parent**, este comprueba que existen la suscripción, los grupos de recursos y las máquinas virtuales de destino. Si las VM existen, el runbook llama al runbook **AutoStop_CreateAlert_Child** de cada VM verificada mediante el runbook primario. Este runbook secundario realiza lo siguiente:

* Crea una regla de alertas de métricas para cada máquina virtual comprobada.

* Desencadena el runbook **AutoStop_VM_Child** para una VM determinada si la CPU caiga por debajo del umbral configurado durante el intervalo de tiempo especificado. A continuación, este runbook intenta detener la máquina virtual.

### <a name="to-target-the-auto-stop-action-against-all-vms-in-a-subscription"></a>Para hacer que el destino de la acción de detención automática sean todas las máquinas virtuales de una suscripción

1. Asegúrese de que la variable `External_Stop_ResourceGroupNames` esté vacía o establecida en * (carácter comodín).

2. [Paso opcional] Si desea excluir algunas VM del apagado automático, puede agregar una lista separada por comas de nombres de VM a la variable `External_ExcludeVMNames`.

3. Habilite la programación de `Schedule_AutoStop_CreateAlert_Parent` para que se ejecute para crear las reglas de alertas de métricas de detención de VM necesarias para todas las VM de la suscripción. La ejecución de este tipo de programación le permite crear nuevas reglas de alertas de métricas a medida que se agregan nuevas VM a la suscripción.

### <a name="to-target-the-auto-stop-action-against-all-vms-in-a-resource-group-or-multiple-resource-groups"></a>Para hacer que el destino de la acción de detención automática sean todas las VM de un grupo de recursos o de varios grupos de recursos

1. Agregue una lista separada por comas de nombres de grupos de recursos a la variable `External_Stop_ResourceGroupNames`.

2. Si desea excluir algunas de las VM del apagado automático, puede agregar una lista separada por comas de nombres de VM a la variable `External_ExcludeVMNames`.

3. Habilite la programación de **Schedule_AutoStop_CreateAlert_Parent** para que se ejecute para crear las reglas de alertas de métricas de detención de VM necesarias para todas las VM de los grupos de recursos. La ejecución de esta operación según una programación le permitirá crear nuevas reglas de alertas de métricas a medida que se agregan nuevas VM a los grupos de recursos.

### <a name="to-target-the-autostop-action-to-a-list-of-vms"></a>Para que el destino de la acción de detención automática sea una lista de VM

1. Cree una nueva [programación](shared-resources/schedules.md#create-a-schedule) y vincúlela al runbook **AutoStop_CreateAlert_Parent**. Para ello, agregue una lista separada por comas con los nombres de las VM al parámetro `VMList`.

2. De manera opcional, si desea excluir algunas VM del apagado automático, puede agregar una lista separada por comas de nombres de VM a la variable `External_ExcludeVMNames`.

## <a name="configure-email-notifications"></a>Configuración de notificaciones de correo electrónico

Para cambiar las notificaciones de correo electrónico después de implementar la solución, modifique el grupo de acciones creado durante la implementación.  

> [!NOTE]
> Las suscripciones en la nube de Azure Government no admiten la funcionalidad de correo electrónico de esta solución.

1. En Azure Portal, vaya a **Supervisar** y, a continuación, **Grupos de acciones**. Seleccione el grupo de acciones denominado **StartStop_VM_Notication**.

    ![Página de la solución Update Management de Automation](media/automation-solution-vm-management/azure-monitor.png)

2. En la página **StartStop_VM_Notification**, haga clic en **Editar detalles** en **Detalles**. Se abre la página **Email/SMS/Push/Voice** (Correo electrónico/SMS/Inserción/Voz). Actualice la dirección de correo electrónico y haga clic en **Aceptar** para guardar los cambios.

    ![Página de la solución Update Management de Automation](media/automation-solution-vm-management/change-email.png)

    También puede agregar acciones adicionales al grupo de acciones; para más información sobre grupos de acciones, consulte [grupos de acciones](../azure-monitor/platform/action-groups.md)

El siguiente es un correo electrónico de ejemplo que se envía cuando la solución cierra las máquinas virtuales.

![Página de la solución Update Management de Automation](media/automation-solution-vm-management/email.png)

## <a name="addexclude-vms"></a><a name="add-exclude-vms"></a>Agregar o excluir VM

La solución proporciona la capacidad de agregar VM como destino de la solución o excluir específicamente las máquinas de la solución.

### <a name="add-a-vm"></a>Agregar una VM

Hay un par de opciones que puede usar para asegurarse de que una máquina virtual se incluye en la solución Start/Stop cuando se ejecuta.

* Cada uno de los [runbooks](automation-solution-vm-management.md#runbooks) primarios de la solución tiene un parámetro `VMList`. Puede pasar una lista separada por comas de nombres de VM a este parámetro al programar el runbook primario adecuado para su situación, y estas VM se incluirán cuando se ejecute la solución.

* Para seleccionar varias VM, establezca `External_Start_ResourceGroupNames` y `External_Stop_ResourceGroupNames` en los nombres de los grupos de recursos que contienen las VM que desea iniciar o detener. También se puede establecer las variables en un valor de `*` para que la solución se ejecute en todos los grupos de recursos de la suscripción.

### <a name="exclude-a-vm"></a>Excluir una VM

Para excluir una VM de la solución, puede agregarla a la variable `External_ExcludeVMNames`. Esta variable es una lista separada por comas de VM específicas que se excluirán de la solución Start/Stop. Esta lista se limita a 140 VM. Si agrega más de 140 VM a esta lista separada por comas, las VM establecidas para excluirse pueden iniciarse o cerrarse accidentalmente.

## <a name="modify-the-startup-and-shutdown-schedules"></a>Modificación de las programaciones de inicio y apagado

La administración de las programaciones de inicio y apagado en esta solución sigue los mismos pasos descritos en [Programación de un runbook en Azure Automation](automation-schedules.md). Debe haber una programación independiente para iniciar y detener las VM.

Se admite la configuración de la solución para simplemente detener las máquinas virtuales en un momento determinado. En este escenario, simplemente crea una programación **Stop** y ninguna programación **Start** correspondiente. Para ello, necesita lo siguiente:

1. Asegúrese de que ha agregado los grupos de recursos para que las VM se apaguen en la variable `External_Stop_ResourceGroupNames`.

2. Cree su propia programación para el tiempo que desea apagar las máquinas virtuales.

3. Navegue hasta el runbook **ScheduledStartStop_Parent** y haga clic en **Programación**. Esto le permite seleccionar la programación que creó en el paso anterior.

4. Seleccione **Configuración de ejecución y parámetros** y establezca el campo **ACTION** en **Stop**.

5. Seleccione **Aceptar** para guardar los cambios.

## <a name="next-steps"></a>Pasos siguientes

* Para aprender a solucionar problemas de Start/Stop VMs during off-hours, consulte [Solución de problemas de Start/Stop VMs during off-hours](troubleshoot/start-stop-vm.md).

* [Revise](automation-solution-vm-management-logs.md) los registros de Automation escritos en los registros de Azure Monitor y las consultas de búsqueda de registros de ejemplo para analizar el estado de los trabajos de runbook de Automation desde Start/Stop VMs.
