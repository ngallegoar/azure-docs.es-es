---
title: Control de mantenimiento
description: Obtenga información sobre cómo controlar cuándo se aplica mantenimiento a las máquinas virtuales de Azure mediante el control de mantenimiento.
author: cynthn
ms.service: virtual-machines
ms.topic: article
ms.workload: infrastructure-services
ms.date: 11/21/2019
ms.author: cynthn
ms.openlocfilehash: 58c0964d170f49066802b955f09dab01eaf998a7
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "79226764"
---
# <a name="preview-control-updates-with-maintenance-control-and-the-azure-cli"></a>Vista previa: Control de las actualizaciones con el control de mantenimiento y la CLI de Azure

Administre las actualizaciones de la plataforma, que no requieren un reinicio, mediante el control de mantenimiento. Azure actualiza con frecuencia su infraestructura para mejorar la confiabilidad, el rendimiento y la seguridad o para poner en marcha nuevas características. La mayoría de las actualizaciones son transparentes para los usuarios. Algunas cargas de trabajo especialmente delicadas, como los juegos, el streaming multimedia o las transacciones financieras, no pueden tolerar ni siquiera unos segundos de bloqueo o desconexión por mantenimiento de una máquina virtual. El control de mantenimiento ofrece la opción de poner en espera las actualizaciones de la plataforma y aplicarlas en un período sucesivo de 35 días. 

El control de mantenimiento le permite decidir cuándo se aplican las actualizaciones a las máquinas virtuales aisladas y los hosts de Azure Dedicated Host.

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


## <a name="install-the-maintenance-extension"></a>Instalación de la extensión de mantenimiento

Si decide instalar la [CLI de Azure](https://docs.microsoft.com/cli/azure/install-azure-cli) localmente, necesita la versión 2.0.76 o posterior.

Instale la extensión de la CLI en versión preliminar `maintenance` localmente o en Cloud Shell. 

```azurecli-interactive
az extension add -n maintenance
```


## <a name="create-a-maintenance-configuration"></a>Creación de una configuración de mantenimiento

Use `az maintenance configuration create` para crear una configuración de mantenimiento. En este ejemplo se crea una configuración de mantenimiento denominada *myConfig* en el ámbito del host. 

```azurecli-interactive
az group create \
   --location eastus \
   --name myMaintenanceRG
az maintenance configuration create \
   -g myMaintenanceRG \
   --name myConfig \
   --maintenanceScope host\
   --location  eastus
```

Copie el identificador de configuración de la salida para usarlo más adelante.

El uso de `--maintenanceScope host` garantiza que la configuración de mantenimiento se utiliza para controlar las actualizaciones del host.

Si intenta crear una configuración con el mismo nombre, pero en una ubicación diferente, obtendrá un error. Los nombres de configuración deben ser únicos dentro de su suscripción.

Puede consultar las configuraciones de mantenimiento disponibles mediante `az maintenance configuration list`.

```azurecli-interactive
az maintenance configuration list --query "[].{Name:name, ID:id}" -o table 
```

## <a name="assign-the-configuration"></a>Asignación de la configuración

Use `az maintenance assignment create` para asignar la configuración a la máquina virtual aislada o al host de Azure Dedicated Host.

### <a name="isolated-vm"></a>Máquina virtual aislada

Aplique la configuración a una máquina virtual con el identificador de la configuración. Especifique `--resource-type virtualMachines` y proporcione el nombre de la máquina virtual para `--resource-name`, y el grupo de recursos para la máquina virtual en `--resource-group`, así como la ubicación de la máquina virtual para `--location`. 

```azurecli-interactive
az maintenance assignment create \
   --resource-group myMaintenanceRG \
   --location eastus \
   --resource-name myVM \
   --resource-type virtualMachines \
   --provider-name Microsoft.Compute \
   --configuration-assignment-name myConfig \
   --maintenance-configuration-id "/subscriptions/1111abcd-1a11-1a2b-1a12-123456789abc/resourcegroups/myMaintenanceRG/providers/Microsoft.Maintenance/maintenanceConfigurations/myConfig"
```

### <a name="dedicated-host"></a>Host dedicado

Para aplicar una configuración a un host dedicado, debe incluir `--resource-type hosts`, `--resource-parent-name` con el nombre del grupo host y `--resource-parent-type hostGroups`. 

El parámetro `--resource-id` es el identificador del host. Puede usar [az vm host get-instance-view](/cli/azure/vm/host#az-vm-host-get-instance-view) para obtener el identificador del host dedicado.

```azurecli-interactive
az maintenance assignment create \
   -g myDHResourceGroup \
   --resource-name myHost \
   --resource-type hosts \
   --provider-name Microsoft.Compute \
   --configuration-assignment-name myConfig \
   --maintenance-configuration-id "/subscriptions/1111abcd-1a11-1a2b-1a12-123456789abc/resourcegroups/myDhResourceGroup/providers/Microsoft.Maintenance/maintenanceConfigurations/myConfig" \
   -l eastus \
   --resource-parent-name myHostGroup \
   --resource-parent-type hostGroups 
```

## <a name="check-configuration"></a>Comprobación de configuración

Puede verificar que la configuración se aplicó correctamente o consultar qué configuración se aplica actualmente mediante `az maintenance assignment list`.

### <a name="isolated-vm"></a>Máquina virtual aislada

```azurecli-interactive
az maintenance assignment list \
   --provider-name Microsoft.Compute \
   --resource-group myMaintenanceRG \
   --resource-name myVM \
   --resource-type virtualMachines \
   --query "[].{resource:resourceGroup, configName:name}" \
   --output table
```

### <a name="dedicated-host"></a>Host dedicado 

```azurecli-interactive
az maintenance assignment list \
   --resource-group myDHResourceGroup \
   --resource-name myHost \
   --resource-type hosts \
   --provider-name Microsoft.Compute \
   --resource-parent-name myHostGroup \
   --resource-parent-type hostGroups 
   --query "[].{ResourceGroup:resourceGroup,configName:name}" \
   -o table
```


## <a name="check-for-pending-updates"></a>Búsqueda de actualizaciones pendientes

Use `az maintenance update list` para ver si hay actualizaciones pendientes. Actualice --subscription para que sea el identificador de la suscripción que contiene la máquina virtual.

Si no hay ninguna actualización, el comando devolverá un mensaje de error, que contendrá el texto: `Resource not found...StatusCode: 404`.

Si hay actualizaciones, solo se devolverá una, aunque haya varias actualizaciones pendientes. Los datos de esta actualización se devolverán en un objeto:

```text
[
  {
    "impactDurationInSec": 9,
    "impactType": "Freeze",
    "maintenanceScope": "Host",
    "notBefore": "2020-03-03T07:23:04.905538+00:00",
    "resourceId": "/subscriptions/9120c5ff-e78e-4bd0-b29f-75c19cadd078/resourcegroups/DemoRG/providers/Microsoft.Compute/hostGroups/demoHostGroup/hosts/myHost",
    "status": "Pending"
  }
]
  ```

### <a name="isolated-vm"></a>Máquina virtual aislada

Compruebe si hay actualizaciones pendientes para una máquina virtual aislada. En este ejemplo, se aplica a la salida formato de tabla para facilitar la lectura.

```azurecli-interactive
az maintenance update list \
   -g myMaintenanceRg \
   --resource-name myVM \
   --resource-type virtualMachines \
   --provider-name Microsoft.Compute \
   -o table
```

### <a name="dedicated-host"></a>Host dedicado

Compruebe si hay actualizaciones pendientes para un host dedicado. En este ejemplo, se aplica a la salida formato de tabla para facilitar la lectura. Reemplace los valores de los recursos por los suyos propios.

```azurecli-interactive
az maintenance update list \
   --subscription 1111abcd-1a11-1a2b-1a12-123456789abc \
   -g myHostResourceGroup \
   --resource-name myHost \
   --resource-type hosts \
   --provider-name Microsoft.Compute \
   --resource-parentname myHostGroup \
   --resource-parent-type hostGroups \
   -o table
```

## <a name="apply-updates"></a>Aplicación de actualizaciones

Use `az maintenance apply update` para aplicar actualizaciones pendientes. Si se ejecuta correctamente, este comando devolverá el archivo JSON que contiene los detalles de la actualización.

### <a name="isolated-vm"></a>Máquina virtual aislada

Cree una solicitud para aplicar actualizaciones a una máquina virtual aislada.

```azurecli-interactive
az maintenance applyupdate create \
   --subscription 1111abcd-1a11-1a2b-1a12-123456789abc \
   --resource-group myMaintenanceRG \
   --resource-name myVM \
   --resource-type virtualMachines \
   --provider-name Microsoft.Compute
```


### <a name="dedicated-host"></a>Host dedicado

Aplique las actualizaciones a un host dedicado.

```azurecli-interactive
az maintenance applyupdate create \
   --subscription 1111abcd-1a11-1a2b-1a12-123456789abc \
   --resource-group myHostResourceGroup \
   --resource-name myHost \
   --resource-type hosts \
   --provider-name Microsoft.Compute \
   --resource-parent-name myHostGroup \
   --resource-parent-type hostGroups
```

## <a name="check-the-status-of-applying-updates"></a>Comprobación del estado de aplicación de las actualizaciones 

Puede comprobar el progreso de las actualizaciones mediante `az maintenance applyupdate get`. 

Puede usar `default` como nombre de la actualización para ver los resultados de la última actualización o reemplazar `myUpdateName` por el nombre de la actualización que se devolvió cuando se ejecutó `az maintenance applyupdate create`.

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

```azurecli-interactive
az maintenance applyupdate get \
   --resource-group myMaintenanceRG \
   --resource-name myVM \
   --resource-type virtualMachines \
   --provider-name Microsoft.Compute \
   --apply-update-name default 
```

### <a name="dedicated-host"></a>Host dedicado

```azurecli-interactive
az maintenance applyupdate get \
   --subscription 1111abcd-1a11-1a2b-1a12-123456789abc \ 
   --resource-group myMaintenanceRG \
   --resource-name myHost \
   --resource-type hosts \
   --provider-name Microsoft.Compute \
   --resource-parent-name myHostGroup \ 
   --resource-parent-type hostGroups \
   --apply-update-name myUpdateName \
   --query "{LastUpdate:lastUpdateTime, Name:name, ResourceGroup:resourceGroup, Status:status}" \
   --output table
```


## <a name="delete-a-maintenance-configuration"></a>Eliminación de una configuración de mantenimiento

Use `az maintenance configuration delete` para eliminar una configuración de mantenimiento. Al eliminar la configuración se quita el control de mantenimiento de los recursos asociados.

```azurecli-interactive
az maintenance configuration delete \
   --subscription 1111abcd-1a11-1a2b-1a12-123456789abc \
   -g myResourceGroup \
   --name myConfig
```

## <a name="next-steps"></a>Pasos siguientes
Para obtener más información, consulte [Mantenimiento y actualizaciones](maintenance-and-updates.md).
