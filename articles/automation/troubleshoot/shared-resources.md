---
title: Solución de problemas relativos a errores con recursos compartidos de Azure Automation
description: Obtenga información sobre la solución de problemas relacionados con los recursos compartidos de Azure Automation que admiten runbooks.
services: automation
author: mgoedtel
ms.author: magoedte
ms.date: 03/12/2019
ms.topic: conceptual
ms.service: automation
manager: carmonm
ms.openlocfilehash: 4cea558b11d7ee7bbe838cecbd061cd487b536d2
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "79235424"
---
# <a name="troubleshoot-errors-with-shared-resources"></a>Solución de problemas relativos a errores con recursos compartidos

En este artículo se describen soluciones para resolver problemas que pueden surgir al usar recursos compartidos en Azure Automation.

## <a name="modules"></a>Módulos

### <a name="scenario-a-module-is-stuck-importing"></a><a name="module-stuck-importing"></a>Escenario: Un módulo está bloqueado en el estado de importación

#### <a name="issue"></a>Problema

Al importar o actualizar los módulos en Azure Automation, encuentra un módulo que está bloqueado en el estado de **importación**.

#### <a name="cause"></a>Causa

La importación de módulos de PowerShell es un proceso complejo de varios pasos. En este proceso existe la posibilidad de que un módulo no se importe correctamente. Si esto ocurre, el módulo que se va a importar puede bloquearse en un estado transitorio. Para más información sobre este proceso, vea [Importing a PowerShell Module](/powershell/scripting/developer/module/importing-a-powershell-module#the-importing-process) (Importación de un módulo de PowerShell).

#### <a name="resolution"></a>Solución

Para resolver este problema, debe quitar el módulo que está bloqueado en el estado de **importación** con el uso del cmdlet [Remove-AzureRmAutomationModule](/powershell/module/azurerm.automation/remove-azurermautomationmodule). A continuación, puede volver a intentar importar el módulo.

```azurepowershell-interactive
Remove-AzureRmAutomationModule -Name ModuleName -ResourceGroupName ExampleResourceGroup -AutomationAccountName ExampleAutomationAccount -Force
```

### <a name="scenario-azurerm-modules-are-stuck-importing-after-trying-to-update-them"></a><a name="update-azure-modules-importing"></a>Escenario: Se bloquea la importación de los módulos de AzureRM después de intentar actualizarlos

#### <a name="issue"></a>Problema

Después de intentar actualizar los módulos de AzureRM, en la cuenta permanece un banner con el siguiente mensaje:

```error
Azure modules are being updated
```

#### <a name="cause"></a>Causa

Hay un problema conocido con la actualización de los módulos de AzureRM de una cuenta de Automation que se encuentra en un grupo de recursos con un nombre numérico que empieza por 0.

#### <a name="resolution"></a>Solución

Para actualizar los módulos de Azure en la cuenta de Automation, esta debe estar en un grupo de recursos con un nombre alfanumérico. En este momento, los grupos de recursos con nombres numéricos que empiezan por 0 no pueden actualizar módulos de AzureRM.

### <a name="scenario-module-fails-to-import-or-cmdlets-cant-be-executed-after-importing"></a><a name="module-fails-to-import"></a>Escenario: No se puede importar el módulo o no se pueden ejecutar cmdlets después de la importación

#### <a name="issue"></a>Problema

Un módulo no se puede importar o se importa correctamente, pero no se extrae ningún cmdlet.

#### <a name="cause"></a>Causa

Algunas razones comunes por las que un módulo no se importa correctamente a Azure Automation son:

* La estructura no coincide con la estructura que Automation necesita.
* El módulo depende de otro módulo que no se ha implementado en su cuenta de Automation.
* Al módulo le faltan sus dependencias en la carpeta.
* El cmdlet `New-AzureRmAutomationModule` se está usando para cargar el módulo y no se ha proporcionado la ruta de acceso de almacenamiento completa o no se ha cargado el módulo usando una dirección URL de acceso público.

#### <a name="resolution"></a>Solución

Cualquiera de las siguientes soluciones resolverá el problema:

* Asegúrese de que el módulo sigue el formato siguiente: nombreDeMódulo.Zip **->** nombreDeMódulo o un número de versión **->** (nombreDeMódulo.psm1, nombreDeMódulo.psd1)
* Abra el archivo. psd1 y compruebe si el módulo tiene dependencias. Si es así, cargue estos módulos en la cuenta de Automation.
* Asegúrese de que todos los archivos .dll a los que se hace referencia están presentes en la carpeta del módulo.

### <a name="scenario-update-azuremoduleps1-suspends-when-updating-modules"></a><a name="all-modules-suspended"></a>Escenario: Update-AzureModule.ps1 se suspende durante la actualización de módulos

#### <a name="issue"></a>Problema

Al usar el runbook [Update-AzureModule.ps1](https://github.com/azureautomation/runbooks/blob/master/Utility/ARM/Update-AzureModule.ps1) para actualizar los módulos de Azure, se suspende el proceso de actualización.

#### <a name="cause"></a>Causa

La configuración predeterminada para determinar cuántos módulos se actualizan simultáneamente es 10 cuando se usa el script `Update-AzureModule.ps1`. El proceso de actualización es propenso a errores cuando se actualizan demasiados módulos al mismo tiempo.

#### <a name="resolution"></a>Solución

No es común que todos los módulos de AzureRM se necesiten en la misma cuenta de Automation. Se recomienda importar únicamente los módulos de AzureRM que necesite.

> [!NOTE]
> Evite importar el módulo **AzureRM**. Importar el módulo **AzureRM** hará que se importen todos los módulos **AzureRM.\*** , esto no es recomendable.

Si se suspende el proceso de actualización, deberá agregar el parámetro `SimultaneousModuleImportJobCount` al script `Update-AzureModules.ps1` y proporcionar un valor menor que el valor predeterminado, que es 10. Se recomienda que, si implementa esta lógica, comience con un valor de 3 o 5. `SimultaneousModuleImportJobCount` es un parámetro del runbook del sistema `Update-AutomationAzureModulesForAccount` que se usa para actualizar módulos de Azure. Este cambio hace que el proceso se ejecute por más tiempo, pero tiene mayores probabilidades de completarse. En el ejemplo siguiente se muestra el parámetro y su ubicación en el runbook:

 ```powershell
         $Body = @"
            {
               "properties":{
               "runbook":{
                   "name":"Update-AutomationAzureModulesForAccount"
               },
               "parameters":{
                    ...
                    "SimultaneousModuleImportJobCount":"3",
                    ... 
               }
              }
           }
"@
```

## <a name="run-as-accounts"></a>Cuentas de ejecución

### <a name="scenario-youre-unable-to-create-or-update-a-run-as-account"></a><a name="unable-create-update"></a>Escenario: No puede crear o actualizar una cuenta de ejecución

#### <a name="issue"></a>Problema

Al intentar crear o actualizar una cuenta de ejecución, recibe un error similar al siguiente mensaje de error:

```error
You do not have permissions to create…
```

#### <a name="cause"></a>Causa

No tiene los permisos que necesita para crear o actualizar la cuenta de ejecución o el recurso está bloqueado en un nivel de grupo de recursos.

#### <a name="resolution"></a>Solución

Para crear o actualizar una cuenta de ejecución, debe tener los permisos adecuados para los distintos recursos que usa la cuenta de ejecución. Para obtener información acerca de los permisos necesarios para crear o actualizar una cuenta de ejecución, consulte [Permisos para configurar cuentas de ejecución](../manage-runas-account.md#permissions).

Si el problema se debe a un bloqueo, verifique que el bloqueo esté correcto para quitarlo. A continuación, navegue hasta el recurso que está bloqueado, haga clic en el bloqueo y elija **Eliminar** para quitar el bloqueo.

### <a name="scenario-you-receive-the-error-unable-to-find-an-entry-point-named-getperadapterinfo-in-dll-iplpapidll-when-executing-a-runbook"></a><a name="iphelper"></a>Escenario: Aparece el error "No se puede encontrar el punto de entrada denominado 'GetPerAdapterInfo' en el archivo DLL 'iplpapi.dll'" al ejecutar un runbook.

#### <a name="issue"></a>Problema

Al ejecutar un runbook aparece la siguiente excepción:

```error
Unable to find an entry point named 'GetPerAdapterInfo' in DLL 'iplpapi.dll'
```

#### <a name="cause"></a>Causa

Este error suele deberse a una configuración incorrecta de la [cuenta de ejecución](../manage-runas-account.md).

#### <a name="resolution"></a>Solución

Asegúrese de que la [cuenta de ejecución](../manage-runas-account.md) está correctamente configurada. Una vez que está correctamente configurada, asegúrese de que tiene el código adecuado en el runbook para autenticarse con Azure. En el ejemplo siguiente se muestra un fragmento de código para la autenticación en Azure en un runbook mediante una cuenta de ejecución.

```powershell
$connection = Get-AutomationConnection -Name AzureRunAsConnection
Connect-AzureRmAccount -ServicePrincipal -Tenant $connection.TenantID `
-ApplicationID $connection.ApplicationID -CertificateThumbprint $connection.CertificateThumbprint
```

## <a name="next-steps"></a>Pasos siguientes

Si su problema no aparece o es incapaz de resolverlo, visite uno de nuestros canales para obtener ayuda adicional:

* Obtenga respuestas de expertos de Azure en los [foros de Azure](https://azure.microsoft.com/support/forums/).
* Póngase en contacto con [@AzureSupport](https://twitter.com/azuresupport): la cuenta de Microsoft Azure oficial para mejorar la experiencia del cliente, que pone en contacto a la comunidad de Azure con los recursos adecuados: respuestas, soporte técnico y expertos.
* Si necesita más ayuda, puede registrar un incidente de soporte técnico de Azure. Vaya al [sitio de soporte técnico de Azure](https://azure.microsoft.com/support/options/) y seleccione **Obtener soporte**.
