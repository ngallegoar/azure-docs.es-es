---
title: Limitación del ámbito de implementación de Change Tracking e Inventario de Azure Automation
description: En este artículo se explica cómo trabajar con las configuraciones de ámbito para limitar el ámbito de una implementación de Change Tracking e Inventario.
services: automation
ms.subservice: change-inventory-management
ms.date: 10/14/2020
ms.topic: conceptual
ms.openlocfilehash: 6eb9a20920f0a340491459f0875fc85b90dfa193
ms.sourcegitcommit: 957c916118f87ea3d67a60e1d72a30f48bad0db6
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/19/2020
ms.locfileid: "92209298"
---
# <a name="limit-change-tracking-and-inventory-deployment-scope"></a>Limitación del ámbito de implementación de Change Tracking e Inventario

En este artículo se describe cómo trabajar con configuraciones de ámbito al usar la característica [Change Tracking e Inventario](overview.md) para implementar cambios en las VM. Para obtener más información, consulte [Soluciones de supervisión como destino en Azure Monitor (versión preliminar)](../../azure-monitor/insights/solution-targeting.md).

## <a name="about-scope-configurations"></a>Acerca de las configuraciones de ámbito

Una configuración de ámbito es un grupo de una o varias búsquedas (consultas) guardadas que se usa para limitar el ámbito de Change Tracking e Inventario a equipos concretos. La configuración de ámbito se usa dentro del área de trabajo de Log Analytics para seleccionar como destino los equipos que se van a habilitar. Cuando agrega un equipo a los cambios de la característica, el equipo también se agrega a una búsqueda guardada en el área de trabajo.

## <a name="set-the-scope-limit"></a>Definición del límite de ámbito

Para limitar el ámbito de la implementación de Change Tracking e Inventario:

1. Inicie sesión en [Azure Portal](https://portal.azure.com).

2. En Azure Portal, seleccione **Todos los servicios** . En la lista de recursos, escriba **Automation** . Cuando comience a escribir, la lista filtrará las sugerencias en función de la entrada. Seleccione **Cuentas de Automation** .

3. En la lista de cuentas de Automation, seleccione la cuenta que eligió al habilitar Seguimiento de cambios e inventario.

4. En la cuenta de Automation, seleccione **Área de trabajo vinculada** , en **Recursos relacionados** .

5. Haga clic en **Ir a área de trabajo** .

6. Seleccione **Configuraciones de ámbito (versión preliminar)** en **Orígenes de datos del área de trabajo** .

7. Seleccione los puntos suspensivos a la derecha de la configuración del ámbito `MicrosoftDefaultScopeConfig-ChangeTracking` y haga clic en **Editar** .

8. En el panel de edición, seleccione **Seleccionar grupos de equipos** . En el panel Grupos de equipos se muestran las búsquedas guardadas que se usan para crear la configuración de ámbito. La búsqueda guardada usada por Change Tracking e Inventario es:

    |Nombre     |Category  |Alias  |
    |---------|---------|---------|
    |MicrosoftDefaultComputerGroup     |  ChangeTracking       | ChangeTracking__MicrosoftDefaultComputerGroup        |

9. Seleccione la búsqueda guardada para ver y editar la consulta usada para rellenar el grupo. La siguiente imagen muestra la consulta y sus resultados:

    ![Búsquedas guardadas](media/manage-scope-configurations/logsearch.png)

## <a name="next-steps"></a>Pasos siguientes

* Para trabajar con Change Tracking e Inventario, vea [Administración de Change Tracking e Inventario](manage-change-tracking.md).
* Para solucionar problemas generales de la característica, vea [Solución de problemas de Change Tracking e Inventario](../troubleshoot/change-tracking.md).
