---
title: Quitar máquinas virtuales de Azure Automation Change Tracking e inventario
description: En este artículo se explica cómo quitar las máquinas virtuales de Change Tracking e inventario.
services: automation
ms.subservice: change-inventory-management
ms.topic: conceptual
ms.date: 10/14/2020
ms.openlocfilehash: 9a7f36299d235ca2578a1ff3518650c5632ef046
ms.sourcegitcommit: 957c916118f87ea3d67a60e1d72a30f48bad0db6
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/19/2020
ms.locfileid: "92209343"
---
# <a name="remove-vms-from-change-tracking-and-inventory"></a>Eliminación de máquinas virtuales de Change Tracking e Inventario

Cuando haya terminado de realizar el seguimiento de los cambios en las máquinas virtuales de su entorno, puede dejar de administrarlas con la característica [Seguimiento de cambios e inventario](overview.md). Para dejar de administrarlos, modificará la consulta de búsqueda guardada `MicrosoftDefaultComputerGroup` en el área de trabajo de Log Analytics que está vinculada a su cuenta de Automation.

## <a name="sign-into-the-azure-portal"></a>Inicie sesión en Azure Portal.

Inicie sesión en [Azure Portal](https://portal.azure.com).

## <a name="to-remove-your-vms"></a>Para quitar las máquinas virtuales

1. En Azure Portal, inicie **Cloud Shell** en la navegación superior de Azure Portal. Si no está familiarizado con Azure Cloud Shell, consulte [Introducción a Azure Cloud Shell](/cloud-shell/overview).

2. Use el siguiente comando para identificar el UUID de un equipo que desea quitar de la administración.

    ```azurecli
    az vm show -g MyResourceGroup -n MyVm -d
    ```

3. En Azure Portal, vaya a **Areas de trabajo de Log Analytics** . Seleccione el área de trabajo de la lista.

4. En el área de trabajo de Log Analytics, seleccione **Registros** y luego elija **Explorador de consultas** en el menú acciones superiores.

5. En **Explorador de consultas** en el panel de la derecha, expanda **Consultas guardadas\Actualizaciones** y seleccione la consulta de búsqueda guardada `MicrosoftDefaultComputerGroup` para modificarla.

6. En el editor de consultas, revise la consulta y busque el UUID de la máquina virtual. Quite el UUID de la máquina virtual y repita los pasos para las demás máquinas virtuales que desee quitar.

7. Guarde la búsqueda guardada cuando haya terminado de modificarla; para ello, seleccione **Guardar** en la barra superior.

>[!NOTE]
>Las máquinas todavía se muestran después de anular la inscripción, ya que se informa de todas las máquinas que se han evaluado en las últimas 24 horas. Después de quitar la máquina, debe esperar 24 horas antes de que ya no aparezcan en la lista.

## <a name="next-steps"></a>Pasos siguientes

Para volver a habilitar esta característica, consulte [Habilitación de Seguimiento de cambios e inventario desde una cuenta de Automation](enable-from-automation-account.md), [Habilitación de Seguimiento de cambios e inventario desde Azure Portal](enable-from-portal.md), [Habilitación de Seguimiento de cambios e inventario desde un runbook](enable-from-runbook.md) o [Habilitación de Seguimiento de cambios e inventario desde una máquina virtual de Azure](enable-from-vm.md).