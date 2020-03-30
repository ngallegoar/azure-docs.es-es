---
title: archivo de inclusión
description: archivo de inclusión
services: virtual-machines-windows
author: cynthn
ms.service: virtual-machines-windows
ms.topic: include
ms.date: 02/11/2019
ms.author: cynthn
ms.custom: include file
ms.openlocfilehash: 40e5a1bf940e46aed566a1e3fa6dcb4e6b2d9230
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "77135074"
---
## <a name="supported-operating-systems-and-drivers"></a>Sistemas operativos y controladores compatibles

### <a name="nvidia-tesla-cuda-drivers"></a>Controladores NVIDIA Tesla (CUDA)

Los controladores NVIDIA Tesla (CUDA) para máquinas virtuales de las series NC, NCv2, NCv3, ND y NDv2 (y opcionales para la serie NV) solo se admiten en las distribuciones de sistemas operativos enumeradas en la tabla siguiente. Los vínculos de descarga de controladores están actualizados en el momento de la publicación. Para ver los controladores más recientes, visite el sitio web de [NVIDIA](https://www.nvidia.com/).

> [!TIP]
> Como alternativa a la instalación manual de controladores de CUDA en una máquina virtual de Windows Server, puede implementar una imagen de [Data Science Virtual Machine](../articles/machine-learning/data-science-virtual-machine/overview.md) de Azure. Las ediciones de DSVM para Windows Server 2016 preinstalan los controladores NVIDIA CUDA, la biblioteca CUDA Deep Neural Network Library y otras herramientas.


| SO | Controlador |
| -------- |------------- |
| Windows Server 2016 | [398.75](https://us.download.nvidia.com/Windows/Quadro_Certified/398.75/398.75-tesla-desktop-winserver2016-international.exe) (.exe) |
| Windows Server 2012 R2 | [398.75](https://us.download.nvidia.com/Windows/Quadro_Certified/398.75/398.75-tesla-desktop-winserver2008-2012r2-64bit-international.exe) (.exe) |

### <a name="nvidia-grid-drivers"></a>Controladores de NVIDIA GRID

Microsoft redistribuye los instaladores del controlador NVIDIA GRID para VM de las series NV y NVv3 que se emplean como estaciones de trabajo virtuales o para aplicaciones virtuales. Instale estos controladores GRID en VM de la serie NV de Azure y solo en los sistemas operativos enumerados en la tabla siguiente. Estos controladores incluyen licencias del software GRID Virtual GPU en Azure. No es necesario configurar un servidor de licencias de software vGPU NVIDIA.

Tenga en cuenta que la extensión de NVIDIA siempre instalará el controlador más reciente. Aquí se proporcionan vínculos a la versión anterior para los clientes que tienen dependencias de una versión anterior.

En Windows Server 2019, Windows Server 2016 y Windows 10 (hasta la compilación 1909):
- [GRID 10.1 (442.06)](https://go.microsoft.com/fwlink/?linkid=874181) (.exe)
- [GRID 10.0 (441.66)](https://download.microsoft.com/download/2/a/3/2a316e62-3be9-4ddb-ae8e-c04b6df6e22d/441.66_grid_win10_server2016_server2019_64bit_international.exe) (.exe) 

Para Windows Server 2012 R2, Windows Server 2008 R2, Windows 8 y Windows 7: 
- [GRID 10.1 (442.06)](https://go.microsoft.com/fwlink/?linkid=874184) (.exe)
- [GRID 10.0 (441.66)](https://download.microsoft.com/download/d/8/0/d80091f8-0d55-47c2-958a-bacd136f432a/441.66_grid_win7_win8_server2008R2_server2012R2_64bit_international.exe) (.exe)  
