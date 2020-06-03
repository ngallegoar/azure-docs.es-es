---
title: Habilitación de Update Management de Azure Automation desde una máquina virtual de Azure
description: En este artículo se indica cómo habilitar Update Management desde una máquina virtual de Azure.
services: automation
ms.date: 03/04/2020
ms.topic: conceptual
ms.custom: mvc
ms.openlocfilehash: cab114e50852f293a3d1caf5bdc9a341f75f2557
ms.sourcegitcommit: 493b27fbfd7917c3823a1e4c313d07331d1b732f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/21/2020
ms.locfileid: "83743959"
---
# <a name="enable-update-management-from-an-azure-vm"></a>Habilitación de Update Management desde una maquina virtual de Azure

En este artículo se describe cómo puede usar una máquina virtual de Azure para habilitar la característica [Update Management](automation-update-management.md) en otras máquinas. Para habilitar máquinas virtuales de Azure a escala, debe habilitar una máquina virtual existente mediante Update Management. 

> [!NOTE]
> Al habilitar Update Management, solo en determinadas regiones se puede vincular un área de trabajo de Log Analytics y una cuenta de Automation. Para obtener una lista de los pares de asignación que se admiten, consulte [Asignación de región para la cuenta de Automation y el área de trabajo de Log Analytics](how-to/region-mappings.md).

## <a name="prerequisites"></a>Prerrequisitos

* Suscripción de Azure. Si aún no tiene ninguna, puede [activar las ventajas de la suscripción a MSDN](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) o suscribirse para obtener una [cuenta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).
* Una [cuenta de Automation](automation-offering-get-started.md) para administrar máquinas.
* Una [máquina virtual](../virtual-machines/windows/quick-create-portal.md).

## <a name="sign-in-to-azure"></a>Inicio de sesión en Azure

Inicie sesión en Azure Portal en https://portal.azure.com.

## <a name="enable-update-management"></a>Habilitación de la Administración de actualizaciones

1. En [Azure Portal](https://portal.azure.com), seleccione **Máquinas virtuales** o busque y seleccione **Máquinas virtuales** en la página principal.

2. Seleccione la máquina virtual para la que desea habilitar Update Management. Las máquina virtuales pueden existir en cualquier región, independientemente de la ubicación de la cuenta de Automation. Los

3. En la página de la máquina virtual, en **Operaciones**, seleccione **Update Management**.

4. Debe tener el permiso `Microsoft.OperationalInsights/workspaces/read` para determinar si la máquina virtual está habilitada para un área de trabajo. Para obtener información sobre los permisos adicionales requeridos, vea los [permisos necesarios para habilitar máquinas](automation-role-based-access-control.md#feature-setup-permissions). Para obtener información sobre cómo habilitar varias máquinas a la vez, vea [Habilitación de Update Management desde una cuenta de Automation](automation-onboard-solutions-from-automation-account.md).

5. Para habilitar Update Management, elija el área de trabajo de Log Analytics y la cuenta de Automation y haga clic en **Habilitar**. La instalación tarda hasta 15 minutos en completarse. 

    ![Habilitación de la Administración de actualizaciones](media/automation-tutorial-update-management/manageupdates-update-enable.png)

## <a name="check-the-scope-configuration"></a><a name="scope-configuration"></a>Comprobación de la configuración de ámbito

Update Management usa una configuración de ámbito dentro del área de trabajo para seleccionar como destino los equipos a fin de habilitar la característica. La configuración de ámbito es un grupo de una o varias búsquedas guardadas que se usa para limitar el ámbito de la característica a equipos concretos. Para más información, vea [Uso de configuraciones de ámbito para Update Management](automation-scope-configurations-update-management.md).

## <a name="next-steps"></a>Pasos siguientes

* Para usar Update Management para máquinas virtuales, vea [Administración de actualizaciones y revisiones para las máquinas virtuales de Azure](automation-tutorial-update-management.md).
* Para ver las configuraciones de ámbito, consulte [Uso de configuraciones de ámbito para Update Management](automation-scope-configurations-update-management.md).
* Si ya no necesita el área de trabajo de Log Analytics, vea las instrucciones en [Desvinculación de un área de trabajo de una cuenta de Automation para Update Management](automation-unlink-workspace-update-management.md).
* Para eliminar máquinas virtuales de Update Management, vea [Eliminación de una máquina virtual desde Update Management](automation-remove-vms-from-update-management.md).
* Para solucionar los errores de Update Management generales, vea [Solución de problemas de Update Management](troubleshoot/update-management.md).
* Para solucionar problemas con el agente de Windows Update, vea [Solución de problemas del agente de actualización de Windows](troubleshoot/update-agent-issues.md).
* Para solucionar problemas con el agente de actualización de Linux, vea [Solución de problemas del agente de actualización de Linux](troubleshoot/update-agent-issues-linux.md).
