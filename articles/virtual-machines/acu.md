---
title: Información general sobre unidades de Azure Compute | Microsoft Docs
description: Información general sobre el concepto de unidades de proceso de Azure. La ACU proporciona un modo de comparar el rendimiento de la CPU en todas las SKU de Azure.
services: virtual-machines
documentationcenter: ''
author: mimckitt
manager: vashan
editor: ''
tags: azure-resource-manager,azure-service-management
ms.service: virtual-machines
ms.devlang: na
ms.topic: article
ms.workload: infrastructure-services
ms.date: 02/03/2020
ms.author: mimckitt
ms.openlocfilehash: e344d09497a30dec546dfaedd3d78f30c7d214d2
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "79535704"
---
# <a name="azure-compute-unit-acu"></a>Unidad de proceso de Azure (ACU)

El concepto de unidad de proceso de Azure (ACU) ofrece una forma de comparar el rendimiento de los procesos (CPU) en todas las SKU de Azure. Esto le ayudará a identificar fácilmente el SKU que tiene más probabilidades de satisfacer sus necesidades de rendimiento. Actualmente, una ACU está estandarizada en una máquina virtual pequeña (Standard_A1) como 100 y todas las demás SKU representan, aproximadamente, qué tanto más rápido esa SKU puede ejecutar una prueba comparativa estándar.

> [!IMPORTANT]
> La ACU es solo una referencia. Los resultados de la carga de trabajo pueden variar.
<br>

| Familia de SKU | ACU \ vCPU | vCPU: núcleo |
| --- | --- |---|
| [A0](sizes-previous-gen.md) |50 | 1:1 |
| [A1 - A4](sizes-previous-gen.md) |100 | 1:1 |
| [A5 - A7](sizes-previous-gen.md) |100 | 1:1 |
| [A1_v2 - A8_v2](sizes-general.md) |100 | 1:1 |
| [A2m_v2 - A8m_v2](sizes-general.md) |100 | 1:1 |
| [A8 - A11](sizes-previous-gen.md) |225* | 1:1 |
| [D1 - D14](sizes-previous-gen.md) |160 - 250 | 1:1 |
| [D1_v2 - D15_v2](dv2-dsv2-series.md) |210 - 250* | 1:1 |
| [DS1 - DS14](sizes-previous-gen.md) |160 - 250 | 1:1 |
| [DS1_v2 - DS15_v2](dv2-dsv2-series.md) |210 - 250* | 1:1 |
| [D_v3](dv3-dsv3-series.md) |160 - 190* | 2:1\*\*\* |
| [Ds_v3](dv3-dsv3-series.md) |160 - 190* | 2:1\*\*\* |
| [E_v3](ev3-esv3-series.md) |160 - 190* | 2:1\*\*\*|
| [Es_v3](ev3-esv3-series.md) |160 - 190* | 2:1\*\*\* |
| [F2s_v2 - F72s_v2](fsv2-series.md) |195 - 210* | 2:1\*\*\* |
| [F1 - F16](sizes-previous-gen.md) |210 - 250* | 1:1 |
| [F1s - F16s](sizes-previous-gen.md) |210 - 250* | 1:1 |
| [G1 - G5](sizes-previous-gen.md) |180 - 240* | 1:1 |
| [GS1 - GS5](sizes-previous-gen.md) |180 - 240* | 1:1 |
| [H](h-series.md) |290 - 300* | 1:1 |
| [HB](hb-series.md) |199 - 216** | 1:1 |
| [HC](hc-series.md) |297 - 315* | 1:1 |
| [L4s - L32s](sizes-previous-gen.md) |180 - 240* | 1:1 |
| [L8s_v2 - L80s_v2](lsv2-series.md) |150 - 175** | 2:1 |
| [M](m-series.md) | 160 - 180 | 2:1\*\*\* |

*Las ACU usan la tecnología Intel® Turbo para incrementar la frecuencia de CPU y brindar una mejora del rendimiento.  El volumen de mejora del rendimiento puede variar según el tamaño de la máquina virtual, la carga de trabajo y las otras cargas de trabajo que se ejecutan en el mismo host.
**Las ACU usan la tecnología AMD® Boost para incrementar la frecuencia de CPU y brindar una mejora del rendimiento.  El volumen de mejora del rendimiento puede variar según el tamaño de la máquina virtual, la carga de trabajo y las otras cargas de trabajo que se ejecutan en el mismo host.
***Con Hyper-Threading y capaz de ejecutar la virtualización anidada

Estos son algunos vínculos con información sobre los distintos tamaños:

- [Uso general](sizes-general.md)
- [Memoria optimizada](sizes-memory.md)
- [Proceso optimizado](sizes-compute.md)
- [GPU optimizada](sizes-gpu.md)
- [Proceso de alto rendimiento](sizes-hpc.md)
- [Almacenamiento optimizado](sizes-storage.md)
