---
title: Implementación de máquinas virtuales en el dispositivo Azure Stack Edge Pro a través de Azure Portal
description: Aprenda a crear y administrar máquinas virtuales en el dispositivo Azure Stack Edge Pro a través de Azure Portal.
services: databox
author: alkohli
ms.service: databox
ms.subservice: edge
ms.topic: how-to
ms.date: 11/02/2020
ms.author: alkohli
Customer intent: As an IT admin, I need to understand how to configure compute on Azure Stack Edge Pro device so I can use it to transform the data before sending it to Azure.
ms.openlocfilehash: 89ef196cb5a124b8b1100871c408400f3fceef5c
ms.sourcegitcommit: 6a350f39e2f04500ecb7235f5d88682eb4910ae8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/01/2020
ms.locfileid: "96465788"
---
# <a name="deploy-vms-on-your-azure-stack-edge-pro-gpu-device-via-the-azure-portal"></a>Implementación de máquinas virtuales en el dispositivo Azure Stack Edge Pro con GPU a través de Azure Portal

Puede crear y administrar máquinas virtuales en un dispositivo Azure Stack Edge con Azure Portal, plantillas, cmdlets de Azure PowerShell y a través de scripts de Python/la CLI de Azure. En este artículo se describe cómo crear y administrar una máquina virtual en un dispositivo Azure Stack Edge mediante Azure Portal. 

Este artículo se aplica a los dispositivos de Azure Stack Edge con GPU, Azure Stack Edge Pro R y Azure Stack Edge Mini R. 

## <a name="vm-deployment-workflow"></a>Flujo de trabajo de implementación de una máquina virtual

El resumen general del flujo de trabajo de implementación es el siguiente:

1. Habilite una interfaz de red para el proceso en el dispositivo Azure Stack Edge. Esto crea un conmutador virtual en la interfaz de red especificada.
1. Habilite la administración en la nube de máquinas virtuales desde Azure Portal.
1. Cargue un VHD en una cuenta de Azure Storage mediante el Explorador de Storage. 
1. Use el VHD cargado para descargar el disco duro virtual en el dispositivo y crear una imagen de máquina virtual a partir del disco duro virtual. 
1. Use los recursos creados en los pasos anteriores:
    1. Imagen de máquina virtual que creó.
    1. VSwitch asociado con la interfaz de red en la que habilitó el proceso.
    1. Subred asociada con VSwitch.

    Y cree o especifique los recursos insertados siguientes:
    1. Nombre de la VM, elija un tamaño de VM compatible, credenciales de inicio de sesión para la VM. 
    1. Cree discos de datos nuevos o conecte discos de datos existentes.
    1. Configure una dirección IP estática o dinámica para la VM. Si proporciona una dirección IP estática, elija una dirección IP libre en el intervalo de la subred de la interfaz de red habilitada para el proceso.

    Use los recursos anteriores para crear una máquina virtual.


## <a name="prerequisites"></a>Requisitos previos

Antes de empezar a crear y administrar máquinas virtuales en el dispositivo a través de Azure Portal, debe asegurarse de lo siguiente:

1. Ha completado la configuración de red en el dispositivo Azure Stack Edge Pro como se describe en [Paso 1: configuración de un dispositivo Azure Stack Edge Pro](azure-stack-edge-j-series-connect-resource-manager.md#step-1-configure-azure-stack-edge-pro-device).

    1. Ha habilitado una interfaz de red para el proceso. Esta dirección IP de la interfaz de red se utiliza para crear un conmutador virtual para la implementación de la máquina virtual. En la interfaz de usuario local del dispositivo, vaya a **Proceso**. Seleccione la interfaz de red que va a usar para crear un conmutador virtual.

        > [!IMPORTANT] 
        > Solo puede configurar un puerto para Proceso.

    1. Habilite Proceso en la interfaz de red. Azure Stack Edge Pro crea y administra el conmutador virtual correspondiente a esa interfaz de red.

1. Tiene acceso a un disco duro virtual de Windows o Linux que usará para crear la imagen de VM de la máquina virtual que desea crear.

## <a name="deploy-a-vm"></a>Implementación de una máquina virtual

Siga estos pasos para crear una máquina virtual en el dispositivo Azure Stack Edge.

### <a name="add-a-vm-image"></a>Incorporación de una imagen de máquina virtual

1. Cargue un VHD en una cuenta de Azure Storage. Siga los pasos descritos en [Carga de un VHD con el Explorador de Azure Storage](../devtest-labs/devtest-lab-upload-vhd-using-storage-explorer.md).

1. En Azure Portal, vaya al recurso de Azure Stack Edge correspondiente a su dispositivo de Azure Stack Edge. Vaya a **Proceso perimetral > Máquinas virtuales**.

    ![Agregar imagen de VM 1](media/azure-stack-edge-gpu-deploy-virtual-machine-portal/add-virtual-machine-image-1.png)

1. Seleccione **Máquinas virtuales** para ir a la página de **información general**. **Habilite** la administración en la nube de máquinas virtuales.
    ![Agregar imagen de VM 2](media/azure-stack-edge-gpu-deploy-virtual-machine-portal/add-virtual-machine-image-2.png)

1. El primer paso es agregar una imagen de VM. En el paso anterior, ya cargó un VHD en la cuenta de almacenamiento. Usará este VHD para crear una imagen de máquina virtual.

    Seleccione **Agregar imagen** para descargar el disco duro virtual de la cuenta de almacenamiento y agregarlo al dispositivo. El proceso de descarga tarda varios minutos según el tamaño del VHD y el ancho de banda de Internet disponible para la descarga. 

    ![Agregar imagen de VM 3](media/azure-stack-edge-gpu-deploy-virtual-machine-portal/add-virtual-machine-image-3.png)

1. En la hoja **Agregar imagen**, escriba los parámetros siguientes. Seleccione **Agregar**.


    |Parámetro  |Descripción  |
    |---------|---------|
    |Descargar del blob de almacenamiento    |Vaya a la ubicación del blob de almacenamiento en la cuenta de almacenamiento donde se cargó el VHD.         |
    |Descargar a    | Establézcalo automáticamente en el dispositivo actual en el que se implementa la máquina virtual.        |
    |Guardar imagen como      | El nombre de la imagen de máquina virtual que va a crear a partir del VHD que ha cargado en la cuenta de almacenamiento.        |
    |Tipo de SO     |Elija entre Windows o Linux como el sistema operativo del VHD que usará para crear la imagen de máquina virtual.         |
   

    ![Agregar imagen de VM 4](media/azure-stack-edge-gpu-deploy-virtual-machine-portal/add-virtual-machine-image-6.png)

1. Se descarga el VHD y se crea la imagen de VM. La creación de la imagen tarda varios minutos en completarse. Verá una notificación de la finalización correcta de la imagen de VM.

    ![Agregar imagen de VM 5](media/azure-stack-edge-gpu-deploy-virtual-machine-portal/add-virtual-machine-image-8.png)


1. Una vez que la imagen de VM se crea correctamente, se agrega a la lista de imágenes de la hoja **Imágenes**.
    ![Agregar imagen de VM 6](media/azure-stack-edge-gpu-deploy-virtual-machine-portal/add-virtual-machine-image-9.png)

    La hoja **Implementaciones** se actualiza para mostrar el estado de la implementación.

    ![Agregar imagen de VM 7](media/azure-stack-edge-gpu-deploy-virtual-machine-portal/add-virtual-machine-image-10.png)

    La imagen que se acaba de agregar también se muestra en la página de **información general**.
    ![Agregar imagen de VM 8](media/azure-stack-edge-gpu-deploy-virtual-machine-portal/add-virtual-machine-image-11.png)


### <a name="add-a-vm"></a>Agregar una VM

Siga estos pasos para crear una máquina virtual después de crear una imagen de VM.

1. En la página de **información general**, seleccione **Agregar máquina virtual**.

    ![Agregar VM 1](media/azure-stack-edge-gpu-deploy-virtual-machine-portal/add-virtual-machine-1.png)

1. En la pestaña **Aspectos básicos**, escriba los parámetros siguientes.


    |Parámetro |Descripción  |
    |---------|---------|
    |Nombre de la máquina virtual     |         |
    |Imagen     | Seleccione de las imágenes de VM disponibles en el dispositivo.        |
    |Size     | Elija entre los [tamaños de VM compatibles](azure-stack-edge-gpu-deploy-virtual-machine-templates.md#supported-vm-sizes).        |
    |Nombre de usuario     | Use el nombre de usuario predeterminado *azureuser*.        |
    |Tipo de autenticación    | Elija una clave pública SSH o una contraseña definida por el usuario.       |
    |Contraseña     | Escriba una contraseña para iniciar sesión en la máquina virtual. La contraseña debe tener un mínimo de 12 caracteres y cumplir los [requisitos de complejidad](../virtual-machines/windows/faq.md#what-are-the-password-requirements-when-creating-a-vm) definidos.        |
    |Confirmar contraseña    | Vuelva a escribir la contraseña.        |


    ![Agregar VM 2](media/azure-stack-edge-gpu-deploy-virtual-machine-portal/add-virtual-machine-basics-1.png)

    Seleccione **Siguiente: Discos**.

1. En la pestaña **Discos**, conectará discos a la VM. 
    
    1. Puede elegir **Crear y conectar un nuevo disco** o **Conectar un disco existente**.

        ![Agregar VM 3](media/azure-stack-edge-gpu-deploy-virtual-machine-portal/add-virtual-machine-disks-1.png)

    1. Seleccione **Crear y conectar un nuevo disco**. En la hoja **Crear nuevo disco**, proporcione un nombre para el disco y el tamaño en GiB.

        ![Agregar VM 4](media/azure-stack-edge-gpu-deploy-virtual-machine-portal/add-virtual-machine-disks-2.png)

    1.  Repita el procedimiento anterior para agregar más discos. Una vez creados los discos, aparecerán en la pestaña **Discos**.

        ![Agregar VM 5](media/azure-stack-edge-gpu-deploy-virtual-machine-portal/add-virtual-machine-disks-3.png)

        Seleccione **Siguiente: Redes**.

1. En la pestaña **Redes**, configurará la conectividad de red para la VM.

    
    |Parámetro  |Descripción |
    |---------|---------|
    |Virtual network    | En la lista desplegable, seleccione el conmutador virtual que se creó en el dispositivo Azure Stack Edge cuando se habilitó el proceso en la interfaz de red.    |
    |Subnet     | Este campo se rellena automáticamente con la subred asociada a la interfaz de red en la que se habilitó el proceso.         |
    |Dirección IP     | Proporcione una dirección IP estática o dinámica para la VM. La dirección IP estática debe ser una dirección IP libre disponible del intervalo de subred especificado.        |

    ![Agregar VM 6](media/azure-stack-edge-gpu-deploy-virtual-machine-portal/add-virtual-machine-networking-1.png)

    Seleccione **Siguiente: Revisar y crear**.

1. En la pestaña **Revisar y crear**, revise las especificaciones de la VM y seleccione **Crear**.

    ![Agregar VM 7](media/azure-stack-edge-gpu-deploy-virtual-machine-portal/add-virtual-machine-review-create-1.png)

1. Se inicia la creación de la VM y puede tardar hasta 20 minutos. Puede ir a **Implementaciones** para administrar la creación de la VM.

    ![Agregar VM 8](media/azure-stack-edge-gpu-deploy-virtual-machine-portal/add-virtual-machine-deployments-page-1.png)

    
1. Una vez que la VM se crea correctamente, la página de **información general** se actualizará para mostrar la VM nueva.

    ![Agregar VM 9](media/azure-stack-edge-gpu-deploy-virtual-machine-portal/add-virtual-machine-overview-page-1.png)

1. Seleccione la VM recién creada para ir a **Máquinas virtuales**.

    ![Agregar VM 10](media/azure-stack-edge-gpu-deploy-virtual-machine-portal/add-virtual-machine-page-1.png)

    Seleccione la máquina virtual para ver los detalles. 

    ![Agregar VM 11](media/azure-stack-edge-gpu-deploy-virtual-machine-portal/add-virtual-machine-details-1.png)

## <a name="connect-to-a-vm"></a>Conexión a una máquina virtual

Los pasos para conectarse pueden ser diferentes dependiendo de si se ha creado una máquina virtual Windows o Linux. No se puede conectar a las máquinas virtuales implementadas en el dispositivo a través de Azure Portal. Debe completar los pasos siguientes para conectarse a la VM Linux o Windows.

### <a name="connect-to-linux-vm"></a>Conexión a una máquina virtual Linux

Siga estos pasos para conectarse a una máquina virtual Linux.

[!INCLUDE [azure-stack-edge-gateway-connect-vm](../../includes/azure-stack-edge-gateway-connect-virtual-machine-linux.md)]

### <a name="connect-to-windows-vm"></a>Conexión a una máquina virtual Windows

Siga estos pasos para conectarse a una máquina virtual Windows.

[!INCLUDE [azure-stack-edge-gateway-connect-vm](../../includes/azure-stack-edge-gateway-connect-virtual-machine-windows.md)]

## <a name="next-steps"></a>Pasos siguientes

Para información sobre cómo administrar el dispositivo Azure Stack Edge Pro, consulte [Uso de la interfaz de usuario web local para administrar Azure Stack Edge Pro](azure-stack-edge-manage-access-power-connectivity-mode.md).
