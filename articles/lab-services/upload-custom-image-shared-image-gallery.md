---
title: 'Azure Lab Services: carga de una imagen personalizada en Shared Image Gallery'
description: Se describe cómo cargar una imagen personalizada en Shared Image Gallery. Los departamentos de TI de la universidad podrán importar imágenes especialmente beneficiosas.
ms.date: 09/30/2020
ms.topic: how-to
ms.openlocfilehash: 93b4141636b629168e9bb7a73e71a9fe4bfc39f5
ms.sourcegitcommit: 8e7316bd4c4991de62ea485adca30065e5b86c67
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/17/2020
ms.locfileid: "94654650"
---
# <a name="upload-a-custom-image-to-shared-image-gallery"></a>Carga de una imagen compartida en Shared Image Gallery

Shared Image Gallery está disponible para importar sus propias imágenes personalizadas para crear laboratorios en Azure Lab Services. Los departamentos de TI de la universidad podrán importar imágenes especialmente beneficiosas por los siguientes motivos: 

* No tiene que crear imágenes manualmente mediante una máquina virtual de plantillas del laboratorio.
* Puede cargar imágenes creadas con otras herramientas, como SCCM, Endpoint Manager, etc.

En este artículo se describen los pasos que se pueden llevar a cabo para obtener una imagen personalizada y usarla en Azure Lab Services. 

> [!IMPORTANT]
> Al mover las imágenes desde un entorno de laboratorio físico a Az Labs, debe reestructurarlas correctamente. No vuelva a reutilizar las imágenes existentes de laboratorios físicos. <br/>Para más información, vea la entrada de blog [Traslado de un laboratorio físico a Azure Lab Services](https://techcommunity.microsoft.com/t5/azure-lab-services/moving-from-a-physical-lab-to-azure-lab-services/ba-p/1654931).

## <a name="bring-custom-image-from-a-physical-lab-environment"></a>Obtener una imagen personalizada de un entorno de laboratorio físico

En los pasos siguientes se explica cómo importar una imagen personalizada que se inicia desde un entorno de laboratorio físico. A continuación, se crea un VHD desde este entorno y se importa en Shared Image Gallery en Azure para que se pueda usar en Azure Lab Services.

Existen muchas opciones para crear un VHD a partir de un entorno de laboratorio físico. En los pasos siguientes se muestra cómo crear un VHD a partir de una máquina virtual de Hyper-V con Windows:

1. Comience con una máquina virtual de Hyper-V en el entorno de laboratorio físico que se ha creado a partir de la imagen.
    1. La máquina virtual debe crearse como una máquina virtual de generación 1.
    1. La máquina virtual debe usar un tamaño de disco fijo. También puede especificar el tamaño del disco en esta ventana. El tamaño del disco no debe ser mayor que 128 GB.<br/>    
    Azure Lab Services no admite imágenes con un tamaño de disco mayor que 128 GB. 
       
        :::image type="content" source="./media/upload-custom-image-shared-image-gallery/connect-virtual-hard-disk.png" alt-text="Conectar disco duro virtual":::   
    1. Cree una imagen de la VM como lo haría normalmente.
1. [Conéctese a la máquina virtual y prepárela para Azure](../virtual-machines/windows/prepare-for-upload-vhd-image.md).
    1. [Establecimiento de configuraciones de Windows para Azure](../virtual-machines/windows/prepare-for-upload-vhd-image.md#set-windows-configurations-for-azure)
    1. [Comprobación de los servicios de Windows que son los mínimos necesarios para garantizar la conectividad de la máquina virtual](../virtual-machines/windows/prepare-for-upload-vhd-image.md#check-the-windows-services)
    1. [Actualización de la configuración del Registro de Escritorio remoto](../virtual-machines/windows/prepare-for-upload-vhd-image.md#update-remote-desktop-registry-settings)
    1. [Configuración de reglas de firewall de Windows](../virtual-machines/windows/prepare-for-upload-vhd-image.md#configure-windows-firewall-rules)
    1. Instalación de actualizaciones de Windows
    1. [Instalación del agente de máquinas virtuales de Azure y configuración adicional como se muestra aquí](../virtual-machines/windows/prepare-for-upload-vhd-image.md#complete-the-recommended-configurations) 
    
    En los pasos anteriores se creará una imagen especializada. Si crea una imagen generalizada, también necesitará ejecutar [SysPrep](../virtual-machines/windows/prepare-for-upload-vhd-image.md#determine-when-to-use-sysprep). <br/>
        Debe crear una imagen especializada si desea mantener el directorio de usuario (que puede contener archivos, información de la cuenta de usuario, etc.) que el software incluido en la imagen necesita.
1. Como **Hyper-V** crea un archivo **VHDX** de forma predeterminada, debe convertirlo en un archivo VHD.
    1. Vaya a **Administrador de Hyper-V** -> **Acción** -> **Editar disco**.
    1. Aquí, tendrá la opción de **Convertir** el disco de un VHDX a un VHD.
    1. Al intentar ampliar el tamaño del disco, asegúrese de no superar los 128 GB.        
        :::image type="content" source="./media/upload-custom-image-shared-image-gallery/choose-action.png" alt-text="Elegir acción":::   
1. Cargue el VHD en Azure para crear un disco administrado.
    1. Puede usar el Explorador de Storage o AzCopy desde la línea de comandos, como se describe en [Carga de un VHD en Azure o copia de un disco administrado en otra región](../virtual-machines/windows/disks-upload-vhd-to-managed-disk-powershell.md).        
    Si el equipo entra en suspensión o se bloquea, es posible que el proceso de carga se interrumpa y se produzca un error.
    1. El resultado de este paso es que ahora tiene un disco administrado que puede ver en Azure Portal. 
        Puede usar la pestaña "Tamaño\Rendimiento" de Azure Portal para elegir el tamaño del disco. Como se mencionó antes, el tamaño no debe ser mayor que 128 GB.
1. Realice una instantánea del disco administrado.
    Esto puede realizarse desde PowerShell, mediante Azure Portal o desde el Explorador de Storage, como se describe en [Creación de una instantánea mediante el portal o PowerShell](../virtual-machines/windows/snapshot-copy-managed-disk.md).
1. En Shared Image Gallery, cree una versión y la definición de un imagen:
    1. [Cree la definición de una imagen](../virtual-machines/windows/shared-images-portal.md#create-an-image-definition).
    1. También debe especificar aquí si va a crear una imagen especializada o generalizada.
1. Cree el laboratorio en Azure Lab Services y seleccione la imagen personalizada en Shared Image Gallery.

    Si expandió el disco después de instalar el sistema operativo en la máquina virtual de Hyper-V original, también tendrá que extender la unidad C en Windows para usar el espacio en disco sin asignar. Para ello, inicie sesión en la máquina virtual de plantilla una vez creado el laboratorio y, después, siga pasos similares a los que se indican en [Extensión de un volumen básico](/windows-server/storage/disk-management/extend-a-basic-volume). Hay opciones para hacerlo en la interfaz de usuario, así como mediante PowerShell.

## <a name="next-steps"></a>Pasos siguientes

* [Introducción a la galería de imágenes compartidas](../virtual-machines/windows/shared-image-galleries.md)
* [Uso de Shared Image Gallery](how-to-use-shared-image-gallery.md)