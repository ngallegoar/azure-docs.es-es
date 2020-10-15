---
title: archivo de inclusión
description: archivo de inclusión
services: virtual-machines
author: cynthn
ms.service: virtual-machines
ms.topic: include
ms.date: 04/28/2020
ms.author: cynthn
ms.custom: include file
ms.openlocfilehash: fc60d1266f4aad4c337611f0fa32fe8293744559
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "82792282"
---
## <a name="list-information"></a>Visualización de la información

Enumere todas las galerías por nombre.

```azurepowershell-interactive
$galleries = Get-AzResource -ResourceType Microsoft.Compute/galleries
$galleries.Name
```

Enumere todas las definiciones de una imagen por nombre.

```azurepowershell-interactive
$imageDefinitions = Get-AzResource -ResourceType Microsoft.Compute/galleries/images
$imageDefinitions.Name
```

Enumere todas las versiones de una imagen por nombre.

```azurepowershell-interactive
$imageVersions = Get-AzResource -ResourceType Microsoft.Compute/galleries/images/versions
$imageVersions.Name
```

Elimine la versión de una imagen. En este ejemplo se elimina la versión de la imagen denominada *1.0.0*.

```azurepowershell-interactive
Remove-AzGalleryImageVersion `
   -GalleryImageDefinitionName myImageDefinition `
   -GalleryName myGallery `
   -Name 1.0.0 `
   -ResourceGroupName myGalleryRG
```





