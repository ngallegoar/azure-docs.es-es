---
title: Eliminación de recursos de una colección de desplazamiento en Azure Resource Mover
description: Obtenga información sobre cómo eliminar recursos de una colección de desplazamiento en Azure Resource Mover.
manager: evansma
author: rayne-wiselman
ms.service: resource-move
ms.topic: how-to
ms.date: 11/30/2020
ms.author: raynew
ms.openlocfilehash: 63548e2bf470c012e0dd8a5f879a51eeb631f453
ms.sourcegitcommit: 6a350f39e2f04500ecb7235f5d88682eb4910ae8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/01/2020
ms.locfileid: "96459278"
---
# <a name="manage-move-collections-and-resource-groups"></a>Administración de grupos de recursos y colecciones de transferencia de recursos

En esta artículo se describe cómo eliminar recursos de una colección de transferencia de recursos o bien cómo quitar un grupo de recursos o una colección de transferencia de recursos en [Azure Resource Mover](overview.md). Las colecciones de desplazamiento se usan al migrar recursos de Azure entre regiones de Azure.

## <a name="remove-a-resource-portal"></a>Eliminación de un recurso (portal)

Puede quitar recursos de una colección de transferencia de recursos en el portal de Resource Mover de la manera siguiente:

1. En **Entre regiones**, seleccione todos los recursos que quiere quitar de la colección y, luego, seleccione **Quitar**. 

    ![Botón que debe seleccionar para quitar](./media/remove-move-resources/portal-select-resources.png)

2. En **Quitar recursos**, haga clic en **Quitar**.

    ![Botón que debe seleccionar para quitar recursos de una colección de desplazamiento](./media/remove-move-resources/remove-portal.png)

## <a name="remove-a-move-collectionresource-group-portal"></a>Eliminación de una colección de transferencia de recursos o un grupo de recursos (portal)

Puede quitar una colección de transferencia de recursos o un grupo de recursos en el portal.

1. Siga las instrucciones del procedimiento anterior para quitar recursos de la colección. Si va a quitar un grupo de recursos, asegúrese de que no contiene ningún recurso.
2. Elimine la colección de transferencia de recursos o el grupo de recursos.  

## <a name="remove-a-resource-powershell"></a>Eliminación de un recurso (PowerShell)

Quite un recurso (en nuestro ejemplo, las máquinas PSDemoVM) de una colección mediante PowerShell, como se indica a continuación:

```azurepowershell-interactive
# Remove a resource using the resource ID
Remove-AzResourceMoverMoveResource -SubscriptionId  <subscription-id> -ResourceGroupName RegionMoveRG-centralus-westcentralus  -MoveCollectionName MoveCollection-centralus-westcentralus -Name PSDemoVM
```
**Salida prevista**

![Salida de texto después de quitar un recurso de una colección de desplazamiento](./media/remove-move-resources/remove-resource.png)

## <a name="remove-a-collection-powershell"></a>Eliminación de una colección (PowerShell)

Quite una colección de desplazamiento completa con PowerShell, como se indica a continuación:

1. Siga las instrucciones anteriores para quitar recursos de la colección mediante PowerShell.
2. Ejecute:

    ```azurepowershell-interactive
    # Remove a resource using the resource ID
    Remove-AzResourceMoverMoveCollection -SubscriptionId <subscription-id> -ResourceGroupName RegionMoveRG-centralus-westcentralus -MoveCollectionName MoveCollection-centralus-westcentralus
    ```
    **Salida prevista**
    
    ![Salida de texto después de quitar una colección de desplazamiento](./media/remove-move-resources/remove-collection.png)

## <a name="vm-resource-state-after-removing"></a>Estado del recurso de VM después de la eliminación

Lo que sucede cuando se quita un recurso de máquina virtual de una colección de desplazamiento depende del estado del recurso, tal y como se resume en la tabla.

###  <a name="remove-vm-state"></a>Eliminación de estado de VM
**Estado del recurso** | **VM** | **Redes**
--- | --- | --- 
**Agregado a una colección de desplazamiento** | Se elimina de la colección de desplazamiento. | Se elimina de la colección de desplazamiento. 
**Dependencias resueltas/acción "preparar" pendiente** | Se elimina de la colección de desplazamiento.  | Se elimina de la colección de desplazamiento. 
**Acción "preparar" en curso**<br/> (o cualquier otro estado en curso) | Error en la operación de eliminación.  | Error en la operación de eliminación.
**Error en la acción "preparar"** | Se elimina de la colección de desplazamiento.<br/>Se elimina cualquier elemento creado en la región de destino, incluidos los discos de réplica. <br/><br/> Los recursos de infraestructura creados durante el traslado deben eliminarse manualmente. | Se elimina de la colección de desplazamiento.  
**Acción "iniciar movimiento" pendiente** | Se elimina de la colección de desplazamiento.<br/><br/> Se elimina cualquier elemento creado en la región de destino, incluidas las VM, los discos de réplica, etc.  <br/><br/> Los recursos de infraestructura creados durante el traslado deben eliminarse manualmente. | Se elimina de la colección de desplazamiento.
**Error en la acción "iniciar movimiento"** | Se elimina de la colección de desplazamiento.<br/><br/> Se elimina cualquier elemento creado en la región de destino, incluidas las VM, los discos de réplica, etc.  <br/><br/> Los recursos de infraestructura creados durante el traslado deben eliminarse manualmente. | Se elimina de la colección de desplazamiento.
**Confirmación pendiente** | Se recomienda descartar el traslado para que los recursos de destino se eliminen primero.<br/><br/> El recurso vuelve al estado **Acción "iniciar movimiento" pendiente** y puede continuar desde allí. | Se recomienda descartar el traslado para que los recursos de destino se eliminen primero.<br/><br/> El recurso vuelve al estado **Acción "iniciar movimiento" pendiente** y puede continuar desde allí. 
**Error de confirmación** | Se recomienda descartar para que los recursos de destino se eliminen primero.<br/><br/> El recurso vuelve al estado **Acción "iniciar movimiento" pendiente** y puede continuar desde allí. | Se recomienda descartar el traslado para que los recursos de destino se eliminen primero.<br/><br/> El recurso vuelve al estado **Acción "iniciar movimiento" pendiente** y puede continuar desde allí.
**Acción "descartar" completada** | El recurso vuelve al estado **Acción "iniciar movimiento" pendiente**.<br/><br/> Se elimina de la colección de desplazamiento, junto con los elementos creados en la máquina virtual de destino: la VM, los discos de réplica, el almacén, etc.  <br/><br/> Los recursos de infraestructura creados durante el traslado deben eliminarse manualmente. <br/><br/> Los recursos de infraestructura creados durante el traslado deben eliminarse manualmente. |  El recurso vuelve al estado **Acción "iniciar movimiento" pendiente**.<br/><br/> Se eliminó de la colección de desplazamiento.
**Error en la acción "descartar"** | Se recomienda descartar los traslados para que los recursos de destino se eliminen primero.<br/><br/> A continuación, el recurso vuelve al estado **Acción "iniciar movimiento" pendiente** y puede continuar desde allí. | Se recomienda descartar los traslados para que los recursos de destino se eliminen primero.<br/><br/> A continuación, el recurso vuelve al estado **Acción "iniciar movimiento" pendiente** y puede continuar desde allí.
**Acción "eliminar origen" pendiente** | Se eliminó de la colección de desplazamiento.<br/><br/> No se elimina ningún elemento creado en la región de destino.  | Se eliminó de la colección de desplazamiento.<br/><br/> No se elimina ningún elemento creado en la región de destino.
**Error en la acción "eliminar origen"** | Se eliminó de la colección de desplazamiento.<br/><br/> No se elimina ningún elemento creado en la región de destino. | Se eliminó de la colección de desplazamiento.<br/><br/> No se elimina ningún elemento creado en la región de destino.

## <a name="sql-resource-state-after-removing"></a>Estado del recurso de SQL después de la eliminación

Lo que sucede cuando se quita un recurso de Azure SQL de una colección de desplazamiento depende del estado del recurso, tal y como se resume en la tabla.

**Estado del recurso** | **SQL** 
--- | --- 
**Agregado a una colección de desplazamiento** | Se elimina de la colección de desplazamiento. 
**Dependencias resueltas/acción "preparar" pendiente** | Se elimina de la colección de desplazamiento. 
**Acción "preparar" en curso**<br/> (o cualquier otro estado en curso)  | Error en la operación de eliminación. 
**Error en la acción "preparar"** | Se elimina de la colección de desplazamiento.<br/><br/>Se eliminan todos los elementos creados en la región de destino. 
**Acción "iniciar movimiento" pendiente** |  Se elimina de la colección de desplazamiento.<br/><br/>Se eliminan todos los elementos creados en la región de destino. La base de datos SQL existe en este momento y se eliminará. 
**Error en la acción "iniciar movimiento"** | Se elimina de la colección de desplazamiento.<br/><br/>Se eliminan todos los elementos creados en la región de destino. La base de datos SQL existe en este momento y se debe eliminar. 
**Confirmación pendiente** | Se recomienda descartar el traslado para que los recursos de destino se eliminen primero.<br/><br/> El recurso vuelve al estado **Acción "iniciar movimiento" pendiente** y puede continuar desde allí.
**Error de confirmación** | Se recomienda descartar el traslado para que los recursos de destino se eliminen primero.<br/><br/> El recurso vuelve al estado **Acción "iniciar movimiento" pendiente** y puede continuar desde allí. 
**Acción "descartar" completada** |  El recurso vuelve al estado **Acción "iniciar movimiento" pendiente**.<br/><br/> Se elimina de la colección de desplazamiento, junto con todos los elementos creados en el destino, incluidas las bases de datos SQL. 
**Error en la acción "descartar"** | Se recomienda descartar los traslados para que los recursos de destino se eliminen primero.<br/><br/> A continuación, el recurso vuelve al estado **Acción "iniciar movimiento" pendiente** y puede continuar desde allí. 
**Acción "eliminar origen" pendiente** | Se eliminó de la colección de desplazamiento.<br/><br/> No se elimina ningún elemento creado en la región de destino. 
**Error en la acción "eliminar origen"** | Se eliminó de la colección de desplazamiento.<br/><br/> No se elimina ningún elemento creado en la región de destino. 

## <a name="next-steps"></a>Pasos siguientes

Pruebe a [migrar una máquina virtual](tutorial-move-region-virtual-machines.md) a otra región con Resource Mover.
