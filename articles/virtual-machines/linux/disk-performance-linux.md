---
title: Rendimiento de la máquina virtual y del disco
description: Obtenga más información sobre cómo funcionan en conjunto las máquinas virtuales y sus discos conectados para el rendimiento
author: albecker1
ms.author: albecker
ms.date: 09/25/2020
ms.topic: conceptual
ms.service: virtual-machines
ms.subservice: disks
ms.openlocfilehash: c248549fae9e5a0b80f70223a2a1a1f9c7ac9c81
ms.sourcegitcommit: f5580dd1d1799de15646e195f0120b9f9255617b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/29/2020
ms.locfileid: "91541533"
---
# <a name="virtual-machine-and-disk-performance"></a>Rendimiento de la máquina virtual y del disco
[!INCLUDE [VM and Disk Performance](../../../includes/virtual-machine-disk-performance.md)]

## <a name="virtual-machine-uncached-vs-cached-limits"></a>Límites de almacenamiento en caché frente a sin caché para máquinas virtuales
Las máquinas virtuales que están habilitadas tanto para Premium Storage como para el almacenamiento en caché de Premium Storage tienen dos límites de ancho de banda de almacenamiento diferentes. Seguiremos examinando la máquina virtual Standard_D8s_v3 como ejemplo. Esta es la documentación de la [serie Dsv3](../dv3-dsv3-series.md) y en ella se describe lo siguiente sobre la máquina Standard_D8s_v3:

[!INCLUDE [VM and Disk Performance](../../../includes/virtual-machine-disk-performance-2.md)]
