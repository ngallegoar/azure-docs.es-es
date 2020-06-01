---
title: Habilitación de Update Management de Azure Automation desde un runbook
description: En este artículo se indica cómo habilitar Update Management desde un runbook.
services: automation
ms.topic: conceptual
ms.date: 05/10/2018
ms.custom: mvc
ms.openlocfilehash: 39ebdb6937b03d72365e9d3785af9571ebb66186
ms.sourcegitcommit: 0b80a5802343ea769a91f91a8cdbdf1b67a932d3
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/25/2020
ms.locfileid: "83836063"
---
# <a name="enable-update-management-from-a-runbook"></a>Habilitación de Update Management desde un runbook

En este artículo se describe cómo puede usar un runbook para habilitar la característica [Update Management](automation-update-management.md) para las máquinas virtuales del entorno. Para habilitar máquinas virtuales de Azure a escala, debe habilitar una máquina virtual existente mediante Update Management. 

> [!NOTE]
> Al habilitar Update Management, solo en determinadas regiones se puede vincular un área de trabajo de Log Analytics y una cuenta de Automation. Para obtener una lista de los pares de asignación que se admiten, consulte [Asignación de región para la cuenta de Automation y el área de trabajo de Log Analytics](how-to/region-mappings.md).

## <a name="prerequisites"></a>Prerrequisitos

* Suscripción de Azure. Si aún no tiene ninguna, puede [activar las ventajas de la suscripción a MSDN](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) o suscribirse para obtener una [cuenta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).
* Una [cuenta de Automation](automation-offering-get-started.md) para administrar máquinas.
* Una [máquina virtual](../virtual-machines/windows/quick-create-portal.md).

## <a name="enable-update-management"></a>Habilitación de la Administración de actualizaciones

1. En su cuenta de Automation, seleccione **Update Management** en **Update Management**.

2. Seleccione el área de trabajo de Log Analytics y, después, haga clic en **Habilitar**. Mientras se está habilitando Update Management, se muestra un banner azul. 

    ![Habilitación de la Administración de actualizaciones](media/automation-onboard-solutions/update-onboard.png)

## <a name="select-azure-vm-to-manage"></a>Selección de la máquina virtual de Azure que se va a administrar

Con Update Management habilitado, puede agregar una máquina virtual de Azure para recibir actualizaciones.

1. En su cuenta de Automation, seleccione **Update Management** en **Update Management**.

2. Seleccione **Agregar máquinas virtuales de Azure** para agregar la máquina virtual.

3. Elija la máquina virtual en la lista y haga clic en **Habilitar** para configurar la máquina virtual para las actualizaciones. 

   ![Habilitación de Update Management para la máquina virtual](media/automation-onboard-solutions/enable-update.png)

    > [!NOTE]
    > Si intenta habilitar otra característica antes de que se complete la instalación de Update Management, recibirá este mensaje: `Installation of another solution is in progress on this or a different virtual machine. When that installation completes the Enable button is enabled, and you can request installation of the solution on this virtual machine.`

## <a name="install-and-update-modules"></a>Instalar y actualizar módulos

Para habilitar de forma correcta Update Management para las máquinas virtuales, es necesario actualizar a los módulos de Azure más recientes e importar el módulo [Az.OperationalInsights](https://docs.microsoft.com/powershell/module/az.operationalinsights/?view=azps-3.7.0).

1. En la cuenta de Automation, seleccione **Módulos** en **Recursos compartidos**. 
2. Seleccione **Actualizar módulos de Azure** para actualizar los módulos de Azure a la versión más reciente. 
3. Haga clic en **Sí** para actualizar todos los módulos de Azure existentes a la versión más reciente.

    ![Actualización de los módulos](media/automation-onboard-solutions/update-modules.png)

4. Vuelva a **Módulos** en **Recursos compartidos**. 
5. Seleccione **Examinar la galería** para abrir la galería de módulos. 
6. Busque `Az.OperationalInsights` e importe este módulo en la cuenta de Automation.

    ![Importar el módulo OperationalInsights](media/automation-onboard-solutions/import-operational-insights-module.png)

## <a name="import-a-runbook-to-enable-update-management"></a>Importación de un runbook para habilitar Update Management

1. En la cuenta de Automation, seleccione **Runbooks** en **Automatización de procesos**.
2. Seleccione **Explorar la galería**.
3. Busque `update and change tracking`.
4. Seleccione el runbook y haga clic en **Importar** en la página Ver origen. 
5. Seleccione **Aceptar** para importar el runbook en la cuenta de Automation.

   ![Importación del runbook para el programa de instalación](media/automation-onboard-solutions/import-from-gallery.png)

6. En la página Runbook, haga clic en **Editar** y, después, seleccione **Publicar**. 
7. En el panel Publicar runbook, haga clic en **Sí** para publicar el runbook.

## <a name="start-the-runbook"></a>Inicio del runbook

Debe haber habilitado Update Management para que una máquina virtual de Azure inicie este runbook. Requiere una máquina virtual y un grupo de recursos existentes con la característica habilitada para los parámetros.

1. Abra el runbook **Enable-MultipleSolution**.

   ![Runbook de varias soluciones](media/automation-onboard-solutions/runbook-overview.png)

2. Haga clic en el botón de inicio y escriba los valores de parámetros en los campos siguientes:

   * **VMNAME**: el nombre de una máquina virtual existente que se va a agregar a Update Management. Deje este campo en blanco para agregar todas las máquinas virtuales del grupo de recursos.
   * **VMRESOURCEGROUP**: el nombre del grupo de recursos de las máquinas virtuales que se van a habilitar.
   * **SUBSCRIPTIONID**: el identificador de la suscripción de la nueva máquina virtual que se va a habilitar. Deje este campo en blanco para usar la suscripción del área de trabajo. Si usa otro identificador de suscripción, agregue la cuenta de ejecución de la cuenta de Automation como colaborador de la suscripción.
   * **ALREADYONBOARDEDVM**: el nombre de la máquina virtual que ya está habilitada manualmente para las actualizaciones.
   * **ALREADYONBOARDEDVMRESOURCEGROUP**: el nombre del grupo de recursos al que pertenece la máquina virtual.
   * **SOLUTIONTYPE**: escriba **Actualizaciones**.

   ![Parámetros del runbook Enable-MultipleSolution](media/automation-onboard-solutions/runbook-parameters.png)

3. Seleccione **Aceptar** para iniciar el trabajo del runbook.
4. Supervise el progreso y los errores en la página de trabajos de runbook.

## <a name="next-steps"></a>Pasos siguientes

* Para programar un runbook, consulte [Administración de programaciones en Azure Automation](shared-resources/schedules.md).
* Para usar Update Management para máquinas virtuales, consulte [Administración de actualizaciones y revisiones para las máquinas virtuales de Azure](automation-tutorial-update-management.md).
* Para ver las configuraciones de ámbito, consulte [Uso de configuraciones de ámbito](automation-scope-configurations-update-management.md).
* Si ya no necesita el área de trabajo de Log Analytics, consulte las instrucciones en [Desvinculación de un área de trabajo de una cuenta de Automation](automation-unlink-workspace-update-management.md).
* Para eliminar máquinas virtuales de Update Management, consulte [Eliminación de una máquina virtual desde Update Management](automation-remove-vms-from-update-management.md).
* Para solucionar los errores de Update Management generales, consulte [Solución de problemas de Update Management](troubleshoot/update-management.md).
* Para solucionar problemas con el agente de Windows Update, consulte [Solución de problemas del agente de actualización de Windows](troubleshoot/update-agent-issues.md).
* Para solucionar problemas con el agente de actualización de Linux, consulte [Solución de problemas del agente de actualización de Linux](troubleshoot/update-agent-issues-linux.md).