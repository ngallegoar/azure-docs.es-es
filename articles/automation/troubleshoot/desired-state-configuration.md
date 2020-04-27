---
title: Solución de problemas de State Configuration (DSC) de Azure Automation
description: En este artículo se ofrece información sobre la solución de problemas de State Configuration (DSC) de Azure Automation.
services: automation
ms.service: automation
ms.subservice: ''
author: mgoedtel
ms.author: magoedte
ms.date: 04/16/2019
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: 4c045e110e21ed201278dcd84f38cb4a376ae8db
ms.sourcegitcommit: acb82fc770128234f2e9222939826e3ade3a2a28
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/21/2020
ms.locfileid: "81679319"
---
# <a name="troubleshoot-issues-with-azure-automation-state-configuration-dsc"></a>Solución de problemas con State Configuration (DSC) de Azure Automation

En este artículo se proporciona información sobre la solución de problemas que surgen al compilar o implementar configuraciones en State Configuration (DSC) de Azure Automation.

>[!NOTE]
>Este artículo se ha actualizado para usar el nuevo módulo Az de Azure PowerShell. Aún puede usar el módulo de AzureRM que continuará recibiendo correcciones de errores hasta diciembre de 2020 como mínimo. Para más información acerca del nuevo módulo Az y la compatibilidad con AzureRM, consulte [Introducing the new Azure PowerShell Az module](https://docs.microsoft.com/powershell/azure/new-azureps-module-az?view=azps-3.5.0) (Presentación del nuevo módulo Az de Azure PowerShell). Para obtener instrucciones sobre la instalación del módulo Az en Hybrid Runbook Worker, consulte [Instalación del módulo de Azure PowerShell](https://docs.microsoft.com/powershell/azure/install-az-ps?view=azps-3.5.0). Puede actualizar los módulos de su cuenta de Automation a la versión más reciente mediante [Actualización de módulos de Azure PowerShell en Azure Automation](../automation-update-azure-modules.md).

## <a name="diagnosing-an-issue"></a>Diagnosticar un problema

Si recibe un error de compilación o implementación de la configuración, los pasos siguientes le ayudarán a diagnosticar el problema.

### <a name="1-ensure-that-your-configuration-compiles-successfully-on-the-local-machine"></a>1. Asegúrese de que la configuración se compila correctamente en la máquina local

State Configuration (DSC) de Azure Automation está integrado en Desired State Configuration (DSC) de PowerShell. Puede encontrar la documentación del lenguaje y la sintaxis de DSC en los [documentos de DSC de PowerShell](https://docs.microsoft.com/powershell/scripting/overview).

Al compilar una configuración de DSC en la máquina local, puede detectar y resolver errores comunes, como los siguientes:

   - Módulos que faltan
   - Errores de sintaxis
   - Errores lógicos

### <a name="2-view-dsc-logs-on-your-node"></a>2. Visualización de los registros de DSC en el nodo

Si la configuración se compila correctamente, pero se produce un error cuando se aplica a un nodo, puede encontrar información detallada en los registros de DSC. Para obtener información sobre dónde encontrar estos registros, consulte [¿Dónde se encuentran los registros de eventos de DSC?](/powershell/scripting/dsc/troubleshooting/troubleshooting#where-are-dsc-event-logs)

El módulo [xDscDiagnostics](https://github.com/PowerShell/xDscDiagnostics) puede ayudarle a analizar la información detallada de los registros de DSC. Si se pone en contacto con el equipo de soporte técnico, necesitarán estos registros para diagnosticar el problema.

Puede instalar el módulo `xDscDiagnostics` en la máquina local con las instrucciones que se indican en [Instalación de la versión estable del módulo](https://github.com/PowerShell/xDscDiagnostics#install-the-stable-version-module).

Para instalar el módulo `xDscDiagnostics` en la máquina de Azure, use [Invoke-AzVMRunCommand](https://docs.microsoft.com/powershell/module/az.compute/invoke-azvmruncommand?view=azps-3.7.0). También puede usar la opción **Ejecutar comando** de Azure Portal siguiendo los pasos que se describen en [Ejecución de scripts de PowerShell en la máquina virtual Windows con el comando Ejecutar](../../virtual-machines/windows/run-command.md).

Para obtener información sobre el uso de **xDscDiagnostics**, consulte [Uso de xDscDiagnostics para analizar los registros de DSC](/powershell/scripting/dsc/troubleshooting/troubleshooting#using-xdscdiagnostics-to-analyze-dsc-logs). Consulte también [Cmdlets de xDscDiagnostics](https://github.com/PowerShell/xDscDiagnostics#cmdlets).

### <a name="3-ensure-that-nodes-and-the-automation-workspace-have-required-modules"></a>3. Asegúrese de que los nodos y el área de trabajo de Automation tengan los módulos necesarios

DSC depende de módulos que hay instalados en el nodo. Al usar State Configuration de Azure Automation, importe los módulos necesarios en la cuenta de Automation mediante los pasos que se indican en [Importación de módulos](../shared-resources/modules.md#importing-modules). Las configuraciones también pueden tener una dependencia en versiones específicas de los módulos. Para más información, consulte [Solución de problemas de módulos](shared-resources.md#modules).

## <a name="scenario-a-configuration-with-special-characters-cannot-be-deleted-from-the-portal"></a><a name="unsupported-characters"></a>Escenario: No se puede eliminar una configuración con caracteres especiales del portal

### <a name="issue"></a>Problema

Al intentar eliminar una configuración de DSC del portal, ve el siguiente error:

```error
An error occurred while deleting the DSC configuration '<name>'.  Error-details: The argument configurationName with the value <name> is not valid.  Valid configuration names can contain only letters,  numbers, and underscores.  The name must start with a letter.  The length of the name must be between 1 and 64 characters.
```

### <a name="cause"></a>Causa

Este error es un problema temporal que está previsto que se resuelva.

### <a name="resolution"></a>Solución

Use el cmdlet [Remove-AzAutomationDscConfiguration] (https://docs.microsoft.com/powershell/module/Az.Automation/Remove-AzAutomationDscConfiguration?view=azps-3.7.0 ) para eliminar la configuración.

### <a name="scenario-failed-to-register-dsc-agent"></a><a name="failed-to-register-agent"></a>Escenario: No se pudo registrar el agente de DSC

### <a name="issue"></a>Problema

Si usa [Set-DscLocalConfigurationManager](https://docs.microsoft.com/powershell/module/psdesiredstateconfiguration/set-dsclocalconfigurationmanager?view=powershell-5.1) u otro cmdlet de DSC, recibirá el siguiente error:

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

### <a name="cause"></a>Causa

Este error se debe normalmente a un firewall, a la máquina que está detrás de un servidor proxy o a otros errores de red.

### <a name="resolution"></a>Solución

Compruebe que la máquina tenga acceso a los puntos de conexión adecuados para DSC y vuelva a intentarlo. Para ver una lista de puertos y direcciones necesarios, consulte [Planeamiento de red](../automation-dsc-overview.md#network-planning).

## <a name="a-nameunauthorizedscenario-status-reports-return-response-code-unauthorized"></a><a name="unauthorized"><a/>Escenario: los informes de estado devuelven el código de respuesta No autorizado

### <a name="issue"></a>Problema

Al registrar un nodo con State Configuration de Azure Automation, recibirá uno de los mensajes de error siguientes:

```error
The attempt to send status report to the server https://{your Automation account URL}/accounts/xxxxxxxxxxxxxxxxxxxxxx/Nodes(AgentId='xxxxxxxxxxxxxxxxxxxxxxxxx')/SendReport returned unexpected response code Unauthorized.
```

```error
VM has reported a failure when processing extension 'Microsoft.Powershell.DSC / Registration of the Dsc Agent with the server failed.
```

### <a name="cause"></a>Causa

Este problema se debe a un certificado que no es válido o ha expirado. Consulte [Expiración y repetición del registro del certificado](../automation-dsc-onboarding.md#re-registering-a-node).

Este problema también puede deberse a una configuración de proxy que no permite el acceso a * **.azure-automation.net**. Para obtener más información, vea [Configuración de redes privadas](../automation-dsc-overview.md#network-planning). 

### <a name="resolution"></a>Solución

Use los pasos siguientes para volver a registrar el nodo DSC con errores.

Paso 1: anulación del registro del nodo.

1. En Azure Portal, vaya a **Inicio** -> **Cuentas de Automation** -> (su cuenta de Automation) -> **State Configuration (DSC)** .
2. Seleccione **Nodos** y, luego, haga clic en el nodo con problemas.
3. Haga clic en **Anular registro** para anular el registro del nodo.

Paso 2: desinstalación de la extensión DSC del nodo.

1. En Azure Portal, vaya a **Inicio** -> **Máquina virtual** -> (nodo con errores) -> **Extensiones**.
2. Seleccione **Microsoft.PowerShell.DSC**, la extensión DSC de PowerShell.
3. Haga clic en **Desinstalar** para desinstalar la extensión.

Paso 3: eliminación de todos los certificados incorrectos o expirados del nodo.

En un nodo con errores de un símbolo del sistema de PowerShell con privilegios elevados, ejecute estos comandos:

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

Paso 4: repetición del registro del nodo con errores.

1. En Azure Portal, vaya a **Inicio** -> **Cuentas de Automation** -> (su cuenta de Automation) -> **State Configuration (DSC)** .
2. Seleccione **Nodos**.
3. Haga clic en **Agregar**.
4. Seleccione el nodo con errores.
5. Haga clic en **Conectar** y seleccione las opciones deseadas.

## <a name="scenario-node-is-in-failed-status-with-a-not-found-error"></a><a name="failed-not-found"></a>Escenario: el nodo se encuentra en estado de error con el error "No encontrado"

### <a name="issue"></a>Problema

El nodo tiene un informe con el estado Erróneo y contiene el error:

```error
The attempt to get the action from server https://<url>//accounts/<account-id>/Nodes(AgentId=<agent-id>)/GetDscAction failed because a valid configuration <guid> cannot be found.
```

### <a name="cause"></a>Causa

Este error suele ocurrir cuando se asigna al nodo un nombre de configuración, como **ABC**, en lugar de un nombre de configuración de nodo (archivo MOF), como **ABC.WebServer**.

### <a name="resolution"></a>Solución

* Asegúrese de estar asignando al nodo un nombre de configuración de nodo y no el nombre de configuración.
* Puede asignar una configuración de nodo a un nodo mediante el Portal de Azure o con un cmdlet de PowerShell.

  * En Azure Portal, vaya a **Inicio** -> **Cuentas de Automation**-> (su cuenta de Automation)-> **State Configuration (DSC)** y, a continuación, seleccione un nodo y haga clic en **Asignar configuración de nodo**.
  * Use el cmdlet [Set-AzAutomationDscNode](https://docs.microsoft.com/powershell/module/Az.Automation/Set-AzAutomationDscNode?view=azps-3.7.0).

## <a name="scenario-no-node-configurations-mof-files-were-produced-when-a-configuration-was-compiled"></a><a name="no-mof-files"></a>Escenario: no se produjeron configuraciones de nodo (archivos MOF) al compilar una configuración

### <a name="issue"></a>Problema

Su trabajo de compilación de DSC suspende con el error:

```error
Compilation completed successfully, but no node configuration **.mof** files were generated.
```

### <a name="cause"></a>Causa

Cuando la expresión que aparece junto a la palabra clave `Node` en la configuración de DSC se evalúa en `$null`, no se produce ninguna configuración de nodo.

### <a name="resolution"></a>Solución

Use cualquiera de las siguientes soluciones para corregir el problema.

* Asegúrese de que la expresión junto a la palabra clave `Node` en la definición de configuración no se está evaluando en Null.
* Si se pasa [ConfigurationData](../automation-dsc-compile.md) al compilar la configuración, asegúrese de que se pasan los valores esperados que la configuración necesita de los datos de configuración.

### <a name="scenario-the-dsc-node-report-becomes-stuck-in-the-in-progress-state"></a><a name="dsc-in-progress"></a>Escenario: el informe de nodo de DSC se queda bloqueado en el estado En curso

### <a name="issue"></a>Problema

El agente DSC genera la salida:

```error
No instance found with given property values
```

### <a name="cause"></a>Causa

Ha actualizado la versión de Windows Management Framework (WMF) y ha dañado la de Instrumental de administración de Windows (WMI).

### <a name="resolution"></a>Solución

Siga las instrucciones de [Problemas y limitaciones conocidos en DSC](https://docs.microsoft.com/powershell/scripting/wmf/known-issues/known-issues-dsc).

## <a name="scenario-unable-to-use-a-credential-in-a-dsc-configuration"></a><a name="issue-using-credential"></a>Escenario: no se puede usar una credencial en una configuración de DSC

### <a name="issue"></a>Problema

El trabajo de compilación de DSC se ha suspendido con el error:

```error
System.InvalidOperationException error processing property 'Credential' of type <some resource name>: Converting and storing an encrypted password as plaintext is allowed only if PSDscAllowPlainTextPassword is set to true.
```

### <a name="cause"></a>Causa

Ha usado una credencial en una configuración, pero no ha proporcionado el valor adecuado de `ConfigurationData` para establecer `PSDscAllowPlainTextPassword` en true para cada configuración de nodo.

### <a name="resolution"></a>Solución

Asegúrese de pasar el valor adecuado de `ConfigurationData` para establecer `PSDscAllowPlainTextPassword` en true para cada configuración de nodo mencionada en la configuración. Consulte [Compilación de configuraciones de DSC en Azure Automation State Configuration](../automation-dsc-compile.md).

## <a name="scenario-failure-processing-extension-error-when-onboarding-from-dsc-extension"></a><a name="failure-processing-extension"></a>Escenario: error de procesamiento de la extensión al realizar la incorporación desde la extensión DSC

### <a name="issue"></a>Problema

Al realizar la incorporación mediante una extensión DSC, se produce el error:

```error
VM has reported a failure when processing extension 'Microsoft.Powershell.DSC'. Error message: \"DSC COnfiguration 'RegistrationMetaConfigV2' completed with error(s). Following are the first few: Registration of the Dsc Agent with the server <url> failed. The underlying error is: The attempt to register Dsc Agent with Agent Id <ID> with the server <url> return unexpected response code BadRequest. .\".
```

### <a name="cause"></a>Causa

Este error suele producirse cuando se asigna al nodo un nombre de configuración que no existe en el servicio.

### <a name="resolution"></a>Solución

* Asegúrese de que asigna al nodo un nombre que coincida exactamente con el nombre del servicio.
* Puede elegir no incluir el nombre de configuración del nodo, lo que da lugar a la incorporación del nodo, pero no a la asignación de una configuración del nodo.

## <a name="scenario-one-or-more-errors-occurred-error-when-registering-a-node-using-powershell"></a><a name="cross-subscription"></a>Escenario: error "Se han producido uno o más errores" al registrar un nodo con PowerShell

### <a name="issue"></a>Problema

Al registrar un nodo mediante [Register-AzAutomationDSCNode](https://docs.microsoft.com/powershell/module/az.automation/register-azautomationdscnode?view=azps-3.7.0) o [Register-AzureRMAutomationDSCNode](https://docs.microsoft.com/powershell/module/azurerm.automation/register-azurermautomationdscnode?view=azurermps-6.13.0), se obtiene el siguiente error:

```error
One or more errors occurred.
```

### <a name="cause"></a>Causa

Este error se produce cuando se intenta registrar un nodo en una suscripción independiente de la que usa la cuenta de Automation.

### <a name="resolution"></a>Solución

Trate el nodo entre suscripciones como si se hubiera definido para una nube independiente o local. Registre el nodo mediante una de estas opciones de incorporación:

* Windows: [máquinas físicas y virtuales con Windows locales o en una nube que no sea Azure/AWS](../automation-dsc-onboarding.md#onboarding-physicalvirtual-windows-machines).
* Linux: [máquinas físicas y virtuales Linux locales o en una nube que no sea Azure](../automation-dsc-onboarding.md#onboarding-physicalvirtual-linux-machines).

## <a name="scenario-error-message---provisioning-failed"></a><a name="agent-has-a-problem"></a>Escenario: mensaje de error: "Error de aprovisionamiento"

### <a name="issue"></a>Problema

Al registrar un nodo, se muestra el error:

```error
Provisioning has failed
```

### <a name="cause"></a>Causa

Este mensaje se produce cuando hay un problema de conectividad entre el nodo y Azure.

### <a name="resolution"></a>Solución

Determine si el nodo está en una red virtual privada (VPN) o si tiene otros problemas para conectarse a Azure. Consulte [Solución de problemas de errores al incorporar soluciones](onboarding.md).

## <a name="scenario-failure-with-a-general-error-when-applying-a-configuration-in-linux"></a><a name="failure-linux-temp-noexec"></a>Escenario: error general al aplicar una configuración en Linux

### <a name="issue"></a>Problema

Al aplicar una configuración en Linux, se produce el error:

```error
This event indicates that failure happens when LCM is processing the configuration. ErrorId is 1. ErrorDetail is The SendConfigurationApply function did not succeed.. ResourceId is [resource]name and SourceInfo is ::nnn::n::resource. ErrorMessage is A general error occurred, not covered by a more specific error code..
```

### <a name="cause"></a>Causa

Si la ubicación **/tmp** se establece en `noexec`, la versión actual de DSC no puede aplicar las configuraciones.

### <a name="resolution"></a>Solución

Quite la opción `noexec` de la ubicación **/tmp**.

## <a name="scenario-node-configuration-names-that-overlap-can-result-in-bad-release"></a><a name="compilation-node-name-overlap"></a>Escenario: los nombres de configuración de nodos que se superponen pueden provocar una versión incorrecta

### <a name="issue"></a>Problema

Si usa un único script de configuración para generar varias configuraciones de nodo y algunos nombres de configuración de nodos son subconjuntos de otros nombres, el servicio de compilación puede acabar asignando la configuración incorrecta. Este problema solo se produce cuando se usa un único script para generar configuraciones con datos de configuración por nodo y solo cuando la superposición de los nombres tiene lugar al principio de la cadena. Un ejemplo es un script de configuración único para generar configuraciones basadas en los datos de nodo pasados como una tabla hash mediante cmdlets, y los datos del nodo incluyen servidores denominados **server** y **1server**.

### <a name="cause"></a>Causa

Este es un problema conocido con el servicio de compilación.

### <a name="resolution"></a>Solución

La mejor solución sería realizar la compilación localmente o en una canalización de CI/CD y cargar los archivos MOF de configuración de nodos directamente en el servicio. Si es necesario que la compilación tenga lugar en el servicio, la siguiente mejor solución es dividir los trabajos de compilación para que no haya nombres superpuestos.

## <a name="scenario-gateway-timeout-error-on-dsc-configuration-upload"></a><a name="gateway-timeout"></a>Escenario: Error de tiempo de espera de puerta de enlace en la carga de la configuración de DSC

#### <a name="issue"></a>Problema

Recibe un error `GatewayTimeout` al cargar una configuración de DSC. 

### <a name="cause"></a>Causa

Las configuraciones de DSC que tardan mucho tiempo en compilarse pueden producir este error.

### <a name="resolution"></a>Solución

Las configuraciones de DSC se pueden analizar más rápidamente si se incluye expresamente el parámetro `ModuleName` en cualquier llamada [Import-DSCResource](https://docs.microsoft.com/powershell/scripting/dsc/configurations/import-dscresource?view=powershell-5.1).

## <a name="next-steps"></a>Pasos siguientes

Si su problema no aparece arriba o no puede resolverlo, intente obtener ayuda adicional a través de uno de los siguientes canales:

* Obtenga respuestas de expertos de Azure en los [foros de Azure](https://azure.microsoft.com/support/forums/).
* Póngase en contacto con [@AzureSupport](https://twitter.com/azuresupport), la cuenta oficial de Microsoft Azure para mejorar la experiencia del cliente, que pone en contacto a la comunidad de Azure con los recursos adecuados: respuestas, soporte técnico y expertos.
* Registrar un incidente de soporte técnico de Azure. Vaya al [sitio de soporte técnico de Azure](https://azure.microsoft.com/support/options/) y seleccione **Obtener soporte**.