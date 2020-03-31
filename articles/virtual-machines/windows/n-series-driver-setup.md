---
title: Instalación de controladores de GPU de la serie N de Azure para Windows
description: Instalación de controladores de GPU de NVIDIA para máquinas virtuales de la serie N que se ejecutan en Windows Server o Windows en Azure
services: virtual-machines-windows
author: cynthn
manager: gwallace
editor: ''
tags: azure-resource-manager
ms.assetid: f3950c34-9406-48ae-bcd9-c0418607b37d
ms.service: virtual-machines-windows
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 09/24/2018
ms.author: cynthn
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 631266f983886e3ca34d609b425f8a71b808b39f
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "77919403"
---
# <a name="install-nvidia-gpu-drivers-on-n-series-vms-running-windows"></a>Instalación de controladores de GPU de NVIDIA en VM de la serie N con Windows 

Para aprovechar las funcionalidades de GPU de las máquinas virtuales de la serie N de Azure que ejecutan Windows, deben instalarse controladores de GPU de NVIDIA. La [extensión de controlador de GPU de NVIDIA](../extensions/hpccompute-gpu-windows.md) instala los controladores CUDA de NVIDIA o GRID adecuados en una máquina virtual de la serie N. Instale o administre la extensión mediante Azure Portal o con herramientas como las plantillas de Azure PowerShell o Azure Resource Manager. Consulte la [documentación de la extensión de controlador de GPU de NVIDIA](../extensions/hpccompute-gpu-windows.md) para los sistemas operativos compatibles y los pasos de implementación.

Si decide instalar manualmente los controladores de GPU, este artículo proporciona pasos de instalación y verificación, controladores y los sistemas operativos compatibles. También está disponible la información de instalación manual del controlador para las [máquinas virtuales Linux](../linux/n-series-driver-setup.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

Para conocer las especificaciones básicas, las capacidades de almacenamiento y los detalles del disco, consulte [Tamaño de máquinas virtuales para GPU Windows](sizes-gpu.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). 

[!INCLUDE [virtual-machines-n-series-windows-support](../../../includes/virtual-machines-n-series-windows-support.md)]

## <a name="driver-installation"></a>Instalación del controlador

1. Conéctese mediante Escritorio remoto a cada máquina virtual de la serie N.

2. Descargue, extraiga e instale el controlador compatible con su sistema operativo Windows.

Después de la instalación de controladores de GRID en una VM, se requiere un reinicio. Después de la instalación de controladores de CUDA, no se requiere un reinicio.

## <a name="verify-driver-installation"></a>Comprobación de la instalación del controlador

Tenga en cuenta que el panel de control de Nvidia solo es accesible con la instalación del controlador GRID. Si ha instalado controladores CUDA, el panel de control de Nvidia no será visible.

Puede comprobar la instalación del controlador en el Administrador de dispositivos. En el ejemplo siguiente se muestra una configuración correcta de la tarjeta Tesla K80 en una máquina virtual de Azure NC.

![Propiedades del controlador de GPU](./media/n-series-driver-setup/GPU_driver_properties.png)

Para consultar el estado del dispositivo de GPU, ejecute la utilidad de línea de comandos [smi nvidia](https://developer.nvidia.com/nvidia-system-management-interface) que se instala con el controlador.

1. Abra un símbolo del sistema y cambie al directorio **C:\Program Files\NVIDIA Corporation\NVSMI**.

2. Ejecute `nvidia-smi`. Si el controlador está instalado, verá una salida parecida a la siguiente. La utilidad **GPU-Util** muestra **0 %** , salvo que se esté ejecutando una carga de trabajo de GPU en la máquina virtual. La versión del controlador y los detalles de GPU pueden ser diferentes de los que se muestran.

![Estado del dispositivo de NVIDIA](./media/n-series-driver-setup/smi.png)  

## <a name="rdma-network-connectivity"></a>Conectividad de red RDMA

La conectividad de red RDMA puede habilitarse en las máquinas virtuales de la serie N que puedan usar RDMA, como la NC24r, implementadas en el mismo conjunto de disponibilidad o en un único grupo de selección de red en un conjunto de escalado de máquinas virtuales. En las máquinas virtuales compatibles con RDMA, es necesario agregar la extensión HpcVmDrivers a las máquinas virtuales para instalar los controladores de dispositivos de red de Windows necesarios para la conectividad RDMA. Para agregar la extensión de máquina virtual a una máquina virtual de la serie N habilitada para RDMA, puede usar cmdlets de [Azure PowerShell](/powershell/azure/overview) para Azure Resource Manager.

Para instalar la versión más reciente de la extensión HpcVMDrivers 1.1 en una máquina virtual compatible con RDMA existente denominada "myVM" en la región de oeste de EE. UU.:
  ```powershell
  Set-AzVMExtension -ResourceGroupName "myResourceGroup" -Location "westus" -VMName "myVM" -ExtensionName "HpcVmDrivers" -Publisher "Microsoft.HpcCompute" -Type "HpcVmDrivers" -TypeHandlerVersion "1.1"
  ```
  Para obtener más información, consulte [Características y extensiones de las máquinas virtuales para Windows](extensions-features.md).

Ahora, la red RDMA admite el tráfico de interfaz de paso de mensajes (MPI) para aplicaciones que se ejecutan con [Microsoft MPI](https://docs.microsoft.com/message-passing-interface/microsoft-mpi) o Intel MPI 5.x. 


## <a name="next-steps"></a>Pasos siguientes

* Los desarrolladores que creen aplicaciones con aceleración por GPU para las GPU Tesla de NVIDIA también pueden descargar e instalar el último [CUDA Toolkit](https://developer.nvidia.com/cuda-downloads). Para obtener más información, consulte la [guía de instalación de CUDA](https://docs.nvidia.com/cuda/cuda-installation-guide-microsoft-windows/index.html#axzz4ZcwJvqYi).


