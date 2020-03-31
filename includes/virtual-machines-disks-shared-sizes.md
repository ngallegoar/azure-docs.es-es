---
title: archivo de inclusión
description: archivo de inclusión
services: virtual-machines
author: roygara
ms.service: virtual-machines
ms.topic: include
ms.date: 02/18/2020
ms.author: rogarana
ms.custom: include file
ms.openlocfilehash: 34699ed89e79448d66343021dd624cb872d0172d
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "77471721"
---
Por ahora, solo los discos SSD prémium pueden habilitar discos compartidos. Los tamaños de disco que admiten esta característica son P15 y superiores. Los distintos tamaños de disco pueden tener un límite de `maxShares` diferente, que no se puede superar al establecer el valor de `maxShares`.

Para cada disco, puede definir un valor de `maxShares` que represente el número máximo de nodos que puede compartir el disco simultáneamente. Por ejemplo, si planea configurar un clúster de conmutación por error de dos nodos, debe establecer `maxShares=2`. El valor máximo es un límite superior. Los nodos pueden unirse al clúster, o salir de él (montaje o desmontaje del disco) siempre que el número de nodos sea inferior al valor de `maxShares` especificado.

> [!NOTE]
> El valor de `maxShares` solo se puede establecer o editar cuando el disco se desasocia de todos los nodos.

En la tabla siguiente se muestran los valores máximos permitidos para `maxShares` por tamaño de disco:

|Tamaños de disco  |Límite de maxShares  |
|---------|---------|
|P15, P20     |2         |
|P30, P40, P50     |5         |
|P60, P70, P80     |10         |

Los límites de IOPS y ancho de banda de un disco no se ven afectados por el valor de `maxShares`. Por ejemplo, el número máximo de IOPS de un disco P15 es 1100 si maxShares = 1 o maxShares > 1.