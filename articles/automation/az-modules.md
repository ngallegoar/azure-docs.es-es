---
title: Uso de módulos de Az en Azure Automation
description: En este artículo se proporciona información sobre el uso de módulos de Az en Azure Automation
services: automation
ms.subservice: shared-capabilities
ms.date: 02/08/2019
ms.topic: conceptual
ms.openlocfilehash: dfbf54c19aef00cbda886a4531797cda7ef3a191
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "76986111"
---
# <a name="az-module-support-in-azure-automation"></a>Compatibilidad con módulos de Az en Azure Automation

Azure Automation admite la capacidad de usar el [módulo de Az de Azure Powershell](/powershell/azure/new-azureps-module-az?view=azps-1.1.0) en sus runbooks. El módulo de Az no se importa automáticamente en las cuentas de Automation nuevas o existentes. En este artículo se describe cómo usar los módulos de Az con Azure Automation.

## <a name="considerations"></a>Consideraciones

Hay muchas cosas que se deben tener en cuenta al usar el módulo de Az en Azure Automation. Las soluciones de nivel superior puede usar runbooks y módulos en su cuenta de Automation. La edición de runbooks o la actualización de módulos puede causar problemas con sus runbooks. Debe probar todos los runbooks y las soluciones con cuidado en una cuenta de Automation independiente antes de importar los nuevos módulos de `Az`. Cualquier modificación en los módulos puede afectar negativamente la solución de [Iniciar/Detener](automation-solution-vm-management.md). No se recomienda modificar los módulos ni los runbooks en cuentas de Automation que contengan soluciones. Este comportamiento no es específico de los módulos de Az. Este comportamiento se debe tener en cuenta cuando introduzca cualquier cambio en su cuenta de Automation.

Al importar un módulo de `Az` en su cuenta de Automation, dicho módulo no se importa automáticamente en la sesión de PowerShell que usan los runbooks. Los módulos se importan en la sesión de PowerShell en las situaciones siguientes:

* Cuando un cmdlet de un módulo se invoca desde un runbook
* Cuando un runbook lo importa explícitamente con el cmdlet `Import-Module`
* Cuando otro módulo que depende del módulo se importa en una sesión de PowerShell

> [!IMPORTANT]
> Es importante asegurarse de que los runbooks de una cuenta de Automation, solo importan módulos de `Az` o de `AzureRM` en las sesiones de PowerShell que los runbooks usan y no ambos. Si `Az` se importa antes que `AzureRM` en un runbook, este se completará, pero se mostrará un [error que hace referencia al método get_SerializationSettings](troubleshoot/runbooks.md#get-serializationsettings) en los flujos de trabajo y es posible que los cmdlets no se hayan ejecutado correctamente. Si importa `AzureRM` y luego `Az`, su runbook logrará completarse, pero aparecerá un error en los flujos de trabajo que indica que tanto `Az` como `AzureRM` no se pueden importar en la misma sesión o utilizarse en el mismo runbook.

## <a name="migrating-to-az-modules"></a>Migración a módulos de Az

Se recomienda que pruebe la migración usando módulos de Az en lugar de módulos de AzureRM en una cuenta de Automation. Una vez creada dicha cuenta de Automation, puede usar los pasos siguientes para asegurarse de que la migración se realiza sin problemas:

### <a name="stop-and-unschedule-all-runbook-that-uses-azurerm-modules"></a>Detener y anular la programación de todos los runbooks que usan módulos de AzureRM

Para asegurarse de que no ejecuta ningún runbook existente que usa cmdlets `AzureRM`, debe detener y anular la programación de todos los runbooks que usan módulos de `AzureRM`. Puede ver qué programaciones existen y cuáles deben quitarse mediante la ejecución del ejemplo siguiente:

  ```powershell-interactive
  Get-AzureRmAutomationSchedule -AutomationAccountName "<AutomationAccountName>" -ResourceGroupName "<ResourceGroupName>" | Remove-AzureRmAutomationSchedule -WhatIf
  ```

Es importante revisar todas las programaciones por separado para asegurarse de que puede volver a programarlas en el futuro para los runbooks si es necesario.

### <a name="import-the-az-modules"></a>Importar los módulos de Az

Importe solamente los módulos de Az que son necesarios para sus runbooks. No importe el módulo de `Az` de acumulación, ya que incluye todos los módulos de `Az.*` que desea importar. Esta guía es la misma para todos los módulos.

El módulo [Az.Accounts](https://www.powershellgallery.com/packages/Az.Accounts/1.1.0) es una dependencia para los demás módulos de `Az.*`. Por este motivo, este módulo debe importarse a su cuenta de Automation antes de importar cualquier otro módulo.

En la cuenta de Automation, seleccione **Módulos** en **Recursos compartidos**. Haga clic en **Examinar la galería** para abrir la página **Examinar la galería**.  En la barra de búsqueda, escriba el nombre del módulo (como `Az.Accounts`). En la página Módulo de PowerShell, haga clic en **Importar** para importar el módulo en la cuenta de Automation.

![Importación de módulos desde una cuenta de Automation](media/az-modules/import-module.png)

Este proceso de importación también se puede realizar a través de la [Galería de PowerShell](https://www.powershellgallery.com) mediante la búsqueda del módulo. Una vez que encuentre el módulo, selecciónelo y, en la pestaña **Azure Automation**, haga clic en **Deploy to Azure Automation** (Implementar en Azure Automation).

![Importación de módulos directamente desde la galería](media/az-modules/import-gallery.png)

## <a name="test-your-runbooks"></a>Prueba de los runbooks

Una vez que los módulos de `Az` se importen en su cuenta de Automation, puede empezar a editar sus runbooks para usar el módulo de Az en su lugar. La mayoría de los cmdlets tienen el mismo nombre, a excepción de `AzureRM`, que se ha cambiado a `Az`. Para una lista de módulos que no siga este proceso, consulte la [lista de excepciones](/powershell/azure/migrate-from-azurerm-to-az#update-cmdlets-modules-and-parameters).

Una manera de probar sus runbooks antes de modificar su runbook para usar los nuevos cmdlets consiste en utilizar `Enable-AzureRMAlias -Scope Process` al principio de un runbook. Mediante esta incorporación a su runbook, este se puede ejecutar sin cambios.

## <a name="after-migration-details"></a>Detalles después de la migración

Una vez que se complete la migración, no inicie más los runbooks mediante módulos de `AzureRM` en la cuenta. Además, se recomienda no importar o actualizar módulos de `AzureRM` en esta cuenta. A partir de este momento, considere esta cuenta migrada a `Az` y trabaje con módulos de `Az` solamente. Cuando se cree una nueva cuenta de Automation, se instalarán los módulos de `AzureRM` existentes y los runbooks de tutorial todavía se crearán con cmdlets `AzureRM`. Estos runbooks no se deben ejecutar.

## <a name="next-steps"></a>Pasos siguientes

Para más información sobre el uso de módulos de Az, consulte [Getting started with Az module](/powershell/azure/get-started-azureps?view=azps-1.1.0) (Introducción al módulo de Az).
