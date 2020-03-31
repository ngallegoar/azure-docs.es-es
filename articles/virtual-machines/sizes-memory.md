---
title: 'Tamaños de las máquinas virtuales de Azure: memoria | Microsoft Docs'
description: Enumera los tamaños diferentes optimizados para memoria disponibles para las máquinas virtuales en Azure. Se proporciona información sobre el número de unidades vCPU, discos de datos y NIC, así como sobre el rendimiento de almacenamiento y el ancho de banda de red para los tamaños de esta serie.
services: virtual-machines
documentationcenter: ''
author: jonbeck7
manager: gwallace
editor: ''
tags: azure-resource-manager,azure-service-management
keywords: Aislamiento de máquina virtual, máquina virtual aislada, aislamiento, aislada
ms.assetid: ''
ms.service: virtual-machines
ms.devlang: na
ms.topic: article
ms.workload: infrastructure-services
ms.date: 02/03/2020
ms.author: jonbeck
ms.openlocfilehash: 26a44cdf3e57508cebb070669a8c1f473a59df8a
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "77492545"
---
# <a name="memory-optimized-virtual-machine-sizes"></a>Tamaños de máquina virtual optimizada para memoria

Los tamaños de VM optimizadas para memoria ofrecen una relación alta de memoria a CPU que es excelente para servidores de bases de datos relacionales, memorias caché de medianas a grandes y análisis en memoria. En este artículo, se proporciona información acerca del número de vCPU, discos de datos y tarjetas de interfaz de red, así como del rendimiento del almacenamiento y del ancho de banda de red para cada tamaño de esta agrupación.

- Las [series de Dv2 y DSv2](dv2-dsv2-series-memory.md), una evolución de la serie D original, presentan una CPU más eficaz. La serie Dv2 es un 35 % aproximadamente más rápida que la serie D. Se ejecuta en procesadores Intel® Xeon® 8171M de 2,1 GHz (Skylake), Intel® Xeon® E5-2673 v4 de 2,3 GHz (Broadwell) o Intel® Xeon® E5-2673 v3 de 2,4 GHz (Haswell) y con la tecnología Intel Turbo Boost Technology 2.0. La serie Dv2 tiene las mismas configuraciones de disco y memoria que la serie D.

    Las series Dv2 y DSv2 son ideales para las aplicaciones que requieren vCPU más rápidas o mejor rendimiento del almacenamiento temporal, o tienen mayor demanda de memoria. Ofrecen una combinación eficaz para muchas aplicaciones de clase empresarial.

- Las [series Eav4 y Easv4](eav4-easv4-series.md) utilizan el procesador EPYC<sup>TM</sup> 7452 de 2,35 GHz de AMD en una configuración de varios subprocesos con una caché L3 de hasta 256 MB, lo que aumenta las opciones para ejecutar la mayoría de las cargas de trabajo optimizadas para memoria. Las series Eav4 y Easv4 tienen las mismas configuraciones de memoria y disco que las series Ev3 y Esv3.

- Las [series Ev3 y Esv3](ev3-esv3-series.md) tienen el procesador Intel® Xeon® 8171M de 2,1 GHz (Skylake) o el procesador Intel® Xeon® E5-2673 v4 de 2,3 GHz (Broadwell) en una configuración de Hyper-Threading. Gracias a esto, proporcionan una mejor propuesta de valor para la mayoría de las cargas de trabajo de uso general y la serie Ev3 se equipara con las máquinas virtuales de uso general de la mayoría de las demás nubes. Se ha ampliado la memoria (de 7 GiB/vCPU a 8 GiB/vCPU), y los límites de disco y red se han ajustado por núcleo para equipararse con el cambio a Hyper-Threading. La serie Ev3 es la continuación de los tamaños de máquina virtual de memoria alta de las familias D/Dv2.

- La [serie M](m-series.md) ofrece un elevado recuento de vCPU (hasta 128 vCPU) y una gran cantidad de memoria (hasta 3,8 TiB). También es ideal para bases de datos extremadamente grandes u otras aplicaciones que se benefician de un elevado número de vCPU y grandes cantidades de memoria.

- La [serie Mv2](mv2-series.md) ofrece el mayor número de vCPU (hasta 416 vCPU) y la memoria más grande (hasta 8,19 TiB) de todas las máquinas virtuales en la nube. Es ideal para bases de datos extremadamente grandes u otras aplicaciones que se benefician de un elevado número de vCPU y grandes cantidades de memoria.

Azure Compute ofrece tamaños de máquinas virtuales que están aislados para un tipo concreto de hardware y dedicados a un solo cliente. Estos tamaños de máquina virtual son más adecuados para cargas de trabajo que requieren un alto grado de aislamiento de otros clientes como, por ejemplo, las cargas de trabajo que incluyen elementos como el cumplimiento normativo y los requisitos legales. Los clientes también puede elegir subdividir aún más los recursos de estas máquinas virtuales aisladas mediante la [compatibilidad de Azure para máquinas virtuales anidadas](https://azure.microsoft.com/blog/nested-virtualization-in-azure/). Consulte las páginas de las familias de máquinas virtuales que aparecen a continuación para ver las opciones de las máquinas virtuales aisladas.

## <a name="other-sizes"></a>Otros tamaños

- [Uso general](sizes-general.md)
- [Proceso optimizado](sizes-compute.md)
- [Almacenamiento optimizado](sizes-storage.md)
- [GPU optimizada](sizes-gpu.md)
- [Proceso de alto rendimiento](sizes-hpc.md)
- [Generaciones anteriores](sizes-previous-gen.md)

## <a name="next-steps"></a>Pasos siguientes

Obtenga más información sobre cómo las [unidades de proceso de Azure (ACU)](acu.md) pueden ayudarlo a comparar el rendimiento en los distintos SKU de Azure.