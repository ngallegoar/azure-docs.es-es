---
title: Habilitación de Change Tracking e Inventario de Azure Automation desde un runbook
description: En este artículo se muestra cómo habilitar Change Tracking e Inventario desde un runbook.
services: automation
ms.subservice: change-inventory-management
ms.topic: conceptual
ms.date: 10/14/2020
ms.openlocfilehash: 842b0a92ba4a2cb6b3ceb54675ef95f9c8275311
ms.sourcegitcommit: 957c916118f87ea3d67a60e1d72a30f48bad0db6
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/19/2020
ms.locfileid: "92209313"
---
# <a name="enable-change-tracking-and-inventory-from-a-runbook"></a>Habilitación de Change Tracking e Inventario desde un runbook

En este artículo se describe cómo puede usar un runbook para habilitar [Seguimiento de cambios e inventario](overview.md) para las máquinas virtuales del entorno. Para habilitar máquinas virtuales de Azure a escala, debe habilitar una máquina virtual existente mediante Change Tracking e Inventario.

> [!NOTE]
> Al habilitar Change Tracking e Inventario, solo se admite la vinculación a un área de trabajo de Log Analytics y una cuenta de Automation en regiones concretas. Para obtener una lista de los pares de asignación que se admiten, consulte [Asignación de región para la cuenta de Automation y el área de trabajo de Log Analytics](../how-to/region-mappings.md).

Este método usa dos runbooks:

* **Enable-MultipleSolution** : el runbook principal que solicita información de configuración, consulta la VM especificada y realiza otras comprobaciones de validación y, luego, invoca el runbook **Enable-AutomationSolution** para configurar Seguimiento de cambios e inventario en cada VM del grupo de recursos especificado.
* **Enable-AutomationSolution** : habilita Seguimiento de cambios e inventario para una o varias máquinas virtuales especificadas en el grupo de recursos de destino. Comprueba que se cumplen los requisitos previos y que la extensión de VM de Log Analytics esté instalada (y la instala si no la encuentra); asimismo, agrega las VM a la configuración de ámbito en el área de trabajo de Log Analytics especificada y vinculada a la cuenta de Automation.

## <a name="prerequisites"></a>Requisitos previos

* Suscripción de Azure. Si aún no tiene ninguna, puede [activar las ventajas de la suscripción a MSDN](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) o suscribirse para obtener una [cuenta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).
* Una [cuenta de Automation](../automation-security-overview.md) para administrar máquinas.
* [Área de trabajo de Log Analytics](../../azure-monitor/platform/design-logs-deployment.md)
* Una [máquina virtual](../../virtual-machines/windows/quick-create-portal.md).
* Dos recursos de Automation que usa el runbook **Enable-AutomationSolution** . Si este runbook aún no existe en su cuenta de Automation, se importa automáticamente mediante el runbook **Enable-MultipleSolution** durante la primera ejecución.
    * *LASolutionSubscriptionId* : id. de la suscripción donde se encuentra el área de trabajo de Log Analytics.
    * *LASolutionWorkspaceId* : id. del área de trabajo de Log Analytics vinculada a su cuenta de Automation.

    Estas variables se usan para configurar el área de trabajo de la VM incorporada. Si no se especifican, el script busca primero cualquier máquina virtual incorporada en Seguimiento de cambios e inventario en su suscripción, seguida de la suscripción en la que se encuentra la cuenta de Automation y otras suscripciones a las que tiene acceso la cuenta de usuario. Si no se configura correctamente, las máquinas podrían incorporarse a un área de trabajo de Log Analytics aleatoria.

## <a name="sign-in-to-azure"></a>Inicio de sesión en Azure

Inicie sesión en [Azure Portal](https://portal.azure.com).

## <a name="enable-change-tracking-and-inventory"></a>Habilitación de Change Tracking e Inventario

1. En Azure Portal, vaya a las **cuentas de Automation** . En la página **Cuentas de Automation** , seleccione su cuenta en la lista.

1. En la cuenta de Automation, seleccione **Inventario** o **Change Tracking** en **Administración de configuración** .

1. Seleccione el área de trabajo de Log Analytics y, después, haga clic en **Habilitar** . Mientras se habilita Inventario o Change Tracking, se muestra un banner.

    ![Habilitación de Change Tracking e Inventario](media/enable-from-automation-account/enable-feature.png)

## <a name="install-and-update-modules"></a>Instalar y actualizar módulos

Para habilitar de forma correcta Update Management para las VM con el runbook, es necesario actualizar a los módulos de Azure más recientes e importar el módulo [Az.OperationalInsights](/powershell/module/az.operationalinsights).

1. En la cuenta de Automation, seleccione **Módulos** en **Recursos compartidos** .

2. Seleccione **Actualizar módulos de Azure** para actualizar los módulos de Azure a la versión más reciente.

3. Haga clic en **Sí** para actualizar todos los módulos de Azure existentes a la versión más reciente.

    ![Actualización de los módulos](media/enable-from-runbook/update-modules.png)

4. Vuelva a **Módulos** en **Recursos compartidos** .

5. Seleccione **Examinar la galería** para abrir la galería de módulos.

6. Busque `Az.OperationalInsights` e importe este módulo en la cuenta de Automation.

    ![Importar el módulo OperationalInsights](media/enable-from-runbook/import-operational-insights-module.png)

## <a name="select-azure-vm-to-manage"></a>Selección de la máquina virtual de Azure que se va a administrar

Con Change Tracking e Inventario habilitado, puede agregar una máquina virtual de Azure para la administración mediante la característica.

1. Desde la cuenta de Automation, seleccione **Change Tracking** o **Inventario** en **Administración de configuración** .

2. Haga clic en **Agregar máquinas virtuales de Azure** para agregar la máquina virtual.

3. Elija la máquina virtual en la lista y haga clic en **Habilitar** . Esta acción habilita Change Tracking e Inventario para la máquina virtual.

   ![Habilitación de Change Tracking e Inventario para la máquina virtual](media/enable-from-runbook/enable-azure-vm.png)

    > [!NOTE]
    > Si intenta habilitar otra característica antes de que se complete la instalación de Change Tracking e Inventario, recibirá este mensaje: `Installation of another solution is in progress on this or a different virtual machine. When that installation completes the Enable button is enabled, and you can request installation of the solution on this virtual machine.`

## <a name="import-a-runbook-to-enable-change-tracking-and-inventory"></a>Importación de un runbook para habilitar Change Tracking e Inventario

1. En la cuenta de Automation, seleccione **Runbooks** en **Automatización de procesos** .

2. Seleccione **Explorar la galería** .

3. Busque la opción **actualizaciones y seguimiento de cambios** .

4. Seleccione el runbook y haga clic en **Importar** en la página **Ver origen** .

5. Seleccione **Aceptar** para importar el runbook en la cuenta de Automation.

   ![Importación del runbook para el programa de instalación](media/enable-from-runbook/import-from-gallery.png)

6. En la página **Runbook** , seleccione el runbook **Enable-MultipleSolution** y, a continuación, haga clic en **Editar** . En el editor de texto, seleccione **Publicar** .

7. Cuando se le pida confirmación, haga clic en **Sí** para publicar el runbook.

## <a name="start-the-runbook"></a>Inicio del runbook

Debe haber habilitado Change Tracking e Inventario para que una máquina virtual de Azure inicie este runbook. Requiere una VM y un grupo de recursos existentes con la característica habilitada para configurar una o más VM en el grupo de recursos de destino.

1. Abra el runbook **Enable-MultipleSolution** .

   ![Runbook de varias soluciones](media/enable-from-runbook/runbook-overview.png)

2. Haga clic en el botón de inicio y escriba los valores de parámetros en los campos siguientes:

   * **VMNAME** : el nombre de una máquina virtual existente que se va a agregar a Change Tracking e Inventario. Deje este campo en blanco para agregar todas las máquinas virtuales del grupo de recursos.
   * **VMRESOURCEGROUP** : el nombre del grupo de recursos de las máquinas virtuales que se van a habilitar.
   * **SUBSCRIPTIONID** : el identificador de la suscripción de la nueva máquina virtual que se va a habilitar. Deje este campo en blanco para usar la suscripción del área de trabajo. Si usa otro identificador de suscripción, agregue la cuenta de ejecución de la cuenta de Automation como colaborador de la suscripción.
   * **ALREADYONBOARDEDVM** : el nombre de la máquina virtual que ya está habilitada manualmente para las actualizaciones.
   * **ALREADYONBOARDEDVMRESOURCEGROUP** : el nombre del grupo de recursos al que pertenece la máquina virtual.
   * **SOLUTIONTYPE** : escriba **ChangeTracking** .

   ![Parámetros del runbook Enable-MultipleSolution](media/enable-from-runbook/runbook-parameters.png)

3. Seleccione **Aceptar** para iniciar el trabajo del runbook.

4. Supervise el progreso del trabajo del runbook y los errores de la página **Trabajos** .

## <a name="next-steps"></a>Pasos siguientes

* Para programar un runbook, consulte [Administración de programaciones en Azure Automation](../shared-resources/schedules.md).

* Para más información sobre cómo trabajar con la característica, consulte [Administración de Change Tracking](manage-change-tracking.md) y [Administración del Inventario](manage-inventory-vms.md).

* Para solucionar problemas generales con la característica, consulte [Solución de problemas de Change Tracking e Inventario](../troubleshoot/change-tracking.md).


