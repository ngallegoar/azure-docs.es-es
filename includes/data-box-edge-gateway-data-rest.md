---
author: alkohli
ms.service: databox
ms.topic: include
ms.date: 08/21/2019
ms.author: alkohli
ms.openlocfilehash: eb55d993ad8960f821c2b72f0a53602166b7cc7e
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "69900606"
---
Para los datos en reposo:

- El acceso a los datos almacenados en recursos compartidos está restringido.

    - Los clientes SMB que tienen acceso a datos del recurso compartido necesitan credenciales de usuario asociadas al recurso compartido. Estas credenciales se definen cuando se crea el recurso compartido.
    - Las direcciones IP de los clientes NFS con acceso a un recurso compartido deben agregarse cuando se crea el recurso compartido.
