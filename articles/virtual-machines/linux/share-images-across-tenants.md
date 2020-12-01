---
title: Uso compartido de imágenes de la galería entre inquilinos
description: Obtenga información sobre cómo compartir imágenes de VM entre inquilinos de Azure mediante Galerías de imágenes compartidas y ejemplos de Linux.
author: axayjo
ms.service: virtual-machines
ms.subservice: imaging
ms.workload: infrastructure-services
ms.topic: how-to
ms.date: 05/04/2019
ms.author: akjosh
ms.reviewer: cynthn
ms.custom: devx-track-azurecli
ms.openlocfilehash: a5e71f09179d414be84896a49a66480f19a0665d
ms.sourcegitcommit: 0a9df8ec14ab332d939b49f7b72dea217c8b3e1e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/18/2020
ms.locfileid: "94843594"
---
# <a name="share-gallery-vm-images-across-azure-tenants---linux-examples"></a>Uso compartido de imágenes de VM de la galería entre inquilinos de Azure: ejemplos de Linux

Las galerías de imágenes compartidas le permiten compartir imágenes mediante Azure RBAC. Puede usar Azure RBAC para compartir imágenes dentro de su inquilino e incluso con personas de fuera de él. Para más información sobre esta opción de uso compartido simple, consulte [Compartir la galería](./shared-images-portal.md#share-the-gallery).

[!INCLUDE [virtual-machines-share-images-across-tenants](../../../includes/virtual-machines-share-images-across-tenants.md)]

> [!IMPORTANT]
> No se puede usar el portal para implementar una VM desde una imagen en otro inquilino de Azure. Para crear una VM desde una imagen que se comparte entre los inquilinos, debe usar la CLI de Azure o [Powershell](../windows/share-images-across-tenants.md).

## <a name="create-a-vm-using-azure-cli"></a>Creación de una máquina virtual mediante la CLI de Azure

Inicie sesión en la entidad de servicio del inquilino 1 mediante el id. de la aplicación, la clave de aplicación y el identificador del inquilino 1. Puede usar `az account show --query "tenantId"` para obtener el identificador del inquilino, si es necesario.

```azurecli-interactive
az account clear
az login --service-principal -u '<app ID>' -p '<Secret>' --tenant '<tenant 1 ID>'
az account get-access-token 
```
 
Inicie sesión en la entidad de servicio del inquilino 2 mediante el id. de la aplicación, la clave de aplicación y el identificador del inquilino 2:

```azurecli-interactive
az login --service-principal -u '<app ID>' -p '<Secret>' --tenant '<tenant 2 ID>'
az account get-access-token
```

Cree la máquina virtual. Reemplace la información del ejemplo por la suya.

```azurecli-interactive
az vm create \
  --resource-group myResourceGroup \
  --name myVM \
  --image "/subscriptions/<Tenant 1 subscription>/resourceGroups/<Resource group>/providers/Microsoft.Compute/galleries/<Gallery>/images/<Image definition>/versions/<version>" \
  --admin-username azureuser \
  --generate-ssh-keys
```

## <a name="next-steps"></a>Pasos siguientes

Si experimenta algún problema, puede [solucionar problemas de galerías de imágenes compartidas](../troubleshooting-shared-images.md).
