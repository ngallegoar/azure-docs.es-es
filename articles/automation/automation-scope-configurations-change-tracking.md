---
title: Uso de configuraciones de ámbito para Change Tracking e Inventario de Azure Automation
description: En este artículo se explica cómo trabajar con configuraciones de ámbito cuando se usa Change Tracking e Inventario.
services: automation
ms.date: 03/04/2020
ms.topic: conceptual
ms.custom: mvc
ms.openlocfilehash: 4fac94cc2f8f378b7e9d8e9485baed6a0ffa838b
ms.sourcegitcommit: 0b80a5802343ea769a91f91a8cdbdf1b67a932d3
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/25/2020
ms.locfileid: "83832170"
---
# <a name="work-with-scope-configurations-for-change-tracking-and-inventory"></a>Uso de configuraciones de ámbito para Change Tracking e Inventario

En este artículo se describe cómo puede trabajar con configuraciones de ámbito cuando se habilita la característica [Update Management](automation-update-management.md) en máquinas virtuales. 

## <a name="sign-in-to-azure"></a>Inicio de sesión en Azure

Inicie sesión en Azure Portal en https://portal.azure.com.

## <a name="check-the-scope-configuration"></a><a name="scope-configuration"></a>Comprobación de la configuración de ámbito

Update Management usa una configuración de ámbito dentro del área de trabajo de Log Analytics para seleccionar como destino los equipos a fin de habilitar Update Management. La configuración de ámbito es un grupo de una o más búsquedas guardadas que se usa para limitar el ámbito de la característica a equipos concretos. Para acceder a las configuraciones de ámbito:

1. En la cuenta de Automation, en **Recursos relacionados**, seleccione **Área de trabajo**. 

2. Elija el área de trabajo en **Orígenes de datos del área de trabajo** y, después, seleccione **Configuraciones de ámbito**.

3. Si el área de trabajo seleccionada todavía no tiene la característica Update Management habilitada, crea la configuración de ámbito `MicrosoftDefaultScopeConfig-ChangeTracking`. 

4. Si el área de trabajo seleccionada ya tiene la característica habilitada, no se vuelve a implementar y no se le agrega la configuración de ámbito. 

5. Seleccione los puntos suspensivos de cualquiera de las configuraciones y, después, haga clic en **Editar**. 

6. En el panel de edición, seleccione **Seleccionar grupos de equipos**. En el panel Grupos de equipos se muestran las búsquedas guardadas que se usan para crear la configuración de ámbito.

## <a name="view-a-saved-search"></a>Visualización de una búsqueda guardada

Cuando se agrega un equipo a Change Tracking e Inventario, también se agrega a una búsqueda guardada en el área de trabajo. La búsqueda guardada es una consulta que contiene los equipos de destino.

1. Vaya al área de trabajo de Log Analytics y seleccione **Búsquedas guardadas** en **General**. La búsqueda guardada que usa Update Management es la siguiente:

    |Nombre     |Category  |Alias  |
    |---------|---------|---------|
    |MicrosoftDefaultComputerGroup     |  ChangeTracking       | ChangeTracking__MicrosoftDefaultComputerGroup        |

2. Seleccione la búsqueda guardada para ver la consulta que se usa para rellenar el grupo. La siguiente imagen muestra la consulta y sus resultados:

    ![Búsquedas guardadas](media/automation-scope-configurations-change-tracking/logsearch.png)

## <a name="next-steps"></a>Pasos siguientes

* Para trabajar con Change Tracking e Inventario, vea [Administración de Change Tracking e Inventario](change-tracking-file-contents.md).
* Para solucionar problemas generales de la característica, vea [Solución de problemas de Change Tracking e Inventario](troubleshoot/change-tracking.md).