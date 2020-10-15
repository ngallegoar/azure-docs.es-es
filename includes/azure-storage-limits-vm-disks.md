---
author: roygara
ms.service: virtual-machines
ms.topic: include
ms.date: 03/18/2019
ms.author: rogarana
ms.openlocfilehash: e89676505bc211d01a4327f8816a048218e579fe
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "91828805"
---
Puede asociar un número de discos de datos a una máquina virtual de Azure. Según los objetivos de escalabilidad y rendimiento de los discos de datos de una máquina virtual, puede determinar el número y el tipo de disco necesarios para satisfacer sus requisitos de capacidad y rendimiento.

> [!IMPORTANT]
> Para obtener un rendimiento óptimo, limite el número de discos muy usados que se conectan a la máquina virtual para evitar una posible limitación. Si todos los discos asociados no se usan mucho al mismo tiempo, la máquina virtual puede admitir un mayor número de discos.

**Para discos administrados de Azure:**

En la tabla siguiente se muestran los límites predeterminado y máximo del número de recursos por región y suscripción. No hay ningún límite en el número de discos administrados, instantáneas e imágenes por grupo de recursos.  

> | Resource | Límite |
> | --- | --- |
> | Discos administrados estándar | 50.000 |
> | Discos administrados SSD estándar | 50.000 |
> | Discos administrados Premium | 50.000 |
> | Instantáneas Standard_LRS | 50.000 |
> | Instantáneas Standard_ZRS | 50.000 |
> | Imagen administrada | 50.000 |

**Para cuentas de almacenamiento estándar:** una cuenta de almacenamiento estándar tiene una tasa de solicitudes máxima total de 20 000 IOPS. El número total de IOPS en todos los discos de máquina virtual de una cuenta de almacenamiento estándar no debe superar este límite.
  
    You can roughly calculate the number of highly utilized disks supported by a single Standard storage account based on the request rate limit. For example, for a Basic tier VM, the maximum number of highly utilized disks is about 66, which is 20,000/300 IOPS per disk. The maximum number of highly utilized disks for a Standard tier VM is about 40, which is 20,000/500 IOPS per disk. 

**Para cuentas de almacenamiento premium:** una cuenta de almacenamiento premium tiene una capacidad total máxima de proceso de 50 Gbps. La capacidad total de proceso en todos los discos de la máquina virtual no debe superar este límite.

