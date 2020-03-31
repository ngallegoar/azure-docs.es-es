---
title: Administración de instantáneas mediante Azure NetApp Files | Microsoft Docs
description: Se describe cómo crear instantáneas para un volumen o realizar una restauración a partir de una instantánea en un nuevo volumen mediante Azure NetApp Files.
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
ms.date: 03/03/2020
ms.author: b-juche
ms.openlocfilehash: 48055a774808aea86452e8410b7e717f5019d172
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "78267900"
---
# <a name="manage-snapshots-by-using-azure-netapp-files"></a>Administración de instantáneas mediante Azure NetApp Files

Puede usar Azure NetApp Files para crear manualmente una instantánea a petición para un volumen o realizar una restauración de una instantánea a un nuevo volumen. El servicio Azure NetApp Files no crea automáticamente instantáneas de volumen.  

## <a name="create-an-on-demand-snapshot-for-a-volume"></a>Creación de una instantánea a petición para un volumen

Solo puede crear instantáneas a petición. Las directivas de instantánea no se admiten actualmente.

1.  En la hoja Volume (Volumen), haga clic en **Snapshots** (Instantáneas).

    ![Desplazamiento hasta las instantáneas](../media/azure-netapp-files/azure-netapp-files-navigate-to-snapshots.png)

2.  Haga clic en **+ Add snapshot** (+Agregar instantánea) para crear una instantánea a petición para un volumen.

    ![Agregar instantánea](../media/azure-netapp-files/azure-netapp-files-add-snapshot.png)

3.  En la ventana Nueva instantánea, proporcione un nombre para la nueva instantánea que va a crear.   

    ![Nueva instantánea](../media/azure-netapp-files/azure-netapp-files-new-snapshot.png)

4. Haga clic en **OK**. 

## <a name="restore-a-snapshot-to-a-new-volume"></a>Restauración de una instantánea a un nuevo volumen

Actualmente, puede restaurar una instantánea solo a un nuevo volumen. 
1. Vaya a la hoja **Administración de instantáneas** desde la hoja Volumen para mostrar la lista de instantáneas. 
2. Seleccione una instantánea para restaurarla.  
3. Haga clic con el botón derecho en el nombre de la instantánea y seleccione **Restaurar al nuevo volumen** en la opción de menú.  

    ![Restauración de la instantánea al nuevo volumen](../media/azure-netapp-files/azure-netapp-files-snapshot-restore-to-new-volume.png)

4. En la ventana Nuevo volumen, proporcione información para el nuevo volumen:  
    * **Nombre**   
        Especifique el nombre para el volumen que va a crear.  
        
        El nombre debe ser único dentro de un grupo de recursos. Debe tener tres caracteres de longitud, como mínimo.  Puede usar cualquier carácter alfanumérico.

    * **Ruta de acceso del archivo**     
        Especifique la ruta de acceso de archivo que se usará para crear la ruta de acceso de exportación para el nuevo volumen. La ruta de acceso de exportación se usa para montar el volumen y tener acceso a él.   
        
        Un destino de montaje es el punto de conexión de la dirección IP del servicio NFS. Se genera automáticamente.   
        
        El nombre de la ruta de acceso de archivo solo puede contener letras, números y guiones ("-"). El nombre debe tener entre 16 y 40 caracteres. 

    * **Cuota**  
        Especifique la cantidad de almacenamiento lógico que se asigna al volumen.  

        El campo **Cuota disponible** muestra la cantidad de espacio no utilizado en el grupo de capacidad elegido que puede usar para crear un nuevo volumen. El tamaño del volumen nuevo no debe superar la cuota disponible.

    *   **Red virtual**  
        Especifique la red virtual de Azure (Vnet) desde la que desea tener acceso al volumen.  
        La red virtual que especifique debe tener una subred delegada en Azure NetApp Files. Solo puede acceder a Azure NetApp Files desde la misma red virtual o desde una red virtual que se encuentre en la misma ubicación que el volumen mediante el emparejamiento de VNET. También puede acceder al volumen desde la red local mediante ExpressRoute. 

    * **Subred**  
        Especifique la subred que desea usar para el volumen.  
        La red virtual que especifique debe estar delegada en el servicio Azure NetApp Files. Puede crear una nueva subred seleccionando **Crear nuevo** en el campo de la subred.  
   <!--
    ![Restored new volume](../media/azure-netapp-files/azure-netapp-files-snapshot-new-volume.png) 
   -->

5. Haga clic en **OK**.   
    El nuevo volumen al que se restaura la instantánea aparece en la hoja Volúmenes.

## <a name="next-steps"></a>Pasos siguientes

[Información sobre la jerarquía del almacenamiento de Azure NetApp Files](azure-netapp-files-understand-storage-hierarchy.md)
