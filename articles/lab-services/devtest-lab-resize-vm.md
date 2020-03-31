---
title: Cambio de tamaño de una máquina virtual en un laboratorio de Azure DevTest Labs
description: Obtenga información sobre cómo cambiar el tamaño de una máquina virtual (VM) en Azure DevTest Labs en función de sus necesidades de CPU, red o rendimiento de disco.
services: devtest-lab,virtual-machines,lab-services
documentationcenter: na
author: spelluru
manager: femila
editor: ''
ms.assetid: 8460f09e-482f-48ba-a57a-c95fe8afa001
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/24/2020
ms.author: spelluru
ms.openlocfilehash: bf7c425766a97aaa1d143133f04502a0aa3c36cb
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "76756184"
---
# <a name="resize-a-vm-in-a-lab-in-azure-devtest-labs"></a>Cambio de tamaño de una máquina virtual en un laboratorio de Azure DevTest Labs
Una de las características importantes de Azure Virtual Machines es que permite cambiar el tamaño de una máquina virtual (VM) en función de sus necesidades de CPU, red o rendimiento de disco. Ahora, Azure DevTest Labs admite esta característica para las máquinas virtuales en un laboratorio. La característica de cambio de tamaño se ajusta a la directiva de laboratorio para los tamaños de máquina virtual permitidos en el laboratorio. Es decir, se puede cambiar el tamaño de una máquina virtual, pero solo a los tamaños permitidos en el laboratorio. 


## <a name="steps-to-resize-a-vm-in-a-lab"></a>Pasos para cambiar el tamaño de una máquina virtual en un laboratorio 
Para cambiar el tamaño de una máquina virtual en un laboratorio de Azure DevTest Labs, realice los pasos siguientes: 

> [!NOTE]
> Si está conectado a la máquina virtual a través de una sesión de escritorio remoto (RDP), guarde el trabajo y desconéctese de la máquina virtual antes de cambiar su tamaño.

1. Inicie sesión en [Azure Portal](https://portal.azure.com).
2. Seleccione **Todos los servicios** y, luego, **DevTest Labs** en la lista.
3. En la lista de laboratorios, seleccione el que incluye la máquina virtual que quiere cambiar de tamaño.  
4. En el panel izquierdo, seleccione **Mis máquinas virtuales**. 
5. En la lista de máquinas virtuales, seleccione una máquina virtual.
6. Haga clic en **Detener** en la barra de herramientas si se está ejecutando la máquina virtual. Compruebe el estado de la operación en la ventana **Notificaciones**. Espere hasta que la máquina virtual está detenida y, después, cierre la ventana **Notificaciones**. 

    ![Parada de la máquina virtual](media/devtest-lab-resize-vm/stop-vm.png)
1. En el menú de la izquierda de la página de su máquina virtual, haga clic en la opción **Tamaño** de **CONFIGURACIÓN**.

    ![Menú Tamaño](media/devtest-lab-resize-vm/size-menu.png)
1. En la ventana **Elija un tamaño**, examine y seleccione un tamaño para la máquina virtual, y haga clic en **Seleccionar**.     
1. Compruebe el estado de la operación de cambio de tamaño en la ventana **Notificaciones**.

    ![Estado de cambio de tamaño](media/devtest-lab-resize-vm/resize-status.png)
10. Una vez completada con éxito la operación de cambio de tamaño, cierre la ventana **Notificaciones**. 
11. Seleccione **Información general** en el menú de la izquierda y después, en la barra de herramientas, seleccione **Reiniciar** para reiniciar la máquina virtual. 

## <a name="next-steps"></a>Pasos siguientes
Para obtener información detallada acerca de la característica de cambio de tamaño compatible con máquinas virtuales de Azure, consulte [Resize virtual machines](https://azure.microsoft.com/blog/resize-virtual-machines/) (Cambio de tamaño de las máquinas virtuales).


