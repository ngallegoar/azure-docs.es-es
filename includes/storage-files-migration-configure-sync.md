---
title: Configuración de Azure File Sync
description: Configure Azure File Sync. Un bloque de texto común, compartido entre los documentos de migración.
author: fauhse
ms.service: storage
ms.topic: conceptual
ms.date: 2/20/2020
ms.author: fauhse
ms.subservice: files
ms.openlocfilehash: 53d7af6b43ff24ab12501570385162759f4c7633
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "78209331"
---
Este paso une todos los recursos y carpetas que ha configurado en la instancia de Windows Server durante los pasos anteriores.

* Inicie sesión en el [Portal de Azure](https://portal.azure.com).
* Busque el recurso del servicio de sincronización de almacenamiento.
* Cree un nuevo *grupo de sincronización* en el recurso del servicio de sincronización de almacenamiento para cada recurso compartido de archivos de Azure. En la terminología de Azure File Sync, el recurso compartido de archivos de Azure se convertirá en *punto de conexión de la nube* en la topología de sincronización que describe con la creación de un grupo de sincronización. Al crear el grupo de sincronización, asígnele un nombre descriptivo, de modo que reconozca el conjunto de archivos que se sincronizan aquí. Asegúrese de hacer referencia al recurso compartido de archivos de Azure con un nombre coincidente.
* Una vez creado el grupo de sincronización, verá que aparece una fila para él en la lista de grupos de sincronización. Haga clic en el nombre (un vínculo) para mostrar el contenido del grupo de sincronización. Verá el recurso compartido de archivos de Azure en "Puntos de conexión en la nube".
* Busque el botón de comando para *+ Agregar de punto de conexión del servidor*. La carpeta del servidor local que ha aprovisionado se convertirá en la ruta de acceso de este *punto de conexión del servidor*.
