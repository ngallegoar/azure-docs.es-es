---
title: Habilitación de Update Management de Azure Automation desde un runbook
description: En este artículo se indica cómo habilitar Update Management desde un runbook.
services: automation
ms.topic: conceptual
ms.date: 09/30/2020
ms.custom: mvc
ms.openlocfilehash: ec102015355e3312f5dc15fa526fa543da75e0de
ms.sourcegitcommit: 8d8deb9a406165de5050522681b782fb2917762d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/20/2020
ms.locfileid: "92221837"
---
# <a name="enable-update-management-from-a-runbook"></a>Habilitación de Update Management desde un runbook

En este artículo se describe cómo puede usar un runbook para habilitar la característica [Update Management](overview.md) para las máquinas virtuales del entorno. Para habilitar VM de Azure a escala, debe habilitar una VM existente mediante Update Management.

> [!NOTE]
> Al habilitar Update Management, solo en determinadas regiones se puede vincular un área de trabajo de Log Analytics y una cuenta de Automation. Para obtener una lista de los pares de asignación que se admiten, consulte [Asignación de región para la cuenta de Automation y el área de trabajo de Log Analytics](../how-to/region-mappings.md).

Este método usa dos runbooks:

* **Enable-MultipleSolution**: el runbook principal que solicita información de configuración, consulta la VM especificada y realiza otras comprobaciones de validación y, a continuación, invoca el runbook **Enable-AutomationSolution** para configurar Update Management en cada VM del grupo de recursos especificado.
* **Enable-AutomationSolution**: habilita Update Management para una o varias VM especificadas en el grupo de recursos de destino. Comprueba que se cumplen los requisitos previos y que la extensión de VM de Log Analytics esté instalada (y la instala si no la encuentra); asimismo, agrega las VM a la configuración de ámbito en el área de trabajo de Log Analytics especificada y vinculada a la cuenta de Automation.

## <a name="prerequisites"></a>Requisitos previos

* Suscripción de Azure. Si aún no tiene ninguna, puede [activar las ventajas de la suscripción a MSDN](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) o suscribirse para obtener una [cuenta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).
* Una [cuenta de Automation](../automation-security-overview.md) para administrar máquinas.
* [Área de trabajo de Log Analytics](../../azure-monitor/platform/design-logs-deployment.md)
* Una [máquina virtual](../../virtual-machines/windows/quick-create-portal.md).
* Dos recursos de Automation que usa el runbook **Enable-AutomationSolution**. Si este runbook aún no existe en su cuenta de Automation, se importa automáticamente mediante el runbook **Enable-MultipleSolution** durante la primera ejecución.
    * *LASolutionSubscriptionId*: id. de la suscripción donde se encuentra el área de trabajo de Log Analytics.
    * *LASolutionWorkspaceId*: id. del área de trabajo de Log Analytics vinculada a su cuenta de Automation.

    Estas variables se usan para configurar el área de trabajo de la VM incorporada. Si no se especifican, el script busca primero cualquier VM incorporada en Update Management en la suscripción, seguida de la suscripción en la que se encuentra la cuenta de Automation y otras suscripciones a las que tiene acceso la cuenta de usuario. Si no se configura correctamente, las máquinas podrían incorporarse a un área de trabajo de Log Analytics aleatoria.

## <a name="sign-in-to-azure"></a>Inicio de sesión en Azure

Inicie sesión en [Azure Portal](https://portal.azure.com).

## <a name="enable-update-management"></a>Habilitación de la Administración de actualizaciones

1. En Azure Portal, vaya a las **cuentas de Automation**. En la página **Cuentas de Automation**, seleccione su cuenta en la lista.

2. En su cuenta de Automation, seleccione **Update Management** en **Update Management**.

3. Seleccione el área de trabajo de Log Analytics y, después, haga clic en **Habilitar**. Mientras se está habilitando Update Management, se muestra un banner azul.

    ![Habilitación de la Administración de actualizaciones](media/enable-from-runbook/enable-update-management.png)

## <a name="install-and-update-modules"></a>Instalar y actualizar módulos

Para habilitar de forma correcta Update Management para las VM con el runbook, es necesario actualizar a los módulos de Azure más recientes e importar el módulo [Az.OperationalInsights](/powershell/module/az.operationalinsights).

1. En la cuenta de Automation, seleccione **Módulos** en **Recursos compartidos**.

2. Seleccione **Actualizar módulos de Azure** para actualizar los módulos de Azure a la versión más reciente.

3. Haga clic en **Sí** para actualizar todos los módulos de Azure existentes a la versión más reciente.

    ![Actualización de los módulos](media/enable-from-runbook/update-modules.png)

4. Vuelva a **Módulos** en **Recursos compartidos**.

5. Seleccione **Examinar la galería** para abrir la galería de módulos.

6. Busque `Az.OperationalInsights` e importe este módulo en la cuenta de Automation.

    ![Importar el módulo OperationalInsights](media/enable-from-runbook/import-operational-insights-module.png)

## <a name="select-azure-vm-to-manage"></a>Selección de la máquina virtual de Azure que se va a administrar

Con Update Management habilitado, puede agregar una máquina virtual de Azure para recibir actualizaciones.

1. En su cuenta de Automation, seleccione **Update Management** en la sección **Update Management**.

2. Seleccione **Agregar máquinas virtuales de Azure** para agregar la máquina virtual.

3. Elija la VM de la lista y haga clic en **Habilitar** para configurar la VM para su administración.

   ![Habilitación de Update Management para la máquina virtual](media/enable-from-runbook/enable-update-management-vm.png)

    > [!NOTE]
    > Si intenta habilitar otra característica antes de que se complete la instalación de Update Management, recibirá este mensaje: `Installation of another solution is in progress on this or a different virtual machine. When that installation completes the Enable button is enabled, and you can request installation of the solution on this virtual machine.`

## <a name="import-a-runbook-to-enable-update-management"></a>Importación de un runbook para habilitar Update Management

1. En la cuenta de Automation, seleccione **Runbooks** en **Automatización de procesos**.

2. Seleccione **Explorar la galería**.

3. Busque la opción **actualizaciones y seguimiento de cambios**.

4. Seleccione el runbook y haga clic en **Importar** en la página **Ver origen**.

5. Seleccione **Aceptar** para importar el runbook en la cuenta de Automation.

   ![Importación del runbook para el programa de instalación](media/enable-from-runbook/import-from-gallery.png)

6. En la página **Runbook**, seleccione el runbook **Enable-MultipleSolution** y, a continuación, haga clic en **Editar**. En el editor de texto, seleccione **Publicar**.

7. Cuando se le pida confirmación, haga clic en **Sí** para publicar el runbook.

## <a name="start-the-runbook"></a>Inicio del runbook

Debe haber habilitado Update Management para que una máquina virtual de Azure inicie este runbook. Requiere una VM y un grupo de recursos existentes con la característica habilitada para configurar una o más VM en el grupo de recursos de destino.

1. Abra el runbook **Enable-MultipleSolution**.

   ![Runbook de varias soluciones](media/enable-from-runbook/runbook-overview.png)

2. Haga clic en el botón de inicio y escriba los valores de parámetros en los campos siguientes:

   * **VMNAME**: el nombre de una máquina virtual existente que se va a agregar a Update Management. Deje este campo en blanco para agregar todas las máquinas virtuales del grupo de recursos.
   * **VMRESOURCEGROUP**: el nombre del grupo de recursos de las máquinas virtuales que se van a habilitar.
   * **SUBSCRIPTIONID**: el identificador de la suscripción de la nueva máquina virtual que se va a habilitar. Deje este campo en blanco para usar la suscripción del área de trabajo. Si usa otro identificador de suscripción, agregue la cuenta de ejecución de la cuenta de Automation como colaborador de la suscripción.
   * **ALREADYONBOARDEDVM**: el nombre de la máquina virtual que ya está habilitada manualmente para las actualizaciones.
   * **ALREADYONBOARDEDVMRESOURCEGROUP**: el nombre del grupo de recursos al que pertenece la máquina virtual.
   * **SOLUTIONTYPE**: escriba **Actualizaciones**.

   ![Parámetros del runbook Enable-MultipleSolution](media/enable-from-runbook/runbook-parameters.png)

3. Seleccione **Aceptar** para iniciar el trabajo del runbook.

4. Supervise el progreso del trabajo del runbook y los errores de la página **Trabajos**.

## <a name="next-steps"></a>Pasos siguientes

* Para usar Update Management para máquinas virtuales, consulte [Administración de actualizaciones y revisiones para las máquinas virtuales](manage-updates-for-vm.md).

* Para solucionar los errores de Update Management generales, consulte [Solución de problemas de Update Management](../troubleshoot/update-management.md).
