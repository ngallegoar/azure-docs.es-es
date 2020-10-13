---
title: Modo de costo de Azure NetApp Files | Microsoft Docs
description: Describe el modelo de costo de Azure NetApp Files para administrar los gastos del servicio.
services: azure-netapp-files
documentationcenter: ''
author: b-juche
manager: ''
editor: ''
ms.assetid: ''
ms.service: azure-netapp-files
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 09/22/2020
ms.author: b-juche
ms.openlocfilehash: 9c4eebae6909c9ef0969bc85bcb9a985db2a7c02
ms.sourcegitcommit: 32c521a2ef396d121e71ba682e098092ac673b30
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/25/2020
ms.locfileid: "91325613"
---
# <a name="cost-model-for-azure-netapp-files"></a>Modelo de costo de Azure NetApp Files 

Entender el modelo de costo de Azure NetApp Files ayuda a administrar los gastos del servicio. 

Para el modelo de costos específico de la replicación entre regiones, consulte [Modelo de costos de la replicación entre regiones](cross-region-replication-introduction.md#cost-model-for-cross-region-replication).

## <a name="calculation-of-capacity-consumption"></a>Cálculo del consumo de la capacidad

Azure NetApp Files se factura en función de la capacidad de almacenamiento aprovisionada.  La capacidad aprovisionada se asigna mediante la creación de grupos de capacidad.  Los grupos de capacidad se facturan en función del importe en dólares, la cantidad aprovisionada de GiB y el mes en incrementos de una hora. El tamaño mínimo de un grupo de capacidad es de 4 TiB, y los grupos de capacidad se pueden expandir posteriormente en incrementos de 1 TiB. Los volúmenes se crean dentro de grupos de capacidad.  A cada volumen se le asigna una cuota que disminuye a partir de la capacidad aprovisionada de los grupos. La cuota que se puede asignar a los volúmenes oscila entre un mínimo de 100 GiB y un máximo de 100 TiB.  

Para un volumen activo, el consumo de la capacidad con respecto a la cuota se basa en la capacidad lógica (efectiva).

Si el consumo de capacidad real de un volumen supera su cuota de almacenamiento, el volumen puede continuar aumentando. Se seguirán permitiendo operaciones de escritura siempre que el tamaño del volumen real sea menor que el límite del sistema (100 TiB).  

La capacidad usada total en un grupo de capacidad con respecto a su cantidad aprovisionada es la suma de la cuota asignada o del consumo real de todos los volúmenes del grupo, lo que sea mayor: 

   ![Cálculo de la capacidad total usada](../media/azure-netapp-files/azure-netapp-files-total-used-capacity.png)

En el diagrama siguiente se ilustran estos conceptos.  
* Hay un grupo de capacidad con 4 TiB de capacidad aprovisionada.  El grupo contiene tres volúmenes.  
    * Al Volumen 1 se le asigna una cuota de 2 TiB y tiene 800 GiB de consumo.  
    * Al Volumen 2 se le asigna una cuota de 1 TiB y tiene 100 GiB de consumo.  
    * Al Volumen 3 se le asigna una cuota de 500 GiB y tiene 800 GiB de consumo (de media).  
* El grupo de capacidad se mide para 4 TiB de capacidad (la cantidad aprovisionada).  
    Se consumen 3,8 TiB de capacidad (2 TiB y 1 TiB de cuota de los volúmenes 1 y 2, y 800 GiB de consumo real para el volumen 3). Y quedan 200 GiB de capacidad restante.

   ![Grupo de capacidad con tres volúmenes](../media/azure-netapp-files/azure-netapp-files-capacity-pool-with-three-vols.png)

## <a name="overage-in-capacity-consumption"></a>Uso por encima del límite del consumo de capacidad  

Cuando el total de capacidad usada de un grupo supera su capacidad aprovisionada, todavía se permiten las operaciones de escritura de datos.  Tras el período de gracia (una hora), si la capacidad usada del grupo todavía supera su capacidad aprovisionada, el tamaño del grupo aumentará de forma automática en incrementos de 1 TiB hasta que la capacidad aprovisionada sea mayor que la capacidad total usada.  Por ejemplo, en la ilustración anterior, si el Volumen 3 sigue creciendo y el consumo real alcanza 1,2 TiB, tras el período de gracia el tamaño del grupo se establecerá de forma automática en 5 TiB.  Como resultado, la capacidad aprovisionada del grupo (5 TiB) supera la capacidad usada (4,2 TiB).  

Aunque el tamaño del grupo de capacidad aumenta automáticamente para satisfacer la demanda del volumen, no se reduce automáticamente cuando se reduce el tamaño del volumen. Si quiere reducir el tamaño del grupo de capacidad después de una disminución del volumen (por ejemplo, después de limpiar los datos de un volumen), debe reducirlo _manualmente_.

## <a name="manual-changes-of-the-pool-size"></a>Cambios manuales del tamaño del grupo  

Puede aumentar o disminuir el tamaño del grupo de forma manual. Pero se aplican las restricciones siguientes:
* Límites mínimos y máximos de servicio.  
    Vea el artículo sobre [límites de recursos](azure-netapp-files-resource-limits.md).
* Un incremento de 1 TiB después de la compra mínima de 4 TiB iniciales.
* Un incremento de facturación mínimo de una hora.
* El tamaño del grupo aprovisionado no se puede disminuir a menos del total de capacidad usada del grupo.
* En el caso de los grupos de capacidad con QoS manual, el tamaño del grupo solo se puede reducir si el tamaño y el nivel de servicio proporcionan más rendimiento que el rendimiento real asignado de todos los volúmenes.

## <a name="behavior-of-maximum-size-pool-overage"></a>Comportamiento del uso por encima del límite del grupo de tamaño máximo   

El tamaño máximo de un grupo de capacidad que puede crear o al que puede cambiar es de 500 TiB.  Cuando el total de capacidad usada en un grupo de capacidad supera los 500 TiB, se producen las situaciones siguientes:
* Todavía se podrán realizar operaciones de escritura de datos (si el volumen está por debajo del máximo del sistema de 100 TiB).
* Tras el período de gracia de una hora, el tamaño del grupo se establecerá de forma automática en incrementos de 1 TiB, hasta que la capacidad aprovisionada del grupo supere la capacidad usada total.
* La capacidad adicional aprovisionada y facturada por encima de 500 TiB no se puede usar para asignar la cuota de volumen. Tampoco se puede usar para ampliar los límites de calidad de servicio de rendimiento.  
    Vea [niveles de servicio](azure-netapp-files-service-levels.md) para obtener información sobre los límites de rendimiento y el ajuste de tamaño de calidad de servicio.

En el diagrama siguiente se ilustran estos conceptos:
* Hay un grupo de capacidad con una capa de almacenamiento Premium y una capacidad de 500 TiB. El grupo contiene nueve volúmenes.
    * A los volúmenes de 1 a 8 se les asigna una cuota de 60 TiB.  La capacidad total usada es 480 TiB.  
        Cada volumen tiene un límite de QoS de 3,75 GiB/s de rendimiento (60 TiB * 64 MiB/s).  
    * Al volumen 9 se le asigna una cuota de 20 TiB.  
        El volumen 9 tiene un límite de QoS de 1,25 GiB/s de rendimiento (20 TiB * 64 MiB/s).
* El volumen 9 es un escenario de uso por encima del límite. Tiene 25 TiB de consumo real.  
    * Tras el período de gracia de una hora, el tamaño del grupo de capacidad se ajustará a 505 TiB.  
        Es decir, la capacidad total usada = 8 * la cuota de 60 TiB para los volúmenes de 1 a 8, y 25 TiB de consumo real para el volumen 9.
    * La capacidad facturada es de 505 TiB.
    * No se puede aumentar la cuota del volumen 9 (dado que la cuota total asignada para el grupo no puede superar los 500 TiB).
    * Es posible que no se pueda asignar rendimiento de QoS adicional (porque la calidad de servicio total para el grupo todavía se basa en 500 TiB).

   ![Grupo de capacidad con nueve volúmenes](../media/azure-netapp-files/azure-netapp-files-capacity-pool-with-nine-vols.png)

## <a name="capacity-consumption-of-snapshots"></a>Consumo de capacidad de instantáneas 

El consumo de la capacidad de instantáneas de Azure NetApp Files se cobra en función de la cuota del volumen primario.  Como resultado, comparte la misma tarifa de facturación que el grupo de capacidad al que pertenece el volumen.  Pero, a diferencia del volumen activo, el consumo de instantáneas se mide según la capacidad incremental consumida.  Las instantáneas de Azure NetApp Files son diferenciales por naturaleza. Según la velocidad de cambio de los datos, las instantáneas a menudo consumen mucha menos capacidad que la capacidad lógica del volumen activo. Por ejemplo, imagine que tiene una instantánea de un volumen de 500 GiB que solo contiene 10 GiB de datos diferenciales. La capacidad que se cobra con respecto a la cuota de volumen para esa instantánea sería de 10 GiB, no de 500 GiB. 

## <a name="next-steps"></a>Pasos siguientes

* [Página de precios de Azure NetApp Files](https://azure.microsoft.com/pricing/details/storage/netapp/)
* [Niveles de servicio para Azure NetApp Files](azure-netapp-files-service-levels.md)
* [Límites de recursos para Azure NetApp Files](azure-netapp-files-resource-limits.md)
* [Modelo de costos de la replicación entre regiones](cross-region-replication-introduction.md#cost-model-for-cross-region-replication)
