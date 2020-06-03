---
title: Habilitación de Change Tracking e Inventario de Azure Automation desde una máquina virtual de Azure
description: En este artículo se muestra cómo habilitar Change Tracking e Inventario desde una máquina virtual de Azure.
services: automation
ms.date: 03/04/2020
ms.topic: conceptual
ms.custom: mvc
ms.openlocfilehash: 5379f2c46bbeaba4ee8509603b7b739b75d08f04
ms.sourcegitcommit: 0b80a5802343ea769a91f91a8cdbdf1b67a932d3
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/25/2020
ms.locfileid: "83836794"
---
# <a name="enable-change-tracking-and-inventory-from-an-azure-vm"></a>Habilitación de Change Tracking e Inventario desde una máquina virtual de Azure

En este artículo se describe cómo puede usar una máquina virtual de Azure para habilitar la característica [Change Tracking e Inventario](change-tracking.md) en otras máquinas. Para habilitar máquinas virtuales de Azure a escala, debe habilitar una máquina virtual existente mediante Change Tracking e Inventario. 

> [!NOTE]
> Al habilitar Change Tracking e Inventario, solo se admite la vinculación a un área de trabajo de Log Analytics y una cuenta de Automation en regiones concretas. Para obtener una lista de los pares de asignación que se admiten, consulte [Asignación de región para la cuenta de Automation y el área de trabajo de Log Analytics](how-to/region-mappings.md).

## <a name="prerequisites"></a>Prerrequisitos

* Suscripción de Azure. Si aún no tiene ninguna, puede [activar las ventajas de la suscripción a MSDN](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) o suscribirse para obtener una [cuenta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).
* Una [cuenta de Automation](automation-offering-get-started.md) para administrar máquinas.
* Una [máquina virtual](../virtual-machines/windows/quick-create-portal.md).

## <a name="sign-in-to-azure"></a>Inicio de sesión en Azure

Inicie sesión en Azure Portal en https://portal.azure.com.

## <a name="enable-change-tracking-and-inventory"></a>Habilitación de Change Tracking e Inventario

1. En [Azure Portal](https://portal.azure.com), seleccione **Máquinas virtuales** o busque y seleccione **Máquinas virtuales** en la página principal.

2. Seleccione la máquina virtual para la que desea habilitar Change Tracking e inventario. Las máquina virtuales pueden existir en cualquier región, independientemente de la ubicación de la cuenta de Automation.

3. En la página de máquina virtual seleccione **Inventario** o **Change Tracking** en **Administración de configuración**.

4. Debe tener el permiso `Microsoft.OperationalInsights/workspaces/read` para determinar si la máquina virtual está habilitada para un área de trabajo. Para obtener información acerca de los permisos adicionales que se requieren, vea [Permisos de configuración de características](automation-role-based-access-control.md#feature-setup-permissions). Para obtener información sobre cómo habilitar varias máquinas a la vez, vea [Habilitación de Change Tracking e Inventario desde una cuenta de Automation](automation-enable-changes-from-auto-acct.md).

5. Para habilitar Change Tracking e Inventario en la máquina virtual, elija el área de trabajo de Log Analytics y la cuenta de Automation, y haga clic en **Habilitar**. La instalación tarda hasta 15 minutos en completarse. 

## <a name="check-the-scope-configuration"></a><a name="scope-configuration"></a>Comprobación de la configuración de ámbito

Change Tracking e Inventario usa una configuración de ámbito en el área de trabajo para definir los equipos y así habilitar la función. La configuración de ámbito es un grupo de una o más búsquedas guardadas que se usa para limitar el ámbito de la característica a equipos concretos. Para obtener más información, vea [Uso de configuraciones de ámbito para Change Tracking e Inventario](automation-scope-configurations-change-tracking.md).

## <a name="next-steps"></a>Pasos siguientes

* Para más información sobre cómo trabajar con la característica, vea [Administración de Change Tracking e Inventario](change-tracking-file-contents.md).
* Para obtener información sobre las configuraciones de ámbito, vea [Uso de configuraciones de ámbito para Change Tracking e Inventario](automation-scope-configurations-change-tracking.md).
* Para obtener información sobre cómo usar la característica para identificar el software instalado en el entorno, vea [Detección del software instalado en las máquinas virtuales](automation-tutorial-installed-software.md).
* Si no quiere integrar la cuenta de Automation con un área de trabajo Log Analytics al habilitar la característica, vea [Desvinculación de un área de trabajo de una cuenta de Automation](automation-unlink-workspace-change-tracking.md).
* Cuando haya terminado de implementar los cambios en las máquinas virtuales, puede quitarlas como se describe en [Eliminación de máquinas virtuales de Change Tracking e Inventario](automation-remove-vms-from-change-tracking.md).
* Para solucionar problemas generales con la característica, vea [Solución de problemas de Change Tracking e Inventario](troubleshoot/change-tracking.md).
