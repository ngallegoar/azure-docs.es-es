---
title: Actualización de módulos de Azure en Azure Automation
description: En este artículo se describe cómo puede actualizar ahora módulos comunes de Azure PowerShell proporcionados de forma predeterminada en Azure Automation.
services: automation
ms.subservice: process-automation
ms.date: 06/14/2019
ms.topic: conceptual
ms.openlocfilehash: 721bb3579fec10df88be471d67f68c0846aa9432
ms.sourcegitcommit: d597800237783fc384875123ba47aab5671ceb88
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/03/2020
ms.locfileid: "80632107"
---
# <a name="how-to-update-azure-powershell-modules-in-azure-automation"></a>Actualización de módulos de Azure PowerShell en Azure Automation

Para actualizar los módulos de Azure en la cuenta de Automation debe usar el [runbook Actualizar módulos de Azure](https://github.com/Microsoft/AzureAutomation-Account-Modules-Update), que está disponible como código abierto. Para empezar a usar el runbook **Update-AutomationAzureModulesForAccount** para actualizar los módulos de Azure, descárguelo desde el [repositorio del runbook Actualizar módulos de Azure](https://github.com/Microsoft/AzureAutomation-Account-Modules-Update) en GitHub. A continuación, puede importarlo en su cuenta de Automation o ejecutarlo como un script. Para obtener información sobre cómo importar un runbook en la cuenta de Automation, consulte [Importación de un runbook](manage-runbooks.md#importing-a-runbook).

Los módulos de AzureRM PowerShell más comunes se proporcionan de manera predeterminada en cada cuenta de Automation. El equipo de Azure actualiza los módulos de Azure con regularidad, por lo tanto, para mantenerse al día debe usar el runbook [Update-AutomationAzureModulesForAccount](https://github.com/Microsoft/AzureAutomation-Account-Modules-Update) para actualizar los módulos de las cuentas de Automation.

Dado que los módulos se actualizan periódicamente por el grupo de productos, pueden producirse cambios con los cmdlets incluidos. Esta acción puede afectar negativamente los runbooks según el tipo de cambio, como cambiar el nombre de un parámetro o dejar de usar un cmdlet por completo.

Para evitar afectar los runbooks y los procesos que automatizan, pruébelos y valídelos antes de continuar. Si no tiene una cuenta de Automation dedicada pensada para este propósito, considere la posibilidad de crear una para que pueda probar muchos escenarios diferentes durante el desarrollo de los runbooks. Estas pruebas deben incluir cambios iterativos, como la actualización de los módulos de PowerShell.

Si desarrolla sus scripts de forma local, se recomienda tener localmente las mismas versiones del módulo que tiene en la cuenta de Automation al realizar pruebas, a fin de asegurarse de que obtendrá los mismos resultados. Una vez validados los resultados y aplicados los cambios necesarios, puede mover los cambios a producción.

> [!NOTE]
> Es posible que una cuenta de Automation nueva no contenga los módulos más recientes.

> [!NOTE]
> No podrá eliminar módulos globales: los módulos que Automation proporciona de forma integrada.

## <a name="considerations"></a>Consideraciones

Éstas son algunas consideraciones a tener en cuenta al usar este proceso para actualizar los módulos de Azure:

* Este runbook admite la actualización de los módulos de **Azure** y **AzureRm** de manera predeterminada. Este runbook también admite la actualización de los módulos de **Az**. Revise el [LÉAME Actualizar el runbook de módulos de Azure](https://github.com/microsoft/AzureAutomation-Account-Modules-Update/blob/master/README.md) para obtener más información sobre cómo actualizar módulos de `Az` con este runbook. Hay otros factores importantes que se deben tener en cuenta al usar los módulos de `Az` en la cuenta de Automation; para obtener más información, consulte [Uso de módulos de Az en la cuenta de Automation](az-modules.md).

* Antes de iniciar este runbook, asegúrese de que la cuenta de Automation tiene una [credencial de cuenta de ejecución de Azure](manage-runas-account.md) creada.

* Puede usar este código como un script normal de PowerShell en lugar de un runbook: simplemente inicie sesión en Azure mediante el comando [Connect-AzureRmAccount](/powershell/module/azurerm.profile/connect-azurermaccount) y, después, pase `-Login $false` al script.

* Para usar este runbook en las nubes soberanas, use el parámetro `AzureRmEnvironment` para pasar el entorno correcto al runbook.  Los valores permitidos son **AzureCloud**, **AzureChinaCloud**, **AzureGermanCloud** y **AzureUSGovernment**. Estos valores se pueden obtener si se usa `Get-AzureRmEnvironment | select Name`. Si no pasa un valor a este parámetro, el runbook de manera predeterminada será la nube pública de Azure **AzureCloud**.

* Si desea utilizar una versión específica del módulo Azure PowerShell en lugar de la última disponible en la Galería de PowerShell, pase estas versiones al parámetro opcional `ModuleVersionOverrides` del runbook **Update-AutomationAzureModulesForAccount**. Para obtener ejemplos, consulte el runbook [Update-AutomationAzureModulesForAccount.ps1](https://github.com/Microsoft/AzureAutomation-Account-Modules-Update/blob/master/Update-AutomationAzureModulesForAccount.ps1
). Los módulos de Azure PowerShell que no se mencionan en el parámetro `ModuleVersionOverrides` se actualizan con las últimas versiones de los módulos en la Galería de PowerShell. Si no se pasa nada al parámetro `ModuleVersionOverrides`, todos los módulos se actualizan con las versiones de módulo más recientes de la Galería de PowerShell. Se trata del mismo comportamiento que tiene el botón **Actualizar módulos de Azure**.

## <a name="next-steps"></a>Pasos siguientes

Visite el [runbook Actualizar módulos de Azure](https://github.com/Microsoft/AzureAutomation-Account-Modules-Update) de código abierto para más información.
