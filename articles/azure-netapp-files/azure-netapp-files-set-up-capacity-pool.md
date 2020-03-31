---
title: Configuración de un grupo de capacidad de Azure NetApp Files | Microsoft Docs
description: Describe cómo configurar un grupo de capacidad para poder crear volúmenes en él.
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
ms.date: 02/15/2019
ms.author: b-juche
ms.openlocfilehash: 8f50b2ad34c705c8d3831d8243f136c41d750dc0
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "60691111"
---
# <a name="set-up-a-capacity-pool"></a>Configuración de un grupo de capacidad

Configurar un grupo de capacidad le permite crear volúmenes en él.  

## <a name="before-you-begin"></a>Antes de empezar 

Debe haber creado ya una cuenta de NetApp.   

[Creación de una cuenta de NetApp](azure-netapp-files-create-netapp-account.md)

## <a name="steps"></a>Pasos 

1. Vaya a la hoja de administración de su cuenta de NetApp y, luego, en el panel de navegación, seleccione **Capacity pools** (Grupos de capacidad).  
    
    ![Ir al grupo de capacidad](../media/azure-netapp-files/azure-netapp-files-navigate-to-capacity-pool.png)

2. Haga clic en **+ Agregar grupos** para crear un nuevo grupo de capacidad.   
    Aparecerá el cuadro de diálogo Nuevo grupo de capacidad.

3. Proporcione la siguiente información para el nuevo grupo de capacidad:  
   * **Nombre**  
     Especifique el nombre para el grupo de capacidad.  
     El nombre del grupo de capacidad debe ser único para cada cuenta de NetApp.

   * **Nivel de servicio**   
     Este campo muestra el rendimiento de destino para el grupo de capacidad.  
     Especifique el nivel de servicio del grupo de capacidad: [**Premium**](azure-netapp-files-service-levels.md#Premium) o [**Standard**](azure-netapp-files-service-levels.md#Standard).

   * **Tamaño**     
     Especifique el tamaño del grupo de capacidad que va a adquirir.        
     El tamaño mínimo del grupo de capacidad es de 4 TiB. Puede crear un grupo con un tamaño que sea múltiplo de 4 TiB.   
      
     ![Nuevo grupo de capacidad](../media/azure-netapp-files/azure-netapp-files-new-capacity-pool.png)

4. Haga clic en **OK**.

## <a name="next-steps"></a>Pasos siguientes 

- [Niveles de servicio para Azure NetApp Files](azure-netapp-files-service-levels.md)
- Consulte la [página de precios de Azure NetApp Files](https://azure.microsoft.com/pricing/details/storage/netapp/) para conocer el precio de los diferentes niveles de servicio
- [Delegación de una subred en Azure NetApp Files](azure-netapp-files-delegate-subnet.md)
