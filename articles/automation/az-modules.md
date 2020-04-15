---
title: Compatibilidad con módulos de Az en Azure Automation
description: En este artículo se proporciona información sobre el uso de módulos de Az en Azure Automation.
services: automation
ms.subservice: shared-capabilities
ms.date: 02/08/2019
ms.topic: conceptual
ms.openlocfilehash: a8d6d25a2ba7f0040b13982f14f3d6081ac32f15
ms.sourcegitcommit: 0450ed87a7e01bbe38b3a3aea2a21881f34f34dd
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/03/2020
ms.locfileid: "80637996"
---
# <a name="az-module-support-in-azure-automation"></a>Compatibilidad con módulos de Az en Azure Automation

Azure Automation admite el uso del [módulo de Az de Azure PowerShell](/powershell/azure/new-azureps-module-az?view=azps-1.1.0) en los runbooks. El módulo acumulativo de Az no se importa automáticamente en las cuentas de Automation nuevas o existentes. 

## <a name="considerations"></a>Consideraciones

Hay varias cosas que se deben tener en cuenta al usar los módulos de Az en Azure Automation:

* Las soluciones de nivel superior en su cuenta de Automation pueden usar runbooks y módulos. Por lo tanto, la edición de runbooks o la actualización de módulos pueden causar problemas con las soluciones. Debe probar todos los runbooks y soluciones con cuidado en una cuenta de Automation independiente antes de importar los nuevos módulos de Az. 

* Cualquier modificación en los módulos puede afectar negativamente la solución de [Iniciar/Detener](automation-solution-vm-management.md). 

* Al importar un módulo de Az en su cuenta de Automation, dicho módulo no se importa automáticamente en la sesión de PowerShell que usan los runbooks. Los módulos se importan en la sesión de PowerShell en las situaciones siguientes:

    * Cuando un runbook invoca un cmdlet desde un módulo
    * Cuando un runbook importa el módulo explícitamente con el cmdlet `Import-Module`
    * Cuando un runbook importa otro módulo dependiente del módulo

> [!NOTE]
> No se recomienda modificar los módulos ni los runbooks en cuentas de Automation que contengan soluciones. Esta disposición no es específica de los módulos de Az. Debe tenerse en cuenta al introducir cualquier cambio en su cuenta de Automation.

> [!IMPORTANT]
> Asegúrese de que los runbooks de una cuenta de Automation importen los módulos AZ o los módulos de [AzureRM](https://www.powershellgallery.com/packages/AzureRM/6.13.1), pero no ambos, en una sesión de PowerShell. Si un runbook importa módulos de Az antes de los módulos de AzureRM, el runbook se completa. Sin embargo, se muestra un error que hace referencia al cmdlet [Get_SerializationSettings](troubleshoot/runbooks.md#get-serializationsettings) en los flujos de trabajo, y es posible que los cmdlets no se ejecuten correctamente. Si el runbook importa módulos de AzureRM antes de los módulos de Az, el runbook también se completa. Sin embargo, en este caso, recibirá un error en los flujos de trabajo que indican que Az y AzureRM no se pueden importar en la misma sesión ni usarse en el mismo runbook.

## <a name="migrating-to-az-modules"></a>Migración a módulos de Az

Se recomienda probar una migración a módulos de Az en una cuenta de Automation de prueba. Una vez creada la cuenta, puede seguir las instrucciones de esta sección para trabajar con los módulos.

### <a name="stop-and-unschedule-all-runbooks-that-use-azurerm-modules"></a>Detención y anulación de la programación de todos los runbooks que usan módulos de AzureRM

Para asegurarse de no ejecutar ningún runbook existente que use módulos de AzureRM, detenga y anule la programación de todos los runbooks afectados. Para ver qué programaciones existen y cuáles deben quitarse, ejecute un código similar al de este ejemplo:

  ```powershell-interactive
  Get-AzureRmAutomationSchedule -AutomationAccountName "<AutomationAccountName>" -ResourceGroupName "<ResourceGroupName>" | Remove-AzureRmAutomationSchedule -WhatIf
  ```

Es importante revisar todas las programaciones por separado para asegurarse de que puede volver a programarlas en el futuro para los runbooks, si es necesario.

### <a name="import-the-az-modules"></a>Importar los módulos de Az

>[!NOTE]
>Haga que la los runbooks importen solo los módulos de Az necesarios. No importe el módulo acumulativo de Az, ya que incluye todos los módulos de Az. Esta guía es la misma para todos los módulos.

El módulo [Az.Accounts](https://www.powershellgallery.com/packages/Az.Accounts/1.1.0) es una dependencia para los demás módulos de Az. Por este motivo, los runbooks deben importar este módulo a su cuenta de Automation antes de importar cualquier otro módulo.

Para importar los módulos en Azure Portal:

1. En la cuenta de Automation, seleccione **Módulos** en **Recursos compartidos**. 
2. Haga clic en **Examinar la galería** para abrir la página Examinar galería.  
3. En la barra de búsqueda, escriba el nombre del módulo, por ejemplo, `Az.Accounts`. 
4. En la página Módulo de PowerShell, haga clic en **Importar** para importar el módulo en la cuenta de Automation.

![Importación de módulos desde una cuenta de Automation](media/az-modules/import-module.png)

Este proceso de importación también se puede realizar a través de la [Galería de PowerShell](https://www.powershellgallery.com) mediante la búsqueda del módulo que se va a importar. Una vez que encuentre el módulo, selecciónelo y elija la pestaña **Azure Automation**, haga clic en **Implementar en Azure Automation**.

![Importación de módulos directamente desde la galería](media/az-modules/import-gallery.png)

## <a name="testing-your-runbooks"></a>Prueba de los runbooks

Una vez que los módulos de Az se importen en su cuenta de Automation, puede empezar a editar los runbooks para usar los módulos. La mayoría de los cmdlets tienen los mismos nombres que para el módulo de AzureRM, excepto que el prefijo AzureRM (o AzureRm) se ha cambiado a Az. Para obtener una lista de módulos que no siguen esta convención de nomenclatura, consulte la [lista de excepciones](/powershell/azure/migrate-from-azurerm-to-az#update-cmdlets-modules-and-parameters).

Una manera de probar la modificación de un runbook para usar los nuevos cmdlets consiste en utilizar `Enable-AzureRmAlias -Scope Process` al principio del runbook. Al incorporar este comando al runbook, el script se puede ejecutar sin cambios.

## <a name="after-migration-details"></a>Detalles posteriores a la migración

Una vez que se complete la migración, no intente iniciar los runbooks mediante los módulos de AzureRM en la cuenta de Automation. Además, se recomienda no importar ni actualizar los módulos de AzureRM en la cuenta. Considere la cuenta como migrada a Az y use módulos de Az únicamente. 

Al crear una nueva cuenta de Automation, los módulos de AzureRM existentes siguen instalados. Todavía puede actualizar los runbooks del tutorial con los cmdlets de AzureRM. Sin embargo, no debe ejecutar estos runbooks.

## <a name="next-steps"></a>Pasos siguientes

Para más información sobre el uso de módulos de Az, consulte [Getting started with Az module](/powershell/azure/get-started-azureps?view=azps-1.1.0) (Introducción al módulo de Az).
