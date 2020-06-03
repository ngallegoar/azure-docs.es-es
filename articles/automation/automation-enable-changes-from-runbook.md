---
title: Habilitación de Change Tracking e Inventario de Azure Automation desde un runbook
description: En este artículo se muestra cómo habilitar Change Tracking e Inventario desde un runbook.
services: automation
ms.topic: conceptual
ms.date: 05/10/2018
ms.custom: mvc
ms.openlocfilehash: 014442dee1be23a189e22a505abf86050601b2aa
ms.sourcegitcommit: 0b80a5802343ea769a91f91a8cdbdf1b67a932d3
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/25/2020
ms.locfileid: "83826749"
---
# <a name="enable-change-tracking-and-inventory-from-a-runbook"></a>Habilitación de Change Tracking e Inventario desde un runbook

En este artículo se describe cómo puede usar un runbook para habilitar la característica [Change Tracking e Inventario](change-tracking.md) para las máquinas virtuales del entorno. Para habilitar máquinas virtuales de Azure a escala, debe habilitar una máquina virtual existente mediante Change Tracking e Inventario. 

> [!NOTE]
> Al habilitar Change Tracking e Inventario, solo se admite la vinculación a un área de trabajo de Log Analytics y una cuenta de Automation en regiones concretas. Para obtener una lista de los pares de asignación que se admiten, consulte [Asignación de región para la cuenta de Automation y el área de trabajo de Log Analytics](how-to/region-mappings.md).

## <a name="prerequisites"></a>Prerrequisitos

* Suscripción de Azure. Si aún no tiene ninguna, puede [activar las ventajas de la suscripción a MSDN](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) o suscribirse para obtener una [cuenta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).
* Una [cuenta de Automation](automation-offering-get-started.md) para administrar máquinas.
* Una [máquina virtual](../virtual-machines/windows/quick-create-portal.md).

## <a name="enable-change-tracking-and-inventory"></a>Habilitación de Change Tracking e Inventario 

1. En Azure Portal, seleccione **Cuentas de Automation** y después la cuenta de Automation en la lista.
1. Seleccione **Inventario** en **Administración de configuración**.
1. Seleccione un área de trabajo de Log Analytics existente o cree una. 
1. Haga clic en **Habilitar**.

    ![Habilitación de Change Tracking e Inventario](media/automation-enable-changes-from-runbook/inventory-onboard.png)

## <a name="select-azure-vm-to-manage"></a>Selección de la máquina virtual de Azure que se va a administrar

Con Change Tracking e Inventario habilitado, puede agregar una máquina virtual de Azure para la administración mediante la característica.

1. Desde la cuenta de Automation, seleccione **Change Tracking** o **Inventario** en **Administración de configuración**.

2. Haga clic en **Agregar máquinas virtuales de Azure** para agregar la máquina virtual.

3. Elija la máquina virtual en la lista y haga clic en **Habilitar**. Esta acción habilita Change Tracking e Inventario para la máquina virtual.

   ![Habilitación de Change Tracking e Inventario para la máquina virtual](media/automation-enable-changes-from-runbook/enable-change-tracking.png)

    > [!NOTE]
    > Si intenta habilitar otra característica antes de que se complete la instalación de Change Tracking e Inventario, recibirá este mensaje: `Installation of another solution is in progress on this or a different virtual machine. When that installation completes the Enable button is enabled, and you can request installation of the solution on this virtual machine.`

## <a name="install-and-update-modules"></a>Instalar y actualizar módulos

Para habilitar de forma correcta Change Tracking e Inventario, es necesario actualizar a los módulos de Azure más recientes e importar el módulo [Az.OperationalInsights](https://docs.microsoft.com/powershell/module/az.operationalinsights/?view=azps-3.7.0).

1. En la cuenta de Automation, seleccione **Módulos** en **Recursos compartidos**. 
2. Seleccione **Actualizar módulos de Azure** para actualizar los módulos de Azure a la versión más reciente. 
3. Haga clic en **Sí** para actualizar todos los módulos de Azure existentes a la versión más reciente.

    ![Actualización de los módulos](media/automation-enable-changes-from-runbook/update-modules.png)

4. Vuelva a **Módulos** en **Recursos compartidos**. 
5. Seleccione **Examinar la galería** para abrir la galería de módulos. 
6. Busque Az.OperationalInsights e importe este módulo en la cuenta de Automation.

    ![Importar el módulo OperationalInsights](media/automation-enable-changes-from-runbook/import-operational-insights-module.png)

## <a name="import-a-runbook-to-enable-change-tracking-and-inventory"></a>Importación de un runbook para habilitar Change Tracking e Inventario

1. En la cuenta de Automation, seleccione **Runbooks** en **Automatización de procesos**.
2. Seleccione **Explorar la galería**.
3. Busque `update and change tracking`.
4. Seleccione el runbook y haga clic en **Importar** en la página Ver origen. 
5. Seleccione **Aceptar** para importar el runbook en la cuenta de Automation.

   ![Importación del runbook para el programa de instalación](media/automation-enable-changes-from-runbook/import-from-gallery.png)

6. En la página Runbook, haga clic en **Editar** y, después, seleccione **Publicar**. 
7. En el panel Publicar runbook, haga clic en **Sí** para publicar el runbook.

## <a name="start-the-runbook"></a>Inicio del runbook

Debe haber habilitado Change Tracking e Inventario para que una máquina virtual de Azure inicie este runbook. Requiere una máquina virtual y un grupo de recursos existentes con la característica habilitada para los parámetros.

1. Abra el runbook **Enable-MultipleSolution**.

   ![Runbooks de varias soluciones](media/automation-enable-changes-from-runbook/runbook-overview.png)

1. Haga clic en el botón de inicio y escriba los valores de parámetros en los campos siguientes:

   * **VMNAME**: el nombre de una máquina virtual existente que se va a agregar a Change Tracking e Inventario. Deje este campo en blanco para agregar todas las máquinas virtuales del grupo de recursos.
   * **VMRESOURCEGROUP**: el nombre del grupo de recursos de las máquinas virtuales que se van a habilitar.
   * **SUBSCRIPTIONID**: el identificador de la suscripción de la nueva máquina virtual que se va a habilitar. Deje este campo en blanco para usar la suscripción del área de trabajo. Si usa otro identificador de suscripción, agregue la cuenta de ejecución de la cuenta de Automation como colaborador de la suscripción.
   * **ALREADYONBOARDEDVM**: el nombre de la máquina virtual que ya está habilitada manualmente para los cambios.
   * **ALREADYONBOARDEDVMRESOURCEGROUP**: el nombre del grupo de recursos al que pertenece la máquina virtual.
   * **SOLUTIONTYPE**: escriba **ChangeTracking**.

   ![Parámetros del runbook Enable-MultipleSolution](media/automation-enable-changes-from-runbook/runbook-parameters.png)

1. Seleccione **Aceptar** para iniciar el trabajo del runbook.
1. Supervise el progreso y los errores en la página de trabajos de runbook.

## <a name="next-steps"></a>Pasos siguientes

* Para programar un runbook, vea [Administración de programaciones en Azure Automation](shared-resources/schedules.md).
* Para obtener más información sobre cómo trabajar con la característica, consulte [Administración de Change Tracking e Inventario](change-tracking-file-contents.md).
* Para obtener información sobre las configuraciones de ámbito, vea [Uso de configuraciones de ámbito para Change Tracking e Inventario](automation-scope-configurations-change-tracking.md).
* Para obtener información sobre cómo usar la característica para identificar el software instalado en el entorno, vea [Detección del software instalado en las máquinas virtuales](automation-tutorial-installed-software.md).
* Si no quiere integrar la cuenta de Automation con un área de trabajo Log Analytics al habilitar la característica, vea [Desvinculación de un área de trabajo de una cuenta de Automation](automation-unlink-workspace-change-tracking.md).
* Cuando haya terminado de implementar los cambios en las máquinas virtuales, puede quitarlas como se describe en [Eliminación de máquinas virtuales de Change Tracking e Inventario](automation-remove-vms-from-change-tracking.md).
* Para solucionar problemas generales con la característica, consulte [Solución de problemas de Change Tracking e Inventario](troubleshoot/change-tracking.md).