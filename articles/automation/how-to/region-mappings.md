---
title: Regiones admitidas para el área de trabajo de Log Analytics vinculada
description: En este artículo se describen las asignaciones de regiones admitidas entre una cuenta de Automation y un área de trabajo de Log Analytics en relación con ciertas características de Azure Automation.
ms.date: 09/03/2020
services: automation
ms.topic: conceptual
ms.custom: references_regions
ms.openlocfilehash: dd831789d5f09ca6a20cce13659d6c479845f74e
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "89440667"
---
# <a name="supported-regions-for-linked-log-analytics-workspace"></a>Regiones admitidas para el área de trabajo de Log Analytics vinculada

En Azure Automation, puede habilitar las características Update Management, Change Tracking e Inventario y Start/Stop VMs during off-hours para servidores y máquinas virtuales. Estas características tienen una dependencia en un área de trabajo de Log Analytics y, por tanto, requieren vincular el área de trabajo a una cuenta de Automation. Sin embargo, solo se admiten determinadas regiones para vincularlas. En general, la asignación *no* es aplicable si tiene previsto vincular una cuenta de Automation a un área de trabajo que no tenga habilitadas estas características.

En este artículo se proporcionan las asignaciones admitidas para habilitar y usar correctamente estas características en su cuenta de Automation.

Para obtener más información, consulte [Área de trabajo de Log Analytics y cuenta de Automation](../../azure-monitor/insights/solutions.md#log-analytics-workspace-and-automation-account).

## <a name="supported-mappings"></a>Asignaciones admitidas

> [!NOTE]
> Como se muestra en la tabla siguiente, solo puede existir una asignación entre Log Analytics y Azure Automation.

En la tabla siguiente se muestran las asignaciones admitidas:

|**Región del área de trabajo de Log Analytics**|**Región de Azure Automation**|
|---|---|
|**EE. UU.**||
|EastUS<sup>1</sup>|EastUS2|
|WestUS2|WestUS2|
|WestCentralUS|WestCentralUS|
|**Canadá**||
|CanadaCentral|CanadaCentral|
|**Asia Pacífico**||
|AustraliaSoutheast|AustraliaSoutheast|
|SoutheastAsia|SoutheastAsia|
|CentralIndia|CentralIndia|
|ChinaEast2<sup>2</sup>|ChinaEast2|
|JapanEast|JapanEast|
|**Europa**||
|UKSouth|UKSouth|
|Oeste de Europa|Oeste de Europa|
|**US Gov**||
|USGovVirginia|USGovVirginia|
|USGovArizona<sup>2</sup>|USGovArizona|

<sup>1</sup> La asignación EastUS de las áreas de trabajo de Log Analytics a cuentas de Automation no es una asignación exacta de región a región, pero es la asignación correcta.

<sup>2</sup> En esta región, solo se admite Update Management y otras características como Change Tracking e Inventario no están disponibles en este momento.

## <a name="unlink-a-workspace"></a>Desvinculación de un área de trabajo

Si decide que ya no quiere integrar su cuenta de Automation con un área de trabajo de Log Analytics, puede desvincular la cuenta directamente desde Azure Portal. Antes de continuar, debe [quitar](move-account.md#remove-features) las soluciones Update Management, Change Tracking e Inventario y Start/Stop VMs during off-hours, si las usa. Si no las quita, no podrá completar la operación de desvinculación.

Con las características quitadas, puede seguir los pasos siguientes para desvincular la cuenta de Automation.

> [!NOTE]
> Algunas características, que incluyen versiones anteriores de la solución de supervisión de Azure SQL, pueden haber creado recursos de Automation que puede que tengan que quitarse antes de desvincular el área de trabajo.

1. En Azure Portal, abra su cuenta de Automation. En la página Cuenta de Automation, seleccione **Área de trabajo vinculada** en **Recursos relacionados**.

2. En la página Desvincular área de trabajo, seleccione **Desvincular área de trabajo**. Se le mostrará un aviso para verificar que quiere continuar.

3. Mientras Azure Automation desvincula la cuenta del área de trabajo de Log Analytics, puede hacer un seguimiento del progreso en la opción **Notificaciones** del menú.

4. Si ha usado Update Management, también puede quitar los siguientes elementos que ya no necesite:

    * Programaciones de actualizaciones: cada una tiene un nombre que coincide con una implementación de actualización que ha creado.
    * Se crean grupos de trabajo híbridos para la característica: cada uno tiene un nombre similar a `machine1.contoso.com_9ceb8108-26c9-4051-b6b3-227600d715c8`.

5. Si ha usado Start/Stop VMs during off-hours, también puede quitar los siguientes elementos que ya no son necesarios:

    * Programaciones de runbook de inicio y detención de máquinas virtuales
    * Runbooks de inicio y detención de máquinas virtuales
    * variables

También puede desvincular el área de trabajo de la cuenta de Automation en el área de trabajo.

1. En el área de trabajo, seleccione **Cuenta de Automation** en **Recursos relacionados**.
2. En la página Cuenta de Automation, seleccione **Desvincular cuenta**.

## <a name="next-steps"></a>Pasos siguientes

* Obtenga información sobre Update Management en [Información general de Update Management](../update-management/update-mgmt-overview.md).
* Obtenga información sobre Change Tracking e Inventario en [Información general de Change Tracking e Inventario](../change-tracking.md).
* Obtenga información sobre Start/Stop VMs during off-hours en [Información general sobre Start/Stop VMs during off-hours](../automation-solution-vm-management.md).
