---
author: cynthn
ms.service: virtual-machines
ms.topic: include
ms.date: 10/26/2018
ms.author: cynthn
ms.openlocfilehash: 28aab15dc67e051190e8d4e35e92240a56fe54a6
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "67186263"
---
Después, si algún servidor del clúster está ejecutando Windows Server 2008 R2 o Windows Server 2012, debe comprobar que la revisión [KB2854082](https://support.microsoft.com/kb/2854082) está instalada en todos los servidores locales o máquinas virtuales de Azure que forman parte del clúster. Cualquier servidor o VM que esté en el clúster, pero no en el grupo de disponibilidad, también debería tener instalada esta revisión.

En la sesión de escritorio remoto para todos los nodos del clúster, descarga la [KB2854082](https://support.microsoft.com/kb/2854082) en un directorio local. A continuación, instale la revisión en todos los nodos del clúster secuencialmente. Si en ese momento el servicio de clúster se está ejecutando en el nodo de clúster, el servidor se reiniciará al final de la instalación de la revisión.

> [!WARNING]
> Detener el servicio de clúster o reiniciar el servidor afectará al estado del cuórum del clúster y al grupo de disponibilidad y puede provocar que el clúster se quede sin conexión. Para mantener la alta disponibilidad del clúster durante la instalación, asegúrate de que:
> 
> * El clúster se encuentra en un estado de cuórum óptimo. 
> * Todos los nodos del clúster están conectados antes de instalar la revisión en cualquier nodo.
> * Antes de instalar la revisión en cualquier otro nodo del clúster, permita que se ejecute la instalación de la revisión hasta su finalización en un nodo, incluido el reinicio completo del servidor.
> 
> 

