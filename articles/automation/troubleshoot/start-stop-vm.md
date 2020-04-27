---
title: Solución de problemas de la solución Start/Stop VMs during off-hours
description: Este artículo contiene información sobre la solución de problemas de la solución Start/Stop VMs during off-hours.
services: automation
ms.service: automation
ms.subservice: process-automation
author: mgoedtel
ms.author: magoedte
ms.date: 04/04/2019
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: 003c2c5a2c09957e7a3a4ac0a26b87a9ac43dace
ms.sourcegitcommit: acb82fc770128234f2e9222939826e3ade3a2a28
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/21/2020
ms.locfileid: "81679163"
---
# <a name="troubleshoot-the-startstop-vms-during-off-hours-solution"></a>Solución de problemas de la solución Start/Stop VMs during off-hours

En este artículo se proporciona información sobre la solución de problemas que surgen al utilizar la solución Start/Stop VMs during off-hours.

>[!NOTE]
>Este artículo se ha actualizado para usar el nuevo módulo Az de Azure PowerShell. Aún puede usar el módulo de AzureRM que continuará recibiendo correcciones de errores hasta diciembre de 2020 como mínimo. Para más información acerca del nuevo módulo Az y la compatibilidad con AzureRM, consulte [Introducing the new Azure PowerShell Az module](https://docs.microsoft.com/powershell/azure/new-azureps-module-az?view=azps-3.5.0) (Presentación del nuevo módulo Az de Azure PowerShell). Para obtener instrucciones sobre la instalación del módulo Az en Hybrid Runbook Worker, consulte [Instalación del módulo de Azure PowerShell](https://docs.microsoft.com/powershell/azure/install-az-ps?view=azps-3.5.0). Puede actualizar los módulos de su cuenta de Automation a la versión más reciente mediante [Actualización de módulos de Azure PowerShell en Azure Automation](../automation-update-azure-modules.md).

## <a name="scenario-the-startstop-vms-during-off-hours-solution-fails-to-properly-deploy"></a><a name="deployment-failure"></a>Escenario: no se puede implementar correctamente la solución Start/Stop VMs during off hours

### <a name="issue"></a>Problema

Al implementar la [solución Start/Stop VM during off hours](../automation-solution-vm-management.md), se producirá uno de los siguientes errores:

```error
Account already exists in another resourcegroup in a subscription. ResourceGroupName: [MyResourceGroup].
```

```error
Resource 'StartStop_VM_Notification' was disallowed by policy. Policy identifiers: '[{\\\"policyAssignment\\\":{\\\"name\\\":\\\"[MyPolicyName]".
```

```error
The subscription is not registered to use namespace 'Microsoft.OperationsManagement'.
```

```error
The subscription is not registered to use namespace 'Microsoft.Insights'.
```

```error
The scope '/subscriptions/000000000000-0000-0000-0000-00000000/resourcegroups/<ResourceGroupName>/providers/Microsoft.OperationalInsights/workspaces/<WorkspaceName>/views/StartStopVMView' cannot perform write operation because following scope(s) are locked: '/subscriptions/000000000000-0000-0000-0000-00000000/resourceGroups/<ResourceGroupName>/providers/Microsoft.OperationalInsights/workspaces/<WorkspaceName>/views/StartStopVMView'. Please remove the lock and try again
```

```error
A parameter cannot be found that matches parameter name 'TagName'
```

```error
Start-AzureRmVm : Run Login-AzureRmAccount to login
```

### <a name="cause"></a>Causa

Se pueden producir errores de implementación por uno de los siguientes motivos:

1. Ya hay una cuenta de Automation con el mismo nombre en la región seleccionada.
2. Hay una directiva que no permite la implementación de la solución Start/Stop VMs during off hours.
3. El tipo de recurso `Microsoft.OperationsManagement`, `Microsoft.Insights` o `Microsoft.Automation` no está registrado.
4. El área de trabajo de Log Analytics está bloqueada.
5. Tiene una versión desfasada de los módulos de AzureRM o la solución Start/Stop VMs during off hours.

### <a name="resolution"></a>Solución

Revise las siguientes correcciones para conocer las posibles soluciones a su problema:

* Las cuentas de Automation deben ser únicas en una misma región de Azure, incluso si se encuentran en distintos grupos de recursos. Compruebe sus cuentas de Automation existentes en la región de destino.
* Una directiva existente impide que se implemente un recurso necesario para la solución Start/Stop VMs during off hours. Vaya a las asignaciones de directivas en Azure Portal y compruebe si tiene una asignación de directiva que no permite la implementación de este recurso. Para más información acerca de esto, consulte [RequestDisallowedByPolicy](../../azure-resource-manager/templates/error-policy-requestdisallowedbypolicy.md).
* Para implementar la solución Start/Stop VMs during off hours, la suscripción debe estar registrada en los siguientes espacios de nombres de recursos de Azure:

    * `Microsoft.OperationsManagement`
    * `Microsoft.Insights`
    * `Microsoft.Automation`

   Consulte [Resolución de errores del registro del proveedor de recursos](../../azure-resource-manager/templates/error-register-resource-provider.md) para más información acerca de errores al registrar los proveedores.
* Si tiene un bloqueo en el área de trabajo de Log Analytics, vaya al área de trabajo de Azure Portal y quite los bloqueos en el recurso.
* Si las soluciones anteriores no resuelven su problema, siga las instrucciones que aparecen en [Actualizar la solución](../automation-solution-vm-management.md#update-the-solution) para volver a implementar la solución de inicio o detención.

## <a name="scenario-all-vms-fail-to-start-or-stop"></a><a name="all-vms-fail-to-startstop"></a>Escenario: error al iniciar o detener todas las VM

### <a name="issue"></a>Problema

Ha configurado la solución Start/Stop VMs during off hours, pero esta no inicia o detiene todas las VM.

### <a name="cause"></a>Causa

Este error puede deberse a alguna de las siguientes razones:

1. Una programación no está configurada correctamente.
2. Es posible que la cuenta de ejecución no esté configurada correctamente.
3. Es posible que un runbook presente errores.
4. Es posible que las VM se hayan excluido.

### <a name="resolution"></a>Solución

Revise la lista siguiente para conocer las posibles soluciones a su problema:

* Compruebe que ha configurado correctamente una programación para la solución Start/Stop VMs during off hours. Para más información sobre cómo configurar una programación, vea el artículo sobre [programación](../automation-schedules.md).

* Compruebe los [flujos de trabajo](../automation-runbook-execution.md#viewing-job-status-from-the-azure-portal) para buscar errores. Busque los trabajos de alguno de los siguientes runbooks:

  * **AutoStop_CreateAlert_Child**
  * **AutoStop_CreateAlert_Parent**
  * **AutoStop_Disable**
  * **AutoStop_VM_Child**
  * **ScheduledStartStop_Base_Classic**
  * **ScheduledStartStop_Child_Classic**
  * **ScheduledStartStop_Child**
  * **ScheduledStartStop_Parent**
  * **SequencedStartStop_Parent**

* Verifique que su [cuenta de ejecución](../manage-runas-account.md) tiene los permisos adecuados para las VM que intenta iniciar o detener. Para obtener información sobre cómo comprobar los permisos para un recurso, vea [Guía de inicio rápido: Visualización de los roles asignados a un usuario mediante Azure Portal](../../role-based-access-control/check-access.md). Deberá proporcionar el identificador de aplicación de la entidad de servicio usada por la cuenta de ejecución. Para obtener este valor, vaya a la cuenta de Automation en Azure Portal, seleccione **Cuentas de ejecución** en **Configuración de cuenta** y haga clic en la cuenta de ejecución apropiada.

* Las máquinas virtuales no pueden iniciarse o detenerse en caso de que se hayan excluido de forma explícita. Las VM excluidas se definen en la variable `External_ExcludeVMNames` en la cuenta de Automation en la que está implementada la solución. En el ejemplo siguiente se muestra cómo consultar ese valor con PowerShell.

  ```powershell-interactive
  Get-AzAutomationVariable -Name External_ExcludeVMNames -AutomationAccountName <automationAccountName> -ResourceGroupName <resourceGroupName> | Select-Object Value
  ```

## <a name="scenario-some-of-my-vms-fail-to-start-or-stop"></a><a name="some-vms-fail-to-startstop"></a>Escenario: Algunas máquinas virtuales no se pueden iniciar o detener

### <a name="issue"></a>Problema

Ha configurado la solución Start/Stop VMs during off hours, pero esta no inicia o detiene algunas de las VM configuradas.

### <a name="cause"></a>Causa

Este error puede deberse a alguna de las siguientes razones:

1. En el escenario en secuencia, es posible que una etiqueta falte o sea incorrecta.
2. Es posible que la VM esté excluida.
3. Es posible que la cuenta de ejecución no tenga permisos suficientes en la VM.
4. Es posible que la VM tenga algún problema que haya impedido su inicio o detención.

### <a name="resolution"></a>Solución

Revise la siguiente lista de posibles soluciones para su problema o para saber dónde buscar:

* Si usa el [escenario en secuencia](../automation-solution-vm-management.md) de la solución Start/Stop VMs during off-hours, debe asegurarse de que cada VM que desea iniciar o detener tiene la etiqueta correcta. Asegúrese de que las máquinas virtuales que desea iniciar tienen la etiqueta `sequencestart` y que aquellas que desea detener tienen la etiqueta `sequencestop`. Ambas etiquetas requieren un valor entero positivo. Puede usar una consulta similar a la del ejemplo siguiente para buscar todas las máquinas virtuales con las etiquetas y sus valores.

  ```powershell-interactive
  Get-AzResource | ? {$_.Tags.Keys -contains "SequenceStart" -or $_.Tags.Keys -contains "SequenceStop"} | ft Name,Tags
  ```

* Las VM no pueden iniciarse o detenerse en caso de que se hayan excluido de forma explícita. Las VM excluidas se definen en la variable `External_ExcludeVMNames` en la cuenta de Automation en la que está implementada la solución. En el ejemplo siguiente se muestra cómo consultar ese valor con PowerShell.

  ```powershell-interactive
  Get-AzAutomationVariable -Name External_ExcludeVMNames -AutomationAccountName <automationAccountName> -ResourceGroupName <resourceGroupName> | Select-Object Value
  ```

* Para iniciar y detener las VM, la cuenta de ejecución de la cuenta de Automation debe tener los permisos apropiados para la VM. Para obtener información sobre cómo comprobar los permisos para un recurso, vea [Guía de inicio rápido: Visualización de los roles asignados a un usuario mediante Azure Portal](../../role-based-access-control/check-access.md). Deberá proporcionar el identificador de aplicación de la entidad de servicio usada por la cuenta de ejecución. Para obtener este valor, vaya a la cuenta de Automation en Azure Portal, seleccione **Cuentas de ejecución** en **Configuración de cuenta** y haga clic en la cuenta de ejecución apropiada.

* Si la VM presenta algún problema para iniciarse o desasignarse, es posible que exista un problema en la propia VM. Por ejemplo, la aplicación en curso de una actualización al intentar apagar, el bloqueo de un servicio, etc. Vaya al recurso de máquina virtual y consulte los **registros de actividad** para ver si en ellos se ha identificado algún error. También puede intentar iniciar sesión en la VM para ver si existe algún error en los registros de eventos. Para más información sobre la solución de problemas de la máquina virtual, vea [Solución de problemas de máquinas virtuales de Azure](../../virtual-machines/troubleshooting/index.yml)

* Compruebe los [flujos de trabajo](../automation-runbook-execution.md#viewing-job-status-from-the-azure-portal) para buscar errores. En Azure Portal, vaya a la cuenta de Automation y seleccione **Trabajos** en **Automatización de procesos**.

## <a name="scenario-my-custom-runbook-fails-to-start-or-stop-my-vms"></a><a name="custom-runbook"></a>Escenario: Mi runbook personalizado no puede iniciar o detener mis máquinas virtuales

### <a name="issue"></a>Problema

Ha creado un runbook personalizado o ha descargado uno de la Galería de PowerShell, pero no funciona correctamente.

### <a name="cause"></a>Causa

El error puede deberse a muchas causas. Vaya a la cuenta de Automation en Azure Portal y seleccione **Trabajos** en **Automatización de procesos**. En la página Trabajos, busque los trabajos del runbook para ver cualquier error que puedan contener.

### <a name="resolution"></a>Solución

Se recomienda lo siguiente:

* Usar la [solución Start/Stop VMs during off-hours](../automation-solution-vm-management.md) para iniciar y detener las VM en Azure Automation. Esta solución la creó Microsoft. 

* Tener en cuenta que Microsoft no admite runbooks personalizados. Puede encontrar una solución para su runbook personalizado en [Solución de problemas relativos a errores con runbooks](runbooks.md). Compruebe los [flujos de trabajo](../automation-runbook-execution.md#viewing-job-status-from-the-azure-portal) para buscar errores. 

## <a name="scenario-vms-dont-start-or-stop-in-the-correct-sequence"></a><a name="dont-start-stop-in-sequence"></a>Escenario: Las máquinas virtuales no se inician o detienen en la secuencia correcta

### <a name="issue"></a>Problema

Las máquinas virtuales configuradas en la solución no se inician o detienen en la secuencia correcta.

### <a name="cause"></a>Causa

Este problema se debe a un etiquetado incorrecto de las VM.

### <a name="resolution"></a>Solución

Realice los pasos siguientes para asegurarse de que la solución está configurada correctamente.

1. Asegúrese de que todas las máquinas virtuales que se van a iniciar o detener tienen una etiqueta `sequencestart` o `sequencestop`, en función de su situación. Estas etiquetas necesitan un número entero positivo como valor. Las máquinas virtuales se procesan en orden ascendente según este valor.
2. Asegúrese de que los grupos de recursos de las máquinas virtuales que se van a iniciar o detener están en las variables `External_Start_ResourceGroupNames` o `External_Stop_ResourceGroupNames`, en función de su situación.
3. Pruebe los cambios mediante la ejecución del runbook `SequencedStartStop_Parent` con el parámetro `WHATIF` establecido en True para obtener una vista previa de dichos cambios.
4. Para más información sobre el uso de la solución para iniciar y detener las VM en secuencia, consulte [Iniciar o detener VM en secuencia](../automation-solution-vm-management.md).

## <a name="scenario-startstop-vms-during-off-hours-job-fails-with-403-forbidden-error"></a><a name="403"></a>Escenario: error de prohibido 403 en el trabajo Start/Stop VMs during off hours

### <a name="issue"></a>Problema

Puede encontrar trabajos con el error `403 forbidden` para los runbooks de la solución Start/Stop VMs during off-hours.

### <a name="cause"></a>Causa

Este problema puede deberse a una cuenta de ejecución mal configurada o expirada. La causa también puede ser que la cuenta de ejecución tenga permisos inadecuados para los recursos de VM.

### <a name="resolution"></a>Solución

Para comprobar si la cuenta de ejecución está configurada correctamente, vaya a la cuenta de Automation en Azure Portal y seleccione **Cuentas de ejecución** en **Configuración de la cuenta**. Si una cuenta de ejecución se ha configurado incorrectamente o ha expirado, el estado muestra la condición.

Si la cuenta de ejecución está mal configurada, debe eliminarla y volver a crearla. Consulte [Administración de cuentas de ejecución de Azure Automation](../manage-runas-account.md).

Si el certificado de la cuenta de ejecución ha expirado, siga los pasos de [Renovación de certificado autofirmado](../manage-runas-account.md#cert-renewal) para renovarlo.

Si faltan permisos, consulte [Inicio rápido: Visualización de los roles asignados a un usuario mediante Azure Portal](../../role-based-access-control/check-access.md). Debe proporcionar el identificador de aplicación de la entidad de servicio usada por la cuenta de ejecución. Para obtener este valor, vaya a la cuenta de Automation en Azure Portal, seleccione **Cuentas de ejecución** en **Configuración de cuenta** y haga clic en la cuenta de ejecución apropiada.

## <a name="scenario-my-problem-isnt-listed-above"></a><a name="other"></a>Escenario: Mi problema no se ha indicado anteriormente

### <a name="issue"></a>Problema

Experimenta un problema u obtiene un resultado inesperado al utilizar la solución Start/Stop VMs during off hours que no se indica en esta página.

### <a name="cause"></a>Causa

Muchas veces los errores pueden deberse a que se usa una versión anterior y obsoleta de la solución.

> [!NOTE]
> La solución Start/Stop VMs during off hours se ha probado con los módulos de Azure importados en su cuenta de Automation al implementar la solución. La solución no funciona actualmente con las versiones más recientes del módulo de Azure. Esto solo afecta a la cuenta de Automation que usa para ejecutar la solución Start/Stop VMs during off hours. Puede seguir usando las versiones más recientes del módulo de Azure en sus otras cuentas de Automation, tal como se describe en [Actualización de módulos de Azure PowerShell en Azure Automation](../automation-update-azure-modules.md).

### <a name="resolution"></a>Solución

Para resolver muchos errores, se recomienda quitar y [actualizar la solución Start/Stop VMs during off hours](../automation-solution-vm-management.md#update-the-solution). Además, puede comprobar los [flujos de trabajo](../automation-runbook-execution.md#viewing-job-status-from-the-azure-portal) para buscar errores. 

## <a name="next-steps"></a>Pasos siguientes

Si su problema no aparece arriba o no puede resolverlo, intente obtener ayuda adicional a través de uno de los siguientes canales:

* Obtenga respuestas de expertos de Azure en los [foros de Azure](https://azure.microsoft.com/support/forums/).
* Póngase en contacto con [@AzureSupport](https://twitter.com/azuresupport), la cuenta oficial de Microsoft Azure para mejorar la experiencia del cliente, que pone en contacto a la comunidad de Azure con los recursos adecuados: respuestas, soporte técnico y expertos.
* Registrar un incidente de soporte técnico de Azure. Vaya al [sitio de soporte técnico de Azure](https://azure.microsoft.com/support/options/) y seleccione **Obtener soporte**.