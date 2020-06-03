---
title: 'Escalado de hosts de sesión con Azure Automation: Azure'
description: Escalado automático de hosts de sesión de Windows Virtual Desktop con Azure Automation
services: virtual-desktop
author: Heidilohr
ms.service: virtual-desktop
ms.topic: conceptual
ms.date: 03/30/2020
ms.author: helohr
manager: lizross
ms.openlocfilehash: f659a40cbb9e3ef2d0e7fe4e527518a76507d5ee
ms.sourcegitcommit: 493b27fbfd7917c3823a1e4c313d07331d1b732f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/21/2020
ms.locfileid: "83745711"
---
# <a name="scale-session-hosts-using-azure-automation"></a>Escalado de hosts de sesión con Azure Automation

>[!IMPORTANT]
>Este contenido se aplica a la versión de otoño de 2019 que no admite objetos de Windows Virtual Desktop para Azure Resource Manager.

Puede reducir el costo total de implementación de Windows Virtual Desktop mediante el escalado de las máquinas virtuales. Esto significa que se cierran y desasignan las máquinas virtuales del host de sesión durante las horas de menos uso y, posteriormente, se vuelven a activar y se reasignan durante las horas punta.

En este artículo, obtendrá información sobre la herramienta de escalado creada con Azure Automation y Azure Logic Apps que escalará automáticamente las máquinas virtuales del host de sesión en el entorno de Windows Virtual Desktop. Para más información sobre cómo usar la herramienta de escalado, consulte los [requisitos previos](#prerequisites).

## <a name="report-issues"></a>Informar de problemas

En la actualidad, los informes de incidencias de la herramienta de escalado se controlan en GitHub en lugar de Soporte técnico de Microsoft. Si encuentra alguna incidencia con la herramienta de escalado, puede notificarla si abre una incidencia de GitHub con la etiqueta "4a-WVD-scaling-logicapps" en la [página RDS de GitHub](https://github.com/Azure/RDS-Templates/issues?q=is%3Aissue+is%3Aopen+label%3A4a-WVD-scaling-logicapps).

## <a name="how-the-scaling-tool-works"></a>Funcionamiento de la herramienta de escalado

La herramienta de escalado proporciona una opción de automatización de bajo costo para aquellos clientes que deseen optimizar los costos de las máquinas virtuales del host de sesión.

Puede usar la herramienta de escalado para:
 
- Programar las máquinas virtuales para que se inicien y se detengan según el horario comercial de horas punta y horas de menos uso.
- Escalar horizontalmente las máquinas virtuales basadas en el número de sesiones por núcleo de CPU.
- Reducir horizontalmente las máquinas virtuales durante las horas de menos uso, lo que mantiene en ejecución el número mínimo de máquinas virtuales del host de sesión.

Para funcionar, la herramienta de escalado utiliza una combinación de runbooks de PowerShell de Azure Automation, webhooks y Azure Logic Apps. Cuando se ejecuta la herramienta, Azure Logic Apps llama a un webhook para iniciar el runbook de Azure Automation. A continuación, el runbook crea un trabajo.

Durante el tiempo de uso en horas punta, el trabajo comprueba el número actual de sesiones y la capacidad de la máquina virtual del host de sesión que se está ejecutando para cada grupo de hosts. Usa esta información para calcular si las máquinas virtuales del host de sesión pueden admitir las sesiones existentes en función del parámetro *SessionThresholdPerCPU* definido en el archivo **createazurelogicapp.ps1**. Si las máquinas virtuales del host de sesión no admiten las sesiones existentes, el trabajo inicia máquinas virtuales del host de sesión adicionales en el grupo de hosts.

>[!NOTE]
>*SessionThresholdPerCPU* no restringe el número de sesiones en la máquina virtual. Este parámetro solo determina cuándo es necesario iniciar nuevas máquinas virtuales para equilibrar la carga de las conexiones. Para restringir el número de sesiones, debe seguir las instrucciones de [Set-RdsHostPool](/powershell/module/windowsvirtualdesktop/set-rdshostpool/) para configurar el parámetro *MaxSessionLimit* en consecuencia.

En las horas de menos uso, el trabajo determina qué máquinas virtuales de los hosts de sesión deben apagarse según el parámetro *MinimumNumberOfRDSH*. El trabajo establecerá las máquinas virtuales de los hosts de sesión en modo de purga para evitar que las nuevas sesiones se conecten a los hosts. Si establece el parámetro *LimitSecondsToForceLogOffUser* en un valor positivo distinto de cero, el trabajo enviará a los usuarios que tengan la sesión iniciada la notificación de que guarden su trabajo, esperará el periodo predeterminado y, después, obligará a los usuarios a cerrar la sesión. Cuando se hayan cerrado todas las sesiones de usuario en la máquina virtual del host de sesión, el trabajo apagará la máquina virtual.

Si establece el parámetro *LimitSecondsToForceLogOffUser* en cero, el trabajo permitirá que el valor de configuración de la sesión de las directivas de grupo especificadas controle el cierre de las sesiones de los usuarios. Para ver estas directivas de grupo, vaya a **Configuración del equipo** > **Directivas** > **Plantillas administrativas** > **Componentes de Windows** > **Terminal Services** > **Terminal Server** > **Límites de tiempo de sesión**. Si hay sesiones activas en una máquina virtual del host de sesión, el trabajo dejará que se ejecute esa máquina. Si no hay sesiones activas, el trabajo apagará la máquina virtual del host de sesión.

El trabajo se ejecuta periódicamente según un intervalo de periodicidad establecido. Puede cambiar este intervalo según el tamaño del entorno de Windows Virtual Desktop, pero recuerde que el inicio y el apagado de las máquinas virtuales pueden tardar algún tiempo, por lo que debe tener en cuenta el retraso. Se recomienda establecer el intervalo de periodicidad en 15 minutos.

Sin embargo, la herramienta también tiene las siguientes limitaciones:

- Esta solución solo se aplica a las máquinas virtuales de host de sesión agrupadas.
- Esta solución administra máquinas virtuales de cualquier región, pero solo se puede utilizar en la misma suscripción que la cuenta de Azure Automation y Azure Logic Apps.

>[!NOTE]
>La herramienta de escalado controla el modo de equilibrio de carga del grupo de hosts que se está escalando. Lo establece para equilibrar la amplitud de la primera carga tanto en las horas punta como en las horas de menos uso.

## <a name="prerequisites"></a>Prerrequisitos

Antes de empezar a configurar la herramienta de escalado, asegúrese de tener preparado lo siguiente:

- Un [grupo de inquilinos y de hosts de Windows Virtual Desktop](create-host-pools-arm-template.md)
- Máquinas virtuales del grupo de hosts de sesión configuradas y registradas en el servicio Windows Virtual Desktop
- Un usuario con [acceso de colaborador](../../role-based-access-control/role-assignments-portal.md) en la suscripción de Azure

La máquina que se usa para implementar la herramienta debe tener: 

- Windows PowerShell 5.1 o posterior
- El módulo Microsoft Az PowerShell

Si tiene todo preparado, vamos a empezar.

## <a name="create-an-azure-automation-account"></a>Creación de una cuenta de Azure Automation

En primer lugar, necesitará una cuenta de Azure Automation para ejecutar el runbook de PowerShell. Aquí se muestra cómo configurar la cuenta:

1. Abra Windows PowerShell como administrador.
2. Ejecute el siguiente cmdlet para iniciar sesión en la cuenta de Azure.

     ```powershell
     Login-AzAccount
     ```

     >[!NOTE]
     >La cuenta debe tener derechos de colaborador en la suscripción de Azure en la que desea implementar la herramienta de escalado.

3. Ejecute el siguiente cmdlet para descargar el script para crear la cuenta de Azure Automation:

     ```powershell
     Set-Location -Path "c:\temp"
     $uri = "https://raw.githubusercontent.com/Azure/RDS-Templates/master/wvd-templates/wvd-scaling-script/createazureautomationaccount.ps1"
     Invoke-WebRequest -Uri $uri -OutFile ".\createazureautomationaccount.ps1"
     ```

4. Ejecute el siguiente cmdlet para ejecutar el script y crear la cuenta de Azure Automation:

     ```powershell
     .\createazureautomationaccount.ps1 -SubscriptionID <azuresubscriptionid> -ResourceGroupName <resourcegroupname> -AutomationAccountName <name of automation account> -Location "Azure region for deployment"
     ```

5. La salida del cmdlet incluirá un URI de webhook. Asegúrese de mantener un registro del URI porque lo usará como parámetro cuando configure la programación de ejecución para Azure Logic Apps.

6. Después de configurar la cuenta de Azure Automation, inicie sesión en la suscripción de Azure y asegúrese de que la cuenta de Azure Automation y el runbook correspondiente han aparecido en el grupo de recursos especificado, tal como se muestra en la siguiente imagen:

![Una imagen de la página de información general de Azure que muestra la cuenta de Automation y el runbook creados recientemente.](../media/automation-account.png)

  Para comprobar si el webhook está donde debe estar, seleccione el nombre del runbook. A continuación, vaya a la sección Recursos del runbook y seleccione **Webhooks**.

## <a name="create-an-azure-automation-run-as-account"></a>Creación de una cuenta de ejecución de Azure Automation

Ahora que tiene una cuenta de Azure Automation, también deberá crear una cuenta de ejecución de Azure Automation para acceder a los recursos de Azure.

La [cuenta de ejecución en Azure Automation](../../automation/manage-runas-account.md) se usa para proporcionar autenticación con objeto de administrar los recursos de Azure con los cmdlets de Azure. Al crear una cuenta de ejecución, se crea un nuevo usuario de entidad de servicio en Azure Active Directory y se asigna el rol de colaborador al usuario de la entidad de servicio en el nivel de suscripción; la cuenta de ejecución de Azure es una excelente manera de autenticarse de forma segura con certificados y un nombre de entidad de seguridad de servicio sin necesidad de almacenar un nombre de usuario y una contraseña en un objeto de credenciales. Para obtener más información sobre la autenticación de la cuenta de ejecución, vea [Limitación de los permisos de las cuentas de ejecución](../../automation/manage-runas-account.md#limit-run-as-account-permissions).

Cualquier usuario que sea miembro del rol de administradores de suscripciones y coadministrador de la suscripción puede crear una cuenta de ejecución siguiendo las instrucciones de la sección siguiente.

Para crear una cuenta de ejecución en la cuenta de Azure:

1. En Azure Portal, seleccione **Todos los servicios**. En la lista de recursos, escriba y seleccione **Cuentas de Automation**.

2. En la página **Cuentas de Automation**, seleccione su cuenta.

3. En el panel de la izquierda de la ventana, seleccione **Cuentas de ejecución** en la sección Configuración de la cuenta.

4. Seleccione **Cuenta de ejecución de Azure**. Cuando se muestre el panel **Agregar cuenta de ejecución de Azure**, revise la información general y, después, seleccione **Crear** para iniciar el proceso de creación de la cuenta.

5. Espere unos minutos para que Azure cree la cuenta de ejecución. Puede realizar el seguimiento del progreso de la creación en el menú en Notificaciones.

6. Cuando el proceso finaliza, crea un recurso denominado AzureRunAsConnection en la cuenta de Automation especificada. El recurso de conexión contiene el identificador de la aplicación, el identificador del inquilino, el identificador de la suscripción y la huella digital del certificado. Recuerde el identificador de la aplicación, porque lo necesitará más adelante.

### <a name="create-a-role-assignment-in-windows-virtual-desktop"></a>Creación de una asignación de roles en Windows Virtual Desktop

A continuación, debe crear una asignación de roles para que AzureRunAsConnection pueda interactuar con Windows Virtual Desktop. Asegúrese de usar PowerShell para iniciar sesión con una cuenta que tenga permisos para crear la asignación de roles.

En primer lugar y, si aún no lo ha hecho, descargue e importe el [módulo de PowerShell para Windows Virtual Desktop](/powershell/windows-virtual-desktop/overview/) que se usará en la sesión de PowerShell. Ejecute los siguientes cmdlets de PowerShell para conectarse a Windows Virtual Desktop y mostrar los inquilinos.

```powershell
Add-RdsAccount -DeploymentUrl "https://rdbroker.wvd.microsoft.com"

Get-RdsTenant
```

Cuando encuentre el inquilino con los grupos de hosts que desea escalar, siga las instrucciones de [Escalado dinámico de host de sesiones](#create-an-azure-automation-account) y use el nombre del inquilino que recibió del cmdlet anterior en el siguiente cmdlet para crear la asignación de roles:

```powershell
New-RdsRoleAssignment -RoleDefinitionName "RDS Contributor" -ApplicationId <applicationid> -TenantName <tenantname>
```

## <a name="create-the-azure-logic-app-and-execution-schedule"></a>Creación de la aplicación lógica de Azure y la programación de ejecución

Por último, deberá crear la aplicación lógica de Azure y configurar una programación de ejecución para la nueva herramienta de escalado.

1.  Abra Windows PowerShell como administrador.

2.  Ejecute el siguiente cmdlet para iniciar sesión en la cuenta de Azure.

     ```powershell
     Login-AzAccount
     ```

3. Ejecute el siguiente cmdlet para descargar el archivo de script createazurelogicapp.ps1 en la máquina local.

     ```powershell
     Set-Location -Path "c:\temp"
     $uri = "https://raw.githubusercontent.com/Azure/RDS-Templates/master/wvd-templates/wvd-scaling-script/createazurelogicapp.ps1"
     Invoke-WebRequest -Uri $uri -OutFile ".\createazurelogicapp.ps1"
     ```

4. Ejecute el siguiente cmdlet para iniciar sesión en Windows Virtual Desktop con una cuenta que tenga permisos de propietario de RDS o de colaborador de RDS.

     ```powershell
     Add-RdsAccount -DeploymentUrl "https://rdbroker.wvd.microsoft.com"
     ```

5. Ejecute el siguiente script de PowerShell para crear la aplicación lógica de Azure y la programación de ejecución.

     ```powershell
     $aadTenantId = (Get-AzContext).Tenant.Id
     
     $azureSubscription = Get-AzSubscription | Out-GridView -PassThru -Title "Select your Azure Subscription"
     Select-AzSubscription -Subscription $azureSubscription.Id
     $subscriptionId = $azureSubscription.Id
     
     $resourceGroup = Get-AzResourceGroup | Out-GridView -PassThru -Title "Select the resource group for the new Azure Logic App"
     $resourceGroupName = $resourceGroup.ResourceGroupName
     $location = $resourceGroup.Location
     
     $wvdTenant = Get-RdsTenant | Out-GridView -PassThru -Title "Select your WVD tenant"
     $tenantName = $wvdTenant.TenantName
     
     $wvdHostpool = Get-RdsHostPool -TenantName $wvdTenant.TenantName | Out-GridView -PassThru -Title "Select the host pool you'd like to scale"
     $hostPoolName = $wvdHostpool.HostPoolName
     
     $recurrenceInterval = Read-Host -Prompt "Enter how often you'd like the job to run in minutes, e.g. '15'"
     $beginPeakTime = Read-Host -Prompt "Enter the start time for peak hours in local time, e.g. 9:00"
     $endPeakTime = Read-Host -Prompt "Enter the end time for peak hours in local time, e.g. 18:00"
     $timeDifference = Read-Host -Prompt "Enter the time difference between local time and UTC in hours, e.g. +5:30"
     $sessionThresholdPerCPU = Read-Host -Prompt "Enter the maximum number of sessions per CPU that will be used as a threshold to determine when new session host VMs need to be started during peak hours"
     $minimumNumberOfRdsh = Read-Host -Prompt "Enter the minimum number of session host VMs to keep running during off-peak hours"
     $limitSecondsToForceLogOffUser = Read-Host -Prompt "Enter the number of seconds to wait before automatically signing out users. If set to 0, users will be signed out immediately"
     $logOffMessageTitle = Read-Host -Prompt "Enter the title of the message sent to the user before they are forced to sign out"
     $logOffMessageBody = Read-Host -Prompt "Enter the body of the message sent to the user before they are forced to sign out"
     
     $automationAccount = Get-AzAutomationAccount -ResourceGroupName $resourceGroup.ResourceGroupName | Out-GridView -PassThru
     $automationAccountName = $automationAccount.AutomationAccountName
     $automationAccountConnection = Get-AzAutomationConnection -ResourceGroupName $resourceGroup.ResourceGroupName -AutomationAccountName $automationAccount.AutomationAccountName | Out-GridView -PassThru -Title "Select the Azure RunAs connection asset"
     $connectionAssetName = $automationAccountConnection.Name
     
     $webHookURI = Read-Host -Prompt "Enter the URI of the WebHook returned by when you created the Azure Automation Account"
     $maintenanceTagName = Read-Host -Prompt "Enter the name of the Tag associated with VMs you don't want to be managed by this scaling tool"

     .\createazurelogicapp.ps1 -ResourceGroupName $resourceGroupName `
       -AADTenantID $aadTenantId `
       -SubscriptionID $subscriptionId `
       -TenantName $tenantName `
       -HostPoolName $hostPoolName `
       -RecurrenceInterval $recurrenceInterval `
       -BeginPeakTime $beginPeakTime `
       -EndPeakTime $endPeakTime `
       -TimeDifference $timeDifference `
       -SessionThresholdPerCPU $sessionThresholdPerCPU `
       -MinimumNumberOfRDSH $minimumNumberOfRdsh `
       -LimitSecondsToForceLogOffUser $limitSecondsToForceLogOffUser `
       -LogOffMessageTitle $logOffMessageTitle `
       -LogOffMessageBody $logOffMessageBody `
       -Location $location `
       -ConnectionAssetName $connectionAssetName `
       -WebHookURI $webHookURI `
       -AutomationAccountName $automationAccountName `
       -MaintenanceTagName $maintenanceTagName
     ```

     Después de ejecutar el script, la aplicación lógica debe aparecer en un grupo de recursos, como se muestra en la siguiente imagen.

     ![Imagen de la página de información general para ver una aplicación lógica de Azure de ejemplo.](../media/logic-app.png)

Para realizar cambios en la programación de ejecución, como cambiar el intervalo de periodicidad o la zona horaria, vaya al programador de escalabilidad automática y seleccione **Editar** para ir al diseñador de aplicaciones lógicas.

![Una imagen del diseñador de aplicaciones lógicas. Los menús Periodicidad y Webhook que permiten al usuario editar los tiempos de periodicidad y el archivo de webhook están abiertos.](../media/logic-apps-designer.png)

## <a name="manage-your-scaling-tool"></a>Administración de la herramienta de escalado

Ahora que ha creado la herramienta de escalado, puede acceder a su salida. En esta sección se describen algunas características que pueden resultarle útiles.

### <a name="view-job-status"></a>Ver estado del trabajo

Puede ver un resumen del estado de todos los trabajos del runbook o profundizar en el estado de un trabajo de runbook específico en Azure Portal.

A la derecha de la cuenta de Automation seleccionada, en "Estadísticas del trabajo" puede ver una lista de los resúmenes de todos los trabajos de runbook. Al abrir la página **Trabajos** en el lado izquierdo de la ventana, se muestran los estados de trabajo actuales, las horas de inicio y las horas de finalización.

![Captura de pantalla de la página de estado del trabajo.](../media/jobs-status.png)

### <a name="view-logs-and-scaling-tool-output"></a>Visualización de registros y salida de la herramienta de escalado

Para ver los registros de las operaciones de escalabilidad y reducción horizontales, abra el runbook y seleccione el nombre de su trabajo.

Vaya al runbook (el nombre predeterminado es WVDAutoScaleRunbook) en el grupo de recursos que hospeda la cuenta de Azure Automation y seleccione **Información general**. En la página Información general, seleccione un trabajo en Trabajos recientes para ver la salida de la herramienta de escalado, tal como se muestra en la siguiente imagen.

![Imagen de la ventana de salida de la herramienta de escalado.](../media/tool-output.png)

