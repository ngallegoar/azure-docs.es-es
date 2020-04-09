---
title: Solución de problemas relativos a errores con runbooks de Azure Automation
description: Obtenga información sobre la solución de problemas que puede encontrar con los runbook de Azure Automation.
services: automation
author: mgoedtel
ms.author: magoedte
ms.date: 01/24/2019
ms.topic: conceptual
ms.service: automation
manager: carmonm
ms.openlocfilehash: 26c5c5b31d5f3f9e1a642c0bafb947190e479055
ms.sourcegitcommit: d597800237783fc384875123ba47aab5671ceb88
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/03/2020
ms.locfileid: "80632626"
---
# <a name="troubleshoot-errors-with-runbooks"></a>Solución de problemas relativos a errores con runbooks

Si se producen errores al ejecutar runbooks en Azure Automation, puede usar los pasos siguientes para ayudar a diagnosticar los problemas.

1. **Asegúrese de que el script de runbook se ejecuta correctamente en la máquina local.** 

    consulte en la [documentación de PowerShell](/powershell/scripting/overview) o la [documentación de Python](https://docs.python.org/3/) los módulos de referencia de lenguaje y aprendizaje. La ejecución local del script puede detectar y resolver errores comunes, como, por ejemplo:

      * Módulos que faltan
      * Errores de sintaxis
      * Errores lógicos

2. **Investigue los [flujos de error](https://docs.microsoft.com/azure/automation/automation-runbook-output-and-messages#runbook-output) de runbook.**

    Examine estos flujos en busca de mensajes específicos y compárelos con los errores documentados en este artículo.

3. **Asegúrese de que los nodos y el área de trabajo de Automation tengan los módulos necesarios.** 

    Si el runbook importa algún módulo, compruebe que está disponible para la cuenta de Automation mediante los pasos indicados en [Importación de módulos](../shared-resources/modules.md#importing-modules). Actualice los módulos a la versión más reciente siguiendo las instrucciones del artículo [Actualización de módulos de Azure en Azure Automation](..//automation-update-azure-modules.md). Para más información sobre solución de problemas, consulte [Solución de problemas de módulos](shared-resources.md#modules).

4. **Haga lo siguiente si el runbook se suspende o registra un error inesperado.**

    * [Compruebe los estados del trabajo](https://docs.microsoft.com/azure/automation/automation-runbook-execution#job-statuses), que definen los estados de los runbooks y algunas causas posibles.
    * [Agregue datos de salida adicionales](https://docs.microsoft.com/azure/automation/automation-runbook-output-and-messages#message-streams) al runbook para identificar lo que sucede antes de que se suspenda el runbook.
    * [Controle las excepciones](https://docs.microsoft.com/azure/automation/automation-runbook-execution#handling-exceptions) producidas por su trabajo.

## <a name="scenario-run-login-azurermaccount-to-log-in"></a><a name="login-azurerm"></a>Escenario: Ejecución de Login-AzureRMAccount para iniciar sesión

### <a name="issue"></a>Problema

Recibe el error siguiente cuando ejecuta un runbook:

```error
Run Login-AzureRMAccount to login.
```

### <a name="cause"></a>Causa

Este error puede producirse cuando no usa una cuenta de ejecución o esta ha expirado. Consulte [Administración de cuentas de ejecución de Azure Automation](https://docs.microsoft.com/azure/automation/manage-runas-account).

Este error tiene dos causas principales:

* Hay versiones diferentes del módulo de AzureRM o Az.
* Intenta acceder a los recursos de otra suscripción.

### <a name="resolution"></a>Solución

Si recibe este error después de actualizar un módulo de AzureRM o Az, debe actualizar todos los módulos a la misma versión.

Si intenta acceder a los recursos de otra suscripción, puede seguir los pasos siguientes para configurar los permisos.

1. Vaya a la cuenta de ejecución de Automation y copie el id. de la aplicación y su huella digital.
  ![Copia del id. de la aplicación y su huella digital](../media/troubleshoot-runbooks/collect-app-id.png)
1. Vaya a Access Control de la suscripción donde la cuenta de Automation NO está hospedada y agregue una asignación de roles nueva.
  ![Control de acceso](../media/troubleshoot-runbooks/access-control.png)
1. Agregue el id. de la aplicación que recopiló anteriormente. Seleccione los permisos de colaborador.
   ![Agregar asignación de roles](../media/troubleshoot-runbooks/add-role-assignment.png)
1. Copie el nombre de la suscripción.
1. Ahora puede usar el código del runbook siguiente para probar los permisos de la cuenta de Automation en la otra suscripción. Reemplace `"\<CertificateThumbprint\>"` por el valor que copió en el paso 1. Reemplace `"\<SubscriptionName\>"` por el valor que copió en el paso 4.

    ```powershell
    $Conn = Get-AutomationConnection -Name AzureRunAsConnection
    Connect-AzureRmAccount -ServicePrincipal -Tenant $Conn.TenantID -ApplicationId $Conn.ApplicationID -CertificateThumbprint "<CertificateThumbprint>"
    #Select the subscription you want to work with
    Select-AzureRmSubscription -SubscriptionName '<YourSubscriptionNameGoesHere>'

    #Test and get outputs of the subscriptions you granted access.
    $subscriptions = Get-AzureRmSubscription
    foreach($subscription in $subscriptions)
    {
        Set-AzureRmContext $subscription
        Write-Output $subscription.Name
    }
    ```

## <a name="scenario-unable-to-find-the-azure-subscription"></a><a name="unable-to-find-subscription"></a>Escenario: No se encuentra la suscripción de Azure

### <a name="issue"></a>Problema

Recibirá el siguiente error al trabajar con los cmdlet `Select-AzureSubscription` o `Select-AzureRmSubscription`:

```error
The subscription named <subscription name> cannot be found.
```

### <a name="error"></a>Error

Este error se puede generar si:

* El nombre de la suscripción no es válido.
* El usuario de Azure Active Directory que intenta obtener los detalles de la suscripción no está configurado como administrador de la suscripción.

### <a name="resolution"></a>Solución

Siga los pasos a continuación para determinar si se ha autenticado en Azure y tiene acceso a la suscripción que intenta seleccionar.

1. Para asegurarse de que el script funciona de forma independiente, pruébelo fuera de Azure Automation.
2. Asegúrese de que el script ejecute el cmdlet `Add-AzureAccount` antes que el cmdlet `Select-AzureSubscription`.
3. Agregue `Disable-AzureRmContextAutosave –Scope Process` al principio del runbook. La llamada de este cmdlet garantiza que las credenciales solo se aplican a la ejecución del runbook actual.
4. Si continúa recibiendo este mensaje de error, modifique el código. Para ello, agregue el parámetro `AzureRmContext` para el cmdlet `Add-AzureAccount` y luego ejecute el código.

   ```powershell
   Disable-AzureRmContextAutosave –Scope Process

   $Conn = Get-AutomationConnection -Name AzureRunAsConnection
   Connect-AzureRmAccount -ServicePrincipal -Tenant $Conn.TenantID -ApplicationId $Conn.ApplicationID -CertificateThumbprint $Conn.CertificateThumbprint

   $context = Get-AzureRmContext

   Get-AzureRmVM -ResourceGroupName myResourceGroup -AzureRmContext $context
    ```

## <a name="scenario-authentication-to-azure-failed-because-multi-factor-authentication-is-enabled"></a><a name="auth-failed-mfa"></a>Escenario: Error de autenticación en Azure porque está habilitada la autenticación multifactor

### <a name="issue"></a>Problema

Recibirá el siguiente error al autenticarse en Azure con el nombre de usuario y la contraseña de Azure:

```error
Add-AzureAccount: AADSTS50079: Strong authentication enrollment (proof-up) is required
```

### <a name="cause"></a>Causa

Si dispone de autenticación multifactor en su cuenta de Azure, no puede usar un usuario de Azure Active Directory para autenticarse en Azure. En su lugar, tiene que usar un certificado o una entidad de servicio para autenticarse.

### <a name="resolution"></a>Solución

Para usar un certificado con los cmdlets del modelo de implementación clásica de Azure, consulte el artículo sobre [Creación y adición de un certificado para administrar servicios de Azure](https://blogs.technet.com/b/orchestrator/archive/2014/04/11/managing-azure-services-with-the-microsoft-azure-automation-preview-service.aspx). Para usar una entidad de servicio con los cmdlets de Azure Resource Manager, consulte [Creación de una entidad de servicio mediante Azure Portal](../../active-directory/develop/howto-create-service-principal-portal.md) y [Autenticación de una entidad de servicio con Azure Resource Manager](../../active-directory/develop/howto-authenticate-service-principal-powershell.md).

## <a name="scenario-you-see-an-error-in-your-job-streams-about-the-get_serializationsettings-method"></a><a name="get-serializationsettings"></a>Escenario: se muestra un error en sus flujos de trabajo sobre el método get_SerializationSettings

### <a name="issue"></a>Problema

Se muestra el siguiente error en sus flujos de trabajo para un runbook:

```error
Connect-AzureRMAccount : Method 'get_SerializationSettings' in type
'Microsoft.Azure.Management.Internal.Resources.ResourceManagementClient' from assembly
'Microsoft.Azure.Commands.ResourceManager.Common, Version=4.0.0.0, Culture=neutral, PublicKeyToken=31bf3856ad364e35'
does not have an implementation.
At line:16 char:1
+ Connect-AzureRMAccount -ServicePrincipal -Tenant $Conn.TenantID -Appl ...
+ ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
    + CategoryInfo          : NotSpecified: (:) [Connect-AzureRmAccount], TypeLoadException
    + FullyQualifiedErrorId : System.TypeLoadException,Microsoft.Azure.Commands.Profile.ConnectAzureRmAccountCommand
```

### <a name="cause"></a>Causa

Este error se debe al uso de cmdlets de módulo tanto de AzureRM como de Az en un runbook. Se produce al importar el módulo de Az antes de importar el módulo de AzureRM.

### <a name="resolution"></a>Solución

Los cmdlets de Az y AzureRM no se pueden importar y usar en el mismo runbook. Para obtener más información acerca de los cmdlets de Az en Azure Automation, consulte [Compatibilidad con módulos de Az en Azure Automation](../az-modules.md).

## <a name="scenario-the-runbook-fails-with-the-error-a-task-was-canceled"></a><a name="task-was-cancelled"></a>Escenario: Se produce el siguiente error en el runbook: Se ha cancelado una tarea

### <a name="issue"></a>Problema

El runbook genera un error similar al ejemplo siguiente:

```error
Exception: A task was canceled.
```

### <a name="cause"></a>Causa

Este error puede deberse al uso de módulos de Azure obsoletos.

### <a name="resolution"></a>Solución

Puede solucionar este error con la actualización de los módulos de Azure a la versión más reciente. 

1. En la cuenta de Automation, haga clic en **Módulos** y en **Actualizar módulos de Azure**. 
2. La actualización tarda aproximadamente 15 minutos. Una vez que haya finalizado, vuelva a ejecutar el runbook en el que se produjo el error.

Para más información acerca de cómo actualizar los módulos, consulte [Actualización de módulos de Azure en Azure Automation](../automation-update-azure-modules.md).

## <a name="scenario-runbooks-fail-when-dealing-with-multiple-subscriptions"></a><a name="runbook-auth-failure"></a>Escenario: se produce un error en los runbooks cuando se trabaja con varias suscripciones

### <a name="issue"></a>Problema

Cuando se ejecuta un runbook, este no puede administrar los recursos de Azure.

### <a name="cause"></a>Causa

El runbook no está utilizando el contexto correcto cuando se ejecuta.

### <a name="resolution"></a>Solución

El contexto de la suscripción podría perderse cuando un runbook invoca varios runbooks. Para asegurarse de que el contexto de la suscripción se pasa a los runbooks, haga que el runbook cliente pase el contexto al cmdlet `Start-AzureRmAutomationRunbook` en el parámetro `AzureRmContext`. Use el cmdlet `Disable-AzureRmContextAutosave` con el parámetro `Scope` establecido en `Process` para garantizar que las credenciales especificadas solo se usan para el runbook actual. Para más información, consulte [Trabajo con varias suscripciones](../automation-runbook-execution.md#working-with-multiple-subscriptions).

```azurepowershell-interactive
# Ensures that any credentials apply only to the execution of this runbook
Disable-AzureRmContextAutosave –Scope Process

# Connect to Azure with Run As account
$ServicePrincipalConnection = Get-AutomationConnection -Name 'AzureRunAsConnection'

Add-AzureRmAccount `
    -ServicePrincipal `
    -TenantId $ServicePrincipalConnection.TenantId `
    -ApplicationId $ServicePrincipalConnection.ApplicationId `
    -CertificateThumbprint $ServicePrincipalConnection.CertificateThumbprint

$AzureContext = Select-AzureRmSubscription -SubscriptionId $ServicePrincipalConnection.SubscriptionID

$params = @{"VMName"="MyVM";"RepeatCount"=2;"Restart"=$true}

Start-AzureRmAutomationRunbook `
    –AutomationAccountName 'MyAutomationAccount' `
    –Name 'Test-ChildRunbook' `
    -ResourceGroupName 'LabRG' `
    -AzureRmContext $AzureContext `
    –Parameters $params –wait
```

## <a name="scenario-term-not-recognized-as-the-name-of-a-cmdlet-function-script"></a><a name="not-recognized-as-cmdlet"></a>Escenario: hay un término que no se reconoce como el nombre de un cmdlet, función o script

### <a name="issue"></a>Problema

El runbook genera un error similar al ejemplo siguiente:

```error
The term 'Connect-AzureRmAccount' is not recognized as the name of a cmdlet, function, script file, or operable program. Check the spelling of the name, or if the path was included verify that the path is correct and try again.
```

### <a name="cause"></a>Causa

Este error puede deberse a los siguientes motivos:

* El módulo que contiene el cmdlet no está importado en la cuenta de Automation.
* El módulo que contiene el cmdlet está importado, pero no está actualizado.

### <a name="resolution"></a>Solución

Realice una de las siguientes tareas para solucionar este error. 

* En el caso de un módulo de Azure, consulte [Actualización de módulos de Azure PowerShell en Azure Automation](../automation-update-azure-modules.md) para obtener información sobre cómo actualizar los módulos en su cuenta de Automation.

* En el caso de un módulo que no es de Azure, asegúrese de que el módulo se importa en su cuenta de Automation.

## <a name="scenario-the-runbook-job-start-was-attempted-three-times-but-failed-to-start-each-time"></a><a name="job-attempted-3-times"></a>Escenario: Se intentó iniciar el trabajo del runbook tres veces, pero en las tres se produjo un error.

### <a name="issue"></a>Problema

Se produce el siguiente error en el runbook:

```error
The job was tried three times but it failed
```

### <a name="cause"></a>Causa

Este error se produce debido a uno de los siguientes problemas:

* Límite de memoria. Un trabajo puede producir un error si emplea más de 400 MB de memoria. Los límites documentados sobre la memoria que se asigna a un espacio aislado se encuentran en [Límites del servicio Automation](../../azure-resource-manager/management/azure-subscription-service-limits.md#automation-limits). 

* Sockets de red. Los espacios aislados de Azure están limitados a 1000 sockets de red simultáneos. Consulte [Límites del servicio Automation](../../azure-resource-manager/management/azure-subscription-service-limits.md#automation-limits).

* Módulo incompatible. Es posible que las dependencias del módulo no sean correctas. En este caso, el runbook suele devolver el mensaje `Command not found` o `Cannot bind parameter`.

* No se realizó ninguna autenticación con Active Directory para espacios aislados. El runbook intentó llamar a un archivo ejecutable o a un subproceso que se ejecuta en un espacio aislado de Azure. No se admite la configuración de runbooks para autenticarse con Azure AD mediante la Biblioteca de autenticación de Azure Active Directory (ADAL).

* Demasiados datos de excepción. El runbook intentó escribir demasiados datos de excepción en el flujo de salida.

### <a name="resolution"></a>Solución

* Límite de memoria, sockets de red. Algunas formas sugeridas para trabajar dentro del límite de memoria son dividir la carga de trabajo entre varios runbooks, procesar menos datos en la memoria, evitar escribir resultados innecesarios de los runbooks y considerar cuántos puntos de control se escriben en los runbooks de flujo de trabajo de PowerShell. Use el método clear, como `$myVar.clear`, para borrar las variables y utilice `[GC]::Collect` para ejecutar inmediatamente la recolección de elementos no utilizados. Estas acciones reducen la superficie de memoria de su runbook en tiempo de ejecución.

* Módulo incompatible. Actualice los módulos de Azure siguiendo los pasos en [Actualización de módulos de Azure PowerShell en Azure Automation](../automation-update-azure-modules.md).

* No se realizó ninguna autenticación con Active Directory para espacios aislados. Al autenticarse en Azure AD con un runbook, asegúrese de que el módulo de Azure AD está disponible en la cuenta de Automation. Asegúrese de conceder a la cuenta de ejecución los permisos necesarios para realizar las tareas que el runbook automatiza.

  Si el runbook no puede llamar a un archivo ejecutable o a un subproceso que se ejecuta en un espacio aislado de Azure, use el runbook en una instancia de [Hybrid Runbook Worker](../automation-hrw-run-runbooks.md). Los roles de Hybrid Worker no están limitados por los límites de memoria y de red como lo están los espacios aislados de Azure.

* Demasiados datos de excepción. Hay un límite de 1 MB en el flujo de salida del trabajo. Asegúrese de que el runbook incluye las llamadas a un archivo ejecutable o a un subproceso mediante bloques `try` y `catch`. Si las operaciones producen una excepción, haga que el código escriba el mensaje de la excepción en una variable de Automation. Esta técnica impedirá que el mensaje se escriba en el flujo de salida del trabajo.

## <a name="scenario-sign-in-to-azure-account-failed"></a><a name="sign-in-failed"></a>Escenario: Error al iniciar sesión en la cuenta de Azure

### <a name="issue"></a>Problema

Se produce uno de los siguientes errores al usar los cmdlets `Add-AzureAccount` o `Connect-AzureRmAccount`:

```error
Unknown_user_type: Unknown User Type
```

```error
No certificate was found in the certificate store with thumbprint
```

### <a name="cause"></a>Causa

Estos errores se producen si el nombre de recurso de credencial no es válido. También pueden producirse si el nombre de usuario y la contraseña que usó para configurar el recurso de credencial de Automation no son válidos.

### <a name="resolution"></a>Solución

Para determinar cuál es el problema, siga estos pasos:

1. Asegúrese de no haber incluido ningún carácter especial. Entre estos se incluye el carácter `\@` en el nombre del recurso de credencial de Automation que esté usando para conectarse a Azure.
2. Compruebe que puede usar el nombre de usuario y la contraseña que están almacenados en la credencial de Azure Automation en su editor de ISE de PowerShell local. Ejecute los siguientes cmdlets en el ISE de PowerShell.

   ```powershell
   $Cred = Get-Credential
   #Using Azure Service Management
   Add-AzureAccount –Credential $Cred
   #Using Azure Resource Manager
   Connect-AzureRmAccount –Credential $Cred
   ```

3. Si la autenticación falla de forma local, no ha configurado correctamente las credenciales de Azure Active Directory. Consulte la entrada de blog [Autenticación en Azure mediante Azure Active Directory](https://azure.microsoft.com/blog/azure-automation-authenticating-to-azure-using-azure-active-directory/) para configurar correctamente la cuenta de Azure Active Directory.

4. Si el error parece ser transitorio, intente agregar lógica de reintento a su rutina de autenticación para hacer de la autenticación un proceso más sólido.

   ```powershell
   # Get the connection "AzureRunAsConnection"
   $connectionName = "AzureRunAsConnection"
   $servicePrincipalConnection = Get-AutomationConnection -Name $connectionName

   $logonAttempt = 0
   $logonResult = $False

   while(!($connectionResult) -And ($logonAttempt -le 10))
   {
       $LogonAttempt++
       #Logging in to Azure...
       $connectionResult = Connect-AzureRmAccount `
                              -ServicePrincipal `
                              -TenantId $servicePrincipalConnection.TenantId `
                              -ApplicationId $servicePrincipalConnection.ApplicationId `
                              -CertificateThumbprint $servicePrincipalConnection.CertificateThumbprint

       Start-Sleep -Seconds 30
   }
   ```

## <a name="scenario-object-reference-not-set-to-an-instance-of-an-object"></a><a name="child-runbook-object"></a>Escenario: Referencia a objeto no establecida como instancia de un objeto

### <a name="issue"></a>Problema

Recibe el siguiente error cuando invoca un runbook secundario con el parámetro `Wait` y el flujo de salida contiene un objeto:

```error
Object reference not set to an instance of an object
```

### <a name="cause"></a>Causa

`Start-AzureRmAutomationRunbook` no controla el flujo de salida correctamente si dicho flujo contiene objetos.

### <a name="resolution"></a>Solución

Se recomienda que implemente una lógica de sondeo y use el cmdlet [Get-AzureRmAutomationJobOutput](/powershell/module/azurerm.automation/get-azurermautomationjoboutput) para recuperar la salida. A continuación se define un ejemplo de esta lógica.

```powershell
$automationAccountName = "ContosoAutomationAccount"
$runbookName = "ChildRunbookExample"
$resourceGroupName = "ContosoRG"

function IsJobTerminalState([string] $status) {
    return $status -eq "Completed" -or $status -eq "Failed" -or $status -eq "Stopped" -or $status -eq "Suspended"
}

$job = Start-AzureRmAutomationRunbook -AutomationAccountName $automationAccountName -Name $runbookName -ResourceGroupName $resourceGroupName
$pollingSeconds = 5
$maxTimeout = 10800
$waitTime = 0
while((IsJobTerminalState $job.Status) -eq $false -and $waitTime -lt $maxTimeout) {
   Start-Sleep -Seconds $pollingSeconds
   $waitTime += $pollingSeconds
   $job = $job | Get-AzureRmAutomationJob
}

$jobResults | Get-AzureRmAutomationJobOutput | Get-AzureRmAutomationJobOutputRecord | Select-Object -ExpandProperty Value
```

## <a name="scenario-runbook-fails-because-of-deserialized-object"></a><a name="fails-deserialized-object"></a>Escenario: Error en runbook debido a un objeto deserializado

### <a name="issue"></a>Problema

Se produce un error en el runbook con el error:

```error
Cannot bind parameter <ParameterName>.

Cannot convert the <ParameterType> value of type Deserialized <ParameterType> to type <ParameterType>.
```

### <a name="cause"></a>Causa

Si el runbook es un flujo de trabajo de PowerShell, almacena objetos complejos en un formato deserializado para conservar el estado del Runbook si se suspende el flujo de trabajo.

### <a name="resolution"></a>Solución

Use cualquiera de las siguientes soluciones para solucionar este problema.

* Si canaliza objetos complejos de un cmdlet a otro, encapsule dichos cmdlets en una actividad `InlineScript`.
* En lugar de pasar el objeto complejo entero, pase solamente el nombre o valor del mismo que necesite.
* Use un runbook de PowerShell en lugar de un runbook de flujo de trabajo de PowerShell.

## <a name="scenario-runbook-job-fails-because-allocated-quota-exceeded"></a><a name="quota-exceeded"></a>Escenario: Error en el trabajo del runbook porque se superó la cuota asignada

### <a name="issue"></a>Problema

Se produce un error en el trabajo de runbook con el error:

```error
The quota for the monthly total job run time has been reached for this subscription
```

### <a name="cause"></a>Causa

Este error se produce cuando la ejecución del trabajo supera la cuota gratuita de 500 minutos para su cuenta. Esta cuota se aplica a todos los tipos de tareas de ejecución de trabajos. Algunas de estas tareas son probar un trabajo, iniciar un trabajo desde el portal, ejecutar un trabajo mediante webhooks o programar un trabajo para que se ejecute mediante Azure Portal o el centro de datos. Para obtener más información sobre precios para, consulte [Precios de Automation](https://azure.microsoft.com/pricing/details/automation/).

### <a name="resolution"></a>Solución

Si quiere usar más de 500 minutos de procesamiento por mes cambie la suscripción del nivel Gratis al nivel Básico.

1. Inicie sesión en la suscripción de Azure.
2. Seleccione la cuenta de Automation que quiera actualizar.
3. Haga clic en **Configuración** y luego en **Precios**.
4. Haga clic en **Habilitar** en la parte inferior de la página para actualizar la cuenta al nivel Básico.

## <a name="scenario-cmdlet-not-recognized-when-executing-a-runbook"></a><a name="cmdlet-not-recognized"></a>Escenario: No se reconoce el cmdlet cuando se ejecuta un runbook

### <a name="issue"></a>Problema

Se produce un error en el trabajo de runbook con el error:

```error
<cmdlet name>: The term <cmdlet name> is not recognized as the name of a cmdlet, function, script file, or operable program.
```

### <a name="cause"></a>Causa

Este error se produce cuando el motor de PowerShell no puede encontrar el cmdlet que está usando en su runbook. Es posible que el módulo que contiene el cmdlet no esté presente en la cuenta, que haya un conflicto de nombres con un nombre de runbook o que el cmdlet también exista en otro módulo y Automation no pueda resolver el nombre.

### <a name="resolution"></a>Solución

Use cualquiera de las siguientes soluciones para solucionar el problema.

* Asegúrese de que ha especificado correctamente el nombre del cmdlet.
* Asegúrese de que el cmdlet exista en su cuenta de Automation y de que no haya ningún conflicto. Para comprobar si está presente el cmdlet, abra un runbook en modo de edición y busque el cmdlet que quiere encontrar en la biblioteca o ejecute `Get-Command <CommandName>`. Una vez que haya comprobado que el cmdlet está disponible para la cuenta y que no hay conflictos de nombres con otros cmdlets o runbooks, agréguelo al lienzo y asegúrese de que está usando un parámetro válido configurado en su runbook.
* Si tiene un conflicto de nombres y el cmdlet está disponible en dos módulos diferentes, use el nombre completo del cmdlet para resolver este problema. Por ejemplo, puede usar `ModuleName\CmdletName`.
* Si está ejecutando el runbook en el entorno local de un grupo de Hybrid Worker, asegúrese de que el cmdlet o el módulo están instalados en la máquina que hospeda la instancia de Hybrid Worker.

## <a name="scenario-a-long-running-runbook-fails-to-complete"></a><a name="long-running-runbook"></a>Escenario: Un runbook de larga ejecución no se puede completar

### <a name="issue"></a>Problema

El runbook muestra un estado Detenido después de ejecutarse durante tres horas. También se muestra este error:

```error
The job was evicted and subsequently reached a Stopped state. The job cannot continue running.
```

Este comportamiento es así por naturaleza en los espacios aislados de Azure debido a la supervisión de [distribución equilibrada](../automation-runbook-execution.md#fair-share) de los procesos en Azure Automation. Si un proceso se ejecuta durante más de tres horas, la distribución equilibrada detiene automáticamente el runbook. El estado de un runbook que va más allá del límite de tiempo de distribución equilibrada varía según el tipo de runbook. Runbooks de PowerShell y Python se establecen en un estado Detenido. Los runbooks del flujo de trabajo de PowerShell se establecen en Error.

### <a name="cause"></a>Causa

El runbook se ejecutó por encima del límite de tres horas permitido por la distribución equilibrada en un espacio aislado de Azure.

### <a name="resolution"></a>Solución

La solución recomendada consiste en ejecutar el runbook en un [Hybrid Runbook Worker](../automation-hrw-run-runbooks.md). Los roles Hybrid Worker no tienen el límite de tres horas de los runbooks de distribución equilibrada que tienen los espacios aislados de Azure. Los runbooks que se ejecutan en instancias de Hybrid Runbook Worker se deben desarrollar para admitir comportamientos de reinicio si existen problemas inesperados en la infraestructura local.

Otra solución consiste en optimizar el runbook mediante la creación de [runbooks secundarios](../automation-child-runbooks.md). Si el runbook recorre en bucle la misma función en varios recursos (por ejemplo, una operación de base de datos en varias bases de datos), la función se puede mover a un runbook secundario. Cada runbook secundario se ejecuta en paralelo en un proceso independiente. Este comportamiento reduce la cantidad total de tiempo que tarda en completarse el runbook primario.

Los cmdlets de PowerShell que habilitan el escenario de runbook secundario son:

* [Start-AzureRMAutomationRunbook](/powershell/module/AzureRM.Automation/Start-AzureRmAutomationRunbook). este cmdlet permite iniciar un runbook y pasar parámetros al mismo.

* [Get-AzureRmAutomationJob](/powershell/module/azurerm.automation/get-azurermautomationjob). Si hay operaciones que deben realizarse después de que se complete el runbook secundario, este cmdlet permite comprobar el estado del trabajo de cada elemento secundario.

## <a name="scenario-status-400-bad-request-when-calling-a-webhook"></a><a name="expired webhook"></a>Escenario: Estado: Solicitud incorrecta 400 al llamar a un webhook

### <a name="issue"></a>Problema

Cuando intenta invocar un webhook para un runbook de Azure Automation, recibe el siguiente error:

```error
400 Bad Request : This webhook has expired or is disabled
```

### <a name="cause"></a>Causa

El webhook al que intenta llamar está deshabilitado o ha expirado.

### <a name="resolution"></a>Solución

Si el webhook está deshabilitado, puede volver a habilitarlo a través de Azure Portal. Si el webhook expiró, debe eliminarlo y volver a crearlo. Solo puede [renovar un webhook](../automation-webhooks.md#renew-webhook) si aún no ha expirado.

## <a name="scenario-429-the-request-rate-is-currently-too-large"></a><a name="429"></a>Escenario: 429: Actualmente la tasa de solicitud es demasiado grande...

### <a name="issue"></a>Problema

Recibe el siguiente mensaje de error al ejecutar el cmdlet `Get-AzureRmAutomationJobOutput`:

```error
429: The request rate is currently too large. Please try again
```

### <a name="cause"></a>Causa

Este error puede producirse al recuperar la salida de trabajo de un runbook que tiene muchos [flujos detallados](../automation-runbook-output-and-messages.md#verbose-stream).

### <a name="resolution"></a>Solución

Realice una de las siguientes acciones para solucionar este error.

* Edite el runbook y reduzca el número de flujos de trabajo que emite.

* Reduzca el número de flujos para recuperar cuando se ejecuta el cmdlet. Para ello, puede establecer el valor del parámetro `Stream` del cmdlet `Get-AzureRmAutomationJobOutput` para que recupere solo los flujos de salida. 

## <a name="scenario-powershell-job-fails-with-error-cannot-invoke-method"></a><a name="cannot-invoke-method"></a>Escenario: El trabajo de PowerShell no se puede realizar y se muestra el error: Cannot invoke method (No se puede invocar el método)

### <a name="issue"></a>Problema

Al iniciar un trabajo de PowerShell en un runbook que se ejecuta en Azure, recibe el siguiente mensaje de error:

```error
Exception was thrown - Cannot invoke method. Method invocation is supported only on core types in this language mode.
```

### <a name="cause"></a>Causa

Este error puede indicar que los runbooks que se ejecutan en un espacio aislado de Azure no se pueden ejecutar en el [modo de lenguaje completo](/powershell/module/microsoft.powershell.core/about/about_language_modes).

### <a name="resolution"></a>Solución

Hay dos maneras de resolver este error.

* En lugar de usar `Start-Job`, use `Start-AzureRmAutomationRunbook` para iniciar el runbook.
* Intente ejecutar el runbook en una instancia de Hybrid Runbook Worker.

Para más información sobre este comportamiento y otros comportamientos de los runbooks de Azure Automation, consulte [Comportamiento del runbook](../automation-runbook-execution.md#runbook-behavior).

## <a name="scenario-linux-hybrid-runbook-worker-receives-a-prompt-for-a-password-when-signing-a-runbook"></a>Escenario: Una instancia de Hybrid Runbook Worker de Linux recibe una solicitud de contraseña al firmar un runbook

### <a name="issue"></a>Problema

La ejecución del comando `sudo` para una instancia de Hybrid Runbook Worker de Linux recupera una solicitud inesperada de contraseña.

### <a name="cause"></a>Causa

La cuenta **nxautomationuser** del agente de Log Analytics para Linux no está configurada correctamente en el archivo **sudoers**. La instancia de Hybrid Runbook Worker necesita la configuración adecuada de los permisos de la cuenta y otros datos para que pueda firmar runbooks en el trabajo de runbook de Linux.

### <a name="resolution"></a>Solución

* Asegúrese de que la instancia de Hybrid Runbook Worker tiene el archivo ejecutable de GnuPG (GPG) en la máquina.

* Compruebe la configuración de la cuenta **nxautomationuser** en el archivo **sudoers**. Consulte [Ejecución de runbooks en Hybrid Runbook Worker](../automation-hrw-run-runbooks.md)

## <a name="scenario-cmdlet-failing-in-pnp-powershell-runbook-on-azure-automation"></a>Escenario: error de cmdlet en el runbook de PowerShell de PnP en Azure Automation

### <a name="issue"></a>Problema

Cuando un runbook escribe un objeto PnP generado por PowerShell directamente en la salida de Azure Automation, la salida del cmdlet no puede volver a transmitirse a Automation.

### <a name="cause"></a>Causa

Este problema normalmente se produce cuando Azure Automation procesa runbooks que invocan cmdlets de PowerShell de PnP; por ejemplo, `add-pnplistitem`, sin detectar los objetos devueltos.

### <a name="resolution"></a>Solución

Edite los scripts para asignar los valores devueltos a variables para que los cmdlets no intenten escribir los objetos completos en la salida estándar. Un script puede redirigir el flujo de salida a un cmdlet, como se muestra a continuación.

```azurecli
  $null = add-pnplistitem
```

Si el script analiza la salida del cmdlet, el script debe almacenar la salida en una variable y manipular la variable en lugar de simplemente transmitir la salida.

```azurecli
$SomeVariable = add-pnplistitem ....
if ($SomeVariable.someproperty -eq ....
```

## <a name="my-problem-isnt-listed-above"></a><a name="other"></a>Mi problema no aparece en la lista anterior

En las secciones siguientes se muestran otros errores comunes y se proporciona documentación adicional para ayudarle a resolver el problema.

### <a name="hybrid-runbook-worker-doesnt-run-jobs-or-isnt-responding"></a>Hybrid Runbook Worker no ejecuta trabajos o no responde.

Si ejecuta los trabajos mediante Hybrid Runbook Worker en lugar de Azure Automation, necesita [solucionar los problemas en Hybrid Worker](https://docs.microsoft.com/azure/automation/troubleshoot/hybrid-runbook-worker).

### <a name="runbook-fails-with-no-permission-or-some-variation"></a>Se produce un error del tipo Sin permisos o alguna variante del mismo

Es posible que las cuentas de ejecución no tengan los mismos permisos en los recursos de Azure que su cuenta actual. Asegúrese de que su cuenta de ejecución tiene [permisos para acceder a todos los recursos](https://docs.microsoft.com/azure/role-based-access-control/role-assignments-portal) que se usan en el script.

### <a name="issues-passing-parameters-into-webhooks"></a>Problemas al pasar parámetros a webhooks

Para pasar parámetros a webhooks, consulte [Inicio de un runbook desde un webhook](../automation-webhooks.md#parameters-used-when-the-webhook-starts-a-runbook).

### <a name="issues-using-az-modules"></a>Problemas al usar módulos de Az

No se admite el uso de módulos Az y de módulos AzureRM en la misma cuenta de Automation. Consulte [Módulos de Az en runbooks](https://docs.microsoft.com/azure/automation/az-modules) para obtener más información.

### <a name="inconsistent-behavior-in-runbooks"></a>Comportamiento incoherente en los runbooks

Siga las instrucciones que se indican en [Ejecución de un runbook](https://docs.microsoft.com/azure/automation/automation-runbook-execution#runbook-behavior) para evitar problemas con trabajos simultáneos, recursos que se crean varias veces o cualquier otra lógica de temporización en los runbooks.

### <a name="runbook-fails-with-the-error-no-permission-forbidden-403-or-some-variation"></a>Se produce un error en el runbook del tipo "Sin permisos, Prohibido (403)" o alguna variación de este.

Es posible que las cuentas de ejecución no tengan los mismos permisos en los recursos de Azure que su cuenta actual. Asegúrese de que su cuenta de ejecución tiene [permisos para acceder a todos los recursos](https://docs.microsoft.com/azure/role-based-access-control/role-assignments-portal) que se usan en el script.

### <a name="runbooks-were-working-but-suddenly-stopped"></a>Los runbooks estaban funcionando, pero se han detenido.

* Asegúrese de que la cuenta de ejecución no ha expirado. Consulte [renovación de la certificación](https://docs.microsoft.com/azure/automation/manage-runas-account#cert-renewal).
* Si usa un [webhook](https://docs.microsoft.com/azure/automation/automation-webhooks#renew-webhook) para iniciar el runbooks, asegúrese de que dicho webhook no haya expirado.

### <a name="passing-parameters-into-webhooks"></a>Paso de parámetros a webhooks

Para pasar parámetros a webhooks, consulte el artículo [Inicio de un runbook desde un webhook](https://docs.microsoft.com/azure/automation/automation-webhooks#parameters-used-when-the-webhook-starts-a-runbook).

### <a name="using-az-modules"></a>Uso de módulos de Az

No se admite el uso de módulos Az y de módulos AzureRM en la misma cuenta de Automation. Consulte el artículo [Módulos de Az en runbooks](https://docs.microsoft.com/azure/automation/az-modules).

### <a name="using-self-signed-certificates"></a>Uso de certificados autofirmados

Para usar certificados autofirmados, consulte la sección [Creación de un certificado nuevo](https://docs.microsoft.com/azure/automation/shared-resources/certificates#creating-a-new-certificate).

### <a name="access-denied-when-using-azure-sandbox-for-a-runbook"></a>Acceso denegado al usar el espacio aislado de Azure para un runbook

El espacio aislado de Azure impide el acceso a todos los servidores COM fuera de proceso. Por ejemplo, una aplicación en espacio aislado o un runbook no pueden realizar llamadas a Instrumental de administración de Windows (WMI) o al servicio Windows Installer (msiserver.exe). Para obtener más información sobre el uso del espacio aislado, consulte [Ejecución de un runbook en Azure Automation](https://docs.microsoft.com/azure/automation/automation-runbook-execution).

## <a name="recommended-documents"></a>Documentos recomendados

* [Inicio de un runbook en Azure Automation](https://docs.microsoft.com/azure/automation/automation-starting-a-runbook)
* [Ejecución de un runbook en Azure Automation](https://docs.microsoft.com/azure/automation/automation-runbook-execution)

## <a name="next-steps"></a>Pasos siguientes

Si su problema no aparece o es incapaz de resolverlo, visite uno de nuestros canales para obtener ayuda adicional:

* Obtenga respuestas de expertos de Azure en los [foros de Azure](https://azure.microsoft.com/support/forums/).
* Póngase en contacto con [@AzureSupport](https://twitter.com/azuresupport): la cuenta de Microsoft Azure oficial para mejorar la experiencia del cliente, que pone en contacto a la comunidad de Azure con los recursos adecuados: respuestas, soporte técnico y expertos.
* Si necesita más ayuda, puede registrar un incidente de soporte técnico de Azure. Vaya al [sitio de soporte técnico de Azure](https://azure.microsoft.com/support/options/) y seleccione **Obtener soporte**.
