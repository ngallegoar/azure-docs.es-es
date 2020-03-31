---
title: Usar una galería de imágenes compartidas en Azure Lab Services | Microsoft Docs
description: Aprenda a configurar una cuenta de laboratorio para usar una galería de imágenes compartidas para que un usuario pueda compartir una imagen con otro, y otro usuario pueda usar la imagen para crear una plantilla de máquina virtual en el laboratorio.
services: lab-services
documentationcenter: na
author: spelluru
manager: ''
editor: ''
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/24/2020
ms.author: spelluru
ms.openlocfilehash: c611ecdb5a2534f7368e533e3e19e6e3f96de57f
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "78190456"
---
# <a name="use-a-shared-image-gallery-in-azure-lab-services"></a>Usar una galería de imágenes compartidas en Azure Lab Services
En este artículo se explica cómo los profesores y administradores del laboratorio cómo pueden guardar una imagen de máquina virtual de plantilla para que otros usuarios puedan reutilizarla. Estas imágenes se guardan en una [galería de imágenes compartidas](../../virtual-machines/windows/shared-image-galleries.md) de Azure. Como primer paso, el administrador del laboratorio asocia una galería de imágenes compartidas existente a la cuenta de laboratorio. Una vez que se ha asociado la galería de imágenes compartidas, los laboratorios creados en la cuenta de laboratorio pueden guardar imágenes en la galería de imágenes compartidas. Otros profesores pueden seleccionar esta imagen de la galería de imágenes compartidas para crear una plantilla para sus clases. 

> [!NOTE]
> Actualmente, Azure Lab Services admite la creación de máquinas virtuales de plantilla basadas solo en imágenes de máquina virtual **generalizadas** (no imágenes especializadas) en una galería de imágenes compartidas. 

## <a name="prerequisites"></a>Prerequisites
- Tener creada una galería de imágenes compartidas con [Azure PowerShell](../../virtual-machines/windows/shared-images.md) o con la [CLI de Azure](../../virtual-machines/linux/shared-images.md).
- Haber asociado la galería de imágenes compartidas a la cuenta de laboratorio. Para obtener instrucciones detalladas, vea [Asociar o desasociar una galería de imágenes compartidas en Azure Lab Services](how-to-attach-detach-shared-image-gallery.md).


## <a name="save-an-image-to-the-shared-image-gallery"></a>Guardar una imagen en la galería de imágenes compartidas
Una vez que se ha asociado una galería de imágenes compartidas, un administrador de cuenta de laboratorio o un profesor puede guardar una imagen en la galería de imágenes compartidas para que otros profesores la puedan reutilizar. 

1. En la página **Template** (Plantilla) del laboratorio, seleccione **Export to Shared Image Gallery** (Exportar a Shared Image Gallery) en la barra de herramientas.

    ![Botón Save image (Guardar imagen)](../media/how-to-use-shared-image-gallery/export-to-shared-image-gallery-button.png)
2. En el cuadro de diálogo **Export to Shared Image Gallery** (Exportar a Shared Image Gallery), escriba un **nombre para la imagen** y seleccione **Export** (Exportar). 

    ![Cuadro de diálogo Export to Shared Image Gallery (Exportar a Shared Image Gallery)](../media/how-to-use-shared-image-gallery/export-to-shared-image-gallery-dialog.png)
3. Puede ver el progreso de esta operación en la página **Template** (Plantilla). Esta operación puede tardar algún tiempo. 

    ![Exportación en curso](../media/how-to-use-shared-image-gallery/exporting-image-in-progress.png)
4. Cuando la exportación se haya realizado correctamente, verá el mensaje siguiente:

    ![Exportación finalizada](../media/how-to-use-shared-image-gallery/exporting-image-completed.png)

También puede cargar una imagen en la galería de imágenes compartidas fuera del contexto de un laboratorio. Para más información, consulte [Introducción a la galería de imágenes compartidas](../../virtual-machines/windows/shared-images.md). 

## <a name="use-an-image-from-the-shared-image-gallery"></a>Usar una imagen de la galería de imágenes compartidas
Un profesor puede elegir una imagen personalizada disponible en la galería de imágenes compartidas para la plantilla durante la creación de un laboratorio.

![Usar una imagen de máquina virtual de la galería](../media/how-to-use-shared-image-gallery/use-shared-image.png)

## <a name="next-steps"></a>Pasos siguientes
Para obtener más información sobre las galerías de imágenes compartidas, vea [Galería de imágenes compartidas](../../virtual-machines/windows/shared-image-galleries.md).
