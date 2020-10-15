---
author: roygara
ms.service: virtual-machines
ms.topic: include
ms.date: 11/09/2018
ms.author: rogarana
ms.openlocfilehash: ef18feb10dabc6a77e6512c6a32ad44b32c6e832
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "80334973"
---
**Discos de máquina virtual no administrados premium: límites por cuenta**

| Resource | Límite |
| --- | --- |
| Capacidad total de disco por cuenta |35 TB |
| Capacidad total de instantáneas por cuenta |10 TB |
| Ancho de banda máximo por cuenta (entrada + salida<sup>1</sup>) |<=50 Gbps |

<sup>1</sup>*Entrada* hace referencia a todos los datos de las solicitudes que se envían a una cuenta de almacenamiento. *Salida* hace referencia a todos los datos de las respuestas que se reciben desde una cuenta de almacenamiento.

**Discos de máquina virtual no administrados premium: límites por disco**

| Tipo de disco de Premium Storage | P10 | P20 | P30 | P40 | P50 |
| --- | --- | --- | --- | --- | --- |
| Tamaño del disco |128 GB |512 GB |1024 GiB (1 TB) |2048 GiB (2 TB)|4095 GiB (4 TB)|
| Número máximo de IOPS por disco |500 |2,300 |5\.000 |7500 |7500 |
| Rendimiento máximo por disco |100 MB/s | 150 MB/s |200 MB/s |250 MB/s |250 MB/s |
| Número máximo de discos por cuenta de almacenamiento |280 |70 |35 | 17 | 8 |

**Discos de máquina virtual no administrados premium: límites por máquina virtual**

| Resource | Límite |
| --- | --- |
| Número máximo de IOPS por máquina virtual |80 000 IOPS con máquina virtual GS5 |
| Rendimiento máximo por máquina virtual |2000 MB/s con máquina virtual GS5 |

