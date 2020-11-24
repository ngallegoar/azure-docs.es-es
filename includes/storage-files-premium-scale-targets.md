---
author: roygara
ms.service: storage
ms.topic: include
ms.date: 08/10/2020
ms.author: rogarana
ms.openlocfilehash: 8dcb58499113b0b7ae0814419f0a76965a0ed945
ms.sourcegitcommit: 8e7316bd4c4991de62ea485adca30065e5b86c67
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/17/2020
ms.locfileid: "94680901"
---
#### <a name="additional-premium-file-share-level-limits"></a>Límites del nivel de recurso compartido de archivos prémium adicionales

|Área  |Destino  |
|---------|---------|
|Aumento o disminución del tamaño mínimo    |1 GiB      |
|IOPS base    |1 IOPS por GiB hasta 100 000|
|Ampliación de IOPS    |3 x IOPS por GiB hasta 100 000|
|Velocidad de salida         |60 MiB/s + 0,06 * GiB aprovisionado        |
|Velocidad de entrada| 40 MiB/s + 0,04 * GiB aprovisionado |

#### <a name="file-level-limits"></a>Límites de nivel de archivo

|Área  |Archivo estándar  |Archivo prémium  |
|---------|---------|---------|
|Size     |1 TiB         |4 TiB         |
|IOPS máximas por archivo      |1,000         |Hasta 8000*         |
|Identificadores simultáneos     |2\.000         |2\.000         |
|Salida     |Consulte los valores de rendimiento de archivo estándar         |300 MiB/s (hasta 1 GiB/s con la versión SMB multicanal)**         |
|Entrada     |Consulte los valores de rendimiento de archivo estándar         |200 MiB/s (hasta 1 GiB/s con la versión preliminar de SMB multicanal)**        |
|Throughput     |Hasta 60 MiB/s         |Consulte los valores de entrada y salida de archivos prémium         |

\* <sup> Se aplica a entradas y salidas de lectura y escritura (normalmente tamaños de E/S más pequeños <= 64 K). Las operaciones de metadatos, que no sean lecturas y escrituras, pueden ser más lentas. </sup>

\*\* <sup> En función de los límites de red de la máquina, el ancho de banda disponible, los tamaños de E/S, la profundidad de la cola y otros factores. Para más información, consulte [Rendimiento de SMB multicanal](../articles/storage/files/storage-files-smb-multichannel-performance.md). </sup>
