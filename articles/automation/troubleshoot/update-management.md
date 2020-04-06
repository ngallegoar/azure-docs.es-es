---
title: Solucionar problemas relativos a errores con Azure Update Management
description: Obtenga información acerca de la solución de problemas con la solución Update Management en Azure.
services: automation
author: mgoedtel
ms.author: magoedte
ms.date: 03/17/2020
ms.topic: conceptual
ms.service: automation
manager: carmonm
ms.openlocfilehash: 91bcdc65a7ff3bcaf09f12d69ba4c7aaeb84ffa7
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "80132852"
---
# <a name="troubleshooting-issues-with-update-management"></a>Solución de problemas relacionados con Update Management

En este artículo se describen soluciones para los problemas que puedan surgir al usar Update Management.

Existe un agente solucionador de problemas para que el agente de Hybrid Worker determine el problema subyacente. Para más información sobre el solucionador de problemas, consulte el artículo sobre [cómo solucionar problemas con el agente de actualización](update-agent-issues.md). Para todos los demás problemas, siga las instrucciones de solución de problemas siguientes.

Si se producen problemas al intentar incorporar la solución a una máquina virtual (VM), compruebe el registro de **Operations Manager** en **Registros de aplicaciones y servicios** en la máquina local en busca de eventos con el id. de evento 4502 y los detalles de evento que contengan `Microsoft.EnterpriseManagement.HealthService.AzureAutomation.HybridAgent`.

En la sección siguiente se destacan los mensajes de error específicos y posibles soluciones para cada uno. Para otros problemas sobre la incorporación, consulte [Solución de problemas de errores al incorporar soluciones](onboarding.md).

## <a name="scenario-you-receive-the-error-failed-to-enable-the-update-solution"></a>Escenario: Recibe el error "No se pudo habilitar la solución de actualización"

### <a name="issue"></a>Problema

Al intentar habilitar la solución Update Management en su cuenta de Automation, se produce este error:

```error
Error details: Failed to enable the Update solution
```

### <a name="cause"></a>Causa

Este error puede ocurrir debido a uno de los siguientes motivos:

* Es posible que los requisitos del firewall de red para el agente de Log Analytics no estén configurados correctamente, lo que impide que el agente resuelva las direcciones URL de DNS.

* El destino de solución no está correctamente configurado y la máquina no recibe las actualizaciones según lo esperado.

* Es posible que también note que la máquina muestra un estado de `Non-compliant` en **Cumplimiento**. Al mismo tiempo, **Preparación de actualización del agente** informa el agente como `Disconnected`.

### <a name="resolution"></a>Solución

* Ejecute el solucionador de problemas para [Windows](update-agent-issues.md#troubleshoot-offline) o [Linux](update-agent-issues-linux.md#troubleshoot-offline), según el sistema operativo.

* Vaya a [Network planning](../automation-hybrid-runbook-worker.md#network-planning) (Planeamiento de red) para obtener información acerca de qué direcciones y puertos deben permitirse para que Update Management funcione.  

* Vaya a [Network planning](../../azure-monitor/platform/log-analytics-agent.md#network-requirements) (Planeamiento de red) para información sobre qué direcciones y puertos deben permitirse para que funcione el agente de Log Analytics.

* Compruebe si hay problemas de configuración de ámbito. La [configuración de ámbito](../automation-onboard-solutions-from-automation-account.md#scope-configuration) determina qué máquinas se configuran para la solución. Si la máquina aparece en el área de trabajo, pero no se muestra en el portal de **Update Management**, tendrá que establecer la configuración de ámbito para dirigirse a las máquinas. Para información sobre la configuración de ámbito, consulte [Incorporación de máquinas en el área de trabajo](../automation-onboard-solutions-from-automation-account.md#onboard-machines-in-the-workspace).

* Para quitar la configuración de trabajo, siga los pasos que aparecen en [Eliminación de una instancia de Hybrid Runbook Worker](../automation-hybrid-runbook-worker.md#remove-a-hybrid-runbook-worker). 

## <a name="scenario-superseded-update-indicated-as-missing-in-update-management"></a>Escenario: En Update Management se indica que falta una actualización reemplazada

### <a name="issue"></a>Problema

Se indica que faltan actualizaciones antiguas en Update Management, en la cuenta de Automation, aunque se han reemplazado. Una actualización reemplazada es aquella que no es necesario instalar, puesto que hay una disponible posterior que corrige la misma vulnerabilidad. Update Management omite la actualización reemplazada y no la convierte en aplicable en favor de la actualización que la reemplaza. Para obtener información sobre un problema relacionado, vea [Actualización reemplazada](https://docs.microsoft.com/windows/deployment/update/windows-update-troubleshooting#the-update-is-not-applicable-to-your-computer).

### <a name="cause"></a>Causa

Las actualizaciones reemplazadas no se están indicando correctamente como rechazadas de modo que se puedan considerar no aplicables.

### <a name="resolution"></a>Solución

Cuando una actualización reemplazada no sea aplicable al 100 %, debe cambiar su estado de aprobación a `Declined`. Para hacer esto con todas las actualizaciones:

1. En la cuenta de Automation, seleccione **Update Management** para ver el estado de las máquinas. Vea [Visualización de la evaluación de la actualización](../manage-update-multi.md#view-an-update-assessment).

2. Compruebe la actualización reemplazada para asegurarse de que no es aplicable al cien por cien. 

3. Marque la actualización como rechazada a menos que tenga una pregunta sobre ella. 

4. Seleccione **Equipos** y, en la columna **Cumplimiento**, fuerce un nuevo examen de cumplimiento. Vea [Administración de actualizaciones para varias máquinas](../manage-update-multi.md).

5. Repita los pasos anteriores para otras actualizaciones reemplazadas.

6. Ejecute el asistente para la limpieza para eliminar archivos de las actualizaciones rechazadas. 

7. En WSUS, limpie manualmente todas las actualizaciones reemplazadas a fin de actualizar la infraestructura.

8. Repita este procedimiento con regularidad para corregir el problema de visualización y minimizar la cantidad de espacio en disco que se usa para la administración de actualizaciones.

## <a name="scenario-machines-dont-show-up-in-the-portal-under-update-management"></a><a name="nologs"></a>Escenario: Las máquinas no se muestran en el portal en Update Management

### <a name="issue"></a>Problema

Los equipos presentan estos síntomas:

* La máquina muestra `Not configured` en la vista de Update Management de una máquina virtual.

* Faltan las máquinas en la vista de Update Management de su cuenta de Azure Automation.

* Tiene máquinas que se muestran como `Not assessed` en **Cumplimiento**. Sin embargo, los datos de latido se ven en registros de Azure Monitor para la Hybrid Runbook Worker, pero no para Update Management.

### <a name="cause"></a>Causa

Este problema puede deberse a problemas de configuración local o a que la configuración de ámbito no se ha realizado correctamente.

Es posible que tenga que volver a registrar y reinstalar Hybrid Runbook Worker.

Es posible que se haya alcanzado una cuota definida en el área de trabajo y que impida el almacenamiento de datos adicional.

### <a name="resolution"></a>Solución

* Ejecute el solucionador de problemas para [Windows](update-agent-issues.md#troubleshoot-offline) o [Linux](update-agent-issues-linux.md#troubleshoot-offline), según el sistema operativo.

* Asegúrese de que las notificaciones de la máquina se envían al área de trabajo correcta. Para instrucciones sobre cómo comprobar este aspecto, consulte [Comprobación de la conectividad del agente a Log Analytics](../../azure-monitor/platform/agent-windows.md#verify-agent-connectivity-to-log-analytics). Asegúrese también de que esta área de trabajo esté vinculada a su cuenta de Azure Automation. Para ello, vaya a la cuenta de Automation y seleccione **Área de trabajo vinculada** en **Recursos relacionados**.

* Asegúrese de que las máquinas se muestran en el área de trabajo de Log Analytics. Ejecute la siguiente consulta en el área de trabajo de Log Analytics que está vinculada a su cuenta de Automation:

  ```loganalytics
  Heartbeat
  | summarize by Computer, Solutions
  ```
 
  Si no ve la máquina en los resultados de consulta, es que no se ha registrado recientemente, lo que significa que probablemente haya un problema de configuración local y puede [volver a instalar el agente](../../azure-monitor/learn/quick-collect-windows-computer.md#install-the-agent-for-windows). Si la máquina se muestra en los resultados de consulta, deberá comprobar la configuración de ámbito especificada en el siguiente elemento de la lista con viñetas.

* Compruebe si hay problemas de configuración de ámbito. La [configuración de ámbito](../automation-onboard-solutions-from-automation-account.md#scope-configuration) determina qué máquinas se configuran para la solución. Si la máquina aparece en el área de trabajo, pero no se muestra en el portal de **Update Management**, tendrá que realizar la configuración de ámbito para dirigirse a las máquinas. Para información sobre cómo hacerlo, consulte [Incorporación de máquinas en el área de trabajo](../automation-onboard-solutions-from-automation-account.md#onboard-machines-in-the-workspace).

* En el área de trabajo, ejecute la siguiente consulta:

  ```loganalytics
  Operation
  | where OperationCategory == 'Data Collection Status'
  | sort by TimeGenerated desc
  ```

  Si recibe el resultado `Data collection stopped due to daily limit of free data reached. Ingestion status = OverQuota`, tiene una cuota definida en el área de trabajo que se ha alcanzado y ha impedido que se guarden los datos. En el área de trabajo, vaya a **Uso y costos estimados** > **Administración del volumen de datos** y compruebe su cuota o elimínela.

* Si con estos pasos no se resuelve el problema, siga los pasos descritos en [Implementación de Hybrid Runbook Worker en Windows](../automation-windows-hrw-install.md) para volver a instalar Hybrid Worker para Windows. Para Linux, siga los pasos que aparecen en [Implementación de Hybrid Runbook Worker en Linux](../automation-linux-hrw-install.md).

## <a name="scenario-unable-to-register-automation-resource-provider-for-subscriptions"></a><a name="rp-register"></a>Escenario: No se puede registrar el proveedor de recursos de Automation para las suscripciones

### <a name="issue"></a>Problema

Al trabajar con soluciones en la cuenta de Automation, se encuentra con el siguiente error:

```error
Error details: Unable to register Automation Resource Provider for subscriptions:
```

### <a name="cause"></a>Causa

El proveedor de recursos de Automation no está registrado en la suscripción.

### <a name="resolution"></a>Solución

Para registrar el proveedor de recursos de Automation, realice los pasos siguientes en Azure Portal:

1. En la lista de servicios de Azure de la parte inferior del portal, seleccione **Todos los servicios** y, a continuación, seleccione **Suscripciones** en el grupo de servicios General.
2. Seleccione su suscripción.
3. En **Configuración**, seleccione **Proveedores de recursos**.
4. En la lista de proveedores de recursos, compruebe que el proveedor de recursos de `Microsoft.Automation` esté registrado.
5. Si no aparece, registre el proveedor de `Microsoft.Automation` siguiendo los pasos que aparecen en [Resolución de errores del registro del proveedor de recursos](/azure/azure-resource-manager/resource-manager-register-provider-errors).

## <a name="scenario-components-for-update-management-solution-enabled-while-vm-continues-to-show-as-being-configured"></a><a name="components-enabled-not-working"></a>Escenario: Componentes de la solución de Update Management habilitados, mientras que la máquina virtual se sigue mostrando como configurada

### <a name="issue"></a>Problema

Continúa recibiendo el mensaje siguiente en una máquina virtual 15 minutos después de la incorporación:

```error
The components for the 'Update Management' solution have been enabled, and now this virtual machine is being configured. Please be patient, as this can sometimes take up to 15 minutes.
```

### <a name="cause"></a>Causa

Este error puede ocurrir debido a uno de los siguientes motivos:

* Está bloqueada la comunicación con la cuenta de Automation.

* Hay un nombre de equipo duplicado con identificadores de equipo de origen diferentes. Este escenario se produce cuando se crea una máquina virtual con un nombre de equipo determinado en distintos grupos de recursos y envía notificaciones a la misma área de trabajo del agente de logística en la suscripción.

* La imagen de máquina virtual que se incorpora puede provenir de una máquina clonada que no se preparó con la preparación del sistema (sysprep) con Microsoft Monitoring Agent (MMA) instalado.

### <a name="resolution"></a>Solución

Para ayudar a determinar el problema exacto con la máquina virtual, ejecute la consulta siguiente en el área de trabajo de Log Analytics que está vinculada a su cuenta de Automation:

```
Update
| where Computer contains "fillInMachineName"
| project TimeGenerated, Computer, SourceComputerId, Title, UpdateState 
```

#### <a name="communication-with-automation-account-blocked"></a>Comunicación con la cuenta de Automation bloqueada

Vaya a [Network planning](../automation-update-management.md#ports) (Planeamiento de red) para obtener información acerca de qué direcciones y puertos deben permitirse para que Update Management funcione.

#### <a name="duplicate-computer-name"></a>Nombre de equipo duplicado

Cambie el nombre de las máquinas virtuales para asegurarse de que los nombres sean únicos en el entorno.

#### <a name="onboarded-image-from-cloned-machine"></a>Imagen incorporada desde una máquina clonada

Si usa una imagen clonada, los distintos nombres de equipo tienen el mismo identificador de equipo de origen. En este caso:

1. En el área de trabajo de Log Analytics, quite la máquina virtual de la búsqueda guardada en la configuración de ámbito `MicrosoftDefaultScopeConfig-Updates` si se muestra. Las búsquedas guardadas se pueden encontrar en la sección **General** del área de trabajo.

2. Ejecute el siguiente cmdlet:

    ```azurepowershell-interactive
    Remove-Item -Path "HKLM:\software\microsoft\hybridrunbookworker" -Recurse -Force
    ```

3. Ejecute `Restart-Service HealthService` para reiniciar el servicio de mantenimiento. Esta operación vuelve a crear la clave y genera un nuevo UUID.

4. Si este enfoque no funciona, en primer lugar, ejecute sysprep en la imagen y, a continuación, instale MMA.

## <a name="scenario-you-receive-a-linked-subscription-error-when-you-create-an-update-deployment-for-machines-in-another-azure-tenant"></a><a name="multi-tenant"></a>Escenario: Recibe un error de la suscripción vinculada al crear una implementación de actualización para las máquinas en otro inquilino de Azure

### <a name="issue"></a>Problema

Encuentra el error siguiente al intentar crear una implementación de actualización para las máquinas en otro inquilino de Azure:

```error
The client has permission to perform action 'Microsoft.Compute/virtualMachines/write' on scope '/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/resourceGroupName/providers/Microsoft.Automation/automationAccounts/automationAccountName/softwareUpdateConfigurations/updateDeploymentName', however the current tenant '00000000-0000-0000-0000-000000000000' is not authorized to access linked subscription '00000000-0000-0000-0000-000000000000'.
```

### <a name="cause"></a>Causa

Este error se produce cuando se crea una implementación de actualización que tiene máquinas virtuales de Azure en otro inquilino que se incluye en una implementación de actualización.

### <a name="resolution"></a>Solución

Use la solución alternativa siguiente para programar estos elementos. Puede usar el cmdlet [New-AzureRmAutomationSchedule](/powershell/module/azurerm.automation/new-azurermautomationschedule) con el parámetro `ForUpdate` para crear una programación. A continuación, use el cmdlet [New-AzureRmAutomationSoftwareUpdateConfiguration](/powershell/module/azurerm.automation/new-azurermautomationsoftwareupdateconfiguration
) y pase las máquinas del otro inquilino al parámetro `NonAzureComputer`. El ejemplo siguiente muestra cómo hacerlo:

```azurepowershell-interactive
$nonAzurecomputers = @("server-01", "server-02")

$startTime = ([DateTime]::Now).AddMinutes(10)

$s = New-AzureRmAutomationSchedule -ResourceGroupName mygroup -AutomationAccountName myaccount -Name myupdateconfig -Description test-OneTime -OneTime -StartTime $startTime -ForUpdate

New-AzureRmAutomationSoftwareUpdateConfiguration  -ResourceGroupName $rg -AutomationAccountName $aa -Schedule $s -Windows -AzureVMResourceId $azureVMIdsW -NonAzureComputer $nonAzurecomputers -Duration (New-TimeSpan -Hours 2) -IncludedUpdateClassification Security,UpdateRollup -ExcludedKbNumber KB01,KB02 -IncludedKbNumber KB100
```

## <a name="scenario-unexplained-reboots"></a><a name="node-reboots"></a>Escenario: Reinicios inexplicables

### <a name="issue"></a>Problema

Aunque haya establecido la opción **Reboot Control** (Control de reinicio) en **No reiniciar nunca**, las máquinas todavía se reinician después de instalar las actualizaciones.

### <a name="cause"></a>Causa

Windows Update se puede modificar mediante varias claves del Registro, cualquiera de ellas puede modificar el comportamiento del reinicio.

### <a name="resolution"></a>Solución

Revise las claves del Registro enumeradas en [Configuración de actualizaciones automáticas mediante la edición del Registro](/windows/deployment/update/waas-wu-settings#configuring-automatic-updates-by-editing-the-registry) y [Claves del Registro usadas para administrar reinicios](/windows/deployment/update/waas-restart#registry-keys-used-to-manage-restart) para asegurarse de que las máquinas estén configuradas correctamente.

## <a name="scenario-machine-shows-failed-to-start-in-an-update-deployment"></a><a name="failed-to-start"></a>Escenario: Una máquina muestra "No se pudo iniciar" en una implementación de actualizaciones

### <a name="issue"></a>Problema

Una máquina muestra un estado `Failed to start`. Al ver los detalles específicos de la máquina, ve el siguiente error:

```error
Failed to start the runbook. Check the parameters passed. RunbookName Patch-MicrosoftOMSComputer. Exception You have requested to create a runbook job on a hybrid worker group that does not exist.
```

### <a name="cause"></a>Causa

Este problema puede ocurrir debido a uno de los siguientes motivos:

* La máquina ya no existe.
* La máquina está desactivada y no se posible establecer comunicación con ella.
* La máquina tiene un problema de conectividad de red y, por lo tanto, no se puede alcanzar la instancia de Hybrid Worker de la máquina.
* Hubo una actualización de MMA que hizo cambiar el identificador de equipo de origen.
* La ejecución de actualizaciones se ha regulado si se ha alcanzado el límite de 2000 trabajos simultáneos en una cuenta de Automation. Cada implementación se considera un trabajo y cada máquina de una implementación de actualizaciones se cuenta como un trabajo. Cualquier otro trabajo de automatización o implementación de actualizaciones actualmente en ejecución en la cuenta de Automation cuenta para el límite de trabajos simultáneos.

### <a name="resolution"></a>Solución

Cuando proceda, use [grupos dinámicos](../automation-update-management-groups.md) para las implementaciones de actualizaciones. Además:

* Compruebe que la máquina aún exista y sea accesible. 
* Si no existe, edite la implementación y quite la máquina.
* Consulte la sección sobre el [planeamiento de red](../automation-update-management.md#ports) para obtener una lista de puertos y direcciones que son necesarios para Update Management y asegúrese de que la máquina cumple estos requisitos.
* Compruebe la conectividad con el Hybrid Runbook Worker mediante el solucionador de problemas del agente de Hybrid Runbook Worker. Para más información sobre el solucionador de problemas, consulte el artículo sobre [cómo solucionar problemas con el agente de actualización](update-agent-issues.md).
* Ejecute la siguiente consulta en Log Analytics para encontrar las máquinas del entorno cuyo identificador de equipo de origen haya cambiado. Busque los equipos que tienen el mismo valor de `Computer`, pero un distinto valor de `SourceComputerId`.

   ```loganalytics
   Heartbeat | where TimeGenerated > ago(30d) | distinct SourceComputerId, Computer, ComputerIP
   ```

* Después de encontrar las máquinas afectadas, debe editar las implementaciones de actualizaciones que se dirijan a esas máquinas, y quitarlas y volverlas a agregar para que `SourceComputerId` refleje el valor correcto.

## <a name="scenario-updates-are-installed-without-a-deployment"></a><a name="updates-nodeployment"></a>Escenario: Las actualizaciones se instalan sin una implementación

### <a name="issue"></a>Problema

Al inscribir una máquina Windows en Update Management, puede ver las actualizaciones instaladas sin una implementación.

### <a name="cause"></a>Causa

En Windows, las actualizaciones se instalan automáticamente en cuanto están disponibles. Este comportamiento puede producir confusión si no ha programado que una actualización se implemente en la máquina.

### <a name="resolution"></a>Solución

La clave del Registro `HKEY_LOCAL_MACHINE\SOFTWARE\Policies\Microsoft\Windows\WindowsUpdate\AU` se establece de manera predeterminada en un valor de 4: `auto download and install`.

En el caso de los clientes de Update Management, se recomienda establecer esta clave en 3: `auto download but do not auto install`.

Para más información, consulte [Configuración de actualizaciones automáticas](https://docs.microsoft.com/windows/deployment/update/waas-wu-settings#configure-automatic-updates).

## <a name="scenario-machine-is-already-registered-to-a-different-account"></a><a name="machine-already-registered"></a>Escenario: La máquina ya está registrada en otra cuenta

### <a name="issue"></a>Problema

Aparece el siguiente mensaje de error:

```error
Unable to Register Machine for Patch Management, Registration Failed with Exception System.InvalidOperationException: {"Message":"Machine is already registered to a different account."}
```

### <a name="cause"></a>Causa

La máquina ya se ha incorporado a otra área de trabajo para Update Management.

### <a name="resolution"></a>Solución

1. Siga los pasos de [Las máquinas no se muestran en el portal en Update Management](#nologs) para asegurarse de que la máquina envía notificaciones al área de trabajo adecuada.
2. Limpie los artefactos en la máquina mediante la [eliminación del grupo Hybrid Runbook](../automation-hybrid-runbook-worker.md#remove-a-hybrid-worker-group) y vuelva a intentarlo.

## <a name="scenario-machine-cant-communicate-with-the-service"></a><a name="machine-unable-to-communicate"></a>Escenario: La máquina no se puede comunicar con el servicio

### <a name="issue"></a>Problema

Aparece uno de los siguientes mensajes de error:

```error
Unable to Register Machine for Patch Management, Registration Failed with Exception System.Net.Http.HttpRequestException: An error occurred while sending the request. ---> System.Net.WebException: The underlying connection was closed: An unexpected error occurred on a receive. ---> System.ComponentModel.Win32Exception: The client and server can't communicate, because they do not possess a common algorithm
```

```error
Unable to Register Machine for Patch Management, Registration Failed with Exception Newtonsoft.Json.JsonReaderException: Error parsing positive infinity value.
```

```error
The certificate presented by the service <wsid>.oms.opinsights.azure.com was not issued by a certificate authority used for Microsoft services. Contact your network administrator to see if they are running a proxy that intercepts TLS/SSL communication.
```

```error
Access is denied. (Exception form HRESULT: 0x80070005(E_ACCESSDENIED))
```

### <a name="cause"></a>Causa

Un proxy, una puerta de enlace o un firewall pueden estar bloqueando la comunicación de red. 

### <a name="resolution"></a>Solución

Revise la red y asegúrese de que están permitidas las direcciones y los puertos adecuados. Consulte los [requisitos de red](../automation-hybrid-runbook-worker.md#network-planning) para obtener una lista de puertos y direcciones que Update Management necesita y las instancias de Hybrid Runbook Worker.

## <a name="scenario-unable-to-create-self-signed-certificate"></a><a name="unable-to-create-selfsigned-cert"></a>Escenario: Error al crear el certificado autofirmado

### <a name="issue"></a>Problema

Aparece uno de los siguientes mensajes de error:

```error
Unable to Register Machine for Patch Management, Registration Failed with Exception AgentService.HybridRegistration. PowerShell.Certificates.CertificateCreationException: Failed to create a self-signed certificate. ---> System.UnauthorizedAccessException: Access is denied.
```

### <a name="cause"></a>Causa

Hybrid Runbook Worker no pudo generar un certificado autofirmado.

### <a name="resolution"></a>Solución

Verifique que la cuenta del sistema tiene acceso de lectura a la carpeta **C:\ProgramData\Microsoft\Crypto\RSA** e inténtelo de nuevo.

## <a name="scenario-the-scheduled-update-failed-with-a-maintenancewindowexceeded-error"></a><a name="mw-exceeded"></a>Escenario: Error en la actualización programada con un error MaintenanceWindowExceeded

### <a name="issue"></a>Problema

La ventana de mantenimiento predeterminada para las actualizaciones es de 120 minutos. Puede aumentar la ventana de mantenimiento a un máximo de seis 6 horas o 360 minutos.

### <a name="resolution"></a>Solución

Edite las implementaciones de actualizaciones programadas con errores y aumente la ventana de mantenimiento.

Para más información sobre las ventanas de mantenimiento, consulte la [instalación de actualizaciones](../automation-tutorial-update-management.md#schedule-an-update-deployment).

## <a name="scenario-machine-shows-as-not-assessed-and-shows-an-hresult-exception"></a><a name="hresult"></a>Escenario: La máquina aparece como "No evaluado" y se muestra una excepción HRESULT

### <a name="issue"></a>Problema

* Tiene máquinas que aparecen como `Not assessed` en **Cumplimiento** y verá un mensaje de excepción debajo de él.
* Se muestra un código de error HRESULT en el portal.

### <a name="cause"></a>Causa

El agente de actualización (Agente de Windows Update en Windows, el administrador de paquetes para la distribución de Linux) no está configurado correctamente. Update Management se basa en el agente de actualización de la máquina para proporcionar las actualizaciones necesarias, el estado de la revisión y los resultados de las revisiones implementadas. Sin esta información, Update Management no puede informar correctamente de las revisiones que son necesarias o que están instaladas.

### <a name="resolution"></a>Solución

Intente realizar actualizaciones de forma local en la máquina. Si no puede, suele deberse a un error de configuración con el agente de actualización.

Este problema suele deberse a problemas de firewall y de configuración de red. Realice lo siguiente:

* Para Linux, consulte la documentación correspondiente para asegurarse de que puede acceder al punto de conexión de red del repositorio de paquetes.
* Para Windows, compruebe la configuración del agente como se indica en [Las actualizaciones no se descargan desde el punto de conexión de la intranet (WSUS/SCCM)](/windows/deployment/update/windows-update-troubleshooting#updates-arent-downloading-from-the-intranet-endpoint-wsussccm).
  * Si las máquinas están configuradas para Windows Update, asegúrese de que puede acceder a los puntos de conexión descritos en [Problemas relacionados con HTTP/proxy](/windows/deployment/update/windows-update-troubleshooting#issues-related-to-httpproxy).
  * Si las máquinas están configuradas para Windows Server Update Services (WSUS), asegúrese de que puede acceder al servidor WSUS configurado por la [clave del Registro WUServer ](/windows/deployment/update/waas-wu-settings).

Si se muestra un código de error HRESULT, haga doble clic en la excepción que se muestra en rojo para ver el mensaje de excepción completo. Revise la siguiente tabla para ver posibles soluciones o acciones recomendadas:

|Excepción  |Acción o resolución  |
|---------|---------|
|`Exception from HRESULT: 0x……C`     | Busque el código de error correspondiente en la [lista de códigos de error de Windows Update](https://support.microsoft.com/help/938205/windows-update-error-code-list) para buscar detalles adicionales sobre la causa de la excepción.        |
|`0x8024402C`</br>`0x8024401C`</br>`0x8024402F`      | Estos errores indican problemas de conectividad de red. Asegúrese de que la máquina tenga conectividad de red con Update Management. Consulte la sección sobre el [planeamiento de red](../automation-update-management.md#ports) para obtener una lista de puertos y direcciones necesarios.        |
|`0x8024001E`| No se completó la operación de actualización porque se estaba cerrando el servicio o el sistema.|
|`0x8024002E`| El servicio de Windows Update está deshabilitado.|
|`0x8024402C`     | Si usa un servidor WSUS, asegúrese de que los valores del Registro para `WUServer` y `WUStatusServer` en la clave del Registro `HKEY_LOCAL_MACHINE\SOFTWARE\Policies\Microsoft\Windows\WindowsUpdate` especifican el servidor WSUS correcto.        |
|`0x80072EE2`|Existe un problema de conectividad de red o un problema al comunicarse con un servidor WSUS configurado. Compruebe la configuración de WSUS y asegúrese de que se puede acceder al servicio desde el cliente.|
|`The service cannot be started, either because it is disabled or because it has no enabled devices associated with it. (Exception from HRESULT: 0x80070422)`     | Asegúrese de que el servicio Windows Update (wuauserv) se está ejecutando y no está deshabilitado.        |
|`0x80070005`| Un error de acceso denegado puede deberse a cualquiera de las siguientes razones:<br> Equipo infectado<br> Windows Update no está configurado correctamente.<br> Error de permiso de archivo en la carpeta %WinDir%\SoftwareDistribution<br> Espacio en disco insuficiente en la unidad del sistema (C:).
|Cualquier otra excepción genérica     | Ejecute una búsqueda en Internet para ver las soluciones posibles y colabore con el equipo de soporte técnico de TI local.         |

Revisar el archivo %Windir%\Windowsupdate.log también puede ayudar a determinar los posibles motivos. Para más información sobre cómo leer el registro, consulte [Cómo leer el archivo Windowsupdate.log](https://support.microsoft.com/en-ca/help/902093/how-to-read-the-windowsupdate-log-file).

También puede descargar y ejecutar el [solucionador de problemas de Windows Update](https://support.microsoft.com/help/4027322/windows-update-troubleshooter) para comprobar si hay algún problema con Windows Update en el equipo.

> [!NOTE]
> La documentación del [solucionador de problemas de Windows Update](https://support.microsoft.com/help/4027322/windows-update-troubleshooter) indica que es para su uso en clientes de Windows, pero también funciona en Windows Server.

## <a name="scenario-update-run-returns-failed-status-linux"></a>Escenario: La ejecución de la actualización devuelve un estado "Error" (Linux)

### <a name="issue"></a>Problema

Se inicia una ejecución de actualización, pero encuentra errores durante la ejecución.

### <a name="cause"></a>Causa

Causas posibles:

* El estado del administrador de paquetes no es correcto.
* El agente de actualización (WUA para Windows, el administrador de paquetes específico de la distribución para Linux) no está configurado correctamente.
* Paquetes específicos interfieren con la aplicación de revisiones basada en la nube.
* La máquina es inaccesible.
* Las actualizaciones tenían dependencias que no se resolvieron.

### <a name="resolution"></a>Solución

Si se producen errores durante una ejecución de actualizaciones después de que se haya iniciado correctamente, [compruebe el trabajo de salida](../manage-update-multi.md#view-results-of-an-update-deployment) desde la máquina afectada en la ejecución. Puede encontrar mensajes de error específicos procedentes de las máquinas que puede investigar e intentar solucionar. Update Management requiere que el administrador de paquetes tenga un estado correcto para que las implementaciones de actualizaciones se realicen con éxito.

Si se observan revisiones, paquetes o actualizaciones específicos inmediatamente antes de que se produzca un error en el trabajo, puede intentar [excluirlos](../automation-tutorial-update-management.md#schedule-an-update-deployment) de la siguiente implementación de actualizaciones. Para recopilar información de registro de Windows Update, consulte [Archivo de registro de Windows Update](/windows/deployment/update/windows-update-logs).

Si no puede resolver un problema de aplicación de revisiones, realice una copia del archivo de registro siguiente y consérvela para solucionar los problemas antes de que se inicie la siguiente implementación de actualizaciones:

```bash
/var/opt/microsoft/omsagent/run/automationworker/omsupdatemgmt.log
```

## <a name="patches-arent-installed"></a>Las revisiones no se instalan

### <a name="machines-dont-install-updates"></a>Las máquinas no instalan las actualizaciones

Intente ejecutar las actualizaciones directamente en la máquina. Si la máquina no se puede aplicar las actualizaciones, consulte la [lista de posibles errores en la guía de solución de problemas](https://docs.microsoft.com/azure/automation/troubleshoot/update-management#hresult).

Si las actualizaciones se ejecutan localmente, intente quitar y volver a instalar el agente en la máquina siguiendo las instrucciones para [quitar una máquina virtual de Update Management](https://docs.microsoft.com/azure/automation/automation-onboard-solutions-from-browse#clean-up-resources).

### <a name="i-know-updates-are-available-but-they-dont-show-as-available-on-my-machines"></a>Sé que hay actualizaciones disponibles, pero no se muestran como disponibles en mis máquinas

Esto sucede a menudo si las máquinas están configuradas para obtener actualizaciones de WSUS o Microsoft Endpoint Configuration Manager pero WSUS y Configuration Manager no han aprobado las actualizaciones.

Puede comprobar si las máquinas están configuradas para WSUS y SSCM mediante la referencia cruzada de la clave del Registro `UseWUServer` con las claves del Registro de la sección [Configuración de actualizaciones automáticas mediante la edición del Registro de este artículo](https://support.microsoft.com/help/328010/how-to-configure-automatic-updates-by-using-group-policy-or-registry-s).

Si las actualizaciones no se aprueban en WSUS, no se instalarán. Puede comprobar si hay actualizaciones no aprobadas en Log Analytics ejecutando la consulta siguiente.

  ```loganalytics
  Update | where UpdateState == "Needed" and ApprovalSource == "WSUS" and Approved == "False" | summarize max(TimeGenerated) by Computer, KBID, Title
  ```

### <a name="updates-show-as-installed-but-i-cant-find-them-on-my-machine"></a>Las actualizaciones se muestran como instaladas, pero no las encuentro en mi máquina

Con frecuencia, unas actualizaciones tienen preferencia sobre otras. Para más información, consulte [La actualización se ha reemplazado](https://docs.microsoft.com/windows/deployment/update/windows-update-troubleshooting#the-update-is-not-applicable-to-your-computer) en la guía de solución de problemas de Windows Update.

### <a name="installing-updates-by-classification-on-linux"></a>Instalación de actualizaciones mediante clasificación en Linux

La implementación de actualizaciones en Linux mediante clasificación ("actualizaciones críticas y de seguridad") tiene advertencias importantes, especialmente para CentOS. Estas limitaciones se documentan en la página de [información general de Update Management](https://docs.microsoft.com/azure/automation/automation-update-management#linux-2).

### <a name="kb2267602-is-consistently-missing"></a>KB2267602 falta constantemente

KB2267602 es la [actualización de la definición de Windows Defender](https://www.microsoft.com/wdsi/definitions). Se actualiza diariamente.

## <a name="next-steps"></a>Pasos siguientes

Si su problema no aparece o no puede resolverlo, intente uno de los siguientes canales para obtener ayuda adicional:

* Obtenga respuestas de expertos de Azure en los [foros de Azure](https://azure.microsoft.com/support/forums/).
* Póngase en contacto con [@AzureSupport](https://twitter.com/azuresupport), la cuenta oficial de Microsoft Azure para mejorar la experiencia del cliente, que pone en contacto a la comunidad de Azure con los recursos adecuados: respuestas, soporte técnico y expertos.
* Registrar un incidente de soporte técnico de Azure. Vaya al [sitio de soporte técnico de Azure](https://azure.microsoft.com/support/options/) y seleccione **Obtener soporte**.
