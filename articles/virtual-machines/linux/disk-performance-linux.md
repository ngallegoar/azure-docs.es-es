---
title: Rendimiento de la máquina virtual y del disco
description: Obtenga más información sobre cómo funcionan en conjunto las máquinas virtuales y sus discos conectados para el rendimiento
author: albecker1
ms.author: albecker
ms.date: 10/12/2020
ms.topic: conceptual
ms.service: virtual-machines
ms.subservice: disks
ms.openlocfilehash: 092368bb66784a00d5116da0b6be6513f8ebb261
ms.sourcegitcommit: 59f506857abb1ed3328fda34d37800b55159c91d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/24/2020
ms.locfileid: "92518100"
---
# <a name="virtual-machine-and-disk-performance"></a>Rendimiento de la máquina virtual y del disco
[!INCLUDE [VM and Disk Performance](../../../includes/virtual-machine-disk-performance.md)]

## <a name="virtual-machine-uncached-vs-cached-limits"></a>Límites de almacenamiento en caché frente a sin caché para máquinas virtuales
Las máquinas virtuales que están habilitadas tanto para Premium Storage como para el almacenamiento en caché de Premium Storage tienen dos límites de ancho de banda de almacenamiento diferentes. Ahora se examinará la máquina virtual Standard_D8s_v3 como ejemplo. Esta es la documentación de la [serie Dsv3](../dv3-dsv3-series.md) y la máquina Standard_D8s_v3:

[!INCLUDE [VM and Disk Performance](../../../includes/virtual-machine-disk-performance-2.md)]

Se ejecutará una prueba comparativa en esta combinación de máquina virtual y disco que crea la actividad de E/S. Para obtener información sobre cómo realizar pruebas comparativas de E/S de almacenamiento en Azure, vea [Pruebas comparativas de la aplicación en Azure Disk Storage](disks-benchmarks.md). Desde la herramienta de pruebas comparativas, puede ver que la combinación de discos y máquinas virtuales es capaz de lograr 22 800 IOPS:

[!INCLUDE [VM and Disk Performance](../../../includes/virtual-machine-disk-performance-3.md)]
