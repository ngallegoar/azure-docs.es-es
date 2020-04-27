---
title: Incorporación de soluciones de actualización, seguimiento de cambios e inventario a Azure Automation
description: Aprenda a incorporar soluciones de actualización y control de cambios a Azure Automation.
services: automation
ms.topic: tutorial
ms.date: 05/10/2018
ms.custom: mvc
ms.openlocfilehash: 721157c333e381799ef08930c667c51a51a4fd6a
ms.sourcegitcommit: b55d7c87dc645d8e5eb1e8f05f5afa38d7574846
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/16/2020
ms.locfileid: "81457627"
---
# <a name="onboard-update-change-tracking-and-inventory-solutions-to-azure-automation"></a>Incorporación de soluciones de actualización, seguimiento de cambios e inventario a Azure Automation

En este tutorial, aprenderá a incorporar soluciones de actualización, control de cambios e inventario en máquinas virtuales a Azure Automation. Así, sabrá:

> [!div class="checklist"]
> * Incorporar una máquina virtual de Azure
> * Habilitar soluciones
> * Instalar y actualizar módulos
> * Importación de un runbook de incorporación
> * Inicio del runbook

>[!NOTE]
>Este artículo se ha actualizado para usar el nuevo módulo Az de Azure PowerShell. Aún puede usar el módulo de AzureRM que continuará recibiendo correcciones de errores hasta diciembre de 2020 como mínimo. Para más información acerca del nuevo módulo Az y la compatibilidad con AzureRM, consulte [Introducing the new Azure PowerShell Az module](https://docs.microsoft.com/powershell/azure/new-azureps-module-az?view=azps-3.5.0) (Presentación del nuevo módulo Az de Azure PowerShell). Para obtener instrucciones sobre la instalación del módulo Az en Hybrid Runbook Worker, consulte [Instalación del módulo de Azure PowerShell](https://docs.microsoft.com/powershell/azure/install-az-ps?view=azps-3.5.0). Puede actualizar los módulos de su cuenta de Automation a la versión más reciente mediante [Actualización de módulos de Azure PowerShell en Azure Automation](automation-update-azure-modules.md).

## <a name="prerequisites"></a>Prerrequisitos

Los siguientes requisitos son necesarios para completar este tutorial:

* Suscripción de Azure. Si aún no tiene ninguna, puede [activar las ventajas de la suscripción a MSDN](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) o suscribirse para obtener una [cuenta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).
* Una [cuenta de Automation](automation-offering-get-started.md) para administrar máquinas.
* Una [máquina virtual](../virtual-machines/windows/quick-create-portal.md) para incorporar.

## <a name="onboard-an-azure-vm"></a>Incorporar una máquina virtual de Azure

Hay varias maneras de incorporar máquinas, puede incorporar la solución [desde una máquina virtual](automation-onboard-solutions-from-vm.md), [desde la exploración en varias máquinas](automation-onboard-solutions-from-browse.md), [desde su cuenta de Automation](automation-onboard-solutions-from-automation-account.md) o mediante un runbook. En este tutorial se explica cómo habilitar Update Management mediante un runbook. Para incorporar máquinas virtuales de Azure a escala, debe incorporar una máquina virtual existente con la solución de control de cambios o de administración de actualizaciones. En este paso, se incorpora una máquina virtual con la solución de administración de actualizaciones y control de cambios.

### <a name="enable-change-tracking-and-inventory"></a>Habilitación de Change Tracking e Inventario

Las soluciones Change Tracking e Inventario permiten realizar un [control de cambios](automation-vm-change-tracking.md) e [inventario](automation-vm-inventory.md) en las máquinas virtuales. En este paso, se habilitan las soluciones en una máquina virtual.

1. En Azure Portal, seleccione **Cuentas de Automation** y, a continuación, seleccione la cuenta de automatización en la lista.
1. Seleccione **Inventario** en **Administración de configuración**.
1. Seleccione un área de trabajo de Log Analytics existente o cree una. 
1. Haga clic en **Habilitar**.

    ![Incorporación de la solución de actualización](media/automation-onboard-solutions/inventory-onboard.png)

### <a name="enable-update-management"></a>Habilitación de la Administración de actualizaciones

La solución Administración de actualizaciones permite administrar las actualizaciones y las revisiones para las máquinas virtuales Windows de Azure. Puede evaluar rápidamente el estado de las actualizaciones disponibles, programar la instalación de las actualizaciones necesarias y revisar los resultados de la implementación para comprobar si las actualizaciones se aplicaron correctamente en la máquina virtual. En este paso, se habilita la solución en la máquina virtual.

1. En su cuenta de Automation, seleccione **Update Management** en la sección **Update Management**.
1. El área de trabajo de Log Analytics seleccionada es el área de trabajo que se usa en el paso anterior. Haga clic en **Habilitar** para incorporar la solución Administración de actualizaciones. Mientras se instala la solución Administración de actualizaciones, se muestra un banner azul. 

    ![Incorporación de la solución de actualización](media/automation-onboard-solutions/update-onboard.png)

### <a name="select-azure-vm-to-be-managed"></a>Seleccione la máquina virtual de Azure que va a ser administrada

Ahora que las soluciones están habilitadas, puede agregar la máquina virtual de Azure que quiera incorporar a esas soluciones.

1. En la cuenta de Automation, seleccione **Change Tracking** en **Administración de configuración**. 
2. En la página Change Tracking, haga clic en **Agregar máquinas virtuales de Azure** para agregar la máquina virtual.

3. Seleccione la máquina virtual en la lista y haga clic en **Habilitar**. De este modo, se habilitan las soluciones Change Tracking e Inventario para la máquina virtual.

   ![Habilitar la solución de actualizaciones para la máquina virtual](media/automation-onboard-solutions/enable-change-tracking.png)

4. Cuando finalice la notificación de la incorporación de la máquina virtual, seleccione **Update Management** en **Update Management**.

5. Seleccione **Agregar máquinas virtuales de Azure** para agregar la máquina virtual.

6. Seleccione la máquina virtual en la lista y seleccione **Habilitar**. De este modo, se habilita la solución Update Management para la máquina virtual.

   ![Habilitar la solución de actualizaciones para la máquina virtual](media/automation-onboard-solutions/enable-update.png)

> [!NOTE]
> Si no espera a que la otra solución finalice, al habilitar la siguiente solución recibirá este mensaje: `Installation of another solution is in progress on this or a different virtual machine. When that installation completes the Enable button is enabled, and you can request installation of the solution on this virtual machine.`

## <a name="install-and-update-modules"></a>Instalar y actualizar módulos

Para automatizar correctamente la incorporación de soluciones, es necesario actualizar a los módulos de Azure más recientes e importar el módulo [Az.OperationalInsights](https://docs.microsoft.com/powershell/module/az.operationalinsights/?view=azps-3.7.0).

## <a name="update-azure-modules"></a>Actualización de los módulos de Azure

1. En la cuenta de Automation, seleccione **Módulos** en **Recursos compartidos**. 
2. Seleccione **Actualizar módulos de Azure** para actualizar los módulos de Azure a la versión más reciente. 
3. Haga clic en **Sí** para actualizar todos los módulos de Azure existentes a la versión más reciente.

![Actualización de los módulos](media/automation-onboard-solutions/update-modules.png) A

### <a name="install-azoperationalinsights-module"></a>Instalación del módulo Az.OperationalInsights

1. En la cuenta de Automation, seleccione **Módulos** en **Recursos compartidos**. 
2. Seleccione **Examinar la galería** para abrir la galería de módulos. 
3. Busque Az.OperationalInsights e importe este módulo en la cuenta de Automation.

![Importar el módulo OperationalInsights](media/automation-onboard-solutions/import-operational-insights-module.png)

## <a name="import-the-onboarding-runbook"></a>Importación de un runbook de incorporación

1. En la cuenta de Automation, seleccione **Runbooks** en **Automatización de procesos**.
1. Seleccione **Explorar la galería**.
1. Busque `update and change tracking`.
3. Seleccione el runbook y haga clic en **Importar** en la página Ver origen. 
4. Seleccione **Aceptar** para importar el runbook en la cuenta de Automation.

   ![Importación de un runbook de incorporación](media/automation-onboard-solutions/import-from-gallery.png)

6. En la página Runbook, haga clic en **Editar** y, después, seleccione **Publicar**. 
7. En el panel Publicar runbook, haga clic en **Sí** para publicar el runbook.

## <a name="start-the-runbook"></a>Inicio del runbook

Para iniciar este runbook, debe haber incorporado soluciones de actualización o de control de cambios a una máquina virtual de Azure. Se requiere una máquina virtual existente y un grupo de recursos con la solución incorporada para los parámetros.

1. Abra el runbook **Enable-MultipleSolution**.

   ![Runbooks de varias soluciones](media/automation-onboard-solutions/runbook-overview.png)

1. Haga clic en el botón Inicio y escriba los siguientes valores de parámetros.

   * **VMNAME**: dejar en blanco. El nombre de una máquina virtual existente para incorporar a la solución de actualización o de control de cambios. Deje este valor en blanco para incorporar todas las máquinas virtuales del grupo de recursos.
   * **VMRESOURCEGROUP**: el nombre del grupo de recursos de las máquinas virtuales que van a ser incorporadas.
   * **SUBSCRIPTIONID**: dejar en blanco. El identificador de la suscripción de la nueva máquina virtual que se va a incorporar. Deje este valor en blanco para usar la suscripción del área de trabajo. Cuando se proporcione un identificador de suscripción diferente, también se debe agregar la cuenta de ejecución de esta cuenta de Automation como colaborador de esta suscripción.
   * **ALREADYONBOARDEDVM**: el nombre de la máquina virtual que se incorporó manualmente a la solución de actualización o de control de cambios.
   * **ALREADYONBOARDEDVMRESOURCEGROUP**: el nombre del grupo de recursos al que pertenece la máquina virtual.
   * **SOLUTIONTYPE**: escriba **Updates** (Actualizaciones) o **ChangeTracking** (Control de cambios).

   ![Parámetros del runbook Enable-MultipleSolution](media/automation-onboard-solutions/runbook-parameters.png)

1. Seleccione **Aceptar** para iniciar el trabajo del runbook.
1. Supervise el progreso y los errores en la página de trabajos de runbook.

## <a name="clean-up-resources"></a>Limpieza de recursos

Para quitar una VM de Update Management:

1. en el área de trabajo de Log Analytics, quite la VM de la búsqueda guardada con la configuración de ámbito `MicrosoftDefaultScopeConfig-Updates`. Las búsquedas guardadas se pueden encontrar en la sección **General** del área de trabajo.
2. Quite el [agente de Log Analytics para Windows](../azure-monitor/learn/quick-collect-windows-computer.md#clean-up-resources) o el [agente de Log Analytics para Linux](../azure-monitor/learn/quick-collect-linux-computer.md#clean-up-resources).

## <a name="next-steps"></a>Pasos siguientes

En este tutorial, ha aprendido a:

> [!div class="checklist"]
> * Incorporar máquinas virtuales de Azure manualmente
> * Instalar y actualizar los módulos de Azure necesarios
> * Importar un runbook que incorpora máquinas virtuales de Azure
> * Iniciar el runbook que incorpora máquinas virtuales de Azure automáticamente

Siga este vínculo para más información sobre programación de runbooks.

> [!div class="nextstepaction"]
> [Programación de runbooks](automation-schedules.md).
