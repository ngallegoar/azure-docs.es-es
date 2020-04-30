---
title: Obtenga información sobre la incorporación de las soluciones Update Management, Change Tracking e Inventory en Azure Automation
description: Obtenga información acerca de cómo incorporar una máquina virtual de Azure con las soluciones Update Management, Change Tracking e Inventory que forman parte de Azure Automation
services: automation
ms.date: 4/11/2019
ms.topic: conceptual
ms.custom: mvc
ms.openlocfilehash: 57378005bd668fa9c0f2aea70c411bbf911130db
ms.sourcegitcommit: b55d7c87dc645d8e5eb1e8f05f5afa38d7574846
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/16/2020
ms.locfileid: "81457661"
---
# <a name="onboard-update-management-change-tracking-and-inventory-solutions"></a>Incorporación de las soluciones Update Management, Change Tracking e Inventory

Azure Automation proporciona soluciones para administrar las actualizaciones de seguridad del sistema operativo, el seguimiento de cambios y el inventario de los componentes instalados en los equipos. Hay varias maneras de incorporar máquinas; puede incorporar la solución [desde una máquina virtual](automation-onboard-solutions-from-vm.md), [explorando varias máquinas](automation-onboard-solutions-from-browse.md), desde su cuenta de Automation o mediante un [runbook](automation-onboard-solutions.md). Este artículo trata la incorporación de estas soluciones desde la cuenta de Automation.

## <a name="sign-in-to-azure"></a>Inicio de sesión en Azure

Inicie sesión en Azure en https://portal.azure.com.

## <a name="enable-solutions"></a>Habilitar soluciones

Vaya a la cuenta de Automation y seleccione **Inventory** o **Change Tracking** en **Administración de configuración**.

Para habilitar la solución, elija el área de trabajo de Log Analytics y la cuenta de Automation y haga clic en **Habilitar**. La solución tarda hasta 15 minutos en habilitarse.

![Incorporación de la solución Inventory](media/automation-onboard-solutions-from-automation-account/onboardsolutions.png)

> [!NOTE]
> Al habilitar las soluciones, solo en determinadas regiones se puede vincular un área de trabajo de Log Analytics y una cuenta de Automation.
>
> Para obtener una lista de los pares de asignación que se admiten, consulte [Asignación de región para la cuenta de Automation y el área de trabajo de Log Analytics](how-to/region-mappings.md).

La solución Change Tracking e Inventario permite realizar un [control de cambios](automation-vm-change-tracking.md) e [inventario](automation-vm-inventory.md) en sus máquinas virtuales. En este paso, se habilita la solución en una máquina virtual.

Cuando se complete la notificación de que se han incorporado las soluciones Change Tracking e Inventory, seleccione **Update Management** en **Administración de configuración**.

La solución Update Management permite administrar las actualizaciones y las revisiones para las máquinas virtuales de Azure e híbridas. Puede evaluar rápidamente el estado de las actualizaciones disponibles, programar la instalación de las actualizaciones necesarias y revisar los resultados de la implementación para comprobar si las actualizaciones se aplicaron correctamente en dichas máquinas.

En la página Habilitar solución, el área de trabajo de Log Analytics seleccionada es la misma área de trabajo que se usa en el paso anterior. Haga clic en **Habilitar** para incorporar la solución Update Management. La solución tarda hasta 15 minutos en habilitarse.

![Incorporación de la solución de actualización](media/automation-onboard-solutions-from-automation-account/onboardsolutions2.png)

## <a name="scope-configuration"></a>Configuración de ámbito

Cada solución utiliza una configuración de ámbito en el área de trabajo para definir los equipos de destino que obtendrán la solución. La configuración de ámbito es un grupo de una o más búsquedas guardadas que se utiliza para limitar el ámbito de la solución a equipos específicos. Para acceder a las Configuraciones de ámbito, en la cuenta de Automation, debajo de **Recursos relacionados**, seleccione **Área de trabajo**. A continuación, en el área de trabajo, en **Orígenes de datos del área de trabajo**, seleccione **Configuraciones de ámbito**.

Si el área de trabajo seleccionada no dispone aún de las soluciones Update Management ni Change Tracking, se crearán las siguientes configuraciones de ámbito:

* **MicrosoftDefaultScopeConfig-ChangeTracking**

* **MicrosoftDefaultScopeConfig-Updates**

Si el área de trabajo seleccionada ya tiene la solución, esta no se implementará de nuevo ni se agregará la configuración de ámbito.

## <a name="saved-searches"></a>Búsquedas guardadas

Cuando un equipo se agrega a las soluciones Update Management o Change Tracking e Inventario, se agrega a una de las dos búsquedas guardadas en el área de trabajo. Estas búsquedas guardadas son consultas que contienen los equipos de destino de estas soluciones.

Vaya al área de trabajo de Log Analytics y seleccione **Búsquedas guardadas** en **General**. Las dos búsquedas guardadas utilizadas por estas soluciones se pueden ver en la tabla siguiente:

|Nombre     |Category  |Alias  |
|---------|---------|---------|
|MicrosoftDefaultComputerGroup     |  ChangeTracking       | ChangeTracking__MicrosoftDefaultComputerGroup        |
|MicrosoftDefaultComputerGroup     | Actualizaciones        | Updates__MicrosoftDefaultComputerGroup         |

Seleccione cualquier búsqueda guardada para ver la consulta utilizada para rellenar el grupo. La siguiente imagen muestra la consulta y sus resultados:

![Búsquedas guardadas](media/automation-onboard-solutions-from-automation-account/savedsearch.png)

## <a name="onboard-azure-vms"></a>Incorporar máquinas virtuales de Azure

Desde la cuenta de Automation, seleccione **Inventory** o **Change Tracking** en **Administración de configuración**, o bien **Update Management** debajo de **Update Management**.

Haga clic en **+ Agregar máquinas virtuales de Azure** y seleccione una o varias máquinas virtuales en la lista. Las máquinas virtuales que no se pueden habilitar se atenúan y no se pueden seleccionar. Las VM de Azure pueden existir en cualquier región independientemente de la ubicación de la cuenta de Automation. En la página **Habilitar la administración de actualizaciones**, haga clic en **Habilitar**. Esta operación agrega las máquinas virtuales seleccionadas a la búsqueda guardada de los grupos de equipos de la solución.

![Habilitar máquinas virtuales de Azure](media/automation-onboard-solutions-from-automation-account/enable-azure-vms.png)

## <a name="onboard-a-non-azure-machine"></a>Incorporación de una máquina que no es de Azure

Las máquinas que no estén en Azure se deben agregar manualmente. Desde la cuenta de Automation, seleccione **Inventory** o **Change Tracking** en **Administración de configuración**, o bien **Update Management** debajo de **Update Management**.

Haga clic en **Agregar un equipo que no es de Azure**. Esta acción abre una nueva ventana del explorador con las [instrucciones para instalar y configurar el agente de Log Analytics para Windows](../azure-monitor/platform/log-analytics-agent.md) para que la máquina pueda empezar a enviar notificaciones a la solución. Si va a incorporar una máquina administrada actualmente por System Center Operations Manager, no se necesita un nuevo agente y se proporciona la información del área de trabajo al agente existente.

## <a name="onboard-machines-in-the-workspace"></a>Incorporación de máquinas en el área de trabajo

Las máquinas instaladas manualmente o las máquinas que ya envían notificaciones al área de trabajo deben agregarse a Azure Automation para poder habilitar la solución. Desde la cuenta de Automation, seleccione **Inventory** o **Change Tracking** en **Administración de configuración**, o bien **Update Management** debajo de **Update Management**.

Seleccione **Administrar máquinas**. Esta operación abrirá la página **Administrar máquinas**. Esta página permite habilitar la solución en un conjunto seleccionado de máquinas, en todas los máquinas disponibles o habilitar la solución para todas las máquinas actuales y habilitarlo en todas las máquinas futuras. Es posible que, si previamente seleccionó la opción **Habilitar en todas las máquinas disponibles y futuras**, el botón **Administrar máquinas** esté atenuado.

![Búsquedas guardadas](media/automation-onboard-solutions-from-automation-account/managemachines.png)

### <a name="all-available-machines"></a>Todas las máquinas disponibles

Para habilitar la solución para todas las máquinas disponibles, seleccione **Habilitar en todas las máquinas disponibles**. Esta operación deshabilita el control que permite agregar máquinas una a una. Esta tarea agrega todos los nombres de las máquinas que informan al área de trabajo en la consulta de la búsqueda guardada de grupos de equipos. Cuando se selecciona, esta acción deshabilita el botón **Administrar máquinas**.

### <a name="all-available-and-future-machines"></a>Todas las máquinas disponibles y futuras

Para habilitar la solución en todas las máquinas disponibles actualmente y en el futuro, seleccione **Habilitar en todas las máquinas disponibles y futuras**. Esta opción elimina las configuraciones de ámbito y las búsquedas guardadas del área de trabajo. De este modo, se abrirá la solución para todas las máquinas de Azure y las que no son de Azure que envíen notificaciones al área de trabajo. Cuando está seleccionada, esta acción deshabilita permanentemente el botón **Administrar máquinas**, ya que no queda ninguna configuración de ámbito.

Puede volver a agregar las configuraciones de ámbito si vuelve a agregar las búsquedas guardadas iniciales. Para más información, consulte [Búsquedas guardadas](#saved-searches).

### <a name="selected-machines"></a>Máquinas seleccionadas

Para habilitar la solución para una o varias máquinas, seleccione **Habilitar en las máquinas seleccionadas** y haga clic en **Agregar** junto a cada máquina que se va a agregar a la solución. Esta tarea agrega los nombres de máquina seleccionados a la consulta de la búsqueda guardada de grupos de equipos de la solución.

## <a name="unlink-workspace"></a>Unlink workspace (Desvincular área de trabajo)

Las siguientes soluciones dependen de un área de trabajo de Log Analytics:

* [Administración de actualizaciones](automation-update-management.md)
* [Seguimiento de cambios](automation-change-tracking.md)
* [Inicio y detención de máquinas virtuales durante las horas de trabajo](automation-solution-vm-management.md)

Si decide que ya no quiere integrar su cuenta de Automation con un área de trabajo de Log Analytics, puede desvincular la cuenta directamente desde Azure Portal.  Para poder continuar, primero deberá quitar las soluciones mencionadas anteriormente; en caso contrario, este proceso no podrá avanzar. Consulte en el artículo de la solución concreta que ha importado los pasos necesarios para quitar dicha solución.

Después de quitar estas soluciones, puede seguir los pasos que se indican a continuación para desvincular la cuenta de Automation.

> [!NOTE]
> Algunas soluciones que incluyen versiones anteriores de la solución de supervisión de SQL Azure pueden haber creado recursos de automatización y también puede que tengan que quitarse antes de desvincularse del área de trabajo.

1. En Azure Portal, abra la cuenta de Automation y, en la página de esta cuenta, seleccione **Área de trabajo vinculada** en la sección **Recursos relacionados** de la izquierda.

2. En la página Desvincular área de trabajo, haga clic en **Desvincular área de trabajo**.

   ![Página Desvincular área de trabajo](media/automation-onboard-solutions-from-automation-account/automation-unlink-workspace-blade.png).

   Aparecerá un aviso para comprobar que desea continuar.

3. Mientras Azure Automation trata de desvincular la cuenta del área de trabajo de Log Analytics, puede hacer un seguimiento del progreso en la opción **Notificaciones** del menú.

Si ha usado la solución de administración de actualizaciones, también puede quitar los siguientes elementos que ya no necesite después de quitar la solución.

* Programaciones de actualización: cada una tendrá nombres que coinciden con las implementaciones de actualizaciones que ha creado.

* Los grupos de Hybrid Worker que se han creado para la solución: cada uno de ellos se llamará de forma similar a machine1.contoso.com_9ceb8108-26c9-4051-b6b3-227600d715c8.

Si ha usado la solución Start and Stop VMs during off-hours, también puede quitar los siguientes elementos, que ya no serán necesarios después de quitar la solución.

* Programaciones de runbook de inicio y detención de máquinas virtuales
* Runbooks de inicio y detención de máquinas virtuales
* variables

También puede desvincular el área de trabajo de la cuenta de Automation desde el área de trabajo de Log Analytics. En el área de trabajo, seleccione **Cuenta de Automation** en **Recursos relacionados**. En la página Cuenta de Automation, seleccione **Desvincular cuenta**.

## <a name="clean-up-resources"></a>Limpieza de recursos

Para quitar una VM de Update Management:

* En el área de trabajo de Log Analytics, quite la VM de la búsqueda guardada con la configuración de ámbito `MicrosoftDefaultScopeConfig-Updates`. Las búsquedas guardadas se pueden encontrar en la sección **General** del área de trabajo.
* Quite el [agente de Log Analytics para Windows](../azure-monitor/learn/quick-collect-windows-computer.md#clean-up-resources) o el [agente de Log Analytics para Linux](../azure-monitor/learn/quick-collect-linux-computer.md#clean-up-resources).

## <a name="next-steps"></a>Pasos siguientes

Continúe con los tutoriales de las soluciones para aprender a utilizarlas.

* [Tutorial: administración de las actualizaciones de la máquina virtual](automation-tutorial-update-management.md)

* [Tutorial: identificación del software en una máquina virtual](automation-tutorial-installed-software.md)

* [Tutorial: solución de problemas de los cambios en una máquina virtual](automation-tutorial-troubleshoot-changes.md)
