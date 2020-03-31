---
title: Administración de recursos locales remotos con funciones de PowerShell
description: Aprenda a configurar Conexiones híbridas en Azure Relay para conectar una aplicación de función de PowerShell a recursos locales, que luego se pueden usar para administrar de forma remota el recurso local.
author: eamono
ms.topic: conceptual
ms.date: 9/5/2019
ms.author: eamono
ms.openlocfilehash: 36fc4c873dccfe9fa814bddccd829ed04207f095
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "74226932"
---
# <a name="managing-hybrid-environments-with-powershell-in-azure-functions-and-app-service-hybrid-connections"></a>Administración de entornos híbridos con PowerShell en Azure Functions y conexiones híbridas de App Service

La característica Conexiones híbridas de Azure App Service permite el acceso a los recursos de otras redes. Para obtener más información sobre esta funcionalidad, consulte la documentación de [Conexiones híbridas](../app-service/app-service-hybrid-connections.md). En este artículo se describe cómo usar esta funcionalidad para ejecutar funciones de PowerShell dirigidas a un servidor local. Este servidor se puede usar luego para administrar todos los recursos dentro del entorno local con una función de Azure PowerShell.


## <a name="configure-an-on-premises-server-for-powershell-remoting"></a>Configuración de un servidor local para la comunicación remota de PowerShell

El siguiente script habilita la comunicación remota de PowerShell y crea una regla de firewall y un cliente de escucha https de WinRM. Con fines de prueba, se usa un certificado autofirmado. En un entorno de producción, se recomienda usar un certificado firmado.

```powershell
# For configuration of WinRM, see
# https://docs.microsoft.com/windows/win32/winrm/installation-and-configuration-for-windows-remote-management.

# Enable PowerShell remoting.
Enable-PSRemoting -Force

# Create firewall rule for WinRM. The default HTTPS port is 5986.
New-NetFirewallRule -Name "WinRM HTTPS" `
                    -DisplayName "WinRM HTTPS" `
                    -Enabled True `
                    -Profile "Any" `
                    -Action "Allow" `
                    -Direction "Inbound" `
                    -LocalPort 5986 `
                    -Protocol "TCP"

# Create new self-signed-certificate to be used by WinRM.
$Thumbprint = (New-SelfSignedCertificate -DnsName $env:COMPUTERNAME  -CertStoreLocation Cert:\LocalMachine\My).Thumbprint

# Create WinRM HTTPS listener.
$Cmd = "winrm create winrm/config/Listener?Address=*+Transport=HTTPS @{Hostname=""$env:COMPUTERNAME ""; CertificateThumbprint=""$Thumbprint""}"
cmd.exe /C $Cmd
```

## <a name="create-a-powershell-function-app-in-the-portal"></a>Creación de una aplicación de función de PowerShell en el portal

La característica Conexiones híbridas de App Service solo está disponible en los planes de tarifa Básico, Estándar y Aislado. Al crear la aplicación de funciones con PowerShell, cree o seleccione uno de estos planes.

1. En [Azure Portal](https://portal.azure.com), seleccione **+ Crear un recurso** en el menú izquierdo y, a continuación, seleccione **Function App**.

1. En **Plan de hospedaje**, elija **Plan de App Service** y, a continuación, seleccione **Plan de App Service/Ubicación**.

1. Seleccione **Crear nuevo**, escriba un nombre de **Plan de App Service**, elija una **Ubicación** en una [región](https://azure.microsoft.com/regions/) cerca de usted o cerca de otros servicios a los que accedan sus funciones y, a continuación, seleccione **Plan de tarifa**.

1. Elija el plan estándar S1 y, luego, seleccione **Aplicar**.

1. Seleccione **Aceptar** para crear el plan y, a continuación, configure la configuración restante de **Function App** tal como se especifica en la tabla que viene inmediatamente después de la siguiente captura de pantalla:

    ![Aplicación de función de PowerShell Core](./media/functions-hybrid-powershell/create-function-powershell-app.png)  

    | Configuración      | Valor sugerido  | Descripción                                        |
    | ------------ |  ------- | -------------------------------------------------- |
    | **Nombre de la aplicación** | Nombre único globalmente | Nombre que identifica la nueva Function App. Los caracteres válidos son `a-z`, `0-9` y `-`.  | 
    | **Suscripción** | Su suscripción | Suscripción en la que se creará esta nueva aplicación de función. |
    | **Grupo de recursos** |  myResourceGroup | Nombre para el nuevo grupo de recursos en el que se va a crear la Function App. También puede usar el valor sugerido. |
    | **SISTEMA OPERATIVO** | Sistema operativo preferido | Seleccione Windows. |
    | **Pila en tiempo de ejecución** | Lenguaje preferido | Elija PowerShell Core. |
    | **Storage** |  Nombre único globalmente |  Cree una cuenta de almacenamiento que use la aplicación de función. Los nombres de las cuentas de almacenamiento deben tener entre 3 y 24 caracteres y solo pueden incluir números y letras en minúscula. También puede usar una cuenta existente.
    | **Application Insights** | Valor predeterminado | Crea un recurso de Application Insights con el mismo *nombre de aplicación* en la región más cercana que lo admita. Si expande esta configuración, puede cambiar el valor de **Nuevo nombre de recurso**  o elegir otro valor en **Ubicación** en la región [geográfica de Azure](https://azure.microsoft.com/global-infrastructure/geographies/) donde desee almacenar los datos. |

1. Una vez validada la configuración, seleccione **Crear**.

1. Seleccione el icono de **Notificación** de la esquina superior derecha del portal y observe el mensaje "Implementación correcta".

1. Seleccione **Ir al recurso** para ver la nueva aplicación de función. También puede seleccionar **Anclar al panel**. Dicho anclaje facilita la vuelta a este recurso de aplicación de función desde el panel.

## <a name="create-a-hybrid-connection-for-the-function-app"></a>Creación de una conexión híbrida para la aplicación de función

Las conexiones híbridas se configuran en la sección de redes de la aplicación de funciones:

1. Seleccione la pestaña **Características de la plataforma** de la aplicación de funciones y, luego, elija **Redes**. 
   ![Información general de la aplicación para redes de plataforma](./media/functions-hybrid-powershell/app-overview-platform-networking.png)  
1. Seleccione **Configure your hybrid connections endpoints** (Configurar los puntos de conexión híbridos).
   ![Redes](./media/functions-hybrid-powershell/select-network-feature.png)  
1. Seleccione **Agregar conexión híbrida**.
   ![Conexión híbrida](./media/functions-hybrid-powershell/hybrid-connection-overview.png)  
1. Escriba la información sobre la conexión híbrida, tal como se muestra justo después de la siguiente captura de pantalla. Tiene la opción de hacer coincidir el valor de **Host de punto de conexión** con el nombre de host del servidor local para que sea más fácil recordar el servidor más tarde cuando se ejecuten comandos remotos. El puerto coincide con el puerto del servicio de administración remota de Windows predeterminado definido en el servidor anterior.
  ![Agregar conexión híbrida](./media/functions-hybrid-powershell/add-hybrid-connection.png)  

    **Nombre de la conexión híbrida**: ContosoHybridOnPremisesServer
    
    **Host de punto de conexión**: finance1
    
    **Puerto de punto de conexión**: 5986
    
    **Espacio de nombres de Service Bus**: Crear nuevo
    
    **Ubicación**: Seleccione una ubicación disponible
    
    **Nombre**: contosopowershellhybrid

5. Haga clic en **Aceptar** para crear la conexión híbrida.

## <a name="download-and-install-the-hybrid-connection"></a>Descarga e instalación de la conexión híbrida

1. Seleccione **Descargar administrador de conexión** para guardar el archivo. msi localmente en el equipo.
![Descargar instalador](./media/functions-hybrid-powershell/download-hybrid-connection-installer.png)  
1. Copie el archivo .msi del equipo local en el servidor local.
1. Ejecute el instalador del Administrador de conexiones híbridas para instalar el servicio en el servidor local.
![Instalar conexión híbrida](./media/functions-hybrid-powershell/hybrid-installation.png)  
1. En el portal, abra la conexión híbrida y copie la cadena de conexión de puerta de enlace en el Portapapeles.
![Copiar cadena de conexión híbrida](./media/functions-hybrid-powershell/copy-hybrid-connection.png)  
1. Abra la interfaz de usuario del Administrador de conexiones híbridas en el servidor local.
![Abrir la interfaz de usuario de conexiones híbridas](./media/functions-hybrid-powershell/hybrid-connection-ui.png)  
1. Seleccione el botón **Indicar manualmente** y pegue la cadena de conexión del Portapapeles.
![Pegar la conexión](./media/functions-hybrid-powershell/enter-manual-connection.png)  
1. Reinicie el Administrador de conexiones híbridas desde PowerShell si no aparece como conectado.
    ```powershell
    Restart-Service HybridConnectionManager
    ```

## <a name="create-an-app-setting-for-the-password-of-an-administrator-account"></a>Creación de una configuración de aplicación para la contraseña de una cuenta de administrador

1. Seleccione la pestaña **Características de la plataforma** de la aplicación de funciones.
1. En **Configuración general**, seleccione **Configuración**.
![Seleccionar la configuración de la plataforma](./media/functions-hybrid-powershell/select-configuration.png)  
1. Expanda **Nueva configuración de la aplicación** para crear una configuración para la contraseña.
1. Asigne a la configuración el nombre _ContosoUserPassword_ y escriba la contraseña.
1. Seleccione **Aceptar** y, luego, Guardar para almacenar la contraseña en la aplicación de funciones.
![Agregar configuración de aplicación para la contraseña](./media/functions-hybrid-powershell/add-appsetting-password.png)  

## <a name="create-a-function-http-trigger-to-test"></a>Creación de un desencadenador http de función para prueba

1. Cree una función de desencadenador http desde la aplicación de funciones.
![Crear un desencadenador http](./media/functions-hybrid-powershell/create-http-trigger-function.png)  
1. Reemplace el código de PowerShell de la plantilla por el código siguiente:

    ```powershell
    # Input bindings are passed in via param block.
    param($Request, $TriggerMetadata)
    
    # Write to the Azure Functions log stream.
    Write-Output "PowerShell HTTP trigger function processed a request."
    
    # Note that ContosoUserPassword is a function app setting, so I can access it as $env:ContosoUserPassword.
    $UserName = "ContosoUser"
    $securedPassword = ConvertTo-SecureString  $Env:ContosoUserPassword -AsPlainText -Force
    $Credential = [System.management.automation.pscredential]::new($UserName, $SecuredPassword)
    
    # This is the name of the hybrid connection Endpoint.
    $HybridEndpoint = "finance1"
    
    $Script = {
        Param(
            [Parameter(Mandatory=$True)]
            [String] $Service
        )
        Get-Service $Service
    }
    
    Write-Output "Scenario 1: Running command via Invoke-Command"
    Invoke-Command -ComputerName $HybridEndpoint `
                   -Credential $Credential `
                   -Port 5986 `
                   -UseSSL `
                   -ScriptBlock $Script `
                   -ArgumentList "*" `
                   -SessionOption (New-PSSessionOption -SkipCACheck)
    ```

3. Seleccione **Guardar** y **Ejecutar** para probar la función.
![Probar la aplicación de funciones](./media/functions-hybrid-powershell/test-function-hybrid.png)  

## <a name="managing-other-systems-on-premises"></a>Administración de otros sistemas locales

Puede usar el servidor local conectado para conectarse a otros servidores y sistemas de administración del entorno local. Esto permite administrar las operaciones del centro de datos desde Azure mediante las funciones de PowerShell. El script siguiente registra una sesión de configuración de PowerShell que se ejecuta con las credenciales proporcionadas. Estas credenciales deben ser de administrador en los servidores remotos. Luego, puede usar esta configuración para acceder a otros puntos de conexión en el servidor local o el centro de datos.

```powershell
# Input bindings are passed in via param block.
param($Request, $TriggerMetadata)

# Write to the Azure Functions log stream.
Write-Host "PowerShell HTTP trigger function processed a request."

# Note that ContosoUserPassword is a function app setting, so I can access it as $env:ContosoUserPassword.
$UserName = "ContosoUser"
$SecuredPassword = ConvertTo-SecureString  $Env:ContosoUserPassword -AsPlainText -Force
$Credential = [System.management.automation.pscredential]::new($UserName, $SecuredPassword)

# This is the name of the hybrid connection Endpoint.
$HybridEndpoint = "finance1"

# The remote server that will be connected to run remote PowerShell commands on
$RemoteServer = "finance2".

Write-Output "Use hybrid connection server as a jump box to connect to a remote machine"

# We are registering an endpoint that runs under credentials ($Credential) that has access to the remote server.
$SessionName = "HybridSession"
$ScriptCommand = {
    param (
        [Parameter(Mandatory=$True)]
        $SessionName)

    if (-not (Get-PSSessionConfiguration -Name $SessionName -ErrorAction SilentlyContinue))
    {
        Register-PSSessionConfiguration -Name $SessionName -RunAsCredential $Using:Credential
    }
}

Write-Output "Registering session on hybrid connection jumpbox"
Invoke-Command -ComputerName $HybridEndpoint `
               -Credential $Credential `
               -Port 5986 `
               -UseSSL `
               -ScriptBlock $ScriptCommand `
               -ArgumentList $SessionName `
               -SessionOption (New-PSSessionOption -SkipCACheck)

# Script to run on the jump box to run against the second machine.
$RemoteScriptCommand = {
    param (
        [Parameter(Mandatory=$True)]
        $ComputerName)
        # Write out the hostname of the hybrid connection server.
        hostname
        # Write out the hostname of the remote server.
        Invoke-Command -ComputerName $ComputerName -Credential $Using:Credential -ScriptBlock {hostname} `
                        -UseSSL -Port 5986 -SessionOption (New-PSSessionOption -SkipCACheck)
}

Write-Output "Running command against remote machine via jumpbox by connecting to the PowerShell configuration session"
Invoke-Command -ComputerName $HybridEndpoint `
               -Credential $Credential `
               -Port 5986 `
               -UseSSL `
               -ScriptBlock $RemoteScriptCommand `
               -ArgumentList $RemoteServer `
               -SessionOption (New-PSSessionOption -SkipCACheck) `
               -ConfigurationName $SessionName
```

Reemplace las siguientes variables de este script con los valores aplicables de su entorno:
* $HybridEndpoint
* $RemoteServer

En los dos escenarios anteriores, puede conectarse a sus entornos locales y administrarlos con PowerShell en Azure Functions y Conexiones híbridas. Le recomendamos que obtenga más información sobre [Conexiones híbridas](../app-service/app-service-hybrid-connections.md) y [PowerShell en Functions](./functions-reference-powershell.md).

También puede usar las [redes virtuales](./functions-create-vnet.md) de Azure para conectarse a su entorno local mediante Azure Functions.

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"] 
> [Más información sobre cómo trabajar con funciones de PowerShell](functions-reference-powershell.md)
