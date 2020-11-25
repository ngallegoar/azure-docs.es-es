---
title: Visualización del estado de mantenimiento de la relación de replicación de Azure NetApp Files | Microsoft Docs
description: Describe cómo ver el estado de replicación en el volumen de origen o el volumen de destino de Azure NetApp Files.
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
ms.openlocfilehash: a14656bc1abc42fdc3e948fdae1ca07469681d21
ms.sourcegitcommit: 30906a33111621bc7b9b245a9a2ab2e33310f33f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/22/2020
ms.locfileid: "95249859"
---
# <a name="display-health-status-of-replication-relationship"></a>Visualización del estado de mantenimiento de la relación de la replicación 

Puede ver el estado de la replicación en el volumen de origen o en el volumen de destino.  

## <a name="display-replication-status"></a>Visualización del estado de la replicación

1. En el volumen de origen o de destino, haga clic en **Replicación** en Servicio de almacenamiento para cualquiera de los volúmenes.

    Se muestra el estado de replicación y la información de mantenimiento siguientes:  
    * **Tipo de punto de conexión**: identifica si el volumen es el origen o el destino de la replicación.
    * **Correcto**: muestra el estado de mantenimiento de la relación de replicación.
    * **Estado del reflejo**: muestra uno de los siguientes valores:
        * *Sin inicializar*:  
            Este es el estado inicial y predeterminado cuando se crea una relación de emparejamiento. El estado permanece así hasta que la inicialización se completa correctamente.
        * *Reflejado*:   
            El volumen de destino se ha inicializado y está listo para recibir actualizaciones de creación de reflejo.
        * *Interrumpido*:   
            Este es el estado que se establece después de interrumpir la relación de emparejamiento. El volumen de destino es `‘RW’` y hay instantáneas.
    * **Estado de la relación**: muestra uno de los siguientes valores: 
        * *Inactivo*:  
            No hay ninguna operación de transferencia en curso y las transferencias futuras no están deshabilitadas.
        * *Transfiriendo*:  
            Hay una operación de transferencia en curso y las transferencias futuras no están deshabilitadas.
    * **Programación de replicación**: muestra la frecuencia con que se realizarán las actualizaciones de creación de reflejo incrementales cuando se complete la inicialización (copia de línea de base).

    * **Progreso total**: muestra la cantidad total en bytes de datos transferidos en la operación de transferencia actual. Esta cantidad es el número real de bits transferidos; puede diferir del espacio lógico que notifican los volúmenes de origen y de destino.  

    ![Estado de mantenimiento de la replicación](../media/azure-netapp-files/cross-region-replication-health-status.png)

> [!NOTE] 
> El estado de la relación de replicación se mostrará como *Incorrecto* si no se completaron los trabajos de replicación anteriores. Este estado es el resultado de la transferencia de grandes volúmenes con un período de transferencia corto (por ejemplo, un tiempo de transferencia de diez minutos para un volumen grande). En este caso, el estado de la relación muestra *Transfiriendo* y el estado de mantenimiento muestra *Incorrecto*.

## <a name="next-steps"></a>Pasos siguientes  

* [Replicación entre regiones](cross-region-replication-introduction.md)
* [Administración de la recuperación ante desastres](cross-region-replication-manage-disaster-recovery.md)
* [Métricas de replicación de volúmenes](azure-netapp-files-metrics.md#replication)
* [Eliminación de volúmenes o replicaciones de volúmenes](cross-region-replication-delete.md)
* [Solución de problemas de la replicación entre regiones](troubleshoot-cross-region-replication.md)

