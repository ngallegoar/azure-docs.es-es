---
title: Habilitación de Azure Monitor para VM en Azure Portal
description: Aprenda a evaluar Azure Monitor para VM en una sola máquina virtual de Azure o en un conjunto de escalado de máquinas virtuales.
ms.subservice: ''
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 03/12/2020
ms.openlocfilehash: 45bc8f16a547d4a95820f9dcd02132844b3be83c
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "79480715"
---
# <a name="enable-azure-monitor-for-vms-in-the-azure-portal"></a>Habilitación de Azure Monitor para VM en Azure Portal

En este artículo se describe cómo habilitar Azure Monitor para VM en un pequeño número de máquinas virtuales (VM) de Azure mediante Azure Portal. El objetivo es supervisar las VM y detectar problemas de disponibilidad o rendimiento. 

Antes de comenzar, revise los [requisitos previos](vminsights-enable-overview.md) y asegúrese de que su suscripción y sus recursos los cumplen.  

## <a name="enable-monitoring-for-a-single-azure-vm"></a>Habilitación de la supervisión de una VM de Azure
Para habilitar la supervisión de la VM de Azure:

1. Inicie sesión en [Azure Portal](https://portal.azure.com).

1. Seleccione **Máquinas virtuales**.

1. En la lista, seleccione una máquina virtual.

1. En la página de la máquina virtual, en la sección **Supervisión**, seleccione **Insights** y luego **Habilitar**.

    ![Habilitar Azure Monitor para VM para una máquina virtual](media/vminsights-enable-single-vm/enable-vminsights-vm-portal.png)

1. En la página **Azure Monitor Insights Onboarding** (Incorporación a Insights de Azure Monitor), si tiene un área de trabajo de Log Analytics existente en la misma suscripción, selecciónela en la lista desplegable.  

    La lista preselecciona el área de trabajo y la ubicación predeterminadas en las que se implementa la VM en la suscripción. 

    >[!NOTE]
    >Para crear un área de trabajo de Log Analytics nueva para almacenar los datos de supervisión de la VM, vea [Creación de un área de trabajo de Log Analytics en Azure Portal](../../azure-monitor/learn/quick-create-workspace.md). El área de trabajo de Log Analytics debe pertenecer a una de las [regiones admitidas](vminsights-enable-overview.md#log-analytics).

6. Recibirá mensajes de estado a medida que se lleve a cabo la configuración.

    ![Habilitar el procesamiento de implementación de supervisión de Azure Monitor para VM](media/vminsights-enable-single-vm/onboard-vminsights-vm-portal-status.png)

## <a name="enable-monitoring-for-a-single-virtual-machine-scale-set"></a>Habilitación de la supervisión de un conjunto de escalado de máquinas virtuales único

Para habilitar la supervisión del conjunto de escalado de máquinas virtuales de Azure, haga lo siguiente:

1. Inicie sesión en [Azure Portal](https://portal.azure.com).

2. Seleccione **Conjuntos de escalado de máquinas virtuales**.

3. Seleccione un conjunto de escalado de máquinas virtuales de la lista.

4. En la página del conjunto de escalado de máquinas virtuales, en la sección **Supervisión**, seleccione **Insights** y luego **Habilitar**.

5. En la página **Insights**, si quiere usar un área de trabajo de Log Analytics existente, selecciónela en la lista desplegable.

    La lista preselecciona el área de trabajo y la ubicación predeterminadas en las que se implementa la VM en la suscripción. 

    ![Habilitación de Azure Monitor para VM para un conjunto de escalado de máquinas virtuales](media/vminsights-enable-single-vm/enable-vminsights-vmss-portal.png)

    >[!NOTE]
    >Para crear un área de trabajo de Log Analytics nueva para almacenar los datos de supervisión del conjunto de escalado de máquinas virtuales, vea [Creación de un área de trabajo de Log Analytics en Azure Portal](../learn/quick-create-workspace.md). El área de trabajo de Log Analytics debe pertenecer a una de las [regiones admitidas](vminsights-enable-overview.md#log-analytics).

6. Recibirá mensajes de estado a medida que se lleve a cabo la configuración.

    >[!NOTE]
    >Si usa un modelo de actualización manual para el conjunto de escalado, actualice las instancias para completar la configuración. Puede iniciar las actualizaciones desde la página **Instancias**, en la sección **Configuración**.
    
    ![Habilitar el procesamiento de implementación de supervisión de Azure Monitor para VM](media/vminsights-enable-single-vm/onboard-vminsights-vmss-portal-status.png)

Ahora que ha habilitado la supervisión para la VM o el conjunto de escalado de máquinas virtuales, la información de supervisión está disponible para su análisis en Azure Monitor para VM. 

## <a name="next-steps"></a>Pasos siguientes

* Para ver las dependencias de las aplicaciones detectadas, consulte [Use la característica de asignación de Azure Monitor para VM (versión preliminar) para conocer los componentes de una aplicación](vminsights-maps.md). 
* Para identificar los cuellos de botella, el uso general y el rendimiento de la máquina virtual, vea [Cómo representar el rendimiento en gráficos con Azure Monitor para VM (versión preliminar)](vminsights-performance.md).
