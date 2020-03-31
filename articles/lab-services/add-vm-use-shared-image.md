---
title: Incorporar una máquina virtual mediante una imagen compartida en Azure DevTest Labs | Microsoft Docs
description: Obtenga información sobre cómo agregar una máquina virtual (VM) mediante una imagen de la galería de imágenes compartidas adjunta en Azure DevTest Labs.
services: devtest-lab,virtual-machines
documentationcenter: na
author: spelluru
manager: ''
editor: ''
ms.assetid: ''
ms.service: devtest-lab
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/02/2019
ms.author: spelluru
ms.openlocfilehash: c13b2c3d00a0ab0af0f7785feae645d907ffdaa3
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "68775788"
---
# <a name="add-a-vm-using-an-image-from-the-attached-shared-image-gallery"></a>Incorporación de una máquina virtual mediante una imagen de la galería de imágenes compartidas adjunta
Con Azure DevTest Labs, puede adjuntar una galería de imágenes compartidas a su laboratorio y, luego, usar las imágenes de la galería como bases para las máquinas virtuales que crea en el laboratorio. Para información sobre cómo adjuntar una galería de imágenes compartidas a un laboratorio, consulte [Configuración de una galería de imágenes compartidas en Azure DevTest Labs](configure-shared-image-gallery.md). En este artículo se muestra cómo agregar una máquina virtual al laboratorio mediante una imagen de la galería de imágenes compartidas adjunta como base. 

## <a name="azure-portal"></a>Portal de Azure
En esta sección, aprenderá a usar Azure Portal para agregar una máquina virtual a su laboratorio en función de una imagen de la galería de imágenes compartidas adjunta. En esta sección no se proporcionan instrucciones detalladas paso a paso para crear una máquina virtual con Azure Portal. Para ver esos detalles, consulte [Creación de una máquina virtual: Azure Portal](devtest-lab-add-vm.md). Solo resalta los pasos en los que se selecciona una imagen de la galería de imágenes compartidas adjunta y una versión de la imagen que quiere usar. 

Cuando agrega una máquina virtual al laboratorio, puede seleccionar una imagen de la galería de imágenes compartidas adjunta como imagen base: 

![Selección de una imagen compartida para la base](./media/add-vm-use-shared-image/select-shared-image-for-base.png)

A continuación, en la pestaña **Configuración avanzada** de la página **Crear recurso de laboratorio**, puede seleccionar la versión de la imagen que quiere usar como imagen base:

![Selección de la versión de imagen](./media/add-vm-use-shared-image/select-version-shared-image.png)

Puede cambiar para usar otra versión de la imagen después de que se crea la máquina virtual. 

## <a name="resource-manager-template"></a>Plantilla de Resource Manager
Si usa una plantilla de Azure Resource Manager para crear una máquina virtual con una imagen de la galería de imágenes compartidas, especifique un valor para **sharedImageId** en la sección **Propiedades**. Vea el ejemplo siguiente: 

```json
"resources": [
{
    ...
    "properties": {
         "sharedImageId": "/subscriptions/111111111-1111-1111-1111-111111111111/resourcegroups/mydtlrg/providers/microsoft.devtestlab/labs/mydtllab/sharedgalleries/spsig/sharedimages/myimagefromgallery",
        "sharedImageVersion": "1.0.1",
        ...
    }
}
],
```

Para ver un ejemplo de una plantilla de Resource Manager completa, consulte el ejemplo [Creación de una máquina virtual mediante una imagen de la galería de imágenes compartidas](https://github.com/Azure/azure-devtestlab/tree/master/samples/DevTestLabs/QuickStartTemplates/101-dtl-create-vm-username-pwd-sharedimage) en el repositorio de GitHub. 

## <a name="rest-api"></a>API DE REST

1. En primer lugar, debe obtener el identificador de la imagen en la galería de imágenes compartidas. Una forma de hacerlo es mostrando todas las imágenes en la galería de imágenes compartidas adjunta con el comando GET siguiente. 

    ```rest
    GET  https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.DevTestLab/labs/{labName}/sharedgalleries/{name}/sharedimages?api-version= 2018-10-15-preview
    ```
2. Invoque el método PUT en las máquinas virtuales pasando el identificador de la imagen compartida que recibió de la llamada anterior a `properties.SharedImageId`.

## <a name="next-steps"></a>Pasos siguientes
Para información sobre cómo adjuntar una galería de imágenes compartidas a un laboratorio y configurarla, consulte [Configuración de una galería de imágenes compartidas en Azure DevTest Labs](configure-shared-image-gallery.md).