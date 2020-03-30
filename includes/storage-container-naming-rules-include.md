---
author: tamram
ms.service: storage
ms.topic: include
ms.date: 10/26/2018
ms.author: tamram
ms.openlocfilehash: aeca3550b5fc58694779e7e54ce6ca547ba30e17
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "67186930"
---
Cada blob del almacenamiento de Azure debe residir en un contenedor. El contenedor forma parte del nombre del blob. Por ejemplo, `mycontainer` es el nombre del contenedor de estos URI de blob de ejemplo:

    https://storagesample.blob.core.windows.net/mycontainer/blob1.txt
    https://storagesample.blob.core.windows.net/mycontainer/photos/myphoto.jpg

Un nombre de contenedor debe ser un nombre DNS válido y cumplir las reglas de nomenclatura siguientes:

1. Los nombres de contenedor deben comenzar por una letra o un número, y pueden contener solo letras, números y el carácter de guión (-).
2. Todos los caracteres de guión (-) deben estar inmediatamente precedidos y seguidos por una letra o un número; no se permiten guiones consecutivos en nombres de contenedor.
3. Todas las letras del nombre de un contenedor deben aparecer en minúsculas.
4. Los nombres de contenedor deben tener entre 3 y 63 caracteres de longitud.

> [!IMPORTANT]
> Tenga en cuenta que el nombre de un contenedor siempre debe estar en minúsculas. Si se incluye una letra mayúscula en un nombre de contenedor o se infringe de algún otro modo las reglas de nomenclatura de contenedores, recibirá un error 400 (solicitud incorrecta). 
> 
> 

