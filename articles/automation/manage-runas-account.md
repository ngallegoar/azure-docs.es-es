---
title: Administración de cuentas de ejecución de Azure Automation
description: En este artículo se describe cómo administrar las cuenta de ejecución con PowerShell o desde el portal.
services: automation
ms.subservice: shared-capabilities
ms.date: 05/24/2019
ms.topic: conceptual
ms.openlocfilehash: 8d7d0baacd5f702e8f435ab440eaf0338a60f4cb
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "79500778"
---
# <a name="manage-azure-automation-run-as-accounts"></a>Administración de cuentas de ejecución de Azure Automation

Las cuentas de ejecución de Azure Automation proporcionan autenticación para administrar los recursos de Azure con los cmdlets de Azure. Cuando crea una cuenta de ejecución, se crea un nuevo usuario de la entidad de servicio en Azure Active Directory (AD) y se asigna el rol Colaborador a este usuario en el nivel de suscripción. Para los runbooks que utilizan instancias de Hybrid Runbook Worker en máquinas virtuales de Azure, puede utilizar [Managed Identities for Azure Resources](automation-hrw-run-runbooks.md#managed-identities-for-azure-resources) en lugar de cuentas de ejecución para autenticarse en sus recursos de Azure.

La entidad de servicio para una cuenta de ejecución no tiene los permisos para leer Azure Active Directory de manera predeterminada. Si quiere agregar permisos para leer o administrar Azure AD, deberá conceder los permisos a la entidad de servicio en **Permisos de API**. Para más información, consulte [Adición de permisos para acceder a las API web](../active-directory/develop/quickstart-configure-app-access-web-apis.md#add-permissions-to-access-web-apis).

>[!NOTE]
>Este artículo se ha actualizado para usar el nuevo módulo Az de Azure PowerShell. Aún puede usar el módulo de AzureRM que continuará recibiendo correcciones de errores hasta diciembre de 2020 como mínimo. Para más información acerca del nuevo módulo Az y la compatibilidad con AzureRM, consulte [Introducing the new Azure PowerShell Az module](https://docs.microsoft.com/powershell/azure/new-azureps-module-az?view=azps-3.5.0) (Presentación del nuevo módulo Az de Azure PowerShell). Para obtener instrucciones sobre la instalación del módulo Az en Hybrid Runbook Worker, consulte [Instalación del módulo de Azure PowerShell](https://docs.microsoft.com/powershell/azure/install-az-ps?view=azps-3.5.0). Puede actualizar los módulos de su cuenta de Automation a la versión más reciente mediante [Actualización de módulos de Azure PowerShell en Azure Automation](automation-update-azure-modules.md).

## <a name="types-of-run-as-accounts"></a>Tipos de cuentas de ejecución

Azure Automation usa dos tipos de cuentas de ejecución:

* Cuenta de ejecución de Azure
* Cuenta de ejecución de Azure clásico

>[!NOTE]
>Las suscripciones del Proveedor de soluciones en la nube (CSP) de Azure solo admiten el modelo de Azure Resource Manager. Los servicios que no son de Azure Resource Manager no están disponibles en el programa. Cuando se usa una suscripción al programa CSP, no se crea la cuenta de ejecución de Azure clásico, sino la cuenta de ejecución de Azure. Para más información acerca de las suscripciones de CSP, consulte [Servicios disponibles en las suscripciones de CSP](https://docs.microsoft.com/azure/cloud-solution-provider/overview/azure-csp-available-services).

### <a name="run-as-account"></a>cuenta de identificación

La cuenta de ejecución administra los recursos del [modelo de implementación de Resource Manager](../azure-resource-manager/management/deployment-models.md). Realiza las tareas que se indican a continuación.

* Crea una aplicación de Azure AD con un certificado autofirmado, crea una cuenta de entidad servicio para la aplicación en Azure AD y asigna el rol Colaborador para esta cuenta en la suscripción actual. Puede cambiar la configuración de certificado a Propietario o a cualquier otro rol. Para más información, consulte [Control de acceso basado en rol en Azure Automation](automation-role-based-access-control.md).
  
* Crea un recurso de certificado de Automation denominado `AzureRunAsCertificate` en la cuenta de Automation especificada. El recurso de certificado contiene la clave privada del certificado que se usa en la aplicación de Azure AD.
  
* Crea un recurso de conexión de Automation denominado `AzureRunAsConnection` en la cuenta de Automation especificada. El recurso de conexión contiene el identificador de la aplicación, el identificador del inquilino, el identificador de la suscripción y la huella digital del certificado.

### <a name="azure-classic-run-as-account"></a>Cuenta de ejecución de Azure clásico

La cuenta de ejecución de Azure clásico administra recursos del [modelo de implementación clásico](../azure-resource-manager/management/deployment-models.md). Debe ser coadministrador de la suscripción para crear o renovar este tipo de cuenta.

La cuenta de ejecución de Azure clásico realiza las siguientes tareas.

  * Crea un certificado de administración en la suscripción.

  * Crea un recurso de certificado de Automation denominado `AzureClassicRunAsCertificate` en la cuenta de Automation especificada. El recurso de certificado contiene la clave privada del certificado que usa el certificado de administración.

  * Crea un recurso de conexión de Automation denominado `AzureClassicRunAsConnection` en la cuenta de Automation especificada. El recurso de conexión contiene el nombre de la suscripción, el id. de suscripción y el nombre del recurso de certificado.

## <a name="run-as-account-permissions"></a><a name="permissions"></a>Permisos de las cuentas de ejecución

En esta sección se definen los permisos para las cuentas de ejecución normales y las cuentas de ejecución clásicas.

### <a name="permissions-to-configure-run-as-accounts"></a>Permisos para configurar cuentas de ejecución

Para crear o actualizar una cuenta de ejecución, debe tener los permisos y privilegios específicos. Un administrador de aplicaciones en Azure Active Directory y un propietario en una suscripción pueden completar todas las tareas. En una situación en la que tenga separación de funciones, la siguiente tabla muestra una lista de las tareas, el cmdlet equivalente y los permisos necesarios:

|Tarea|Cmdlet  |Permisos mínimos  |Donde se establecen los permisos|
|---|---------|---------|---|
|Crear una aplicación de Azure AD|[New-AzADApplication](https://docs.microsoft.com/powershell/module/az.resources/new-azadapplication?view=azps-3.5.0)     | Rol de desarrollador de aplicaciones<sup>1</sup>        |[Azure AD](../active-directory/develop/howto-create-service-principal-portal.md#required-permissions)</br>Inicio > Azure AD > Registros de aplicaciones |
|Agregar una credencial a la aplicación.|[New-AzADAppCredential](https://docs.microsoft.com/powershell/module/az.resources/new-azadappcredential?view=azps-3.5.0)     | Administrador de la aplicación o administrador global<sup>1</sup>         |[Azure AD](../active-directory/develop/howto-create-service-principal-portal.md#required-permissions)</br>Inicio > Azure AD > Registros de aplicaciones|
|Crear y obtener una entidad de servicio de Azure AD|[New-AzADServicePrincipal](https://docs.microsoft.com/powershell/module/az.resources/new-azadserviceprincipal?view=azps-3.5.0)</br>[Get-AzADServicePrincipal](https://docs.microsoft.com/powershell/module/az.resources/get-azadserviceprincipal?view=azps-3.5.0)     | Administrador de la aplicación o administrador global<sup>1</sup>        |[Azure AD](../active-directory/develop/howto-create-service-principal-portal.md#required-permissions)</br>Inicio > Azure AD > Registros de aplicaciones|
|Asignar u obtener el rol de RBAC para la entidad de seguridad especificada|[New-AzRoleAssignment](https://docs.microsoft.com/powershell/module/az.resources/new-azroleassignment?view=azps-3.5.0)</br>[Get-AzRoleAssignment](https://docs.microsoft.com/powershell/module/Az.Resources/Get-AzRoleAssignment?view=azps-3.5.0)      | Administrador de acceso de usuario o Propietario, o bien tener los permisos siguientes:</br></br><code>Microsoft.Authorization/Operations/read</br>Microsoft.Authorization/permissions/read</br>Microsoft.Authorization/roleDefinitions/read</br>Microsoft.Authorization/roleAssignments/write</br>Microsoft.Authorization/roleAssignments/read</br>Microsoft.Authorization/roleAssignments/delete</code></br></br> | [Suscripción](../role-based-access-control/role-assignments-portal.md)</br>Inicio > Suscripciones > \<nombre de la suscripción\> -Control de acceso (IAM)|
|Crear o quitar un certificado de Automation|[New-AzAutomationCertificate](https://docs.microsoft.com/powershell/module/Az.Automation/New-AzAutomationCertificate?view=azps-3.5.0)</br>[Remove-AzAutomationCertificate](https://docs.microsoft.com/powershell/module/az.automation/remove-azautomationcertificate?view=azps-3.5.0)     | Colaborador en el grupo de recursos         |Grupo de recursos de la cuenta de Automation|
|Crear o quitar una conexión de Automation|[New-AzAutomationConnection](https://docs.microsoft.com/powershell/module/az.automation/new-azautomationconnection?view=azps-3.5.0)</br>[Remove-AzAutomationConnection](https://docs.microsoft.com/powershell/module/az.automation/remove-azautomationconnection?view=azps-3.5.0)|Colaborador en el grupo de recursos |Grupo de recursos de la cuenta de Automation|

<sup>1</sup> Los usuarios que no son administradores en el inquilino de Azure AD pueden [registrar aplicaciones de AD](../active-directory/develop/howto-create-service-principal-portal.md#required-permissions) si la opción **Los usuarios pueden registrar aplicaciones** del inquilino de Azure AD en la página Configuración de usuario está establecida en **Sí**. Si el registro de la aplicación está configurado en **No**, el usuario que realiza esta acción debe ser como se define en esta tabla.

Si no es miembro de la instancia de Active Directory de la suscripción antes de que se le agregue al rol Administrador global de la suscripción, se le agregará como invitado. En este caso, recibirá una advertencia `You do not have permissions to create…` en la página Agregar cuenta de Automation. 

Si es miembro de la instancia de Active Directory de la suscripción cuando se asigna el rol Administrador global, también puede recibir una advertencia `You do not have permissions to create…` en la página Agregar cuenta de Automation. En este caso, puede solicitar la eliminación de la instancia de Active Directory de la suscripción y, a continuación, solicitar que se vuelva a agregar, a fin de convertirse en usuario completo de Active Directory.

Para comprobar que se ha corregido la situación causante del mensaje de error:

1. En el panel de Azure Active Directory de Azure Portal, seleccione **Usuarios y grupos**. 
2. Seleccione **Todos los usuarios**.
3. Elija su nombre y, a continuación, seleccione **Perfil**. 
4. Asegúrese de que el valor del atributo **Tipo de usuario**  en el perfil de usuario no esté establecido en **Invitado**.

### <a name="permissions-to-configure-classic-run-as-accounts"></a><a name="permissions-classic"></a>Permisos para configurar cuentas de ejecución clásicas

Para configurar o renovar las cuentas de ejecución clásicas, debe tener el rol Coadministrador en el nivel de suscripción. Para más información sobre los permisos de suscripción clásicos, consulte [Administradores de la suscripción clásica de Azure](../role-based-access-control/classic-administrators.md#add-a-co-administrator).

## <a name="creating-a-run-as-account-in-azure-portal"></a>Creación de una cuenta de ejecución en Azure Portal

Realice los pasos que se describen a continuación para actualizar su cuenta de Azure Automation en Azure Portal. Cree las cuentas de ejecución y de ejecución clásica de forma individual. Si no es necesario administrar los recursos clásicos, basta con crear la cuenta de ejecución de Azure.

1. Inicie sesión en el Portal de Azure con una cuenta que sea miembro del rol Administradores de suscripciones y coadministrador de la suscripción.
2. Busque y seleccione **Cuentas de Automation**.
3. En la página Cuentas de Automation, seleccione su cuenta de Automation en la lista.
4. En el panel izquierdo, seleccione **Cuentas de ejecución** en la sección Configuración de la cuenta.
5. En función de la cuenta que necesite, seleccione **Cuenta de ejecución de Azure** o **Cuenta de ejecución de Azure clásica**. 
6. En función de la cuenta que le interese, use el panel **Agregar cuenta de ejecución de Azure** o **Agregar cuenta de ejecución de Azure clásico**. Después de revisar la información general, haga clic en **Crear**.
6. Mientras Azure crea la cuenta de ejecución, se puede seguir el progreso en **Notificaciones** en el menú. También se muestra un banner que indica que se está creando la cuenta. El proceso puede tardar unos minutos en completarse.

## <a name="creating-a-run-as-account-using-powershell"></a>Creación de una cuenta de ejecución con PowerShell

En la lista siguiente se proporcionan los requisitos para crear una cuenta de ejecución en PowerShell. Estos requisitos se aplican a ambos tipos de cuentas de ejecución.

* Windows 10 o Windows Server 2016 con módulos de Azure Resource Manager 3.4.1 y versiones posteriores. El script de PowerShell no admite versiones anteriores de Windows.
* Azure PowerShell 1.0 y versiones superiores. Para más información sobre la versión 1.0 de PowerShell, consulte [Instalación y configuración de Azure PowerShell](/powershell/azureps-cmdlets-docs).
* Una cuenta de Automation, a la que se hace referencia como el valor de los parámetros `AutomationAccountName` y `ApplicationDisplayName`.
* Permisos equivalentes a los que se muestran en [Permisos para configurar cuentas de ejecución](#permissions).

Para obtener los valores de `SubscriptionId`, `ResourceGroupName` y , que son los parámetros necesarios para el script de PowerShell, complete los pasos siguientes.

1. En Azure Portal, seleccione **Cuentas de Automation**.
1. En la página Cuentas de Automation, seleccione su cuenta de Automation.
1. En la sección Configuración de la cuenta, seleccione **Propiedades**.
1. Anote los valores de **ID. DE SUSCRIPCIÓN**, **NOMBRE** y **GRUPO DE RECURSOS** en la página Propiedades. Estos valores corresponden a los valores de los parámetros de script de PowerShell `AutomationAccountName`, `SubscriptionId` y `ResourceGroupName`, respectivamente.

   ![Página de propiedades de la cuenta de Automation](media/manage-runas-account/automation-account-properties.png)

### <a name="powershell-script-to-create-a-run-as-account"></a>Script de PowerShell para crear una cuenta de ejecución

En esta sección se proporciona un script de PowerShell para crear una cuenta de ejecución. El script incluye compatibilidad con varias configuraciones.

* Creación de una cuenta de ejecución mediante un certificado autofirmado.
* Creación de una cuenta de ejecución y una cuenta de ejecución clásica mediante un certificado autofirmado.
* Cree una cuenta de ejecución y una cuenta de ejecución clásica mediante un certificado emitido por su entidad de certificación (CA) empresarial.
* Creación de una cuenta de ejecución y una cuenta de ejecución clásica mediante un certificado autofirmado en la nube de Azure Government.

El script usa varios cmdlets de Azure Resource Manager para crear recursos. Para los cmdlets y los permisos que necesitan, consulte [Permisos para configurar cuentas de ejecución](#permissions-to-configure-run-as-accounts).

Guarde el script en el equipo con el nombre de archivo **New-RunAsAccount.ps1**.

```powershell
    #Requires -RunAsAdministrator
    Param (
        [Parameter(Mandatory = $true)]
        [String] $ResourceGroup,

        [Parameter(Mandatory = $true)]
        [String] $AutomationAccountName,

        [Parameter(Mandatory = $true)]
        [String] $ApplicationDisplayName,

        [Parameter(Mandatory = $true)]
        [String] $SubscriptionId,

        [Parameter(Mandatory = $true)]
        [Boolean] $CreateClassicRunAsAccount,

        [Parameter(Mandatory = $true)]
        [String] $SelfSignedCertPlainPassword,

        [Parameter(Mandatory = $false)]
        [string] $EnterpriseCertPathForRunAsAccount,

        [Parameter(Mandatory = $false)]
        [String] $EnterpriseCertPlainPasswordForRunAsAccount,

        [Parameter(Mandatory = $false)]
        [String] $EnterpriseCertPathForClassicRunAsAccount,

        [Parameter(Mandatory = $false)]
        [String] $EnterpriseCertPlainPasswordForClassicRunAsAccount,

        [Parameter(Mandatory = $false)]
        [ValidateSet("AzureCloud", "AzureUSGovernment")]
        [string]$EnvironmentName = "AzureCloud",

        [Parameter(Mandatory = $false)]
        [int] $SelfSignedCertNoOfMonthsUntilExpired = 12
    )

    function CreateSelfSignedCertificate([string] $certificateName, [string] $selfSignedCertPlainPassword,
        [string] $certPath, [string] $certPathCer, [string] $selfSignedCertNoOfMonthsUntilExpired ) {
        $Cert = New-SelfSignedCertificate -DnsName $certificateName -CertStoreLocation cert:\LocalMachine\My `
            -KeyExportPolicy Exportable -Provider "Microsoft Enhanced RSA and AES Cryptographic Provider" `
            -NotAfter (Get-Date).AddMonths($selfSignedCertNoOfMonthsUntilExpired) -HashAlgorithm SHA256

        $CertPassword = ConvertTo-SecureString $selfSignedCertPlainPassword -AsPlainText -Force
        Export-PfxCertificate -Cert ("Cert:\localmachine\my\" + $Cert.Thumbprint) -FilePath $certPath -Password $CertPassword -Force | Write-Verbose
        Export-Certificate -Cert ("Cert:\localmachine\my\" + $Cert.Thumbprint) -FilePath $certPathCer -Type CERT | Write-Verbose
    }

    function CreateServicePrincipal([System.Security.Cryptography.X509Certificates.X509Certificate2] $PfxCert, [string] $applicationDisplayName) {
        $keyValue = [System.Convert]::ToBase64String($PfxCert.GetRawCertData())
        $keyId = (New-Guid).Guid

        # Create an Azure AD application, AD App Credential, AD ServicePrincipal

        # Requires Application Developer Role, but works with Application administrator or GLOBAL ADMIN
        $Application = New-AzADApplication -DisplayName $ApplicationDisplayName -HomePage ("http://" + $applicationDisplayName) -IdentifierUris ("http://" + $keyId)
        # Requires Application administrator or GLOBAL ADMIN
        $ApplicationCredential = New-AzADAppCredential -ApplicationId $Application.ApplicationId -CertValue $keyValue -StartDate $PfxCert.NotBefore -EndDate $PfxCert.NotAfter
        # Requires Application administrator or GLOBAL ADMIN
        $ServicePrincipal = New-AzADServicePrincipal -ApplicationId $Application.ApplicationId
        $GetServicePrincipal = Get-AzADServicePrincipal -ObjectId $ServicePrincipal.Id

        # Sleep here for a few seconds to allow the service principal application to become active (ordinarily takes a few seconds)
        Sleep -s 15
        # Requires User Access Administrator or Owner.
        $NewRole = New-AzRoleAssignment -RoleDefinitionName Contributor -ServicePrincipalName $Application.ApplicationId -ErrorAction SilentlyContinue
        $Retries = 0;
        While ($NewRole -eq $null -and $Retries -le 6) {
            Sleep -s 10
            New-AzRoleAssignment -RoleDefinitionName Contributor -ServicePrincipalName $Application.ApplicationId | Write-Verbose -ErrorAction SilentlyContinue
            $NewRole = Get-AzRoleAssignment -ServicePrincipalName $Application.ApplicationId -ErrorAction SilentlyContinue
            $Retries++;
        }
        return $Application.ApplicationId.ToString();
    }

    function CreateAutomationCertificateAsset ([string] $resourceGroup, [string] $automationAccountName, [string] $certifcateAssetName, [string] $certPath, [string] $certPlainPassword, [Boolean] $Exportable) {
        $CertPassword = ConvertTo-SecureString $certPlainPassword -AsPlainText -Force
        Remove-AzAutomationCertificate -ResourceGroupName $resourceGroup -AutomationAccountName $automationAccountName -Name $certifcateAssetName -ErrorAction SilentlyContinue
        New-AzAutomationCertificate -ResourceGroupName $resourceGroup -AutomationAccountName $automationAccountName -Path $certPath -Name $certifcateAssetName -Password $CertPassword -Exportable:$Exportable  | write-verbose
    }

    function CreateAutomationConnectionAsset ([string] $resourceGroup, [string] $automationAccountName, [string] $connectionAssetName, [string] $connectionTypeName, [System.Collections.Hashtable] $connectionFieldValues ) {
        Remove-AzAutomationConnection -ResourceGroupName $resourceGroup -AutomationAccountName $automationAccountName -Name $connectionAssetName -Force -ErrorAction SilentlyContinue
        New-AzAutomationConnection -ResourceGroupName $ResourceGroup -AutomationAccountName $automationAccountName -Name $connectionAssetName -ConnectionTypeName $connectionTypeName -ConnectionFieldValues $connectionFieldValues
    }

    Import-Module AzureRm.Profile
    Import-Module AzureRm.Resources

    $AureRmProfileVersion = (Get-Module AzureRm.Profile).Version
    if (!(($AzureRmProfileVersion.Major -ge 3 -and $AzureRmProfileVersion.Minor -ge 4) -or ($AzureRmProfileVersion.Major -gt 3))) {
        Write-Error -Message "Please install the latest Azure PowerShell and retry. Relevant doc url : https://docs.microsoft.com/powershell/azureps-cmdlets-docs/ "
        return
    }

    # To use the new Az modules to create your Run As accounts, please uncomment the following lines and ensure you comment out the previous 8 lines that import the AzureRM modules to avoid any issues. To learn about about using Az modules in your Automation account see https://docs.microsoft.com/azure/automation/az-modules.

    # Import-Module Az.Automation
    # Enable-AzureRmAlias


    Connect-AzAccount -Environment $EnvironmentName
    $Subscription = Get-AzSubscription -SubscriptionId $SubscriptionId

    # Create a Run As account by using a service principal
    $CertifcateAssetName = "AzureRunAsCertificate"
    $ConnectionAssetName = "AzureRunAsConnection"
    $ConnectionTypeName = "AzureServicePrincipal"

    if ($EnterpriseCertPathForRunAsAccount -and $EnterpriseCertPlainPasswordForRunAsAccount) {
        $PfxCertPathForRunAsAccount = $EnterpriseCertPathForRunAsAccount
        $PfxCertPlainPasswordForRunAsAccount = $EnterpriseCertPlainPasswordForRunAsAccount
    }
    else {
        $CertificateName = $AutomationAccountName + $CertifcateAssetName
        $PfxCertPathForRunAsAccount = Join-Path $env:TEMP ($CertificateName + ".pfx")
        $PfxCertPlainPasswordForRunAsAccount = $SelfSignedCertPlainPassword
        $CerCertPathForRunAsAccount = Join-Path $env:TEMP ($CertificateName + ".cer")
        CreateSelfSignedCertificate $CertificateName $PfxCertPlainPasswordForRunAsAccount $PfxCertPathForRunAsAccount $CerCertPathForRunAsAccount $SelfSignedCertNoOfMonthsUntilExpired
    }

    # Create a service principal
    $PfxCert = New-Object -TypeName System.Security.Cryptography.X509Certificates.X509Certificate2 -ArgumentList @($PfxCertPathForRunAsAccount, $PfxCertPlainPasswordForRunAsAccount)
    $ApplicationId = CreateServicePrincipal $PfxCert $ApplicationDisplayName

    # Create the Automation certificate asset
    CreateAutomationCertificateAsset $ResourceGroup $AutomationAccountName $CertifcateAssetName $PfxCertPathForRunAsAccount $PfxCertPlainPasswordForRunAsAccount $true

    # Populate the ConnectionFieldValues
    $SubscriptionInfo = Get-AzSubscription -SubscriptionId $SubscriptionId
    $TenantID = $SubscriptionInfo | Select TenantId -First 1
    $Thumbprint = $PfxCert.Thumbprint
    $ConnectionFieldValues = @{"ApplicationId" = $ApplicationId; "TenantId" = $TenantID.TenantId; "CertificateThumbprint" = $Thumbprint; "SubscriptionId" = $SubscriptionId}

    # Create an Automation connection asset named AzureRunAsConnection in the Automation account. This connection uses the service principal.
    CreateAutomationConnectionAsset $ResourceGroup $AutomationAccountName $ConnectionAssetName $ConnectionTypeName $ConnectionFieldValues

    if ($CreateClassicRunAsAccount) {
        # Create a Run As account by using a service principal
        $ClassicRunAsAccountCertifcateAssetName = "AzureClassicRunAsCertificate"
        $ClassicRunAsAccountConnectionAssetName = "AzureClassicRunAsConnection"
        $ClassicRunAsAccountConnectionTypeName = "AzureClassicCertificate "
        $UploadMessage = "Please upload the .cer format of #CERT# to the Management store by following the steps below." + [Environment]::NewLine +
        "Log in to the Microsoft Azure portal (https://portal.azure.com) and select Subscriptions -> Management Certificates." + [Environment]::NewLine +
        "Then click Upload and upload the .cer format of #CERT#"

        if ($EnterpriseCertPathForClassicRunAsAccount -and $EnterpriseCertPlainPasswordForClassicRunAsAccount ) {
            $PfxCertPathForClassicRunAsAccount = $EnterpriseCertPathForClassicRunAsAccount
            $PfxCertPlainPasswordForClassicRunAsAccount = $EnterpriseCertPlainPasswordForClassicRunAsAccount
            $UploadMessage = $UploadMessage.Replace("#CERT#", $PfxCertPathForClassicRunAsAccount)
        }
        else {
            $ClassicRunAsAccountCertificateName = $AutomationAccountName + $ClassicRunAsAccountCertifcateAssetName
            $PfxCertPathForClassicRunAsAccount = Join-Path $env:TEMP ($ClassicRunAsAccountCertificateName + ".pfx")
            $PfxCertPlainPasswordForClassicRunAsAccount = $SelfSignedCertPlainPassword
            $CerCertPathForClassicRunAsAccount = Join-Path $env:TEMP ($ClassicRunAsAccountCertificateName + ".cer")
            $UploadMessage = $UploadMessage.Replace("#CERT#", $CerCertPathForClassicRunAsAccount)
            CreateSelfSignedCertificate $ClassicRunAsAccountCertificateName $PfxCertPlainPasswordForClassicRunAsAccount $PfxCertPathForClassicRunAsAccount $CerCertPathForClassicRunAsAccount $SelfSignedCertNoOfMonthsUntilExpired
        }

        # Create the Automation certificate asset
        CreateAutomationCertificateAsset $ResourceGroup $AutomationAccountName $ClassicRunAsAccountCertifcateAssetName $PfxCertPathForClassicRunAsAccount $PfxCertPlainPasswordForClassicRunAsAccount $false

        # Populate the ConnectionFieldValues
        $SubscriptionName = $subscription.Subscription.Name
        $ClassicRunAsAccountConnectionFieldValues = @{"SubscriptionName" = $SubscriptionName; "SubscriptionId" = $SubscriptionId; "CertificateAssetName" = $ClassicRunAsAccountCertifcateAssetName}

        # Create an Automation connection asset named AzureRunAsConnection in the Automation account. This connection uses the service principal.
        CreateAutomationConnectionAsset $ResourceGroup $AutomationAccountName $ClassicRunAsAccountConnectionAssetName $ClassicRunAsAccountConnectionTypeName   $ClassicRunAsAccountConnectionFieldValues

        Write-Host -ForegroundColor red       $UploadMessage
    }
```

>[!NOTE]
>`Add-AzAccount` y `Add-AzureRMAccount` son alias de [Connect-AzAccount](https://docs.microsoft.com/powershell/module/az.accounts/connect-azaccount?view=azps-3.5.0). Puede usar estos cmdlets o bien [actualizar los módulos](automation-update-azure-modules.md) de la cuenta de Automation a las versiones más recientes. Es posible que deba actualizar los módulos incluso si acaba de crear una nueva cuenta de Automation.

### <a name="execute-the-powershell-script"></a>Ejecución del script de PowerShell

1. En el equipo, inicie **Windows PowerShell** desde la pantalla **Inicio** con permisos de usuario elevados.
1. Desde el shell de línea de comandos con privilegios elevados, vaya a la carpeta que contiene el script.
1. Ejecute el script mediante los valores de parámetro de la configuración que necesita.
1. Si está creando una cuenta de ejecución clásica, después de ejecutar el script, cargue el certificado público (extensión de nombre del archivo **.cer**) en el almacén de administración para la suscripción en la que se creó la cuenta de Automation.

Después de ejecutar el script, se le pedirá que se autentique en Azure. Inicie sesión con una cuenta que sea miembro del rol de administradores de la suscripción y coadministrador de la suscripción.

#### <a name="create-a-run-as-account-by-using-a-self-signed-certificate"></a>Creación de una cuenta de ejecución mediante un certificado autofirmado

```powershell
    .\New-RunAsAccount.ps1 -ResourceGroup <ResourceGroupName> -AutomationAccountName <NameofAutomationAccount> -SubscriptionId <SubscriptionId> -ApplicationDisplayName <DisplayNameofAADApplication> -SelfSignedCertPlainPassword <StrongPassword> -CreateClassicRunAsAccount $false
```

#### <a name="create-a-run-as-account-and-a-classic-run-as-account-by-using-a-self-signed-certificate"></a>Creación de una cuenta de ejecución y una cuenta de ejecución clásica mediante un certificado autofirmado

```powershell
    .\New-RunAsAccount.ps1 -ResourceGroup <ResourceGroupName> -AutomationAccountName <NameofAutomationAccount> -SubscriptionId <SubscriptionId> -ApplicationDisplayName <DisplayNameofAADApplication> -SelfSignedCertPlainPassword <StrongPassword> -CreateClassicRunAsAccount $true
```

#### <a name="create-a-run-as-account-and-a-classic-run-as-account-by-using-an-enterprise-certificate"></a>Creación de una cuenta de ejecución y una cuenta de ejecución clásica mediante un certificado de empresa

```powershell
    .\New-RunAsAccount.ps1 -ResourceGroup <ResourceGroupName> -AutomationAccountName <NameofAutomationAccount> -SubscriptionId <SubscriptionId> -ApplicationDisplayName <DisplayNameofAADApplication>  -SelfSignedCertPlainPassword <StrongPassword> -CreateClassicRunAsAccount $true -EnterpriseCertPathForRunAsAccount <EnterpriseCertPfxPathForRunAsAccount> -EnterpriseCertPlainPasswordForRunAsAccount <StrongPassword> -EnterpriseCertPathForClassicRunAsAccount <EnterpriseCertPfxPathForClassicRunAsAccount> -EnterpriseCertPlainPasswordForClassicRunAsAccount <StrongPassword>
```

Si creó una cuenta de ejecución clásica con un certificado público de empresa (archivo **.cer**) , use este certificado. Consulte [Carga de un certificado de API de administración en Azure Portal](../azure-api-management-certs.md).

#### <a name="create-a-run-as-account-and-a-classic-run-as-account-by-using-a-self-signed-certificate-in-the-azure-government-cloud"></a>Creación de una cuenta de ejecución y una cuenta de ejecución clásica mediante un certificado autofirmado en la nube de Azure Government

```powershell
    .\New-RunAsAccount.ps1 -ResourceGroup <ResourceGroupName> -AutomationAccountName <NameofAutomationAccount> -SubscriptionId <SubscriptionId> -ApplicationDisplayName <DisplayNameofAADApplication> -SelfSignedCertPlainPassword <StrongPassword> -CreateClassicRunAsAccount $true  -EnvironmentName AzureUSGovernment
```

Si creó una cuenta de ejecución clásica con un certificado público autofirmado (archivo **.cer**), el script la crea y la guarda en la carpeta de archivos temporales del equipo. Se puede encontrar en el perfil de usuario `%USERPROFILE%\AppData\Local\Temp`, que usó para ejecutar la sesión de PowerShell.

## <a name="deleting-a-run-as-or-classic-run-as-account"></a>Eliminación de una cuenta de ejecución o una cuenta de ejecución clásica

En esta sección se describe cómo eliminar una cuenta de ejecución o una cuenta de ejecución clásica. Al realizar esta acción, la cuenta de Automation se conserva. Después de eliminar la cuenta, puede volver a crearla en Azure Portal.

1. Abra la cuenta de Automation en Azure Portal.

2. En el panel izquierdo, seleccione **Cuentas de ejecución** en la sección Configuración de la cuenta.

3. En la página de propiedades Cuentas de ejecución, seleccione la cuenta de ejecución o la cuenta de ejecución clásica que quiere eliminar. 

4. En el panel Propiedades de la cuenta seleccionada, haga clic en **Eliminar**.

   ![Eliminación de una cuenta de ejecución](media/manage-runas-account/automation-account-delete-runas.png)

5. Mientras se está eliminando la cuenta, puede seguir el progreso desde el menú, en **Notificaciones**.

6. Después de eliminar la cuenta, puede volver a crearla en la página de propiedades de Cuentas de ejecución. Para ello, seleccione la opción de creación **Cuenta de ejecución de Azure**.

   ![Nueva creación de la cuenta de ejecución de Automation](media/manage-runas-account/automation-account-create-runas.png)

## <a name="renewing-a-self-signed-certificate"></a><a name="cert-renewal"></a>Renovación de un certificado autofirmado

El certificado autofirmado que creó para la cuenta de ejecución expira un año después de la fecha de creación. En algún momento antes de que expire su cuenta de ejecución, debe renovar el certificado. Se puede renovar en cualquier momento antes de que expire. 

Cuando se renueva el certificado autofirmado, el certificado válido actual se conserva para garantizar que los runbooks que se ponen en la cola o que se ejecutan activamente, y que se autentican con la cuenta de ejecución, no se ven negativamente afectados. El certificado es válido hasta la fecha de expiración.

>[!NOTE]
>Si cree que se ha puesto en peligro la cuenta de ejecución, puede eliminarla y volver a crearla.

>[!NOTE]
>Si ha configurado la cuenta de ejecución para usar un certificado emitido por la entidad de certificación de la empresa y usa esta opción para renovar un certificado autofirmado, el certificado de empresa se reemplaza por uno autofirmado.

Realice los pasos siguientes para renovar el certificado autofirmado.

1. Abra la cuenta de Automation en Azure Portal.

1. Seleccione **Cuentas de ejecución** en la sección Configuración de la cuenta.

    ![Panel de propiedades de la cuenta de Automation](media/manage-runas-account/automation-account-properties-pane.png)

1. En la página de propiedades de Cuentas de ejecución, seleccione la cuenta de ejecución o la cuenta de ejecución clásica para la que quiere renovar el certificado.

1. En el panel Propiedades de la cuenta seleccionada, haga clic en **Renovar certificado**.

    ![Renovación del certificado para una cuenta de ejecución](media/manage-runas-account/automation-account-renew-runas-certificate.png)

1. Mientras se está renovando el certificado, puede seguir el progreso desde el menú, en **Notificaciones**.

## <a name="setting-up-automatic-certificate-renewal-with-an-automation-runbook"></a><a name="auto-cert-renewal"></a>Configuración de la renovación automática de certificados con un runbook de Automation

Para renovar los certificados automáticamente, puede usar un runbook de Automation. Este script en [GitHub](https://github.com/ikanni/PowerShellScripts/blob/master/AzureAutomation/RunAsAccount/GrantPermissionToRunAsAccountAADApplication-ToRenewCertificateItself-CreateSchedule.ps1) habilita esta funcionalidad en la cuenta de Automation.

>[!NOTE]
>Debe ser un administrador global o un administrador de empresa en Azure AD para ejecutar el script.

El script crea una programación semanal para renovar los certificados de cuenta de ejecución. Agrega un runbook **Update-AutomationRunAsCredential** a la cuenta de Automation. Puede ver el código del runbook en GitHub, en el script [Update-AutomationRunAsCredential.ps1](https://github.com/azureautomation/runbooks/blob/master/Utility/ARM/Update-AutomationRunAsCredential.ps1). Puede usar el código de PowerShell en el archivo para renovar los certificados manualmente según sea necesario.

Para probar el proceso de renovación de inmediato, realice los pasos siguientes.

1. Edite el runbook **Update-AutomationRunAsCredential** y coloque un carácter de comentario (#) en la línea 122, delante del comando **Exit(1)** .

   ```powershell
   #Exit(1)
   ```

2. Publique el runbook.
3. Inicie el runbook.
4. Compruebe la renovación correcta con el código siguiente:

   ```powershell
   (Get-AzAutomationCertificate -AutomationAccountName TestAA
                                -Name AzureRunAsCertificate
                                -ResourceGroupName TestAutomation).ExpiryTime.DateTime
   ```
    Salida:

   ```Output
   Thursday, November 7, 2019 7:00:00 PM
   ```

5. Después de la prueba, edite el runbook y quite el carácter de comentario que agregó en el paso 1.
6. Publique el runbook.

## <a name="limiting-run-as-account-permissions"></a><a name="limiting-run-as-account-permissions"></a>Limitación de los permisos de las cuentas de ejecución

Para controlar la orientación de Automation hacia los recursos de Azure, puede ejecutar el script [Update-AutomationRunAsAccountRoleAssignments.ps1](https://aka.ms/AA5hug8). Este script cambia la entidad de servicio de la cuenta de ejecución existente para crear y usar una definición de rol personalizada. Este rol tiene permisos para todos los recursos excepto para [Key Vault](https://docs.microsoft.com/azure/key-vault/).

>[!IMPORTANT]
>Después de ejecutar el script **Update-AutomationRunAsAccountRoleAssignments.ps1**, los runbooks que acceden a Key Vault mediante cuentas de ejecución ya no funcionan. Antes de ejecutar el script, debe revisar los runbooks de su cuenta para las llamadas a Azure Key Vault. Para permitir el acceso a Key Vault desde los runbooks de Azure Automation, debe [agregar la cuenta de ejecución a los permisos de Key Vault](#add-permissions-to-key-vault).

Si necesita restringir aún más lo que la entidad de servicio de ejecución puede hacer, puede agregar otros tipos de recursos al elemento `NotActions` de la definición de roles personalizada. El siguiente ejemplo restringe el acceso a `Microsoft.Compute/*`. Si agrega este tipo definición de recurso a `NotActions` para la definición de roles, el rol no podrá acceder a ningún recurso de proceso. Para más información sobre las definiciones de roles, consulte [Descripción de definiciones de roles para los recursos de Azure](../role-based-access-control/role-definitions.md).

```powershell
$roleDefinition = Get-AzRoleDefinition -Name 'Automation RunAs Contributor'
$roleDefinition.NotActions.Add("Microsoft.Compute/*")
$roleDefinition | Set-AzRoleDefinition
```

Puede determinar si la entidad de servicio que se usa en la cuenta de ejecución está en la definición del rol Colaborador o en una personalizada. 

1. Vaya a la cuenta de Automation y seleccione **Cuentas de ejecución** en la sección Configuración de la cuenta.
2. Seleccione **Cuenta de ejecución de Azure**. 
3. Seleccione **Rol** para localizar la definición de roles que se está usando.

[![](media/manage-runas-account/verify-role.png "Verify the Run As Account role")](media/manage-runas-account/verify-role-expanded.png#lightbox)

También puede determinar la definición de roles que usan las cuentas de ejecución para varias suscripciones o cuentas de Automation. Para ello, use el script [Check-AutomationRunAsAccountRoleAssignments.ps1](https://aka.ms/AA5hug5) en la Galería de PowerShell.

### <a name="add-permissions-to-key-vault"></a>Incorporación de permisos a Key Vault

Puede permitir que Azure Automation compruebe si Key Vault y la entidad de servicio de la cuenta de ejecución usan una definición de roles personalizada. Debe:

* Conceder permisos a Key Vault.
* Establecer la directiva de acceso.

Puede usar el script [Extend-AutomationRunAsAccountRoleAssignmentToKeyVault.ps1](https://aka.ms/AA5hugb) en el Galería de PowerShell para asignar sus permisos de la cuenta de ejecución a KeyVault. Consulte [Concesión de acceso a una instancia de Key Vault para las aplicaciones](../key-vault/key-vault-group-permissions-for-apps.md) para obtener más detalles sobre cómo establecer permisos en Key Vault.

## <a name="resolving-misconfiguration-issues-for-run-as-accounts"></a>Solución de problemas de configuración incorrecta de las cuentas de ejecución

Puede que algunos elementos de configuración necesarios para la cuenta de ejecución o la cuenta de ejecución clásica se hayan eliminado o no se hayan creado correctamente durante la configuración inicial. Algunos errores de configuración posibles son:

* Recurso de certificado
* Recurso de conexión
* Cuenta de ejecución quitada del rol Colaborador
* Entidad de servicio o aplicación en Azure AD

En estos casos de errores de configuración, la cuenta de Automation detecta los cambios y muestra el estado `Incomplete` en el panel de propiedades de Cuentas de ejecución de la cuenta.

![Estado de configuración incompleta de la cuenta de ejecución](media/manage-runas-account/automation-account-runas-incomplete-config.png)

Al seleccionar la cuenta de ejecución, el panel de propiedades de la cuenta muestra el mensaje de error siguiente:

```text
The Run As account is incomplete. Either one of these was deleted or not created - Azure Active Directory Application, Service Principal, Role, Automation Certificate asset, Automation Connect asset - or the Thumbprint is not identical between Certificate and Connection. Please delete and then re-create the Run As Account.
```

Rápidamente puede resolver estos problemas de la cuenta de ejecución con solo eliminarla cuenta y volver a crearla.

## <a name="next-steps"></a>Pasos siguientes

* Para más información sobre las entidades de servicio, consulte [Objetos de aplicación y de entidad de servicio](../active-directory/develop/app-objects-and-service-principals.md).
* Para más información sobre los certificados y los servicios de Azure, consulte [Introducción a los certificados para Azure Cloud Services](../cloud-services/cloud-services-certs-create.md).
