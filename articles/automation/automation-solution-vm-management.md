---
title: Solución Start/Stop VMs during off-hours
description: Esta solución de administración de máquinas virtuales inicia y detiene las máquinas virtuales de Azure Resource Manager según una programación y realiza una supervisión proactiva desde los registros de Azure Monitor.
services: automation
ms.subservice: process-automation
ms.date: 02/25/2020
ms.topic: conceptual
ms.openlocfilehash: cbf181b9a6d3860854c7b61cca0e6c50810cced9
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "79235492"
---
# <a name="startstop-vms-during-off-hours-solution-in-azure-automation"></a>Solución Start/Stop VMs during off-hours en Azure Automation

La solución Start/Stop VMs during off-hours inicia y detiene las máquinas virtuales de Azure según las programaciones definidas por el usuario, proporciona información detallada mediante registros de Azure Monitor y envía mensajes de correo electrónico opcionales mediante [grupos de acciones](../azure-monitor/platform/action-groups.md). Admite tanto máquinas virtuales clásicas como Azure Resource Manager para la mayoría de los escenarios. Para usar esta solución con máquinas virtuales clásicas, necesita una cuenta de ejecución clásica, que no se crea de forma predeterminada. Para obtener instrucciones sobre cómo crear una cuenta de ejecución clásica, consulte [Cuentas de ejecución clásicas](automation-create-standalone-account.md#classic-run-as-accounts).

> [!NOTE]
> La solución Start/Stop VMs during off-hours se ha probado con los módulos de Azure que se importan en su cuenta de Automation al implementar la solución. La solución no funciona actualmente con las versiones más recientes del módulo de Azure. Esto solo afecta a la cuenta de Automation que use para ejecutar la solución Start/Stop VMs during off-hours. Puede seguir usando las versiones más recientes del módulo de Azure en sus otras cuentas de Automation, tal como se describe en [Actualización de módulos de Azure PowerShell en Azure Automation](automation-update-azure-modules.md).

Esta solución proporciona una opción de automatización descentralizada de bajo costo para los usuarios que desean optimizar sus costos de máquinas virtuales. Con esta solución, es posible:

- Programar el inicio y la detención de las máquinas virtuales.
- Programar el inicio y la detención de las máquinas virtuales mediante etiquetas de Azure (no se admite en las máquinas virtuales clásicas).
- Detener automáticamente las máquinas virtuales si se detecta un bajo uso de la CPU

Las siguientes son limitaciones a la solución actual:

- Esta solución administrar máquinas virtuales de cualquier región, pero solo se puede utilizar en la misma suscripción que la cuenta de Azure Automation.
- Esta solución está disponible en Azure y AzureGov para cualquier región que admita un área de trabajo de Log Analytics, una cuenta de Azure Automation y alertas. Las regiones de AzureGov no admiten la funcionalidad de correo electrónico en este momento.

> [!NOTE]
> Si usa la solución para las máquinas virtuales clásicas, todas las máquinas virtuales se procesarán secuencialmente por servicio en la nube. Las máquinas virtuales aún se procesarán en paralelo entre diferentes servicios en la nube. Si tiene más de 20 máquinas virtuales por servicio en la nube, se recomienda crear varias programaciones con el runbook primario **ScheduledStartStop_Parent** y especificar 20 máquinas virtuales por programación. En las propiedades de la programación, especifique los nombres de las máquinas virtuales como una lista separada por comas en el parámetro **VMList**. De lo contrario, si el trabajo de Automation para esta solución se ejecuta durante más de tres horas, se descargará temporalmente o se detendrá debido al límite de tiempo de [distribución equilibrada](automation-runbook-execution.md#fair-share).
>
> Las suscripciones de Proveedor de soluciones en la nube de Azure (Azure CSP) solo admiten el modelo de Azure Resource Manager, los servicios que no sean de Azure Resource Manager no están disponibles en el programa. Cuando se ejecuta la solución Start/Stop, puede recibir errores porque contiene cmdlets para administrar los recursos clásicos. Para más información sobre CSP, consulte [Servicios disponibles en las suscripciones de CSP](https://docs.microsoft.com/azure/cloud-solution-provider/overview/azure-csp-available-services). Si usa una suscripción de CSP, debe modificar la variable [**External_EnableClassicVMs**](#variables) a **False** después de la implementación.

[!INCLUDE [azure-monitor-log-analytics-rebrand](../../includes/azure-monitor-log-analytics-rebrand.md)]

## <a name="prerequisites"></a>Prerrequisitos

Los runbooks para esta solución funcionan con una [cuenta de ejecución de Azure](automation-create-runas-account.md). La cuenta de ejecución es el método de autenticación preferido, ya que emplea la autenticación mediante certificado, en lugar de una contraseña que puede expirar o cambiar con frecuencia.

Se recomienda usar una cuenta de Automation independiente para la solución Start/Stop VM. Esto se debe a que las versiones del módulo de Azure se actualizan con frecuencia y puede que sus parámetros cambien. La solución Start/Stop VM no se actualiza con la misma cadencia, por lo que es posible que no funcione con las versiones más recientes de los cmdlets que usa. También se recomienda probar las actualizaciones del módulo en una cuenta de prueba de Automation antes de importarlas en su cuenta (o sus cuentas) de producción de Automation.

### <a name="permissions-needed-to-deploy"></a>Permisos necesarios para la implementación

Hay determinados permisos que debe tener un usuario para implementar la solución Start/Stop VMs during off-hours. Estos permisos son diferentes si usa un área de trabajo de Log Analytics y una cuenta de Automation que se han creado previamente o crea otras durante la implementación. Si es un colaborador de la suscripción y un administrador global del inquilino de Azure Active Directory, no tiene que configurar los permisos siguientes. Si no tiene esos derechos o necesita configurar un rol personalizado, vea los permisos necesarios a continuación.

#### <a name="pre-existing-automation-account-and-log-analytics-workspace"></a>Cuenta de Automation y área de trabajo de Log Analytics ya existentes

Para implementar la solución Start/Stop VMs during off-hours en una cuenta de Automation y un área de trabajo de Log Analytics existentes, el usuario que implementa la solución requiere los siguientes permisos en el **grupo de recursos**. Para obtener más información sobre los roles, consulte [Roles personalizados en los recursos de Azure](../role-based-access-control/custom-roles.md).

| Permiso | Ámbito|
| --- | --- |
| Microsoft.Automation/automationAccounts/read | Grupo de recursos |
| Microsoft.Automation/automationAccounts/variables/write | Grupo de recursos |
| Microsoft.Automation/automationAccounts/schedules/write | Grupo de recursos |
| Microsoft.Automation/automationAccounts/runbooks/write | Grupo de recursos |
| Microsoft.Automation/automationAccounts/connections/write | Grupo de recursos |
| Microsoft.Automation/automationAccounts/certificates/write | Grupo de recursos |
| Microsoft.Automation/automationAccounts/modules/write | Grupo de recursos |
| Microsoft.Automation/automationAccounts/modules/read | Grupo de recursos |
| Microsoft.automation/automationAccounts/jobSchedules/write | Grupo de recursos |
| Microsoft.Automation/automationAccounts/jobs/write | Grupo de recursos |
| Microsoft.Automation/automationAccounts/jobs/read | Grupo de recursos |
| Microsoft.OperationsManagement/solutions/write | Grupo de recursos |
| Microsoft.OperationalInsights/workspaces/* | Grupo de recursos |
| Microsoft.Insights/diagnosticSettings/write | Grupo de recursos |
| Microsoft.Insights/ActionGroups/Write | Grupo de recursos |
| Microsoft.Insights/ActionGroups/read | Grupo de recursos |
| Microsoft.Resources/subscriptions/resourceGroups/read | Grupo de recursos |
| Microsoft.Resources/deployments/* | Grupo de recursos |

#### <a name="new-automation-account-and-a-new-log-analytics-workspace"></a>Nueva cuenta de Automation y nueva área de trabajo de Log Analytics

Para implementar la solución Start/Stop VMs during off-hours en una nueva cuenta de Automation y un área de trabajo de Log Analytics, el usuario que implementa la solución necesita los permisos que se han definido en la sección anterior, así como los siguientes permisos:

- Coadministrador de la suscripción: solo es necesario para crear la cuenta de ejecución clásica si va a administrar máquinas virtuales clásicas. Las [cuentas de ejecución clásicas](automation-create-standalone-account.md#classic-run-as-accounts) ya no se crean de forma predeterminada.
- Miembro del rol **Desarrollador de aplicaciones** de [Azure Active Directory](../active-directory/users-groups-roles/directory-assign-admin-roles.md). Para obtener más información sobre cómo configurar las cuentas de ejecución, consulte [Permisos para configurar cuentas de ejecución](manage-runas-account.md#permissions).
- Colaborador de la suscripción o los siguientes permisos.

| Permiso |Ámbito|
| --- | --- |
| Microsoft.Authorization/Operations/read | Subscription|
| Microsoft.Authorization/permissions/read |Subscription|
| Microsoft.Authorization/roleAssignments/read | Subscription |
| Microsoft.Authorization/roleAssignments/write | Subscription |
| Microsoft.Authorization/roleAssignments/delete | Subscription |
| Microsoft.Automation/automationAccounts/connections/read | Grupo de recursos |
| Microsoft.Automation/automationAccounts/certificates/read | Grupo de recursos |
| Microsoft.Automation/automationAccounts/write | Grupo de recursos |
| Microsoft.OperationalInsights/workspaces/write | Grupo de recursos |

## <a name="deploy-the-solution"></a>Implementación de la solución

Realice los siguientes pasos para agregar la solución Start/Stop VMs during off-hours a su cuenta de Automation y, después, configure las variables para personalizar la solución.

1. En una cuenta de Automation, seleccione **Start/Stop VM** en **Recursos relacionados**. Desde aquí, puede hacer clic en **Más información acerca de la solución y cómo habilitarla**. Si ya tiene una solución Start/Stop VM implementada, puede seleccionarla; para ello, haga clic en **Administrar la solución** y búsquela en la lista.

   ![Habilitación desde una cuenta de Automation](./media/automation-solution-vm-management/enable-from-automation-account.png)

   > [!NOTE]
   > También puede crearla desde cualquier lugar en Azure Portal; para ello, haga clic en **Crear un recurso**. En la página de Marketplace, escriba una palabra clave como **Iniciar** o **Inciar/Detener**. Cuando comience a escribir, la lista se filtrará en función de la entrada. Como alternativa, puede escribir en una o varias palabras clave del nombre completo de la solución y presione Entrar. Seleccione **Start/Stop VMs during off-hours** en los resultados de la búsqueda.

2. En la página **Start/Stop VMs during off-hours**, revise la información de resumen y luego haga clic en **Crear**.

   ![Portal de Azure](media/automation-solution-vm-management/azure-portal-01.png)

3. Aparece la página **Agregar solución**. Se le pide que configure la solución antes de importarla en la suscripción de Automation.

   ![Página Agregar solución de administración de VM](media/automation-solution-vm-management/azure-portal-add-solution-01.png)

4. En el la página **Agregar solución**, seleccione **Área de trabajo**. Seleccione un área de trabajo de Log Analytics que esté vinculada a la suscripción de Azure en la que se encuentra la cuenta de Automation. Si no tiene ningún área de trabajo, seleccione **Crear nueva área de trabajo**. En la página **Área de trabajo de Log Analytics**, siga estos pasos:
   - Especifique un nombre para la nueva **área de trabajo de Log Analytics** como, por ejemplo, "ContosoLAWorkspace".
   - Seleccione la **suscripción** a la que vincularlo en la lista desplegable si la opción predeterminada seleccionada no es adecuada.
   - En **Grupo de recursos**, puede crear un grupo de recursos nuevo o seleccionar uno existente.
   - Seleccione una **ubicación**.
   - Seleccione un **plan de tarifa**. Elija la opción **Por GB (independiente)** . Los registros de Azure Monitor tienen [precios](https://azure.microsoft.com/pricing/details/log-analytics/) actualizados y el nivel por GB es la única opción.

   > [!NOTE]
   > Al habilitar las soluciones, solo en determinadas regiones se puede vincular un área de trabajo de Log Analytics y una cuenta de Automation.
   >
   > Para obtener una lista de los pares de asignación que se admiten, consulte [Asignación de región para la cuenta de Automation y el área de trabajo de Log Analytics](how-to/region-mappings.md).

5. Después de proporcionar la información necesaria en la página **Área de trabajo de Log Analytics**, haga clic en **Crear**. Se puede realizar un seguimiento de su progreso en la opción **Notificaciones** del menú, que le devuelve a la página **Agregar solución** cuando haya finalizado.
6. En la página **Agregar solución**, seleccione **Cuenta de Automation**. Si va a crear una nueva área de trabajo de Log Analytics, puede crear una cuenta de Automation para asociarla, o seleccionar una cuenta de Automation existente que no esté ya vinculada a un área de trabajo de Log Analytics. Seleccione una cuenta de Automation existente o haga clic en **Crear una cuenta de Automation** y, en la página **Agregar cuenta de Automation**, especifique la siguiente información:
   - En el campo **Nombre**, escriba el nombre de la cuenta de Automation.

     Las restantes opciones se rellenan automáticamente en función del área de trabajo de Log Analytics seleccionada. Dichas opciones no se pueden modificar. Una cuenta de ejecución de Azure es el método de autenticación predeterminado para los Runbooks incluidos en esta solución. Después de hacer clic en **Aceptar**, se validan las opciones de configuración y se crea la cuenta de Automation. Puede realizar un seguimiento de su progreso en **Notificaciones** en el menú.

7. Por último, en la página **Agregar solución**, seleccione **Configuración**. Aparece la página **Parámetros**.

   ![Página Parámetros para la solución](media/automation-solution-vm-management/azure-portal-add-solution-02.png)

   En este caso, se le pedirá que:
   - Especifique los **nombres del grupo de recursos de destino**. Estos valores son nombres de grupos de recursos que contienen máquinas virtuales que se administrarán mediante esta solución. Puede especificar más de un nombre y separarlos con una coma (los valores no distinguen mayúsculas de minúsculas). Se puede usar un carácter comodín si quiere dirigirse a las máquinas virtuales de todos los grupos de recursos de la suscripción. Este valor se almacena en las variables **External_Start_ResourceGroupNames** y **External_Stop_ResourceGroupNames**.
   - Especifique el **lista de exclusiones de máquinas virtuales (cadena)** . Este valor es el nombre de una o varias máquinas virtuales del grupo de recursos de destino. Puede especificar más de un nombre y separarlos con una coma (los valores no distinguen mayúsculas de minúsculas). Se admite el uso de un carácter comodín. Este valor se almacena en la variable **External_ExcludeVMNames**.
   - Seleccione una **programación**. Seleccione una fecha y hora para la programación. Se creará una programación recurrente diaria a partir de la hora que ha seleccionado. No se puede seleccionar otra región. Para configurar su zona horaria concreta después de configurar la solución, consulte el siguiente artículo [Modificación de la programación de inicio y apagado](#modify-the-startup-and-shutdown-schedules).
   - Para recibir **notificaciones por correo electrónico** de un grupo de acciones, acepte el valor predeterminado **Sí** y proporcione una dirección de correo electrónico válida. Si selecciona **No** pero decide en un momento posterior que desea recibir notificaciones por correo electrónico, puede actualizar el [grupo de acciones](../azure-monitor/platform/action-groups.md) que se crea con las direcciones de correo electrónico válidas separadas por un punto y coma. También deberá habilitar las reglas de alerta siguientes:

     - AutoStop_VM_Child
     - Scheduled_StartStop_Parent
     - Sequenced_StartStop_Parent

     > [!IMPORTANT]
     > El valor predeterminado de los **nombres del grupo de recursos de destino** es **&ast;** . Este se dirige a todas las máquinas virtuales de una suscripción. Si no desea que la solución se dirija a todas las máquinas virtuales de su suscripción, este valor debe actualizarse a una lista de nombres del grupo de recursos antes de habilitar las programaciones.

8. Cuando haya configurado los valores iniciales necesarios para la solución, haga clic en **Aceptar** para cerrar la página **Parámetros** y seleccione **Crear**. Cuando todos los valores se hayan validado, la solución se implementa en su suscripción. Este proceso puede tardar varios segundos en finalizar y se puede realizar un seguimiento de su progreso en **Notificaciones** en el menú.

> [!NOTE]
> Si tiene una suscripción al Proveedor de soluciones en la nube de Azure (CSP de Azure), una vez completada la implementación, acceda a su cuenta de Automation, vaya a **Variables** en **Recursos compartidos** y establezca la variable [**External_EnableClassicVMs**](#variables) en **False**. Esta operación detiene la solución para que deje de buscar recursos de la máquina virtual clásica.

## <a name="scenarios"></a>Escenarios

La solución contiene tres escenarios diferentes. Dichos escenarios son:

### <a name="scenario-1-startstop-vms-on-a-schedule"></a>Escenario 1: Iniciar o detener las máquinas virtuales según una programación

Este escenario es la configuración predeterminada cuando se implementa la solución por primera vez. Por ejemplo, puede configurarla para que detenga todas las máquinas virtuales de una suscripción por la noche, cuando salga del trabajo, e iniciarlas por la mañana cuando vuelva a la oficina. Al configurar las programaciones **Scheduled-StartVM** y **Scheduled-StopVM** durante la implementación, inician y detienen las máquinas virtuales de destino. Se admite la configuración de esta solución para simplemente detener las máquinas virtuales. Consulte [Modificación de las programaciones de inicio y apagado](#modify-the-startup-and-shutdown-schedules) para información sobre cómo configurar una programación personalizada.

> [!NOTE]
> La zona horaria es su zona horaria actual cuando configura el parámetro de hora de la programación. Sin embargo, en Azure Automation se almacena en formato UTC. No es necesario realizar ninguna conversión de zona horaria ya que se controla durante la implementación.

Mediante la configuración de las siguientes variables, controlará qué máquinas virtuales están en el ámbito: **External_Start_ResourceGroupNames**, **External_Stop_ResourceGroupNames** y **External_ExcludeVMNames**.

Puede habilitar que el destino de la acción sea una suscripción y un grupo de recursos, o una lista específica de máquinas virtuales, pero no ambas cosas.

#### <a name="target-the-start-and-stop-actions-against-a-subscription-and-resource-group"></a>Destino de las acciones de inicio y detención a un grupo de recursos y una suscripción

1. Configure las variables **External_Stop_ResourceGroupNames** y **External_ExcludeVMNames** para especificar las máquinas virtuales de destino.
2. Habilite y actualice las programaciones **Scheduled-StartVM** y **Scheduled-StopVM**.
3. Ejecute el runbook **ScheduledStartStop_Parent** con el parámetro ACTION establecido en **start** y el parámetro WHATIF establecido en **True** para obtener una vista previa de los cambios.

#### <a name="target-the-start-and-stop-action-by-vm-list"></a>Destino de las acciones de inicio y detención por lista de máquinas virtuales

1. Ejecute el runbook **ScheduledStartStop_Parent** con el parámetro ACTION establecido en **start**, agregue una lista separada por comas de máquinas virtuales en el parámetro *VMList* y establezca el parámetro WHATIF en **True**. Obtenga una vista previa de los cambios.
1. Configure el parámetro **External_ExcludeVMNames** con una lista separada por comas de máquinas virtuales (VM1, VM2, VM3).
1. Este escenario no respeta las variables **External_Start_ResourceGroupNames** y **External_Stop_ResourceGroupnames**. Para este escenario, es preciso que cree su propia programación de Automation. Para más información, consulte [Programación de un runbook en Azure Automation](../automation/automation-schedules.md).

> [!NOTE]
> El valor de **Target ResourceGroup Names** se almacena como el valor tanto de **External_Start_ResourceGroupNames** como de **External_Stop_ResourceGroupNames**. A fin de lograr un mayor detalle, puede modificar cada una de estas variables para distintos grupos de recursos de destino. Para la acción de inicio, use **External_Start_ResourceGroupNames** y para la acción de detención use **External_Stop_ResourceGroupNames**. Las máquinas virtuales se agregan automáticamente a las programaciones de inicio y detención.

### <a name="scenario-2-startstop-vms-in-sequence-by-using-tags"></a>Escenario 2: Iniciar o detener las máquinas virtuales en secuencia mediante el uso de etiquetas

En un entorno que incluya dos, o más, componentes de varias máquinas virtuales que admitan una carga de trabajo distribuida, es importante que se pueda decidir la secuencia en que los componentes se inician o detienen en orden. Para realizar este escenario, lleve a cabo los siguientes pasos:

#### <a name="target-the-start-and-stop-actions-against-a-subscription-and-resource-group"></a>Destino de las acciones de inicio y detención a un grupo de recursos y una suscripción

1. Agregue una etiqueta **sequencestart** y **sequencestop** con un valor entero positivo a las máquinas virtuales cuyo destino se establece en las variables **External_Start_ResourceGroupNames** y **External_Stop_ResourceGroupNames**. Las acciones de inicio y detención se realizan en orden ascendente. Para aprender a etiquetar una máquina virtual, consulte [Etiquetado de una máquina virtual en Azure](../virtual-machines/windows/tag.md) y [Etiquetado de una máquina virtual Linux en Azure](../virtual-machines/linux/tag.md).
1. Modifique las programaciones de **Sequenced-StartVM** y **Sequenced-StopVM** a una fecha y hora que cumplan sus requisitos, y habilite la programación.
1. Ejecute el runbook **SequencedStartStop_Parent** con el parámetro ACTION establecido en **start** y el parámetro WHATIF establecido en **True** para obtener una vista previa de los cambios.
1. Obtenga una vista previa de la acción y realice los cambios necesarios antes de implementarla en las máquinas virtuales de producción. Cuando esté preparado, ejecute manualmente el runbook con el parámetro establecido en **False** o deje que la programación de Automation **Sequenced-StartVM** y **Sequenced-StopVM** se ejecuten automáticamente siguiendo su programación prescrita.

#### <a name="target-the-start-and-stop-action-by-vm-list"></a>Destino de las acciones de inicio y detención por lista de máquinas virtuales

1. Agregue una etiqueta **sequencestart** y **sequencestop** con un valor entero positivo para las VM que pretende agregar al parámetro **VMList**.
1. Ejecute el runbook **SequencedStartStop_Parent** con el parámetro ACTION establecido en **start**, agregue una lista separada por comas de máquinas virtuales en el parámetro *VMList* y establezca el parámetro WHATIF en **True**. Obtenga una vista previa de los cambios.
1. Configure el parámetro **External_ExcludeVMNames** con una lista separada por comas de máquinas virtuales (VM1, VM2, VM3).
1. Este escenario no respeta las variables **External_Start_ResourceGroupNames** y **External_Stop_ResourceGroupnames**. Para este escenario, es preciso que cree su propia programación de Automation. Para más información, consulte [Programación de un runbook en Azure Automation](../automation/automation-schedules.md).
1. Obtenga una vista previa de la acción y realice los cambios necesarios antes de implementarla en las máquinas virtuales de producción. Cuando esté preparado, ejecute manualmente el runbook monitoring-and-diagnostics/monitoring-action-groups con el parámetro establecido en **False** o deje que la programación de Automation **Sequenced-StartVM** y **Sequenced-StopVM** se ejecuten automáticamente siguiendo su programación prescrita.

## <a name="solution-components"></a>Componentes de soluciones

Esta solución incluye runbooks preconfigurados, programaciones e integración con los registros de Azure Monitor para que pueda personalizar el inicio y el apagado de las máquinas virtuales para adaptarlas a las necesidades de su empresa.

### <a name="runbooks"></a>Runbooks

En la tabla siguiente se enumeran los runbooks implementados en su cuenta de Automation por esta solución. No realice cambios en el código del runbook. En su lugar, escriba un runbook que aporte una nueva funcionalidad.

> [!IMPORTANT]
> No ejecute directamente ningún runbook cuyo nombre tenga anexado "child".

Todos los runbooks primarios incluyen el parámetro All _WhatIf_. Cuando se establece en **True**, _Whatif_ permite detallar el comportamiento exacto del runbook cuando se ejecuta sin el parámetro _WhatIf_ y valida que el destino son las máquinas virtuales correctas. Los runbooks solo realizan sus acciones definidas cuando el parámetro _WhatIf_ está establecido en **False**.

|Runbook | Parámetros | Descripción|
| --- | --- | ---|
|AutoStop_CreateAlert_Child | VMObject <br> AlertAction <br> WebHookURI | Se llama desde el runbook primario. Este runbook crea alertas según el recurso para el escenario de AutoStop.|
|AutoStop_CreateAlert_Parent | VMList<br> WhatIf: Verdadero o falso  | Crea o actualiza las reglas de alerta de Azure en las máquinas virtuales de los grupos de recursos o la suscripción de destino. <br> VMList: lista de máquinas virtuales separadas por comas. Por ejemplo, _vm1, vm2, vm3_.<br> *WhatIf* valida la lógica de runbook sin ejecución.|
|AutoStop_Disable | None | Deshabilita la programación predeterminada y las alertas de AutoStop.|
|AutoStop_StopVM_Child | WebHookData | Se llama desde el runbook primario. Las reglas de alertas llaman a este runbook para detener la máquina virtual.|
|Bootstrap_Main | None | Se usa una vez para realizar configuraciones de arranque como webhookURI, a las que normalmente no se puede acceder desde Azure Resource Manager. Este runbook se quita automáticamente tras la implementación correcta.|
|ScheduledStartStop_Child | VMName <br> Acción: Iniciar o detener <br> ResourceGroupName | Se llama desde el runbook primario. Ejecuta una acción de inicio o detención para la detención programada.|
|ScheduledStartStop_Parent | Acción: Iniciar o detener <br>VMList <br> WhatIf: Verdadero o falso | Este valor afecta a todas las máquinas virtuales de la suscripción. Edite **External_Start_ResourceGroupNames** y **External_Stop_ResourceGroupNames** para que se ejecute solo en estos grupos de recursos de destino. También puede excluir máquinas virtuales específicas si actualiza la variable **External_ExcludeVMNames**.<br> VMList: lista de máquinas virtuales separadas por comas. Por ejemplo, _vm1, vm2, vm3_.<br> _WhatIf_ valida la lógica de runbook sin ejecución.|
|SequencedStartStop_Parent | Acción: Iniciar o detener <br> WhatIf: Verdadero o falso<br>VMList| Cree etiquetas denominadas **sequencestart** y **sequencestop** en todas las máquinas virtuales en las que quiera secuenciar la actividad de inicio y detención. Estos nombres de etiqueta distinguen entre mayúsculas y minúsculas. El valor de la etiqueta debe ser un entero positivo (1, 2, 3) que se corresponda con el orden en que se desee que se realice el inicio o la detención. <br> VMList: lista de máquinas virtuales separadas por comas. Por ejemplo, _vm1, vm2, vm3_. <br> _WhatIf_ valida la lógica de runbook sin ejecución. <br> **Nota**: Las máquinas virtuales deben estar en grupos de recursos definidos como External_Start_ResourceGroupNames, External_Stop_ResourceGroupNames y External_ExcludeVMNames, en las variables de Azure Automation. Para que las acciones surtan efecto, deben tener las etiquetas correspondientes.|

### <a name="variables"></a>variables

En la tabla siguiente se enumeran las variables creadas en su cuenta de Automation. Modifique solo las variables cuyo prefijo sea **External**. La modificación de variables con el prefijo **Internal** produce efectos no deseados.

|Variable | Descripción|
|---------|------------|
|External_AutoStop_Condition | El operador condicional necesario para configurar la condición antes de desencadenar una alerta. Los valores que se aceptan son **GreaterThan**, **GreaterThanOrEqual**, **LessThan** y **LessThanOrEqual**.|
|External_AutoStop_Description | Alerta para detener la máquina virtual si el porcentaje de CPU supera el umbral.|
|External_AutoStop_MetricName | El nombre de la métrica de rendimiento para el que se configura la regla de alertas de Azure.|
|External_AutoStop_Threshold | Umbral para la regla de alertas de Azure especificada en la variable _External_AutoStop_MetricName_. Los valores de porcentaje pueden oscilar entre 1 y 100.|
|External_AutoStop_TimeAggregationOperator | El operador de agregación de tiempo, que se aplica al tamaño de la ventana seleccionada para evaluar la condición. Los valores que se aceptan son **Promedio**, **Mínimo**, **Máximo**, **Total** y **Último**.|
|External_AutoStop_TimeWindow | El tamaño de la ventana en la que Azure analiza la métrica seleccionada para desencadenar una alerta. Este parámetro acepta la entrada en formato timespan. Los valores posibles son de 5 minutos a 6 horas.|
|External_EnableClassicVMs| Especifica si la solución se centra en las máquinas virtuales clásicas. El valor predeterminado es True. Este debe establecerse en False para las suscripciones de CSP. Las máquinas virtuales clásicas requieren una [cuenta de ejecución clásica](automation-create-standalone-account.md#classic-run-as-accounts).|
|External_ExcludeVMNames | Escriba los nombres de máquina virtual que se van a excluir y sepárelos con una coma sin espacios en blanco. Esto se limita a 140 VM. Si agrega más de 140 VM a esta lista separada por comas, las VM establecidas para excluirse pueden iniciarse o cerrarse accidentalmente.|
|External_Start_ResourceGroupNames | Especifica uno o varios grupos de recursos y separa los valores con una coma, destinados a las acciones de inicio.|
|External_Stop_ResourceGroupNames | Especifica uno o varios grupos de recursos y separa los valores con una coma, destinados a las acciones de detención.|
|Internal_AutomationAccountName | Especifica el nombre de la cuenta de Automation.|
|Internal_AutoSnooze_WebhookUri | Especifica el URI de Webhook llamado para el escenario de AutoStop.|
|Internal_AzureSubscriptionId | Especifica el identificador de la suscripción de Azure.|
|Internal_ResourceGroupName | Especifica el nombre el grupo de recursos de la cuenta de Automation.|

En todos los escenarios, las variables **External_Start_ResourceGroupNames**, **External_Stop_ResourceGroupNames** y **External_ExcludeVMNames** son necesarias para dirigirse a las máquinas virtuales, aunque hay que proporcionar una lista separada por comas de las máquinas virtuales para los runbooks **AutoStop_CreateAlert_Parent**, **SequencedStartStop_Parent** y **ScheduledStartStop_Parent**. Es decir, para que se realicen acciones de iniciar o detener tengan, las máquinas virtuales deben residir en el recurso de destino. La lógica funciona de forma similar a la directiva de Azure, en el sentido de que el destino puede ser la suscripción o el grupo de recursos, y las acciones las pueden heredar las máquinas virtuales recién creadas. Este enfoque evita tener que mantener una programación independiente para cada máquina virtual y administrar los inicios y detenciones en escala.

### <a name="schedules"></a>Programaciones

En la tabla siguiente se enumera cada una de las programaciones predeterminadas que se crean en su cuenta de Automation. Puede modificarlas o crear sus propias programaciones personalizadas. De forma predeterminada, todas las programaciones están deshabilitadas, excepto **Scheduled_StartVM** y **Scheduled_StopVM**.

No se deben habilitar todas las programaciones, ya que ello podría crear acciones de programación que se superpongan. Lo mejor es determinar las optimizaciones que se desean y realizar las modificaciones oportunas. Vea los escenarios de ejemplo en la sección de información general para obtener una explicación.

|Nombre de programación | Frecuencia | Descripción|
|--- | --- | ---|
|Schedule_AutoStop_CreateAlert_Parent | Cada ocho horas | Ejecuta el runbook AutoStop_CreateAlert_Parent cada ocho horas, que a su vez detiene los valores basados en la máquina virtual en External_Start_ResourceGroupNames, External_Stop_ResourceGroupNames y External_ExcludeVMNames en las variables de Azure Automation. Como alternativa, puede especificar una lista separada por comas de las máquinas virtuales mediante el parámetro VMList.|
|Scheduled_StopVM | Definido por el usuario, diario | Ejecuta el runbook Scheduled_Parent con un parámetro de _Stop_ cada día a la hora especificada. Detiene automáticamente todas las máquinas virtuales que cumplan las reglas definidas por las variables de los recursos. Habilite la programación relacionada, **Scheduled-StartVM**.|
|Scheduled_StartVM | Definido por el usuario, diario | Ejecuta el runbook Scheduled_Parent con un parámetro de _Start_ cada día a la hora especificada. Inicia automáticamente todas las máquinas virtuales que cumplan las reglas definidas por las variables adecuadas. Habilite la programación relacionada, **Scheduled-StopVM**.|
|Sequenced-StopVM | 1:00 AM (UTC), todos los viernes | Ejecuta el runbook Sequenced_Parent con un parámetro de _Stop_ todos los viernes a la hora especificada. Detiene secuencialmente (de forma ascendente) todas las máquinas virtuales que tengan la etiqueta **SequenceStop** definida por las variables adecuadas. Para más información sobre los valores de etiqueta y las variables de recurso, consulte la sección Runbooks. Habilite la programación relacionada, **Sequenced-StartVM**.|
|Sequenced-StartVM | 1:00 PM (hora UTC), todos los lunes | Ejecuta el runbook Sequenced_Parent con un parámetro de _Start_ cada lunes a la hora determinada. Inicia secuencialmente (de forma descendente) todas las máquinas virtuales que tengan la etiqueta **SequenceStart** definida por las variables adecuadas. Para más información sobre los valores de etiqueta y las variables de recurso, consulte la sección Runbooks. Habilite la programación relacionada, **Sequenced-StopVM**.|

## <a name="azure-monitor-logs-records"></a>Registros de Azure Monitor

Automation crea dos tipos de registros en el área de trabajo de Log Analytics: registros de trabajo y flujos de trabajo.

### <a name="job-logs"></a>Registros de trabajo

|Propiedad | Descripción|
|----------|----------|
|Autor de llamada |  La persona que inicia la operación. Los valores posibles son una dirección de correo electrónico o el sistema para los trabajos programados.|
|Category | Clasificación del tipo de datos. Para Automation, el valor será JobLogs.|
|CorrelationId | GUID que es el identificador de correlación del trabajo de Runbook.|
|JobId | GUID que es el identificador del trabajo de Runbook.|
|operationName | Especifica el tipo de operación realizada en Azure. En Automation, el valor es Job.|
|resourceId | Especifica el tipo de recurso en Azure. Para Automation, el valor es la cuenta de Automation asociada al Runbook.|
|ResourceGroup | Especifica el nombre del grupo de recursos del trabajo de Runbook.|
|ResourceProvider | Especifica el servicio de Azure que proporciona los recursos que puede implementar y administrar. Para Automation, el valor es Azure Automation.|
|ResourceType | Especifica el tipo de recurso en Azure. Para Automation, el valor es la cuenta de Automation asociada al Runbook.|
|resultType | El estado del trabajo de Runbook. Los valores posibles son:<br>Started<br>Stopped<br>Suspended<br>Con error<br>- Correcto|
|resultDescription | Describe el estado de resultado del trabajo de Runbook. Los valores posibles son:<br>- Se inicia el trabajo<br>- Error del trabajo<br>- Trabajo completado|
|RunbookName | Especifica el nombre del Runbook.|
|SourceSystem | Especifica el sistema de origen para los datos enviados. En Automation, el valor es OpsManager|
|StreamType | Especifica el tipo de evento. Los valores posibles son:<br>- Detallado<br>- Salida<br>Error<br>Warning (Advertencia)|
|SubscriptionId | Especifica el identificador de suscripción del trabajo.
|Time | Fecha y hora en que se ejecuta el trabajo de Runbook.|

### <a name="job-streams"></a>Transmisiones de trabajo

|Propiedad | Descripción|
|----------|----------|
|Autor de llamada |  La persona que inicia la operación. Los valores posibles son una dirección de correo electrónico o el sistema para los trabajos programados.|
|Category | Clasificación del tipo de datos. Para Automation, el valor es JobStreams.|
|JobId | GUID que es el identificador del trabajo de Runbook.|
|operationName | Especifica el tipo de operación realizada en Azure. En Automation, el valor es Job.|
|ResourceGroup | Especifica el nombre del grupo de recursos del trabajo de Runbook.|
|resourceId | Especifica el identificador de recurso en Azure. Para Automation, el valor es la cuenta de Automation asociada al Runbook.|
|ResourceProvider | Especifica el servicio de Azure que proporciona los recursos que puede implementar y administrar. Para Automation, el valor es Azure Automation.|
|ResourceType | Especifica el tipo de recurso en Azure. Para Automation, el valor es la cuenta de Automation asociada al Runbook.|
|resultType | El resultado del trabajo de Runbook en el momento en que se generó el evento. Un valor posible es:<br>- En curso|
|resultDescription | Incluye la secuencia de salida del Runbook.|
|RunbookName | El nombre del Runbook.|
|SourceSystem | Especifica el sistema de origen para los datos enviados. En Automation, el valor es OpsManager.|
|StreamType | El tipo de flujo de trabajo. Los valores posibles son:<br>- Progreso<br>- Salida<br>Warning (Advertencia)<br>Error<br>DEBUG<br>- Detallado|
|Time | Fecha y hora en que se ejecuta el trabajo de Runbook.|

Cuando se realiza cualquier búsqueda de registros que devuelve registros de la categoría **JobLogs** o **JobStreams**, puede seleccionar las vistas **JobLogs** o **JobStreams**, que muestran un conjunto de iconos que resumen las actualizaciones devueltas en la búsqueda.

## <a name="sample-log-searches"></a>Búsquedas de registros de ejemplo

En la tabla siguiente se proporcionan búsquedas de registros de ejemplo para los registros de trabajo recopilados por esta solución.

|Consultar | Descripción|
|----------|----------|
|Búsqueda de trabajos del runbook ScheduledStartStop_Parent que hayan finalizado correctamente | <code>search Category == "JobLogs" <br>&#124;  where ( RunbookName_s == "ScheduledStartStop_Parent" ) <br>&#124;  where ( ResultType == "Completed" )  <br>&#124;  summarize AggregatedValue = count() by ResultType, bin(TimeGenerated, 1h) <br>&#124;  sort by TimeGenerated desc</code>|
|Búsqueda de trabajos del runbook SequencedStartStop_Parent que hayan finalizado correctamente | <code>search Category == "JobLogs" <br>&#124;  where ( RunbookName_s == "SequencedStartStop_Parent" ) <br>&#124;  where ( ResultType == "Completed" ) <br>&#124;  summarize AggregatedValue = count() by ResultType, bin(TimeGenerated, 1h) <br>&#124;  sort by TimeGenerated desc</code>|

## <a name="viewing-the-solution"></a>Visualización de la solución

Para tener acceso a la solución, navegue a su cuenta de Automation y seleccione **Área de trabajo** en **RECURSOS RELACIONADOS**. En la página de Log Analytics, seleccione **Soluciones** en **GENERAL**. En la página **Soluciones**, seleccione la solución **Start-Stop-VM[workspace]** en la lista.

Al seleccionar la solución se muestra la página de la solución **Start-Stop-VM[workspace]** . En él puede examinar detalles importantes como el icono **StartStopVM**. Al igual que en el área de trabajo de Log Analytics, este icono muestra un número y una representación gráfica de los trabajos de runbook de la solución que se han iniciado y han finalizado correctamente.

![Página de la solución Update Management de Automation](media/automation-solution-vm-management/azure-portal-vmupdate-solution-01.png)

Desde aquí puede realizar más análisis de los registros de trabajos. Para ello, solo debe hacer clic en el icono del anillo. El panel de la solución muestra el historial de trabajos y las consultas de búsqueda de registros predefinidas. Cambie al portal avanzado de Log Analytics para realizar búsquedas basadas en sus consultas de búsqueda.

## <a name="configure-email-notifications"></a>Configuración de notificaciones de correo electrónico

Para cambiar las notificaciones de correo electrónico después de implementar la solución, modifique el grupo de acciones creado durante la implementación.  

> [!NOTE]
> Las suscripciones en la nube de Azure Government no admiten la funcionalidad de correo electrónico de esta solución.

En Azure Portal, vaya a Monitor -> Grupos de acciones. Seleccione el grupo de acciones denominado **StartStop_VM_Notication**.

![Página de la solución Update Management de Automation](media/automation-solution-vm-management/azure-monitor.png)

En la página **StartStop_VM_Notification**, haga clic en **Editar detalles** en **Detalles**. Se abre la página **Email/SMS/Push/Voice** (Correo electrónico/SMS/Inserción/Voz). Actualice la dirección de correo electrónico y haga clic en **Aceptar** para guardar los cambios.

![Página de la solución Update Management de Automation](media/automation-solution-vm-management/change-email.png)

También puede agregar acciones adicionales al grupo de acciones; para más información sobre grupos de acciones, consulte [grupos de acciones](../azure-monitor/platform/action-groups.md)

El siguiente es un correo electrónico de ejemplo que se envía cuando la solución cierra las máquinas virtuales.

![Página de la solución Update Management de Automation](media/automation-solution-vm-management/email.png)

## <a name="addexclude-vms"></a><a name="add-exclude-vms"></a>Agregar o excluir VM

La solución proporciona la capacidad de agregar VM como destino de la solución o excluir específicamente las máquinas de la solución.

### <a name="add-a-vm"></a>Agregar una VM

Hay un par de opciones que puede usar para asegurarse de que una VM se incluye en la solución Start/Stop cuando se ejecuta.

* Cada uno de los [runbooks](#runbooks) primarios de la solución tiene un parámetro **VMList**. Puede pasar una lista separada por comas de nombres de VM a este parámetro al programar el runbook primario adecuado para su situación, y estas VM se incluirán cuando se ejecute la solución.

* Para seleccionar varias VM, establezca **External_Start_ResourceGroupNames** y **External_Stop_ResourceGroupNames** con los nombres de los grupos de recursos que contienen las VM que quiere iniciar o detener. También se puede establecer este valor como `*`, para que la solución se ejecute en todos los grupos de recursos de la suscripción.

### <a name="exclude-a-vm"></a>Excluir una VM

Para excluir una VM de la solución, puede agregarla a la variable **External_ExcludeVMNames**. Esta variable es una lista separada por comas de VM específicas que se excluirán de la solución Start/Stop. Esta lista se limita a 140 VM. Si agrega más de 140 VM a esta lista separada por comas, las VM establecidas para excluirse pueden iniciarse o cerrarse accidentalmente.

## <a name="modify-the-startup-and-shutdown-schedules"></a>Modificación de las programaciones de inicio y apagado

La administración de las programaciones de inicio y apagado en esta solución sigue los mismos pasos descritos en [Programación de un runbook en Azure Automation](automation-schedules.md). Debe haber una programación independiente para iniciar y detener las VM.

Se admite la configuración de la solución para simplemente detener las máquinas virtuales en un momento determinado. En este escenario, creará simplemente una programación **Stop** y ninguna programación **Start** correspondiente. Para ello, necesita lo siguiente:

1. Asegúrese de que ha agregado los grupos de recursos para que las máquinas virtuales se cierren en la variable **External_Stop_ResourceGroupNames**.
2. Cree su propia programación para el tiempo que desea apagar las máquinas virtuales.
3. Navegue hasta el runbook **ScheduledStartStop_Parent** y haga clic en **Programación**. Esto le permite seleccionar la programación que creó en el paso anterior.
4. Seleccione **Parameters and run settings** (Parámetros y configuración de ejecución) y establezca el parámetro ACTION en "Stop".
5. Haga clic en **Aceptar** para guardar los cambios.

## <a name="update-the-solution"></a>Actualización de la solución

Si ha implementado una versión anterior de esta solución, debe eliminarla de la cuenta antes de implementar una versión actualizada. Siga los pasos para [quitar la solución](#remove-the-solution) y luego siga los pasos anteriores para [implementarla](#deploy-the-solution).

## <a name="remove-the-solution"></a>Eliminación de la solución

Si decide que ya no necesita usar la solución, puede eliminarla de la cuenta de Automation. La eliminación de la solución solo quita los runbooks. No elimina las programaciones ni las variables que se crearon cuando se agregó la solución. Esos recursos se deben eliminar manualmente si no se usan con otros runbooks.

Para eliminar la solución, realice los siguientes pasos.

1. En su cuenta de Automation, en **Recursos relacionados**, seleccione **Área de trabajo vinculada**.
1. Seleccione **Ir al área de trabajo**.
1. En **General**, seleccione **Soluciones**. 
1. En la página **Soluciones**, seleccione la solución **Start-Stop-VM[Workspace]** . En la página **VMManagementSolution[Workspace]** , en el menú, seleccione **Eliminar**.<br><br> ![Eliminación de la solución de administración de máquinas virtuales](media/automation-solution-vm-management/vm-management-solution-delete.png)
1. En la ventana **Eliminar solución**, confirme que desea eliminar la solución.
1. Mientras se comprueba la información y se elimina la solución, puede realizar un seguimiento de su progreso en **Notificaciones** en el menú. Cuando comience el proceso para eliminar la solución, se le devuelve a la página **Soluciones**.

No se eliminarán la cuenta de Automation ni el área de trabajo de Log Analytics como parte de este proceso. Si no desea conservar el área de trabajo de Log Analytics, debe eliminarla manualmente, lo que se puede hacer desde Azure Portal:

1. En Azure Portal, busque y seleccione **Áreas de trabajo de Log Analytics**.
1. En la página **Áreas de trabajo de Log Analytics**, seleccione el área de trabajo.
1. Seleccione **Eliminar** en el menú de la página de configuración del área de trabajo.

Si no desea conservar los componentes de la cuenta de Azure Automation, puede eliminarlos manualmente. Para obtener la lista de los runbooks, las variables y las programaciones creadas por la solución, consulte los [componentes de la solución](#solution-components).

## <a name="next-steps"></a>Pasos siguientes

- Para aprender a crear diferentes consultas de búsqueda y a revisar los registros de trabajos de Automation con los registros de Azure Monitor, consulte [Búsquedas de registros en los registros de Azure Monitor](../log-analytics/log-analytics-log-searches.md).
- Para más información acerca de la ejecución de un runbook, cómo supervisar trabajos del runbook y otros detalles técnicos, consulte [Ejecución de un runbook en Automatización de Azure](automation-runbook-execution.md).
- Para obtener más información sobre los registros de Azure Monitor y los orígenes de recopilación de datos, consulte [Introducción a la recopilación de datos de almacenamiento en los registros de Azure Monitor](../azure-monitor/platform/collect-azure-metrics-logs.md).
