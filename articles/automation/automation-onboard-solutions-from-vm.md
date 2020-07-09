---
title: Habilitación de Update Management de Azure Automation desde una máquina virtual de Azure
description: En este artículo se indica cómo habilitar Update Management desde una máquina virtual de Azure.
services: automation
ms.date: 03/04/2020
ms.topic: conceptual
ms.custom: mvc
ms.openlocfilehash: a1fe48b8bc776faf98f71eb882720e8180a2c573
ms.sourcegitcommit: ec682dcc0a67eabe4bfe242fce4a7019f0a8c405
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/09/2020
ms.locfileid: "86186034"
---
# <a name="enable-update-management-from-an-azure-vm"></a>Habilitación de Update Management desde una maquina virtual de Azure

En este artículo se describe cómo puede usar una máquina virtual de Azure para habilitar la característica [Update Management](automation-update-management.md) en otras máquinas. Para habilitar máquinas virtuales de Azure a escala, debe habilitar una máquina virtual existente mediante Update Management. 

> [!NOTE]
> Al habilitar Update Management, solo en determinadas regiones se puede vincular un área de trabajo de Log Analytics y una cuenta de Automation. Para obtener una lista de los pares de asignación que se admiten, consulte [Asignación de región para la cuenta de Automation y el área de trabajo de Log Analytics](how-to/region-mappings.md).

## <a name="prerequisites"></a>Requisitos previos

* Suscripción de Azure. Si aún no tiene ninguna, puede [activar las ventajas de la suscripción a MSDN](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) o suscribirse para obtener una [cuenta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).
* Una [cuenta de Automation](./index.yml) para administrar máquinas.
* Una [máquina virtual](../virtual-machines/windows/quick-create-portal.md).

## <a name="sign-in-to-azure"></a>Inicio de sesión en Azure

Inicie sesión en Azure Portal en https://portal.azure.com.

## <a name="enable-the-feature-for-deployment"></a>Habilitación de la característica de implementación

1. En [Azure Portal](https://portal.azure.com), seleccione **Máquinas virtuales** o busque y seleccione **Máquinas virtuales** en la página principal.

2. Seleccione la máquina virtual para la que desea habilitar Update Management. Las máquina virtuales pueden existir en cualquier región, independientemente de la ubicación de la cuenta de Automation. Los

3. En la página de la máquina virtual, en **Operaciones**, seleccione **Update Management**.

4. Debe tener el permiso `Microsoft.OperationalInsights/workspaces/read` para determinar si la máquina virtual está habilitada para un área de trabajo. Para obtener información sobre los permisos adicionales requeridos, vea los [permisos necesarios para habilitar máquinas](automation-role-based-access-control.md#feature-setup-permissions). Para obtener información sobre cómo habilitar varias máquinas a la vez, vea [Habilitación de Update Management desde una cuenta de Automation](automation-onboard-solutions-from-automation-account.md).

5. Para habilitar Update Management, elija el área de trabajo de Log Analytics y la cuenta de Automation y haga clic en **Habilitar**. La instalación tarda hasta 15 minutos en completarse. 

    ![Habilitación de la Administración de actualizaciones](media/automation-tutorial-update-management/manageupdates-update-enable.png)

## <a name="next-steps"></a>Pasos siguientes

* Para usar Update Management para máquinas virtuales, consulte [Administración de actualizaciones y revisiones para las máquinas virtuales de Azure](automation-tutorial-update-management.md).
* Para solucionar los errores de Update Management generales, consulte [Solución de problemas de Update Management](troubleshoot/update-management.md).
* Para solucionar problemas con el agente de Windows Update, consulte [Solución de problemas del agente de actualización de Windows](troubleshoot/update-agent-issues.md).
* Para solucionar problemas con el agente de actualización de Linux, consulte [Solución de problemas del agente de actualización de Linux](troubleshoot/update-agent-issues-linux.md).
