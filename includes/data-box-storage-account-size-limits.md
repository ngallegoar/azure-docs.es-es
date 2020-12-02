---
author: alkohli
ms.service: databox
ms.subservice: heavy
ms.topic: include
ms.date: 06/24/2020
ms.author: alkohli
ms.openlocfilehash: 2868e5a53686cfa94dc206c1aab65fe866d19b6d
ms.sourcegitcommit: a43a59e44c14d349d597c3d2fd2bc779989c71d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/25/2020
ms.locfileid: "96026957"
---
Estos son los límites del tamaño de los datos que se copian en la cuenta de almacenamiento. Asegúrese de que los datos que carga se ajustan a estos límites. Para obtener la información más actualizada sobre estos límites, consulte [Objetivos de rendimiento y escalabilidad de Blob Storage](../articles/storage/blobs/scalability-targets.md) y [Objetivos de rendimiento y escalabilidad de Azure Files](../articles/storage/files/storage-files-scale-targets.md).

| Tamaño de los datos que se copian en la cuenta de almacenamiento de Azure                      | Límite predeterminado          |
|---------------------------------------------------------------------|------------------------|
| Blob en bloques y blob en páginas                                            | El límite máximo es el mismo que el [límite de almacenamiento definido para la suscripción de Azure](../articles/azure-resource-manager/management/azure-subscription-service-limits.md#storage-limits) e incluye datos de todos los orígenes, incluido Data Box. |
| Azure Files                                                          | Data Box admite recursos compartidos de archivos de gran tamaño (100 TiB) si se habilita antes de la creación del pedido de Data Box. <br> Si no se habilita antes de la creación del pedido, se admitirá un tamaño máximo de recurso compartido de archivos de 5 TiB. <br> Todavía no se admiten recursos compartidos de archivos Premium.<br> Todas las carpetas de *StorageAccount_AzureFiles* deben seguir este límite. <br> Para más información, consulte [Habilitación y creación de recursos compartidos de archivos grandes](../articles/storage/files/storage-files-how-to-create-large-file-share.md).      |