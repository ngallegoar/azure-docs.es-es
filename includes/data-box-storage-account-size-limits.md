---
author: alkohli
ms.service: databox
ms.subservice: heavy
ms.topic: include
ms.date: 06/18/2019
ms.author: alkohli
ms.openlocfilehash: 6a9c31eb46a457433834d5940b3f7e190ebe1476
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "75469664"
---
Estos son los límites del tamaño de los datos que se copian en la cuenta de almacenamiento. Asegúrese de que los datos que carga se ajustan a estos límites. Para obtener la información más actualizada sobre estos límites, consulte [Objetivos de rendimiento y escalabilidad de Blob Storage](../articles/storage/blobs/scalability-targets.md) y [Objetivos de rendimiento y escalabilidad de Azure Files](../articles/storage/files/storage-files-scale-targets.md).

| Tamaño de los datos que se copian en la cuenta de almacenamiento de Azure                      | Límite predeterminado          |
|---------------------------------------------------------------------|------------------------|
| Blob en bloques y blob en páginas                                            | 2 PB para Estados Unidos y Europa.<br>500 TB para todas las demás regiones, que incluye Reino Unido.  <br> Aquí se incluyen datos de todos los orígenes, incluido Data Box Disk.|
| Archivos de Azure                                                          | 5 TB por recurso compartido.<br> Todas las carpetas de *StorageAccount_AzureFiles* deben seguir este límite.       |
