---
title: Cambio dinámico del nivel de servicio de un volumen para Azure NetApp Files | Microsoft Docs
description: Describe cómo cambiar dinámicamente el nivel de servicio de un volumen.
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
ms.date: 07/24/2020
ms.author: b-juche
ms.openlocfilehash: bd28f949d35d38c9e64af7ff4196aa1754fbc37a
ms.sourcegitcommit: d7bd8f23ff51244636e31240dc7e689f138c31f0
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/24/2020
ms.locfileid: "87172546"
---
# <a name="dynamically-change-the-service-level-of-a-volume"></a>Cambio dinámico del nivel de servicio de un volumen

Puede cambiar el nivel de servicio de un volumen existente al moverlo a otro grupo de capacidad que use el [nivel de servicio](azure-netapp-files-service-levels.md) que quiere para dicho volumen. Este cambio de nivel de servicio local para el volumen no requiere que se migren los datos. Tampoco afecta al acceso al volumen.  

> [!IMPORTANT] 
> El uso de esta característica requiere el uso de una lista blanca. Envíe un correo electrónico a anffeedback@microsoft.com con su identificador de suscripción para solicitar esta característica.

Esta funcionalidad le permite satisfacer sus necesidades de carga de trabajo a petición.  Puede cambiar un volumen existente para que use un nivel de servicio superior para mejorar el rendimiento, o para que use un nivel de servicio inferior para la optimización de costos. Por ejemplo, si actualmente el volumen está en un grupo de capacidad que usa el nivel de servicio *Estándar* y quiere que el volumen use el nivel de servicio *Premium*, puede trasladar el volumen de forma dinámica a un grupo de capacidad que use el nivel de servicio *Premium*.  

El grupo de capacidad al que quiera trasladar el volumen ya debe existir. Además, el grupo puede contener otros volúmenes.  Si quiere trasladar el volumen a un grupo de capacidad nuevo, debe [crear el grupo de capacidad](azure-netapp-files-set-up-capacity-pool.md) antes de la migración.  

## <a name="considerations"></a>Consideraciones

* Después de que el volumen se haya movido a otro grupo de capacidad, ya no tendrá acceso a los registros de actividad del volumen anteriores, ni a las métricas del volumen. El volumen se iniciará con nuevos registros de actividad y métricas en el grupo de capacidad nuevo.

* Si mueve un volumen a un grupo de capacidad de un nivel de servicio superior (por ejemplo, al pasar del nivel de servicio *Estándar* a *Premium* o *Ultra*), deberá esperar al menos siete días para mover el volumen a un grupo de capacidad de un nivel de servicio inferior (por ejemplo, para pasar de *Ultra* a *Premium* o *Estándar*).  
Este período de espera no se aplica si mueve el volumen a un grupo de capacidad que tenga el mismo nivel de servicio o un nivel de servicio inferior.

## <a name="steps"></a>Pasos

1.  En la página Volúmenes, haga clic con el botón secundario en el volumen cuyo nivel de servicio quiere cambiar. Seleccione **Cambiar grupo**.

    ![Clic con el botón derecho en el volumen](../media/azure-netapp-files/right-click-volume.png)

2. En la ventana Cambiar grupo, seleccione el grupo de capacidad al que quiere trasladar el volumen. 

    ![Cambiar grupo](../media/azure-netapp-files/change-pool.png)

3.  Haga clic en **OK**.


## <a name="next-steps"></a>Pasos siguientes  

* [Niveles de servicio para Azure NetApp Files](azure-netapp-files-service-levels.md)
* [Configuración de un grupo de capacidad](azure-netapp-files-set-up-capacity-pool.md)
