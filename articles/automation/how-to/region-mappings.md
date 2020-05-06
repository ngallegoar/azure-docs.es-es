---
title: Asignaciones de áreas de trabajo de Log Analytics y Azure Automation
description: En este artículo se describen las asignaciones permitidas entre una cuenta de Automation y un área de trabajo de Log Analytics para admitir una solución.
services: automation
ms.service: automation
ms.subservice: process-automation
author: mgoedtel
ms.author: magoedte
ms.date: 04/23/2020
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: 607cebca3e6e8ddd95900ecdbd7041e5f7bb50cc
ms.sourcegitcommit: fad3aaac5af8c1b3f2ec26f75a8f06e8692c94ed
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/27/2020
ms.locfileid: "82165780"
---
# <a name="workspace-mappings"></a>Asignaciones de áreas de trabajo

Cuando se habilitan soluciones como Update Management, Change Tracking e Inventario o Start/Stop VMs during off hours, solo se admiten ciertas regiones para vincular un área de trabajo de Log Analytics y una cuenta de Automation en la suscripción. Esta asignación solo se aplica a la cuenta de Automation y al área de trabajo de Log Analytics. El área de trabajo Log Analytics y la cuenta de Automation deben estar en la misma suscripción, pero pueden estar en distintos grupos de recursos implementados en la misma región.

Para obtener más información, consulte el [área de trabajo de Log Analytics y la cuenta de Automation](../../azure-monitor/insights/solutions.md#log-analytics-workspace-and-automation-account).

## <a name="supported-mappings"></a>Asignaciones admitidas

En la tabla siguiente se muestran las asignaciones admitidas:

|**Región del área de trabajo de Log Analytics**|**Región de Azure Automation**|
|---|---|
|**EE. UU.**||
|EastUS<sup>1</sup>|EastUS2|
|WestUS2|WestUS2|
|WestCentralUS<sup>2</sup>|WestCentralUS<sup>2</sup>|
|**Canadá**||
|CanadaCentral|CanadaCentral|
|**Asia Pacífico**||
|AustraliaSoutheast|AustraliaSoutheast|
|SoutheastAsia|SoutheastAsia|
|CentralIndia|CentralIndia|
|JapanEast|JapanEast|
|**Europa**||
|UKSouth|UKSouth|
|Oeste de Europa|Oeste de Europa|
|**US Gov**||
|USGovVirginia|USGovVirginia|

<sup>1</sup> La asignación EastUS de áreas de trabajo de Log Analytics a cuentas de Automation no es una asignación exacta de región a región, pero es la asignación correcta.

<sup>2</sup> Debido a restricciones de capacidad, la región no está disponible cuando se crean recursos nuevos. Incluye cuentas de Automation y áreas de trabajo de Log Analytics. Sin embargo, los recursos vinculados preexistentes en la región deberían seguir funcionando.

## <a name="unlink-workspace"></a>Unlink workspace (Desvincular área de trabajo)

Si decide que ya no quiere integrar su cuenta de Automation con un área de trabajo de Log Analytics, puede desvincular la cuenta directamente desde Azure Portal. Antes de continuar, debe quitar las soluciones Update Management, Change Tracking e Inventario y Start/Stop VMs during off hours, si las usa. Si no las quita, no podrá completar la operación de desvinculación. Revise el artículo para cada una de las que esté habilitando a fin de comprender los pasos necesarios para quitarla.

Después de quitarlas, puede realizar los pasos siguientes para desvincular la cuenta de Automation.

> [!NOTE]
> Algunas soluciones que incluyen versiones anteriores de la solución de supervisión de SQL Azure pueden haber creado recursos de Automation y también puede que tengan que quitarse antes de desvincular el área de trabajo.

1. En Azure Portal, abra su cuenta de Automation. En la página Cuenta de Automation, seleccione **Área de trabajo vinculada** en **Recursos relacionados**.

2. En la página Desvincular área de trabajo, haga clic en **Desvincular área de trabajo**. Se muestra un aviso para comprobar que desea continuar.

3. Aunque Azure Automation trate de desvincular la cuenta del área de trabajo de Log Analytics, puede seguir el progreso en **Notificaciones** en el menú.

4. Si ha usado Update Management, también puede quitar los siguientes elementos que ya no necesite después de quitarlo.

    * Programaciones de actualizaciones: cada una tiene un nombre que coincide con una implementación de actualizaciones que ha creado.
    * Grupos de Hybrid Worker creados para la solución: cada uno de ellos tiene un nombre similar a `machine1.contoso.com_9ceb8108-26c9-4051-b6b3-227600d715c8`.

5. Si ha usado Start/Stop VMs during off hours, también puede quitar los siguientes elementos que ya no necesite después de quitarlo.

    * Programaciones de runbook de inicio y detención de máquinas virtuales
    * Runbooks de inicio y detención de máquinas virtuales
    * variables

También puede desvincular el área de trabajo de la cuenta de Automation en el área de trabajo.

1. En el área de trabajo, seleccione **Cuenta de Automation** en **Recursos relacionados**. 
2. En la página Cuenta de Automation, seleccione **Desvincular cuenta**.

## <a name="next-steps"></a>Pasos siguientes

* Obtenga información sobre la incorporación de Update Management y Change Tracking e Inventario:

    * Desde una [máquina virtual](../automation-onboard-solutions-from-vm.md)
    * Desde la [cuenta de Automation](../automation-onboard-solutions-from-automation-account.md)
    * Al [examinar varias máquinas](../automation-onboard-solutions-from-browse.md)
    * Desde un [runbook](../automation-onboard-solutions.md)

* Obtenga información sobre la incorporación de Start/Stop VMs during off hours:

    * [Introducción a Start/Stop VMs during off-hours](../automation-solution-vm-management.md)
