---
title: Creación de una máquina virtual a partir de una versión de imagen especializada mediante la CLI de Azure
description: Cree una máquina virtual mediante una versión de imagen especializada en Shared Image Gallery mediante la CLI de Azure.
author: cynthn
ms.service: virtual-machines
ms.subservice: imaging
ms.workload: infrastructure-services
ms.topic: how-to
ms.date: 04/23/2020
ms.author: cynthn
ms.reviewer: akjosh
ms.openlocfilehash: 1ccf03deee2a2f72c1eb2008e1acc5bf67d16447
ms.sourcegitcommit: e0330ef620103256d39ca1426f09dd5bb39cd075
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/05/2020
ms.locfileid: "82793896"
---
# <a name="create-a-vm-using-a-specialized-image-version-with-the-azure-cli"></a>Creación de una máquina virtual mediante una versión de imagen especializada con la CLI de Azure

Cree una máquina virtual a partir de una [versión de imagen especializada](https://docs.microsoft.com/azure/virtual-machines/linux/shared-image-galleries#generalized-and-specialized-images) almacenada en Shared Image Gallery. Si desea crear una máquina virtual mediante una versión de imagen generalizada, consulte [Creación de una máquina virtual a partir de una versión de imagen generalizada](vm-generalized-image-version-cli.md).

Reemplace los nombres de los recursos según sea necesario en este ejemplo. 

Enumere las definiciones de imagen en una galería mediante [az sig image-definition list](/cli/azure/sig/image-definition#az-sig-image-definition-list) para ver el nombre y el identificador de las definiciones.

```azurecli-interactive 
resourceGroup=myGalleryRG
gallery=myGallery
az sig image-definition list \
   --resource-group $resourceGroup \
   --gallery-name $gallery \
   --query "[].[name, id]" \
   --output tsv
```

Cree la máquina virtual con [az vm create](/cli/azure/vm#az-vm-create), usando el parámetro --specialized para indicar que la imagen es una imagen especializada. 

Use el identificador de la definición de imagen en `--image` a fin de crear la máquina virtual a partir de la versión más reciente de la imagen que está disponible. También puede crear la máquina virtual a partir de una versión específica si proporciona el identificador de la versión de la imagen en `--image`. 

En este ejemplo, se creará una máquina virtual a partir de la versión más reciente de la imagen *myImageDefinition*.

```azurecli
az group create --name myResourceGroup --location eastus
az vm create --resource-group myResourceGroup \
    --name myVM \
    --image "/subscriptions/<Subscription ID>/resourceGroups/myGalleryRG/providers/Microsoft.Compute/galleries/myGallery/images/myImageDefinition" \
    --specialized
```


## <a name="next-steps"></a>Pasos siguientes
[Azure Image Builder (versión preliminar)](./linux/image-builder-overview.md) puede ayudar a automatizar la creación de versiones de la imagen, incluso se puede usar para actualizar y [crear una nueva versión de la imagen a partir de una versión de imagen existente](./linux/image-builder-gallery-update-image-version.md). 

Puede crear también recursos de galería de imágenes compartidas con plantillas. Hay varias plantillas de Inicio rápido de Azure disponibles: 

- [Creación de una galería de imágenes compartidas](https://azure.microsoft.com/resources/templates/101-sig-create/)
- [Creación de una definición de imagen en una galería de imágenes compartidas](https://azure.microsoft.com/resources/templates/101-sig-image-definition-create/)
- [Creación de una versión de imagen en una galería de imágenes compartidas](https://azure.microsoft.com/resources/templates/101-sig-image-version-create/)
- [Creación de una máquina virtual a partir de la versión de la imagen](https://azure.microsoft.com/resources/templates/101-vm-from-sig/)


