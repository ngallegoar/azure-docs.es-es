---
title: 'Serie HC: Azure Virtual Machines'
description: Especificaciones de las máquinas virtuales de la serie HC.
author: ju-shim
ms.service: virtual-machines
ms.subservice: sizes
ms.topic: conceptual
ms.date: 10/09/2020
ms.author: amverma
ms.reviewer: jushiman
ms.openlocfilehash: 254e8591892fe08e8f05c49ed47a4cb00da16e78
ms.sourcegitcommit: 50802bffd56155f3b01bfb4ed009b70045131750
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "91930193"
---
# <a name="hc-series"></a>Serie HC

Las máquinas virtuales de la serie HC están optimizadas para aplicaciones basadas en cálculos intensivos, como el análisis implícito de elementos finitos, la dinámica molecular y la química computacional. Las máquinas virtuales HC cuentan con 44 núcleos de procesador Intel Xeon Platinum 8168, 8 GB de RAM por núcleo de CPU y no tienen hyperthreading. La plataforma Intel Xeon Platinum admite el rico ecosistema de herramientas de software de Intel, como la biblioteca Intel Math Kernel Library y funcionalidades avanzadas de procesamiento vectorial, como AVX-512.

Las máquinas virtuales de la serie HC incluyen la característica MeliBox EDR InfiniBand a 100 GB/s. Estas máquinas virtuales están conectadas en un árbol FAT sin bloqueos para un rendimiento de RDMA optimizado y coherente. Estas máquinas virtuales admiten el enrutamiento adaptativo y el transporte conectado dinámico (DCT, además de los transportes estándar RC y UD). Estas características mejoran el rendimiento, la escalabilidad y la coherencia de las aplicaciones, y se recomienda su uso.

[ACU](acu.md): 297-315<br>
[Premium Storage](premium-storage-performance.md): Compatible<br>
[Almacenamiento en caché de Premium Storage](premium-storage-performance.md): Compatible<br>
[Migración en vivo](maintenance-and-updates.md): No compatible<br>
[Actualizaciones con conservación de memoria](maintenance-and-updates.md): No compatible<br>
[Compatibilidad con generación de VM](generation-2.md): Generación 1 y 2<br>
<br>

| Size | vCPU | Procesador | Memoria (GiB) | Ancho de banda de memoria, en GB/s | Frecuencia de CPU base (GHz) | Frecuencia de todos los núcleos (GHz, pico) | Frecuencia de cada núcleo (GHz, pico) | Rendimiento de RDMA (GB/s) | Compatibilidad con MPI | Almacenamiento temporal (GiB) | Discos de datos máx. | vNIC Ethernet máx. |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| Standard_HC44rs | 44 | Intel Xeon Platinum 8168 | 352 | 191 | 2.7 | 3.4 | 3.7 | 100 | All | 700 | 4 | 8 |

[!INCLUDE [virtual-machines-common-sizes-table-defs](../../includes/virtual-machines-common-sizes-table-defs.md)]

## <a name="other-sizes"></a>Otros tamaños

- [Uso general](sizes-general.md)
- [Memoria optimizada](sizes-memory.md)
- [Almacenamiento optimizado](sizes-storage.md)
- [GPU optimizada](sizes-gpu.md)
- [Proceso de alto rendimiento](sizes-hpc.md)
- [Generaciones anteriores](sizes-previous-gen.md)

## <a name="next-steps"></a>Pasos siguientes

- Obtenga más información sobre la [configuración de máquinas virtuales](./workloads/hpc/configure.md), la [habilitación de InfiniBand](./workloads/hpc/enable-infiniband.md), la [configuración de MPI](./workloads/hpc/setup-mpi.md) y la optimización de las aplicaciones HPC para Azure en el archivo sobre [cargas de trabajo de HPC](./workloads/hpc/overview.md).
- En los [blogs de la comunidad de Azure Compute Tech](https://techcommunity.microsoft.com/t5/azure-compute/bg-p/AzureCompute), encontrará los anuncios más recientes y algunos ejemplos y resultados de HPC.
- Si desea una visión de alto nivel de la arquitectura de la ejecución de cargas de trabajo de HPC, consulte [Informática de alto rendimiento (HPC) en Azure](/azure/architecture/topics/high-performance-computing/).
- Obtenga más información sobre cómo las [unidades de proceso de Azure (ACU)](acu.md) pueden ayudarlo a comparar el rendimiento en los distintos SKU de Azure.
