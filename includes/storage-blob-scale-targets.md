---
author: tamram
ms.service: storage
ms.topic: include
ms.date: 11/08/2019
ms.author: tamram
ms.openlocfilehash: 2ed88d8abb7cbe96093b68d89030e6e464a35541
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "75392442"
---
| Resource | Destino        |
|----------|---------------|
| Tamaño máximo de contenedor de un blob | Igual que la capacidad máxima de la cuenta de almacenamiento |
| Número máximo de bloques en un blob en bloques o blob en anexos | 50 000 bloques |
| Tamaño máximo de un bloque en un blob en bloques | 100 MiB |
| Tamaño máximo de un blob en bloques | 50 000 x 100 MiB (4,75 TiB, aproximadamente) |
| Tamaño máximo de un bloque en un blob en anexos | 4 MiB |
| Tamaño máximo de un blob en anexos | 50 000 x 4 MiB (195 GiB, aproximadamente) |
| Tamaño máximo de un blob en páginas | 8 TiB |
| Número máximo de directivas de acceso almacenadas por contenedor de blobs | 5 |
|Velocidad de solicitudes de destino para un solo blob | Hasta 500 solicitudes por segundo |
|Rendimiento de destino para un blob en una sola página | Hasta 60 MiB por segundo |
|Rendimiento de destino para un blob en un solo bloque |Hasta los límites de entrada/salida de cuenta de almacenamiento<sup>1</sup> |

<sup>1</sup> El rendimiento de un único blob depende de varios factores, incluidos, sin limitación: simultaneidad, tamaño de la solicitud, nivel de rendimiento, velocidad de origen de las cargas y destino de las descargas. Para aprovechar las mejoras de rendimiento de [blobs en bloques de alto rendimiento](https://azure.microsoft.com/blog/high-throughput-with-azure-blob-storage/), cargue blobs o bloques más grandes. En concreto, llame a la operación [Put Blob](/rest/api/storageservices/put-blob) o [Put Block](/rest/api/storageservices/put-block) con un tamaño de blob o de bloque superior a 4 MiB para las cuentas de almacenamiento estándar. Para los blobs en bloques prémium o las cuentas de almacenamiento de Data Lake Storage Gen2, use un tamaño de bloque o de blob superior a 256 KiB.
