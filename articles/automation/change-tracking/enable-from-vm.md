---
title: Habilitación de Change Tracking e Inventario de Azure Automation desde una máquina virtual de Azure
description: En este artículo se muestra cómo habilitar Change Tracking e Inventario desde una máquina virtual de Azure.
services: automation
ms.subservice: change-inventory-management
ms.date: 10/14/2020
ms.topic: conceptual
ms.openlocfilehash: 9b14dcb9ce2f2426d8d1496541022602a114cb6e
ms.sourcegitcommit: 957c916118f87ea3d67a60e1d72a30f48bad0db6
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/19/2020
ms.locfileid: "92209295"
---
# <a name="enable-change-tracking-and-inventory-from-an-azure-vm"></a>Habilitación de Change Tracking e Inventario desde una máquina virtual de Azure

En este artículo se describe cómo puede usar una máquina virtual de Azure para habilitar la característica [Seguimiento de cambios e inventario](overview.md) en otras máquinas. Para habilitar máquinas virtuales de Azure a escala, debe habilitar una máquina virtual existente mediante Change Tracking e Inventario.

> [!NOTE]
> Al habilitar Change Tracking e Inventario, solo se admite la vinculación a un área de trabajo de Log Analytics y una cuenta de Automation en regiones concretas. Para obtener una lista de los pares de asignación que se admiten, consulte [Asignación de región para la cuenta de Automation y el área de trabajo de Log Analytics](../how-to/region-mappings.md).

## <a name="prerequisites"></a>Requisitos previos

* Suscripción de Azure. Si aún no tiene ninguna, puede [activar las ventajas de la suscripción a MSDN](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) o suscribirse para obtener una [cuenta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).
* Una [cuenta de Automation](../index.yml) para administrar máquinas.
* Una [máquina virtual](../../virtual-machines/windows/quick-create-portal.md).

## <a name="sign-in-to-azure"></a>Inicio de sesión en Azure

Inicie sesión en Azure Portal en https://portal.azure.com.

## <a name="enable-change-tracking-and-inventory"></a>Habilitación de Change Tracking e Inventario

1. En [Azure Portal](https://portal.azure.com), seleccione **Máquinas virtuales** o busque y seleccione **Máquinas virtuales** en la página principal.

2. Seleccione la máquina virtual para la que desea habilitar Change Tracking e inventario. Las máquina virtuales pueden existir en cualquier región, independientemente de la ubicación de la cuenta de Automation.

3. En la página de máquina virtual seleccione **Inventario** o **Change Tracking** en **Administración de configuración** .

4. Debe tener el permiso `Microsoft.OperationalInsights/workspaces/read` para determinar si la máquina virtual está habilitada para un área de trabajo. Para obtener información acerca de los permisos adicionales que se requieren, vea [Permisos de configuración de características](../automation-role-based-access-control.md#feature-setup-permissions). Para obtener información sobre cómo habilitar varias máquinas a la vez, vea [Habilitación de Change Tracking e Inventario desde una cuenta de Automation](enable-from-automation-account.md).

5. Para habilitar Change Tracking e Inventario en la máquina virtual, elija el área de trabajo de Log Analytics y la cuenta de Automation, y haga clic en **Habilitar** . La instalación tarda hasta 15 minutos en completarse.

## <a name="next-steps"></a>Pasos siguientes

* Para más información sobre cómo trabajar con la característica, consulte [Administración de Change Tracking](manage-change-tracking.md) y [Administración del inventario](manage-inventory-vms.md).

* Para solucionar problemas generales con la característica, consulte [Solución de problemas de Change Tracking e Inventario](../troubleshoot/change-tracking.md).