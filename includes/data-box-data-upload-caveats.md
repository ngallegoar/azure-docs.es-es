---
author: alkohli
ms.service: databox
ms.subservice: heavy
ms.topic: include
ms.date: 11/05/2019
ms.author: alkohli
ms.openlocfilehash: 16647b6a13e64073ab570d36a8a380d0e36bd855
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "73800110"
---
- No copie los archivos directamente en ninguno de los recursos compartidos creados previamente. Debe crear una carpeta en el recurso compartido y, después, copiar los archivos en ella.
- Una carpeta en *StorageAccount_BlockBlob* y *StorageAccount_PageBlob* es un contenedor. Por ejemplo, los contenedores se crean como *StorageAccount_BlockBlob/container* y *StorageAccount_PageBlob/container*.
- Todas las carpetas que se han creado creó directamente en *StorageAccount_AzureFiles* se traducen en un recurso compartido de archivos de Azure.
- Si tiene un objeto existente de Azure (por ejemplo, un blob o un archivo) en la nube con el mismo nombre que el objeto que se está copiando, Data Box sobrescribirá el archivo en la nube.
- Todos los archivos escritos en los recursos compartidos *StorageAccount_BlockBlob* y *StorageAccount_BlockBlob* se cargan como blob en bloques y blob en páginas, respectivamente.
- Azure Blob Storage no admite directorios. Si crea una carpeta en la carpeta *StorageAccount_BlockBlob* y, después, se crean carpetas virtuales en el nombre del blob. Para Azure Files, se mantiene la estructura de directorios real.
- Todas las jerarquías de directorios vacías (sin archivos) creadas en las carpetas *StorageAccount_BlockBlob* y *StorageAccount_BlockBlob* no se cargan.
- Si se han producido errores al cargar datos en Azure, se crea un registro de errores en la cuenta de almacenamiento de destino. La ruta de acceso a este registro de errores está disponible cuando se completa la carga. Puede examinar el registro para realizar acciones correctivas. No elimine datos del origen sin comprobar los datos cargados.
- Los metadatos de archivo y los permisos NTFS no se conservan cuando se cargan los datos en Azure Files. Por ejemplo, el atributo *Última modificación* de los archivos no se conservará cuando se copien los datos.
