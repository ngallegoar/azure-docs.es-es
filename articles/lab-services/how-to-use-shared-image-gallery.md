---
title: Usar una galería de imágenes compartidas en Azure Lab Services | Microsoft Docs
description: Aprenda a configurar una cuenta de laboratorio para usar una galería de imágenes compartidas para que un usuario pueda compartir una imagen con otro, y otro usuario pueda usar la imagen para crear una plantilla de máquina virtual en el laboratorio.
ms.topic: article
ms.date: 09/11/2020
ms.openlocfilehash: d9f4e75163f591680cc8f85ac42c1b6ada5f2365
ms.sourcegitcommit: 8e7316bd4c4991de62ea485adca30065e5b86c67
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/17/2020
ms.locfileid: "94647773"
---
# <a name="use-a-shared-image-gallery-in-azure-lab-services"></a>Usar una galería de imágenes compartidas en Azure Lab Services
En este artículo se muestra cómo los formadores y los administradores de laboratorio pueden guardar una imagen de máquina virtual de plantilla en una [galería de imágenes compartidas](../virtual-machines/windows/shared-image-galleries.md) para que otras personas puedan usarla para crear laboratorios. 

> [!IMPORTANT]
> Al usar una instancia de Shared Image Gallery, Azure Lab Services solo admite imágenes con menos de 128 GB de espacio en disco del sistema operativo. Las imágenes con más de 128 GB de espacio en disco o varios discos no se mostrarán en la lista de imágenes de máquina virtual durante la creación del laboratorio.

## <a name="scenarios"></a>Escenarios
Estos son los pares de escenarios compatibles con esta característica: 

- Un administrador de la cuenta de laboratorio adjunta una galería de imágenes compartidas a la cuenta de laboratorio y carga una imagen en la galería de imágenes compartidas fuera del contexto de un laboratorio. A continuación, los creadores del laboratorio pueden usar esa imagen de la galería de imágenes compartidas para crear laboratorios. 
- El administrador de una cuenta de laboratorio adjunta una galería de imágenes compartidas a la cuenta de laboratorio. Un creador del laboratorio (instructor) guarda la imagen personalizada de su laboratorio en la galería de imágenes compartidas. A continuación, otros creadores del laboratorio pueden seleccionar esta imagen de la galería de imágenes compartidas para crear una plantilla para sus laboratorios. 

    Cuando una imagen se guarda en una galería de imágenes compartida, Azure Lab Services replica la imagen guardada en otras regiones disponibles en la misma [ubicación geográfica](https://azure.microsoft.com/global-infrastructure/geographies/). Ello garantiza que la imagen está disponible para los laboratorios creados en otras regiones de la misma ubicación geográfica. Guardar las imágenes en una galería de imágenes compartida conlleva un costo adicional, lo que incluye el costo de todas las imágenes replicadas. Este costo es independiente del costo de uso de Azure Lab Services. Para más información sobre los precios de Shared Image Gallery, vea [Introducción a la galería de imágenes compartidas - Facturación]( https://docs.microsoft.com/azure/virtual-machines/windows/shared-image-galleries#billing).
    
## <a name="prerequisites"></a>Requisitos previos
- Tener creada una galería de imágenes compartidas con [Azure PowerShell](../virtual-machines/shared-images-powershell.md) o con la [CLI de Azure](../virtual-machines/shared-images-cli.md).
- Haber asociado la galería de imágenes compartidas a la cuenta de laboratorio. Para obtener instrucciones detalladas, vea [Asociar o desasociar una galería de imágenes compartidas en Azure Lab Services](how-to-attach-detach-shared-image-gallery.md).


## <a name="save-an-image-to-the-shared-image-gallery"></a>Guardar una imagen en la galería de imágenes compartidas
Una vez que se ha asociado una galería de imágenes compartidas, un administrador de cuenta de laboratorio o un formador puede guardar una imagen en la galería de imágenes compartidas para que otros formaodres la puedan reutilizar. 

1. En la página **Template** (Plantilla) del laboratorio, seleccione **Export to Shared Image Gallery** (Exportar a Shared Image Gallery) en la barra de herramientas.

    ![Botón Save image (Guardar imagen)](./media/how-to-use-shared-image-gallery/export-to-shared-image-gallery-button.png)
2. En el cuadro de diálogo **Export to Shared Image Gallery** (Exportar a Shared Image Gallery), escriba un **nombre para la imagen** y seleccione **Export** (Exportar). 

    ![Cuadro de diálogo Export to Shared Image Gallery (Exportar a Shared Image Gallery)](./media/how-to-use-shared-image-gallery/export-to-shared-image-gallery-dialog.png)
3. Puede ver el progreso de esta operación en la página **Template** (Plantilla). Esta operación puede tardar algún tiempo. 

    ![Exportación en curso](./media/how-to-use-shared-image-gallery/exporting-image-in-progress.png)
4. Cuando la exportación se haya realizado correctamente, verá el mensaje siguiente:

    ![Exportación finalizada](./media/how-to-use-shared-image-gallery/exporting-image-completed.png)

    Después de guardar la imagen en la galería de imágenes compartidas, puede usar esa imagen de la galería al crear otro laboratorio. También puede cargar una imagen en la galería de imágenes compartidas fuera del contexto de un laboratorio. Para más información, consulte [Introducción a la galería de imágenes compartidas](../virtual-machines/shared-images-powershell.md). 

    > [!IMPORTANT]
    > Cuando [guarda una imagen de plantilla de un laboratorio](how-to-use-shared-image-gallery.md#save-an-image-to-the-shared-image-gallery) de Azure Lab Services en una galería de imágenes compartidas, la imagen se carga en la galería como una **imagen especializada**. Las [imágenes especializadas](../virtual-machines/windows/shared-image-galleries.md#generalized-and-specialized-images) conservan la información específica de la máquina y los perfiles de usuario. Todavía puede cargar directamente una imagen generalizada en la galería fuera de Azure Lab Services.    

## <a name="use-an-image-from-the-shared-image-gallery"></a>Usar una imagen de la galería de imágenes compartidas
Un formador puede elegir una imagen personalizada disponible en la galería de imágenes compartidas para la plantilla durante la creación de un laboratorio.

![Usar una imagen de máquina virtual de la galería](./media/how-to-use-shared-image-gallery/use-shared-image.png)

> [!NOTE]
> Puede crear una plantilla de máquina virtual basada en imágenes **generalizadas** y **especializadas** en Azure Lab Services. 


## <a name="next-steps"></a>Pasos siguientes
Para obtener más información sobre las galerías de imágenes compartidas, vea [Galería de imágenes compartidas](../virtual-machines/windows/shared-image-galleries.md).