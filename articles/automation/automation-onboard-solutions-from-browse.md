---
title: Aprenda a incorporar las soluciones Update Management, Change Tracking e Inventory en varias máquinas virtuales en Azure Automation
description: Obtenga información acerca de cómo incorporar una máquina virtual de Azure con las soluciones Update Management, Change Tracking e Inventory que forman parte de Azure Automation
services: automation
ms.date: 04/11/2019
ms.topic: article
ms.custom: mvc
ms.openlocfilehash: d97fbe6ac515a2559340474105d73b7c9b9c6ee4
ms.sourcegitcommit: 4499035f03e7a8fb40f5cff616eb01753b986278
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/03/2020
ms.locfileid: "82731924"
---
# <a name="enable-update-management-change-tracking-and-inventory-solutions-on-multiple-vms"></a>Habilitación de las soluciones Update Management, Change Tracking e Inventory en varias máquinas virtuales

Azure Automation proporciona soluciones para administrar las actualizaciones de seguridad del sistema operativo, el seguimiento de cambios y el inventario de los componentes instalados en los equipos. Hay varias formas de incorporar máquinas, puede incorporar la solución desde una máquina virtual, [desde la cuenta de Automation](automation-onboard-solutions-from-vm.md), [al explorar máquinas virtuales](automation-onboard-solutions-from-automation-account.md) o mediante un [runbook](automation-onboard-solutions.md). Este artículo trata la incorporación de estas soluciones al examinar máquinas virtuales en Azure.

## <a name="sign-in-to-azure"></a>Inicio de sesión en Azure

Inicie sesión en Azure en https://portal.azure.com

## <a name="enable-solutions"></a>Habilitar soluciones

En Azure Portal, vaya a **Máquinas virtuales**.

Mediante las casillas, seleccione las máquinas virtuales que desee incorporar con Change Tracking, Inventory o Update Management. La incorporación está disponible para un máximo de tres grupos de recursos a la vez. Las VM de Azure pueden existir en cualquier región independientemente de la ubicación de la cuenta de Automation.

![Lista de máquinas virtuales](media/automation-onboard-solutions-from-browse/vmlist.png)
> [!TIP]
> Use los controles de filtro para modificar la lista de máquinas virtuales y, después, haga clic en la casilla superior para seleccionar todas las máquinas virtuales de la lista.

En la barra de comandos, haga clic en **Servicios** y seleccione **Change tracking**, **Inventory** o **Update Management**.

> [!NOTE]
> Change Tracking e Inventory usan la misma solución. Cuando una está habilitada, la otra también está habilitada.

La siguiente imagen es de Update Management. Change Tracking e Inventory tienen el mismo diseño y comportamiento.

La lista de máquinas virtuales se filtra para mostrar solo las que se encuentran en la misma suscripción y ubicación. Si las máquinas virtuales están en más de tres grupos de recursos, se seleccionan los tres primeros.

### <a name="onboarding-limitations"></a><a name="resource-group-limit"></a> Limitaciones de incorporación

El número de grupos de recursos que puede usar para incorporar está limitado por los [límites de implementación de Resource Manager](../azure-resource-manager/templates/cross-resource-group-deployment.md). Las implementaciones de Resource Manager, que no se deben confundir con las implementaciones de actualizaciones, están limitadas a 5 grupos de recursos por implementación. Para garantizar la integridad de la incorporación, dos de esos grupos de recursos están reservados para configurar el área de trabajo de Log Analytics, la cuenta de Automation y los recursos relacionados. Esto le deja tres grupos de recursos para seleccionar para la implementación. Este límite solo se aplica a la incorporación simultánea, no al número de grupos de recursos que se pueden administrar mediante una solución de Automation.

También puede usar un runbook para la incorporación; para más información, consulte [Incorporación de soluciones de actualización y control de cambios a Azure Automation](automation-onboard-solutions.md).

Use los controles de filtro para seleccionar máquinas virtuales de distintas suscripciones, ubicaciones y grupos de recursos.

![Habilitar la solución Update Management](media/automation-onboard-solutions-from-browse/onboardsolutions.png)

Revise las opciones del área de trabajo de Log Analytics y de la cuenta de Automation. De forma predeterminada, se seleccionan un área de trabajo y una cuenta de Automation existentes. Si quiere usar un área de trabajo de Log Analytics y una cuenta de Automation diferentes, haga clic en **PERSONALIZAR** para seleccionarlas en la página **Configuración personalizada**. Al elegir un área de trabajo de Log Analytics, se realiza una comprobación para determinar si está vinculada con una cuenta de Automation. Si se encuentra una cuenta de Automation vinculada, verá la siguiente pantalla. Cuando termine, haga clic en **Aceptar**.

![Seleccionar área de trabajo y cuenta](media/automation-onboard-solutions-from-browse/selectworkspaceandaccount.png)

Si el área de trabajo seleccionada no está vinculada a una cuenta de Automation, verá la siguiente pantalla. Seleccione una cuenta de Automation y haga clic en **Aceptar** cuando haya terminado.

![No hay área de trabajo](media/automation-onboard-solutions-from-browse/no-workspace.png)

> [!NOTE]
> Al habilitar las soluciones, solo en determinadas regiones se puede vincular un área de trabajo de Log Analytics y una cuenta de Automation.
>
> Para obtener una lista de los pares de asignación que se admiten, consulte [Asignación de región para la cuenta de Automation y el área de trabajo de Log Analytics](how-to/region-mappings.md).

Anule la selección de las casillas que haya junto a las máquinas virtuales que no desee habilitar. Ya se ha anulado la selección de las máquinas virtuales que no se puedan habilitar.

Haga clic en **Habilitar** para habilitar la solución. La solución tarda hasta 15 minutos en habilitarse.

## <a name="unlink-workspace"></a>Unlink workspace (Desvincular área de trabajo)

Las siguientes soluciones dependen de un área de trabajo de Log Analytics:

* [Administración de actualizaciones](automation-update-management.md)
* [Seguimiento de cambios](automation-change-tracking.md)
* [Inicio y detención de máquinas virtuales durante las horas de trabajo](automation-solution-vm-management.md)

Si decide que ya no quiere integrar su cuenta de Automation con un área de trabajo de Log Analytics, puede desvincular la cuenta directamente desde Azure Portal. Antes de continuar, primero deberá quitar las soluciones mencionadas anteriormente; en caso contrario, este proceso no podrá continuar. Revise el artículo de la solución concreta que ha importado para conocer los pasos necesarios para quitarla.

Después de quitar estas soluciones, puede realizar los pasos siguientes para desvincular la cuenta de Automation.

> [!NOTE]
> Algunas soluciones que incluyen versiones anteriores de la solución de supervisión de SQL Azure pueden haber creado recursos de automatización y también puede que tengan que quitarse antes de desvincularse del área de trabajo.

1. En Azure Portal, abra su cuenta de Automation y, en la página de la cuenta de Automation, seleccione **Área de trabajo vinculada** en la sección **Recursos relacionados** de la izquierda.

2. En la página Desvincular área de trabajo, haga clic en **Desvincular área de trabajo**.

   ![Página Desvincular área de trabajo](media/automation-onboard-solutions-from-browse/automation-unlink-workspace-blade.png).

   Recibirá un aviso para comprobar que desea continuar.

3. Aunque Azure Automation trate de desvincular la cuenta del área de trabajo de Log Analytics, puede seguir el progreso en **Notificaciones** en el menú.

Si ha usado la solución de administración de actualizaciones, también puede quitar los siguientes elementos que ya no necesite después de quitar la solución.

* Programaciones de actualizaciones: cada una tendrá nombres que coinciden con las implementaciones de las actualizaciones que ha creado.

* Los grupos de Hybrid Worker que se han creado para la solución: cada uno de ellos se llamará de forma similar a machine1.contoso.com_9ceb8108 - 26 c 9-4051-b6b3-227600d715c8).

Si ha usado la solución de inicio y detención de máquinas virtuales durante las horas de trabajo, también puede quitar los siguientes elementos que ya no necesite después de quitar la solución.

* Programaciones de runbook de inicio y detención de máquinas virtuales
* Runbooks de inicio y detención de máquinas virtuales
* variables

También puede desvincular el área de trabajo de la cuenta de Automation desde el área de trabajo de Log Analytics. En el área de trabajo, seleccione **Cuenta de Automation** en **Recursos relacionados**. En la página Cuenta de Automation, seleccione **Desvincular cuenta**.

## <a name="troubleshooting"></a>Solución de problemas

Al incorporar varias máquinas, puede haber algunas que se muestren como `Cannot enable`. Hay diferentes motivos por los que algunas máquinas no se pueden habilitar. En las secciones siguientes se muestran las posibles razones para que aparezca el estado `Cannot enable` en una VM al intentar incorporarla.

### <a name="vm-reports-to-a-different-workspace-workspacename--change-configuration-to-use-it-for-enabling"></a>La máquina virtual depende de otra área de trabajo: '\<NombreDeAreaDeTrabajo\>'.  Cambie la configuración para usarla para habilitar

**Causa**: este error muestra que la máquina virtual que intenta incorporar depende de otra área de trabajo.

**Solución**: Haga clic en **Usar como configuración** para cambiar la cuenta de Automation y el área de trabajo de Log Analytics de destino.

### <a name="vm-reports-to-a-workspace-that-is-not-available-in-this-subscription"></a>La máquina virtual depende de un área de trabajo que no está disponible en esta suscripción

**Causa**: el área de trabajo de la que depende la máquina virtual:

* Está en otra suscripción, o bien
* No existe, o bien
* Está en un grupo de recursos al que no tiene permisos de acceso

**Solución**: busque la cuenta de Automation asociada al área de trabajo de la que depende la máquina virtual e incorpore la máquina virtual, para lo cual debe cambiar la configuración del ámbito.

### <a name="vm-operating-system-version-or-distribution-is-not-supported"></a>La versión o distribución del sistema operativo de la máquina virtual no son compatibles

**Causa:** la solución no se admite en todas las distribuciones de Linux o todas las versiones de Windows.

**Solución:** Consulte la [lista de clientes admitidos](automation-update-management.md#supported-client-types).

### <a name="classic-vms-cannot-be-enabled"></a>Las máquinas virtuales clásicas no se puede habilitar

**Causa**: no se admiten las máquinas virtuales que usan el modelo de implementación clásica.

**Solución**: migre la máquina virtual al modelo de implementación de Resource Manager. Para aprender a hacerlo, consulte [Migración de los recursos del modelo de implementación clásica](../virtual-machines/windows/migration-classic-resource-manager-overview.md).

### <a name="vm-is-stopped-deallocated"></a>La máquina virtual está detenida. (desasignada)

**Causa**: la máquina virtual no está en estado de **ejecución**.

**Solución**: para incorporar una máquina virtual a una solución debe estar en ejecución. Haga clic en el vínculo insertado **Iniciar VM** para iniciar la máquina virtual sin salir de la página.

## <a name="clean-up-resources"></a>Limpieza de recursos

Para quitar una VM de Update Management:

* En el área de trabajo de Log Analytics, quite la VM de la búsqueda guardada con la configuración de ámbito `MicrosoftDefaultScopeConfig-Updates`. Las búsquedas guardadas se pueden encontrar en la sección **General** del área de trabajo.
* Quite el [agente de Log Analytics para Windows](../azure-monitor/learn/quick-collect-windows-computer.md#clean-up-resources) o el [agente de Log Analytics para Linux](../azure-monitor/learn/quick-collect-linux-computer.md#clean-up-resources).

## <a name="next-steps"></a>Pasos siguientes

Ahora que está habilitada la solución para las máquinas virtuales, visite el artículo de información general de Update Management para crear una **implementación de actualizaciones** para las máquinas.

> [!div class="nextstepaction"]
> [Update Management: administración de actualizaciones y revisiones para las máquinas virtuales de Azure](./automation-tutorial-update-management.md)

Tutoriales acerca de las soluciones y cómo utilizarlas:

* [Tutorial: administración de las actualizaciones de la máquina virtual](automation-tutorial-update-management.md)

* [Tutorial: identificación del software en una máquina virtual](automation-tutorial-installed-software.md)

* [Tutorial: solución de problemas de los cambios en una máquina virtual](automation-tutorial-troubleshoot-changes.md)
