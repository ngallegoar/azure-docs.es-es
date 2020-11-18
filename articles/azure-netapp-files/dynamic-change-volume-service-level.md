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
ms.date: 11/06/2020
ms.author: b-juche
ms.openlocfilehash: fe4b2925a34ae7c06bb0b597f0bcdcc3f4d80896
ms.sourcegitcommit: 22da82c32accf97a82919bf50b9901668dc55c97
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/08/2020
ms.locfileid: "94363228"
---
# <a name="dynamically-change-the-service-level-of-a-volume"></a>Cambio dinámico del nivel de servicio de un volumen

> [!IMPORTANT] 
> El registro de la versión preliminar pública para esta característica está en espera hasta nuevo aviso. 

Puede cambiar el nivel de servicio de un volumen existente al moverlo a otro grupo de capacidad que use el [nivel de servicio](azure-netapp-files-service-levels.md) que quiere para dicho volumen. Este cambio de nivel de servicio local para el volumen no requiere que se migren los datos. Tampoco afecta al acceso al volumen.  

Esta funcionalidad le permite satisfacer sus necesidades de carga de trabajo a petición.  Puede cambiar un volumen existente para que use un nivel de servicio superior para mejorar el rendimiento, o para que use un nivel de servicio inferior para la optimización de costos. Por ejemplo, si actualmente el volumen está en un grupo de capacidad que usa el nivel de servicio *Estándar* y quiere que el volumen use el nivel de servicio *Premium*, puede trasladar el volumen de forma dinámica a un grupo de capacidad que use el nivel de servicio *Premium*.  

El grupo de capacidad al que quiera trasladar el volumen ya debe existir. Además, el grupo puede contener otros volúmenes.  Si quiere trasladar el volumen a un grupo de capacidad nuevo, debe [crear el grupo de capacidad](azure-netapp-files-set-up-capacity-pool.md) antes de la migración.  

## <a name="considerations"></a>Consideraciones

* Después de que el volumen se haya movido a otro grupo de capacidad, ya no tendrá acceso a los registros de actividad del volumen anteriores, ni a las métricas del volumen. El volumen se iniciará con nuevos registros de actividad y métricas en el grupo de capacidad nuevo.

* Si mueve un volumen a un grupo de capacidad de un nivel de servicio superior (por ejemplo, al pasar del nivel de servicio *Estándar* a *Premium* o *Ultra*), deberá esperar al menos siete días para mover ese volumen *de nuevo* a un grupo de capacidad de un nivel de servicio inferior (por ejemplo, para pasar de *Ultra* a *Premium* o *Standard*).  
<!-- 
## Register the feature

The feature to move a volume to another capacity pool is currently in preview. If you are using this feature for the first time, you need to register the feature first.

1. Register the feature: 

    ```azurepowershell-interactive
    Register-AzProviderFeature -ProviderNamespace Microsoft.NetApp -FeatureName ANFTierChange
    ```

2. Check the status of the feature registration: 

    > [!NOTE]
    > The **RegistrationState** may be in the `Registering` state for up to 60 minutes before changing to`Registered`. Wait until the status is **Registered** before continuing.

    ```azurepowershell-interactive
    Get-AzProviderFeature -ProviderNamespace Microsoft.NetApp -FeatureName ANFTierChange
    ```
You can also use [Azure CLI commands](/cli/azure/feature?preserve-view=true&view=azure-cli-latest) `az feature register` and `az feature show` to register the feature and display the registration status. 
--> 
## <a name="move-a-volume-to-another-capacity-pool"></a>Traslado de un volumen a otro grupo de capacidad

1.  En la página Volúmenes, haga clic con el botón secundario en el volumen cuyo nivel de servicio quiere cambiar. Seleccione **Cambiar grupo**.

    ![Clic con el botón derecho en el volumen](../media/azure-netapp-files/right-click-volume.png)

2. En la ventana Cambiar grupo, seleccione el grupo de capacidad al que quiere trasladar el volumen. 

    ![Cambiar grupo](../media/azure-netapp-files/change-pool.png)

3.  Haga clic en **OK**.


## <a name="next-steps"></a>Pasos siguientes  

* [Niveles de servicio para Azure NetApp Files](azure-netapp-files-service-levels.md)
* [Configuración de un grupo de capacidad](azure-netapp-files-set-up-capacity-pool.md)
