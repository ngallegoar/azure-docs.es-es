---
title: Habilitación de Update Management de Azure Automation desde una cuenta de Automation
description: En este artículo se indica cómo habilitar Update Management desde una cuenta de Automation.
services: automation
ms.date: 4/11/2019
ms.topic: conceptual
ms.custom: mvc
ms.openlocfilehash: a521ff690f59b6beafd1113b177b43193dc7447e
ms.sourcegitcommit: 493b27fbfd7917c3823a1e4c313d07331d1b732f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/21/2020
ms.locfileid: "83743988"
---
# <a name="enable-update-management-from-an-automation-account"></a>Habilitación de Update Management desde una cuenta de Automation

En este artículo se describe cómo puede usar su cuenta de Automation para habilitar la característica [Update Management](automation-update-management.md) para las máquinas virtuales del entorno. Para habilitar máquinas virtuales de Azure a escala, debe habilitar una máquina virtual existente mediante Update Management. 

> [!NOTE]
> Al habilitar Update Management, solo en determinadas regiones se puede vincular un área de trabajo de Log Analytics y una cuenta de Automation. Para obtener una lista de los pares de asignación que se admiten, consulte [Asignación de región para la cuenta de Automation y el área de trabajo de Log Analytics](how-to/region-mappings.md).

## <a name="prerequisites"></a>Prerrequisitos

* Suscripción de Azure. Si aún no tiene ninguna, puede [activar las ventajas de la suscripción a MSDN](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) o suscribirse para obtener una [cuenta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).
* Una [cuenta de Automation](automation-offering-get-started.md) para administrar máquinas.
* Una [máquina virtual](../virtual-machines/windows/quick-create-portal.md).

## <a name="sign-in-to-azure"></a>Inicio de sesión en Azure

Inicie sesión en Azure en https://portal.azure.com.

## <a name="enable-update-management"></a>Habilitación de la Administración de actualizaciones

1. En su cuenta de Automation, seleccione **Update Management** en **Update Management**.

2. Para habilitar Update Management, elija el área de trabajo de Log Analytics y la cuenta de Automation y haga clic en **Habilitar**. La instalación tarda hasta 15 minutos en completarse.

    ![Habilitación de la Administración de actualizaciones](media/automation-onboard-solutions-from-automation-account/onboardsolutions2.png)

## <a name="check-the-scope-configuration"></a><a name="scope-configuration"></a>Comprobación de la configuración de ámbito

Update Management usa una configuración de ámbito dentro del área de trabajo para seleccionar como destino los equipos a fin de habilitar la característica. La configuración de ámbito es un grupo de una o varias búsquedas guardadas que se usa para limitar el ámbito de la característica a equipos concretos. Para más información, vea [Uso de configuraciones de ámbito para Update Management](automation-scope-configurations-update-management.md).

## <a name="enable-azure-vms"></a>Habilitar máquinas virtuales de Azure

1. En su cuenta de Automation, seleccione **Update Management** en **Update Management**.

2. Haga clic en **+ Agregar máquinas virtuales de Azure** y seleccione una o varias máquinas virtuales en la lista. Las máquinas virtuales que no se pueden habilitar aparecen atenuadas y no se pueden seleccionar. Las VM de Azure pueden existir en cualquier región, con independencia de la ubicación de la cuenta de Automation. 

3. Haga clic en **Habilitar** para agrega las máquinas virtuales seleccionadas a la búsqueda guardada de los grupos de equipos para la característica.

    ![Habilitar máquinas virtuales de Azure](media/automation-onboard-solutions-from-automation-account/enable-azure-vms.png)

## <a name="enable-non-azure-vms"></a>Habilitación de máquinas virtuales que no son de Azure

Las máquinas que no estén en Azure se deben agregar manualmente. 

1. En su cuenta de Automation, seleccione **Update Management** en **Update Management**.

2. Haga clic en **Agregar un equipo que no es de Azure**. Esta acción abre una nueva ventana del explorador con las [instrucciones para instalar y configurar el agente de Log Analytics para Windows](../azure-monitor/platform/log-analytics-agent.md), para que la máquina pueda empezar a enviar notificaciones sobre las operaciones de Update Management. Si va a habilitar una máquina administrada actualmente por Operations Manager, no se necesita un agente nuevo y la información del área de trabajo se escribe en el agente existente.

## <a name="enable-machines-in-the-workspace"></a>Habilitación de máquinas en el área de trabajo

Las máquinas instaladas manualmente o las máquinas que ya envían notificaciones al área de trabajo deben agregarse a Azure Automation para poder habilitar Update Management. 

1. En su cuenta de Automation, seleccione **Update Management** en **Update Management**.

2. Seleccione **Administrar máquinas**. Es posible que, si previamente ha seleccionado la opción **Habilitar en todas las máquinas disponibles y futuras**, el botón **Administrar máquinas** esté atenuado.

    ![Búsquedas guardadas](media/automation-onboard-solutions-from-automation-account/managemachines.png)

4. Para habilitar Update Management en todas las máquinas disponibles, seleccione **Habilitar en todas las máquinas disponibles** en la página Administrar máquinas. Esta operación deshabilita el control que permite agregar máquinas una a una. Esta tarea agrega todos los nombres de las máquinas que informan al área de trabajo en la consulta de la búsqueda guardada de grupos de equipos. Cuando se selecciona, esta acción deshabilita el botón **Administrar máquinas**.

5. Para habilitar la característica en todas las máquinas disponibles y futuras, seleccione **Habilitar en todas las máquinas disponibles y futuras**. Esta opción elimina las búsquedas guardadas y las configuraciones de ámbito del área de trabajo y abre la característica para todas las máquinas de Azure y que no son de Azure que envían notificaciones al área de trabajo. Cuando está seleccionada, esta acción deshabilita permanentemente el botón **Administrar máquinas**, ya que no queda ninguna configuración de ámbito.

6. Si es necesario, puede agregar de nuevo las configuraciones de ámbito si vuelve a agregar las búsquedas guardadas iniciales. Para más información, vea [Uso de configuraciones de ámbito para Update Management](automation-scope-configurations-update-management.md).

7. Para habilitar la característica para una o varias máquinas, seleccione **Habilitar en las máquinas seleccionadas** y haga clic en **Agregar** junto a cada máquina para habilitar la característica. Esta tarea agrega los nombres de máquina seleccionados a la consulta de la búsqueda guardada de grupos de equipos para la característica.

## <a name="next-steps"></a>Pasos siguientes

* Para usar Update Management para máquinas virtuales, vea [Administración de actualizaciones y revisiones para las máquinas virtuales de Azure](automation-tutorial-update-management.md).
* Para ver las configuraciones de ámbito, consulte [Uso de configuraciones de ámbito para Update Management](automation-scope-configurations-update-management.md).
* Si ya no necesita el área de trabajo de Log Analytics, vea las instrucciones en [Desvinculación de un área de trabajo de una cuenta de Automation para Update Management](automation-unlink-workspace-update-management.md).
* Para eliminar máquinas virtuales de Update Management, vea [Eliminación de una máquina virtual desde Update Management](automation-remove-vms-from-update-management.md).
* Para solucionar los errores de Update Management generales, vea [Solución de problemas de Update Management](troubleshoot/update-management.md).
* Para solucionar problemas con el agente de Windows Update, vea [Solución de problemas del agente de actualización de Windows](troubleshoot/update-agent-issues.md).
* Para solucionar problemas con el agente de actualización de Linux, vea [Solución de problemas del agente de actualización de Linux](troubleshoot/update-agent-issues-linux.md).
