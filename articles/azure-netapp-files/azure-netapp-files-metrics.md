---
title: Métricas de Azure NetApp Files | Microsoft Docs
description: Azure NetApp Files proporciona métricas sobre el almacenamiento asignado, el uso de almacenamiento real, el número de IOPS del volumen y la latencia. Use estas métricas para comprender el uso y el rendimiento.
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
ms.date: 10/13/2020
ms.author: b-juche
ms.openlocfilehash: c79586703c49fe37d4d0915f49b69e6aa842083e
ms.sourcegitcommit: 2c586a0fbec6968205f3dc2af20e89e01f1b74b5
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/14/2020
ms.locfileid: "92017529"
---
# <a name="metrics-for-azure-netapp-files"></a>Métricas de Azure NetApp Files

Azure NetApp Files proporciona métricas sobre el almacenamiento asignado, el uso de almacenamiento real, el número de IOPS del volumen y la latencia. Mediante el análisis de estas métricas, puede comprender mejor el patrón de uso y el rendimiento de volumen de las cuentas de NetApp.  

## <a name="usage-metrics-for-capacity-pools"></a><a name="capacity_pools"></a>Métricas de uso de grupos de capacidad

- *Tamaño asignado del grupo*   
    El tamaño aprovisionado del grupo.

- *Grupo asignado al tamaño del volumen*  
    Cuota de volumen (GiB) total en un grupo de capacidad determinado (es decir, el total de los tamaños aprovisionados de los volúmenes en el grupo de capacidad).  
    Es el tamaño que se selecciona durante la creación del volumen.  

- *Tamaño consumido del grupo*  
    Espacio lógico (GiB) total usado en todos los volúmenes de un grupo de capacidad.  

- *Tamaño de instantánea total del grupo*    
    Suma del tamaño de la instantánea de todos los volúmenes del grupo.

## <a name="usage-metrics-for-volumes"></a><a name="volumes"></a>Métricas de uso de volúmenes

<!-- ANF-5023: fixed version: 2020.08, 2020.09
- *Percentage Volume Consumed Size*    
    The percentage of the volume consumed, including snapshots.  
-->
- *Tamaño asignado del volumen*   
    Tamaño aprovisionado de un volumen
- *Tamaño de cuota del volumen*    
    Tamaño de la cuota (GiB) con el que se aprovisiona el volumen.   
- *Tamaño consumido del volumen*   
    Tamaño lógico del volumen (bytes usados).  
    Este tamaño incluye el espacio lógico usado por las instantáneas y los sistemas de archivos activos.  
- *Tamaño de instantánea de volumen*   
   Tamaño de todas las instantáneas de un volumen.  

## <a name="performance-metrics-for-volumes"></a>Métricas de rendimiento de los volúmenes

- *Latencia de lectura media*   
    Tiempo promedio de las lecturas del volumen en milisegundos.
- *Latencia de escritura media*   
    Tiempo promedio de las escrituras del volumen en milisegundos.
- *IOPS de lectura*   
    Número de lecturas en el volumen por segundo.
- *IOPS de escritura*   
    Número de escrituras en el volumen por segundo.
<!-- These two metrics are not yet available, until ~ 2020.09
- *Read MiB/s*   
    Read throughput in bytes per second.
- *Write MiB/s*   
    Write throughput in bytes per second.
--> 
<!-- ANF-4128; 2020.07
- *Pool Provisioned Throughput*   
    The total throughput a capacity pool can provide to its volumes based on "Pool Provisioned Size" and "Service Level".
- *Pool Allocated to Volume Throughput*   
    The total throughput allocated to volumes in a given capacity pool (that is, the total of the volumes' allocated throughput in the capacity pool).
-->

<!-- ANF-6443; 2020.11
- *Pool Consumed Throughput*    
    The total throughput being consumed by volumes in a given capacity pool.
-->


## <a name="volume-replication-metrics"></a><a name="replication"></a>Métricas de replicación de volúmenes

> [!NOTE] 
> El tamaño de transferencia de red (por ejemplo, las métricas de la *transferencia total de replicación de volúmenes*) pueden diferir de los volúmenes de origen o de destino de una replicación entre regiones. Este comportamiento es el resultado del uso de un motor de replicación eficaz para minimizar el costo de la transferencia de red.

- *¿Es correcto el estado de replicación del volumen?*    
    La condición de la relación de replicación. `1` indica un estado correcto. `0` indica un estado incorrecto.

- *¿Se está transfiriendo la replicación del volumen?*     
    Indica si el estado de la replicación del volumen es "transfiriéndose". 
 
- *Tiempo de retardo de replicación del volumen*   
    Cantidad de tiempo en segundos que los datos del reflejo se retrasan detrás del origen. 

- *Duración de la última transferencia de replicación de volumen*   
    Cantidad de tiempo en segundos que tardó en completarse la última transferencia. 

- *Tamaño de última transferencia de replicación de volumen*    
    Número total de bytes transferidos como parte de la última transferencia. 

- *Progreso de la replicación del volumen*    
    La cantidad total de datos transferidos para la operación de transferencia actual. 

- *Transferencia total de la replicación del volumen*   
    Los bytes acumulados transferidos para la relación. 

## <a name="next-steps"></a>Pasos siguientes

* [Información sobre la jerarquía del almacenamiento de Azure NetApp Files](azure-netapp-files-understand-storage-hierarchy.md)
* [Configuración de un grupo de capacidad](azure-netapp-files-set-up-capacity-pool.md)
* [Creación de un volumen de Azure NetApp Files](azure-netapp-files-create-volumes.md)
