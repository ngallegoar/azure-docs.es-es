---
title: 'Tutorial: Creación de imágenes de máquina virtual personalizadas con Azure PowerShell'
description: En este tutorial, aprenderá a usar Azure PowerShell para crear una imagen de máquina virtual personalizada en Azure.
documentationcenter: virtual-machines
author: cynthn
manager: gwallace
tags: azure-resource-manager
ms.service: virtual-machines-windows
ms.topic: tutorial
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 11/30/2018
ms.author: cynthn
ms.custom: mvc
ms.openlocfilehash: 7360798f2f95184145a856babf501e3080cbaaf4
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/24/2020
ms.locfileid: "76274200"
---
# <a name="tutorial-create-a-custom-image-of-an-azure-vm-with-azure-powershell"></a>Tutorial: Creación de una imagen personalizada de una máquina virtual de Azure con Azure PowerShell

Las imágenes personalizadas son como las imágenes de Marketplace, pero las puede crear usted mismo. Las imágenes personalizadas pueden usarse en el arranque de las implementaciones y para garantizar la coherencia entre varias máquinas virtuales. En este tutorial, creará su propia imagen personalizada de una máquina virtual de Azure con PowerShell. Aprenderá a:

> [!div class="checklist"]
> * Ejecutar Sysprep y generalizar máquinas virtuales
> * Crear una imagen personalizada
> * Crear una imagen personalizada a partir de una máquina virtual
> * Enumerar todas las imágenes en su suscripción
> * Eliminar una imagen

En la versión preliminar pública, tenemos el servicio [Image Builder de Azure para máquinas virtuales](https://docs.microsoft.com/azure/virtual-machines/windows/image-builder-overview). Solo tiene que describir sus personalizaciones en una plantilla y esta servirá para llevar a cabo los pasos de creación de imágenes de este artículo. [Pruebe Azure Image Builder (versión preliminar)](https://docs.microsoft.com/azure/virtual-machines/windows/image-builder).

## <a name="before-you-begin"></a>Antes de empezar

Los siguientes pasos explican cómo tomar una máquina virtual existente y convertirla en una imagen personalizada reutilizable que puede usar para crear nuevas instancias de máquinas virtuales.

Para completar el ejemplo de este tutorial, debe tener una máquina virtual. Si es necesario, este [script de ejemplo](../scripts/virtual-machines-windows-powershell-sample-create-vm.md) puede crear una. Al trabajar en el tutorial, reemplace los nombres de grupo de recursos y máquina virtual cuando proceda.

## <a name="launch-azure-cloud-shell"></a>Inicio de Azure Cloud Shell

Azure Cloud Shell es un shell interactivo gratuito que puede usar para ejecutar los pasos de este artículo. Tiene las herramientas comunes de Azure preinstaladas y configuradas para usarlas en la cuenta. 

Para abrir Cloud Shell, seleccione **Pruébelo** en la esquina superior derecha de un bloque de código. También puede ir a [https://shell.azure.com/powershell](https://shell.azure.com/powershell) para iniciar Cloud Shell en una pestaña independiente del explorador. Seleccione **Copiar** para copiar los bloques de código, péguelos en Cloud Shell y, luego, presione Entrar para ejecutarlos.

## <a name="prepare-vm"></a>Preparación de la VM

Para crear una imagen de una máquina virtual, debe preparar la máquina virtual de origen; para ello, debe generalizarla, desasignarla y después marcarla como generalizada con Azure.

### <a name="generalize-the-windows-vm-using-sysprep"></a>Generalización de VM con Windows mediante Sysprep

Entre otras características, Sysprep elimina toda la información personal de la cuenta y prepara, entre otras cosas, la máquina para usarse como imagen. Para obtener más información sobre Sysprep, vea [Uso de Sysprep: Introducción](https://technet.microsoft.com/library/bb457073.aspx).


1. Conexión a una máquina virtual.
2. Abra una ventana del símbolo del sistema como administrador. Cambie el directorio a *%windir%\system32\sysprep* y, después, ejecute `sysprep.exe`.
3. En **Herramienta de preparación del sistema**, seleccione **Iniciar la Configuración rápida (OOBE)** y asegúrese de que la casilla **Generalizar** está activada.
4. En **Opciones de apagado**, seleccione **Apagar** y, a continuación, haga clic en **Aceptar**.
5. Cuando Sysprep finaliza, apaga la máquina virtual. **No reinicie la VM**.

### <a name="deallocate-and-mark-the-vm-as-generalized"></a>Desasignar y marcar la máquina virtual como generalizada

Para crear una imagen, la VM debe desasignarse y estar marcada como generalizada en Azure.

Desasigne la máquina virtual mediante [Stop-AzVM](https://docs.microsoft.com/powershell/module/az.compute/stop-azvm).

```azurepowershell-interactive
Stop-AzVM `
   -ResourceGroupName myResourceGroup `
   -Name myVM -Force
```

Establezca el estado de la máquina virtual en `-Generalized` mediante [Set-AzVm](https://docs.microsoft.com/powershell/module/az.compute/set-azvm). 
   
```azurepowershell-interactive
Set-AzVM `
   -ResourceGroupName myResourceGroup `
   -Name myVM -Generalized
```


## <a name="create-the-image"></a>Crear la imagen

Ahora puede crear una imagen de la máquina virtual mediante [New-AzImageConfig](https://docs.microsoft.com/powershell/module/az.compute/new-azimageconfig) y [New-AzImage](https://docs.microsoft.com/powershell/module/az.compute/new-azimage). En el ejemplo siguiente se crea una imagen denominada *myImage* a partir de la máquina virtual llamada *myVM*.

Obtenga la máquina virtual. 

```azurepowershell-interactive
$vm = Get-AzVM `
   -Name myVM `
   -ResourceGroupName myResourceGroup
```

Cree la configuración de la imagen.

```azurepowershell-interactive
$image = New-AzImageConfig `
   -Location EastUS `
   -SourceVirtualMachineId $vm.ID 
```

Cree la imagen.

```azurepowershell-interactive
New-AzImage `
   -Image $image `
   -ImageName myImage `
   -ResourceGroupName myResourceGroup
``` 

 
## <a name="create-vms-from-the-image"></a>Creación de máquinas virtuales a partir de la imagen

Ahora que tiene una imagen, puede crear una o varias VM desde la imagen. Crear una máquina virtual a partir de una imagen personalizada es muy similar a crear una máquina virtual mediante una imagen de Marketplace. Cuando use una imagen de Marketplace, tendrá que proporcionar información sobre la imagen, el proveedor de esa imagen, la oferta, la SKU y la versión. Mediante el parámetro simplificado establecido para el cmdlet [New-AzVM](https://docs.microsoft.com/powershell/module/az.compute/new-azvm), basta con que proporcione el nombre de la imagen personalizada, siempre que se encuentre en el mismo grupo de recursos. 

En este ejemplo se crea una máquina virtual denominada *myVMfromImage* a partir de la imagen *myImage*, en *myResourceGroup*.


```azurepowershell-interactive
New-AzVm `
    -ResourceGroupName "myResourceGroup" `
    -Name "myVMfromImage" `
    -ImageName "myImage" `
    -Location "East US" `
    -VirtualNetworkName "myImageVnet" `
    -SubnetName "myImageSubnet" `
    -SecurityGroupName "myImageNSG" `
    -PublicIpAddressName "myImagePIP" `
    -OpenPorts 3389
```

Se recomienda limitar el número de implementaciones simultáneas a 20 máquinas virtuales desde una sola imagen. Si planea realizar implementaciones simultáneas a gran escala de más de 20 máquinas virtuales a partir de la misma imagen personalizada, debe usar [Shared Image Gallery](shared-image-galleries.md) con varias réplicas de imágenes. 


## <a name="image-management"></a>Administración de imágenes 

Estos son algunos ejemplos de tareas de imágenes administradas comunes y cómo completarlas con PowerShell.

Enumere todas las imágenes por nombre.

```azurepowershell-interactive
$images = Get-AzResource -ResourceType Microsoft.Compute/images 
$images.name
```

Elimine una imagen. Este ejemplo elimina la imagen *myImage* de *myResourceGroup*.

```azurepowershell-interactive
Remove-AzImage `
    -ImageName myImage `
    -ResourceGroupName myResourceGroup
```

## <a name="next-steps"></a>Pasos siguientes

En este tutorial, ha creado una imagen de máquina virtual personalizada. Ha aprendido a:

> [!div class="checklist"]
> * Ejecutar Sysprep y generalizar máquinas virtuales
> * Crear una imagen personalizada
> * Crear una imagen personalizada a partir de una máquina virtual
> * Enumerar todas las imágenes en su suscripción
> * Eliminar una imagen

En al siguiente tutorial aprenderá a crear máquinas virtuales de alta disponibilidad.

> [!div class="nextstepaction"]
> [Creación de máquinas virtuales de alta disponibilidad](tutorial-availability-sets.md)



