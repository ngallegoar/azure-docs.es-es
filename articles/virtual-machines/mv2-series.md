---
title: 'Serie Mv2: Azure Virtual Machines'
description: Especificaciones de las máquinas virtuales de la serie Mv2.
services: virtual-machines
author: ayshakeen
ms.service: virtual-machines
ms.topic: article
ms.date: 02/03/2020
ms.author: lahugh
ms.openlocfilehash: 6fb0f93d3ac124b21bbc52ddc57bc720de6406e6
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "78163903"
---
# <a name="mv2-series"></a>Serie Mv2

La serie Mv2 ofrece una plataforma de alto rendimiento y baja latencia que se ejecuta en un procesador Intel® Xeon® Platinum 8180M de 2,5 GHz (Skylake) con tecnología Hyper-Threading con una frecuencia básica de 2,5 GHz y una frecuencia turbo máxima de 3,8 GHz. Todos los tamaños de máquina virtual de la serie Mv2 pueden usar discos persistentes estándar y premium. Las instancias de la serie Mv2 son tamaños de máquinas virtuales optimizados para memoria que proporcionan un rendimiento de proceso sin precedentes para admitir grandes bases de datos y cargas de trabajo en memoria, con una relación elevada de memoria y CPU que es perfecta para servidores de bases de datos relacionales, grandes almacenamientos en caché y análisis en memoria.

Las máquinas virtuales de la serie Mv2 cuentan con la tecnología Hyper-Threading de Intel®.

Premium Storage: Compatible

Almacenamiento en caché de Premium Storage: Compatible

Migración en vivo: No compatible

Actualizaciones con conservación de memoria: No compatible

Acelerador de escritura: [Compatible](https://docs.microsoft.com/azure/virtual-machines/windows/how-to-enable-write-accelerator)

|Size | vCPU | Memoria: GiB | GiB de almacenamiento temporal (SSD) | Discos de datos máx. | Rendimiento máximo de almacenamiento temporal y en caché: IOPS / MBps (tamaño de caché en GiB) | Rendimiento máximo del disco sin almacenamiento en la caché: IOPS / MBps | Nº máx. de NIC/ancho de banda de red esperado (Mbps) |
|---|---|---|---|---|---|---|---|
| Standard_M208ms_v2<sup>1</sup> | 208 | 5700 | 4096 | 64 | 80000/800 (7040) | 40000/1000 | 8 / 16000 |
| Standard_M208s_v2<sup>1</sup> | 208 | 2850 | 4096 | 64 | 80000/800 (7040) | 40000/1000 | 8 / 16000 |
| Standard_M416ms_v2<sup>1, 2</sup> | 416 | 11 400 | 8192 | 64 | 250 000 / 1600 (14 080) | 80000 / 2000 | 8 / 32 000 |
| Standard_M416s_v2<sup>1, 2</sup> | 416 | 5700 | 8192 | 64 | 250 000 / 1600 (14 080) | 80000 / 2000 | 8 / 32 000 |

<sup>1</sup> Las máquinas virtuales de la serie Mv2 son solo de la generación 2. Si usa Linux, consulte [Compatibilidad para máquinas virtuales de generación 2 en Azure](./linux/generation-2.md) para obtener instrucciones sobre cómo buscar y seleccionar una imagen.

<sup>2</sup> Para los tamaños M416ms_v2 y M416s_v2, tenga en cuenta que inicialmente solo se admite la siguiente imagen: "GEN2: SUSE Linux Enterprise Server (SLES) 12 SP4 para SAP Applications".

[!INCLUDE [virtual-machines-common-sizes-table-defs](../../includes/virtual-machines-common-sizes-table-defs.md)]

## <a name="other-sizes"></a>Otros tamaños

- [Uso general](sizes-general.md)
- [Memoria optimizada](sizes-memory.md)
- [Almacenamiento optimizado](sizes-storage.md)
- [GPU optimizada](sizes-gpu.md)
- [Proceso de alto rendimiento](sizes-hpc.md)
- [Generaciones anteriores](sizes-previous-gen.md)

## <a name="next-steps"></a>Pasos siguientes

Obtenga más información sobre cómo las [unidades de proceso de Azure (ACU)](acu.md) pueden ayudarlo a comparar el rendimiento en los distintos SKU de Azure.
