---
title: Habilitación de Change Tracking e Inventario de Azure Automation desde una cuenta de Automation
description: En este artículo se muestra cómo habilitar Change Tracking e Inventario desde una cuenta de Automation.
services: automation
ms.date: 4/11/2019
ms.topic: conceptual
ms.custom: mvc
ms.openlocfilehash: 302e0cc9b77605afd4012e95fc7c0ab8a22fef3e
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "86186323"
---
# <a name="enable-change-tracking-and-inventory-from-an-automation-account"></a>Habilitación de Change Tracking e Inventario desde una cuenta de Automation

En este artículo se describe cómo puede usar la cuenta de Automation para habilitar la característica [Change Tracking e Inventario](change-tracking.md) para las máquinas virtuales del entorno. Para habilitar máquinas virtuales de Azure a escala, debe habilitar una máquina virtual existente mediante Change Tracking e Inventario. 

> [!NOTE]
> Al habilitar Change Tracking e Inventario, solo se admite la vinculación a un área de trabajo de Log Analytics y una cuenta de Automation en regiones concretas. Para obtener una lista de los pares de asignación que se admiten, consulte [Asignación de región para la cuenta de Automation y el área de trabajo de Log Analytics](how-to/region-mappings.md).

## <a name="prerequisites"></a>Requisitos previos

* Suscripción de Azure. Si aún no tiene ninguna, puede [activar las ventajas de la suscripción a MSDN](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) o suscribirse para obtener una [cuenta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).
* Una [cuenta de Automation](./index.yml) para administrar máquinas.
* Una [máquina virtual](../virtual-machines/windows/quick-create-portal.md).

## <a name="sign-in-to-azure"></a>Inicio de sesión en Azure

Inicie sesión en Azure en https://portal.azure.com.

## <a name="enable-change-tracking-and-inventory"></a>Habilitación de Change Tracking e Inventario

1. Vaya a la cuenta de Automation y seleccione **Inventory** o **Change Tracking** en **Administración de configuración**.

2. Para habilitar Change Tracking e Inventario, elija el área de trabajo de Log Analytics y la cuenta de Automation, y haga clic en **Habilitar**. La instalación tarda hasta 15 minutos en completarse.

    ![Habilitación de Change Tracking e Inventario](media/automation-enable-changes-from-auto-acct/onboardsolutions.png)

## <a name="enable-azure-vms"></a>Habilitar máquinas virtuales de Azure

1. En la cuenta de Automation, seleccione **Inventario** o **Change Tracking** en **Administración de configuración**.

2. Haga clic en **+ Agregar máquinas virtuales de Azure** y seleccione una o varias máquinas virtuales en la lista. Las máquinas virtuales que no se pueden habilitar aparecen atenuadas y no se pueden seleccionar. Las VM de Azure pueden existir en cualquier región, con independencia de la ubicación de la cuenta de Automation. 

3. Haga clic en **Habilitar** para agrega las máquinas virtuales seleccionadas a la búsqueda guardada de los grupos de equipos para la característica. Para obtener más información, consulte [Limitación del ámbito de implementación de Change Tracking e Inventario](automation-scope-configurations-change-tracking.md).

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

5. Si es necesario, puede agregar de nuevo las configuraciones de ámbito si vuelve a agregar las búsquedas guardadas iniciales. Para obtener más información, consulte [Limitación del ámbito de implementación de Change Tracking e Inventario](automation-scope-configurations-change-tracking.md).

6. Para habilitar la característica para una o varias máquinas, seleccione **Habilitar en las máquinas seleccionadas** y haga clic en **Agregar** junto a cada máquina para habilitar la característica. Esta tarea agrega los nombres de máquina seleccionados a la consulta de la búsqueda guardada de grupos de equipos para la característica.

## <a name="next-steps"></a>Pasos siguientes

* Para obtener más información sobre la característica, consulte [Administración de Change Tracking e Inventario](change-tracking-file-contents.md).
* Para solucionar problemas generales con la característica, consulte [Solución de problemas de Change Tracking e Inventario](troubleshoot/change-tracking.md).
