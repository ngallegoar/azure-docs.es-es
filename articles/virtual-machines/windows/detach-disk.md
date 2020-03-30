---
title: 'Desconexión de un disco de datos de una máquina virtual de Windows: Azure'
description: Desconecte un disco de datos de una máquina virtual de Azure creada mediante el modelo de implementación de Resource Manager.
services: virtual-machines-windows
author: cynthn
manager: gwallace
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.topic: article
ms.date: 01/08/2020
ms.author: cynthn
ms.subservice: disks
ms.openlocfilehash: 301f3abd26f702f3f29c8833c835ba7d0e41bcaf
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "75834601"
---
# <a name="how-to-detach-a-data-disk-from-a-windows-virtual-machine"></a>Desacoplamiento de un disco de datos de una máquina virtual de Windows

Cuando ya no necesite un disco de datos que se encuentra conectado a una máquina virtual, puede desconectarlo fácilmente. Esto quita el disco de la máquina virtual, pero no lo quita del almacenamiento.

> [!WARNING]
> Si se desconecta un disco, no se elimina automáticamente. Si se ha suscrito a Almacenamiento premium, seguirá acumulando cargos de almacenamiento por el disco. Para más información, vea [Precios y facturación al utilizar Premium Storage](disks-types.md#billing).

Si desea volver a usar los datos existentes en el disco, puede acoplarlo de nuevo a la misma máquina virtual (o a otra).

 

## <a name="detach-a-data-disk-using-powershell"></a>Desconexión de un disco de datos con PowerShell

También puede quitar un disco de datos *en caliente* con PowerShell, pero asegúrese de que no haya nada que use activamente el disco antes de desconectarlo de la máquina virtual.

En este ejemplo, se quita el disco denominado **myDisk** de la máquina virtual **myVM** del grupo de recursos **myResourceGroup**. Quite primero el disco con el cmdlet [Remove-AzVMDataDisk](https://docs.microsoft.com/powershell/module/az.compute/remove-azvmdatadisk). Luego, actualice el estado de la máquina virtual con el cmdlet [Update-AzVM](https://docs.microsoft.com/powershell/module/az.compute/update-azvm) para completar el proceso de eliminación del disco de datos.

```azurepowershell-interactive
$VirtualMachine = Get-AzVM `
   -ResourceGroupName "myResourceGroup" `
   -Name "myVM"
Remove-AzVMDataDisk `
   -VM $VirtualMachine `
   -Name "myDisk"
Update-AzVM `
   -ResourceGroupName "myResourceGroup" `
   -VM $VirtualMachine
```

El disco permanece en el almacenamiento pero ya no está acoplado a una máquina virtual.

## <a name="detach-a-data-disk-using-the-portal"></a>Desconexión de un disco de datos mediante el portal

También puede quitar un disco de datos *en caliente*, pero asegúrese de que no haya nada que use activamente el disco antes de desconectarlo de la máquina virtual.

1. En el menú de la izquierda, seleccione **Máquinas virtuales**.
1. Seleccione la máquina virtual que tiene el disco de datos que quiere desasociar.
1. En **Configuración**, seleccione **Discos**.
1. En la parte superior del panel **Discos**, seleccione **Editar**.
1. En el panel **Discos**, en el extremo derecho del disco de datos que quiere desasociar, seleccione **Desasociar**.
1. Seleccione **Guardar** en la parte superior de la página para guardar los cambios.

El disco permanece en el almacenamiento pero ya no está acoplado a una máquina virtual.

## <a name="next-steps"></a>Pasos siguientes

Si desea reutilizar el disco de datos, basta con que lo [conecte a otra máquina virtual](attach-managed-disk-portal.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
