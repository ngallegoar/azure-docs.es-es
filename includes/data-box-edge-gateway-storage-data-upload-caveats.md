---
author: alkohli
ms.service: databox
ms.topic: include
ms.date: 02/21/2019
ms.author: alkohli
ms.openlocfilehash: 8c87e14071b3bb40421ab655c172df739570e295
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "67186770"
---
Las advertencias siguientes se aplican a los datos cuando se trasladan en Azure.

- Se recomienda que no haya más de un dispositivo que escriba en el mismo contenedor.
- Si tiene un objeto existente de Azure (por ejemplo, un blob o un archivo) en la nube con el mismo nombre que el objeto que se está copiando, el dispositivo sobrescribirá el archivo en la nube.
- Una jerarquía de directorios vacía (sin archivos) que se creó en las carpetas de recurso compartido no se carga en los contenedores de blobs.
- Puede copiar los datos mediante el método de arrastrar y colocar con el Explorador de archivos o mediante la línea de comandos. Si el tamaño total de los archivos que se copian es mayor que 10 GB, se recomienda usar un programa de copia masiva, como Robocopy o rsync. Las herramientas de copia masiva reintentan la operación de copia en caso de errores intermitentes y proporcionan resistencia adicional.
- Si el recurso compartido asociado al contenedor de almacenamiento de Azure carga los blobs que no coinciden con el tipo de blobs definido para el recurso compartido en el momento de creación, estos blobs no se actualizan. Por ejemplo, se crea un recurso compartido de blob en bloques en el dispositivo. Se asocia el recurso compartido a un contenedor en la nube existente que tiene blobs en páginas. Se actualiza ese recurso compartido para descargar los archivos. Se modifican algunos de los archivos actualizados que ya están almacenados como blobs en páginas en la nube. Aparecerán errores de carga.
