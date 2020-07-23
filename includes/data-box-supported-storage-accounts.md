---
author: alkohli
ms.service: databox
ms.subservice: pod
ms.topic: include
ms.date: 06/08/2020
ms.author: alkohli
ms.openlocfilehash: da36e2bbf358a1c61d2b9b3f7ede592ac88fd427
ms.sourcegitcommit: 3541c9cae8a12bdf457f1383e3557eb85a9b3187
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/09/2020
ms.locfileid: "86200291"
---
Aquí se proporciona una lista de los tipos de cuentas de almacenamiento compatibles y de almacenamiento para el dispositivo Data Box. Para una lista completa de todos los tipos distintos de cuentas de almacenamiento y todas sus funcionalidades, consulte [Tipos de cuentas de almacenamiento](/azure/storage/common/storage-account-overview#types-of-storage-accounts).

En el caso de los pedidos de importación, en la siguiente tabla se muestran las cuentas de almacenamiento admitidas.

| **Tipos de cuenta de almacenamiento/almacenamiento compatible** | **Blob en bloques** |**Blob en páginas*** |**Azure Files** |**Notas**|
| --- | --- | -- | -- | -- |
| Estándar clásico | Y | Y | Y |
| Estándar de uso general v1  | Y | Y | Y | Se admiten frecuentes y esporádicos.|
| Premium de uso general v1  |  | Y| | |
| Estándar de uso general v2  | Y | Y | Y | Se admiten frecuentes y esporádicos.|
| Premium de uso general v2  |  |Y | | |
| Estándar de Blob Storage |Y | | |Se admiten frecuentes y esporádicos. |

\* *: los datos cargados en blobs en páginas deben tener 512 bytes alineados como discos duros virtuales.*

En el caso de los pedidos de exportación, en la siguiente tabla se muestran las cuentas de almacenamiento admitidas.

| **Tipos de cuenta de almacenamiento/almacenamiento compatible** | **Blob en bloques** |**Blob en páginas*** |**Azure Files** |**Niveles de acceso admitidos**|
| --- | --- | -- | -- | -- |
| Estándar clásico | Y | Y | Y | |
| Estándar de uso general v1  | Y | Y | Y | Acceso frecuente y esporádico|
| Premium de uso general v1  |  | Y| | |
| Estándar de uso general v2  | Y | Y | Y | Acceso frecuente y esporádico|
| Premium de uso general v2  |  |Y | | |
| Estándar de Blob Storage |Y | | |Acceso frecuente y esporádico |
| Almacenamiento prémium de blobs en bloques |Y | | |Acceso frecuente y esporádico |
| Almacenamiento prémium de blobs en páginas | |Y | | |

> [!IMPORTANT]
> - En el caso de las cuentas de uso general, Data Box no admite los tipos de almacenamiento Queue, Table, Disk y Azure Data Lake Gen 2.
> - Data Box no admite blobs en anexos en las cuentas de Blob Storage y de almacenamiento de blobs en bloque.
> - Data Box no es compatible con las cuentas prémium de File Storage.
> - Los datos cargados en blobs en páginas deben tener 512 bytes alineados como discos duros virtuales.
> - Se pueden exportar hasta 80 TB.
> - El historial de archivos y las instantáneas de blobs no se exportan.


