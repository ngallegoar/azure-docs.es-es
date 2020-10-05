---
title: Eliminación de recursos de una colección de desplazamiento en Azure Resource Mover
description: Obtenga información sobre cómo eliminar recursos de una colección de desplazamiento en Azure Resource Mover.
manager: evansma
author: rayne-wiselman
ms.service: resource-move
ms.topic: how-to
ms.date: 09/08/2020
ms.author: raynew
ms.openlocfilehash: 241ccbda67f7a2518d0c44a0d362673922ad4284
ms.sourcegitcommit: 3be3537ead3388a6810410dfbfe19fc210f89fec
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/10/2020
ms.locfileid: "89652726"
---
# <a name="remove-resources-from-a-move-collection"></a>Eliminación de recursos de una colección de desplazamiento

En esta artículo se describe cómo eliminar recursos de una colección de desplazamiento en [Azure Resource Mover](overview.md). Las colecciones de desplazamiento se usan al migrar recursos de Azure entre regiones de Azure.

## <a name="remove-a-resource-portal"></a>Eliminación de un recurso (portal)

Quite un elemento en el portal de Resource Mover como se muestra a continuación:

1. En **Entre regiones**, seleccione los recursos que quiere quitar de la colección, > **Quitar**.

    ![Botón que debe seleccionar para quitar](./media/remove-move-resources/portal-select-resources.png)

1. En **Quitar recursos**, haga clic en **Quitar**.

    ![Botón que debe seleccionar para quitar recursos de una colección de desplazamiento](./media/remove-move-resources/remove-portal.png)

## <a name="remove-a-resource-powershell"></a>Eliminación de un recurso (PowerShell)

Quite un recurso (en nuestro ejemplo, las máquinas PSDemoVM) de una colección mediante PowerShell, como se indica a continuación:

```azurepowershell-interactive
# Remove a resource using the resource ID
Remove-AzResourceMoverMoveResource -SubscriptionId  <subscription-id> -ResourceGroupName RegionMoveRG-centralus-westcentralus  -MoveCollectionName MoveCollection-centralus-westcentralus - Name PSDemoVM
```
**Salida esperada**
![Salida de texto después de quitar un recurso de una colección de desplazamiento](./media/remove-move-resources/remove-resource.png)

## <a name="remove-a-collection-powershell"></a>Eliminación de una colección (PowerShell)

Quite una colección de desplazamiento completa con PowerShell, como se indica a continuación:

```azurepowershell-interactive
# Remove a resource using the resource ID
Remove-AzResourceMoverMoveResource -SubscriptionId  <subscription-id> -ResourceGroupName RegionMoveRG-centralus-westcentralus  -MoveCollectionName MoveCollection-centralus-westcentralus 
```
**Salida esperada**
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