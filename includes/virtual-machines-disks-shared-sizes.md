---
title: archivo de inclusión
description: archivo de inclusión
services: virtual-machines
author: roygara
ms.service: virtual-machines
ms.topic: include
ms.date: 04/06/2020
ms.author: rogarana
ms.custom: include file
ms.openlocfilehash: 0b185d545e129c941d5df2e8ce86ee684174b666
ms.sourcegitcommit: ae3d707f1fe68ba5d7d206be1ca82958f12751e8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/10/2020
ms.locfileid: "81008367"
---
De momento, solo los discos Ultra Disks y SSD Premium pueden habilitar discos compartidos. Los distintos tamaños de disco pueden tener un límite de `maxShares` diferente, que no se puede superar al establecer el valor de `maxShares`. En el caso de los discos SSD Premium, los tamaños de disco que admiten el uso compartido de discos son P15 y superiores.

Para cada disco, puede definir un valor de `maxShares` que represente el número máximo de nodos que puede compartir el disco simultáneamente. Por ejemplo, si planea configurar un clúster de conmutación por error de dos nodos, debe establecer `maxShares=2`. El valor máximo es un límite superior. Los nodos pueden unirse al clúster, o salir de él (montaje o desmontaje del disco) siempre que el número de nodos sea inferior al valor de `maxShares` especificado.

> [!NOTE]
> El valor de `maxShares` solo se puede establecer o editar cuando el disco se desasocia de todos los nodos.

### <a name="premium-ssd-ranges"></a>Rangos de discos SSD Premium

En la tabla siguiente se muestran los valores máximos permitidos para `maxShares` por tamaños de discos Premium:

|Tamaños de disco  |Límite de maxShares  |
|---------|---------|
|P15, P20     |2         |
|P30, P40, P50     |5         |
|P60, P70, P80     |10         |

Los límites de IOPS y ancho de banda de un disco no se ven afectados por el valor de `maxShares`. Por ejemplo, el número máximo de IOPS de un disco P15 es 1100 si maxShares = 1 o maxShares > 1.

### <a name="ultra-disk-ranges"></a>Rangos de discos Ultra Disks

El valor mínimo de `maxShares` es de 1, mientras que el valor máximo de `maxShares` es de 5. No hay restricciones de tamaño en los discos Ultra Disks, cualquier disco de tamaño ultra puede usar cualquier valor para `maxShares`, hasta el valor máximo incluido.