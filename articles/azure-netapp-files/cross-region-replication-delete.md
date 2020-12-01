---
title: Eliminación de volúmenes o replicaciones de volúmenes para la replicación entre regiones de Azure NetApp Files | Microsoft Docs
description: Describe cómo eliminar una conexión de replicación que ya no es necesaria entre los volúmenes de origen y de destino.
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
ms.date: 11/18/2020
ms.author: b-juche
ms.openlocfilehash: 5ce7a591acd8203775808457219b0ec392cd696e
ms.sourcegitcommit: 30906a33111621bc7b9b245a9a2ab2e33310f33f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/22/2020
ms.locfileid: "95249901"
---
# <a name="delete-volume-replications-or-volumes"></a>Eliminación de volúmenes o replicaciones de volúmenes

En este artículo se describe cómo eliminar replicaciones de volúmenes. También se describe cómo eliminar el volumen de origen o de destino.

## <a name="delete-volume-replications"></a>Eliminación de replicaciones de volúmenes

Puede finalizar la conexión de replicación entre los volúmenes de origen y de destino mediante la eliminación de la replicación de volúmenes. Debe eliminar la replicación del volumen de destino. La operación de eliminación solo quita la autorización para la replicación; no quita el volumen de origen ni de destino. 

1. Debe interrumpir el emparejamiento de replicación antes de eliminar la replicación del volumen. Para interrumpir el emparejamiento de la replicación: 

    1. Seleccione el volumen de *destino*. Haga clic en **Replicación** en el servicio de almacenamiento.  

    2.  Antes de continuar, compruebe los campos siguientes:  
        * Asegúrese de que en Estado del reflejo aparezca ***Reflejado** _.   
            No intente romper el emparejamiento de la replicación si el Estado del reflejo es _No inicializado*.
        * Asegúrese de que el Estado de la relación es ***Inactivo** _.   
            No intente romper el emparejamiento de la replicación si el Estado de la relación es _Transfiriendo*.   

        Consulte [Visualización del estado de mantenimiento de la relación de la replicación](cross-region-replication-display-health-status.md). 

    3.  Haga clic en **Romper emparejamiento**.  

    4.  Indique **Sí** cuando se le solicite y haga clic en **Interrumpir**. 

        ![Romper emparejamiento de replicación](../media/azure-netapp-files/cross-region-replication-break-replication-peering.png)


1. Para eliminar la replicación de volúmenes, seleccione **Replicación** desde el volumen de origen o de destino.  

2. Haga clic en **Eliminar**.    

3. Para confirmar la eliminación, escriba **Sí** y haga clic en **Eliminar**.   

    ![Eliminación de la replicación](../media/azure-netapp-files/cross-region-replication-delete-replication.png)

## <a name="delete-source-or-destination-volumes"></a>Eliminación de volúmenes de origen o de destino

Si quiere eliminar el volumen de origen o de destino, debe seguir los pasos a continuación en el orden descrito. De lo contrario, se producirá el error `Volume with replication cannot be deleted`.  

1. Desde el volumen de destino, [elimine la replicación del volumen](#delete-volume-replications).   

2. Elimine el volumen de origen o de destino según sea necesario; para ello, haga clic con el botón derecho en el nombre del volumen y seleccione **Eliminar**.   

    ![Captura de pantalla que muestra el menú contextual de un volumen.](../media/azure-netapp-files/cross-region-replication-delete-volume.png)

## <a name="next-steps"></a>Pasos siguientes  

* [Replicación entre regiones](cross-region-replication-introduction.md)
* [Requisitos y consideraciones del uso de la replicación entre regiones](cross-region-replication-requirements-considerations.md)
* [Visualización del estado de mantenimiento de la relación de la replicación](cross-region-replication-display-health-status.md)
* [Solución de problemas de replicación entre regiones](troubleshoot-cross-region-replication.md)

