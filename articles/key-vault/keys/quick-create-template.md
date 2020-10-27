---
title: 'Inicio rápido de Azure: Creación de un almacén de claves de Azure y una clave mediante una plantilla de Azure Resource Manager | Microsoft Docs'
description: Inicio rápido que muestra cómo crear almacenes de claves de Azure y agregar claves a los almacenes mediante una plantilla de Azure Resource Manager.
services: key-vault
author: sebansal
tags: azure-resource-manager
ms.service: key-vault
ms.subservice: keys
ms.topic: quickstart
ms.custom: mvc,subject-armqs
ms.date: 10/14/2020
ms.author: sebansal
ms.openlocfilehash: 55641dacf8f7efb18b479dd4b4253787df540341
ms.sourcegitcommit: 33368ca1684106cb0e215e3280b828b54f7e73e8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/16/2020
ms.locfileid: "92132455"
---
# <a name="quickstart-create-an-azure-key-vault-and-a-key-by-using-arm-template-preview"></a>Inicio rápido: Creación de un almacén de claves de Azure y una clave mediante una plantilla de Resource Manager (versión preliminar)

[Azure Key Vault](../general/overview.md) es un servicio en la nube que proporciona un almacén seguro para secretos, como claves, contraseñas y certificados, entre otros. Este inicio rápido se centra en el proceso de implementar una plantilla de Azure Resource Manager para crear un almacén de claves y una clave.

## <a name="prerequisites"></a>Requisitos previos

Para completar este artículo:

* Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de empezar.

* La plantilla necesita el identificador de objeto de usuario de Azure AD para configurar los permisos. El procedimiento siguiente permite obtener el identificador de objeto (GUID).

    1. Ejecute el siguiente comando de Azure PowerShell o la CLI de Azure, para lo que debe seleccionar **Pruébelo** y, a continuación, pegar el script en el panel de shell. Para pegar el script, haga clic con el botón derecho en el shell y, a continuación, seleccione **Pegar** .

        # <a name="cli"></a>[CLI](#tab/CLI)
        ```azurecli-interactive
        echo "Enter your email address that is used to sign in to Azure:" &&
        read upn &&
        az ad user show --id $upn --query "objectId" &&
        echo "Press [ENTER] to continue ..."
        ```

        # <a name="powershell"></a>[PowerShell](#tab/PowerShell)
        ```azurepowershell-interactive
        $upn = Read-Host -Prompt "Enter your email address used to sign in to Azure"
        (Get-AzADUser -UserPrincipalName $upn).Id
        Write-Host "Press [ENTER] to continue..."
        ```

        ---

    2. Anote el identificador de objeto. Lo necesitará en la sección siguiente de este inicio rápido.

## <a name="review-the-template"></a>Revisión de la plantilla

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "vaultName": {
            "type": "string",
            "metadata": {
                "description": "The name of the key vault to be created."
            }
        },
        "skuName": {
            "type": "string",
            "defaultValue": "Standard",
            "allowedValues": [
                "Standard",
                "Premium"
            ],
            "metadata": {
                "description": "The SKU of the vault to be created."
            }
        },
        "keyName": {
            "type": "string",
            "metadata": {
                "description": "The name of the key to be created."
            }
        },
        "keyType": {
            "type": "string",
            "metadata": {
                "description": "The JsonWebKeyType of the key to be created."
            }
        },
        "keyOps": {
            "type": "array",
            "defaultValue": [],
            "metadata": {
                "description": "The permitted JSON web key operations of the key to be created."
            }
        },
        "keySize": {
            "type": "int",
            "defaultValue": -1,
            "metadata": {
                "description": "The size in bits of the key to be created."
            }
        },
        "curveName": {
            "type": "string",
            "defaultValue": "",
            "metadata": {
                "description": "The JsonWebKeyCurveName of the key to be created."
            }
        }
    },
    "resources": [
        {
            "type": "Microsoft.KeyVault/vaults",
            "name": "[parameters('vaultName')]",
            "apiVersion": "2019-09-01",
            "location": "[resourceGroup().location]",
            "properties": {
                "enableRbacAuthorization": false,
                "enableSoftDelete": false,
                "enabledForDeployment": false,
                "enabledForDiskEncryption": false,
                "enabledForTemplateDeployment": false,
                "tenantId": "[subscription().tenantId]",
                "accessPolicies": [],
                "sku": {
                    "name": "[parameters('skuName')]",
                    "family": "A"
                },
                "networkAcls": {
                    "defaultAction": "Allow",
                    "bypass": "AzureServices"
                }
            }
        },
        {
            "type": "Microsoft.KeyVault/vaults/keys",
            "name": "[concat(parameters('vaultName'), '/', parameters('keyName'))]",
            "apiVersion": "2019-09-01",
            "location": "[resourceGroup().location]",
            "dependsOn": [
                "[resourceId('Microsoft.KeyVault/vaults', parameters('vaultName'))]"
            ],
            "properties": {
                "kty": "[parameters('keyType')]",
                "keyOps": "[parameters('keyOps')]",
                "keySize": "[if(equals(parameters('keySize'), -1), json('null'), parameters('keySize'))]",
                "curveName": "[parameters('curveName')]"
            }
        }
    ],
    "outputs": {
        "proxyKey": {
            "type": "object",
            "value": "[reference(resourceId('Microsoft.KeyVault/vaults/keys', parameters('vaultName'), parameters('keyName')))]"
        }
    }
}

```

Puede encontrar más ejemplos de plantillas de Azure Key Vault en [Plantillas de inicio rápido de Azure](https://azure.microsoft.com/resources/templates/?resourceType=Microsoft.Keyvault&pageNumber=1&sort=Popular).


## <a name="review-deployed-resources"></a>Revisión de los recursos implementados

Puede usar Azure Portal para comprobar el almacén de claves y la clave o utilizar el siguiente script de la CLI de Azure o de Azure PowerShell para enumerar la clave que ha creado.

# <a name="cli"></a>[CLI](#tab/CLI)

```azurecli-interactive
echo "Enter your key vault name:" &&
read keyVaultName &&
az keyvault key list --vault-name $keyVaultName &&
echo "Press [ENTER] to continue ..."
```

# <a name="powershell"></a>[PowerShell](#tab/PowerShell)

```azurepowershell-interactive
$keyVaultName = Read-Host -Prompt "Enter your key vault name"
Get-AzKeyVaultKey -vaultName $keyVaultName
Write-Host "Press [ENTER] to continue..."
```

---

## <a name="clean-up-resources"></a>Limpieza de recursos

Otras guías de inicio rápido y tutoriales de Key Vault se basan en esta. Si tiene pensado seguir trabajando en otras guías de inicio rápido y tutoriales, considere la posibilidad de dejar estos recursos activos.
Cuando ya no lo necesite, elimine el grupo de recursos; de este modo se eliminarán también Key Vault y los recursos relacionados. Para eliminar el grupo de recursos mediante la CLI de Azure o Azure PowerShell:

# <a name="cli"></a>[CLI](#tab/CLI)

```azurecli-interactive
echo "Enter the Resource Group name:" &&
read resourceGroupName &&
az group delete --name $resourceGroupName &&
echo "Press [ENTER] to continue ..."
```

# <a name="powershell"></a>[PowerShell](#tab/PowerShell)

```azurepowershell-interactive
$resourceGroupName = Read-Host -Prompt "Enter the Resource Group name"
Remove-AzResourceGroup -Name $resourceGroupName
Write-Host "Press [ENTER] to continue..."
```

---

## <a name="next-steps"></a>Pasos siguientes

En este inicio rápido, ha creado un almacén de claves y una clave mediante una plantilla de Resource Manager y ha validado la implementación. Para más información sobre Key Vault y Azure Resource Manager, continúe con los artículos siguientes.

- Lea una [introducción a Azure Key Vault](../general/overview.md).
- Obtenga más información sobre [Azure Resource Manager](../../azure-resource-manager/management/overview.md).
- Consulte los [procedimientos recomendados de Azure Key Vault](../general/best-practices.md).
