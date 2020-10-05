---
title: Administración de la recuperación ante desastres para la replicación entre regiones de Azure NetApp Files | Microsoft Docs
description: Describe cómo administrar la recuperación ante desastres mediante la replicación entre regiones de Azure NetApp Files.
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
ms.topic: how-to
ms.date: 09/16/2020
ms.author: b-juche
ms.openlocfilehash: ad006279a656758ba856cd3f39c17b0410e525e6
ms.sourcegitcommit: 7374b41bb1469f2e3ef119ffaf735f03f5fad484
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/16/2020
ms.locfileid: "90708440"
---
# <a name="manage-disaster-recovery-using-cross-region-replication"></a>Administración de la recuperación ante desastres mediante la replicación entre regiones 

Una replicación continua entre los volúmenes de origen y de destino (consulte [Creación de emparejamiento de replicación](cross-region-replication-create-peering.md)) le prepara para un evento de recuperación ante desastres. 

Cuando se produce este tipo de evento, puede [aplicar una conmutación por error en el volumen de destino](#break-replication-peering-to-activate-the-destination-volume), lo que permite al cliente leer y escribir en el volumen de destino. 

Después de la recuperación ante desastres, puede llevar a cabo una conmutación por recuperación en el volumen de origen con una [operación de resincronización](#resync-replication-to-reactivate-the-source-volume) que sobrescribe los datos del volumen de origen con los datos del volumen de destino.  A continuación, [restablezca la replicación de origen a destino](#reestablish-source-to-destination-replication) y vuelva a montar el volumen de origen para que el cliente tenga acceso a él. 

A continuación se describen los detalles. 

## <a name="break-replication-peering-to-activate-the-destination-volume"></a>Interrupción del emparejamiento de replicación para activar el volumen de destino

Cuando necesite activar el volumen de destino (por ejemplo, si desea conmutar por error a la región de destino), debe romper el emparejamiento de replicación y, a continuación, montar el volumen de destino.  

1. Para romper el emparejamiento de la replicación, seleccione el volumen de destino. Haga clic en **Replicación** en el servicio de almacenamiento.  

2.  Antes de continuar, compruebe los campos siguientes:  
    * Asegúrese de que en Estado del reflejo aparece ***Reflejado***.   
        No intente romper el emparejamiento de la replicación si el Estado del reflejo es *No inicializado*.
    * Asegúrese de que el Estado de la relación es ***Inactivo***.   
        No intente romper el emparejamiento de la replicación si el Estado de la relación es *Transfiriendo*.   

    Consulte [Visualización del estado de mantenimiento de la relación de la replicación](cross-region-replication-display-health-status.md). 

3.  Haga clic en **Romper emparejamiento**.  

4.  Indique **Sí** cuando se le solicite y haga clic en el botón **Romper**. 

    ![Romper emparejamiento de replicación](../media/azure-netapp-files/cross-region-replication-break-replication-peering.png)

5.  Monte el volumen de destino siguiendo los pasos descritos en [Montaje o desmontaje de un volumen para máquinas virtuales Linux](azure-netapp-files-mount-unmount-volumes-for-virtual-machines.md).   
    Este paso permite que un cliente tenga acceso al volumen de destino.

## <a name="resync-replication-to-reactivate-the-source-volume"></a>Resincronización de la replicación para reactivar el volumen de origen   

Después de la recuperación ante desastres, puede reactivar el volumen de origen mediante una operación de resincronización.  La operación de resincronización invierte el proceso de replicación y sincroniza los datos del volumen de destino con el volumen de origen.  

> [!IMPORTANT] 
> La operación de resincronización sobrescribe los datos del volumen de origen con los datos del volumen de destino.  La interfaz de usuario le advierte sobre la potencial pérdida de datos. Se le pedirá que confirme la acción de resincronización antes de iniciar la operación.

1. Para resincronizar la replicación, seleccione el volumen de *origen*. Haga clic en **Replicación** en el servicio de almacenamiento. A continuación, haga clic en **Resincronizar**.  

2. Indique **Sí** cuando se le solicite y haga clic en el botón **Resincronizar**. 
 
    ![Resincronizar replicación](../media/azure-netapp-files/cross-region-replication-resync-replication.png)

3. Supervise el estado de mantenimiento del volumen de origen siguiendo los pasos descritos en [Visualización del estado de mantenimiento de la relación de la replicación](cross-region-replication-display-health-status.md).   
    Cuando el estado de mantenimiento del volumen de origen muestra los valores siguientes, la operación de resincronización está completa y los cambios realizados en el volumen de destino se capturan ahora en el volumen de origen:   

    * Estado reflejado: *Reflejado*  
    * Estado de la transferencia: *Idle*  

## <a name="reestablish-source-to-destination-replication"></a>Restablecimiento de la replicación de origen a destino

Una vez completada la operación de resincronización del destino al origen, deberá romper de nuevo el emparejamiento de la replicación para volver a establecer la replicación de origen a destino. También debe volver a montar el volumen de origen para que el cliente pueda acceder a él.  

1. Rompa el emparejamiento de la replicación:  
    a. Seleccione el volumen de *destino*. Haga clic en **Replicación** en el servicio de almacenamiento.  
    b. Antes de continuar, compruebe los campos siguientes:   
    * Asegúrese de que en Estado del reflejo aparece ***Reflejado***.   
    No intente romper el emparejamiento de la replicación si el Estado del reflejo es *No inicializado*.  
    * Asegúrese de que el Estado de la relación es ***Inactivo***.   
    No intente romper el emparejamiento de la replicación si el Estado de la relación es *Transfiriendo*.    

        Consulte [Visualización del estado de mantenimiento de la relación de la replicación](cross-region-replication-display-health-status.md). 

    c. Haga clic en **Romper emparejamiento**.   
    d. Indique **Sí** cuando se le solicite y haga clic en el botón **Romper**.  

2. Vuelva a sincronizar el volumen de origen con el volumen de destino:  
    a. Seleccione el volumen de *destino*. Haga clic en **Replicación** en el servicio de almacenamiento. A continuación, haga clic en **Resincronizar**.   
    b. Indique **Sí** cuando se le solicite y haga clic en el botón **Resincronizar**.

3. Vuelva a montar el volumen de origen siguiendo los pasos descritos en [Montaje o desmontaje de un volumen para máquinas virtuales Linux](azure-netapp-files-mount-unmount-volumes-for-virtual-machines.md).  
    Este paso permite a un cliente tener acceso al volumen de origen.

## <a name="next-steps"></a>Pasos siguientes  

* [Replicación entre regiones](cross-region-replication-introduction.md)
* [Requisitos y consideraciones del uso de la replicación entre regiones](cross-region-replication-requirements-considerations.md)
* [Visualización del estado de mantenimiento de la relación de la replicación](cross-region-replication-display-health-status.md)
* [Métricas de replicación de volúmenes](azure-netapp-files-metrics.md#replication)
* [Solución de problemas de la replicación entre regiones](troubleshoot-cross-region-replication.md)

