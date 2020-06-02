---
title: Administración de recursos locales remotos con funciones de PowerShell
description: Aprenda a configurar Conexiones híbridas en Azure Relay para conectar una aplicación de función de PowerShell a recursos locales, que luego se pueden usar para administrar de forma remota el recurso local.
author: eamono
ms.topic: conceptual
ms.date: 04/26/2020
ms.author: eamono
ms.openlocfilehash: 6034d1327d263eda49881af5eedf94ae06495128
ms.sourcegitcommit: a8ee9717531050115916dfe427f84bd531a92341
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/12/2020
ms.locfileid: "83122279"
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

1. En el menú de Azure Portal o en la página **Principal**, seleccione **Crear un recurso**.

1. En la página **Nuevo**, seleccione **Compute** > **Function App**.

1. En la página **Básico**, utilice la configuración de la aplicación de funciones que se especifica en la tabla siguiente.

    | Configuración      | Valor sugerido  | Descripción |
    | ------------ | ---------------- | ----------- |
    | **Suscripción** | Su suscripción | Suscripción en la que se creará esta nueva aplicación de función. |
    | **[Grupo de recursos](../azure-resource-manager/management/overview.md)** |  *myResourceGroup* | Nombre para el nuevo grupo de recursos en el que se va a crear la Function App. |
    | **Nombre de la aplicación de función** | Nombre único globalmente | Nombre que identifica la nueva Function App. Los caracteres válidos son `a-z` (no distingue mayúsculas de minúsculas), `0-9` y `-`.  |
    |**Publicar**| Código | Opción para publicar archivos de código o un contenedor de Docker. |
    | **Pila en tiempo de ejecución** | Lenguaje preferido | Elija PowerShell Core. |
    |**Versión**| Número de la versión | Elija la versión del entorno de ejecución instalado.  |
    |**Región**| Región preferida | Elija una [región](https://azure.microsoft.com/regions/) cerca de usted o cerca de otros servicios a los que tendrán acceso las funciones. |

    :::image type="content" source="./media/functions-hybrid-powershell/function-app-create-basics.png" alt-text="Cree una aplicación de funciones: conceptos básicos." border="true":::

1. Seleccione **Siguiente: Hospedaje**. En la página **Hospedaje**, escriba la siguiente configuración.

    | Configuración      | Valor sugerido  | Descripción |
    | ------------ | ---------------- | ----------- |
    | **[Cuenta de almacenamiento](../storage/common/storage-account-create.md)** |  Nombre único globalmente |  Cree una cuenta de almacenamiento que use la aplicación de función. Los nombres de las cuentas de almacenamiento deben tener entre 3 y 24 caracteres y solo pueden incluir números y letras en minúscula. También puede usar una cuenta existente que debe cumplir los [requisitos de la cuenta de almacenamiento](../azure-functions/functions-scale.md#storage-account-requirements). |
    |**Sistema operativo**| Sistema operativo preferido | Se preselecciona un sistema operativo en función de la selección de pila en tiempo de ejecución, pero puede cambiar esta configuración si es necesario. |
    | **[Tipo de plan](../azure-functions/functions-scale.md)** | **Plan de App Service** | Elija **Plan de App Service**. Si ejecuta un plan de App Service, debe administrar el [escalado de la aplicación de función](../azure-functions/functions-scale.md).  |

    :::image type="content" source="./media/functions-hybrid-powershell/function-app-create-hosting.png" alt-text="Cree una aplicación de funciones: hospedaje." border="true":::

1. Seleccione **Siguiente: Supervisión**. En la página **Supervisión**, escriba la siguiente configuración.

    | Configuración      | Valor sugerido  | Descripción |
    | ------------ | ---------------- | ----------- |
    | **[Application Insights](../azure-functions/functions-monitoring.md)** | Valor predeterminado | Crea un recurso de Application Insights con el mismo *nombre de aplicación* en la región más cercana que lo admita. Si expande esta configuración o selecciona **Crear nuevo** puede cambiar el nombre de Application Insights o elegir otra región de una [geografía de Azure](https://azure.microsoft.com/global-infrastructure/geographies/) en la que quiera almacenar los datos. |

    :::image type="content" source="./media/functions-hybrid-powershell/function-app-create-monitoring.png" alt-text="Cree una aplicación de funciones: supervisión." border="true":::

1. Seleccione **Revisar y crear** para revisar las selecciones de configuración de la aplicación.

1. En la página **Revisar y crear**, revise la configuración y, a continuación, seleccione **Crear** para aprovisionar e implementar la aplicación de función.

1. Seleccione el icono **Notificaciones** de la esquina superior derecha del portal y observe el mensaje **Implementación correcta**.

1. Seleccione **Ir al recurso** para ver la nueva aplicación de función. También puede seleccionar **Anclar al panel**. Dicho anclaje facilita la vuelta a este recurso de aplicación de función desde el panel.

## <a name="create-a-hybrid-connection-for-the-function-app"></a>Creación de una conexión híbrida para la aplicación de función

Las conexiones híbridas se configuran en la sección de redes de la aplicación de funciones:

1. En **configuración** en la aplicación de funciones que acaba de crear, seleccione **Redes**. 
1. Seleccione **Configure your hybrid connections endpoints** (Configurar los puntos de conexión híbridos).
   
    :::image type="content" source="./media/functions-hybrid-powershell/configure-hybrid-connection-endpoint.png" alt-text="Configure los puntos de conexión de la conexión híbrida." border="true":::

1. Seleccione **Agregar conexión híbrida**.
   
    :::image type="content" source="./media/functions-hybrid-powershell/hybrid-connection-overview.png" alt-text="Agregue una conexión híbrida." border="true":::

1. Escriba la información sobre la conexión híbrida, tal como se muestra justo después de la siguiente captura de pantalla. Tiene la opción de hacer coincidir el valor de **Host de punto de conexión** con el nombre de host del servidor local para que sea más fácil recordar el servidor más tarde cuando se ejecuten comandos remotos. El puerto coincide con el puerto del servicio de administración remota de Windows predeterminado definido en el servidor anterior.
  
      :::image type="content" source="./media/functions-hybrid-powershell/add-hybrid-connection.png" alt-text="Agregue una conexión híbrida." border="true":::

    | Configuración      | Valor sugerido  |
    | ------------ | ---------------- |
    | **Nombre de la conexión híbrida** | ContosoHybridOnPremisesServer |
    | **Host del punto de conexión** | finance1 |
    | **Puerto de punto de conexión** | 5986 |
    | **Espacio de nombres de ServiceBus** | Crear nuevo |
    | **Ubicación** | Seleccione una ubicación disponible |
    | **Nombre** | contosopowershellhybrid | 

1. Haga clic en **Aceptar** para crear la conexión híbrida.

## <a name="download-and-install-the-hybrid-connection"></a>Descarga e instalación de la conexión híbrida

1. Seleccione **Descargar administrador de conexión** para guardar el archivo *.msi* localmente en el equipo.

    :::image type="content" source="./media/functions-hybrid-powershell/download-hybrid-connection-installer.png" alt-text="Descargue el instalador." border="true":::

1. Copie el archivo *.msi* del equipo local en el servidor local.
1. Ejecute el instalador del Administrador de conexiones híbridas para instalar el servicio en el servidor local.

    :::image type="content" source="./media/functions-hybrid-powershell/hybrid-installation.png" alt-text="Instale la conexión híbrida." border="true":::

1. En el portal, abra la conexión híbrida y copie la cadena de conexión de puerta de enlace en el Portapapeles.

    :::image type="content" source="./media/functions-hybrid-powershell/copy-hybrid-connection.png" alt-text="Copie la cadena de conexión híbrida." border="true":::

1. Abra la interfaz de usuario del Administrador de conexiones híbridas en el servidor local.

    :::image type="content" source="./media/functions-hybrid-powershell/hybrid-connection-ui.png" alt-text="Abra la interfaz de usuario de conexiones híbridas." border="true":::

1. Seleccione **Indicar manualmente** y pegue la cadena de conexión del portapapeles.

    :::image type="content" source="./media/functions-hybrid-powershell/enter-manual-connection.png" alt-text="Pegue la conexión híbrida." border="true":::

1. Reinicie el Administrador de conexiones híbridas desde PowerShell si no aparece como conectado.
    ```powershell
    Restart-Service HybridConnectionManager
    ```

## <a name="create-an-app-setting-for-the-password-of-an-administrator-account"></a>Creación de una configuración de aplicación para la contraseña de una cuenta de administrador

1. En **Configuración** de la aplicación de funciones, seleccione **Configuración**. 
1. Seleccione **+ Nueva configuración de la aplicación**.

    :::image type="content" source="./media/functions-hybrid-powershell/select-configuration.png" alt-text="Configure una contraseña para la cuenta de administrador." border="true":::

1. Asigne a la configuración el nombre **ContosoUserPassword** y escriba la contraseña. Seleccione **Aceptar**.
1. Seleccione **Guardar** para almacenar la contraseña en la aplicación de funciones.

    :::image type="content" source="./media/functions-hybrid-powershell/save-administrator-password.png" alt-text="Guarde la contraseña para la cuenta de administrador." border="true":::

## <a name="create-a-function-http-trigger"></a>Creación de un desencadenador HTTP de función

1. En la aplicación de funciones, seleccione **Funciones** y, a continuación, seleccione **+ Agregar**.

    :::image type="content" source="./media/functions-hybrid-powershell/create-http-trigger-function.png" alt-text="Cree un desencadenador HTTP." border="true":::

1. Seleccione la plantilla del **desencadenador HTTP**.

    :::image type="content" source="./media/functions-hybrid-powershell/select-http-trigger-template.png" alt-text="Seleccione la plantilla del desencadenador HTTP" border="true":::.

1. Asigne un nombre a la nueva función y seleccione **Crear función**.

    :::image type="content" source="./media/functions-hybrid-powershell/create-new-http-function.png" alt-text="Asigne un nombre a la nueva función de desencadenador HTTP y créela." border="true":::

## <a name="test-the-function"></a>Prueba de la función

1. En la nueva función, seleccione **Código y prueba**. Reemplace el código de PowerShell de la plantilla por el código siguiente:

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

1. Seleccione **Guardar**.

    :::image type="content" source="./media/functions-hybrid-powershell/save-http-function.png" alt-text="Cambie el código de PowerShell y guarde la función de desencadenador HTTP." border="true":::

 1. Seleccione **Prueba** y, a continuación, seleccione **Ejecutar** para probar la función. Revise los registros para comprobar que la prueba se realizó correctamente.

     :::image type="content" source="./media/functions-hybrid-powershell/test-function-hybrid.png" alt-text="Pruebe la función de desencadenador HTTP." border="true":::

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
