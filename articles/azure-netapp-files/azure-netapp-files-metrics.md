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
ms.date: 09/10/2020
ms.author: b-juche
ms.openlocfilehash: 1690a844ff700a2975be8e972fd90ba71eeb937c
ms.sourcegitcommit: 7374b41bb1469f2e3ef119ffaf735f03f5fad484
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/16/2020
ms.locfileid: "90707788"
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
    La suma del tamaño de la instantánea de todos los volúmenes del grupo.

## <a name="usage-metrics-for-volumes"></a><a name="volumes"></a>Métricas de uso de volúmenes

<!--
- *Volume Quota Size*    
    The quota size (GiB) the volume is provisioned with.   
    This size is the size you selected during capacity pool creation. 
-->
- *Tamaño consumido del volumen*   
    Espacio lógico total usado en un volumen (GiB).  
    Este tamaño incluye el espacio lógico usado por las instantáneas y los sistemas de archivos activos.  
- *Tamaño de instantánea de volumen*   
   Espacio lógico incremental usado por las instantáneas de un volumen.  

## <a name="performance-metrics-for-volumes"></a>Métricas de rendimiento de los volúmenes

- *Latencia de lectura media*   
    Tiempo promedio de las lecturas del volumen en milisegundos.
- *Latencia de escritura media*   
    Tiempo promedio de las escrituras del volumen en milisegundos.
- *IOPS de lectura*   
    Número de lecturas en el volumen por segundo.
- *IOPS de escritura*   
    Número de escrituras en el volumen por segundo.

## <a name="volume-replication-metrics"></a><a name="replication"></a>Métricas de replicación de volúmenes

- *¿Es correcto el estado de replicación del volumen?*    
    La condición de la relación de replicación. 

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
