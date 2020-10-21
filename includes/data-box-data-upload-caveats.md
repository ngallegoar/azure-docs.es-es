---
author: alkohli
ms.service: databox
ms.subservice: heavy
ms.topic: include
ms.date: 09/30/2020
ms.author: alkohli
ms.openlocfilehash: 80411ac191d0385ddafd4a3cc490d322ce039689
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "91639244"
---
- No copie los archivos directamente en ninguno de los recursos compartidos creados previamente. Debe crear una carpeta en el recurso compartido y, después, copiar los archivos en ella.
- Una carpeta en *StorageAccount_BlockBlob* y *StorageAccount_PageBlob* es un contenedor. Por ejemplo, los contenedores se crean como *StorageAccount_BlockBlob/container* y *StorageAccount_PageBlob/container*.
- Todas las carpetas que se han creado creó directamente en *StorageAccount_AzureFiles* se traducen en un recurso compartido de archivos de Azure.
- Si un objeto que se va a copiar tiene el mismo nombre que un objeto de Azure, como un blob o un archivo, que ya está en la nube, Data Box sobrescribe el archivo en la nube.
- Todos los archivos escritos en los recursos compartidos *StorageAccount_BlockBlob* y *StorageAccount_BlockBlob* se cargan como blob en bloques y blob en páginas, respectivamente.
- Azure Blob Storage no admite directorios. Si crea una carpeta en la carpeta *StorageAccount_BlockBlob* y, después, se crean carpetas virtuales en el nombre del blob. Para Azure Files, se mantiene la estructura de directorios real.
- Todas las jerarquías de directorios vacías (sin archivos) creadas en las carpetas *StorageAccount_BlockBlob* y *StorageAccount_BlockBlob* no se cargan.
- Si se han producido errores al cargar datos en Azure, se crea un registro de errores en la cuenta de almacenamiento de destino. La ruta de acceso a este registro de errores está disponible cuando se completa la carga. Puede examinar el registro para realizar acciones correctivas. No elimine datos del origen sin comprobar los datos cargados.
- Los metadatos de archivo y los permisos NTFS se pueden conservar cuando se cargan los datos en Azure Files mediante las instrucciones de [Conservación de las ACL de archivo, los atributos y las marcas de tiempo con Azure Data Box](../articles/databox/data-box-file-acls-preservation.md).