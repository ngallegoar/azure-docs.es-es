---
title: Control del mantenimiento de máquinas virtuales de Azure mediante PowerShell
description: Aprenda a controlar cuándo se aplica el mantenimiento a las máquinas virtuales de Azure mediante el control de mantenimiento y PowerShell.
author: cynthn
ms.service: virtual-machines
ms.topic: article
ms.workload: infrastructure-services
ms.date: 01/31/2020
ms.author: cynthn
ms.openlocfilehash: dc47afe9cb6eca1b10f8caca7b85087023c5eadf
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "80060133"
---
# <a name="preview-control-updates-with-maintenance-control-and-azure-powershell"></a>Vista previa: Control de las actualizaciones con el control de mantenimiento y Azure PowerShell

Administre las actualizaciones de la plataforma, que no requieren un reinicio, mediante el control de mantenimiento. Azure actualiza con frecuencia su infraestructura para mejorar la confiabilidad, el rendimiento y la seguridad o para poner en marcha nuevas características. La mayoría de las actualizaciones son transparentes para los usuarios. Algunas cargas de trabajo especialmente delicadas, como los juegos, el streaming multimedia o las transacciones financieras, no pueden tolerar ni siquiera unos segundos de bloqueo o desconexión por mantenimiento de una máquina virtual. El control de mantenimiento ofrece la opción de poner en espera las actualizaciones de la plataforma y aplicarlas en un período sucesivo de 35 días. 

El control de mantenimiento permite decidir cuándo se aplican las actualizaciones a las máquinas virtuales aisladas.

Con el control de mantenimiento, puede:
- Aplicar actualizaciones por lotes en un único paquete de actualización.
- Esperar hasta 35 días para aplicar las actualizaciones. 
- Automatizar las actualizaciones de la plataforma para la ventana de mantenimiento mediante Azure Functions.
- Mantener el trabajo de las configuraciones entre suscripciones y grupos de recursos. 

> [!IMPORTANT]
> El control de mantenimiento actualmente está en su versión preliminar pública.
> Esta versión preliminar se ofrece sin Acuerdo de Nivel de Servicio y no se recomienda para cargas de trabajo de producción. Es posible que algunas características no sean compatibles o que tengan sus funcionalidades limitadas. Para más información, consulte [Términos de uso complementarios de las Versiones Preliminares de Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).
> 

## <a name="limitations"></a>Limitaciones

- Las máquinas virtuales deben estar en un [host dedicado](./linux/dedicated-hosts.md) o bien crearse con un [tamaño de máquina virtual aislada](./linux/isolation.md).
- Después de 35 días, se aplicará automáticamente una actualización.
- El usuario debe tener acceso de **colaborador del recurso**.


## <a name="enable-the-powershell-module"></a>Habilitación del módulo de PowerShell

Asegúrese de que `PowerShellGet` está actualizado.

```azurepowershell-interactive
Install-Module -Name PowerShellGet -Repository PSGallery -Force
```

Los cmdlets de PowerShell Az.Maintenance se encuentran en versión preliminar, por lo que el módulo se debe instalar con el parámetro `AllowPrerelease` en Cloud Shell o en la instalación local de PowerShell.   

```azurepowershell-interactive
Install-Module -Name Az.Maintenance -AllowPrerelease
```

Si lo va a instalar localmente, asegúrese de abrir el símbolo del sistema de PowerShell como administrador.

También es posible que se le pida que confirme que desea realizar la instalación desde un *repositorio que no es de confianza*. Escriba `Y` o seleccione **Sí a todo** para instalar el módulo.



## <a name="create-a-maintenance-configuration"></a>Creación de una configuración de mantenimiento

Cree un grupo de recursos como contenedor para la configuración. En este ejemplo, se crea un grupo de recursos denominado *myMaintenanceRG* en  *eastus*. Si ya tiene un grupo de recursos que desea usar, puede omitir esta parte y reemplazar el nombre del grupo de recursos por el que posee en el resto de los ejemplos.

```azurepowershell-interactive
New-AzResourceGroup `
   -Location eastus `
   -Name myMaintenanceRG
```

Use [New-AzMaintenanceConfiguration](https://docs.microsoft.com/powershell/module/az.maintenance/new-azmaintenanceconfiguration) para crear una configuración de mantenimiento. En este ejemplo se crea una configuración de mantenimiento denominada *myConfig* en el ámbito del host. 

```azurepowershell-interactive
$config = New-AzMaintenanceConfiguration `
   -ResourceGroup myMaintenanceRG `
   -Name myConfig `
   -MaintenanceScope host `
   -Location  eastus
```

El uso de `-MaintenanceScope host` garantiza que la configuración de mantenimiento se utiliza para controlar las actualizaciones del host.

Si intenta crear una configuración con el mismo nombre, pero en una ubicación diferente, obtendrá un error. Los nombres de configuración deben ser únicos dentro de su suscripción.

Puede consultar las configuraciones de mantenimiento disponibles mediante [Get-AzMaintenanceConfiguration](https://docs.microsoft.com/powershell/module/az.maintenance/get-azmaintenanceconfiguration).

```azurepowershell-interactive
Get-AzMaintenanceConfiguration | Format-Table -Property Name,Id
```

## <a name="assign-the-configuration"></a>Asignación de la configuración

Use [New-AzConfigurationAssignment](https://docs.microsoft.com/powershell/module/az.maintenance/new-azconfigurationassignment) para asignar la configuración a la máquina virtual aislada o a Azure Dedicated Host.

### <a name="isolated-vm"></a>Máquina virtual aislada

Aplique la configuración a una máquina virtual con el identificador de la configuración. Especifique `-ResourceType VirtualMachines` y escriba el nombre de la máquina virtual para `-ResourceName`, y el grupo de recursos de la máquina virtual para `-ResourceGroupName`. 

```azurepowershell-interactive
New-AzConfigurationAssignment `
   -ResourceGroupName myResourceGroup `
   -Location eastus `
   -ResourceName myVM `
   -ResourceType VirtualMachines `
   -ProviderName Microsoft.Compute `
   -ConfigurationAssignmentName $config.Name `
   -MaintenanceConfigurationId $config.Id
```

### <a name="dedicated-host"></a>Host dedicado

Para aplicar una configuración a un host dedicado, también es preciso incluir `-ResourceType hosts`, `-ResourceParentName` con el nombre del grupo host y `-ResourceParentType hostGroups`. 


```azurepowershell-interactive
New-AzConfigurationAssignment `
   -ResourceGroupName myResourceGroup `
   -Location eastus `
   -ResourceName myHost `
   -ResourceType hosts `
   -ResourceParentName myHostGroup `
   -ResourceParentType hostGroups `
   -ProviderName Microsoft.Compute `
   -ConfigurationAssignmentName $config.Name `
   -MaintenanceConfigurationId $config.Id
```

## <a name="check-for-pending-updates"></a>Búsqueda de actualizaciones pendientes

Use [Get-AzMaintenanceUpdate](https://docs.microsoft.com/powershell/module/az.maintenance/get-azmaintenanceupdate) para ver si hay actualizaciones pendientes. Use `-subscription` para especificar la suscripción de Azure de la máquina virtual, en caso de que no sea la misma en la que ha iniciado sesión.

Si no hay ninguna actualización para mostrar, este comando no devolverá nada. De lo contrario, devolverá un objeto PSApplyUpdate:

```json
{
   "maintenanceScope": "Host",
   "impactType": "Freeze",
   "status": "Pending",
   "impactDurationInSec": 9,
   "notBefore": "2020-02-21T16:47:44.8728029Z",
   "properties": {
      "resourceId": "/subscriptions/39c6cced-4d6c-4dd5-af86-57499cd3f846/resourcegroups/Ignite2019/providers/Microsoft.Compute/virtualMachines/MCDemo3"
} 
```

### <a name="isolated-vm"></a>Máquina virtual aislada

Compruebe si hay actualizaciones pendientes para una máquina virtual aislada. En este ejemplo, se aplica a la salida formato de tabla para facilitar la lectura.

```azurepowershell-interactive
Get-AzMaintenanceUpdate `
  -ResourceGroupName myResourceGroup `
  -ResourceName myVM `
  -ResourceType VirtualMachines `
  -ProviderName Microsoft.Compute | Format-Table
```


### <a name="dedicated-host"></a>Host dedicado

Compruebe si hay actualizaciones pendientes para un host dedicado. En este ejemplo, se aplica a la salida formato de tabla para facilitar la lectura. Reemplace los valores de los recursos por los suyos propios.

```azurepowershell-interactive
Get-AzMaintenanceUpdate `
   -ResourceGroupName myResourceGroup `
   -ResourceName myHost `
   -ResourceType hosts `
   -ResourceParentName myHostGroup `
   -ResourceParentType hostGroups `
   -ProviderName Microsoft.Compute | Format-Table
```


## <a name="apply-updates"></a>Aplicación de actualizaciones

Use [New-AzApplyUpdate](https://docs.microsoft.com/powershell/module/az.maintenance/new-azapplyupdate) para aplicar las actualizaciones pendientes.

### <a name="isolated-vm"></a>Máquina virtual aislada

Cree una solicitud para aplicar actualizaciones a una máquina virtual aislada.

```azurepowershell-interactive
New-AzApplyUpdate `
   -ResourceGroupName myResourceGroup `
   -ResourceName myVM `
   -ResourceType VirtualMachines `
   -ProviderName Microsoft.Compute
```

Si se ejecuta correctamente, este comando devolverá un objeto `PSApplyUpdate`. Puede usar el atributo de nombre en el comando `Get-AzApplyUpdate` para comprobar el estado de la actualización. Consulte [Comprobación del estado de actualización](#check-update-status).

### <a name="dedicated-host"></a>Host dedicado

Aplique las actualizaciones a un host dedicado.

```azurepowershell-interactive
New-AzApplyUpdate `
   -ResourceGroupName myResourceGroup `
   -ResourceName myHost `
   -ResourceType hosts `
   -ResourceParentName myHostGroup `
   -ResourceParentType hostGroups `
   -ProviderName Microsoft.Compute
```

## <a name="check-update-status"></a>Comprobación del estado de actualización
Use [Get-AzApplyUpdate](https://docs.microsoft.com/powershell/module/az.maintenance/get-azapplyupdate) para comprobar el estado de una actualización. Los comandos que se muestran a continuación muestran el estado de la última actualización mediante `default` para el parámetro `-ApplyUpdateName`. Puede sustituir el nombre de la actualización (devuelto por el comando [New-AzApplyUpdate](https://docs.microsoft.com/powershell/module/az.maintenance/new-azapplyupdate)) para obtener el estado de una actualización específica.

```text
Status         : Completed
ResourceId     : /subscriptions/12ae7457-4a34-465c-94c1-17c058c2bd25/resourcegroups/TestShantS/providers/Microsoft.Comp
ute/virtualMachines/DXT-test-04-iso
LastUpdateTime : 1/1/2020 12:00:00 AM
Id             : /subscriptions/12ae7457-4a34-465c-94c1-17c058c2bd25/resourcegroups/TestShantS/providers/Microsoft.Comp
ute/virtualMachines/DXT-test-04-iso/providers/Microsoft.Maintenance/applyUpdates/default
Name           : default
Type           : Microsoft.Maintenance/applyUpdates
```
LastUpdateTime será la hora a la que se completó la actualización, tanto si la inició el usuario o la plataforma en caso de que no se usara la ventana de mantenimiento automático. Si nunca se ha aplicado una actualización mediante el control de mantenimiento, se mostrará el valor predeterminado.

### <a name="isolated-vm"></a>Máquina virtual aislada

Compruebe si hay actualizaciones para una máquina virtual específica.

```azurepowershell-interactive
Get-AzApplyUpdate `
   -ResourceGroupName myResourceGroup `
   -ResourceName myVM `
   -ResourceType VirtualMachines `
   -ProviderName Microsoft.Compute `
   -ApplyUpdateName default
```

### <a name="dedicated-host"></a>Host dedicado

Compruebe las actualizaciones a un host dedicado.

```azurepowershell-interactive
Get-AzApplyUpdate `
   -ResourceGroupName myResourceGroup `
   -ResourceName myHost `
   -ResourceType hosts `
   -ResourceParentName myHostGroup `
   -ResourceParentType hostGroups `
   -ProviderName Microsoft.Compute `
   -ApplyUpdateName myUpdateName
```

## <a name="remove-a-maintenance-configuration"></a>Eliminación de una configuración de mantenimiento

Use [Remove-AzMaintenanceConfiguration](https://docs.microsoft.com/powershell/module/az.maintenance/remove-azmaintenanceconfiguration) para eliminar una configuración de mantenimiento.

```azurepowershell-interactive
Remove-AzMaintenanceConfiguration `
   -ResourceGroupName myResourceGroup `
   -Name $config.Name
```

## <a name="next-steps"></a>Pasos siguientes
Para obtener más información, consulte [Mantenimiento y actualizaciones](maintenance-and-updates.md).
