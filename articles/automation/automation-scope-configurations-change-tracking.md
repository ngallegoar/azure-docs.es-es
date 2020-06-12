---
title: Limitación del ámbito de implementación de Change Tracking e Inventario de Azure Automation
description: En este artículo se explica cómo trabajar con las configuraciones de ámbito para limitar el ámbito de una implementación de Change Tracking e Inventario.
services: automation
ms.date: 03/04/2020
ms.topic: conceptual
ms.custom: mvc
ms.openlocfilehash: 49655d11858086b16099a1864fd4d2dc5988f02a
ms.sourcegitcommit: 6a9f01bbef4b442d474747773b2ae6ce7c428c1f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/27/2020
ms.locfileid: "84117425"
---
# <a name="limit-change-tracking-and-inventory-deployment-scope"></a>Limitación del ámbito de implementación de Change Tracking e Inventario

En este artículo se describe cómo trabajar con configuraciones de ámbito al usar la característica [Change Tracking e Inventario](change-tracking.md) para implementar cambios en las VM. Para obtener más información, consulte [Soluciones de supervisión como destino en Azure Monitor (versión preliminar)](https://docs.microsoft.com/azure/azure-monitor/insights/solution-targeting). 

## <a name="about-scope-configurations"></a>Acerca de las configuraciones de ámbito

Una configuración de ámbito es un grupo de una o varias búsquedas (consultas) guardadas que se usa para limitar el ámbito de Change Tracking e Inventario a equipos concretos. La configuración de ámbito se usa dentro del área de trabajo de Log Analytics para seleccionar como destino los equipos que se van a habilitar. Cuando agrega un equipo a los cambios de la característica, el equipo también se agrega a una búsqueda guardada en el área de trabajo.

## <a name="set-the-scope-limit"></a>Definición del límite de ámbito

Para limitar el ámbito de la implementación de Change Tracking e Inventario:

1. En la cuenta de Automation, seleccione **Área de trabajo vinculada**, en **Recursos relacionados**.

2. Haga clic en **Ir al área de trabajo**.

3. Seleccione **Configuraciones de ámbito (versión preliminar)** en **Orígenes de datos del área de trabajo**.

4. Seleccione los puntos suspensivos a la derecha de la configuración del ámbito `MicrosoftDefaultScopeConfig-ChangeTracking` y haga clic en **Editar**. 

5. En el panel de edición, seleccione **Seleccionar grupos de equipos**. En el panel Grupos de equipos se muestran las búsquedas guardadas que se usan para crear la configuración de ámbito. La búsqueda guardada usada por Change Tracking e Inventario es:

    |Nombre     |Category  |Alias  |
    |---------|---------|---------|
    |MicrosoftDefaultComputerGroup     |  ChangeTracking       | ChangeTracking__MicrosoftDefaultComputerGroup        |

6. Seleccione la búsqueda guardada para ver y editar la consulta usada para rellenar el grupo. La siguiente imagen muestra la consulta y sus resultados:

    ![Búsquedas guardadas](media/automation-scope-configurations-change-tracking/logsearch.png)

## <a name="next-steps"></a>Pasos siguientes

* Para trabajar con Change Tracking e Inventario, vea [Administración de Change Tracking e Inventario](change-tracking-file-contents.md).
* Para solucionar problemas generales de la característica, vea [Solución de problemas de Change Tracking e Inventario](troubleshoot/change-tracking.md).