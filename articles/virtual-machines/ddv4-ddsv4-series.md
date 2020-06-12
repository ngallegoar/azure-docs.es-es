---
title: 'Series Ddv4 y Ddsv4: Máquinas virtuales de Azure'
description: Especificaciones para las máquinas virtuales de la serie Dv4, Ddv4, Dsv4 y Ddsv4
author: brbell
ms.author: brbell
ms.reviewer: cynthn
ms.custom: mimckitt
ms.service: virtual-machines
ms.topic: conceptual
ms.date: 06/01/2020
ms.openlocfilehash: 42b42a7477326196546ad445367691192f00569a
ms.sourcegitcommit: 309cf6876d906425a0d6f72deceb9ecd231d387c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/01/2020
ms.locfileid: "84263252"
---
# <a name="ddv4-and-ddsv4-series"></a>Series Ddv4 y Ddsv4

Las máquinas virtuales de las series Ddv4 y Ddsv4 se ejecutan en los procesadores Intel &reg;Xeon&reg; Platinum 8272CL (Cascade Lake) en una configuración con tecnología Hyper-Threading, lo que proporciona una mejor propuesta de valor para la mayoría de las cargas de trabajo de uso general. Presenta una velocidad de reloj turbo sostenida de todos los núcleos de 3,4 GHz, las tecnologías [Intel&reg; Turbo Boost 2.0](https://www.intel.com/content/www/us/en/architecture-and-technology/turbo-boost/turbo-boost-technology.html), [Intel&reg; Hyper-Threading](https://www.intel.com/content/www/us/en/architecture-and-technology/hyper-threading/hyper-threading-technology.html) y las extensiones de vector avanzadas 512 de [Intel&reg; (Intel &reg;AVX-512)](https://www.intel.com/content/www/us/en/architecture-and-technology/avx-512-overview.html). Estos nuevos tamaños de máquina virtual tendrán un almacenamiento local del 50 % más grande, así como una mayor E/S por segundo del disco local para lectura y escritura, en comparación con los tamaños [Dv3/Dsv3](https://docs.microsoft.com/azure/virtual-machines/dv3-dsv3-series) con [máquinas virtuales de segunda generación](https://docs.microsoft.com/azure/virtual-machines/linux/generation-2).

Los casos de uso de la serie D son las aplicaciones empresariales, las bases de datos relacionales, el almacenamiento en caché en memoria y el análisis.

> [!IMPORTANT]
> Si va a implementar una nueva máquina virtual mediante la serie Ddv4 o Ddsv4 y piensa usar una imagen de Linux, debe usar RHEL 8.x, CentOS 8.x u Oracle 7.x o posterior. Si elige RHEL 7.x, CentOS 7.x u Oracle 6.x, se producirá un error de pánico de kernel. Microsoft está implementando activamente una corrección. Solo se ven afectados RHEL, CentOS y Oracle. 

## <a name="ddv4-series"></a>Serie Ddv4

Los tamaños de la serie Ddv4 se ejecutan en Intel&reg; Xeon&reg; Platinum 8272CL (Cascade Lake). La serie Ddv4 ofrece una combinación de vCPU, memoria y disco temporal para la mayoría de las cargas de trabajo de producción.

Los nuevos tamaños de máquinas virtuales Ddv4 incluyen almacenamiento SSD local rápido y más grande (hasta 2400 GiB) y están diseñados para aplicaciones que se benefician del almacenamiento local de baja latencia y alta velocidad, como aplicaciones que requieren lecturas y escrituras rápidas en el almacenamiento temporal o que necesitan almacenamiento temporal para cachés o archivos temporales. Puede conectar discos SSD estándar y almacenamiento de disco duro estándar a las máquinas virtuales Ddv4. El almacenamiento en disco de datos remotos se factura de forma independiente a las máquinas virtuales.

ACU: 195-210

Premium Storage:  No compatible

Almacenamiento en caché de Premium Storage:  No compatible

Migración en vivo: Compatible

Actualizaciones con conservación de memoria: Compatible

| Size | vCPU | Memoria: GiB | GiB de almacenamiento temporal (SSD) | Discos de datos máx. | Rendimiento máximo de almacenamiento temporal y en caché: IOPS/Mbps | Nº máx. de NIC/ancho de banda de red esperado (Mbps) |
|---|---|---|---|---|---|---|
| Standard_D2d_v4 | 2 | 8 | 75 | 4 | 19000/120 | 2/1000 |
| Standard_D4d_v4 | 4 | 16 | 150 | 8 | 38500/242 | 2/2000 |
| Standard_D8d_v4 | 8 | 32 | 300 | 16 | 77000/485 | 4/4000 |
| Standard_D16d_v4 | 16 | 64 | 600 | 32 | 154000/968 | 8/8000 |
| Standard_D32d_v4 | 32 | 128 | 1200 | 32 | 308000/1936 | 8/16000 |
| Standard_D48d_v4 | 48 | 192 | 1800 | 32 | 462000/2904 | 8/24000 |
| Standard_D64d_v4 | 64 | 256 | 2400 | 32 | 615000/3872 | 8/30000 |

## <a name="ddsv4-series"></a>Serie Ddsv4

La serie Ddsv4 se ejecuta en Intel&reg; Xeon&reg; Platinum 8272CL (Cascade Lake). La serie Ddsv4 ofrece una combinación de vCPU, memoria y disco temporal para la mayoría de las cargas de trabajo de producción.

Los nuevos tamaños de máquinas virtuales Ddsv4 incluyen almacenamiento SSD local rápido y más grande (hasta 2400 GiB) y están diseñados para aplicaciones que se benefician del almacenamiento local de baja latencia y alta velocidad, como aplicaciones que requieren lecturas y escrituras rápidas en el almacenamiento temporal o que necesitan almacenamiento temporal para cachés o archivos temporales. 

 > [!NOTE]
 >El precio y los medidores de facturación para los tamaños Ddsv4 son los mismos que para la serie Ddv4.

ACU: 195-210

Premium Storage:  Compatible

Almacenamiento en caché de Premium Storage:  Compatible

Migración en vivo: Compatible

Actualizaciones con conservación de memoria: Compatible

| Size | vCPU | Memoria: GiB | GiB de almacenamiento temporal (SSD) | Discos de datos máx. | Rendimiento máximo de almacenamiento temporal y en caché: IOPS/Mbps | Rendimiento máximo del disco sin almacenamiento en la caché: IOPS/Mbps | Nº máx. de NIC/ancho de banda de red esperado (Mbps) |
|---|---|---|---|---|---|---|---|
| Standard_D2ds_v4 | 2 | 8 | 75 | 4 | 19000/120(50) | 3000/48 | 2/1000 |
| Standard_D4ds_v4 | 4 | 16 | 150 | 8 | 38500/242(100) | 6400/96 | 2/2000 |
| Standard_D8ds_v4 | 8 | 32 | 300 | 16 | 77000/485(200) | 12800/192 | 4/4000 |
| Standard_D16ds_v4 | 16 | 64 | 600 | 32 | 154000/968(400) | 25600/384 | 8/8000 |
| Standard_D32ds_v4 | 32 | 128 | 1200 | 32 | 308000/1936(800) | 51200/768 | 8/16000 |
| Standard_D48ds_v4 | 48 | 192 | 1800 | 32 | 462000/2904(1200) | 76800/1152 | 8/24000 |
| Standard_D64ds_v4 | 64 | 256 | 2400 | 32 | 615000/3872(1600) | 80000/1200 | 8/30000 |

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
