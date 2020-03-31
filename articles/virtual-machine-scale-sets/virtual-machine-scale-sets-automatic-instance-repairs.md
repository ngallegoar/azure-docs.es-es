---
title: Reparaciones automáticas de instancias con conjuntos de escalado de máquinas virtuales de Azure
description: Aprenda a configurar la directiva de reparaciones automáticas para instancias de máquinas virtuales en un conjunto de escalado.
author: avirishuv
manager: vashan
tags: azure-resource-manager
ms.service: virtual-machine-scale-sets
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm
ms.topic: conceptual
ms.date: 02/28/2020
ms.author: avverma
ms.openlocfilehash: f335b0fb3396103c321d740bcf6d125e60e95086
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "78274580"
---
# <a name="preview-automatic-instance-repairs-for-azure-virtual-machine-scale-sets"></a>Vista previa: Reparaciones automáticas de instancias para conjuntos de escalado de máquinas virtuales de Azure

La habilitación de las reparaciones automáticas de instancias para conjuntos de escalado de máquinas virtuales de Azure ayuda a lograr una alta disponibilidad para las aplicaciones al mantener un conjunto de instancias correctas. Si se detecta que una instancia del conjunto de escalado tiene un estado incorrecto según lo indicado por la [extensión de estado de la aplicación](./virtual-machine-scale-sets-health-extension.md) o por los [sondeos de estado del equilibrador de carga](../load-balancer/load-balancer-custom-probe-overview.md), esta característica realiza automáticamente la reparación de la instancia mediante la eliminación de la instancia incorrecta y la creación de una nueva para reemplazarla.

> [!NOTE]
> Esta característica en versión preliminar se ofrece sin Acuerdo de Nivel de Servicio y no se recomienda para cargas de trabajo de producción.

## <a name="requirements-for-using-automatic-instance-repairs"></a>Requisitos para el uso de reparaciones automáticas de instancias

**Participación en la versión preliminar de las reparaciones automáticas de instancias**

Use la API REST o Azure PowerShell para participar en la versión preliminar de la reparación automática de instancias. En estos pasos se registrará la suscripción para la característica en versión preliminar. Tenga en cuenta que esta es una configuración que se requiere solo una vez para usar esta característica. Si la suscripción ya está registrada para la versión preliminar de las reparaciones automáticas de instancias, no es necesario volver a registrarla. 

Uso de la API de REST 

1. Regístrese para la característica en [Características: registro](/rest/api/resources/features/register). 

```
POST on '/subscriptions/{subscriptionId}/providers/Microsoft.Features/providers/Microsoft.Compute/features/RepairVMScaleSetInstancesPreview/register?api-version=2015-12-01'
```

```json
{
  "properties": {
    "state": "Registering"
  },
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Features/providers/Microsoft.Compute/features/RepairVMScaleSetInstancesPreview",
  "type": "Microsoft.Features/providers/features",
  "name": "Microsoft.Compute/RepairVMScaleSetInstancesPreview"
}
```

2. Espere unos minutos para que el *estado* cambie a *Registrado*. Puede usar la siguiente API para confirmarlo.

```
GET on '/subscriptions/{subscriptionId}/providers/Microsoft.Features/providers/Microsoft.Compute/features/RepairVMScaleSetInstancesPreview?api-version=2015-12-01'
```

```json
{
  "properties": {
    "state": "Registered"
  },
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Features/providers/Microsoft.Compute/features/RepairVMScaleSetInstancesPreview",
  "type": "Microsoft.Features/providers/features",
  "name": "Microsoft.Compute/RepairVMScaleSetInstancesPreview"
}
```

3. Cuando el *Estado* cambia a *Registrado*, ejecute lo siguiente.

```
POST on '/subscriptions/{subscriptionId}/providers/Microsoft.Compute/register?api-version=2015-12-01'
```

Uso de Azure PowerShell

1. Regístrese para la característica mediante el cmdlet [Register-AzureRmResourceProvider](/powershell/module/azurerm.resources/register-azurermresourceprovider) seguido de [Register-AzureRmProviderFeature](/powershell/module/azurerm.resources/register-azurermproviderfeature).

```azurepowershell-interactive
Register-AzureRmResourceProvider `
 -ProviderNamespace Microsoft.Compute

Register-AzureRmProviderFeature `
 -ProviderNamespace Microsoft.Compute `
 -FeatureName RepairVMScaleSetInstancesPreview
```

2. Espere unos minutos para que *RegistrationState* cambie a *Registrado*. Puede usar el siguiente cmdlet para confirmarlo.

```azurepowershell-interactive
Get-AzureRmProviderFeature `
 -ProviderNamespace Microsoft.Compute `
 -FeatureName RepairVMScaleSetInstancesPreview
 ```

 La respuesta debe ser la siguiente.

| FeatureName                           | ProviderName            | RegistrationState       |
|---------------------------------------|-------------------------|-------------------------|
| RepairVMScaleSetInstancesPreview      | Microsoft.Compute       | Registrado              |

3. Cuando *RegistrationState* cambia a *Registrado*, ejecute el siguiente cmdlet.

```azurepowershell-interactive
Register-AzureRmResourceProvider `
 -ProviderNamespace Microsoft.Compute
```

**Habilitación de la supervisión del estado de la aplicación para el conjunto de escalado**

El conjunto de escalado debe tener la supervisión del estado de la aplicación para las instancias habilitadas. Esto puede hacerse mediante la [extensión de estado de la aplicación](./virtual-machine-scale-sets-health-extension.md) o los [sondeos de estado del equilibrador de carga](../load-balancer/load-balancer-custom-probe-overview.md). Solo se puede habilitar uno de ellos a la vez. La extensión de mantenimiento de la aplicación o el equilibrador de carga sondea el punto de conexión de la aplicación configurado en las instancias de máquina virtual para determinar el estado de mantenimiento de la aplicación. El orquestador del conjunto de escalado usa este estado de mantenimiento para supervisar el estado de las instancias y realizar reparaciones cuando sea necesario.

**Configuración del punto de conexión para proporcionar el estado de mantenimiento**

Antes de habilitar la directiva de reparaciones automáticas de instancias, asegúrese de que las instancias del conjunto de escalado tienen configurado el punto de conexión de aplicación para emitir el estado de mantenimiento de la aplicación. Cuando una instancia devuelve el estado 200 (correcto) en este punto de conexión de la aplicación, la instancia se marca con estado "Correcto". En todos los demás casos, la instancia se marca con estado "Incorrecto", incluidos los escenarios siguientes:

- Cuando no hay ningún punto de conexión de aplicación configurado dentro de las instancias de máquina virtual para proporcionar el estado de mantenimiento de la aplicación
- Cuando el punto de conexión de la aplicación no está configurado correctamente
- Cuando no se puede tener acceso al punto de conexión de la aplicación

En el caso de las instancias marcadas con el estado "Incorrecto", el conjunto de escalado desencadena las reparaciones automáticas. Asegúrese de que el punto de conexión de la aplicación está configurado correctamente antes de habilitar la directiva de reparaciones automáticas para evitar reparaciones de instancias no intencionadas mientras se configura el punto de conexión.

**Habilitación del grupo de selección de ubicación único**

Esta versión preliminar solo está disponible para conjuntos de escalado implementados como un grupo de selección de ubicación único. La propiedad *singlePlacementGroup* debe establecerse en *true* para que el conjunto de escalado use la característica de reparaciones automáticas de instancias. Más información acerca de los [grupos de selección de ubicación](./virtual-machine-scale-sets-placement-groups.md#placement-groups).

**Versión de API**

La directiva de reparaciones automáticas es compatible con Compute API versión 2018-10-01 o superior.

**Restricciones en los movimientos de recursos o suscripciones**

Como parte de esta versión preliminar, actualmente no se admiten los movimientos de recursos o suscripciones para los conjuntos de escalado cuando la directiva de reparaciones automáticas está habilitada.

**Restricción para los conjuntos de escalado de Service Fabric**

Esta característica en versión preliminar no se admite actualmente para los conjuntos de escalado de Service Fabric.

## <a name="how-do-automatic-instance-repairs-work"></a>¿Cómo funciona la reparación automática de instancias?

La característica de reparación automática de instancias se basa en la supervisión del estado de las instancias individuales de un conjunto de escalado. Las instancias de máquina virtual de un conjunto de escalado se pueden configurar para emitir el estado de mantenimiento de la aplicación mediante la [extensión de estado de aplicación](./virtual-machine-scale-sets-health-extension.md) o los [sondeos de estado del equilibrador de carga](../load-balancer/load-balancer-custom-probe-overview.md). Si una instancia se encuentra en un estado incorrecto, el conjunto de escalado realiza la acción de reparación eliminando la instancia incorrecta y creando una nueva para reemplazarla. Esta característica se puede habilitar en el modelo de conjunto de escalado de máquinas virtuales mediante el objeto *automaticRepairsPolicy*.

### <a name="batching"></a>Lotes

Las operaciones de reparaciones automáticas de instancias se realizan en lotes. En un momento dado, no se reparan más del 5 % de las instancias del conjunto de escalado mediante la directiva de reparaciones automáticas. Esto ayuda a evitar la eliminación simultánea y la nueva creación de un gran número de instancias si se encuentran en estado incorrecta al mismo tiempo.

### <a name="grace-period"></a>Período de gracia

Cuando una instancia pasa por una operación de cambio de estado debido a una acción PUT, PATCH o POST realizada en el conjunto de escalado (por ejemplo, restablecimiento de la imagen, reimplementación, actualización, etc.), se realiza cualquier acción de reparación en esa instancia solo después de esperar el período de gracia. El período de gracia es la cantidad de tiempo que se permite que la instancia vuelva al estado correcto. El período de gracia se inicia después de que se haya completado el cambio de estado. Esto ayuda a evitar cualquier operación de reparación prematura o accidental. El período de gracia se respeta para cualquier instancia recién creada en el conjunto de escalado (incluido el creado como resultado de la operación de reparación). El período de gracia se especifica en minutos en formato ISO 8601 y se puede establecer mediante la propiedad *automaticRepairsPolicy.gracePeriod*. El período de gracia puede oscilar entre 30 minutos y 90 minutos, y tiene un valor predeterminado de 30 minutos.

El proceso de reparaciones automáticas de instancias funciona de la siguiente manera:

1. La [extensión de estado de aplicación](./virtual-machine-scale-sets-health-extension.md) o los [sondeos de estado del equilibrador de carga](../load-balancer/load-balancer-custom-probe-overview.md) hacen ping en el punto de conexión de la aplicación dentro de cada máquina virtual del conjunto de escalado para obtener el estado de mantenimiento de la aplicación para cada instancia.
2. Si el punto de conexión responde con un estado 200 (correcto), la instancia se marca con estado "Correcto". En todos los demás casos (incluido si no se puede tener acceso al punto de conexión), la instancia se marca con estado "Incorrecto".
3. Cuando una instancia se encuentra en un estado incorrecto, el conjunto de escalado desencadena una acción de reparación al eliminar la instancia incorrecta y crear una nueva para reemplazarla.
4. Las reparaciones de instancias se realizan en lotes. En un momento dado, no se reparan más del 5 % del total de instancias del conjunto de escalado. Si un conjunto de escalado tiene menos de 20 instancias, las reparaciones se realizan para una instancia incorrecta a la vez.
5. El proceso anterior continúa hasta que se reparen todas las instancias incorrectas del conjunto de escalado.

## <a name="instance-protection-and-automatic-repairs"></a>Protección de instancias y reparaciones automáticas

Si una instancia de un conjunto de escalado está protegida por la aplicación de *[Protección contra las acciones del conjunto de escalado](./virtual-machine-scale-sets-instance-protection.md#protect-from-scale-set-actions)* , las reparaciones automáticas no se realizan en esa instancia.

## <a name="enabling-automatic-repairs-policy-when-creating-a-new-scale-set"></a>Habilitación de la directiva de reparaciones automáticas al crear un nuevo conjunto de escalado

Para habilitar la directiva de reparaciones automáticas al crear un nuevo conjunto de escalado, asegúrese de que se cumplen todos los [requisitos](#requirements-for-using-automatic-instance-repairs) para optar a esta característica. El punto de conexión de la aplicación debe configurarse correctamente para las instancias del conjunto de escalado a fin de evitar la activación de reparaciones imprevistas mientras se configura el punto de conexión. En el caso de los conjuntos de escalado recién creados, cualquier reparación de instancias se realiza solo después de esperar la duración del período de gracia. Para habilitar la reparación automática de la instancia en un conjunto de escalado, use el objeto *automaticRepairsPolicy* en el modelo del conjunto de escalado de máquinas virtuales.

### <a name="rest-api"></a>API DE REST

En el ejemplo siguiente se muestra cómo habilitar la reparación automática de instancias en un modelo de conjunto de escalado. Use la API versión 2018-10-01 o posteriores.

```
PUT or PATCH on '/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Compute/virtualMachineScaleSets/{vmScaleSetName}?api-version=2019-07-01'
```

```json
{
  "properties": {
    "automaticRepairsPolicy": {
            "enabled": "true",
            "gracePeriod": "PT30M"
        }
    }
}
```

### <a name="azure-powershell"></a>Azure PowerShell

La característica de reparación automática de instancias se puede habilitar al crear un nuevo conjunto de escalado mediante el cmdlet [New-AzVmssConfig](/powershell/module/az.compute/new-azvmssconfig). Este script de ejemplo le guía por la creación de un conjunto de escalado y los recursos asociados mediante el archivo de configuración: [Creación de un conjunto de escalado de máquinas virtuales completo](./scripts/powershell-sample-create-complete-scale-set.md) Puede configurar la directiva de reparaciones automáticas de instancias al agregar los parámetros *EnableAutomaticRepair* y *AutomaticRepairGracePeriod* al objeto de configuración para crear el conjunto de escalado. En el ejemplo siguiente se habilita la característica con un período de gracia de 30 minutos.

```azurepowershell-interactive
New-AzVmssConfig `
 -Location "EastUS" `
 -SkuCapacity 2 `
 -SkuName "Standard_DS2" `
 -UpgradePolicyMode "Automatic" `
 -EnableAutomaticRepair $true `
 -AutomaticRepairGracePeriod "PT30M"
```

## <a name="enabling-automatic-repairs-policy-when-updating-an-existing-scale-set"></a>Habilitación de la directiva de reparaciones automáticas al actualizar un conjunto de escalado existente

Antes de habilitar la directiva de reparaciones automáticas en un conjunto de escalado existente, asegúrese de que se cumplen todos los [requisitos](#requirements-for-using-automatic-instance-repairs) para optar a esta característica. El punto de conexión de la aplicación debe configurarse correctamente para las instancias del conjunto de escalado a fin de evitar la activación de reparaciones imprevistas mientras se configura el punto de conexión. Para habilitar la reparación automática de la instancia en un conjunto de escalado, use el objeto *automaticRepairsPolicy* en el modelo del conjunto de escalado de máquinas virtuales.

### <a name="rest-api"></a>API DE REST

En el ejemplo siguiente se habilita la directiva con un período de gracia de 40 minutos. Use la API versión 2018-10-01 o posteriores.

```
PUT or PATCH on '/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Compute/virtualMachineScaleSets/{vmScaleSetName}?api-version=2019-07-01'
```

```json
{
  "properties": {
    "automaticRepairsPolicy": {
            "enabled": "true",
            "gracePeriod": "PT40M"
        }
    }
}
```

### <a name="azure-powershell"></a>Azure PowerShell

Use el cmdlet [Update-AzVmss](/powershell/module/az.compute/update-azvmss) para modificar la configuración de la característica de reparación automática de instancias en un conjunto de escalado existente. En el ejemplo siguiente se actualiza el período de gracia a 40 minutos.

```azurepowershell-interactive
Update-AzVmss `
 -ResourceGroupName "myResourceGroup" `
 -VMScaleSetName "myScaleSet" `
 -EnableAutomaticRepair $true `
 -AutomaticRepairGracePeriod "PT40M"
```

## <a name="troubleshoot"></a>Solución de problemas

**Error al habilitar la directiva de reparaciones automáticas**

Si recibe un error de "BadRequest" con un mensaje de error que indica que "no se pudo encontrar el miembro 'automaticRepairsPolicy' en el objeto de tipo 'properties'", compruebe la versión de API usada para el conjunto de escalado de máquinas virtuales. Se requiere la versión 2018-10-01 de la API o una posterior para esta característica.

**La instancia no se repara incluso cuando la directiva está habilitada**.

La instancia puede estar en el período de gracia. Esta es la cantidad de tiempo que hay que esperar después de cualquier cambio de estado en la instancia antes de realizar reparaciones. Esto es para evitar reparaciones prematuras o accidentales. La acción de reparar debe realizarse una vez completado el período de gracia de la instancia.

**Visualización del estado de mantenimiento de la aplicación para instancias del conjunto de escalado**

Puede usar [Get Instance View API](/rest/api/compute/virtualmachinescalesetvms/getinstanceview) para las instancias de un conjunto de escalado de máquinas virtuales para ver el estado de mantenimiento de la aplicación. Con Azure PowerShell, puede usar el cmdlet [Get-AzVmssVM](/powershell/module/az.compute/get-azvmssvm) con la marca *-InstanceView*. El estado de mantenimiento de la aplicación se proporciona en la propiedad *vmHealth*.

## <a name="next-steps"></a>Pasos siguientes

Aprenda a configurar la [extensión de estado de la aplicación](./virtual-machine-scale-sets-health-extension.md) o los [sondeos de estado del equilibrador de carga](../load-balancer/load-balancer-custom-probe-overview.md) para los conjuntos de escalado.
