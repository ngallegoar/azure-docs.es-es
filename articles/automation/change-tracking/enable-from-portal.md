---
title: Habilitación de Change Tracking e Inventario de Azure Automation desde Azure Portal
description: En este artículo se muestra cómo habilitar la característica Change Tracking e Inventario desde Azure Portal.
services: automation
ms.subservice: change-inventory-management
ms.date: 10/14/2020
ms.topic: conceptual
ms.openlocfilehash: 6ca5b1df05c04937598278378e001f41f08ef23c
ms.sourcegitcommit: d22a86a1329be8fd1913ce4d1bfbd2a125b2bcae
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/26/2020
ms.locfileid: "96183641"
---
# <a name="enable-change-tracking-and-inventory-from-azure-portal"></a>Habilitación de Change Tracking e Inventario desde Azure Portal

En este artículo se describe cómo se puede habilitar [Seguimiento de cambios e inventario](overview.md) para una o varias máquinas virtuales de Azure en Azure Portal. Para habilitar máquinas virtuales de Azure a escala, debe habilitar una máquina virtual existente mediante Change Tracking e Inventario.

El número de grupos de recursos que puede usar para administrar las máquinas virtuales está limitado por los [límites de implementación de Resource Manager](../../azure-resource-manager/templates/deploy-to-resource-group.md). Las implementaciones de Resource Manager están limitadas a cinco grupos de recursos por implementación. Dos de esos grupos de recursos están reservados para configurar el área de trabajo de Log Analytics, la cuenta de Automation y los recursos relacionados. Esto le deja con tres grupos de recursos que seleccionar para su administración mediante Change Tracking e Inventario. Este límite solo se aplica a la instalación simultánea, no al número de grupos de recursos que una característica de Automation puede administrar.

> [!NOTE]
> Al habilitar Change Tracking e Inventario, solo se admite la vinculación a un área de trabajo de Log Analytics y una cuenta de Automation en regiones específicas. Para obtener una lista de los pares de asignación que se admiten, consulte [Asignación de región para la cuenta de Automation y el área de trabajo de Log Analytics](../how-to/region-mappings.md).

## <a name="prerequisites"></a>Requisitos previos

* Suscripción de Azure. Si aún no tiene ninguna, puede [activar las ventajas de la suscripción a MSDN](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) o suscribirse para obtener una [cuenta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).
* Una [cuenta de Automation](../index.yml) para administrar máquinas.
* Una [máquina virtual](../../virtual-machines/windows/quick-create-portal.md).

## <a name="sign-in-to-azure"></a>Inicio de sesión en Azure

Inicie sesión en Azure en https://portal.azure.com.

## <a name="enable-change-tracking-and-inventory"></a>Habilitación de Change Tracking e Inventario

1. En Azure Portal, vaya a **Máquinas virtuales**.

2. Use las casillas para elegir las máquinas virtuales que se van a agregar a Change Tracking e Inventario. Puede agregar máquinas para un máximo de tres grupos de recursos distintos a la vez. Las máquinas virtuales de Azure pueden existir en cualquier región independientemente de la ubicación de la cuenta de Automation.

    ![Lista de máquinas virtuales](media/enable-from-portal/vmlist.png)

    > [!TIP]
    > Use los controles de filtro para seleccionar máquinas virtuales de distintas suscripciones, ubicaciones y grupos de recursos. Puede hacer clic en la casilla superior para seleccionar todas las máquinas virtuales de una lista.

3. Seleccione **Change Tracking** o **Inventario** en **Administración de configuración**.

4. La lista de máquinas virtuales se filtra para mostrar solo las que se encuentran en la misma suscripción y ubicación. Si las máquinas virtuales están en más de tres grupos de recursos, se seleccionan los tres primeros.

5. De forma predeterminada, se seleccionan un área de trabajo de Log Analytics y una cuenta de Automation existentes. Si quiere usar un área de trabajo de Log Analytics y una cuenta de Automation diferentes, haga clic en **PERSONALIZAR** para seleccionarlas en la página Configuración personalizada. Al elegir un área de trabajo de Log Analytics, se realiza una comprobación para determinar si está vinculada a una cuenta de Automation. Si se encuentra una cuenta de Automation vinculada, verá la siguiente pantalla. Cuando termine, haga clic en **Aceptar**.

    ![Seleccionar área de trabajo y cuenta](media/enable-from-portal/select-workspace-and-account.png)

6. Si el área de trabajo seleccionada no está vinculada a una cuenta de Automation, verá la siguiente pantalla. Seleccione una cuenta de Automation y haga clic en **Aceptar** cuando haya terminado.

    ![No hay área de trabajo](media/enable-from-portal/no-workspace.png)

7. Anule la selección de las casillas que haya junto a las máquinas virtuales que no desee habilitar. Ya se ha anulado la selección de las máquinas virtuales que no se puedan habilitar.

8. Haga clic en **Habilitar** para habilitar la característica que ha seleccionado. La instalación tarda hasta 15 minutos en completarse.

## <a name="next-steps"></a>Pasos siguientes

* Para más información sobre cómo trabajar con la característica, consulte [Administración de Change Tracking](manage-change-tracking.md) y [Administración del inventario](manage-inventory-vms.md).
* Para solucionar problemas generales con la característica, consulte [Solución de problemas de Change Tracking e Inventario](../troubleshoot/change-tracking.md).