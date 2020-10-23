---
title: Creación de un disco administrado a partir de la versión de una imagen
description: Cree un disco administrado a partir de la versión de una imagen en una instancia de Shared Image Gallery.
author: cynthn
ms.service: virtual-machines
ms.subservice: imaging
ms.topic: how-to
ms.workload: infrastructure
ms.date: 10/06/2020
ms.author: cynthn
ms.reviewer: olayemio
ms.openlocfilehash: bf4a1feb91a1ac4b0bca0d6afdbac41a8be3aa4f
ms.sourcegitcommit: 2e72661f4853cd42bb4f0b2ded4271b22dc10a52
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/14/2020
ms.locfileid: "92049529"
---
# <a name="create-a-managed-disk-from-an-image-version"></a>Creación de un disco administrado a partir de la versión de una imagen

Si es necesario, puede crear un disco administrado a partir de la versión de una imagen almacenada en una instancia de Shared Image Gallery.


## <a name="cli"></a>CLI

Establezca la variable `source` como identificador de la versión de la imagen y, luego, use [az disk create](/cli/azure/disk.md#az_disk_create) para crear el disco administrado. 


Puede ver una lista de las versiones de una imagen mediante [az sig image-version list](/cli/azure/sig/image-version.md#az_sig_image_version_list). En este ejemplo, vamos a buscar todas las versiones de una imagen que forman parte de la definición de la imagen *myImageDefinition* en la galería de imágenes *myGallery*.

```azurecli-interactive
az sig image-version list \
   --resource-group myResourceGroup\
   --gallery-name myGallery \
   --gallery-image-definition myImageDefinition \
   -o table
```


En este ejemplo, se crea un disco administrado denominado *myManagedDisk* en la región *EastUS*, en un grupo de recursos denominado *myResourceGroup*. 

```azurecli-interactive
source="/subscriptions/<subscriptionId>/resourceGroups/<resourceGroupName>/providers/Microsoft.Compute/galleries/<galleryName>/images/<galleryImageDefinition>/versions/<imageVersion>"

az disk create --resource-group myResourceGroup --location EastUS --name myManagedDisk --gallery-image-reference $source 
```

Si se trata de un disco de datos, agregue `--gallery-image-reference-lun` para especificar el LUN.

## <a name="powershell"></a>PowerShell

Puede mostrar todas las versiones de una imagen mediante [Get-AzResource](/powershell/module/az.resources/get-azresource). 

```azurepowershell-interactive
Get-AzResource `
   -ResourceType Microsoft.Compute/galleries/images/versions | `
   Format-Table -Property Name,ResourceId,ResourceGroupName
```



Una vez que tenga toda la información que necesita, puede usar [Get-AzGalleryImageVersion](/powershell/module/az.compute/get-azgalleryimageversion) para obtener la versión de la imagen de origen que quiera usar y asignarla a una variable. En este ejemplo, vamos a obtener la versión de imagen `1.0.0` de la definición `myImageDefinition`, en la galería de origen `myGallery`, en el grupo de recursos `myResourceGroup`.

```azurepowershell-interactive
$sourceImgVer = Get-AzGalleryImageVersion `
   -GalleryImageDefinitionName myImageDefinition `
   -GalleryName myGallery `
   -ResourceGroupName myResourceGroup `
   -Name 1.0.0
```

Configure algunas variables correspondientes a la información del disco.

```azurepowershell-interactive
$location = "East US"
$resourceGroup = "myResourceGroup"
$diskName = "myDisk"
```

Cree una configuración de disco y, luego, el disco con el id. de la versión de la imagen de origen. En el caso de `-GalleryImageReference`, solo se necesita el LUN si el origen es un disco de datos.

```azurepowershell-interactive
$diskConfig = New-AzDiskConfig `
   -Location $location `
   -CreateOption FromImage `
   -GalleryImageReference @{Id = $sourceImgVer.Id; Lun=1}
```

Cree el disco.

```azurepowershell-interactive
New-AzDisk -Disk $diskConfig `
   -ResourceGroupName $resourceGroup `
   -DiskName $diskName
```

## <a name="next-steps"></a>Pasos siguientes

También puede crear la versión de una imagen a partir de un disco administrado mediante la [CLI de Azure](image-version-managed-image-cli.md) o [PowerShell](image-version-managed-image-powershell.md).


