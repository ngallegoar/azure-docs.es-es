---
title: Eliminación de la característica de Azure Automation Seguimiento de cambios e inventario
description: En este artículo se indica cómo dejar de usar Seguimiento de cambios e inventario y desvincular una cuenta de Automation del área de trabajo de Log Analytics.
services: automation
ms.subservice: change-inventory-management
ms.date: 10/14/2020
ms.topic: conceptual
ms.openlocfilehash: 2e3e5abdfbb2bf2e9d7a12a677422adc67336775
ms.sourcegitcommit: 957c916118f87ea3d67a60e1d72a30f48bad0db6
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/19/2020
ms.locfileid: "92209344"
---
# <a name="remove-change-tracking-and-inventory-from-automation-account"></a>Eliminación de Seguimiento de cambios e inventario de una cuenta de Automation

Después de habilitar la administración de las máquinas virtuales mediante Seguimiento de cambios e inventario de Azure Automation, puede decidir dejar de usar esta característica y quitar la configuración de la cuenta y el área de trabajo vinculada de Log Analytics. En este artículo se explica cómo eliminar por completo Seguimiento de cambios e inventario de las máquinas virtuales administradas, la cuenta de Automation y el área de trabajo de Log Analytics.

## <a name="sign-into-the-azure-portal"></a>Inicie sesión en Azure Portal.

Inicie sesión en [Azure Portal](https://portal.azure.com).

## <a name="remove-management-of-vms"></a>Eliminación de la administración de máquinas virtuales

Antes de quitar Seguimiento de cambios e inventario debe dejar de administrar las máquinas virtuales. Vea [Eliminación de máquinas virtuales de Seguimiento de cambios e inventario](remove-vms-from-change-tracking.md) para anular la inscripción en la característica.

## <a name="remove-changetracking-solution"></a>Eliminación de la solución Seguimiento de cambios e inventario

Para desvincular la cuenta de Automation del área de trabajo, debe seguir estos pasos para quitar por completo Seguimiento de cambios e inventario. Va a quitar la solución **Seguimiento de cambios e inventario** del área de trabajo.

1. En Azure Portal, seleccione **Todos los servicios** . En la lista de recursos, escriba **Log Analytics** . Cuando comience a escribir, la lista filtrará las sugerencias en función de la entrada. Seleccione **Log Analytics** .

2. En la lista de áreas de trabajo de Log Analytics, seleccione la que eligió al habilitar Seguimiento de cambios e inventario.

3. A la izquierda, seleccione **Soluciones** .  

4. En la lista de soluciones, seleccione **ChangeTracking(nombre de área de trabajo)** . En la página **Introducción** de la solución, seleccione **Eliminar** . Cuando se le solicite confirmación, haga clic en **Sí** .

## <a name="unlink-workspace-from-automation-account"></a>Desvinculación de un área de trabajo de una cuenta de Automation

1. En Azure Portal, seleccione **Cuentas de Automation** .

2. Abra la cuenta de Automation y seleccione **Área de trabajo vinculada** en **Recursos relacionados** a la izquierda.

3. En la página **Desvincular área de trabajo** , seleccione **Desvincular área de trabajo** y responda a los mensajes.

   ![Página Desvincular área de trabajo](media/remove-feature/automation-unlink-workspace-blade.png)

Mientras intenta desvincular el área de trabajo de Log Analytics, puede realizar el seguimiento del progreso en la opción **Notificaciones** del menú.

También puede desvincular el área de trabajo de Log Analytics de la cuenta de Automation desde el área de trabajo:

1. En Azure Portal, seleccione **Log Analytics** .

2. En el área de trabajo, seleccione **Cuenta de Automation** en **Recursos relacionados** .

3. En la página Cuenta de Automation, seleccione **Desvincular cuenta** .

Mientras intenta desvincular la cuenta de Automation, puede seguir el progreso en **Notificaciones** desde el menú.

## <a name="next-steps"></a>Pasos siguientes

Para volver a habilitar esta característica, vea [Habilitación de Seguimiento de cambios e inventario desde una cuenta de Automation](enable-from-automation-account.md), [Habilitación de Seguimiento de cambios e inventario mediante el examen de Azure Portal](enable-from-portal.md), [Habilitación de Seguimiento de cambios e inventario desde un runbook](enable-from-runbook.md) o [Habilitación de Seguimiento de cambios e inventario desde una máquina virtual de Azure](enable-from-vm.md).
