---
title: Tamaños de máquina virtual Linux en Azure
description: Enumera los tamaños diferentes disponibles para las máquinas virtuales Linux en Azure.
author: ju-shim
ms.service: virtual-machines
ms.subservice: sizes
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 02/03/2020
ms.author: jushiman
ms.openlocfilehash: 2620795d8c4fc8adf11fc0795048ff9999d6ef83
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/02/2020
ms.locfileid: "84783362"
---
# <a name="sizes-for-linux-virtual-machines-in-azure"></a>Tamaños de las máquinas virtuales Linux en Azure

En este artículo se describen los tamaños y las opciones disponibles para las máquinas virtuales de Azure que puede usar para ejecutar las aplicaciones y cargas de trabajo de Linux. También ofrece consideraciones de implementación que hay que tener en cuenta siempre que planee usar estos recursos. Este artículo también está disponible para [máquinas virtuales Windows](../windows/sizes.md?toc=/azure/virtual-machines/windows/toc.json&bc=/azure/virtual-machines/windows/breadcrumb/toc.json).

| Tipo | Tamaños | Descripción |
|------|-------|-------------|
| [Uso general](../sizes-general.md)   | B, Dsv3, Dv3, Dasv4, Dav4, DSv2, Dv2, Av2, DC, DCv2, Dv4, Dsv4, Ddv4, Ddsv4  | Uso equilibrado de la CPU en proporción de memoria. Ideal para desarrollo y pruebas, bases de datos pequeñas o medianas, y servidores web de tráfico bajo o medio. |
| [Proceso optimizado](../sizes-compute.md) | Fsv2 | Uso elevado de la CPU en proporción de memoria. Bueno para servidores web de tráfico medio, aplicaciones de red, procesos por lotes y servidores de aplicaciones. |
| [Memoria optimizada](../sizes-memory.md) | Esv3, Ev3, Easv4, Eav4, Ev4, Esv4, Edv4, Edsv4, Mv2, M, DSv2, Dv2 | Memoria alta en proporción de CPU. Excelente para servidores de bases de datos relacionales, memorias caché de capacidad media o grande y análisis en memoria.                 |
| [Almacenamiento optimizado](../sizes-storage.md) | Lsv2 | Alto rendimiento de disco y de E/S ideales para macrodatos, bases de datos SQL y NoSQL, almacenamiento de datos y bases de datos transaccionales grandes.  |
| [GPU](../sizes-gpu.md) | NC, NCv2, NCv3, ND, NDv2 (versión preliminar), NV, NVv3, NVv4 | Máquinas virtuales especializadas específicas para la representación de gráficos pesados y la edición de vídeo, así como para el entrenamiento e inferencia de modelos (ND) con aprendizaje profundo. Están disponibles con uno o varios GPU. |
| [Proceso de alto rendimiento](../sizes-hpc.md) | HB, HBv2, HC, H | Nuestras máquinas virtuales de CPU más rápidas y eficaces con interfaces de red de alto rendimiento opcionales (RDMA). |

- Para obtener información sobre los precios de los diferentes tamaños, consulte [Precios de máquinas virtuales](https://azure.microsoft.com/pricing/details/virtual-machines/#Linux). 
- Para ver la disponibilidad de los tamaños de máquina virtual en las regiones de Azure, consulte [Productos disponibles por región](https://azure.microsoft.com/regions/services/).
- Para ver los límites generales de las máquinas virtuales de Azure, consulte [Límites, cuotas y restricciones de suscripción y servicios de Microsoft Azure](../../azure-subscription-service-limits.md).
- Obtenga más información sobre cómo las [unidades de proceso de Azure (ACU)](../acu.md) pueden ayudarlo a comparar el rendimiento en los distintos SKU de Azure.

## <a name="rest-api"></a>API DE REST

Para obtener información sobre el uso de la API de REST para consultar los tamaños de máquina virtual, consulte lo siguiente:

- [Lista de tamaños de máquina virtual disponibles para cambio de tamaño](https://docs.microsoft.com/rest/api/compute/virtualmachines/listavailablesizes)
- [Lista de tamaños de máquina virtual disponibles para una suscripción](https://docs.microsoft.com/rest/api/compute/resourceskus/list)
- [Lista de tamaños de máquina virtual disponibles en un conjunto de disponibilidad](https://docs.microsoft.com/rest/api/compute/availabilitysets/listavailablesizes)

## <a name="acu"></a>ACU

Obtenga más información sobre cómo las [unidades de proceso de Azure (ACU)](../acu.md) pueden ayudarlo a comparar el rendimiento en los distintos SKU de Azure.

## <a name="benchmark-scores"></a>Puntuaciones de pruebas comparativas

Más información sobre el rendimiento de los procesos para las máquinas virtuales Linux con las [puntuaciones de pruebas comparativas de CoreMark](compute-benchmark-scores.md).

## <a name="next-steps"></a>Pasos siguientes

Obtenga más información sobre los diferentes tamaños de máquina virtual que están disponibles:

- [Uso general](../sizes-general.md)
- [Proceso optimizado](../sizes-compute.md)
- [Memoria optimizada](../sizes-memory.md)
- [Almacenamiento optimizado](../sizes-storage.md)
- [GPU](../sizes-gpu.md)
- [Proceso de alto rendimiento](../sizes-hpc.md)
- Consulte la página [Generación anterior](../sizes-previous-gen.md) para las series A estándar, Dv1 (D1-4 y D11-14 v1) y las series A8-A11
