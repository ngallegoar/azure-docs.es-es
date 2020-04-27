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
ms.openlocfilehash: 2dbe7dc171b6e0ec81c99a460a4f997eeb9e27a5
ms.sourcegitcommit: acb82fc770128234f2e9222939826e3ade3a2a28
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/21/2020
ms.locfileid: "81681885"
---
# <a name="move-your-azure-automation-account-to-another-subscription"></a>Traslado de la cuenta de Azure Automation a otra suscripción

Azure Automation permite mover algunos recursos a un nuevo grupo de recursos o a una nueva suscripción. Puede mover recursos mediante Azure Portal, PowerShell, la CLI de Azure o la API REST. Para más información sobre el proceso, consulte [Movimiento de recursos a un grupo de recursos o suscripción nuevos](../../azure-resource-manager/management/move-resource-group-and-subscription.md).

La cuenta de Azure Automation es uno de los recursos que se pueden mover. En este artículo, conocerá el procedimiento para mover cuentas de Automation a otro recurso o suscripción. Los pasos generales para mover la cuenta de Automation son:

1. Quite sus soluciones.
2. Desvincule el área de trabajo.
3. Mueva la cuenta de Automation.
4. Elimine las cuentas de ejecución y vuelva a crearlas.
5. Vuelva a habilitar las soluciones.

>[!NOTE]
>Este artículo se ha actualizado para usar el nuevo módulo Az de Azure PowerShell. Aún puede usar el módulo de AzureRM que continuará recibiendo correcciones de errores hasta diciembre de 2020 como mínimo. Para más información acerca del nuevo módulo Az y la compatibilidad con AzureRM, consulte [Introducing the new Azure PowerShell Az module](https://docs.microsoft.com/powershell/azure/new-azureps-module-az?view=azps-3.5.0) (Presentación del nuevo módulo Az de Azure PowerShell). Para obtener instrucciones sobre la instalación del módulo Az en Hybrid Runbook Worker, consulte [Instalación del módulo de Azure PowerShell](https://docs.microsoft.com/powershell/azure/install-az-ps?view=azps-3.5.0). Puede actualizar los módulos de su cuenta de Automation a la versión más reciente mediante [Actualización de módulos de Azure PowerShell en Azure Automation](../automation-update-azure-modules.md).

## <a name="remove-solutions"></a>Eliminación de las soluciones

Para desvincular el área de trabajo de la cuenta de Automation, se deben quitar estas soluciones del área de trabajo:

- Change Tracking e Inventario
- Administración de actualizaciones
- Inicio y detención de máquinas virtuales fuera del horario laboral

1. Ubique el grupo de recursos en Azure Portal.
2. Busque cada una de las soluciones y haga clic en **Eliminar** en la página Eliminar recursos.

    ![Eliminación de soluciones de Azure Portal](../media/move-account/delete-solutions.png)

    Si lo prefiere, puede eliminar las soluciones mediante el cmdlet [Remove-AzResource](https://docs.microsoft.com/powershell/module/Az.Resources/Remove-AzResource?view=azps-3.7.0):

    ```azurepowershell-interactive
    $workspaceName = <myWorkspaceName>
    $resourceGroupName = <myResourceGroup>
    Remove-AzResource -ResourceType 'Microsoft.OperationsManagement/solutions' -ResourceName "ChangeTracking($workspaceName)" -ResourceGroupName $resourceGroupName
    Remove-AzResource -ResourceType 'Microsoft.OperationsManagement/solutions' -ResourceName "Updates($workspaceName)" -ResourceGroupName $resourceGroupName
    Remove-AzResource -ResourceType 'Microsoft.OperationsManagement/solutions' -ResourceName "Start-Stop-VM($workspaceName)" -ResourceGroupName $resourceGroupName
    ```

### <a name="remove-alert-rules-for-the-startstop-vms-during-off-hours-solution"></a>Eliminación de reglas de alerta para la solución Start/Stop VMs during off hours

Para la solución Start/Stop VMs during off hours, también debe quitar las reglas de alerta creadas por la solución.

1. En Azure Portal, vaya al grupo de recursos y seleccione **Supervisión** > **Alertas** > **Administrar reglas de alerta**.

![Página de alertas que muestra la selección de Administrar reglas de alerta](../media/move-account/alert-rules.png)

2. En la página Reglas, debería ver una lista de las alertas configuradas en ese grupo de recursos. La solución crea estas reglas:

    * AutoStop_VM_Child
    * ScheduledStartStop_Parent
    * SequencedStartStop_Parent

3. Seleccione las reglas de una en una y haga clic en **Eliminar** para quitarlas.

    ![Página de reglas que solicita confirmación de eliminación de las reglas seleccionadas](../media/move-account/delete-rules.png)

    > [!NOTE]
    > Si no ve ninguna regla de alerta en la página Reglas, cambie el campo **Estado** a Deshabilitado para mostrar las alertas deshabilitadas, ya que es posible que las haya deshabilitado.

4. Cuando se quiten las reglas de alerta, quite el grupo de acciones que se creó para las notificaciones de la solución Start/Stop VMs during off hours. En Azure Portal, seleccione **Monitor** > **Alertas** > **Administrar los grupos de acciones**.

5. Seleccione **StartStop_VM_Notification**. 

6. En la página del grupo de acciones, seleccione **Eliminar**.

    ![Página Grupo de acciones](../media/move-account/delete-action-group.png)

    Si lo prefiere, puede eliminar el grupo de acciones mediante el cmdlet [Remove-AzActionGroup](https://docs.microsoft.com/powershell/module/az.monitor/remove-azactiongroup?view=azps-3.7.0):

    ```azurepowershell-interactive
    Remove-AzActionGroup -ResourceGroupName <myResourceGroup> -Name StartStop_VM_Notification
    ```

## <a name="unlink-your-workspace"></a>Desvinculación del área de trabajo

Ahora puede desvincular el área de trabajo:

1. En Azure Portal, seleccione **Cuenta de Automation** > **Recursos relacionados** > **Área de trabajo vinculada**. 

2. Seleccione **Desvincular área de trabajo** para desvincular el área de trabajo de la cuenta de Automation.

    ![Desvinculación de un área de trabajo de una cuenta de Automation](../media/move-account/unlink-workspace.png)

## <a name="move-your-automation-account"></a>Traslado de la cuenta de Automation

Ahora puede mover la cuenta de Automation y sus runbooks. 

1. En Azure Portal, vaya al grupo de recursos de la cuenta de Automation. Seleccione **Mover** > **Mover a otra suscripción**.

    ![Página del grupo de recursos, traslado a otra suscripción](../media/move-account/move-resources.png)

2. Seleccione los recursos del grupo de recursos que quiere mover. Asegúrese de incluir los recursos de la cuenta de Automation, los runbooks y el área de trabajo de Log Analytics.

## <a name="recreate-run-as-accounts"></a>Volver a crear las cuentas de ejecución

Las [cuentas de ejecución](../manage-runas-account.md) crean una entidad de servicio en Azure Active Directory para autenticarse con recursos de Azure. Al cambiar las suscripciones, la cuenta de Automation ya no usa la cuenta de ejecución existente. Para volver a crear las cuentas de ejecución:

1. Vaya a la cuenta de Automation en la nueva suscripción y seleccione **Cuentas de ejecución** en **Configuración de la cuenta**. Verá que las cuentas de ejecución se muestran ahora como incompletas.

    ![Cuentas de ejecución incompletas](../media/move-account/run-as-accounts.png)

2. Elimine las cuentas de ejecución de una en una mediante el botón **Eliminar** de la página Propiedades. 

    > [!NOTE]
    > Si no tiene permisos para crear o ver las cuentas de ejecución, verá el siguiente mensaje: `You do not have permissions to create an Azure Run As account (service principal) and grant the Contributor role to the service principal.` Para conocer los permisos necesarios para configurar una cuenta de ejecución, consulte [Permisos para configurar cuentas de ejecución](../manage-runas-account.md#permissions).

3. Después de eliminar las cuentas de ejecución, seleccione **Crear** en **Cuenta de ejecución de Azure**. 

4. En la página Agregar cuenta de ejecución de Azure, seleccione **Crear** para crear la cuenta de ejecución y la entidad de servicio. 

5. Repita los pasos anteriores con la cuenta de ejecución de Azure clásico.

## <a name="enable-solutions"></a>Habilitar soluciones

Después de volver a crear las cuentas de ejecución, debe volver a habilitar las soluciones que quitó antes del traslado: 

1. Para iniciar la solución Change Tracking e Inventario, seleccione Change Tracking e Inventario en su cuenta de Automation. Elija el área de trabajo de Log Analytics que trasladó y seleccione **Habilitar**.

2. Repita el paso 1 para la solución Update Management.

    ![Nueva habilitación de las soluciones en la cuenta de Automation trasladada](../media/move-account/reenable-solutions.png)

3. Las máquinas incorporadas con sus soluciones son visibles cuando una vez se conecta el área de trabajo de Log Analytics existente. Para activar la solución Start/Stop VMs during off hours, debe volver a implementar la solución. En **Recursos relacionados**, seleccione **Start/Stop VMs** > **Más información acerca de la solución y cómo habilitarla** > **Crear** para iniciar la implementación.

4. En la página Agregar solución, elija el área de trabajo de Log Analytics y la cuenta de Automation.

    ![Menú Agregar solución](../media/move-account/add-solution-vm.png)

5. Configure la solución como se describe en [Solución Start/Stop VMs during off hours de Azure Automation](../automation-solution-vm-management.md).

## <a name="verify-the-move"></a>Comprobación del traslado

Una vez completado el traslado, compruebe las que funcionalidades de la lista siguiente estén habilitadas. 

|Capacidad|Pruebas|Solución de problemas|
|---|---|---|
|Runbooks|Un runbook puede ejecutar recursos de Azure y conectarse a ellos correctamente.|[Solución de problemas de runbooks](../troubleshoot/runbooks.md)
|Control de código fuente|Puede ejecutar una sincronización manual en el repositorio de control de código fuente.|[Integración de control de código fuente](../source-control-integration.md)|
|Change Tracking e Inventario|Compruebe que puede ver los datos del inventario actual en las máquinas.|[Solución de problemas de Change Tracking](../troubleshoot/change-tracking.md)|
|Administración de actualizaciones|Asegúrese de que ve las máquinas y están en buen estado.</br>Ejecute una implementación de actualizaciones de software de prueba.|[Solución de problemas de Update management](../troubleshoot/update-management.md)|
|Recursos compartidos|Compruebe que ve todos los recursos compartidos, como [credenciales ](../shared-resources/credentials.md), [variables](../shared-resources/variables.md) y similares.|

## <a name="next-steps"></a>Pasos siguientes

Para más información sobre el traslado de recursos en Azure, consulte [Traslado de recursos de Azure](../../azure-resource-manager/management/move-support-resources.md).
