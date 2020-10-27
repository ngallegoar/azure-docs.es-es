---
title: Creación de una oferta de máquina virtual de Azure en Azure Marketplace mediante su propia imagen
description: Aprenda a publicar una oferta de máquina virtual de Azure en Azure Marketplace mediante su propia imagen.
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: how-to
author: emuench
ms.author: krsh
ms.date: 10/20/2020
ms.openlocfilehash: 42022d1204c3b524ee2e9ef2770f616fba89dc8c
ms.sourcegitcommit: b6f3ccaadf2f7eba4254a402e954adf430a90003
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/20/2020
ms.locfileid: "92283381"
---
# <a name="how-to-create-a-virtual-machine-using-your-own-image"></a>Procedimiento para crear una máquina virtual mediante su propia imagen

En este artículo se describe cómo crear e implementar una imagen de máquina virtual (VM) proporcionada por el usuario.

> [!NOTE]
> Antes de comenzar este procedimiento, examine los [requisitos técnicos](marketplace-virtual-machines.md#technical-requirements) de las ofertas de máquina virtual de Azure, incluidos los requisitos de disco duro virtual (VHD).

Para usar una imagen base aprobada, siga las instrucciones de [Creación de una imagen de máquina virtual a partir de una base aprobada](azure-vm-create-using-approved-base.md).

## <a name="configure-the-vm"></a>Configuración de la máquina virtual

En esta sección se describe cómo ajustar el tamaño, actualizar y generalizar una máquina virtual de Azure. Estos pasos son necesarios para preparar la máquina virtual para su implementación en Azure Marketplace.

### <a name="size-the-vhds"></a>Ajuste del tamaño de los discos duros virtuales

[!INCLUDE [Discussion of VHD sizing](includes/vhd-size.md)]

### <a name="install-the-most-current-updates"></a>Instalar las actualizaciones más recientes

[!INCLUDE [Discussion of most current updates](includes/most-current-updates.md)]

### <a name="perform-additional-security-checks"></a>Realizar comprobaciones de seguridad adicionales

[!INCLUDE [Discussion of addition security checks](includes/additional-security-checks.md)]

### <a name="perform-custom-configuration-and-scheduled-tasks"></a>Realizar una configuración personalizada y tareas programadas

[!INCLUDE [Discussion of custom configuration and scheduled tasks](includes/custom-config.md)]

## <a name="upload-the-vhd-to-azure"></a>Carga del disco duro virtual en Azure

Configure y prepare la máquina virtual para que se cargue como se describe en los artículos sobre [preparación de un VHD o un VHDX de Windows antes de cargarlo en Azure](../virtual-machines/windows/prepare-for-upload-vhd-image.md) o [creación y carga de un disco duro virtual Linux](../virtual-machines/linux/create-upload-generic.md).

## <a name="extract-the-vhd-from-image-if-using-image-building-services"></a>Extracción del disco duro virtual de la imagen (si se usan servicios de creación de imágenes)

Si usa un servicio de creación de imágenes como [Packer](https://www.packer.io/), es posible que deba extraer el disco duro virtual de la imagen. No hay ninguna forma directa de hacerlo. Tendrá que crear una máquina virtual y extraer el disco duro virtual desde el disco de la máquina virtual.

### <a name="create-the-vm-on-the-azure-portal"></a>Creación de la máquina virtual en Azure Portal

Siga estos pasos para crear la imagen base de la máquina virtual en [Azure Portal](https://ms.portal.azure.com/).

1. Inicie sesión en [Azure Portal](https://ms.portal.azure.com/).
2. Seleccione **Máquinas virtuales** .
3. Seleccione **+ Agregar** para abrir la pantalla **Crear una máquina virtual** .
4. Seleccione la imagen en la lista desplegable, o bien **Examinar todas las imágenes públicas y privadas** si desea buscar o examinar todas las imágenes de máquina virtual disponibles.
5. Para crear una máquina virtual de **segunda generación** , vaya a la pestaña **Opciones avanzadas** y seleccione la opción **Generación 2** .

    :::image type="content" source="media/create-vm/vm-gen-option.png" alt-text="Selección Generación 1 o Generación 2.":::

6. Seleccione el tamaño de la máquina virtual que va a implementar.

    :::image type="content" source="media/create-vm/create-virtual-machine-sizes.png" alt-text="Selección Generación 1 o Generación 2.":::

7. Proporcione los otros detalles necesarios para crear la máquina virtual.
8. Seleccione **Revisar y crear** para revisar sus selecciones. Cuando aparezca el mensaje **Validación superada** , seleccione **Crear** .

Azure comienza el aprovisionamiento de la máquina virtual especificada. Para seguir su progreso, seleccione la pestaña **Máquinas virtuales** en el menú de la izquierda. En cuanto se crea, el estado de la máquina virtual cambia a **En ejecución** .

### <a name="connect-to-your-vm"></a>Conexión a la máquina virtual

Consulte la documentación siguiente para conectarse a una máquina virtual [Windows](../virtual-machines/windows/connect-logon.md) o [Linux](../virtual-machines/linux/ssh-from-windows.md#connect-to-your-vm).

[!INCLUDE [Discussion of addition security checks](includes/size-connect-generalize.md)]

## <a name="next-steps"></a>Pasos siguientes

- Siguiente paso recomendado: [Pruebe la imagen de la máquina virtual](azure-vm-image-test.md) para asegurarse de que cumple los requisitos de publicación de Azure Marketplace. Esta información es opcional.
- Si no prueba la imagen de la máquina virtual, vaya al artículo sobre la [generación del identificador URI de SAS](azure-vm-get-sas-uri.md).
- Si tiene dificultades para crear un disco duro virtual basado en Azure, consulte las [preguntas frecuentes que pueden surgir al crear una máquina virtual en Azure Marketplace](azure-vm-create-faq.md).
