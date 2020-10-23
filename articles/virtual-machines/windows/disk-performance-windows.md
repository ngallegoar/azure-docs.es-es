---
title: Rendimiento de la máquina virtual y del disco
description: Obtenga más información sobre cómo funcionan en conjunto las máquinas virtuales y sus discos conectados para el rendimiento
author: albecker1
ms.author: albecker
ms.date: 10/12/2020
ms.topic: conceptual
ms.service: virtual-machines
ms.subservice: disks
ms.openlocfilehash: 88cf9c28a9d325d617f4b049015f0cd238a2fb31
ms.sourcegitcommit: 2c586a0fbec6968205f3dc2af20e89e01f1b74b5
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/14/2020
ms.locfileid: "92016553"
---
# <a name="virtual-machine-and-disk-performance"></a>Rendimiento de la máquina virtual y del disco
[!INCLUDE [VM and Disk Performance](../../../includes/virtual-machine-disk-performance.md)]

## <a name="virtual-machine-uncached-vs-cached-limits"></a>Límites de almacenamiento en caché frente a sin caché para máquinas virtuales
 Las máquinas virtuales que están habilitadas para Premium Storage y para el almacenamiento en caché de Premium Storage tienen dos límites de ancho de banda de almacenamiento diferentes. Seguiremos examinando la máquina virtual Standard_D8s_v3 como ejemplo. Esta es la documentación de la [serie Dsv3](../dv3-dsv3-series.md) y en ella se describe lo siguiente sobre la máquina Standard_D8s_v3:

[!INCLUDE [VM and Disk Performance](../../../includes/virtual-machine-disk-performance-2.md)]

Vamos a ejecutar una prueba comparativa en esta combinación de discos y máquinas virtuales que creará actividad de E/S; podrá obtener información sobre cómo realizar pruebas comparativas de E/S de almacenamiento en Azure [aquí](disks-benchmarks.md). Desde la herramienta de pruebas comparativas, puede ver que la combinación de discos y máquinas virtuales es capaz de lograr 22 800 IOPS:

[!INCLUDE [VM and Disk Performance](../../../includes/virtual-machine-disk-performance-3.md)]