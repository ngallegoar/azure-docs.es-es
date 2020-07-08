---
title: Reimplementación de una máquina virtual en un laboratorio de Azure DevTest Labs | Microsoft Docs
description: Obtenga información sobre cómo volver a implementar una máquina virtual (mover de un nodo de Azure a otro) en Azure DevTest Labs.
ms.topic: article
ms.date: 06/26/2020
ms.openlocfilehash: da0edf13adaa0d7ecd84ee2c190f376c19b398db
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/02/2020
ms.locfileid: "85480242"
---
# <a name="redeploy-a-vm-in-a-lab-in-azure-devtest-labs"></a>Reimplementación de una máquina virtual en un laboratorio de Azure DevTest Labs
Si no puede conectarse a la máquina virtual (VM) de un laboratorio mediante una conexión a escritorio remoto, vuelva a implementar la máquina virtual e intente de nuevo la conexión. Cuando se vuelve a implementar una máquina virtual, DevTest Labs mueve la VM desde el nodo en el que se ejecuta a un nuevo nodo dentro de la infraestructura de Azure. Después, inicia la VM conservando todas las opciones de configuración y los recursos asociados. Gracias a esta característica, se evita perder tiempo en solucionar los problemas relacionados con la conexión al escritorio remoto o con el acceso de la aplicación a las VM basadas en Windows del laboratorio. 

## <a name="steps-to-redeploy-a-vm-in-a-lab"></a>Pasos para volver a implementar una máquina virtual en un laboratorio 
Para volver a implementar una máquina virtual en un laboratorio de Azure DevTest Labs, realice los pasos siguientes: 

1. Inicie sesión en [Azure Portal](https://portal.azure.com).
2. Seleccione **Todos los servicios** y, luego, **DevTest Labs** en la lista.
3. En la lista de laboratorios, seleccione el que incluye la máquina virtual que quiere volver a implementar.  
4. En el panel izquierdo, seleccione **Mis máquinas virtuales**. 
5. En la lista de máquinas virtuales, seleccione una máquina virtual.
6. En el menú de la izquierda de la página de su máquina virtual, haga clic en la opción **Reimplementar** de **OPERACIONES**.

    ![Volver a implementar](media/devtest-lab-redeploy-vm/redeploy.png)
7. Lea la información en la página y seleccione el botón **Reimplementar**. 9. Compruebe el estado de la operación de reimplementación en la ventana **Notificaciones**.

    ![Estado de la reimplementación](media/devtest-lab-redeploy-vm/redeploy-status.png)

## <a name="next-steps"></a>Pasos siguientes
Obtenga información sobre cómo cambiar el tamaño de una máquina virtual en Azure DevTest Labs en [Cambio del tamaño de una VM](devtest-lab-resize-vm.md).


