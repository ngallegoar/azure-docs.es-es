---
title: 'Serie M: Azure Virtual Machines'
description: Especificaciones de las máquinas virtuales de la serie M.
author: ayshakeen
ms.service: virtual-machines
ms.subservice: sizes
ms.topic: article
ms.date: 03/31/2020
ms.author: jushiman
ms.openlocfilehash: 4050a74dfef1fb31939ba716cfbaf78577f1aaef
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/02/2020
ms.locfileid: "84675951"
---
# <a name="m-series"></a>Serie M

La serie M ofrece un elevado recuento de vCPU (hasta 128 vCPU) y una gran cantidad de memoria (hasta 3,8 TiB). También es muy conveniente para bases de datos extremadamente grandes u otras aplicaciones que se benefician de un elevado número de vCPU y grandes cantidades de memoria. Los tamaños de la serie M se admiten tanto en la CPU Intel&reg; Xeon&reg; E7-8890 v3 a 2,50 GHz como en Intel&reg; Xeon&reg; Platinum 8280M (Cascade Lake).

Las máquinas virtuales de la serie M cuentan con la tecnología Hyper-Threading de Intel&reg;.

ACU: 160-180

Premium Storage:  Compatible

Almacenamiento en caché de Premium Storage:  Compatible

Migración en vivo: No compatible

Actualizaciones con conservación de memoria: No compatible

Acelerador de escritura:  [Compatible](https://docs.microsoft.com/azure/virtual-machines/windows/how-to-enable-write-accelerator)

| Size | vCPU | Memoria: GiB | GiB de almacenamiento temporal (SSD) | Discos de datos máx. | Rendimiento máximo de almacenamiento temporal y en caché: IOPS/Mbps (tamaño de caché en GiB) | Rendimiento máximo del disco sin almacenamiento en la caché: IOPS/Mbps | Nº máx. de NIC/ancho de banda de red esperado (Mbps) |
|---|---|---|---|---|---|---|---|
| Standard_M8ms                    | 8   | 218,75 | 256   | 8  | 10000/100 (793)     | 5000/125   | 4/2000  |
| Standard_M16ms                   | 16  | 437,5  | 512   | 16 | 20000/200 (1587)    | 10000/250  | 8/4000  |
| Standard_M32ts                   | 32  | 192    | 1024  | 32 | 40000/400 (3174)    | 20000/500  | 8/8000  |
| Standard_M32ls                   | 32  | 256    | 1024  | 32 | 40000/400 (3174)    | 20000/500  | 8/8000  |
| Standard_M32ms                   | 32  | 875    | 1024  | 32 | 40000/400 (3174)    | 20000/500  | 8/8000  |
| Standard_M64s <sup>1</sup>       | 64  | 1024   | 2048  | 64 | 80000/800 (6348)    | 40000/1000 | 8/16000 |
| Standard_M64ls <sup>1</sup>      | 64  | 512    | 2048  | 64 | 80000/800 (6348)    | 40000/1000 | 8/16000 |
| Standard_M64ms <sup>1</sup>      | 64  | 1792   | 2048  | 64 | 80000/800 (6348)    | 40000/1000 | 8/16000 |
| Standard_M128s <sup>1</sup>    | 128 | 2048   | 4096  | 64 | 160000/1600 (12696) | 80000/2000 | 8/30000 |
| Standard_M128ms <sup>1,2</sup>   | 128 | 3892   | 4096  | 64 | 160000/1600 (12696) | 80000/2000 | 8/30000 |
| Standard_M64 <sup>1</sup>        | 64  | 1024   | 7168  | 64 | 80000/800 (1228)    | 40000/1000 | 8/16000 |
| Standard_M64m <sup>1</sup>       | 64  | 1792   | 7168  | 64 | 80000/800 (1228)    | 40000/1000 | 8/16000 |
| Standard_M128 <sup>1</sup>     | 128 | 2048   | 14336 | 64 | 250000/1600 (2456)  | 80000/2000 | 8/32000 |
| Standard_M128m <sup>1</sup>    | 128 | 3892   | 14336 | 64 | 250000/1600 (2456)  | 80000/2000 | 8/32000 |

<sup>1</sup> Si hay más de 64 vCPU, se necesitará uno de estos sistemas operativos invitados compatibles: Windows Server 2016, Ubuntu 16.04 LTS, SLES 12 SP2 y Red Hat Enterprise Linux, CentOS 7.3 u Oracle Linux 7.3 con LIS 4.2.1.

<sup>2</sup> La instancia está aislada en el hardware dedicado a un solo cliente.

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
