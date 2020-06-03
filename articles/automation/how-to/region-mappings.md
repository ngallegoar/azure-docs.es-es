---
title: Regiones admitidas para el área de trabajo de Log Analytics vinculada
description: En este artículo se describen las asignaciones de regiones admitidas entre una cuenta de Automation y un área de trabajo de Log Analytics.
services: automation
ms.service: automation
ms.subservice: process-automation
author: mgoedtel
ms.author: magoedte
ms.date: 04/23/2020
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: 24ff69e76736ffa93cecb795be563f172c422355
ms.sourcegitcommit: 493b27fbfd7917c3823a1e4c313d07331d1b732f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/21/2020
ms.locfileid: "83744765"
---
# <a name="supported-regions-for-linked-log-analytics-workspace"></a>Regiones admitidas para el área de trabajo de Log Analytics vinculada

En Azure Automation, puede habilitar las características Update Management, Change Tracking e Inventario y Start/Stop VMs during off-hours para las máquinas virtuales. Sin embargo, un área de trabajo de Log Analytics y una cuenta de Automation en la suscripción solo se pueden vincular en determinadas regiones. Esta asignación de regiones solo se aplica a la cuenta de Automation y al área de trabajo de Log Analytics. El área de trabajo Log Analytics y la cuenta de Automation deben estar en la misma suscripción, pero pueden estar en distintos grupos de recursos implementados en la misma región. Para obtener más información, consulte el [área de trabajo de Log Analytics y la cuenta de Automation](../../azure-monitor/insights/solutions.md#log-analytics-workspace-and-automation-account).

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

<sup>1</sup> La asignación EastUS de las áreas de trabajo de Log Analytics a cuentas de Automation no es una asignación exacta de región a región, pero es la asignación correcta.

<sup>2</sup> Debido a restricciones de capacidad, la región no está disponible cuando crea recursos nuevos. Incluye cuentas de Automation y áreas de trabajo de Log Analytics. Sin embargo, los recursos vinculados preexistentes en la región deberían seguir funcionando.

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

* Obtenga información sobre Update Management en [Información general de Update Management](../automation-update-management.md).
* Obtenga información sobre Change Tracking e Inventario en [Información general de Change Tracking e Inventario](../change-tracking.md).
* Obtenga información sobre Start/Stop VMs during off-hours en [Información general sobre Start/Stop VMs during off-hours](../automation-solution-vm-management.md).
