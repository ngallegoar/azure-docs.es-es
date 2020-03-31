---
title: Traslado de la cuenta de Azure Automation a otra suscripción
description: En este artículo se describe cómo mover la cuenta de Automation a otra suscripción.
services: automation
ms.service: automation
ms.subservice: process-automation
author: mgoedtel
ms.author: magoedte
ms.date: 03/11/2019
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: 1aa759a2984764169eb28935e095d0f7c0f90c08
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "75969832"
---
# <a name="move-your-azure-automation-account-to-another-subscription"></a>Traslado de la cuenta de Azure Automation a otra suscripción

Azure ofrece la posibilidad de mover algunos recursos a un nuevo grupo de recursos o a una nueva suscripción. Puede mover recursos mediante Azure Portal, PowerShell, la CLI de Azure o la API REST. Para más información sobre el proceso, consulte [Movimiento de recursos a un grupo de recursos o suscripción nuevos](../../azure-resource-manager/management/move-resource-group-and-subscription.md).

Las cuentas de Azure Automation son uno de los recursos que se pueden mover. En este artículo, conocerá los pasos para mover cuentas de Automation a otro recurso o suscripción.

Los pasos generales para mover la cuenta de Automation son:

1. Quite sus soluciones.
2. Desvincule el área de trabajo.
3. Mueva la cuenta de Automation.
4. Elimine las cuentas de ejecución y vuelva a crearlas.
5. Vuelva a habilitar las soluciones.

## <a name="remove-solutions"></a>Eliminación de las soluciones

Para desvincular área de trabajo de la cuenta de Automation, se deben eliminar estas soluciones del área de trabajo:
- **Change Tracking e Inventario**
- **Administración de actualizaciones**
- **Iniciar/detener las VM fuera de las horas de trabajo**

En el grupo de recursos, busque cada solución y seleccione **Eliminar**. En la página **Eliminar recursos**, confirme los recursos que quiere quitar y seleccione **Eliminar**.

![Eliminación de soluciones de Azure Portal](../media/move-account/delete-solutions.png)

Puede realizar la misma tarea con el cmdlet [Remove-AzureRmResource](/powershell/module/azurerm.resources/remove-azurermresource), como se muestra en el ejemplo siguiente:

```azurepowershell-interactive
$workspaceName = <myWorkspaceName>
$resourceGroupName = <myResourceGroup>
Remove-AzureRmResource -ResourceType 'Microsoft.OperationsManagement/solutions' -ResourceName "ChangeTracking($workspaceName)" -ResourceGroupName $resourceGroupName
Remove-AzureRmResource -ResourceType 'Microsoft.OperationsManagement/solutions' -ResourceName "Updates($workspaceName)" -ResourceGroupName $resourceGroupName
Remove-AzureRmResource -ResourceType 'Microsoft.OperationsManagement/solutions' -ResourceName "Start-Stop-VM($workspaceName)" -ResourceGroupName $resourceGroupName
```

### <a name="additional-steps-for-startstop-vms"></a>Pasos adicionales para iniciar o detener máquinas virtuales

Para la solución **Start/Stop VMs**, también debe quitar las reglas de alerta creadas por la solución.

En Azure Portal, vaya al grupo de recursos y seleccione **Supervisión** > **Alertas** > **Administrar reglas de alerta**.

![Página de alertas que muestra la selección de Administrar reglas de alerta](../media/move-account/alert-rules.png)

En la página **Reglas**, verá una lista de las alertas configuradas en ese grupo de recursos. La solución **Start/Stop VMs** crea tres reglas de alerta:

* AutoStop_VM_Child
* ScheduledStartStop_Parent
* SequencedStartStop_Parent

Seleccione las tres reglas de alerta y, luego, **Eliminar**. Esta acción quitará estas reglas de alerta.

![Página de reglas que solicita confirmación de eliminación de las reglas seleccionadas](../media/move-account/delete-rules.png)

> [!NOTE]
> Si no ve ninguna regla de alerta en la página **Reglas**, cambie el **estado** para mostrar las alertas **deshabilitadas**, ya que es posible que las haya deshabilitado.

Cuando se quiten las reglas de alerta, quite el grupo de acciones que se creó para las notificaciones de la solución **Start/Stop VMs**.

En Azure Portal, seleccione **Monitor** > **Alertas** > **Administrar los grupos de acciones**.

Seleccione **StartStop_VM_Notification** en la lista. En la página del grupo de acciones, seleccione **Eliminar**.

![Página del grupo de acciones, selección de Eliminar](../media/move-account/delete-action-group.png)

De forma similar, puede eliminar el grupo de acciones mediante PowerShell con el cmdlet [Remove-AzureRmActionGroup](/powershell/module/azurerm.insights/remove-azurermactiongroup), tal como se muestra en el ejemplo siguiente:

```azurepowershell-interactive
Remove-AzureRmActionGroup -ResourceGroupName <myResourceGroup> -Name StartStop_VM_Notification
```

## <a name="unlink-your-workspace"></a>Desvinculación del área de trabajo

En Azure Portal, seleccione **Cuenta de Automation** > **Recursos relacionados** > **Área de trabajo vinculada**. Seleccione **Desvincular área de trabajo** para desvincular el área de trabajo de la cuenta de Automation.

![Desvinculación de un área de trabajo de una cuenta de Automation](../media/move-account/unlink-workspace.png)

## <a name="move-your-automation-account"></a>Traslado de la cuenta de Automation

Después de quitar los elementos anteriores, puede continuar y quitar la cuenta de Automation y sus runbooks. En Azure Portal, vaya al grupo de recursos de la cuenta de Automation. Seleccione **Mover** > **Mover a otra suscripción**.

![Página del grupo de recursos, traslado a otra suscripción](../media/move-account/move-resources.png)

Seleccione los recursos del grupo de recursos que quiere mover. Asegúrese de incluir los recursos **cuenta de Automation**, **runbook** y **área de trabajo de Log Analytics**.

Una vez completado el traslado, existen pasos adicionales necesarios para conseguir que todo funcione.

## <a name="re-create-run-as-accounts"></a>Volver a crear las cuentas de ejecución

Las [cuentas de ejecución](../manage-runas-account.md) crean una entidad de servicio en Azure Active Directory para autenticarse con recursos de Azure. Al cambiar las suscripciones, la cuenta de Automation ya no usa la cuenta de ejecución existente.

Vaya a la cuenta de Automation en la nueva suscripción y seleccione **Cuentas de ejecución** en **Configuración de la cuenta**. Verá que las cuentas de ejecución se muestran ahora como incompletas.

![Cuentas de ejecución incompletas](../media/move-account/run-as-accounts.png)

Seleccione cada una de ellas. En la página **Propiedades**, seleccione **Eliminar** para eliminar la cuenta de ejecución.

> [!NOTE]
> Si no tiene permisos para crear o ver las cuentas de ejecución, verá el siguiente mensaje: `You do not have permissions to create an Azure Run As account (service principal) and grant the Contributor role to the service principal.` Para conocer los permisos necesarios para configurar una cuenta de ejecución, consulte [Permisos para configurar cuentas de ejecución](../manage-runas-account.md#permissions).

Después de eliminan las cuentas de ejecución, seleccione **Crear** en **Cuenta de ejecución de Azure**. En la página **Agregar cuenta de ejecución de Azure**, seleccione **Crear** para crear la cuenta de ejecución y la entidad de servicio. Repita los pasos anteriores con la **cuenta de ejecución de Azure**.

## <a name="enable-solutions"></a>Habilitar soluciones

Después de volver a crear las cuentas de ejecución, habilitará de nuevo las soluciones que quitara antes del traslado. Para activar **Change Tracking e Inventario** y **Update Management**, seleccione la funcionalidad respectiva en la cuenta de Automation. Elija el área de trabajo de Log Analytics que trasladó y seleccione **Habilitar**.

![Nueva habilitación de las soluciones en la cuenta de Automation trasladada](../media/move-account/reenable-solutions.png)

Las máquinas que incorporó con sus soluciones serán visibles cuando se haya conectado al área de trabajo de Log Analytics existente.

Para activar la solución **Start/Stop VMs during off-hours**, deberá volver a implementar la solución. En **Recursos relacionados**, seleccione **Start/Stop VMs** > **Más información acerca de la solución y cómo habilitarla** > **Crear** para iniciar la implementación.

En la página **Agregar solución**, elija el área de trabajo de Log Analytics y la cuenta de Automation.

![Menú Agregar solución](../media/move-account/add-solution-vm.png)

Para instrucciones detalladas sobre cómo configurar la solución, consulte [Start/Stop VMs during off-hours solution en Azure Automation](../automation-solution-vm-management.md).

## <a name="post-move-verification"></a>Comprobación posterior al traslado

Una vez finalizado el traslado, compruebe la siguiente lista de tareas que deben comprobarse:

|Capacidad|Pruebas|Vínculo de solución de problemas|
|---|---|---|
|Runbooks|Un runbook puede ejecutar recursos de Azure y conectarse a ellos correctamente.|[Solución de problemas de runbooks](../troubleshoot/runbooks.md)
|Control de código fuente|Puede ejecutar una sincronización manual en el repositorio de control de código fuente.|[Integración de control de código fuente](../source-control-integration.md)|
|Change Tracking e Inventario|Compruebe que puede ver los datos del inventario actual en las máquinas.|[Solución de problemas de Change Tracking](../troubleshoot/change-tracking.md)|
|Administración de actualizaciones|Asegúrese de que ve las máquinas y están en buen estado.</br>Ejecute una implementación de actualizaciones de software de prueba.|[Solución de problemas de Update management](../troubleshoot/update-management.md)|
|Recursos compartidos|Verifique que ve todos los recursos compartidos, como [Credenciales ](../shared-resources/credentials.md), [Variables](../shared-resources/variables.md), etc.|

## <a name="next-steps"></a>Pasos siguientes

Para más información sobre el traslado de recursos en Azure, consulte [Traslado de recursos de Azure](../../azure-resource-manager/management/move-support-resources.md).
