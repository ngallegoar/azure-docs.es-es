---
title: Recuperación de contenedores de cuenta de almacenamiento
description: Recuperación de contenedores de cuenta de almacenamiento
services: storage
author: v-miegge
ms.topic: troubleshooting
ms.author: kartup
ms.date: 10/30/2019
ms.service: storage
manager: dcscontentpm
ms.openlocfilehash: 8b9b69fb32edab54fb2df558ea292df264e00cbb
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "77562282"
---
# <a name="storage-account-container-recovery"></a>Recuperación de contenedores de cuenta de almacenamiento

Azure Storage ofrece resistencia de datos gracias a las réplicas automatizadas. Sin embargo, esto no evita que el código de aplicación o los usuarios dañen los datos, de forma accidental o malintencionada. Mantener la fidelidad de los datos en caso de error de la aplicación o del usuario requiere técnicas más avanzadas, como copiar los datos en una ubicación de almacenamiento secundaria con un registro de auditoría.

## <a name="checking-azure-storage-account-type"></a>Comprobación del tipo de cuenta de Azure Storage

1. Acceda a [Azure Portal](https://portal.azure.com/).

2. Busque su cuenta de almacenamiento.

3. En la sección **Información general**, busque **Replicación**.

   ![Imagen](media/storage-account-container-recovery/1.png)

4. Si el tipo de replicación es **GRS/RA-GRS**, la recuperación del contenedor de cuentas es posible sin ninguna garantía. Para todos los demás tipos de replicación, no es posible.

5. Recopile la información siguiente y abra una solicitud de soporte técnico con Soporte técnico de Microsoft.

   * Nombre de la cuenta de almacenamiento:
   * Nombre del contenedor:
   * Hora de eliminación:

   En la tabla siguiente se proporciona información general sobre el ámbito de la recuperación del contenedor de la cuenta de almacenamiento en función de la estrategia de replicación.

   |Tipo de contenido|LRS|ZRS|GRS|RA - GRS| 
   |---|---|---|---|---|
   |Contenedor de almacenamiento|No|No|Sí|Sí| 

   * Se puede intentar restaurar el contenedor de la cuenta de almacenamiento, pero sin ninguna garantía. 

## <a name="things-not-to-do-for-recover-to-be-successful"></a>Cosas que no hacer para que la recuperación sea correcta

* No vuelva a crear el contenedor (con el mismo nombre).  
* Si ya ha creado el contenedor, debe eliminarlo antes de abrir una solicitud de soporte técnico para la recuperación.

## <a name="steps-to-prevent-this-in-the-future"></a>Pasos para evitarlo en el futuro

1. Para evitar esto en el futuro, la característica más recomendada es [Eliminación temporal](https://docs.microsoft.com/azure/storage/blobs/storage-blob-soft-delete).

2. También se recomienda la característica [Instantánea](https://docs.microsoft.com/rest/api/storageservices/Creating-a-Snapshot-of-a-Blob).
 
## <a name="next-steps"></a>Pasos siguientes

A continuación se muestran dos códigos de ejemplo de la característica:

  * [Creación y administración de una instantánea de blob en .NET](https://docs.microsoft.com/azure/storage/storage-blob-snapshots)
  * [Uso de instantáneas de blob con PowerShell](https://blogs.msdn.microsoft.com/cie/2016/05/17/using-blob-snapshots-with-powershell/)
  

