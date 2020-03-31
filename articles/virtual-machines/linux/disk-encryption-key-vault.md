---
title: Creación y configuración de un almacén de claves para Azure Disk Encryption
description: En este artículo se proporcionan los pasos necesarios para crear y configurar un almacén de claves para su uso con Azure Disk Encryption
ms.service: virtual-machines-linux
ms.topic: article
author: msmbaldwin
ms.author: mbaldwin
ms.date: 08/06/2019
ms.custom: seodec18
ms.openlocfilehash: a818d9fe9707d1789fbe8e77489fc380fd2c92dd
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "78970624"
---
# <a name="creating-and-configuring-a-key-vault-for-azure-disk-encryption"></a>Creación y configuración de un almacén de claves para Azure Disk Encryption

Azure Disk Encryption usa Azure Key Vault para controlar y administrar las claves y los secretos de cifrado de discos.  Para más información sobre los almacenes de claves, consulte [Introducción a Azure Key Vault](../../key-vault/key-vault-get-started.md) y [Protección de un almacén de claves](../../key-vault/key-vault-secure-your-key-vault.md). 

> [!WARNING]
> - Si ya ha usado Azure Disk Encryption con Azure AD para cifrar una VM, debe seguir usando esta opción para cifrar la VM. Para más información, consulte [Creación y configuración de un almacén de claves para Azure Disk Encryption con Azure AD (versión anterior)](disk-encryption-key-vault-aad.md).

La creación y configuración de un almacén de claves para su uso con Azure Disk Encryption conlleva estos tres pasos:

1. Creación de un grupo de recursos, en caso de que sea necesario.
2. Creación de un almacén de claves. 
3. Establecimiento de directivas de acceso avanzadas del almacén de claves.

Estos pasos se muestran en los siguientes tutoriales de inicio rápido:

- [Creación y cifrado de una máquina virtual Linux con la CLI de Azure](disk-encryption-cli-quickstart.md)
- [Creación y cifrado de una máquina virtual Linux con Azure PowerShell](disk-encryption-cli-quickstart.md)

Si lo desea, también puede generar o importar una clave de cifrado de claves (KEK).

> [!Note]
> Los pasos que se describen en este artículo se automatizan en el [script de la CLI de requisitos previos de Azure Disk Encryption](https://github.com/ejarvi/ade-cli-getting-started) y en el [script de PowerShell de requisitos previos de Azure Disk Encryption](https://github.com/Azure/azure-powershell/tree/master/src/Compute/Compute/Extension/AzureDiskEncryption/Scripts).

## <a name="install-tools-and-connect-to-azure"></a>Instalación de herramientas y conexión a Azure

Los pasos que se describen en este artículo se pueden completar con la [CLI de Azure](/cli/azure/), el [módulo Az de Azure PowerShell](/powershell/azure/overview) o [Azure Portal](https://portal.azure.com). 

Aunque se puede acceder al portal a través de un explorador, tanto la CLI de Azure como Azure PowerShell requieren una instalación local; consulte [Azure Disk Encryption para Linux: instalación de herramientas](disk-encryption-linux.md#install-tools-and-connect-to-azure) para más información.

### <a name="connect-to-your-azure-account"></a>Conexión a la cuenta de Azure

Antes de usar la CLI de Azure o Azure PowerShell, es preciso conectarse a su suscripción de Azure. Para ello, [inicie sesión con la CLI de Azure](/cli/azure/authenticate-azure-cli?view=azure-cli-latest), [inicie sesión con Azure Powershell](/powershell/azure/authenticate-azureps?view=azps-2.5.0) o escriba sus credenciales en Azure Portal cuando se le soliciten.

```azurecli-interactive
az login
```

```azurepowershell-interactive
Connect-AzAccount
```

## <a name="create-a-resource-group"></a>Crear un grupo de recursos

*Si ya tiene un grupo de recursos, puede pasar a [Creación de un almacén de claves](#create-a-key-vault).*

Un grupo de recursos es un contenedor lógico en el que se implementan y se administran los recursos de Azure. 

Cree un grupo de recursos mediante el comando [az group create](/cli/azure/group?view=azure-cli-latest#az-group-create) de la CLI de Azure, el comando [New-AzResourceGroup](/powershell/module/az.resources/new-azresourcegroup) de Azure PowerShell o bien desde [Azure Portal](https://portal.azure.com).

### <a name="azure-cli"></a>Azure CLI

```azurecli-interactive
az group create --name "myResourceGroup" --location eastus
```
### <a name="azure-powershell"></a>Azure PowerShell
```azurepowershell-interactive
New-AzResourceGroup -Name "myResourceGroup" -Location "EastUS"
```

## <a name="create-a-key-vault"></a>Creación de un Almacén de claves

*Si ya tiene un almacén de claves, puede pasar a la sección [Establecimiento de directivas de acceso avanzadas del almacén de claves](#set-key-vault-advanced-access-policies).*

Cree un almacén de claves mediante el comando [az keyvault create](/cli/azure/keyvault?view=azure-cli-latest#az-keyvault-create) de la CLI de Azure, el comando [New-AzKeyvault](/powershell/module/az.keyvault/new-azkeyvault) de Azure Powershell, [Azure Portal](https://portal.azure.com) o una [plantilla de Resource Manager](https://github.com/Azure/azure-quickstart-templates/tree/master/101-key-vault-create).

>[!WARNING]
> Para asegurarse de que los secretos de cifrado no traspasan los límites regionales, Azure Disk Encryption requiere que Key Vault y las máquinas virtuales se encuentren en la misma región. Cree y use un almacén de claves que se encuentre en la misma región que las máquinas virtuales que se van a cifrar. 

Cada instancia de Key Vault debe tener un nombre único. Reemplace <nombre-almacén de claves-único> por el nombre del almacén de claves en los ejemplos siguientes.

### <a name="azure-cli"></a>Azure CLI

Al crear un almacén de claves mediante la CLI de Azure, agregue la marca "--enabled-for-disk-encryption".

```azurecli-interactive
az keyvault create --name "<your-unique-keyvault-name>" --resource-group "myResourceGroup" --location "eastus" --enabled-for-disk-encryption
```

### <a name="azure-powershell"></a>Azure PowerShell

Al crear un almacén de claves mediante Azure PowerShell, agregue la marca "-EnabledForDiskEncryption".

```azurepowershell-interactive
New-AzKeyvault -name "<your-unique-keyvault-name>" -ResourceGroupName "myResourceGroup" -Location "eastus" -EnabledForDiskEncryption
```
### <a name="resource-manager-template"></a>Plantilla de Resource Manager

También puede crear un almacén de claves mediante la [plantilla de Resource Manager](https://github.com/Azure/azure-quickstart-templates/tree/master/101-key-vault-create).

1. En la plantilla de inicio rápido de Azure, haga clic en **Deploy to Azure** (Implementar en Azure).
2. Seleccione la suscripción, el grupo de recursos, la ubicación del grupo de recursos, el nombre del almacén de claves, el identificador de objeto, los términos legales y el contrato, y luego haga clic en **Comprar**. 


##  <a name="set-key-vault-advanced-access-policies"></a>Establecimiento de directivas de acceso avanzadas del almacén de claves

La plataforma Azure necesita acceso a las claves de cifrado o secretos del almacén de claves para ponerlos a disposición de la máquina virtual para el proceso de arranque y descifrado de los volúmenes. 

Si no ha habilitado el almacén de claves para el cifrado de discos, la implementación o la implementación de plantillas en el momento de la creación (como se muestra en el paso anterior), debe actualizar sus directivas de acceso avanzadas.  

### <a name="azure-cli"></a>Azure CLI

Use [az keyvault update](/cli/azure/keyvault#az-keyvault-update) para habilitar el cifrado de disco para el almacén de claves. 

 - **Habilitar Key Vault para el cifrado de disco:** Es necesario Enabled-for-disk-encryption. 

     ```azurecli-interactive
     az keyvault update --name "<your-unique-keyvault-name>" --resource-group "MyResourceGroup" --enabled-for-disk-encryption "true"
     ```  

 - **Habilitar Key Vault para la implementación, si es necesario:** permite que el proveedor de recursos Microsoft.Compute recupere los secretos de este almacén de claves cuando se hace referencia al almacén de claves en la creación de recursos, por ejemplo, cuando se crea una máquina virtual.

     ```azurecli-interactive
     az keyvault update --name "<your-unique-keyvault-name>" --resource-group "MyResourceGroup" --enabled-for-deployment "true"
     ``` 

 - **Habilitar Key Vault para la implementación de plantillas, si es necesario:** permite que Resource Manager recupere secretos del almacén.
     ```azurecli-interactive  
     az keyvault update --name "<your-unique-keyvault-name>" --resource-group "MyResourceGroup" --enabled-for-template-deployment "true"
     ```

###  <a name="azure-powershell"></a>Azure PowerShell
 Use el cmdlet [Set-AzKeyVaultAccessPolicy](/powershell/module/az.keyvault/set-azkeyvaultaccesspolicy) de PowerShell del almacén de claves para habilitar el cifrado de disco para el almacén de claves.

  - **Habilitar Key Vault para el cifrado de disco:** se requiere EnabledForDiskEncryption para el cifrado de Azure Disk.
      
     ```azurepowershell-interactive 
     Set-AzKeyVaultAccessPolicy -VaultName "<your-unique-keyvault-name>" -ResourceGroupName "MyResourceGroup" -EnabledForDiskEncryption
     ```

  - **Habilitar Key Vault para la implementación, si es necesario:** permite que el proveedor de recursos Microsoft.Compute recupere los secretos de este almacén de claves cuando se hace referencia al almacén de claves en la creación de recursos, por ejemplo, cuando se crea una máquina virtual.

     ```azurepowershell-interactive
      Set-AzKeyVaultAccessPolicy -VaultName "<your-unique-keyvault-name>" -ResourceGroupName "MyResourceGroup" -EnabledForDeployment
     ```

  - **Habilitar Key Vault para la implementación de plantillas, si es necesario:** permite que Azure Resource Manager obtenga los secretos de este almacén de claves cuando se hace referencia al almacén de claves en una implementación de plantilla.

     ```azurepowershell-interactive             
     Set-AzKeyVaultAccessPolicy -VaultName "<your-unique-keyvault-name>" -ResourceGroupName "MyResourceGroup" -EnabledForTemplateDeployment
     ```

### <a name="azure-portal"></a>Portal de Azure

1. Seleccione el almacén de claves, vaya a **Directivas de acceso** y **haga clic para mostrar las directivas de acceso avanzadas**.
2. Active la casilla etiquetada **Habilitar el acceso a Azure Disk Encryption para el cifrado de volúmenes**.
3. Seleccione **Habilitar el acceso a Azure Virtual Machines para la implementación** o **Habilitar el acceso a Azure Resource Manager para la implementación de plantillas**, si es necesario. 
4. Haga clic en **Save**(Guardar).

    ![Directivas de acceso avanzado de Azure Key Vault](./media/disk-encryption/keyvault-portal-fig4.png)


## <a name="set-up-a-key-encryption-key-kek"></a>Configuración de una clave de cifrado de claves (KEK)

Si desea usar una clave de cifrado de claves (KEK) para una brindar una capa adicional de seguridad para las claves de cifrado, agregue una KEK a su almacén de claves. Cuando se especifica una clave de cifrado de claves, Azure Disk Encryption usa esa clave para encapsular los secretos de cifrado antes de escribirlos en Key Vault.

Puede generar una nueva clave de cifrado de claves mediante el comando [az keyvault key create](/cli/azure/keyvault/key?view=azure-cli-latest#az-keyvault-key-create) de la CLI de Azure, el cmdlet [Add-AzKeyVaultKey](/powershell/module/az.keyvault/add-azkeyvaultkey) de Azure PowerShell o [Azure Portal](https://portal.azure.com/). Debe generar un tipo de clave RSA; Azure Disk Encryption no admite aún el uso de claves de curva elíptica.

En su lugar, puede importar una clave de cifrado de claves en el HSM de administración de claves local. Para más información, consulte la [documentación de Key Vault](../../key-vault/key-vault-hsm-protected-keys.md). 

Las direcciones URL de la KEK del almacén de claves deben tener versiones. Azure exige esta restricción del control de versiones. Para ver direcciones URL de KEK y de secretos válidas, vea los ejemplos siguientes:

* Ejemplo de dirección URL de secreto válida: *https://contosovault.vault.azure.net/secrets/EncryptionSecretWithKek/xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx*
* Ejemplo de dirección URL de clave de cifrado de claves válida: *https://contosovault.vault.azure.net/keys/diskencryptionkek/xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx*

Azure Disk Encryption no admite la especificación de números de puerto como parte de los secretos del almacén de claves y las direcciones URL de KEK. Para ver ejemplos de direcciones URL de almacén de claves admitidas y no admitidas, consulte los siguientes:

  * Dirección URL de almacén de claves aceptable: *https://contosovault.vault.azure.net/secrets/contososecret/xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx*
  * Dirección URL de almacén de claves inaceptable *https://contosovault.vault.azure.net:443/secrets/contososecret/xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx*

### <a name="azure-cli"></a>Azure CLI

Use el comando [az keyvault key create](/cli/azure/keyvault/key?view=azure-cli-latest#az-keyvault-key-create) de la CLI de Azure para generar una clave de cifrado de claves y almacenarla en el almacén de claves.

```azurecli-interactive
az keyvault key create --name "myKEK" --vault-name "<your-unique-keyvault-name>" --kty RSA-HSM
```

En su lugar, puede importar una clave privada mediante el comando [az keyvault key import](/cli/azure/keyvault/key?view=azure-cli-latest#az-keyvault-key-import) de la CLI de Azure:

En cualquier caso, especificará el nombre de la clave de cifrado de claves al parámetro --key-encryption-key del comando [az vm encryption enable](/cli/azure/vm/encryption?view=azure-cli-latest#az-vm-encryption-enable) de la CLI de Azure. 

```azurecli-interactive
az vm encryption enable -g "MyResourceGroup" --name "myVM" --disk-encryption-keyvault "<your-unique-keyvault-name>" --key-encryption-key "myKEK"
```

###  <a name="azure-powershell"></a>Azure PowerShell 

Use el cmdlet [Add-AzKeyVaultKey](/powershell/module/az.keyvault/add-azkeyvaultkey?view=azps-2.5.0) de Azure PowerShell para generar una clave de cifrado de claves y almacenarla en el almacén de claves.

 ```powershell-interactive
Add-AzKeyVaultKey -Name "myKEK" -VaultName "<your-unique-keyvault-name>" -Destination "HSM"
```

En su lugar, puede importar una clave privada mediante el comando [az keyvault key import](/cli/azure/keyvault/key?view=azure-cli-latest#az-keyvault-key-import) de Azure PowerShell.

En cualquier caso, especificará tanto el identificador del almacén de claves de la clave de cifrado de claves como la dirección URL de la clave de cifrado de claves en los parámetros [Set-AzVMDiskEncryptionExtension](/powershell/module/az.compute/set-azvmdiskencryptionextension?view=azps-2.5.0) -KeyEncryptionKeyVaultId y -KeyEncryptionKeyUrl de Azure PowerShell. Tenga en cuenta que en este ejemplo se asume que se usa el mismo almacén de claves para la clave de cifrado de discos y la clave de cifrado de claves.

 ```powershell-interactive
$KeyVault = Get-AzKeyVault -VaultName "<your-unique-keyvault-name>" -ResourceGroupName "myResourceGroup"
$KEK = Get-AzKeyVaultKey -VaultName "<your-unique-keyvault-name>" -Name "myKEK"

Set-AzVMDiskEncryptionExtension -ResourceGroupName MyResourceGroup -VMName "MyVM" -DiskEncryptionKeyVaultUrl $KeyVault.VaultUri -DiskEncryptionKeyVaultId $KeyVault.ResourceId -KeyEncryptionKeyVaultId $KeyVault.ResourceId -KeyEncryptionKeyUrl $KEK.Id -SkipVmBackup -VolumeType All
 ```
 
## <a name="next-steps"></a>Pasos siguientes

- [Script de la CLI de requisitos previos de Azure Disk Encryption](https://github.com/ejarvi/ade-cli-getting-started)
- [Script de PowerShell de requisitos previos de Azure Disk Encryption](https://github.com/Azure/azure-powershell/tree/master/src/Compute/Compute/Extension/AzureDiskEncryption/Scripts)
- Información acerca de [Escenarios de Azure Disk Encryption en máquinas virtuales Linux](disk-encryption-linux.md)
- Aprenda a [solucionar los problemas de Azure Disk Encryption](disk-encryption-troubleshooting.md)
- Lea los [scripts de ejemplo de Azure Disk Encryption](disk-encryption-sample-scripts.md)
