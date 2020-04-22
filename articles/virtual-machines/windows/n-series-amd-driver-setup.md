---
title: Instalación de controladores de GPU de AMD de la serie N de Azure para Windows
description: Instalación de controladores de GPU de NVIDIA para máquinas virtuales de la serie N que se ejecutan en Windows Server o Windows en Azure
services: virtual-machines-windows
author: vikancha
manager: jkabat
editor: ''
tags: azure-resource-manager
ms.service: virtual-machines-windows
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 12/4/2019
ms.author: vikancha
ms.openlocfilehash: 02213feb507e9a032a50241fddf31714b9dfd7ee
ms.sourcegitcommit: ae3d707f1fe68ba5d7d206be1ca82958f12751e8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/10/2020
ms.locfileid: "81011090"
---
# <a name="install-amd-gpu-drivers-on-n-series-vms-running-windows"></a>Instalación de controladores de GPU de AMD en máquinas virtuales de la serie N con Windows

Para aprovechar las funcionalidades de GPU de las nuevas máquinas virtuales de la serie NVv4 de Azure que ejecutan Windows, deben instalarse controladores de GPU de AMD. La extensión del controlador AMD estará disponible en las próximas semanas. En este artículo se describen sistemas operativos compatibles, controladores y pasos de instalación y verificación manuales.

Para conocer las especificaciones básicas, las capacidades de almacenamiento y los detalles del disco, consulte [Tamaño de máquinas virtuales para GPU Windows](sizes-gpu.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).



## <a name="supported-operating-systems-and-drivers"></a>Sistemas operativos y controladores compatibles

| SO | Controlador |
| -------- |------------- |
| Windows 10 EVD: compilación 1903 <br/><br/>Windows 10: compilación 1809<br/><br/>Windows Server 2016<br/><br/>Windows Server 2019 | [20.Q1.1](https://download.microsoft.com/download/3/8/9/3893407b-e8aa-4079-8592-735d7dd1c19a/Radeon-Pro-Software-for-Enterprise-GA.exe) (.exe) |


## <a name="driver-installation"></a>Instalación del controlador

1. Conéctese mediante Escritorio remoto a cada máquina virtual de la serie NVv4.

2. Si es un cliente de la versión preliminar de NVv4, detenga la máquina virtual y espere a que cambie al estado Detenido (desasignado).

3. Inicie la máquina virtual y descargue la versión más reciente de [AMD Cleanup Utility](https://download.microsoft.com/download/4/f/1/4f19b714-9304-410f-9c64-826404e07857/AMDCleanupUtilityni.exe). Ejecute "amdcleanuputility-x64.exe" para desinstalar el controlador existente. NO use ninguna utilidad de limpieza existente que se haya instalado con el controlador anterior.  

4. Descargue e instale el controlador más reciente.

5. Reinicie la máquina virtual.

## <a name="verify-driver-installation"></a>Comprobación de la instalación del controlador

Puede comprobar la instalación del controlador en el Administrador de dispositivos. En el ejemplo siguiente se muestra la configuración correcta de la tarjeta Radeon Instinct MI25 en una máquina virtual NVv4 de Azure.
<br />
![Propiedades del controlador de GPU](./media/n-series-amd-driver-setup/device-manager.png)

Puede usar dxdiag para comprobar las propiedades de presentación de GPU, incluida la RAM de vídeo. En el ejemplo siguiente, se muestra la partición de 1/2 de la tarjeta Radeon Instinct MI25 en una máquina virtual NVv4 de Azure.
<br />
![Propiedades del controlador de GPU](./media/n-series-amd-driver-setup/dxdiag-output.png)

Si ejecuta la compilación 1903 de Windows 10 o posterior, dxdiag no mostrará ninguna información en la pestaña "Mostrar". Use la opción "Guardar la información" de la parte inferior, y el archivo de salida mostrará la información relacionada con la GPU MI25 de AMD.

![Propiedades del controlador de GPU](./media/n-series-amd-driver-setup/dxdiag-details.png)


