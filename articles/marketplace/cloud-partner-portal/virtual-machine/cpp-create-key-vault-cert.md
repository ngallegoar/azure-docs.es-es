---
title: Creación de un certificado de Azure Key Vault | Azure Marketplace
description: Se explica cómo registrar una máquina virtual desde un disco duro virtual implementado en Azure.
author: dsindona
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 11/29/2018
ms.author: dsindona
ms.openlocfilehash: 9981f8eda174bbe04b54933528d20d270d360824
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/28/2020
ms.locfileid: "82148255"
---
# <a name="create-certificates-for-azure-key-vault"></a>Creación de certificados para Azure Key Vault

> [!IMPORTANT]
> A partir del 13 de abril de 2020, comenzaremos el traslado de la administración de las ofertas de máquinas virtuales de Azure al Centro de partners. Después de la migración, las ofertas se crearán y administrarán en el Centro de partners. Siga las instrucciones que se indican en [Certificación de imágenes de máquina virtual de Azure](https://docs.microsoft.com/azure/marketplace/partner-center-portal/azure-vm-image-certification) para administrar las ofertas migradas.

En este artículo se explica cómo aprovisionar los certificados autofirmados requeridos para establecer una conectividad con Administración remota de Windows (WinRM) a una máquina virtual (VM) hospedada en Azure. Este proceso consta de tres pasos:

1.    Cree el certificado de seguridad. 
2.    Cree la instancia de Azure Key Vault para almacenar este certificado. 
3.    Almacene los certificados en este almacén de claves. 

Puede usar un grupo de recursos de Azure nuevo o existente para este trabajo.  El enfoque anterior se usa en la explicación siguiente.



[!INCLUDE [updated-for-az](../../../../includes/updated-for-az.md)]

## <a name="create-the-certificate"></a>Creación del certificado

Edite y ejecute el siguiente script de Azure PowerShell para crear el archivo de certificado (.pfx) en una carpeta local.  Tendrá que reemplazar los valores para los parámetros siguientes:

|  **Parámetro**        |   **Descripción**                                                               |
|  -------------        |   ---------------                                                               |
| `$certroopath` | Carpeta local donde guardar el archivo .pfx  |
| `$location`    | Una de las ubicaciones geográficas estándares de Azure  |
| `$vmName`      | Nombre de la máquina virtual de Azure planeada   |
| `$certname`    | Nombre del certificado; debe coincidir con el nombre de dominio completo de la VM planeada  |
| `$certpassword` | Contraseña para los certificados; debe coincidir con la contraseña usada para la VM planeada  |
|  |  |

```powershell
    # Certification creation script 
    
    # pfx certification stored path
    $certroopath = "C:\certLocation"
    
    #location of the resource group
    $location = "westus"
    
    # Azure virtual machine name that we are going to create
    $vmName = "testvm000000906"
    
    # Certification name - should match with FQDN of Windows Azure creating VM 
    $certname = "$vmName.$location.cloudapp.azure.com"
    
    # Certification password - should be match with password of Windows Azure creating VM 
    $certpassword = "SecretPassword@123"
    
    $cert=New-SelfSignedCertificate -DnsName "$certname" -CertStoreLocation cert:\LocalMachine\My
    $pwd = ConvertTo-SecureString -String $certpassword -Force -AsPlainText
    $certwithThumb="cert:\localMachine\my\"+$cert.Thumbprint
    $filepath="$certroopath\$certname.pfx"
    Export-PfxCertificate -cert $certwithThumb -FilePath $filepath -Password $pwd
    Remove-Item -Path $certwithThumb 

```
> [!TIP]
> Mantenga la misma sesión de consola de PowerShell activa durante estos pasos para que se conserven los valores de los distintos parámetros.

> [!WARNING]
> Si guarda este script, almacénelo solo en una ubicación segura, ya que contiene información de seguridad (una contraseña).


## <a name="create-the-key-vault"></a>Creación del almacén de claves

Copie el contenido de la [plantilla de implementación de Key Vault](./cpp-key-vault-deploy-template.md) en un archivo en el equipo local (en el siguiente script de ejemplo, este recurso es `C:\certLocation\keyvault.json`).  Edite y ejecute el siguiente script de Azure PowerShell para crear una instancia de Azure Key Vault y el grupo de recursos asociado.  Tendrá que reemplazar los valores para los parámetros siguientes:

|  **Parámetro**        |   **Descripción**                                                               |
|  -------------        |   ---------------                                                               |
| `$postfix`            | Cadena numérica arbitraria anexada a identificadores de implementación                     |
| `$rgName`             | Nombre del grupo de recursos (RG) de Azure para crear                                        |
|  `$location`          | Una de las ubicaciones geográficas estándares de Azure                                  |
| `$kvTemplateJson`     | Ruta de acceso del archivo (keyvault.json) que contiene la plantilla de Resource Manager para el almacén de claves |
| `$kvname`             | Nombre del nuevo almacén de claves                                                       |
|  |  |

```powershell
    # Creating Key vault in resource group
    
    # "Random" number for deployment identifiers
    $postfix = "0101048"
    
    # Resource group name
    $rgName = "TestRG$postfix"
    
    # Location of Resource Group
    $location = "westus"
    
    # Key vault template location
    $kvTemplateJson = "C:\certLocation\keyvault.json"
    
    # Key vault name
    $kvname = "isvkv$postfix"
    
    # code snippet to get the Azure user object ID
    try
       {
        $accounts = Get-AzureAccount
        $accountNum = 0
        $accounts.Id | %{ ++$accountNum; Write-Host $accountNum $_}
        Write-Host "`nPlease select User, e.g. 1:" -ForegroundColor DarkYellow
        [Int] $accountChoice = Read-Host 
        
        While($accountChoice -lt 1 -or $accountChoice -gt $accounts.Length)
        {
            Write-Host "incorrect input" -ForegroundColor Red 
            Write-Host "`nPlease select User, e.g. 1:" -ForegroundColor DarkYellow
            [Int] $accountChoice = Read-Host
        }
        
        $accountSelected = $accounts[$accountChoice-1]
        echo $accountSelected
        $id = $accountSelected.Id
                              
        Write-Host "User $id Selected"
        $myobjectId=(Get-AzADUser -Mail $id)[0].Id
      }
      catch
      {
      Write-Host $_.Exception.Message    
      Break
      } 

    # code snippet to get Azure User Tenant Id
      # SELECT Subscriptions
        #**************************************
        try
        {
        $subslist=Get-AzureSubscription
        for($i=1; $i -le $subslist.Length;$i++)
        {
           Write-Host ($i.ToString() +":"+ $subslist[$i-1].SubscriptionName)
        }
        Write-Host "`nPlease pick subscription from above, e.g. 1:" -ForegroundColor DarkYellow
        [int] $selectedsub=Read-Host
    
        While($selectedsub -lt 1 -or $selectedsub -gt $subslist.Length)
        {
            Write-Host "incorrect input" -ForegroundColor Red 
            for($i=1; $i -le $subslist.Length;$i++)
             {
              Write-Host ($i.ToString() +":"+ $subslist[$i-1].SubscriptionName)
             }
            Write-Host "`nPlease pick subscription from above, e.g. 1:" -ForegroundColor DarkYellow
           [int] $selectedsub=Read-Host
        }
        if($selectedsub -ge 1 -and $selectedsub -le $subslist.Length)
        {
        $mysubid=$subslist[$selectedsub-1].SubscriptionId
        $mysubName=$subslist[$selectedsub-1].SubscriptionName
        $mytenantId=$subslist[$selectedsub-1].TenantId
        Write-Host "$mysubName selected"
        }
        }
        catch
        {
        Write-Host $_.Exception.Message    
        Break
        }
    
    # Create a resource group
     Write-Host "Creating Resource Group $rgName"
     Create-ResourceGroup -rgName $rgName -location $location
     Write-Host "-----------------------------------" 
    
    # Create key vault and configure access
    New-AzResourceGroupDeployment -Name "kvdeploy$postfix" -ResourceGroupName $rgName -TemplateFile $kvTemplateJson -keyVaultName $kvname -tenantId $mytenantId -objectId $myobjectId
    
    Set-AzKeyVaultAccessPolicy -VaultName $kvname -ObjectId $myobjectId -PermissionsToKeys all -PermissionsToSecrets all 
        
```

## <a name="store-the-certificate"></a>Almacenamiento del certificado

Ahora puede almacenar los certificados, contenidos en el archivo .pfx, en el nuevo almacén de claves mediante el script siguiente. 

```powershell
    #push certificate to key vault secret

     $fileName =$certroopath+"\$certname"+".pfx" 
      
     $fileContentBytes = get-content $fileName -Encoding Byte
     $fileContentEncoded = [System.Convert]::ToBase64String($fileContentBytes)
    
            $jsonObject = @"
    {
    "data": "$filecontentencoded",
    "dataType" :"pfx",
    "password": "$certpassword"
    }
"@
            echo $certpassword
            $jsonObjectBytes = [System.Text.Encoding]::UTF8.GetBytes($jsonObject)
            $jsonEncoded = [System.Convert]::ToBase64String($jsonObjectBytes)
            $secret = ConvertTo-SecureString -String $jsonEncoded -AsPlainText -Force
            $objAzureKeyVaultSecret=Set-AzureKeyVaultSecret -VaultName $kvname -Name "ISVSecret$postfix" -SecretValue $secret
            echo $objAzureKeyVaultSecret.Id 
    
```


## <a name="next-steps"></a>Pasos siguientes

A continuación podrá [implementar una VM desde su imagen de VM de usuario](./cpp-deploy-vm-user-image.md).
