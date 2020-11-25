---
title: Pasar credenciales a Azure mediante Desired State Configuration
description: Aprenda a pasar credenciales de forma segura a máquinas virtuales de Azure con Desired State Configuration (DSC) de PowerShell.
services: virtual-machines-windows
documentationcenter: ''
author: bobbytreed
manager: carmonm
editor: ''
tags: azure-resource-manager
keywords: dsc
ms.assetid: ea76b7e8-b576-445a-8107-88ea2f3876b9
ms.service: virtual-machines-windows
ms.subservice: extensions
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: na
ms.date: 05/02/2018
ms.author: robreed
ms.openlocfilehash: f191ab5819cd69ce90699db60be1665ef77e10a9
ms.sourcegitcommit: cd9754373576d6767c06baccfd500ae88ea733e4
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/20/2020
ms.locfileid: "94955895"
---
# <a name="pass-credentials-to-the-azure-dscextension-handler"></a>Pasar credenciales al controlador de extensiones DSC de Azure

En este artículo se trata la extensión Desired State Configuration (DSC) de Azure. Para ver información general sobre el controlador de extensiones de DSC, consulte [Introducción al controlador de extensiones de configuración de Azure Desired State](dsc-overview.md).

 

## <a name="pass-in-credentials"></a>Pase de credenciales

Como parte del proceso de configuración, es posible que deba configurar las cuentas de usuario, obtener acceso a los servicios o instalar un programa en un contexto de usuario. Para llevar a cabo estas acciones, debe proporcionar las credenciales.

Puede usar DSC para establecer configuraciones parametrizadas. En una configuración parametrizada, las credenciales se pasan a la configuración y se almacenan de forma segura en archivos .mof. El controlador de extensiones de Azure simplifica la administración de credenciales, ya que administra los certificados de forma automática.

El siguiente script de configuración de DSC crea una cuenta de usuario local con la contraseña especificada:

```powershell
configuration Main
{
    param(
        [Parameter(Mandatory=$true)]
        [ValidateNotNullorEmpty()]
        [PSCredential]
        $Credential
    )
    Node localhost {
        User LocalUserAccount
        {
            Username = $Credential.UserName
            Password = $Credential
            Disabled = $false
            Ensure = "Present"
            FullName = "Local User Account"
            Description = "Local User Account"
            PasswordNeverExpires = $true
        }
    }
}
```

Es importante incluir **node localhost** como parte de la configuración. El controlador de extensiones busca específicamente la instrucción **node localhost**. Si falta esta instrucción, los siguientes pasos no funcionarán. También es importante incluir la conversión de tipo **[PsCredential]** . Este tipo específico desencadena la extensión para cifrar la credencial.

Para publicar este script en Azure Blob Storage:

`Publish-AzVMDscConfiguration -ConfigurationPath .\user_configuration.ps1`

Para establecer la extensión DSC de Azure y proporcionar la credencial:

```powershell
$configurationName = 'Main'
$configurationArguments = @{ Credential = Get-Credential }
$configurationArchive = 'user_configuration.ps1.zip'
$vm = Get-AzVM -Name 'example-1'

$vm = Set-AzVMDscExtension -VMName $vm -ConfigurationArchive $configurationArchive -ConfigurationName $configurationName -ConfigurationArgument @configurationArguments

$vm | Update-AzVM
```

## <a name="how-a-credential-is-secured"></a>Cómo se protege una credencial

Al ejecutar este código, se solicita una credencial. Después de proporcionar la credencial, se almacena brevemente en la memoria. Cuando la credencial se publica usando el cmdlet **Set-AzVMDscExtension**, esta se transmite a través de HTTPS a la VM. En la máquina virtual, Azure almacena la credencial cifrada en el disco mediante el certificado de máquina virtual local. La credencial se descifra brevemente en la memoria y se vuelve a cifrar para pasarla a DSC.

Este proceso no es el mismo que cuando se [usan configuraciones seguras sin el controlador de extensiones](/powershell/scripting/dsc/pull-server/securemof). El entorno de Azure ofrece una manera de transmitir datos de configuración de forma segura mediante certificados. Cuando utiliza el controlador de la extensión DSC, no es necesario proporcionar **$CertificatePath** o una entrada **$CertificateID**/  **$Thumbprint** en **ConfigurationData**.

## <a name="next-steps"></a>Pasos siguientes

- Obtenga una [introducción al controlador de extensiones DSC de Azure](dsc-overview.md).
- Examine la [plantilla de Azure Resource Manager para la extensión de DSC](dsc-template.md).
- Para obtener más información sobre DSC de PowerShell, vaya al [centro de documentación de PowerShell](/powershell/scripting/dsc/overview/overview).
- Para obtener más funcionalidades que pueda administrar con DSC de PowerShell y más recursos de DSC, examine la [Galería de PowerShell](https://www.powershellgallery.com/packages?q=DscResource&x=0&y=0).