---
title: Habilitación de Change Tracking e Inventario de Azure Automation desde una cuenta de Automation
description: En este artículo se muestra cómo habilitar Change Tracking e Inventario desde una cuenta de Automation.
services: automation
ms.date: 4/11/2019
ms.topic: conceptual
ms.custom: mvc
ms.openlocfilehash: 28c61e637a37c158dcd80c0f02b748b4813945fb
ms.sourcegitcommit: 0b80a5802343ea769a91f91a8cdbdf1b67a932d3
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/25/2020
ms.locfileid: "83826849"
---
# <a name="enable-change-tracking-and-inventory-from-an-automation-account"></a>Habilitación de Change Tracking e Inventario desde una cuenta de Automation

En este artículo se describe cómo puede usar la cuenta de Automation para habilitar la característica [Change Tracking e Inventario](change-tracking.md) para las máquinas virtuales del entorno. Para habilitar máquinas virtuales de Azure a escala, debe habilitar una máquina virtual existente mediante Change Tracking e Inventario. 

> [!NOTE]
> Al habilitar Change Tracking e Inventario, solo se admite la vinculación a un área de trabajo de Log Analytics y una cuenta de Automation en regiones concretas. Para obtener una lista de los pares de asignación que se admiten, consulte [Asignación de región para la cuenta de Automation y el área de trabajo de Log Analytics](how-to/region-mappings.md).

## <a name="prerequisites"></a>Prerrequisitos

* Suscripción de Azure. Si aún no tiene ninguna, puede [activar las ventajas de la suscripción a MSDN](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) o suscribirse para obtener una [cuenta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).
* Una [cuenta de Automation](automation-offering-get-started.md) para administrar máquinas.
* Una [máquina virtual](../virtual-machines/windows/quick-create-portal.md).

## <a name="sign-in-to-azure"></a>Inicio de sesión en Azure

Inicie sesión en Azure en https://portal.azure.com.

## <a name="enable-change-tracking-and-inventory"></a>Habilitación de Change Tracking e Inventario

1. Vaya a la cuenta de Automation y seleccione **Inventory** o **Change Tracking** en **Administración de configuración**.

2. Para habilitar Change Tracking e Inventario, elija el área de trabajo de Log Analytics y la cuenta de Automation, y haga clic en **Habilitar**. La instalación tarda hasta 15 minutos en completarse.

    ![Habilitación de Change Tracking e Inventario](media/automation-enable-changes-from-auto-acct/onboardsolutions.png)

## <a name="check-the-scope-configuration"></a><a name="scope-configuration"></a>Comprobación de la configuración de ámbito

Change Tracking e Inventario usa una configuración de ámbito en el área de trabajo para definir los equipos de destino que reciben los cambios. La configuración de ámbito es un grupo de una o más búsquedas guardadas que se usa para limitar el ámbito de la característica a equipos concretos. Para obtener más información, vea [Uso de configuraciones de ámbito para Change Tracking e Inventario](automation-scope-configurations-change-tracking.md).

## <a name="enable-azure-vms"></a>Habilitar máquinas virtuales de Azure

1. En la cuenta de Automation, seleccione **Inventario** o **Change Tracking** en **Administración de configuración**.

2. Haga clic en **+ Agregar máquinas virtuales de Azure** y seleccione una o varias máquinas virtuales en la lista. Las máquinas virtuales que no se pueden habilitar aparecen atenuadas y no se pueden seleccionar. Las VM de Azure pueden existir en cualquier región, con independencia de la ubicación de la cuenta de Automation. 

3. Haga clic en **Habilitar** para agrega las máquinas virtuales seleccionadas a la búsqueda guardada de los grupos de equipos para la característica. Para obtener más información, vea [Uso de configuraciones de ámbito para Change Tracking e Inventario](automation-scope-configurations-change-tracking.md).

    ![Habilitar máquinas virtuales de Azure](media/automation-enable-changes-from-auto-acct/enable-azure-vms.png)

## <a name="enable-non-azure-vms"></a>Habilitación de máquinas virtuales que no son de Azure

Las máquinas que no estén en Azure se deben agregar manualmente. 

1. Desde la cuenta de Automation, seleccione **Inventario** o **Change Tracking** en **Administración de configuración**.

2. Haga clic en **Agregar un equipo que no es de Azure**. Esta acción abre una nueva ventana del explorador con las [instrucciones para instalar y configurar el agente de Log Analytics para Windows](../azure-monitor/platform/log-analytics-agent.md) de forma que la máquina pueda empezar a notificar operaciones de Change Tracking e Inventario. Si va a habilitar una máquina administrada actualmente por Operations Manager, no se necesita un agente nuevo y la información del área de trabajo se escribe en el agente existente.

## <a name="enable-machines-in-the-workspace"></a>Habilitación de máquinas en el área de trabajo

Las máquinas instaladas manualmente o las que ya envían notificaciones al área de trabajo se deben agregar a Azure Automation para poder habilitar Change Tracking e Inventario. 

1. En la cuenta de Automation, seleccione **Inventario** o **Change Tracking** en **Administración de configuración**.

2. Seleccione **Administrar máquinas**. Es posible que, si previamente ha seleccionado la opción **Habilitar en todas las máquinas disponibles y futuras**, el botón **Administrar máquinas** esté atenuado.

    ![Búsquedas guardadas](media/automation-enable-changes-from-auto-acct/managemachines.png)

3. Para habilitar Change Tracking e Inventario en todas las máquinas disponibles, seleccione **Habilitar en todas las máquinas disponibles** en la página Administrar máquinas. Esta operación deshabilita el control que permite agregar máquinas una a una. Esta tarea agrega todos los nombres de las máquinas que informan al área de trabajo en la consulta de la búsqueda guardada de grupos de equipos. Cuando se selecciona, esta acción deshabilita el botón **Administrar máquinas**.

4. Para habilitar la característica en todas las máquinas disponibles y futuras, seleccione **Habilitar en todas las máquinas disponibles y futuras**. Esta opción elimina las búsquedas guardadas y las configuraciones de ámbito del área de trabajo y abre la característica para todas las máquinas de Azure y que no son de Azure que envían notificaciones al área de trabajo. Cuando está seleccionada, esta acción deshabilita permanentemente el botón **Administrar máquinas**, ya que no queda ninguna configuración de ámbito.

5. Si es necesario, puede agregar de nuevo las configuraciones de ámbito si vuelve a agregar las búsquedas guardadas iniciales. Para obtener más información, vea [Uso de configuraciones de ámbito para Change Tracking e Inventario](automation-scope-configurations-change-tracking.md).

6. Para habilitar la característica para una o varias máquinas, seleccione **Habilitar en las máquinas seleccionadas** y haga clic en **Agregar** junto a cada máquina para habilitar la característica. Esta tarea agrega los nombres de máquina seleccionados a la consulta de la búsqueda guardada de grupos de equipos para la característica.

## <a name="next-steps"></a>Pasos siguientes

* Para obtener más información sobre la característica, vea [Administración de Change Tracking e Inventario](change-tracking-file-contents.md).
* Para obtener información sobre las configuraciones de ámbito, vea [Uso de configuraciones de ámbito para Change Tracking e Inventario](automation-scope-configurations-change-tracking.md).
* Para obtener información sobre cómo usar la característica para identificar el software instalado en el entorno, vea [Detección del software instalado en las máquinas virtuales](automation-tutorial-installed-software.md).
* Si no quiere integrar la cuenta de Automation con un área de trabajo Log Analytics al habilitar la característica, vea [Desvinculación de un área de trabajo de una cuenta de Automation](automation-unlink-workspace-change-tracking.md).
* Cuando haya terminado de implementar los cambios en las máquinas virtuales, puede quitarlas como se describe en [Eliminación de máquinas virtuales de Change Tracking e Inventario](automation-remove-vms-from-change-tracking.md).
* Para solucionar problemas generales con la característica, consulte [Solución de problemas de Change Tracking e Inventario](troubleshoot/change-tracking.md).