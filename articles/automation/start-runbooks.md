---
title: Inicio de un runbook en Azure Automation
description: Resume los distintos métodos que pueden usarse para iniciar un runbook de Azure Automation y proporciona detalles sobre cómo utilizar el Portal de Azure y Windows PowerShell.
services: automation
ms.subservice: process-automation
ms.date: 03/16/2018
ms.topic: conceptual
ms.openlocfilehash: 7f2c0dda952959db3bffba6016f48b986016c19e
ms.sourcegitcommit: acb82fc770128234f2e9222939826e3ade3a2a28
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/21/2020
ms.locfileid: "81679464"
---
# <a name="start-a-runbook-in-azure-automation"></a>Inicio de un runbook en Azure Automation

La tabla siguiente le ayuda a determinar el método para iniciar un runbook en Azure Automation que sea más adecuado para su escenario concreto. Este artículo incluye detalles acerca de cómo iniciar un runbook con Azure Portal y Windows PowerShell. En otra documentación a la que puede acceder desde los siguientes vínculos se proporcionan detalles sobre los otros métodos.

| **Método** | **Características** |
| --- | --- |
| [Azure Portal](#start-a-runbook-with-the-azure-portal) |<li>Método más sencillo con la interfaz de usuario interactiva.<br> <li>Forma para proporcionar valores de parámetro simples.<br> <li>Seguimiento sencillo del estado del trabajo.<br> <li>Acceso autenticado con inicio de sesión de Azure. |
| [Windows PowerShell](/powershell/module/azurerm.automation/start-azurermautomationrunbook) |<li>Permite llamar desde la línea de comandos con los cmdlets de Windows PowerShell.<br> <li>Puede incluirse en una solución automatizada con varios pasos.<br> <li>La solicitud se autentica con el certificado o el usuario de OAuth principal/servicio principal.<br> <li>Permite proporcionar valores de parámetro simples y complejos.<br> <li>Realizar el seguimiento del estado del trabajo.<br> <li>Responder a la necesidad del cliente para admitir los cmdlets de PowerShell. |
| [API de Azure Automation](/rest/api/automation/) |<li>El método más flexible, pero también más complejo.<br> <li>Permite llamar desde cualquier código personalizado que pueda realizar solicitudes HTTP.<br> <li>La solicitud se autentica con el certificado o el usuario de OAuth principal/servicio principal.<br> <li>Permite proporcionar valores de parámetro simples y complejos. *Si va a llamar a un runbook de Python mediante la API, la carga de JSON se tiene que serializar.*<br> <li>Realizar el seguimiento del estado del trabajo. |
| [Webhooks](automation-webhooks.md) |<li>Permite iniciar el runbook desde una única solicitud HTTP.<br> <li>Autenticado con el token de seguridad de la dirección URL.<br> <li>El cliente no puede reemplazar los valores de parámetro especificados al crear webhook. El runbook puede definir un único parámetro que se rellena con los detalles de la solicitud HTTP.<br> <li>No tiene capacidad de realizar un seguimiento del estado del trabajo a través de la dirección URL de webhook. |
| [Respuesta a una alerta de Azure](../log-analytics/log-analytics-alerts.md) |<li>Permite iniciar un runbook en respuesta a una alerta de Azure.<br> <li>Permite configurar webhook para el runbook y vincular a la alerta.<br> <li>Autenticado con el token de seguridad de la dirección URL. |
| [Programación](automation-schedules.md) |<li>Permite iniciar automáticamente el runbook en una programación según la hora, diaria, semanal o mensual.<br> <li>Permite manipular la programación a través de Azure Portal, los cmdlets de PowerShell o la API de Azure.<br> <li>Permite proporcionar valores de parámetro que se usarán con la programación. |
| [Desde otro Runbook](automation-child-runbooks.md) |<li>Permite usar un runbook como una actividad en otro runbook.<br> <li>Es útil para la funcionalidad utilizada por varios runbooks.<br> <li>Permite proporcionar valores de parámetro a un runbook secundario y usar la salida de un runbook principal. |

La siguiente imagen ilustra el proceso paso a paso detallado en el ciclo de vida de un runbook. En ella figuran las distintas formas de iniciar un runbook en Azure Automation, los componentes requeridos para que Hybrid Runbook Worker ejecute runbooks de Azure Automation y las interacciones entre los distintos componentes. Para obtener información sobre cómo ejecutar runbooks de Automation en su centro de datos, consulte [Trabajos híbridos de runbook](automation-hybrid-runbook-worker.md)

![Arquitectura de runbook](media/automation-starting-runbook/runbooks-architecture.png)

>[!NOTE]
>Este artículo se ha actualizado para usar el nuevo módulo Az de Azure PowerShell. Aún puede usar el módulo de AzureRM que continuará recibiendo correcciones de errores hasta diciembre de 2020 como mínimo. Para más información acerca del nuevo módulo Az y la compatibilidad con AzureRM, consulte [Introducing the new Azure PowerShell Az module](https://docs.microsoft.com/powershell/azure/new-azureps-module-az?view=azps-3.5.0) (Presentación del nuevo módulo Az de Azure PowerShell). Para obtener instrucciones sobre la instalación del módulo Az en Hybrid Runbook Worker, consulte [Instalación del módulo de Azure PowerShell](https://docs.microsoft.com/powershell/azure/install-az-ps?view=azps-3.5.0). Puede actualizar los módulos de su cuenta de Automation a la versión más reciente mediante [Actualización de módulos de Azure PowerShell en Azure Automation](automation-update-azure-modules.md).

## <a name="runbook-parameters"></a>Parámetros de runbook

Cuando se inicia un runbook desde Azure Portal o Windows PowerShell, la instrucción se envía a través del servicio web Azure Automation. Este servicio no admite parámetros con tipos de datos complejos. Si necesita proporcionar un valor para un parámetro complejo, se debe llamar en línea desde otro runbook, tal como se describe en [Runbooks secundarios en Azure Automation](automation-child-runbooks.md).

El servicio web de Azure Automation proporciona una funcionalidad especial para parámetros que usan ciertos tipos de datos, tal como se describe en las secciones siguientes:

### <a name="named-values"></a>Valores con nombre

Si el parámetro tiene el tipo de datos [object], puede usar el siguiente formato JSON para enviar una lista de valores con nombre: *{"Nombre1":Valor1, "Nombre2":Valor2, "Nombre3":Valor3}* . Estos valores deben ser tipos simples. El runbook recibe el parámetro como un elemento [PSCustomObject](/dotnet/api/system.management.automation.pscustomobject) con propiedades que corresponden a cada valor con nombre.

Considere el siguiente runbook de prueba que acepta un parámetro denominado user.

```powershell
Workflow Test-Parameters
{
   param (
      [Parameter(Mandatory=$true)][object]$user
   )
    $userObject = $user | ConvertFrom-JSON
    if ($userObject.Show) {
        foreach ($i in 1..$userObject.RepeatCount) {
            $userObject.FirstName
            $userObject.LastName
        }
    }
}
```

El texto siguiente podría utilizarse para el parámetro de usuario.

```json
{FirstName:'Joe',LastName:'Smith',RepeatCount:'2',Show:'True'}
```

Esta acción devuelve la siguiente salida:

```output
Joe
Smith
Joe
Smith
```

### <a name="arrays"></a>Matrices

Si el parámetro es una matriz como [array] o [string], puede usar entonces el siguiente formato JSON para enviarle una lista de valores: *[Value1,Value2,Value3]* . Estos valores deben ser tipos simples.

Considere el siguiente runbook de prueba que acepta un parámetro denominado *user*.

```powershell
Workflow Test-Parameters
{
   param (
      [Parameter(Mandatory=$true)][array]$user
   )
    if ($user[3]) {
        foreach ($i in 1..$user[2]) {
            $ user[0]
            $ user[1]
        }
    }
}
```

El texto siguiente podría utilizarse para el parámetro de usuario.

```input
["Joe","Smith",2,true]
```

Esta acción devuelve la siguiente salida:

```output
Joe
Smith
Joe
Smith
```

### <a name="credentials"></a>Credenciales

Si el parámetro es de tipo de datos `PSCredential`, puede proporcionar el nombre de un [recurso de credenciales](automation-credentials.md) de Azure Automation. El runbook recupera la credencial con el nombre que especifique. El runbook de prueba siguiente acepta un parámetro denominado `credential`.

```powershell
Workflow Test-Parameters
{
   param (
      [Parameter(Mandatory=$true)][PSCredential]$credential
   )
   $credential.UserName
}
```

El texto siguiente se puede utilizar para el parámetro user suponiendo la existencia previa de un recurso de credenciales denominado `My Credential`.

```input
My Credential
```

Suponiendo que el nombre de usuario de la credencial es `jsmith`, se muestra la salida siguiente.

```output
jsmith
```

## <a name="start-a-runbook-with-the-azure-portal"></a>Inicio de un runbook con Azure Portal

1. En Azure Portal, seleccione **Automation** y, a continuación, haga clic en el nombre de una cuenta de Automation.
2. En el menú central, seleccione **Runbooks**.
3. En la página Runbooks, seleccione un runbook y, a continuación, haga clic en **Iniciar**.
4. Si el runbook tiene parámetros, se le pide que proporcione los valores de cada parámetro con un cuadro de texto. Para más información sobre los parámetros, consulte [Parámetros de runbook](#runbook-parameters).
5. En el panel Trabajo, puede ver el estado del trabajo de runbook.

## <a name="start-a-runbook-with-powershell"></a>Inicio de un runbook con PowerShell

Puede utilizar [Start-AzureRmAutomationRunbook](https://docs.microsoft.com/powershell/module/az.automation/start-azautomationrunbook?view=azps-3.7.0) para iniciar un runbook con Windows PowerShell. El código de ejemplo siguiente inicia un runbook denominado **Test-Runbook**.

```azurepowershell-interactive
Start-AzAutomationRunbook -AutomationAccountName "MyAutomationAccount" -Name "Test-Runbook" -ResourceGroupName "ResourceGroup01"
```

`Start-AzAutomationRunbook` devuelve un objeto de trabajo que puede usar para realizar un seguimiento del estado una vez que se inicia el runbook. A continuación, puede usar este objeto de trabajo con [Get-AzureAutomationJob](https://docs.microsoft.com/powershell/module/Az.Automation/Get-AzAutomationJob?view=azps-3.7.0) para determinar el estado del trabajo y [Get-AzureAutomationJobOutput](https://docs.microsoft.com/powershell/module/az.automation/get-azautomationjoboutput?view=azps-3.7.0) para obtener su salida. El ejemplo siguiente inicia un runbook denominado **Test-Runbook**, espera hasta que se ha completado y después muestra la salida.

```azurepowershell-interactive
$runbookName = "Test-Runbook"
$ResourceGroup = "ResourceGroup01"
$AutomationAcct = "MyAutomationAccount"

$job = Start-AzAutomationRunbook –AutomationAccountName $AutomationAcct -Name $runbookName -ResourceGroupName $ResourceGroup

$doLoop = $true
While ($doLoop) {
   $job = Get-AzAutomationJob –AutomationAccountName $AutomationAcct -Id $job.JobId -ResourceGroupName $ResourceGroup
   $status = $job.Status
   $doLoop = (($status -ne "Completed") -and ($status -ne "Failed") -and ($status -ne "Suspended") -and ($status -ne "Stopped"))
}

Get-AzAutomationJobOutput –AutomationAccountName $AutomationAcct -Id $job.JobId -ResourceGroupName $ResourceGroup –Stream Output
```

Si el runbook requiere parámetros, tiene que proporcionarlos como una [tabla hash](https://technet.microsoft.com/library/hh847780.aspx). La clave de la tabla hash tiene que coincidir con el nombre del parámetro y el valor es el valor del parámetro. En el ejemplo siguiente se muestra cómo iniciar un runbook con dos parámetros de cadena denominados FirstName y LastName, un número entero denominado RepeatCount y un parámetro booleano denominado Show. Para más información sobre los parámetros, consulte [Parámetros de runbook](#runbook-parameters).

```azurepowershell-interactive
$params = @{"FirstName"="Joe";"LastName"="Smith";"RepeatCount"=2;"Show"=$true}
Start-AzureRmAutomationRunbook –AutomationAccountName "MyAutomationAccount" –Name "Test-Runbook" -ResourceGroupName "ResourceGroup01" –Parameters $params
```

## <a name="next-steps"></a>Pasos siguientes

* Para obtener información sobre cómo ejecutar runbooks de Automation en su centro de datos, consulte [Trabajos híbridos de runbook](automation-hybrid-runbook-worker.md).
* Para aprender más sobre la creación runbooks modulares para usarse por otros runbooks para funciones específicas o comunes, consulte [Runbooks secundarios](automation-child-runbooks.md).
* Para obtener más información sobre PowerShell, incluidos los módulos de referencia de lenguaje y aprendizaje, consulte la [documentación de PowerShell](https://docs.microsoft.com/powershell/scripting/overview).
