---
title: Asignaciones de áreas de trabajo de Log Analytics y Azure Automation
description: En este artículo se describen las asignaciones permitidas entre una cuenta de Automation y un área de trabajo de Log Analytics para admitir una solución.
services: automation
ms.service: automation
ms.subservice: process-automation
author: mgoedtel
ms.author: magoedte
ms.date: 05/20/2019
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: 965d6b206bb64e90fe59798ce0c37ccf029117f5
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "74849520"
---
# <a name="workspace-mappings"></a>Asignaciones de áreas de trabajo

Cuando se habilitan soluciones como Update Management, Change Tracking e Inventario o la solución Start/Stop VMs during off-hours, solo ciertas regiones pueden vincular un área de trabajo de Log Analytics y una cuenta de Automation. Esta asignación solo se aplica a la cuenta de Automation y al área de trabajo de Log Analytics. Los recursos que dependen de la cuenta de Automation o del área de trabajo de Log Analytics pueden residir en otras regiones.

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

<sup>2</sup> Debido a restricciones de capacidad, la región no está disponible cuando se crean recursos nuevos. Esto incluye cuentas de Automation y áreas de trabajo de Log Analytics. Sin embargo, los recursos vinculados preexistentes en la región deberían seguir funcionando.

## <a name="unlink-workspace"></a>Unlink workspace (Desvincular área de trabajo)

Si decide que ya no quiere integrar su cuenta de Automation con un área de trabajo de Log Analytics, puede desvincular la cuenta directamente desde Azure Portal. Antes de continuar, debe quitar las soluciones Update Management, Change Tracking e Inventario o Start/Stop VMs during off-hours si las está usando. Si no las quita, se impedirá que siga este proceso. Revise el artículo de la solución concreta que ha importado para conocer los pasos necesarios para quitarla.

Después de quitar estas soluciones, puede realizar los pasos siguientes para desvincular la cuenta de Automation.

> [!NOTE]
> Algunas soluciones que incluyen versiones anteriores de la solución de supervisión de SQL Azure pueden haber creado recursos de automatización y también puede que tengan que quitarse antes de desvincularse del área de trabajo.

1. En Azure Portal, abra su cuenta de Automation y, en la página de la cuenta de Automation, seleccione **Área de trabajo vinculada** en la sección **Recursos relacionados** de la izquierda.

2. En la página Desvincular área de trabajo, haga clic en **Desvincular área de trabajo**. Aparecerá un aviso para comprobar que desea continuar.

3. Aunque Azure Automation trate de desvincular la cuenta del área de trabajo de Log Analytics, puede seguir el progreso en **Notificaciones** en el menú.

Si ha usado la solución de administración de actualizaciones, también puede quitar los siguientes elementos que ya no necesite después de quitar la solución.

* Programaciones de actualizaciones: cada una tendrá nombres que coinciden con las implementaciones de las actualizaciones que ha creado.

* Grupos de Hybrid Worker creados para la solución: cada uno de ellos tendrá un nombre similar a `machine1.contoso.com_9ceb8108-26c9-4051-b6b3-227600d715c8`.

Si ha usado la solución de inicio y detención de máquinas virtuales durante las horas de trabajo, también puede quitar los siguientes elementos que ya no necesite después de quitar la solución.

* Programaciones de runbook de inicio y detención de máquinas virtuales
* Runbooks de inicio y detención de máquinas virtuales
* variables

También puede desvincular el área de trabajo de la cuenta de Automation del área de trabajo de Log Analytics. En el área de trabajo, seleccione **Cuenta de Automation** en **Recursos relacionados**. En la página Cuenta de Automation, seleccione **Desvincular cuenta**.

## <a name="next-steps"></a>Pasos siguientes

Sepa cómo incorporar las soluciones siguientes:

Update Management y Change Tracking e Inventario:

* Desde una [máquina virtual](../automation-onboard-solutions-from-vm.md)
* Desde la [cuenta de Automation](../automation-onboard-solutions-from-automation-account.md)
* Al [examinar varias máquinas](../automation-onboard-solutions-from-browse.md)
* Desde un [runbook](../automation-onboard-solutions.md)

Inicio y detención de máquinas virtuales durante las horas de trabajo

* [Implementación de Start/Stop VMs during off-hours](../automation-solution-vm-management.md)
