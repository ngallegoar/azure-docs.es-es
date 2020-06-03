---
title: Habilitación de Change Tracking e Inventario de Azure Automation desde una máquina virtual de Azure
description: En este artículo se muestra cómo habilitar Change Tracking e Inventario desde una máquina virtual de Azure.
services: automation
ms.date: 03/04/2020
ms.topic: conceptual
ms.custom: mvc
ms.openlocfilehash: 332ff4f6e63a831c1523c3f959708f9dc48a72a9
ms.sourcegitcommit: 1692e86772217fcd36d34914e4fb4868d145687b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/29/2020
ms.locfileid: "84171062"
---
# <a name="enable-change-tracking-and-inventory-from-an-azure-vm"></a>Habilitación de Change Tracking e Inventario desde una máquina virtual de Azure

En este artículo se describe cómo puede usar una máquina virtual de Azure para habilitar la característica [Change Tracking e Inventario](change-tracking.md) en otras máquinas. Para habilitar máquinas virtuales de Azure a escala, debe habilitar una máquina virtual existente mediante Change Tracking e Inventario. 

> [!NOTE]
> Al habilitar Change Tracking e Inventario, solo se admite la vinculación a un área de trabajo de Log Analytics y una cuenta de Automation en regiones concretas. Para obtener una lista de los pares de asignación que se admiten, consulte [Asignación de región para la cuenta de Automation y el área de trabajo de Log Analytics](how-to/region-mappings.md).

## <a name="prerequisites"></a>Requisitos previos

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

## <a name="next-steps"></a>Pasos siguientes

* Para obtener más información sobre cómo trabajar con la característica, consulte [Administración de Change Tracking e Inventario](change-tracking-file-contents.md).
* Para solucionar problemas generales con la característica, consulte [Solución de problemas de Change Tracking e Inventario](troubleshoot/change-tracking.md).
