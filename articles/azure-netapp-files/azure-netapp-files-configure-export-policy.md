---
title: 'Configuración de la directiva de exportación para el volumen NFS: Azure NetApp Files'
description: Describe cómo configurar la directiva de exportación para controlar el acceso a un volumen NFS mediante Azure NetApp Files
services: azure-netapp-files
author: b-juche
ms.author: b-juche
ms.service: azure-netapp-files
ms.workload: storage
ms.topic: how-to
ms.date: 10/18/2019
ms.openlocfilehash: e59648ee76b6715029c690329cbf8f4f1eee7243
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/02/2020
ms.locfileid: "85483659"
---
# <a name="configure-export-policy-for-an-nfs-volume"></a>Configuración de la directiva de exportación para un volumen NFS

Si lo desea, puede configurar la directiva de exportación para controlar el acceso a un volumen de Azure NetApp Files. La directiva de exportación de Azure NetApp Files solo es compatible con volúmenes NFS.  Se admiten NFSv3 y NFSv4. 

## <a name="steps"></a>Pasos 

1.  Haga clic en **Exportar directiva** en el panel de navegación de Azure NetApp Files. 

2.  Especifique la información de los campos siguientes para crear una regla de directiva de exportación:   
    *  **Índice**   
        Especifique el número de índice para la regla.  
        Una directiva de exportación puede constar de hasta cinco reglas. Las reglas se evalúan según el orden en la lista de números de índice. Primero se evalúan las reglas con números de índice inferior. Por ejemplo, la regla con el número de índice 1 se evalúa antes que la que tenga el número de índice 2. 

    * **Clientes permitidos**   
        Especifique el valor en uno de los siguientes formatos:  
        * Dirección IPv4, por ejemplo, `10.1.12.24` 
        * Dirección IPv4 con una máscara de subred expresada como un número de bits, por ejemplo, `10.1.12.10/4`

    * **Acceder**  
        Seleccione uno de los tipos de accesos siguientes:  
        * Sin acceso 
        * Lectura y escritura
        * Solo lectura

    ![Directiva de exportación](../media/azure-netapp-files/azure-netapp-files-export-policy.png) 


## <a name="next-steps"></a>Pasos siguientes 
* [Administración de volúmenes](azure-netapp-files-manage-volumes.md)
* [Instalación o desinstalación de un volumen para máquinas virtuales](azure-netapp-files-mount-unmount-volumes-for-virtual-machines.md)
* [Administración de instantáneas](azure-netapp-files-manage-snapshots.md)
