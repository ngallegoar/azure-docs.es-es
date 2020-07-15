---
title: Habilitación de Azure Monitor para VM en Azure Portal
description: Aprenda a evaluar Azure Monitor para VM en una sola máquina virtual de Azure o en un conjunto de escalado de máquinas virtuales.
ms.subservice: ''
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 06/25/2020
ms.openlocfilehash: 4cdb9390b3146df74f2cbe8eba7b170a5d11fb2c
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/02/2020
ms.locfileid: "85507065"
---
# <a name="enable-azure-monitor-for-single-vm-or-vmss-in-the-azure-portal"></a>Habilitación de Azure Monitor para una sola máquina virtual o un conjunto de escalado de máquinas virtuales en Azure Portal
En este artículo se describe cómo habilitar Azure Monitor para VM para una sola máquina virtual o un conjunto de escalado de máquinas virtuales mediante Azure Portal. Este procedimiento se puede usar para:

- Máquina virtual de Azure
- Conjunto de escalado de máquinas virtuales de Azure
- Máquina de Azure Arc

Antes de comenzar, revise los [requisitos previos](vminsights-enable-overview.md) y asegúrese de que su suscripción y sus recursos los cumplen.  

## <a name="enable-azure-monitor-for-vms"></a>Habilitar Azure Monitor para VM

1. Inicie sesión en [Azure Portal](https://portal.azure.com).

1. Seleccione **Máquinas virtuales**, **Conjuntos de escalado de máquinas virtuales** o **Máquinas: Azure Arc**.

1. Seleccione un recurso de la lista.

1. En la sección **Supervisión** del menú, seleccione **Insights** y, a continuación, **Habilitar**. En el ejemplo siguiente se muestra una máquina virtual de Azure, pero el menú es similar para Azure VMSS o Azure Arc.

    ![Habilitar Azure Monitor para VM para una máquina virtual](media/vminsights-enable-single-vm/enable-vminsights-vm-portal.png)

1. Si la máquina virtual aún no está conectada a un área de trabajo Log Analytics, se le pedirá que seleccione una. Si anteriormente no ha [creado un área de trabajo](../../azure-monitor/learn/quick-create-workspace.md), puede seleccionar una predeterminada para la ubicación en la que se implementa la máquina virtual o el conjunto de escalado de máquinas virtuales en la suscripción. Se creará y configurará el área de trabajo si aún no existe.

2. Recibirá mensajes de estado a medida que se lleve a cabo la configuración.

    >[!NOTE]
    >Si usa un modelo de actualización manual para el conjunto de escalado, actualice las instancias para completar la configuración. Puede iniciar las actualizaciones desde la página **Instancias**, en la sección **Configuración**.

    ![Habilitar el procesamiento de implementación de supervisión de Azure Monitor para VM](media/vminsights-enable-single-vm/onboard-vminsights-vm-portal-status.png)



## <a name="next-steps"></a>Pasos siguientes

* Para ver las dependencias de las aplicaciones detectadas, consulte [Use la característica de asignación de Azure Monitor para VM (versión preliminar) para conocer los componentes de una aplicación](vminsights-maps.md). 
* Para identificar los cuellos de botella, el uso general y el rendimiento de la máquina virtual, vea [Cómo representar el rendimiento en gráficos con Azure Monitor para VM (versión preliminar)](vminsights-performance.md).
