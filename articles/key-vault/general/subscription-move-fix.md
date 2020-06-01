---
title: 'Cambio del identificador de inquilino del almacén de claves después de mover una suscripción: Azure Key Vault | Microsoft Docs'
description: Aprenda a cambiar el identificador de inquilino de Key Vault después de que una suscripción se haya movido a otro inquilino
services: key-vault
author: amitbapat
manager: rkarlin
tags: azure-resource-manager
ms.service: key-vault
ms.subservice: general
ms.topic: tutorial
ms.date: 08/12/2019
ms.author: ambapat
ms.openlocfilehash: 9e35f5c9288860056a910f54f9601b2178a628bb
ms.sourcegitcommit: 0b80a5802343ea769a91f91a8cdbdf1b67a932d3
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/25/2020
ms.locfileid: "83828090"
---
# <a name="change-a-key-vault-tenant-id-after-a-subscription-move"></a>Cambio del identificador de inquilino de Key Vault después de mover una suscripción

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]


Cuando se crea un Key Vault nuevo en una suscripción, se asocia automáticamente a un identificador de inquilino de Azure Active Directory predeterminado de dicha suscripción. Las entradas de la directiva de acceso también se asocian con este identificador de inquilino. 

Al mover una suscripción de Azure del inquilino A al inquilino B, las entidades de servicio (usuarios y aplicaciones) de este último inquilino no pueden acceder a las instancias de Key Vault existentes. Para corregir este problema, es preciso:

* Cambiar el identificador de inquilino asociado a todas las instancias de Key Vault existentes en la suscripción al inquilino B.
* Quitar todas las entradas de la directiva de acceso existentes.
* Agregar nuevas entradas de la directiva de acceso asociadas al inquilino B.

Por ejemplo, si tiene el almacén de claves "myvault" en una suscripción que se ha movido del inquilino A al inquilino B, puede usar Azure PowerShell para cambiar el identificador del inquilino y quitar las directivas de acceso antiguas.

```azurepowershell
Select-AzSubscription -SubscriptionId <your-subscriptionId>                # Select your Azure Subscription
$vaultResourceId = (Get-AzKeyVault -VaultName myvault).ResourceId          # Get your key vault's Resource ID 
$vault = Get-AzResource –ResourceId $vaultResourceId -ExpandProperties     # Get the properties for your key vault
$vault.Properties.TenantId = (Get-AzContext).Tenant.TenantId               # Change the Tenant that your key vault resides in
$vault.Properties.AccessPolicies = @()                                     # Access policies can be updated with real
                                                                           # applications/users/rights so that it does not need to be                             # done after this whole activity. Here we are not setting 
                                                                           # any access policies. 
Set-AzResource -ResourceId $vaultResourceId -Properties $vault.Properties  # Modifies the key vault's properties.
````

O bien, puede usar la CLI de Azure.

```azurecli
az account set -s <your-subscriptionId>                                    # Select your Azure Subscription
tenantId=$(az account show --query tenantId)                               # Get your tenantId
az keyvault update -n myvault --remove Properties.accessPolicies           # Remove the access policies
az keyvault update -n myvault --set Properties.tenantId=$tenantId          # Update the key vault tenantId
```

Ahora que el almacén está asociado al identificador de inquilino correcto y que se han quitado las entradas de directiva de acceso antiguas, establezca nuevas entradas de directiva de acceso con el cmdlet [Set-AzKeyVaultAccessPolicy](/powershell/module/az.keyvault/Set-azKeyVaultAccessPolicy) de Azure PowerShell o el comando [az keyvault set-policy](/cli/azure/keyvault?view=azure-cli-latest#az-keyvault-set-policy) de la CLI de Azure.

Si usa una identidad administrada para los recursos de Azure, tendrá que actualizarla también al nuevo inquilino de Azure AD. Para más información sobre las identidades administradas, consulte [Autenticación de Key Vault con una identidad administrada](managed-identity.md).


Si usa MSI, también tendrá que actualizar la identidad de MSI, ya que la anterior ya no estará en el inquilino de AAD correcto.

## <a name="next-steps"></a>Pasos siguientes

Si le queda alguna duda acerca de Azure Key Vault, visite la [página de preguntas más frecuentes de Microsoft para Azure Key Vault](https://docs.microsoft.com/answers/topics/azure-key-vault.html).
