---
title: Deshabilitación de la supervisión en Azure Monitor para VM
description: En este artículo se describe cómo puede interrumpir la supervisión de las máquinas virtuales en Azure Monitor para VM.
ms.subservice: ''
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 03/12/2020
ms.openlocfilehash: 80473aa494b8fbcea5e43870b7717cd3472dd7d1
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "79480528"
---
# <a name="disable-monitoring-of-your-vms-in-azure-monitor-for-vms"></a>Deshabilitación de la supervisión de las máquinas virtuales en Azure Monitor para VM

Después de habilitar la supervisión de las máquinas virtuales (VM), más adelante puede elegir deshabilitar la supervisión en Azure Monitor para VM. En este artículo se muestra cómo deshabilitar la supervisión de una o más VM.  

Actualmente, Azure Monitor para VM no permite deshabilitar de forma selectiva la supervisión de VM. Puede que el área de trabajo de Log Analytics admita Azure Monitor para VM y otras soluciones. También puede recopilar otros datos de supervisión. Si el área de trabajo de Log Analytics proporciona estos servicios, debe entender el efecto y métodos de la deshabilitación de la supervisión antes de empezar.

Azure Monitor para VM se basa en los siguientes componentes para ofrecer su experiencia:

* Un área de trabajo de Log Analytics, que almacena los datos de supervisión de las máquinas virtuales y otros orígenes.
* Una colección de contadores de rendimiento configurados en el área de trabajo. La colección actualiza la configuración de supervisión en todas las VM conectadas al área de trabajo.
* `VMInsights`, que es una solución de supervisión configurada en el área de trabajo. Esta solución actualiza la configuración de supervisión en todas las máquinas virtuales conectadas al área de trabajo.
* `MicrosoftMonitoringAgent` y `DependencyAgent`, que son extensiones de Azure VM. Estas extensiones recopilan y envían datos al área de trabajo.

Cuando se prepare para deshabilitar la supervisión de las VM, tenga en cuenta estas consideraciones:

* Si realizó la evaluación con una sola VM y usó el área de trabajo de Log Analytics predeterminada preseleccionada, puede deshabilitar la supervisión mediante la desinstalación del agente de dependencia de la máquina virtual y la desconexión del agente de Log Analytics de esta área de trabajo. Este enfoque es adecuado si tiene la intención de utilizar la máquina virtual para otros fines y decide posteriormente volver a conectarla a un área de trabajo diferente.
* Si seleccionó un área de trabajo de Log Analytics existente que admite otras soluciones de supervisión y recopilación de datos de otros orígenes, puede quitar los componentes de la solución del área de trabajo sin que el área de trabajo se vea afectada.  

>[!NOTE]
> Después de quitar los componentes de la solución del área de trabajo, puede seguir viendo los datos de rendimiento y de mapa para las máquinas virtuales de Azure. Los datos, finalmente, dejarán de aparecer en las vistas **Rendimiento** y **Mapa**. La opción **Habilitar** estará disponible en la máquina virtual de Azure seleccionada para que pueda volver a habilitar la supervisión en el futuro.  

## <a name="remove-azure-monitor-for-vms-completely"></a>Quitar completamente Azure Monitor para VM

Si aún necesita el área de trabajo de Log Analytics, siga estos pasos para quitar por completo Azure Monitor para VM. Va a quitar la solución `VMInsights` del área de trabajo.  

1. Inicie sesión en [Azure Portal](https://portal.azure.com).
2. En Azure Portal, seleccione **Todos los servicios**. En la lista de recursos, escriba **Log Analytics**. Cuando comience a escribir, la lista filtrará las sugerencias en función de la entrada. Seleccione **Log Analytics**.
3. En la lista de áreas de trabajo de Log Analytics, seleccione el área de trabajo que eligió al incorporarse a Azure Monitor para VM.
4. A la izquierda, seleccione **Soluciones**.  
5. En la lista de soluciones, seleccione **VMInsights(nombre de área de trabajo)** . En la página **Introducción** de la solución, seleccione **Eliminar**. Cuando se le solicite confirmación, haga clic en **Sí**.

## <a name="disable-monitoring-and-keep-the-workspace"></a>Deshabilitar la supervisión y conservar el área de trabajo  

Si el área de trabajo de Log Analytics aún debe admitir la supervisión de otros orígenes, siga estos pasos para deshabilitar la supervisión en la VM que usó para evaluar Azure Monitor para VM. Para las VM de Azure, quitará la extensión de VM del agente de dependencias y la extensión de VM del agente de Log Analytics para Windows o Linux directamente de la VM. 

>[!NOTE]
>No quite el agente de Log Analytics si: 
>
> * Azure Automation administra la VM para orquestar los procesos o para administrar la configuración o las actualizaciones. 
> * Azure Security Center administra la VM para la seguridad y la detección de amenazas. 
>
> Si quita el agente de Log Analytics, impedirá que esos servicios y soluciones administren de forma proactiva la VM. 

1. Inicie sesión en [Azure Portal](https://portal.azure.com). 
2. En Azure Portal, seleccione **Virtual Machines**. 
3. En la lista, seleccione una máquina virtual. 
4. En el panel izquierdo, seleccione **Extensiones**. En la página **Extensiones**, seleccione **DependencyAgent**.
5. En la página de propiedades de la extensión, seleccione **Desinstalar**.
6. En la página **Extensiones**, seleccione **MicrosoftMonitoringAgent**. En la página de propiedades de la extensión, seleccione **Desinstalar**.  
