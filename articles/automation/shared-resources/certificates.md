---
title: Administración de certificados en Azure Automation
description: Los certificados se almacenan de manera segura en Azure Automation, de modo que los runbooks o las configuraciones de DSC puedan acceder a ellos para realizar la autenticación en Azure y recursos de terceros. Este artículo explica los detalles de los certificados y cómo trabajar con ellos en la creación de textos y de gráficos.
services: automation
ms.service: automation
ms.subservice: shared-capabilities
author: mgoedtel
ms.author: magoedte
ms.date: 04/02/2019
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: 69bc1a0333365e15452c6d3b253266d37d99b608
ms.sourcegitcommit: ffc6e4f37233a82fcb14deca0c47f67a7d79ce5c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/21/2020
ms.locfileid: "81732823"
---
# <a name="manage-certificates-in-azure-automation"></a>Administración de certificados en Azure Automation

Los certificados se almacenan de manera segura en Azure Automation, de modo que los runbooks o las configuraciones de DSC puedan acceder a ellos mediante la actividad [Get-AzAutomationCertificate](https://docs.microsoft.com/powershell/module/Az.Automation/Get-AzAutomationCertificate?view=azps-3.7.0) para recursos de Azure Resource Manager. El almacenamiento seguro de certificados le permite crear runbooks y configuraciones de DSC que usan certificados para autenticación o agregarlos a Azure o a recursos de terceros.

Los recursos protegidos en Azure Automation incluyen credenciales, certificados, conexiones y variables cifradas. Estos recursos se cifran y se almacenan en Azure Automation con una clave única que se genera para cada cuenta de Automation. Esta clave se almacena en una instancia de Key Vault administrada por el sistema. Antes de almacenar un recurso seguro, la clave se carga desde Key Vault y luego se usa para cifrar el recurso. Este proceso se administra mediante Azure Automation.

>[!NOTE]
>Este artículo se ha actualizado para usar el nuevo módulo Az de Azure PowerShell. Aún puede usar el módulo de AzureRM que continuará recibiendo correcciones de errores hasta diciembre de 2020 como mínimo. Para más información acerca del nuevo módulo Az y la compatibilidad con AzureRM, consulte [Introducing the new Azure PowerShell Az module](https://docs.microsoft.com/powershell/azure/new-azureps-module-az?view=azps-3.5.0) (Presentación del nuevo módulo Az de Azure PowerShell). Para obtener instrucciones sobre la instalación del módulo Az en Hybrid Runbook Worker, consulte [Instalación del módulo de Azure PowerShell](https://docs.microsoft.com/powershell/azure/install-az-ps?view=azps-3.5.0). Puede actualizar los módulos de su cuenta de Automation a la versión más reciente mediante [Actualización de módulos de Azure PowerShell en Azure Automation](../automation-update-azure-modules.md).

## <a name="az-powershell-cmdlets"></a>Cmdlets de Az PowerShell

Para Az, los cmdlets de la tabla siguiente se usan para crear y administrar recursos de credenciales de automatización con Windows PowerShell. Estos cmdlets se incluyen como parte del [módulo Az.Automation](/powershell/azure/overview), que está disponible para usarse en las configuraciones de DSC y los runbooks de Automation.

|Cmdlet |Descripción|
| --- | ---|
|[Add-AzureCertificate](/powershell/module/servicemanagement/azure/add-azurecertificate)|Carga un certificado de servicio para el servicio en la nube especificado.|
|[Get-AzAutomationCertificate](https://docs.microsoft.com/powershell/module/Az.Automation/Get-AzAutomationCertificate?view=azps-3.7.0)|Recupera información sobre un certificado para utilizarlo en un runbook o en la configuración de DSC. Solo puede recuperar el certificado mediante la actividad `Get-AutomationCertificate`.|
|[New-AzAutomationCertificate](https://docs.microsoft.com/powershell/module/Az.Automation/New-AzAutomationCertificate?view=azps-3.7.0)|Crea un certificado nuevo en Azure Automation.|
|[Remove-AzAutomationCertificate](https://docs.microsoft.com/powershell/module/Az.Automation/Remove-AzAutomationCertificate?view=azps-3.7.0)|Quita un certificado de Azure Automation.|
|[Set-AzAutomationCertificate](https://docs.microsoft.com/powershell/module/Az.Automation/Set-AzAutomationCertificate?view=azps-3.7.0)|Establece las propiedades de un certificado existente, incluyendo la carga del archivo de certificado y el establecimiento de la contraseña para un archivo **.pfx**.|

## <a name="activities"></a>Actividades

Las actividades de la tabla siguiente se usan para obtener acceso a los certificados de un runbook y las configuraciones de DSC.

| Actividades | Descripción |
|:---|:---|
|`Get-AutomationCertificate`|Obtiene un certificado para usarlo en un runbook o una configuración de DSC. Devuelve un objeto [System.Security.Cryptography.X509Certificates.X509Certificate2](/dotnet/api/system.security.cryptography.x509certificates.x509certificate2).|

> [!NOTE] 
> Debe evitar el uso de variables en el parámetro `Name` de `Get-AutomationCertificate` en un runbook o configuración de DSC. El uso de variables en este parámetro complica la detección de dependencias entre runbooks o configuraciones de DSC y variables de Automation durante el tiempo de diseño.

## <a name="python-2-functions"></a>Funciones de Python 2

La función de la tabla siguiente se usa para acceder a los certificados de un runbook de Python 2.

| Función | Descripción |
|:---|:---|
| `automationassets.get_automation_certificate` | Recupera información de un recurso de certificado. |

> [!NOTE]
> Para acceder a las funciones del recurso, debe importar el módulo `automationassets` al principio del runbook de Python.

## <a name="creating-a-new-certificate"></a>Creación de un certificado nuevo

Cuando crea un certificado nuevo, debe cargar un archivo .cer o .pfx a Azure Automation. Si marca el certificado como exportable, podrá transferirlo fuera del almacén de certificados de Azure Automation. Si no es exportable, solo se puede usar para firmar dentro del runbook o la configuración de DSC. Azure Automation requiere que el certificado tenga el **proveedor de servicios criptográficos AES y RSA mejorado de Microsoft**.

### <a name="create-a-new-certificate-with-the-azure-portal"></a>Creación de un certificado nuevo con Azure Portal

1. En la cuenta de Automation, haga clic en **Recursos** para abrir la página Recursos.
2. Seleccione **Certificados** para abrir la página Certificados.
3. Haga clic en **Agregar un certificado** en la parte superior de la página.
4. Escriba un nombre para el certificado en el campo **Nombre**.
5. Para buscar un archivo **.cer** o **.pfx**, haga clic en **Seleccionar un archivo** en **Cargar un archivo de certificado**. Si selecciona un archivo **.pfx**, especifique una contraseña e indique si se puede exportar.
6. Haga clic en **Crear** para guardar el recurso de certificado nuevo.

### <a name="create-a-new-certificate-with-powershell"></a>Creación de un certificado nuevo con PowerShell

En el ejemplo siguiente se muestra cómo crear un nuevo certificado de Automation y marcarlo como exportable. En este ejemplo se importa un archivo **.pfx** existente.

```powershell-interactive
$certificateName = 'MyCertificate'
$PfxCertPath = '.\MyCert.pfx'
$CertificatePassword = ConvertTo-SecureString -String 'P@$$w0rd' -AsPlainText -Force
$ResourceGroup = "ResourceGroup01"

New-AzAutomationCertificate -AutomationAccountName "MyAutomationAccount" -Name $certificateName -Path $PfxCertPath –Password $CertificatePassword -Exportable -ResourceGroupName $ResourceGroup
```

### <a name="create-a-new-certificate-with-a-resource-manager-template"></a>Creación de un certificado nuevo con una plantilla de Resource Manager

En el ejemplo siguiente se muestra cómo implementar un certificado en su cuenta de Automation mediante una plantilla de Resource Manager a través de PowerShell:

```powershell-interactive
$AutomationAccountName = "<automation account name>"
$PfxCertPath = '<PFX cert path'
$CertificatePassword = '<password>'
$certificateName = '<certificate name>'
$AutomationAccountName = '<automation account name>'
$flags = [System.Security.Cryptography.X509Certificates.X509KeyStorageFlags]::Exportable `
    -bor [System.Security.Cryptography.X509Certificates.X509KeyStorageFlags]::PersistKeySet `
    -bor [System.Security.Cryptography.X509Certificates.X509KeyStorageFlags]::MachineKeySet
# Load the certificate into memory
$PfxCert = New-Object -TypeName System.Security.Cryptography.X509Certificates.X509Certificate2 -ArgumentList @($PfxCertPath, $CertificatePassword, $flags)
# Export the certificate and convert into base 64 string
$Base64Value = [System.Convert]::ToBase64String($PfxCert.Export([System.Security.Cryptography.X509Certificates.X509ContentType]::Pkcs12))
$Thumbprint = $PfxCert.Thumbprint


$json = @"
{
    '`$schema': 'https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#',
    'contentVersion': '1.0.0.0',
    'resources': [
        {
            'name': '$AutomationAccountName/$certificateName',
            'type': 'Microsoft.Automation/automationAccounts/certificates',
            'apiVersion': '2015-10-31',
            'properties': {
                'base64Value': '$Base64Value',
                'thumbprint': '$Thumbprint',
                'isExportable': true
            }
        }
    ]
}
"@

$json | out-file .\template.json
New-AzResourceGroupDeployment -Name NewCert -ResourceGroupName TestAzureAuto -TemplateFile .\template.json
```

## <a name="using-a-certificate"></a>Uso de un certificado

Para usar un certificado, use la actividad `Get-AutomationCertificate`. No puede usar el cmdlet [Get-AzAutomationCertificate](https://docs.microsoft.com/powershell/module/Az.Automation/Get-AzAutomationCertificate?view=azps-3.7.0), debido a que devuelve información sobre el recurso de certificado, pero no el certificado en sí.

### <a name="textual-runbook-example"></a>Ejemplo de un runbook textual

En el ejemplo siguiente se muestra cómo agregar un certificado a un servicio en la nube en un runbook. En este ejemplo, la contraseña se recupera a partir de una variable de automatización cifrada.

```powershell-interactive
$serviceName = 'MyCloudService'
$cert = Get-AutomationCertificate -Name 'MyCertificate'
$certPwd = Get-AzAutomationVariable -ResourceGroupName "ResourceGroup01" `
–AutomationAccountName "MyAutomationAccount" –Name 'MyCertPassword'
Add-AzureCertificate -ServiceName $serviceName -CertToDeploy $cert
```

### <a name="graphical-runbook-example"></a>Ejemplos de un runbook gráfico

Para agregar una actividad `Get-AutomationCertificate` a un runbook gráfico, haga clic con el botón derecho en el certificado en el panel de biblioteca y seleccione **Agregar al lienzo**.

![Adición del certificado al lienzo](../media/certificates/automation-certificate-add-to-canvas.png)

La imagen siguiente muestra un ejemplo de cómo usar un certificado en un runbook gráfico. Este es el mismo ejemplo anteriormente mostrado para agregar un certificado a un servicio en la nube desde un runbook textual.

![Creación gráfica de ejemplo](../media/certificates/graphical-runbook-add-certificate.png)

### <a name="python-2-example"></a>Ejemplo de Python 2

En el ejemplo siguiente se muestra cómo acceder a los certificados en runbooks de Python2.

```python
# get a reference to the Azure Automation certificate
cert = automationassets.get_automation_certificate("AzureRunAsCertificate")

# returns the binary cert content  
print cert
```

## <a name="next-steps"></a>Pasos siguientes

- Para obtener más información sobre cómo trabajar con vínculos para controlar el flujo lógico de las actividades que el runbook realiza, consulte [Vínculos en la creación gráfica](../automation-graphical-authoring-intro.md#links-and-workflow). 
