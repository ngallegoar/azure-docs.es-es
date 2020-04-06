---
author: alkohli
ms.service: databox
ms.subservice: pod
ms.topic: include
ms.date: 05/22/2019
ms.author: alkohli
ms.openlocfilehash: bc156b8c18f46cccf6fc775b82f76383b8c43861
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "66242118"
---
Aquí se proporciona una lista de los tipos de cuentas de almacenamiento compatibles y de almacenamiento para el dispositivo Data Box. Para una lista completa de todos los tipos distintos de cuentas de almacenamiento y todas sus funcionalidades, consulte [Tipos de cuentas de almacenamiento](/azure/storage/common/storage-account-overview#types-of-storage-accounts).

| **Tipos de cuenta de almacenamiento/almacenamiento compatible** | **Blob en bloques** |**Blob en páginas*** |**Azure Files** |**Notas**|
| --- | --- | -- | -- | -- |
| Estándar clásico | Y | Y | Y |
| Estándar de uso general v1  | Y | Y | Y | Se admiten frecuentes y esporádicos.|
| Premium de uso general v1  |  | Y| | |
| Estándar de uso general v2  | Y | Y | Y | Se admiten frecuentes y esporádicos.|
| Premium de uso general v2  |  |Y | | |
| Estándar de Blob Storage |Y | | |Se admiten frecuentes y esporádicos. |

\* *: los datos cargados en blobs en páginas deben tener 512 bytes alineados como discos duros virtuales.*

>[!NOTE]
> No se admiten las cuentas de Azure Data Lake Storage Gen 2.
