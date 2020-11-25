---
title: Eliminación de replicaciones para la replicación entre regiones de Azure NetApp Files | Microsoft Docs
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
ms.date: 11/17/2020
ms.author: b-juche
ms.openlocfilehash: e08b69271ba9d115c26418bc5e421ee6c94b031d
ms.sourcegitcommit: e2dc549424fb2c10fcbb92b499b960677d67a8dd
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/17/2020
ms.locfileid: "94695714"
---
# <a name="delete-replications"></a>Eliminación de replicaciones

Puede finalizar la conexión de replicación entre los volúmenes de origen y de destino mediante la eliminación de la replicación de volúmenes. Puede realizar la operación de eliminación desde el volumen de origen o de destino. La operación de eliminación solo quita la autorización para la replicación; no quita el volumen de origen ni de destino. 

## <a name="steps"></a>Pasos

1. Debe interrumpir el emparejamiento de replicación antes de eliminar la replicación del volumen.    
    Consulte [Visualización del estado de mantenimiento de la relación de la replicación](cross-region-replication-display-health-status.md) e [Interrupción del emparejamiento de replicación](cross-region-replication-manage-disaster-recovery.md#break-replication-peering-to-activate-the-destination-volume).  

1. Para eliminar la replicación de volúmenes, seleccione **Replicación** desde el volumen de origen o de destino.  

2. Haga clic en **Eliminar**.    

3. Para confirmar la eliminación, escriba **Sí** y haga clic en **Eliminar**.   

    ![Eliminación de la replicación](../media/azure-netapp-files/cross-region-replication-delete-replication.png)

## <a name="next-steps"></a>Pasos siguientes  

* [Replicación entre regiones](cross-region-replication-introduction.md)
* [Requisitos y consideraciones del uso de la replicación entre regiones](cross-region-replication-requirements-considerations.md)
* [Visualización del estado de mantenimiento de la relación de la replicación](cross-region-replication-display-health-status.md)
* [Solución de problemas de replicación entre regiones](troubleshoot-cross-region-replication.md)

