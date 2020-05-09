---
title: Actualización de módulos de Azure PowerShell en Azure Automation
description: En este artículo se describe cómo puede actualizar ahora módulos comunes de Azure PowerShell proporcionados de forma predeterminada en Azure Automation.
services: automation
ms.subservice: process-automation
ms.date: 06/14/2019
ms.topic: conceptual
ms.openlocfilehash: bb73b3d644e96f9596f887faaf62eb15f01956ab
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/28/2020
ms.locfileid: "81769666"
---
# <a name="update-azure-powershell-modules-in-azure-automation"></a>Actualización de módulos de Azure PowerShell en Azure Automation

Para actualizar los módulos de Azure en la cuenta de Automation, debe usar el [runbook para actualizar módulos de Azure](https://github.com/Microsoft/AzureAutomation-Account-Modules-Update), que está disponible como código abierto. Para empezar a usar el runbook **Update-AutomationAzureModulesForAccount** para actualizar los módulos de Azure, descárguelo desde el [repositorio del runbook Actualizar módulos de Azure](https://github.com/Microsoft/AzureAutomation-Account-Modules-Update) en GitHub. A continuación, puede importarlo en la cuenta de Automation o ejecutarlo como un script. Para aprender a importar un runbook en la cuenta de Automation, consulte [Importación de un runbook](manage-runbooks.md#importing-a-runbook).

Los módulos de PowerShell más comunes se proporcionan de forma predeterminada en cada cuenta de Automation. El equipo de Azure actualiza periódicamente los módulos de Azure. Por lo tanto, para mantener actualizados los módulos de las cuentas de Automation, debe usar el runbook [Update-AutomationAzureModulesForAccount](https://github.com/Microsoft/AzureAutomation-Account-Modules-Update).

Dado que los módulos se actualizan periódicamente por el grupo de productos, pueden producirse cambios con los cmdlets incluidos. Estos cambios, como el cambio de nombre de un parámetro o dejar en desuso un cmdlet por completo, pueden afectar negativamente a los runbooks.

Para evitar afectar los runbooks y los procesos que automatizan, pruébelos y valídelos antes de continuar. Si no tiene una cuenta de Automation dedicada pensada para este propósito, considere la posibilidad de crear una para que pueda probar muchos escenarios diferentes durante el desarrollo de los runbooks. Estas pruebas deben incluir cambios iterativos, por ejemplo, la actualización de los módulos de PowerShell.

Si desarrolla sus scripts de forma local, se recomienda tener localmente las mismas versiones del módulo que tiene en la cuenta de Automation al realizar pruebas, a fin de asegurarse de que obtendrá los mismos resultados. Una vez validados los resultados y aplicados los cambios necesarios, puede mover los cambios a producción.

> [!NOTE]
> Es posible que una cuenta de Automation nueva no contenga los módulos más recientes.

> [!NOTE]
> No podrá eliminar los módulos globales, que son los módulos que Automation proporciona de forma integrada.

>[!NOTE]
>Este artículo se ha actualizado para usar el nuevo módulo Az de Azure PowerShell. Aún puede usar el módulo de AzureRM que continuará recibiendo correcciones de errores hasta diciembre de 2020 como mínimo. Para más información acerca del nuevo módulo Az y la compatibilidad con AzureRM, consulte [Introducing the new Azure PowerShell Az module](https://docs.microsoft.com/powershell/azure/new-azureps-module-az?view=azps-3.5.0) (Presentación del nuevo módulo Az de Azure PowerShell). Para obtener instrucciones sobre la instalación del módulo Az en Hybrid Runbook Worker, consulte [Instalación del módulo de Azure PowerShell](https://docs.microsoft.com/powershell/azure/install-az-ps?view=azps-3.5.0). 

## <a name="considerations"></a>Consideraciones

Estas son algunas consideraciones que hay que tener en cuenta al usar este artículo para actualizar los módulos de Azure:

* El runbook descrito en este artículo permite actualizar los módulos de Azure, AzureRM y Az de forma predeterminada. Revise el archivo [LÉAME del runbook para actualizar módulos de Azure](https://github.com/microsoft/AzureAutomation-Account-Modules-Update/blob/master/README.md) para más información sobre cómo actualizar los módulos de Az.Automation con este runbook. Hay otros factores importantes que se deben tener en cuenta al usar los módulos de Az en la cuenta de Automation. Consulte [Administración de módulos en Azure Automation](shared-resources/modules.md) para más información.

* Antes de iniciar este runbook, asegúrese de que la cuenta de Automation tiene una [credencial de cuenta de ejecución de Azure](manage-runas-account.md) creada.

* Puede usar este código como un script normal de PowerShell en lugar de un runbook: simplemente inicie sesión en Azure mediante el cmdlet [Connect-AzAccount](https://docs.microsoft.com/powershell/module/az.accounts/connect-azaccount?view=azps-3.7.0) y, a continuación, pase `-Login $false` al script.

* Para usar este runbook en las nubes soberanas, use el parámetro `AzEnvironment` para pasar el entorno correcto al runbook.  Los valores permitidos son AzureCloud (nube pública de Azure), AzureChinaCloud, AzureGermanCloud y AzureUSGovernment. Estos valores se pueden recuperar con `Get-AzEnvironment | select Name`. Si no pasa un valor a este cmdlet, el runbook toma como valor predeterminado AzureCloud.

* Si desea utilizar una versión específica del módulo de Azure PowerShell en lugar del último módulo disponible en la Galería de PowerShell, pase estas versiones al parámetro opcional `ModuleVersionOverrides` del runbook **Update-AutomationAzureModulesForAccount**. Para obtener ejemplos, consulte el runbook [Update-AutomationAzureModulesForAccount.ps1](https://github.com/Microsoft/AzureAutomation-Account-Modules-Update/blob/master/Update-AutomationAzureModulesForAccount.ps1
). Los módulos de Azure PowerShell que no se mencionan en el parámetro `ModuleVersionOverrides` se actualizan con las últimas versiones de los módulos en la Galería de PowerShell. Si no se pasa nada al parámetro `ModuleVersionOverrides`, todos los módulos se actualizan con las versiones de módulo más recientes de la Galería de PowerShell. Se trata del mismo comportamiento que tiene el botón **Actualizar módulos de Azure**.

## <a name="next-steps"></a>Pasos siguientes

Visite el [runbook Actualizar módulos de Azure](https://github.com/Microsoft/AzureAutomation-Account-Modules-Update) de código abierto para más información.
