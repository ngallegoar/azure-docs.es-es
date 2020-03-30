---
title: Incorporación de las soluciones Update Management, Change Tracking e Inventory desde una máquina virtual de Azure
description: Aprenda a incorporar las soluciones Update Management, Change Tracking e Inventory que forman parte de Azure Automation, desde una máquina virtual de Azure.
services: automation
ms.date: 03/04/2020
ms.topic: conceptual
ms.custom: mvc
ms.openlocfilehash: 621b429f5dc3a6b6620e4d41ad46763e1d4fa226
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "78299540"
---
# <a name="onboard-update-management-change-tracking-and-inventory-solutions-from-an-azure-virtual-machine"></a>Incorporación de las soluciones Update Management, Change Tracking e Inventory desde una máquina virtual de Azure

Azure Automation proporciona soluciones para administrar las actualizaciones de seguridad del sistema operativo, el seguimiento de cambios y el inventario de los componentes instalados en los equipos. Hay varias maneras de incorporar las soluciones. Puede incorporar la solución desde una máquina virtual, [desde la cuenta de Automation](automation-onboard-solutions-from-automation-account.md), [mediante la exploración de varias máquinas](automation-onboard-solutions-from-browse.md) o usando un [runbook](automation-onboard-solutions.md). En este artículo se describe la incorporación de estas soluciones desde una máquina virtual de Azure.

## <a name="sign-in-to-azure"></a>Inicio de sesión en Azure

Inicie sesión en Azure Portal en https://portal.azure.com.

## <a name="enable-the-solutions"></a>Habilitación de las soluciones

En primer lugar, habilite una o las tres soluciones en la máquina virtual:

1. En [Azure Portal](https://portal.azure.com), en el panel izquierdo, seleccione **Máquinas virtuales** o busque y seleccione **Máquinas virtuales** en la **Página principal**.
2. Seleccione la máquina virtual para la que quiere habilitar una solución.
3. En la página de la máquina virtual, en **Operaciones**, seleccione **Update Management**, **Inventory** o **Change Tracking**. La máquina virtual puede existir en cualquier región independientemente de la ubicación de la cuenta de Automation. Al incorporar una solución de una máquina virtual, debe tener el permiso `Microsoft.OperationalInsights/workspaces/read` para determinar si la VM está incorporada a un área de trabajo. Para obtener información acerca de los permisos adicionales que se requieren, consulte los [permisos necesarios para incorporar máquinas](automation-role-based-access-control.md#onboarding).

Para más información sobre la incorporación en varias máquinas a la vez, consulte [Incorporación de las soluciones Update Management, Change Tracking e Inventory](automation-onboard-solutions-from-automation-account.md).

Para habilitar la solución, elija el área de trabajo de Azure Log Analytics y la cuenta de Automation, y seleccione **Habilitar**. La solución tarda hasta 15 minutos en habilitarse.

![Incorporación de la solución Update Management](media/automation-tutorial-update-management/manageupdates-update-enable.png)

Vaya a las otras soluciones y, a continuación, seleccione **Habilitar**. Las listas desplegables de cuentas de Automation y área de trabajo de Log Analytics están deshabilitadas porque estas soluciones usan la misma área de trabajo y cuenta de Automation que la solución anteriormente habilitada.

> [!NOTE]
> **Change tracking** y **Inventory** usan la misma solución. Cuando una de estas soluciones está habilitada, también se habilita la otra.

## <a name="scope-configuration"></a>Configuración de ámbito

Cada solución emplea una configuración de ámbito en el área de trabajo para definir los equipos de destino que obtendrán la solución. La configuración de ámbito es un grupo de una o más búsquedas guardadas que se usa para limitar el ámbito de la solución a equipos específicos. Para acceder a las configuraciones de ámbito, en la cuenta de Automation, en **Recursos relacionados**, seleccione **Área de trabajo**. En el área de trabajo, en **Orígenes de datos del área de trabajo**, seleccione **Configuraciones de ámbito**.

Si el área de trabajo seleccionada no dispone aún de las soluciones Update Management ni Change Tracking, se crean las siguientes configuraciones de ámbito:

* **MicrosoftDefaultScopeConfig-ChangeTracking**

* **MicrosoftDefaultScopeConfig-Updates**

Si el área de trabajo seleccionada ya tiene la solución, esta no se vuelve a implementar y no se agrega la configuración de ámbito.

Seleccione los puntos suspensivos ( **...** ) en cualquiera de las configuraciones y, luego, seleccione **Editar**. En el panel **Editar configuración de ámbito**, seleccione **Seleccionar grupos de equipos**. En el panel **Grupos de equipos** se muestran las búsquedas guardadas que se usan para crear la configuración de ámbito.

## <a name="saved-searches"></a>Búsquedas guardadas

Cuando un equipo se agrega a las soluciones Update Management, Change Tracking o Inventory, se agrega a una de las dos búsquedas guardadas del área de trabajo. Las búsquedas guardadas son consultas que contienen los equipos que son el destino de estas soluciones.

Vaya a su área de trabajo. En **General**, seleccione **Búsquedas guardadas**. Las dos búsquedas guardadas que usan estas soluciones se muestran en la tabla siguiente:

|Nombre     |Category  |Alias  |
|---------|---------|---------|
|MicrosoftDefaultComputerGroup     |  ChangeTracking       | ChangeTracking__MicrosoftDefaultComputerGroup        |
|MicrosoftDefaultComputerGroup     | Actualizaciones        | Updates__MicrosoftDefaultComputerGroup         |

Seleccione cualquiera de las búsquedas guardadas para ver la consulta que se usa para rellenar el grupo. La siguiente imagen muestra la consulta y sus resultados:

![Búsquedas guardadas](media/automation-onboard-solutions-from-vm/logsearch.png)

## <a name="unlink-workspace"></a>Unlink workspace (Desvincular área de trabajo)

Las siguientes soluciones dependen de un área de trabajo de Log Analytics:

* [Administración de actualizaciones](automation-update-management.md)
* [Seguimiento de cambios](automation-change-tracking.md)
* [Inicio y detención de máquinas virtuales durante las horas de trabajo](automation-solution-vm-management.md)

Si decide que ya no quiere integrar su cuenta de Automation con un área de trabajo de Log Analytics, puede desvincular la cuenta directamente desde Azure Portal.  Antes de continuar, primero deberá quitar las soluciones mencionadas anteriormente; en caso contrario, este proceso no podrá continuar. Revise el artículo de la solución concreta que ha importado para conocer los pasos necesarios para quitarla.

Después de quitar estas soluciones, puede realizar los pasos siguientes para desvincular la cuenta de Automation.

> [!NOTE]
> Algunas soluciones que incluyen versiones anteriores de la solución de supervisión de SQL Azure pueden haber creado recursos de automatización y también puede que tengan que quitarse antes de desvincularse del área de trabajo.

1. En Azure Portal, abra su cuenta de Automation y, en la página de la cuenta de Automation, seleccione **Área de trabajo vinculada** en la sección **Recursos relacionados** de la izquierda.

2. En la página Desvincular área de trabajo, haga clic en **Desvincular área de trabajo**.

   ![Página Desvincular área de trabajo](media/automation-onboard-solutions-from-vm/automation-unlink-workspace-blade.png).

   Recibirá un aviso para comprobar que desea continuar.

3. Aunque Azure Automation trate de desvincular la cuenta del área de trabajo de Log Analytics, puede seguir el progreso en **Notificaciones** en el menú.

Si ha usado la solución de administración de actualizaciones, también puede quitar los siguientes elementos que ya no necesite después de quitar la solución.

* Programaciones de actualización: cada una tendrá nombres que coinciden con las implementaciones de actualizaciones que ha creado.

* Los grupos de Hybrid Worker que se han creado para la solución: cada uno de ellos se llamará de forma similar a machine1.contoso.com_9ceb8108-26c9-4051-b6b3-227600d715c8.

Si ha usado la solución de inicio y detención de máquinas virtuales durante las horas de trabajo, también puede quitar los siguientes elementos que ya no necesite después de quitar la solución.

* Programaciones de runbook de inicio y detención de máquinas virtuales
* Runbooks de inicio y detención de máquinas virtuales
* variables

También puede desvincular el área de trabajo de la cuenta de Automation desde el área de trabajo de Log Analytics. En el área de trabajo, seleccione **Cuenta de Automation** en **Recursos relacionados**. En la página Cuenta de Automation, seleccione **Desvincular cuenta**.

## <a name="clean-up-resources"></a>Limpieza de recursos

Para quitar una VM de Update Management:

* En el área de trabajo de Log Analytics, quite la VM de la búsqueda guardada con la configuración de ámbito `MicrosoftDefaultScopeConfig-Updates`. Las búsquedas guardadas se pueden encontrar en la sección **General** del área de trabajo.
* Quite [Microsoft Monitoring Agent](../azure-monitor/learn/quick-collect-windows-computer.md#clean-up-resources) o el [agente de Log Analytics para Linux](../azure-monitor/learn/quick-collect-linux-computer.md#clean-up-resources).

## <a name="next-steps"></a>Pasos siguientes

Continúe con los tutoriales de las soluciones para aprender a usarlas:

* [Tutorial: administración de las actualizaciones de la máquina virtual](automation-tutorial-update-management.md)

* [Tutorial: identificación del software en una máquina virtual](automation-tutorial-installed-software.md)

* [Tutorial: solución de problemas de los cambios en una máquina virtual](automation-tutorial-troubleshoot-changes.md)
