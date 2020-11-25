---
title: Movimiento de recursos entre regiones mediante PowerShell en Azure Resource Mover
description: Aprenda a mover recursos entre regiones mediante PowerShell en Azure Resource Mover.
manager: evansma
author: rayne-wiselman
ms.service: resource-move
ms.topic: how-to
ms.date: 09/10/2020
ms.author: raynew
ms.openlocfilehash: 176f12a0a06a5bcae601463e30189bc139d3531f
ms.sourcegitcommit: c95e2d89a5a3cf5e2983ffcc206f056a7992df7d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/24/2020
ms.locfileid: "95543857"
---
# <a name="move-resources-across-regions-in-powershell"></a>Movimiento de recursos entre regiones en PowerShell

Aprenda a mover recursos de Azure a una región diferente mediante PowerShell en Azure Resource Mover. 

> [!NOTE]
> Azure Resource Mover se encuentra actualmente en versión preliminar.



## <a name="before-you-start"></a>Antes de empezar

- Su suscripción de Azure debe tener acceso a Resource Mover y se debe tener permisos de [propietario](../role-based-access-control/built-in-roles.md#owner) o [administrador de acceso de usuario](../role-based-access-control/built-in-roles.md#user-access-administrator) para la suscripción.
- Resource Mover no hace un seguimiento de los cambios y las actualizaciones, por lo que deberá realizar los cambios necesarios en los recursos antes de empezar a moverlos.
- Al mover recursos con PowerShell, actualmente no se puede editar ninguna configuración de región de destino. Modifique estos valores directamente en el portal.
- Al agregar recursos a una colección de movimiento, como forma de preparación para moverlos a otra región, los metadatos sobre el movimiento se almacenan en un grupo de recursos creado para tal fin. Actualmente, este grupo de recursos puede residir en las regiones Este de EE. UU. 2 o Europa del Norte. Los recursos de Azure pueden moverse entre regiones públicas mediante los metadatos existentes en cualquiera de estas regiones.

## <a name="sample-values"></a>Valores de ejemplo

Usaremos estos valores en los ejemplos de script:

**Configuración** | **Valor** 
--- | ---
Id. de suscripción | subscription-id
Ubicación del servicio Resource Mover | Este de EE. UU. 2
Grupo de recursos de metadatos | RegionMoveRG-centralus-westcentralus
Ubicación del grupo de recursos de metadatos | Este de EE. UU. 2
Nombre de la colección de movimiento | MoveCollection-centralus-westcentralus
Región de origen |  centralus
Grupo de recursos de origen | PSDemoRM
Región de destino | westcentralus
Grupo de recursos de destino | PSDemoRMtgt
Máquina virtual que se va a mover | PSDemoVM

## <a name="sign-in-to-azure"></a>Inicio de sesión en Azure

Inicie sesión en la suscripción de Azure.

```azurepowershell-interactive
# Sign in to an Azure subscription
Connect-AzAccount – Subscription "<subscription-id>"
```

## <a name="create-the-resource-group-for-metadata"></a>Creación del grupo de recursos para los metadatos.

Cree un grupo de recursos que contenga los metadatos de la colección de movimiento y la información de configuración.

```azurepowershell-interactive
# Sign in to an Azure subscription
New-AzResourceGroup -Name MoveCollection-centralus-westcentralus -Location "East US 2"
```

**Salida prevista**:

![Texto de salida después de crear el grupo de recursos](./media/move-region-powershell/output-create-resource-group.png)

## <a name="register-the-resource-provider"></a>Registrar el proveedor de recursos

Registre el proveedor de recursos Microsoft.Migrate para que se pueda crear el recurso MoveCollection.

```azurepowershell-interactive
# Register the provider
Register-AzResourceProvider -ProviderNamespace Microsoft.Migrate 

# Wait for registration
While(((Get-AzResourceProvider -ProviderNamespace Microsoft.Migrate)| where {$_.RegistrationState -eq "Registered" -and $_.ResourceTypes.ResourceTypeName -eq "moveCollections"}|measure).Count -eq 0)
  {
      Start-Sleep -Seconds 5
      Write-Output "Waiting for registration to complete."
  }
```

## <a name="create-the-move-collection"></a>Creación de la colección de movimiento

El objeto MoveCollection almacena los metadatos y la información de configuración de los recursos que se quieren mover.

- Para que el objeto MoveCollection tenga acceso a la suscripción en la que se encuentra el servicio Azure Resource Mover, necesita una [identidad administrada asignada por el sistema](../active-directory/managed-identities-azure-resources/overview.md#managed-identity-types) (antes conocida como identidad de servicio administrada (MSI)) en la que confíe la suscripción.
- A la identidad se le asigna el rol de colaborador o administrador de acceso de usuario en la suscripción de origen.
- Para asignar un rol a la identidad, necesita un rol en la suscripción (por ejemplo, propietario o administrador de acceso de usuario), con estos permisos:
    -  Microsoft.Authorization/roleAssignments/write
    - Microsoft.Authorization/roleAssignments/delete


```azurepowershell-interactive
# Create a MoveCollection object
New-AzResourceMoverMoveCollection -Name "MoveCollection-centralus-westcentralus " -ResourceGroupName "RegionMoveRG-centralus-westcentralus " -SubscriptionId "<subscription-id>" -SourceRegion "centralus" -TargetRegion "westcentralus" -Location  "East US 2" -IdentityType SystemAssigned
```
**Salida prevista**:

![Texto de salida después de crear la colección de movimiento](./media/move-region-powershell/output-move-collection.png)


## <a name="assign-a-managed-identity"></a>Asignación de una identidad administrada 

Establezca el identificador de la suscripción, recupere la entidad de seguridad del objeto MoveCollection y asígnele los roles de Azure.


```azurepowershell-interactive
# Set the subscriptionId
$subscriptionId = "<subscription-id>"

# Retrieve the principal managed identity of the MoveCollection
$moveCollection = Get-AzResourceMoverMoveCollection -SubscriptionId $subscriptionId -ResourceGroupName "RegionMoveRG-centralus-westcentralus " -Name "MoveCollection-centralus-westcentralus "

# Set the IdentityPrincipalID
$identityPrincipalId = $moveCollection.IdentityPrincipalId

# Assign the role to the IdentityPrincipalID
New-AzRoleAssignment -ObjectId $identityPrincipalId -RoleDefinitionName Contributor -Scope "/subscriptions/$subscriptionId"

New-AzRoleAssignment -ObjectId $identityPrincipalId -RoleDefinitionName "User Access Administrator" -Scope "/subscriptions/$subscriptionId"

```

## <a name="add-resources-to-the-move-collection"></a>Adición de recursos a la colección de movimientos

Recupere el identificador del recurso de origen que quiere mover. A continuación, agréguelo a la colección de movimiento.

```azurepowershell-interactive
# Retrieve the resource ID
Get-AzResource -Name PSDemoVM -ResourceGroupName PSDemoRM
```

**Salida prevista**

![Texto de salida después de recuperar el identificador de recurso](./media/move-region-powershell/output-retrieve-resource.png)


```azurepowershell-interactive
# Add the resource to the move collection
New-AzResourceMoverMoveResource -SubscriptionId “<subscription-id>” -ResourceGroupName “RegionMoveRG-centralus-westcentralus ” -MoveCollectionName “MoveCollection-centralus-westcentralus ” -SourceId “/subscriptions/e80eb9fa-c996-4435-aa32-5af6f3d3077c/resourceGroups/PSDemoRM/providers/Microsoft.Compute/virtualMachines/PSDemoVM” -Name “PSDemoVM” -ResourceSettingResourceType “ Microsoft.Compute/virtualMachines” -ResourceSettingTargetResourceName “PSDemoVM”
```

**Salida prevista**
![Texto de salida después de agregar el recurso](./media/move-region-powershell/output-add-resource.png)

## <a name="resolve-dependencies"></a>Resolución de dependencias

Valide los recursos agregados y resuelva las dependencias de otros recursos.

```azurepowershell-interactive
# Resolve dependencies
Resolve-AzResourceMoverMoveCollectionDependency -SubscriptionId “<subscription-id>” -ResourceGroupName “RegionMoveRG-centralus-westcentralus” -MoveCollectionName “MoveCollection-centralus-westcentralus”
```
**Salida si existen dependencias**

Si el recurso de la colección de movimiento tiene dependencias de otros recursos, se emite un mensaje con errores.

![Texto de salida después de comprobar las dependencias](./media/move-region-powershell/dependency-error.png)

### <a name="retrieve-dependencies"></a>Recuperación de dependencias

Si el recurso que quiere mover tiene dependencias de otros recursos, puede recuperar información sobre las dependencias que faltan.

```azurepowershell-interactive
# Identify dependencies
Get-AzResourceMoverUnresolvedDependency -MoveCollectionName “MoveCollection-centralus-westcentralus ” -ResourceGroupName “RegionMoveRG-centralus-westcentralus ” -SubscriptionId  “<subscription-id>”
```

**Salida prevista**

![Texto de salida después de recuperar las dependencias](./media/move-region-powershell/missing-dependencies.png)

En este ejemplo, se identifica que faltan dos dependencias:

- Grupo de recursos de origen: PSDemoRM
- NIC en máquina virtual: PSDemoVM62

## <a name="add-dependencies-to-collection"></a>Adición de dependencias a la colección


Con los identificadores de recursos que recuperó, identifique los nombres de los recursos que faltan y agréguelos a la colección de movimiento.

### <a name="add-the-nic"></a>Incorporación de la NIC

Recupere el nombre y el tipo de la NIC y agréguela a la colección.

```azurepowershell-interactive
# Get the NIC name and resource type
Get-AzResource -ResourceId “/subscriptions/ <subscription-id> /resourcegroups/psdemorm/providers/microsoft.network/networkinterfaces/psdemovm62”

```

**Salida prevista**

![Texto de salida después de recuperar la NIC](./media/move-region-powershell/output-retrieve-nic.png)

Ahora, agregue la NIC a la colección de movimiento. En este ejemplo, se asigna el mismo nombre a la NIC de destino. Mantenga la configuración y la coherencia de mayúsculas y minúsculas.

```azurepowershell-interactive
# Add the NIC to the collection. 
New-AzResourceMoverMoveResource -SubscriptionId “<subscription-id>” -ResourceGroupName “RegionMoveRG-centralus-westcentralus” -MoveCollectionName “MoveCollection-centralus-westcentralus ” -SourceId “/subscriptions/<subscription-id>/resourceGroups/PSDemoRM/providers/Microsoft.Network/networkInterfaces/psdemovm62” -Name “psdemovm62” -ResourceSettingResourceType “Microsoft.Network/networkInterfaces” -ResourceSettingTargetResourceName “psdemovm62”
```

**Salida prevista**

![Texto de salida después de agregar la NIC a la colección](./media/move-region-powershell/add-nic.png)

## <a name="add-the-source-resource-group"></a>Adición del grupo de recursos de origen

```azurepowershell-interactive
# Get the resource group name and resource type
Get-AzResource -ResourceId “/subscriptions/<subscription-id>/resourcegroups/psdemorm”

# Add the resource group to the collection. 
New-AzResourceMoverMoveResource -SubscriptionId “<subscription-id>” -ResourceGroupName “RegionMoveRG-centralus-westcentralus” -MoveCollectionName “MoveCollection-centralus-westcentralus” -SourceId “/subscriptions/<subscription-id>/resourcegroups/psdemorm” -Name “psdemorm” -ResourceSettingResourceType “resourcegroups” -ResourceSettingTargetResourceName “psdemorm”
```

**Salida prevista**

![Texto de salida después de agregar el grupo de recursos a la colección](./media/move-region-powershell/add-source-resource-group.png)

## <a name="recheck-dependencies"></a>Nueva comprobación de las dependencias

Vuelva a comprobar si faltan dependencias.

```azurepowershell-interactive
Get-AzResourceMoverUnresolvedDependency -MoveCollectionName “MoveCollection-centralus-westcentralus ” -ResourceGroupName “RegionMoveRG-centralus-westcentralus ” -SubscriptionId  “<subscription-id>”
```

**Salida**

Tenemos dependencias adicionales que faltan.

![Texto de salida tras comprobar de nuevo si hay dependencias](./media/move-region-powershell/recheck-dependencies.png)

## <a name="retrieve-additional-dependencies"></a>Recuperación de dependencias adicionales

```azurepowershell-interactive
# Identify dependencies
Get-AzResourceMoverUnresolvedDependency -MoveCollectionName “MoveCollection-centralus-westcentralus ” -ResourceGroupName “RegionMoveRG-centralus-westcentralus” -SubscriptionId  “<subscription-id>”
```

**Salida prevista**


![Texto de salida después de recuperar dependencias adicionales](./media/move-region-powershell/additional-missing-dependencies.png)

En este ejemplo, se identifican tres dependencias más que faltan:

- Dirección IP pública: psdemovm-ip
- Red virtual de Azure: psdemorm-vnet
- Grupo de seguridad de red (NSG): psdemovm-nsg

## <a name="add-additional-dependencies"></a>Adición de dependencias adicionales

1. [Siga las instrucciones](#add-dependencies-to-collection) para agregar estos recursos a la colección de movimiento.
2. Después de agregar recursos, vuelva a validarlos hasta que se agreguen todas las dependencias.


## <a name="prepare-and-move-the-source-resource-group"></a>Preparación y movimiento del grupo de recursos de origen

En la región de origen, debe preparar los recursos antes de moverlos. El proceso de preparación depende de los recursos que mueva. Por ejemplo, en el caso de recursos sin estado, puede preparar y exportar una plantilla de Azure Resource Manager (ARM). O bien, en el caso de recursos con estado, es posible que se inicie la replicación. 

Antes de poder preparar los recursos de origen, debe preparar y mover el grupo de recursos de origen.

### <a name="prepare"></a>Preparación

```azurepowershell-interactive
# Prepare the source resource group
Invoke-AzResourceMoverPrepare -SubscriptionId “<subscription-id>” -ResourceGroupName “RegionMoveRG-centralus-westcentralus ” -MoveCollectionName “MoveCollection-centralus-westcentralus ” -MoveResource “PSDemoRM”
```

**Salida prevista**

![Texto de salida después de preparar el grupo de recursos de origen](./media/move-region-powershell/prepare-source-resource-group.png)

### <a name="initiate-move"></a>Inicio del movimiento

```azurepowershell-interactive
# Initiate move
Invoke-AzResourceMoverInitiateMove -SubscriptionId “<subscription-id>” -ResourceGroupName “RegionMoveRG-centralus-westcentralus ” -MoveCollectionName “MoveCollection-centralus-westcentralus ” -MoveResource “PSDemoRM”

**Expected output**

![Output text after initiating move of the source resource group](./media/move-region-powershell/initiate-move-resource-group.png)


```azurepowershell-interactive
# Commit move
Invoke-AzResourceMoverCommit -SubscriptionId “<subscription-id” -ResourceGroupName “RegionMoveRG-centralus-westcentralus ” -MoveCollectionName “PS-centralus-
westcentralus-demoRM” -MoveResource “PSDemoRM”
```

**Salida prevista**

![Texto de salida después de confirmar el grupo de recursos de origen](./media/move-region-powershell/commit-move-resource-group.png)


## <a name="prepare-resources"></a>Preparación de recursos

Después de mover el grupo de recursos de origen a la región de destino, recupere una lista de los recursos de la colección de movimiento y prepárese para moverlos.

```azurepowershell-interactive
# Retrieve resources in the collection
Get-AzResourceMoverMoveResource  -SubscriptionId “ <subscription-id> “ -ResourceGroupName “RegionMoveRG-centralus-westcentralus ” -MoveCollectionName “MoveCollection-centralus-westcentralus ”   | Where-Object {  $_.MoveStatusMoveState -eq “PreparePending” } | Select Name
```
**Salida prevista**

![Texto de salida después de recuperar los recursos de la colección](./media/move-region-powershell/collection-resources.png)

Ahora, prepare los recursos.

```azurepowershell-interactive
# Prepare the resources, using the resource name
Invoke-AzResourceMoverPrepare -SubscriptionId  <subscription-id> -ResourceGroupName RegionMoveRG-centralus-westcentralus  -MoveCollectionName MoveCollection-centralus-westcentralus   -MoveResource $('psdemovm62', 'PSDemoVM-ip', 'PSDemoRM-vnet','PSDemoVM-nsg', ‘PSDemoVM’)
```
Como alternativa, puede preparar y mover los recursos con el identificador de recurso, en lugar del nombre:

```azurepowershell-interactive
# Prepare the resources using the resource ID
Invoke-AzResourceMoverPrepare -SubscriptionId  <subscription-id> -ResourceGroupName RegionMoveRG-centralus-westcentralus  -MoveCollectionName MoveCollection-centralus-westcentralus  -MoveResourceInputType MoveResourceSourceId  -MoveResource $('/subscriptions/<subscription-id>/resourceGroups/PSDemoRM/providers/Microsoft.Network/networkSecurityGroups/PSDemoVM-nsg')
```

**Salida prevista**

![Texto de salida después de preparar los recursos de la colección](./media/move-region-powershell/collection-prepare.png)

## <a name="initiate-resource-move"></a>Inicio del movimiento de recursos

Después de preparar los recursos, inicie el movimiento.

```azurepowershell-interactive
# Initiate the move 
Invoke-AzResourceMoverInitiateMove -SubscriptionId <subscription-id> -ResourceGroupName RegionMoveRG-centralus-westcentralus  -MoveCollectionName MoveCollection-centralus-westcentralus   -MoveResource $('psdemovm62', 'PSDemoVM-ip', 'PSDemoRM-vnet','PSDemoVM-nsg', ‘PSDemoVM’)
```
**Salida prevista**
![Texto de salida después de iniciar el movimiento de recursos](./media/move-region-powershell/initiate-resource-move.png)

## <a name="discard-or-commit-the-move"></a>Descarte o confirmación del movimiento

Tras la operación inicial de traslado, puede decidir si desea confirmarla o descartarla. 

- **Descartar**: puede descartar un traslado cuando esté haciendo pruebas y no quiera trasladar verdaderamente el recurso de origen. Si el traslado se descarta, el recurso volverá a tener el estado *Initiate move pending* (Iniciar movimiento pendiente). Después, puede volver a iniciar el movimiento si es necesario.
- **Confirmación**: la confirmación completa la operación de traslado a la región de destino. Tras la confirmación, un recurso de origen tendrá el estado *Delete source pending* (Eliminar origen pendiente) y el usuario puede decidir si desea eliminarlo.

### <a name="discard"></a>Discard (Descartar)

Para descartar el movimiento:

```azurepowershell-interactive
# Discard the move 
Invoke-AzResourceMoverDiscard -SubscriptionId  <subscription-id> `-ResourceGroupName RegionMoveRG-centralus-westcentralus  -MoveCollectionName MoveCollection-centralus-westcentralus   -MoveResource $('psdemovm62', 'PSDemoVM-ip', 'PSDemoRM-vnet','PSDemoVM-nsg', ‘PSDemoVM’)
```
**Salida prevista**
![Texto de salida después de descartar el movimiento](./media/move-region-powershell/discard-move.png)


### <a name="commit"></a>Commit

Para confirmar el movimiento:

```azurepowershell-interactive
# Commit the move 
Invoke-AzResourceMoverCommit -SubscriptionId  <subscription-id> -ResourceGroupName RegionMoveRG-centralus-westcentralus  -MoveCollectionName MoveCollection-centralus-westcentralus   -MoveResource $('psdemovm62', 'PSDemoVM-ip', 'PSDemoRM-vnet','PSDemoVM-nsg', ‘PSDemoVM’)
```
**Salida prevista**

![Texto de salida después de confirmar el movimiento](./media/move-region-powershell/commit-move.png)

## <a name="delete-source-resources"></a>Eliminación de los recursos de origen

Después de confirmar el movimiento y comprobar que los recursos funcionan según lo previsto en la región de destino, puede eliminar los recursos de origen mediante [Azure Portal](../azure-resource-manager/management/manage-resources-portal.md#delete-resources), [PowerShell](../azure-resource-manager/management/manage-resources-powershell.md#delete-resources) o la [CLI de Azure](../azure-resource-manager/management/manage-resources-cli.md#delete-resources).

## <a name="next-steps"></a>Pasos siguientes

[Aprenda a](remove-move-resources.md) eliminar recursos de una colección de movimiento.