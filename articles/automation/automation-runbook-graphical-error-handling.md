---
title: Control de errores en runbooks gráficos de Azure Automation
description: En este artículo se describe cómo implementar la lógica de control de errores en runbooks gráficos de Azure Automation.
services: automation
ms.subservice: process-automation
ms.date: 03/16/2018
ms.topic: conceptual
ms.openlocfilehash: f1aa605b3e6f32b260ea4a9eee9c056277fcd12d
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "79367081"
---
# <a name="error-handling-in-azure-automation-graphical-runbooks"></a>Control de errores en runbooks gráficos de Azure Automation

Un principio de diseño clave que se debe tener en cuenta para su runbook gráfico de Azure Automation es la identificación de problemas que puede experimentar el runbook durante la ejecución. Dichos problemas pueden incluir condiciones de correcto, estados de error esperado y condiciones de error inesperado.

A menudo, aunque se produzca un error de no terminación en una actividad de runbook, Windows PowerShell controla la actividad procesando cualquier actividad posterior, independientemente del error. Es probable que el error genere una excepción, pero se podrá ejecutar la siguiente actividad.

Su runbook gráfico debe incluir el código de control de errores para lidiar con problemas de ejecución. Para validar el resultado de una actividad o controlar un error, se puede usar una actividad de código de PowerShell, definir una lógica condicional en el vínculo de salida de la actividad o aplicar otro método.

Los runbooks gráficos de Azure Automation se han mejorado con la funcionalidad de inclusión del control de errores. Ahora es posible convertir las excepciones en errores de no terminación y crear vínculos de error entre las actividades. El proceso mejorado permite a su runbook detectar errores y administrar cualquier condición realizada o inesperada. 

>[!NOTE]
>Este artículo se ha actualizado para usar el nuevo módulo Az de Azure PowerShell. Aún puede usar el módulo de AzureRM que continuará recibiendo correcciones de errores hasta diciembre de 2020 como mínimo. Para más información acerca del nuevo módulo Az y la compatibilidad con AzureRM, consulte [Introducing the new Azure PowerShell Az module](https://docs.microsoft.com/powershell/azure/new-azureps-module-az?view=azps-3.5.0) (Presentación del nuevo módulo Az de Azure PowerShell). Para obtener instrucciones sobre la instalación del módulo Az en Hybrid Runbook Worker, consulte [Instalación del módulo de Azure PowerShell](https://docs.microsoft.com/powershell/azure/install-az-ps?view=azps-3.5.0). Puede actualizar los módulos de su cuenta de Automation a la versión más reciente mediante [Actualización de módulos de Azure PowerShell en Azure Automation](automation-update-azure-modules.md).

## <a name="powershell-error-types"></a>Tipos de errores de PowerShell

Los tipos de errores de PowerShell que se pueden producir durante la ejecución de runbooks son: errores de terminación y errores de no terminación.
 
### <a name="terminating-error"></a>Error de terminación

Un error de terminación es un error grave durante la ejecución que detiene un comando o la ejecución del script por completo. Entre los ejemplos se incluyen cmdlets inexistentes, errores de sintaxis que impiden la ejecución de un cmdlet y otros errores irrecuperables.

### <a name="non-terminating-error"></a>Error de no terminación

Un error de no terminación es un error que no es grave, lo que permite que la ejecución no se interrumpa pese a la condición de error. Entre los ejemplos se incluyen errores operativos, como los errores de archivos no encontrados y problemas de permisos.

## <a name="when-to-use-error-handling"></a>Cuándo se debe utilizar el control de errores

Use el control de errores en su runbook cuando una actividad crítica produzca un error o una excepción. Es importante impedir que se procese la siguiente actividad del runbook, así como controlar el error adecuadamente. Controlar el error es especialmente crítico cuando los runbooks dan soporte a un proceso de operaciones de un negocio o un servicio.

Por cada actividad que pueda generar un error, puede agregar un vínculo de error que señale a cualquier otra actividad. La actividad de destino puede ser de cualquier tipo, entre las que se incluyen la actividad de código, la invocación de un cmdlet, la invocación de otro runbook, etc. La actividad de destino también puede tener vínculos salientes, ya sean vínculos regulares o de error. Los vínculos permiten al runbook implementar una lógica de control de errores compleja sin tener que recurrir a una actividad de código.

La práctica recomendada consiste en crear un runbook específico para el control de errores con una funcionalidad común, pero dicha práctica no es obligatoria. Por ejemplo, piense en un runbook que intenta iniciar una máquina virtual e instalar una aplicación en ella. Si la máquina virtual no se inicia correctamente, realiza las siguientes acciones:

1. Envía una notificación acerca de este problema.
2. Inicia otro runbook que aprovisiona automáticamente una nueva máquina virtual.

Una solución es tener un vínculo de error en el runbook que apunte a una actividad que controle el paso uno. Por ejemplo, el runbook puede conectar el cmdlet `Write-Warning` a una actividad para el paso dos, como el cmdlet [Start-AzAutomationRunbook](https://docs.microsoft.com/powershell/module/az.automation/start-azautomationrunbook?view=azps-3.5.0).

Este comportamiento también se puede generalizar para que se pueda usar en muchos runbooks. Solo es preciso colocar estas dos actividades en un runbook de control de errores independiente. Antes de que su runbook original llame a este runbook de control de errores se puede construir un mensaje personalizado a partir de sus datos y, después, pasarlo como parámetro al runbook de control de errores.

## <a name="how-to-use-error-handling"></a>Uso del control de errores

Todas las actividades de su runbook tienen una opción de configuración que convierte las excepciones en errores que no provocan la terminación. De manera predeterminada esta opción está deshabilitada. Se recomienda habilitar esta opción en cualquier actividad en la que el runbook controle los errores. Esta opción garantiza que el runbook trate tanto los errores de la actividad que provocan terminación como los que no la provocan como errores que no la provocan, con un vínculo de error.  

Después de habilitar la opción de configuración, haga que el runbook cree una actividad que controle el error. Si la actividad produce algún error, se seguirán los vínculos de error salientes. Los vínculos regulares no se seguirán, incluso si la actividad también produce la salida regular.<br><br> ![Ejemplo de vínculo de error de runbook de Automation](media/automation-runbook-graphical-error-handling/error-link-example.png)

En el ejemplo siguiente, un runbook recupera una variable que contiene el nombre del equipo de una máquina virtual. A continuación, intenta iniciar la máquina virtual con la siguiente actividad.<br><br> ![Ejemplo de control de errores de runbook de Automation](media/automation-runbook-graphical-error-handling/runbook-example-error-handling.png)<br><br>      

La actividad `Get-AutomationVariable` y el cmdlet [Start-AzVM](https://docs.microsoft.com/powershell/module/Az.Compute/Start-AzVM?view=azps-3.5.0) están configurados para convertir excepciones en errores. Si hay problemas para obtener la variable o para iniciar la máquina virtual, el código genera errores.<br><br> ![Configuración de la actividad de control de errores de runbook de Automation](media/automation-runbook-graphical-error-handling/activity-blade-convertexception-option.png).

Los vínculos de errores fluyen de estas actividades a una única actividad de código de `error management`. Esta actividad se configura con una expresión simple de PowerShell que utiliza la palabra clave `throw` para detener el procesamiento, junto con `$Error.Exception.Message` para obtener el mensaje que describe la excepción actual.<br><br> ![Ejemplo de código de control de errores de runbook de Automation](media/automation-runbook-graphical-error-handling/runbook-example-error-handling-code.png)

## <a name="next-steps"></a>Pasos siguientes

* Para más información acerca de los vínculos y tipos de vínculo de los runbooks gráficos, consulte [Creación gráfica en Azure Automation](automation-graphical-authoring-intro.md#links-and-workflow).

* Para más información acerca de la ejecución de un runbook, la supervisión de trabajos del runbook y otros detalles técnicos, consulte [Ejecución de un runbook en Azure Automation](automation-runbook-execution.md).