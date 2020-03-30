---
title: Cifrado de discos en una máquina virtual de Windows en Azure
description: Cifrado de discos virtuales en una máquina virtual Windows con Azure PowerShell
services: virtual-machines-windows
documentationcenter: ''
author: cynthn
manager: gwallace
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machines-windows
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 10/30/2018
ms.author: cynthn
ms.openlocfilehash: 096fe779c077424b01df3ead5965cb799866a03b
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "74033537"
---
# <a name="encrypt-virtual-disks-on-a-windows-vm"></a>Cifrado de discos virtuales en una máquina virtual Windows
Para mejorar la seguridad y el cumplimiento de las máquinas virtuales, se pueden cifrar los discos virtuales en Azure. Los discos se cifran mediante claves criptográficas que están protegidas en Azure Key Vault. Estas claves criptográficas se pueden controlar y se puede auditar su uso. En este artículo se describe cómo cifrar los discos virtuales en una máquina virtual Windows con Azure PowerShell. También puede [cifrar máquinas virtuales Linux](../linux/disk-encryption-overview.md).

 

## <a name="overview-of-disk-encryption"></a>Introducción al cifrado de discos
Los discos virtuales en VM Windows se cifran en reposo mediante BitLocker. El cifrado de los discos virtuales en Azure no conlleva ningún cargo. Las claves criptográficas se almacenan en Azure Key Vault con protección de software, o puede importar o generar las claves en módulos de seguridad de hardware (HSM) certificados conforme a las normas FIPS 140-2 de nivel 2. Las claves criptográficas se usan para cifrar y descifrar los discos virtuales conectados a la máquina virtual. El usuario tiene el control de estas claves criptográficas y se puede auditar su uso. 

El proceso para cifrar una máquina virtual es el siguiente:

1. Cree una clave criptográfica en Azure Key Vault.
1. Configure la clave criptográfica para poder utilizarla para el cifrado de discos.
1. Habilite el cifrado de disco para los discos virtuales.
1. Las claves criptográficas necesarias se solicitan desde Azure Key Vault.
1. Los discos virtuales se cifran con la clave criptográfica proporcionada.


## <a name="requirements-and-limitations"></a>Requisitos y limitaciones

Requisitos y escenarios admitidos para el cifrado de discos:

* Habilitación del cifrado en nuevas máquinas virtuales Windows desde imágenes de Azure Marketplace o una imagen de disco duro virtual personalizada.
* Habilitación del cifrado en máquinas virtuales de Windows existentes en Azure.
* Habilitación del cifrado en máquinas virtuales Windows configuradas mediante Espacios de almacenamiento.
* Deshabilitación del cifrado en las unidades de datos y del sistema operativo en máquinas virtuales de Windows.
* Nivel estándar de máquinas virtuales, como las máquinas virtuales de la serie A, D, DS, G y GS.

    > [!NOTE]
    > Todos los recursos (incluyendo Key Vault, cuenta de Storage, máquina virtual, etc.) tienen que pertenecer a la misma región y suscripción de Azure.

El cifrado del disco no se admite actualmente en los siguientes escenarios:

* Máquina s virtuales de nivel básico
* Máquinas virtuales creadas con el modelo de implementación clásica.
* Actualización de las claves criptográficas en una máquina virtual ya cifrada.
* Integración con el Servicio de administración de claves local.


## <a name="create-an-azure-key-vault-and-keys"></a>Creación de Azure Key Vault y claves
Antes de empezar, asegúrese de tener instalada la versión más reciente del módulo de Azure PowerShell. Para obtener más información, consulte [Instalación y configuración de Azure PowerShell](/powershell/azure/overview). En los siguientes ejemplos de comandos, reemplace todos los parámetros de ejemplo con sus propios datos de nombres, ubicación y valores de clave, tales como *myResourceGroup*, *myKeyVault*, *myVM* y demás.

El primer paso es crear un almacén de Azure Key Vault para almacenar las claves criptográficas. Azure Key Vault puede almacenar claves, secretos o contraseñas que permiten su implementación segura en las aplicaciones y los servicios. Para el cifrado de discos virtuales, se crea una instancia de Key Vault para almacenar una clave criptográfica que se usa para cifrar o descifrar los discos virtuales. 

Habilite el proveedor de Azure Key Vault dentro de la suscripción de Azure con [Register-AzResourceProvider](https://docs.microsoft.com/powershell/module/az.resources/register-azresourceprovider) y, después, cree un grupo de recursos con [New-AzResourceGroup](https://docs.microsoft.com/powershell/module/az.resources/new-azresourcegroup). En el ejemplo siguiente, se crea un grupo de recursos denominado *myResourceGroup* en la ubicación del *este de EE. UU.* :

```azurepowershell-interactive
$rgName = "myResourceGroup"
$location = "East US"

Register-AzResourceProvider -ProviderNamespace "Microsoft.KeyVault"
New-AzResourceGroup -Location $location -Name $rgName
```

El almacén de Azure Key Vault que contiene las claves criptográficas y los recursos de proceso asociados, como el almacenamiento y la propia máquina virtual, tienen que encontrarse en la misma región. Cree una instancia de Azure Key Vault con [New-AzKeyVault](https://docs.microsoft.com/powershell/module/az.keyvault/new-azkeyvault) y habilite Key Vault para usarlo con el cifrado de discos. Especifique un nombre de Key Vault único para *keyVaultName* de la siguiente manera:

```azurepowershell-interactive
$keyVaultName = "myKeyVault$(Get-Random)"
New-AzKeyVault -Location $location `
    -ResourceGroupName $rgName `
    -VaultName $keyVaultName `
    -EnabledForDiskEncryption
```

Puede almacenar las claves criptográficas utilizando protección de software o de módulo de seguridad de hardware (HSM).  Una instancia estándar de Key Vault solo almacena claves protegidas por software. Para usar HSM se necesita una instancia premium de Key Vault con un costo adicional. Para crear una instancia premium de Key Vault, en el paso anterior agregue el parámetro *-Sku "Premium"* . En el ejemplo siguiente se usa claves protegidas por software ya que hemos creado un almacén de Key Vault estándar. 

En ambos modelos de protección, la plataforma Windows Azure debe tener acceso para solicitar las claves criptográficas cuando la máquina virtual arranca para descifrar los discos virtuales. Cree una clave criptográfica en la instancia de Key Vault con [Add-AzureKeyVaultKey](https://docs.microsoft.com/powershell/module/az.keyvault/add-azkeyvaultkey). En el ejemplo siguiente se crea una clave llamada *myKey*:

```azurepowershell-interactive
Add-AzKeyVaultKey -VaultName $keyVaultName `
    -Name "myKey" `
    -Destination "Software"
```

## <a name="create-a-virtual-machine"></a>Creación de una máquina virtual
Para probar el proceso de cifrado, cree una máquina virtual con [New-AzVm](https://docs.microsoft.com/powershell/module/az.compute/new-azvm). En el ejemplo siguiente se crea una máquina virtual llamada *myVM* mediante una imagen de *Windows Server 2016 Datacenter*. Cuando le solicite las credenciales, escriba el nombre de usuario y la contraseña que se usan para la máquina virtual:

```azurepowershell-interactive
$cred = Get-Credential

New-AzVm `
    -ResourceGroupName $rgName `
    -Name "myVM" `
    -Location $location `
    -VirtualNetworkName "myVnet" `
    -SubnetName "mySubnet" `
    -SecurityGroupName "myNetworkSecurityGroup" `
    -PublicIpAddressName "myPublicIpAddress" `
    -Credential $cred
```


## <a name="encrypt-a-virtual-machine"></a>Cifrado de una máquina virtual
Cifre la máquina virtual con [Set-AzVMDiskEncryptionExtension](https://docs.microsoft.com/powershell/module/az.compute/set-azvmdiskencryptionextension) mediante la clave de Azure Key Vault. En el ejemplo siguiente se recupera toda la información de clave y después se cifra la máquina virtual denominada *myVM*:

```azurepowershell-interactive
$keyVault = Get-AzKeyVault -VaultName $keyVaultName -ResourceGroupName $rgName;
$diskEncryptionKeyVaultUrl = $keyVault.VaultUri;
$keyVaultResourceId = $keyVault.ResourceId;
$keyEncryptionKeyUrl = (Get-AzKeyVaultKey -VaultName $keyVaultName -Name myKey).Key.kid;

Set-AzVMDiskEncryptionExtension -ResourceGroupName $rgName `
    -VMName "myVM" `
    -DiskEncryptionKeyVaultUrl $diskEncryptionKeyVaultUrl `
    -DiskEncryptionKeyVaultId $keyVaultResourceId `
    -KeyEncryptionKeyUrl $keyEncryptionKeyUrl `
    -KeyEncryptionKeyVaultId $keyVaultResourceId
```

Acepte el mensaje para continuar con el cifrado de la máquina virtual. La VM se reinicia durante el proceso. Una vez que finalice el proceso de cifrado y se reinicie la máquina virtual, revise el estado del cifrado con [Get-AzVmDiskEncryptionStatus](https://docs.microsoft.com/powershell/module/az.compute/get-azvmdiskencryptionstatus):

```azurepowershell-interactive
Get-AzVmDiskEncryptionStatus  -ResourceGroupName $rgName -VMName "myVM"
```

La salida es similar a la del ejemplo siguiente:

```azurepowershell-interactive
OsVolumeEncrypted          : Encrypted
DataVolumesEncrypted       : Encrypted
OsVolumeEncryptionSettings : Microsoft.Azure.Management.Compute.Models.DiskEncryptionSettings
ProgressMessage            : OsVolume: Encrypted, DataVolumes: Encrypted
```

## <a name="next-steps"></a>Pasos siguientes
* Para más información sobre cómo administrar Azure Key Vault, consulte [Configuración de Key Vault para máquinas virtuales](key-vault-setup.md).
* Para obtener más información acerca del cifrado de discos, por ejemplo, cómo preparar una máquina virtual personalizada cifrada para cargar en Azure, consulte [Azure Disk Encryption](../../security/fundamentals/encryption-overview.md).
