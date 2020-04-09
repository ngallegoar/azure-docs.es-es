---
title: Ejecución de runbooks en Azure Automation Hybrid Runbook Worker
description: Este artículo proporciona información acerca de cómo ejecutar runbooks en máquinas de un centro de datos local o en el proveedor de la nube con el rol Hybrid Runbook Worker.
services: automation
ms.subservice: process-automation
ms.date: 01/29/2019
ms.topic: conceptual
ms.openlocfilehash: 902734ddc7195d643c3aedb4054f57723d1a51c2
ms.sourcegitcommit: d597800237783fc384875123ba47aab5671ceb88
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/03/2020
ms.locfileid: "80632140"
---
# <a name="running-runbooks-on-a-hybrid-runbook-worker"></a>Ejecución de runbooks en Hybrid Runbook Worker

Los runbooks que tienen como destino una instancia de Hybrid Runbook Worker normalmente administran recursos en el equipo local o recursos en el entorno local en el que se implementa el rol de trabajo. Los runbooks en Azure Automation normalmente administran recursos en la nube de Azure. Aunque se usan de manera diferente, los runbooks que se ejecutan en Azure Automation y los runbooks que se ejecutan en una instancia de Hybrid Runbook Worker son idénticos en estructura.

Cuando cree un runbook para que se ejecute en una instancia de Hybrid Runbook Worker, debe editarlo y probarlo en la máquina que hospeda el rol de trabajo. La máquina host tiene todos los módulos de PowerShell y el acceso de red necesarios para administrar y acceder a los recursos locales. Una vez que un runbook se ha probado en la máquina de Hybrid Runbook Worker, puede cargarlo en el entorno de Azure Automation, donde se puede ejecutar en el rol de trabajo. 

>[!NOTE]
>Este artículo se ha actualizado para usar el nuevo módulo Az de Azure PowerShell. Aún puede usar el módulo de AzureRM que continuará recibiendo correcciones de errores hasta diciembre de 2020 como mínimo. Para más información acerca del nuevo módulo Az y la compatibilidad con AzureRM, consulte [Introducing the new Azure PowerShell Az module](https://docs.microsoft.com/powershell/azure/new-azureps-module-az?view=azps-3.5.0) (Presentación del nuevo módulo Az de Azure PowerShell). Para obtener instrucciones sobre la instalación del módulo Az en Hybrid Runbook Worker, consulte [Instalación del módulo de Azure PowerShell](https://docs.microsoft.com/powershell/azure/install-az-ps?view=azps-3.5.0). Puede actualizar los módulos de su cuenta de Automation a la versión más reciente mediante [Actualización de módulos de Azure PowerShell en Azure Automation](automation-update-azure-modules.md).

## <a name="runbook-permissions-for-a-hybrid-runbook-worker"></a>Permisos del runbook para una instancia de Hybrid Runbook Worker

Puesto que acceden a recursos que no son de Azure, los runbooks que se ejecutan en una instancia de Hybrid Runbook Worker no pueden usar el mecanismo de autenticación que se usa normalmente para los runbooks que se autentican en los recursos de Azure. Un runbook proporciona su propia autenticación a los recursos locales o configura la autenticación mediante [identidades administradas para los recursos de Azure](../active-directory/managed-identities-azure-resources/tutorial-windows-vm-access-arm.md#grant-your-vm-access-to-a-resource-group-in-resource-manager). También puede especificar una cuenta de ejecución para proporcionar un contexto de usuario para todos los runbooks.

### <a name="runbook-authentication"></a>Autenticación de runbook

De forma predeterminada, los runbooks se ejecutan en el equipo local. En Windows, se ejecutan en el contexto de la cuenta de sistema local. En el caso de Linux, se ejecutan en el contexto de la cuenta de usuario especial **nxautomation**. En cualquier escenario, los runbooks deben proporcionar su propia autenticación a los recursos a los que tienen acceso.

Puede usar los recursos de [Credencial](automation-credentials.md) y [Certificado](automation-certificates.md) en el runbook con cmdlets que le permitan especificar las credenciales, para que el runbook pueda autenticarse en los distintos recursos. El ejemplo siguiente muestra una porción de un runbook que reinicia un equipo. Recupera las credenciales desde un recurso de credencial y el nombre del equipo desde un recurso de variable y, a continuación, usa estos valores con el cmdlet `Restart-Computer`.

```powershell
$Cred = Get-AutomationPSCredential -Name "MyCredential"
$Computer = Get-AutomationVariable -Name "ComputerName"

Restart-Computer -ComputerName $Computer -Credential $Cred
```

También puede usar una actividad [InlineScript](automation-powershell-workflow.md#inlinescript). `InlineScript` permite ejecutar bloques de código en otro equipo con las credenciales especificadas por el [parámetro común PSCredential](/powershell/module/psworkflow/about/about_workflowcommonparameters).

### <a name="run-as-account"></a>cuenta de identificación

En lugar de hacer que el runbook proporcione su propia autenticación a los recursos locales, puede especificar una cuenta de ejecución para un grupo de Hybrid Runbook Worker. Para ello, debe definir un [recurso de credencial](automation-credentials.md) que tenga acceso a los recursos locales. Estos recursos incluyen almacenes de certificados y todos los runbooks se ejecutan con estas credenciales en una instancia de Hybrid Runbook Worker del grupo.

El nombre de usuario de la credencial debe tener uno de los siguientes formatos:

* dominio\nombre de usuario
* username@domain
* nombre de usuario (para cuentas locales en el equipo local)

Utilice el procedimiento siguiente para especificar una cuenta de ejecución para un grupo de Hybrid Runbook Worker.

1. Cree un [recurso de credencial](automation-credentials.md) con acceso a los recursos locales.
2. Abra la cuenta de Automation en Azure Portal.
3. Seleccione el icono **Grupos de Hybrid Worker** y luego seleccione el grupo.
4. Seleccione **Toda la configuración** y luego **Configuración del grupo de Hybrid Worker**.
5. Cambie el valor de **Ejecutar como** de **Predeterminado** a **Personalizado**.
6. Seleccione la credencial y haga clic en **Guardar**.

### <a name="managed-identities-for-azure-resources"></a><a name="managed-identities-for-azure-resources"></a>Identidades administradas de recursos de Azure

Las instancias de Hybrid Runbook Worker en máquinas virtuales de Azure pueden usar identidades administradas para los recursos de Azure para autenticarse en los recursos de Azure. El uso de identidades administradas para los recursos de Azure en lugar de cuentas de ejecución ofrece ventajas porque no es necesario:

* Exportar el certificado de ejecución y después importarlo en Hybrid Runbook Worker.
* Renovar el certificado utilizado por la cuenta de ejecución.
* Administrar el objeto de conexión de ejecución en el código del runbook.

Realice los pasos siguientes para usar una identidad administrada para los recursos de Azure en una instancia de Hybrid Runbook Worker.

1. Cree una máquina virtual de Azure.
2. Configure identidades administradas para los recursos de Azure en la máquina virtual. Consulte [Configuración de identidades administradas para recursos de Azure en una máquina virtual mediante Azure Portal](../active-directory/managed-identities-azure-resources/qs-configure-portal-windows-vm.md#enable-system-assigned-managed-identity-on-an-existing-vm).
3. Conceda acceso a la máquina virtual a un grupo de recursos en Resource Manager. Consulte [Uso de una identidad administrada asignada por el sistema de una máquina virtual Windows para acceder a Resource Manager](../active-directory/managed-identities-azure-resources/tutorial-windows-vm-access-arm.md#grant-your-vm-access-to-a-resource-group-in-resource-manager).
4. Instale Hybrid Runbook Worker en la máquina virtual. Consulte [Implementación de Hybrid Runbook Worker en Windows](automation-windows-hrw-install.md).
5. Actualice el runbook para que use el cmdlet [Connect-AzAccount](https://docs.microsoft.com/powershell/module/az.accounts/connect-azaccount?view=azps-3.5.0) con el parámetro `Identity` para autenticarse en los recursos de Azure. Esta configuración reduce la necesidad de usar una cuenta de ejecución y realizar la administración de las cuentas asociadas.

```powershell
    # Connect to Azure using the managed identities for Azure resources identity configured on the Azure VM that is hosting the hybrid runbook worker
    Connect-AzAccount -Identity

    # Get all VM names from the subscription
    Get-AzVM | Select Name
```

> [!NOTE]
> `Connect-AzAccount -Identity` funciona para una instancia de Hybrid Runbook Worker que use una identidad asignada por el sistema y una única identidad asignada por el usuario. Si usa varias identidades asignadas por el usuario en Hybrid Runbook Worker, el runbook debe especificar el parámetro *AccountId* para `Connect-AzAccount` a fin de seleccionar una identidad asignada por el usuario específica.

### <a name="automation-run-as-account"></a><a name="runas-script"></a>Cuenta de ejecución de Automation

Como parte del proceso de compilación automatizado para la implementación de recursos en Azure, es posible que necesite acceder a los sistemas locales para admitir una tarea o un conjunto de pasos de la secuencia de implementación. Para proporcionar la autenticación en Azure mediante la cuenta de ejecución, debe instalar el certificado de la cuenta de ejecución.

El siguiente runbook de PowerShell, llamado **Export-RunAsCertificateToHybridWorker**, exporta el certificado de ejecución de la cuenta de Azure Automation. El runbook descarga e importa el certificado en el almacén de certificados del equipo local en una instancia de Hybrid Runbook Worker que está conectada a la misma cuenta. Una vez que completa dicho paso, el runbook comprueba que el rol de trabajo se puede autenticar correctamente en Azure con la cuenta de ejecución.

```azurepowershell-interactive
<#PSScriptInfo
.VERSION 1.0
.GUID 3a796b9a-623d-499d-86c8-c249f10a6986
.AUTHOR Azure Automation Team
.COMPANYNAME Microsoft
.COPYRIGHT
.TAGS Azure Automation
.LICENSEURI
.PROJECTURI
.ICONURI
.EXTERNALMODULEDEPENDENCIES
.REQUIREDSCRIPTS
.EXTERNALSCRIPTDEPENDENCIES
.RELEASENOTES
#>

<#
.SYNOPSIS
Exports the Run As certificate from an Azure Automation account to a hybrid worker in that account.

.DESCRIPTION
This runbook exports the Run As certificate from an Azure Automation account to a hybrid worker in that account. Run this runbook on the hybrid worker where you want the certificate installed. This allows the use of the AzureRunAsConnection to authenticate to Azure and manage Azure resources from runbooks running on the hybrid worker.

.EXAMPLE
.\Export-RunAsCertificateToHybridWorker

.NOTES
LASTEDIT: 2016.10.13
#>

# Generate the password used for this certificate
Add-Type -AssemblyName System.Web -ErrorAction SilentlyContinue | Out-Null
$Password = [System.Web.Security.Membership]::GeneratePassword(25, 10)

# Stop on errors
$ErrorActionPreference = 'stop'

# Get the management certificate that will be used to make calls into Azure Service Management resources
$RunAsCert = Get-AutomationCertificate -Name "AzureRunAsCertificate"

# location to store temporary certificate in the Automation service host
$CertPath = Join-Path $env:temp  "AzureRunAsCertificate.pfx"

# Save the certificate
$Cert = $RunAsCert.Export("pfx",$Password)
Set-Content -Value $Cert -Path $CertPath -Force -Encoding Byte | Write-Verbose

Write-Output ("Importing certificate into $env:computername local machine root store from " + $CertPath)
$SecurePassword = ConvertTo-SecureString $Password -AsPlainText -Force
Import-PfxCertificate -FilePath $CertPath -CertStoreLocation Cert:\LocalMachine\My -Password $SecurePassword -Exportable | Write-Verbose

# Test to see if authentication to Azure Resource Manager is working
$RunAsConnection = Get-AutomationConnection -Name "AzureRunAsConnection"

Connect-AzAccount `
    -ServicePrincipal `
    -Tenant $RunAsConnection.TenantId `
    -ApplicationId $RunAsConnection.ApplicationId `
    -CertificateThumbprint $RunAsConnection.CertificateThumbprint | Write-Verbose

Set-AzContext -Subscription $RunAsConnection.SubscriptionID | Write-Verbose

# List automation accounts to confirm that Azure Resource Manager calls are working
Get-AzAutomationAccount | Select-Object AutomationAccountName
```

>[!NOTE]
>En el caso de los runbooks de PowerShell, `Add-AzAccount` y `Add-AzureRMAccount` son alias de `Connect-AzAccount`. Al buscar los elementos de biblioteca, si no ve `Connect-AzAccount`, puede usar `Add-AzAccount` o puede actualizar los módulos en la cuenta de Automation.

Para finalizar la preparación de la cuenta de ejecución:

1. Guarde el runbook **Export-RunAsCertificateToHybridWorker** en el equipo con la extensión **.ps1**.
2. Impórtelo en la cuenta de Automation.
3. Edite el runbook y cambie el valor de la variable `Password` por su propia contraseña. 
4. Publique el runbook.
5. Ejecute el runbook y establezca como destino el grupo de Hybrid Runbook Worker que ejecuta y autentica los runbooks con la cuenta de ejecución. 
6. Examine el flujo de trabajo para ver si informa sobre el intento de importar el certificado en el almacén del equipo local y sigue con varias líneas. Este comportamiento depende de cuántas cuentas de Automation se definan en la suscripción y del grado de éxito de la autenticación.

## <a name="job-behavior-on-hybrid-runbook-workers"></a>Comportamiento del trabajo en Hybrid Runbook Worker

Azure Automation controla los trabajos en Hybrid Runbook Worker de forma ligeramente diferente a los trabajos que se ejecutan en espacios aislados de Azure. Una diferencia importante es que no hay ningún límite en la duración del trabajo en Hybrid Runbook Worker. Los runbooks que se ejecutan en espacios aislados de Azure están limitados a tres horas debido al [reparto equitativo](automation-runbook-execution.md#fair-share).

En el caso de un runbook de larga ejecución, querrá asegurarse de que es resistente a posibles reinicios, por ejemplo, si se reinicia la máquina que hospeda el rol de trabajo. Si la máquina host de Hybrid Runbook Worker se reinicia, cualquier trabajo de runbook en ejecución se reinicia desde el principio o desde el último punto de comprobación para runbooks de Flujo de trabajo de PowerShell. Si un trabajo de runbook se reinicia más de tres veces, se suspende.

Recuerde que los trabajos de Hybrid Runbook Worker se ejecutan en la cuenta del sistema local en Windows o en la cuenta **nxautomation** en Linux. En el caso de Linux, debe asegurarse de que la cuenta **nxautomation** tiene acceso a la ubicación en la que se almacenan los módulos del runbook. Cuando use el cmdlet [Install-Module](/powershell/module/powershellget/install-module), asegúrese de especificar AllUsers para el parámetro `Scope` a fin de asegurarse de que la cuenta **nxautomation** tiene acceso. Para obtener más información sobre PowerShell en Linux, consulte [Problemas conocidos de PowerShell en plataformas que no son de Windows](https://docs.microsoft.com/powershell/scripting/whats-new/known-issues-ps6?view=powershell-6#known-issues-for-powershell-on-non-windows-platforms).

## <a name="starting-a-runbook-on-a-hybrid-runbook-worker"></a>Inicio de un runbook en Hybrid Runbook Worker

[Inicio de un runbook en Azure Automation](automation-starting-a-runbook.md) describe los distintos métodos para iniciar un runbook. El inicio de un runbook en una instancia de Hybrid Runbook Worker usa la opción **Ejecutar en**, que le permite especificar el nombre de un grupo de Hybrid Runbook Worker. Si se especifica un grupo, uno de los roles de trabajo del grupo recupera y ejecuta el runbook. Si el runbook no especifica esta opción, Azure Automation ejecuta el runbook como de costumbre.

Cuando inicie un runbook en Azure Portal, verá la opción **Ejecutar en**, para la que puede seleccionar **Azure** o **Hybrid Worker**. Si selecciona **Hybrid Worker**, puede elegir el grupo de Hybrid Runbook Worker en una lista desplegable.

Utilice el parámetro `RunOn` con el cmdlet `Start-AzureAutomationRunbook`. En el ejemplo siguiente se usa Windows PowerShell para iniciar un runbook llamado **Test-Runbook** en un grupo de Hybrid Runbook Worker llamado MyHybridGroup.

```azurepowershell-interactive
Start-AzureAutomationRunbook –AutomationAccountName "MyAutomationAccount" –Name "Test-Runbook" -RunOn "MyHybridGroup"
```

> [!NOTE]
> El parámetro `RunOn` se agregó a `Start-AzureAutomationRunbook` en la versión 0.9.1 de Microsoft Azure PowerShell. Si tiene instalada una versión anterior, deberá [descargar la versión más reciente](https://azure.microsoft.com/downloads/) . Solo necesita instalar esta versión en la estación de trabajo en la que inicia el runbook desde PowerShell. No es necesario que la instale en el equipo de Hybrid Runbook Worker, a menos que tenga la intención de iniciar runbooks desde ese equipo.

## <a name="working-with-signed-runbooks-on-a-windows-hybrid-runbook-worker"></a>Trabajo con runbooks firmados en Hybrid Runbook Worker en Windows

Puede configurar una instancia de Hybrid Runbook Worker en Windows para que solo ejecute runbooks firmados.

> [!IMPORTANT]
> Una vez que haya configurado una instancia de Hybrid Runbook Worker para que solo ejecute runbooks firmados, los runbooks que no estén firmados no se ejecutarán en el rol de trabajo.

### <a name="create-signing-certificate"></a>Creación de certificado de firma

En el ejemplo siguiente se crea un certificado autofirmado que se puede usar para firmar runbooks. Este código crea el certificado y lo exporta para que Hybrid Runbook Worker pueda importarlo más adelante. También se devuelve la huella digital para su uso posterior al hacer referencia al certificado.

```powershell
# Create a self-signed certificate that can be used for code signing
$SigningCert = New-SelfSignedCertificate -CertStoreLocation cert:\LocalMachine\my `
                                        -Subject "CN=contoso.com" `
                                        -KeyAlgorithm RSA `
                                        -KeyLength 2048 `
                                        -Provider "Microsoft Enhanced RSA and AES Cryptographic Provider" `
                                        -KeyExportPolicy Exportable `
                                        -KeyUsage DigitalSignature `
                                        -Type CodeSigningCert


# Export the certificate so that it can be imported to the hybrid workers
Export-Certificate -Cert $SigningCert -FilePath .\hybridworkersigningcertificate.cer

# Import the certificate into the trusted root store so the certificate chain can be validated
Import-Certificate -FilePath .\hybridworkersigningcertificate.cer -CertStoreLocation Cert:\LocalMachine\Root

# Retrieve the thumbprint for later use
$SigningCert.Thumbprint
```

### <a name="import-certificate-and-configure-workers-for-signature-validation"></a>Importación del certificado y configuración de los roles de trabajo para la validación de firmas

Copie el certificado que ha creado en cada instancia de Hybrid Runbook Worker de un grupo. Ejecute el script siguiente para importar el certificado y configurar los roles de trabajo para que usen la validación de firma en los runbooks.

```powershell
# Install the certificate into a location that will be used for validation.
New-Item -Path Cert:\LocalMachine\AutomationHybridStore
Import-Certificate -FilePath .\hybridworkersigningcertificate.cer -CertStoreLocation Cert:\LocalMachine\AutomationHybridStore

# Import the certificate into the trusted root store so the certificate chain can be validated
Import-Certificate -FilePath .\hybridworkersigningcertificate.cer -CertStoreLocation Cert:\LocalMachine\Root

# Configure the hybrid worker to use signature validation on runbooks.
Set-HybridRunbookWorkerSignatureValidation -Enable $true -TrustedCertStoreLocation "Cert:\LocalMachine\AutomationHybridStore"
```

### <a name="sign-your-runbooks-using-the-certificate"></a>Firma de los runbooks con el certificado

Con las instancias de Hybrid Runbook Worker configuradas para usar solo runbooks firmados, debe firmar los runbooks que se van a utilizar en Hybrid Runbook Worker. Use el código de PowerShell de ejemplo siguiente para firmar estos runbooks.

```powershell
$SigningCert = ( Get-ChildItem -Path cert:\LocalMachine\My\<CertificateThumbprint>)
Set-AuthenticodeSignature .\TestRunbook.ps1 -Certificate $SigningCert
```

Una vez firmado el runbook, debe importarlo en la cuenta de Automation y publicarlo con el bloque de firma. Para saber cómo importar runbooks, consulte [Importar un runbook de un archivo en Azure Automation](manage-runbooks.md#importing-a-runbook).

## <a name="working-with-signed-runbooks-on-a-linux-hybrid-runbook-worker"></a>Trabajo con runbooks firmados en Hybrid Runbook Worker en Linux

Para poder trabajar con runbooks firmados, una instancia de Hybrid Runbook Worker en Linux debe tener [GPG](https://gnupg.org/index.html) ejecutable en el equipo local.

> [!IMPORTANT]
> Una vez que haya configurado una instancia de Hybrid Runbook Worker para que solo ejecute runbooks firmados, los runbooks que no estén firmados no se ejecutarán en el rol de trabajo.

### <a name="create-a-gpg-keyring-and-keypair"></a>Creación un conjunto de claves y un par de claves de GPG

Para crear el conjunto de claves y el par de claves de GPG, utilice la cuenta **nxautomation** de Hybrid Runbook Worker.

1. Use la aplicación sudo para iniciar sesión como la cuenta **nxautomation**.

    ```bash
    sudo su – nxautomation
    ```

2. Una vez que use **nxautomation**, genere el par de claves de GPG. GPG le guía por los pasos. Debe proporcionar el nombre, la dirección de correo electrónico, la hora de expiración y la frase de contraseña. A continuación, espere hasta que haya suficiente entropía en el equipo para que se genere la clave.

    ```bash
    sudo gpg --generate-key
    ```

3. Dado que el directorio de GPG se generó con sudo, debe cambiar el propietario a **nxautomation** con el comando siguiente.

    ```bash
    sudo chown -R nxautomation ~/.gnupg
    ```

### <a name="make-the-keyring-available-to-the-hybrid-runbook-worker"></a>Puesta a disposición del conjunto de claves para Hybrid Runbook Worker

Una vez creado el conjunto de claves, debe ponerlo a disposición de Hybrid Runbook Worker. Modifique el archivo de configuración **/var/opt/microsoft/omsagent/state/automationworker/diy/worker.conf** para incluir el siguiente código de ejemplo en la sección de archivo `[worker-optional]`.

```bash
gpg_public_keyring_path = /var/opt/microsoft/omsagent/run/.gnupg/pubring.kbx
```

### <a name="verify-that-signature-validation-is-on"></a>Verificación de que la validación de firmas está activada

Si se ha deshabilitado la validación de firmas en el equipo, debe activarla; para ello, ejecute el siguiente comando de sudo. Reemplace `<LogAnalyticsworkspaceId>` por el identificador del área de trabajo.

```bash
sudo python /opt/microsoft/omsconfig/modules/nxOMSAutomationWorker/DSCResources/MSFT_nxOMSAutomationWorkerResource/automationworker/scripts/require_runbook_signature.py --true <LogAnalyticsworkspaceId>
```

### <a name="sign-a-runbook"></a>Ejecución de un runbook

Una vez configurada la validación de firmas, use el siguiente comando de GPG para firmar un runbook.

```bash
gpg –-clear-sign <runbook name>
```

El runbook firmado se llama `<runbook name>.asc`.

El runbook firmado ahora se puede cargar en Azure Automation y se puede ejecutar como un runbook normal.

## <a name="next-steps"></a>Pasos siguientes

* Para más información sobre los métodos para iniciar un runbook, consulte [Inicio de un runbook en Azure Automation](automation-starting-a-runbook.md).
* Para más información sobre cómo usar el editor de texto para trabajar con runbooks de PowerShell en Azure Automation, consulte [Edición de un runbook en Azure Automation](automation-edit-textual-runbook.md).
* Si los runbooks no finalizan correctamente, revise la guía de solución de problemas sobre [errores de ejecución de un runbook](troubleshoot/hybrid-runbook-worker.md#runbook-execution-fails).
* Para obtener más información sobre PowerShell, incluidos los módulos de referencia de lenguaje y aprendizaje, consulte la [documentación de PowerShell](https://docs.microsoft.com/powershell/scripting/overview).
