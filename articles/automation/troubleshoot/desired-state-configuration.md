---
title: Solución de problemas de Desired State Configuration (DSC) de Azure Automation
description: En este artículo se ofrece información sobre la solución de problemas con Desired State Configuration (DSC).
services: automation
ms.service: automation
ms.subservice: ''
author: mgoedtel
ms.author: magoedte
ms.date: 04/16/2019
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: 9f33dc9528d5f7043dda2c6fad207a9a51347a2b
ms.sourcegitcommit: d597800237783fc384875123ba47aab5671ceb88
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/03/2020
ms.locfileid: "80631493"
---
# <a name="troubleshoot-issues-with-azure-automation-desired-state-configuration-dsc"></a>Solución de problemas de Desired State Configuration (DSC) de Azure Automation

En este artículo se ofrece información sobre la solución de problemas con Desired State Configuration (DSC).

## <a name="diagnosing-an-issue"></a>Diagnosticar un problema

Si se producen errores al compilar o implementar configuraciones en Azure State Configuration, aquí tiene algunos pasos que le ayudarán a diagnosticar el problema.

### <a name="1-ensure-that-your-configuration-compiles-successfully-on-the-local-machine"></a>1. Asegúrese de que la configuración se compila correctamente en la máquina local

Azure State Configuration se basa en DSC de PowerShell. Puede encontrar la documentación del lenguaje y la sintaxis de DSC en los [documentos de DSC de PowerShell](https://docs.microsoft.com/powershell/scripting/overview).

Al compilar la configuración de DSC en la máquina local, puede detectar y resolver errores comunes, por ejemplo:

   - Módulos que faltan
   - Errores de sintaxis
   - Errores lógicos

### <a name="2-view-dsc-logs-on-your-node"></a>2. Visualización de los registros de DSC en el nodo

Si la configuración se compila correctamente, pero se produce un error cuando se aplica a un nodo, puede encontrar información detallada en los registros de DSC. Para obtener información sobre dónde encontrar estos registros, consulte [¿Dónde se encuentran los registros de eventos de DSC?](/powershell/scripting/dsc/troubleshooting/troubleshooting#where-are-dsc-event-logs)

El módulo [xDscDiagnostics](https://github.com/PowerShell/xDscDiagnostics) puede ayudarle a analizar la información detallada de los registros de DSC. Si se pone en contacto con el equipo de soporte técnico, necesitarán estos registros para diagnosticar el problema.

Puede instalar el módulo xDscDiagnostics en la máquina local con las instrucciones que se encuentran en [Instalación de la versión estable del módulo](https://github.com/PowerShell/xDscDiagnostics#install-the-stable-version-module).

Para instalar el módulo xDscDiagnostics en la máquina de Azure, use [Invoke-AzVMRunCommand](/powershell/module/azurerm.compute/invoke-azurermvmruncommand). También puede usar la opción **Ejecutar comando** desde el portal, siguiendo los pasos que se describen en [Ejecución de scripts de PowerShell en la máquina virtual Windows con el comando Ejecutar](../../virtual-machines/windows/run-command.md).

Para obtener información sobre el uso de xDscDiagnostics, consulte [Uso de xDscDiagnostics para analizar los registros de DSC](/powershell/scripting/dsc/troubleshooting/troubleshooting#using-xdscdiagnostics-to-analyze-dsc-logs). Consulte también [Cmdlets de xDscDiagnostics](https://github.com/PowerShell/xDscDiagnostics#cmdlets).

### <a name="3-ensure-that-nodes-and-the-automation-workspace-have-required-modules"></a>3. Asegúrese de que los nodos y el área de trabajo de Automation tengan los módulos necesarios

DSC depende de módulos que hay instalados en el nodo. Al usar State Configuration de Azure Automation, importe los módulos necesarios a la cuenta de Automation mediante los pasos indicados en [Importación de módulos](../shared-resources/modules.md#importing-modules). Las configuraciones también pueden tener una dependencia en versiones específicas de los módulos. Para más información, consulte [Solución de problemas de módulos](shared-resources.md#modules).

## <a name="common-errors-when-working-with-dsc"></a>Errores comunes al trabajar con DSC

### <a name="scenario-a-configuration-with-special-characters-cannot-be-deleted-from-the-portal"></a><a name="unsupported-characters"></a>Escenario: No se puede eliminar una configuración con caracteres especiales del portal

#### <a name="issue"></a>Problema

Al intentar eliminar una configuración de DSC del portal, ve el siguiente error:

```error
An error occurred while deleting the DSC configuration '<name>'.  Error-details: The argument configurationName with the value <name> is not valid.  Valid configuration names can contain only letters,  numbers, and underscores.  The name must start with a letter.  The length of the name must be between 1 and 64 characters.
```

#### <a name="cause"></a>Causa

Este error es un problema temporal que está previsto que se resuelva.

#### <a name="resolution"></a>Solución

* Use el cmdlet de Az "Remove-AzAutomationDscConfiguration" para eliminar la configuración.
* Todavía no se ha actualizado la documentación de este cmdlet.  Hasta entonces, consulte la documentación del módulo AzureRM.
  * [Remove-AzureRmAutomationDSCConfiguration](/powershell/module/azurerm.automation/Remove-AzureRmAutomationDscConfiguration)

### <a name="scenario-failed-to-register-dsc-agent"></a><a name="failed-to-register-agent"></a>Escenario: No se pudo registrar el agente de DSC

#### <a name="issue"></a>Problema

Al intentar ejecutar `Set-DscLocalConfigurationManager` u otro cmdlet de DSC, recibe el error:

```error
Registration of the Dsc Agent with the server
https://<location>-agentservice-prod-1.azure-automation.net/accounts/00000000-0000-0000-0000-000000000000 failed. The
underlying error is: Failed to register Dsc Agent with AgentId 00000000-0000-0000-0000-000000000000 with the server htt
ps://<location>-agentservice-prod-1.azure-automation.net/accounts/00000000-0000-0000-0000-000000000000/Nodes(AgentId='00000000-0000-0000-0000-000000000000'). .
    + CategoryInfo          : InvalidResult: (root/Microsoft/...gurationManager:String) [], CimException
    + FullyQualifiedErrorId : RegisterDscAgentCommandFailed,Microsoft.PowerShell.DesiredStateConfiguration.Commands.Re
   gisterDscAgentCommand
    + PSComputerName        : <computerName>
```

#### <a name="cause"></a>Causa

Este error se debe normalmente a un firewall, a la máquina que está detrás de un servidor proxy o a otros errores de red.

#### <a name="resolution"></a>Solución

Compruebe que la máquina tenga acceso a los puntos de conexión adecuadas para DSC de Automatización de Azure y vuelva a intentarlo. Para ver una lista de puertos y direcciones necesarios, consulte [Planeamiento de red](../automation-dsc-overview.md#network-planning).

### <a name="a-nameunauthorizedscenario-status-reports-return-response-code-unauthorized"></a><a name="unauthorized"><a/>Escenario: los informes de estado devuelven el código de respuesta "No autorizado"

#### <a name="issue"></a>Problema

Al registrar un nodo con State Configuration (DSC), recibirá uno de los mensajes de error siguientes:

```error
The attempt to send status report to the server https://{your Automation account URL}/accounts/xxxxxxxxxxxxxxxxxxxxxx/Nodes(AgentId='xxxxxxxxxxxxxxxxxxxxxxxxx')/SendReport returned unexpected response code Unauthorized.
```

```error
VM has reported a failure when processing extension 'Microsoft.Powershell.DSC / Registration of the Dsc Agent with the server failed.
```

### <a name="cause"></a>Causa

Este problema se debe a un certificado que no es válido o ha expirado.  Para más información, consulte [Expiración y repetición del registro del certificado](../automation-dsc-onboarding.md#re-registering-a-node).

### <a name="resolution"></a>Solución

Siga los pasos que se indican a continuación para volver a registrar el nodo DSC con errores.

En primer lugar, elimine el registro del nodo mediante los pasos siguientes.

1. En Azure Portal, en **Inicio** -> **Cuentas de Automation** -> {su cuenta de Automation} -> **State Configuration (DSC)**
2. Haga clic en "Nodos" y, luego, en el nodo con problemas.
3. Haga clic en "Anular registro" para anular el registro del nodo.

En segundo lugar, desinstale la extensión DSC del nodo.

1. En Azure Portal, en **Inicio** -> **Máquina virtual** -> {nodo con errores} -> **Extensiones**
2. Haga clic en "Microsoft.Powershell.DSC".
3. Haga clic en "Desinstalar" para desinstalar la extensión DSC de PowerShell.

En tercer lugar, quite todos los certificados incorrectos o expirados del nodo.

En un símbolo del sistema de PowerShell con privilegios elevados en el nodo con errores, ejecute lo siguiente:

```powershell
$certs = @()
$certs += dir cert:\localmachine\my | ?{$_.FriendlyName -like "DSC"}
$certs += dir cert:\localmachine\my | ?{$_.FriendlyName -like "DSC-OaaS Client Authentication"}
$certs += dir cert:\localmachine\CA | ?{$_.subject -like "CN=AzureDSCExtension*"}
"";"== DSC Certificates found: " + $certs.Count
$certs | FL ThumbPrint,FriendlyName,Subject
If (($certs.Count) -gt 0)
{ 
    ForEach ($Cert in $certs) 
    {
        RD -LiteralPath ($Cert.Pspath) 
    }
}
```

Por último, vuelva a registrar el nodo con errores mediante estos pasos.

1. En Azure Portal, en **Inicio** -> **Cuentas de Automation** -> {su cuenta de Automation} -> **State Configuration (DSC)**
2. Haga clic en "Nodos".
3. Haga clic en el botón "Agregar".
4. Seleccione el nodo con errores.
5. Haga clic en "Conectar" y seleccione las opciones deseadas.

### <a name="scenario-node-is-in-failed-status-with-a-not-found-error"></a><a name="failed-not-found"></a>Escenario: el nodo se encuentra en estado de error con el error "No encontrado"

#### <a name="issue"></a>Problema

El nodo tiene un informe con estado **erróneo** y contiene el error:

```error
The attempt to get the action from server https://<url>//accounts/<account-id>/Nodes(AgentId=<agent-id>)/GetDscAction failed because a valid configuration <guid> cannot be found.
```

#### <a name="cause"></a>Causa

Este error suele ocurrir cuando se asigna al nodo un nombre de configuración (por ejemplo, ABC), en lugar de un nombre de configuración de nodo (por ejemplo, ABC.WebServer).

#### <a name="resolution"></a>Solución

* Asegúrese de estar asignando al nodo un "nombre de configuración de nodo" y no el "nombre de configuración".
* Puede asignar una configuración de nodo a un nodo mediante el Portal de Azure o con un cmdlet de PowerShell.

  * Para asignar una configuración de nodo a un nodo mediante Azure Portal, abra la página **Nodos DSC**, seleccione un nodo y haga clic en el botón **Asignar configuración de nodo**.
  * Para asignar una configuración de nodo a un nodo mediante un cmdlet de PowerShell, use el cmdlet **Set-AzureRmAutomationDscNode**.

### <a name="scenario-no-node-configurations-mof-files-were-produced-when-a-configuration-is-compiled"></a><a name="no-mof-files"></a>Escenario: no se produjeron configuraciones de nodo (archivos MOF) al compilarse una configuración

#### <a name="issue"></a>Problema

Su trabajo de compilación de DSC suspende con el error:

```error
Compilation completed successfully, but no node configuration.mofs were generated.
```

#### <a name="cause"></a>Causa

Cuando la expresión que aparece junto a la palabra clave **Node** en la configuración de DSC se evalúa como `$null`, no se produce ninguna configuración de nodo.

#### <a name="resolution"></a>Solución

Cualquiera de las siguientes soluciones resolverá el problema:

* Asegúrese de que la expresión junto a la palabra clave **Node** en la definición de configuración no se está evaluando como $null.
* Si se pasan datos de configuración al compilar la configuración, asegúrese de que se pasan los valores esperados que la configuración necesita de [ConfigurationData](../automation-dsc-compile.md).

### <a name="scenario-the-dsc-node-report-becomes-stuck-in-progress-state"></a><a name="dsc-in-progress"></a>Escenario: el informe de nodo de DSC se queda bloqueado en el estado "en curso"

#### <a name="issue"></a>Problema

El agente DSC genera la salida:

```error
No instance found with given property values
```

#### <a name="cause"></a>Causa

Ha actualizado la versión de WMF y ha dañado WMI.

#### <a name="resolution"></a>Solución

Para solucionar el problema, siga las instrucciones que se indican en el artículo [Problemas y limitaciones conocidos de DSC](https://docs.microsoft.com/powershell/scripting/wmf/known-issues/known-issues-dsc).

### <a name="scenario-unable-to-use-a-credential-in-a-dsc-configuration"></a><a name="issue-using-credential"></a>Escenario: no se puede usar una credencial en una configuración de DSC

#### <a name="issue"></a>Problema

El trabajo de compilación de DSC se ha suspendido con el error:

```error
System.InvalidOperationException error processing property 'Credential' of type <some resource name>: Converting and storing an encrypted password as plaintext is allowed only if PSDscAllowPlainTextPassword is set to true.
```

#### <a name="cause"></a>Causa

Ha usado una credencial en una configuración, pero no ha proporcionado el valor adecuado de **ConfigurationData** para establecer **PSDscAllowPlainTextPassword** en true en cada configuración de nodo.

#### <a name="resolution"></a>Solución

* Asegúrese de pasar el valor adecuado de **ConfigurationData** para establecer **PSDAllowPlainTextPassword** como true para cada configuración de nodo mencionada en la configuración. Para más información, consulte [Compilación de configuraciones de DSC en State Configuration de Azure Automation](../automation-dsc-compile.md).

### <a name="scenario-onboarding-from-dsc-extension-failure-processing-extension-error"></a><a name="failure-processing-extension"></a>Escenario: Se produce un error de procesamiento de la extensión al realizar la incorporación desde la extensión DSC

#### <a name="issue"></a>Problema

Al realizar la incorporación mediante la extensión DSC, se produce el error:

```error
VM has reported a failure when processing extension 'Microsoft.Powershell.DSC'. Error message: \"DSC COnfiguration 'RegistrationMetaConfigV2' completed with error(s). Following are the first few: Registration of the Dsc Agent with the server <url> failed. The underlying error is: The attempt to register Dsc Agent with Agent Id <ID> with the server <url> return unexpected response code BadRequest. .\".
```

#### <a name="cause"></a>Causa

Este error suele producirse cuando se asigna al nodo un nombre de configuración que no existe en el servicio.

#### <a name="resolution"></a>Solución

* Asegúrese de que asigna al nodo un nombre de configuración que coincida exactamente con el nombre del servicio.
* Puede elegir no incluir el nombre de configuración del nodo, lo que dará lugar a la incorporación del nodo, pero no a la asignación de una configuración del nodo

### <a name="scenario-registering-a-node-with-powershell-returns-the-error-one-or-more-errors-occurred"></a><a name="cross-subscription"></a>Escenario: al registrar un nodo con PowerShell, se devuelve el error "Se han producido uno o más errores"

#### <a name="issue"></a>Problema

Al registrar un nodo mediante `Register-AzAutomationDSCNode` o `Register-AzureRMAutomationDSCNode`, recibe este error.

```error
One or more errors occurred.
```

#### <a name="cause"></a>Causa

Este error se produce cuando se intenta registrar un nodo que reside en una suscripción independiente de la cuenta de Automation.

#### <a name="resolution"></a>Solución

Trate el nodo entre suscripciones como si se encontrara en una nube independiente o local.

Siga estos pasos para registrar el nodo.

* Windows: [máquinas físicas y virtuales con Windows locales o en una nube que no sea Azure/AWS](../automation-dsc-onboarding.md#onboarding-physicalvirtual-windows-machines-on-premises-or-in-a-cloud-other-than-azure-including-aws-ec2-instances).
* Linux: [máquinas físicas y virtuales Linux locales o en una nube que no sea Azure](../automation-dsc-onboarding.md#onboarding-physicalvirtual-linux-machines-on-premises-or-in-a-cloud-other-than-azure).

### <a name="scenario-error-message---provisioning-failed"></a><a name="agent-has-a-problem"></a>Escenario: mensaje de error: "Error de aprovisionamiento"

#### <a name="issue"></a>Problema

Al registrar un nodo, se muestra el error:

```error
Provisioning has failed
```

#### <a name="cause"></a>Causa

Este mensaje se produce cuando hay un problema de conectividad entre el nodo y Azure.

#### <a name="resolution"></a>Solución

Determine si el nodo está en una red virtual privada o si tiene otros problemas para conectarse a Azure.

Para más información, consulte [Solución de problemas de errores al incorporar soluciones](onboarding.md).

### <a name="scenario-applying-a-configuration-in-linux-a-failure-occurs-with-a-general-error"></a><a name="failure-linux-temp-noexec"></a>Escenario: Al aplicar una configuración en Linux, se produce un error general

#### <a name="issue"></a>Problema

Al aplicar una configuración en Linux, se produce el error:

```error
This event indicates that failure happens when LCM is processing the configuration. ErrorId is 1. ErrorDetail is The SendConfigurationApply function did not succeed.. ResourceId is [resource]name and SourceInfo is ::nnn::n::resource. ErrorMessage is A general error occurred, not covered by a more specific error code..
```

#### <a name="cause"></a>Causa

Los clientes han identificado que si se establece la ubicación `/tmp` en `noexec`, la versión actual de DSC no podrá aplicar las configuraciones.

#### <a name="resolution"></a>Solución

* Quite la opción `noexec` de la ubicación `/tmp`.

### <a name="scenario-node-configuration-names-that-overlap-could-result-in-bad-release"></a><a name="compilation-node-name-overlap"></a>Escenario: los nombres de configuración de nodos que se superponen podrían provocar una versión incorrecta

#### <a name="issue"></a>Problema

Si se usa un único script de configuración para generar varias configuraciones de nodo y algunas de ellas tienen un nombre que es un subconjunto de otros, un problema en el servicio de compilación podría dar lugar a la asignación de la configuración incorrecta.  Este problema solo se produce cuando se usa un único script para generar configuraciones con datos de configuración por nodo y solo cuando la superposición de los nombres tiene lugar al principio de la cadena.

Por ejemplo, si se usa un único script de configuración para generar configuraciones basadas en los datos de nodo pasados como una tabla hash mediante cmdlets, y los datos del nodo incluyen un servidor llamado "server" y "1server".

#### <a name="cause"></a>Causa

Problema conocido con el servicio de compilación.

#### <a name="resolution"></a>Solución

La mejor solución sería realizar la compilación localmente o en una canalización de CI/CD y cargar los archivos MOF directamente en el servicio.  Si es necesario que la compilación tenga lugar en el servicio, la siguiente mejor solución sería dividir los trabajos de compilación para que no haya nombres superpuestos.

### <a name="scenario-gateway-timeout-error-on-dsc-configuration-upload"></a><a name="gateway-timeout"></a>Escenario: Error de tiempo de espera de puerta de enlace en la carga de la configuración de DSC

#### <a name="issue"></a>Problema

Recibe un error `GatewayTimeout` al cargar una configuración de DSC. 

#### <a name="cause"></a>Causa

Las configuraciones de DSC que tardan mucho tiempo en compilarse pueden producir este error.

#### <a name="resolution"></a>Solución

Las configuraciones de DSC se pueden analizar más rápidamente si se incluye expresamente el parámetro `ModuleName` en cualquier llamada `Import-DscResource`. Para más información, vea [Uso de Import-DSCResource](https://docs.microsoft.com/powershell/scripting/dsc/configurations/import-dscresource?view=powershell-5.1).

## <a name="next-steps"></a>Pasos siguientes

Si su problema no aparece o es incapaz de resolverlo, visite uno de nuestros canales para obtener ayuda adicional:

* Obtenga respuestas de expertos de Azure en los [foros de Azure](https://azure.microsoft.com/support/forums/).
* Póngase en contacto con [@AzureSupport](https://twitter.com/azuresupport): la cuenta de Microsoft Azure oficial para mejorar la experiencia del cliente, que pone en contacto a la comunidad de Azure con los recursos adecuados: respuestas, soporte técnico y expertos.
* Si necesita más ayuda, puede registrar un incidente de soporte técnico de Azure. Vaya al [sitio de soporte técnico de Azure](https://azure.microsoft.com/support/options/) y seleccione **Obtener soporte**.
