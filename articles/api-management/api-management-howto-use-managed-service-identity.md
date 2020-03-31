---
title: Uso de identidades administradas en Azure API Management | Microsoft Docs
description: Aprenda a usar las identidades administradas en API Management.
services: api-management
documentationcenter: ''
author: miaojiang
manager: anneta
editor: ''
ms.service: api-management
ms.workload: integration
ms.topic: article
ms.date: 10/18/2017
ms.author: apimpm
ms.openlocfilehash: 49576b805e6c6d01340e663bfb5d8e9013917625
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "79226596"
---
# <a name="use-managed-identities-in-azure-api-management"></a>Uso de identidades administradas en Azure API Management

En este artículo se muestra cómo crear una identidad administrada para una instancia de servicio de API Management y cómo acceder a otros recursos. Una identidad administrada generada por Azure Active Directory (Azure AD) permite a la instancia de API Management acceder de forma fácil y segura a otros recursos protegidos de Azure AD, como Azure Key Vault. Esta identidad está administrada por Azure y no requiere que aprovisione o rote los secretos. Para más información sobre las identidades administradas, consulte el artículo sobre [qué son las identidades administradas para recursos de Azure](../active-directory/managed-identities-azure-resources/overview.md).

## <a name="create-a-managed-identity-for-an-api-management-instance"></a>Creación de una identidad administrada para una instancia de API Management

### <a name="using-the-azure-portal"></a>Uso de Azure Portal

Para configurar una identidad administrada en el portal, primero tendrá que crear una instancia de API Management como lo hace normalmente y, a continuación, habilitar la característica.

1. Cree una instancia de API Management en el portal como lo haría normalmente. Navegue hasta el portal.
2. Seleccione **Identidades de servicio administradas**.
3. Cambie Registrar en Azure Active Directory a Activado. Haga clic en Guardar.

![Habilitación de MSI](./media/api-management-msi/enable-msi.png)

### <a name="using-the-azure-resource-manager-template"></a>Uso de la plantilla de Azure Resource Manager

Puede crear una instancia de API Management con una identidad mediante la inclusión de la siguiente propiedad en la definición de recursos:

```json
"identity" : {
    "type" : "SystemAssigned"
}
```

Esto indica a Azure que debe crear y administrar la identidad para la instancia de API Management.

Por ejemplo, una plantilla de Azure Resource Manager completa podría tener el aspecto siguiente:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2014-04-01-preview/deploymentTemplate.json#",
    "contentVersion": "0.9.0.0",
    "resources": [{
        "apiVersion": "2017-03-01",
        "name": "contoso",
        "type": "Microsoft.ApiManagement/service",
        "location": "[resourceGroup().location]",
        "tags": {},
        "sku": {
            "name": "Developer",
            "capacity": "1"
        },
        "properties": {
            "publisherEmail": "admin@contoso.com",
            "publisherName": "Contoso"
        },
        "identity": {
            "type": "systemAssigned"
        }
    }]
}
```
## <a name="use-the-managed-service-identity-to-access-other-resources"></a>Uso de la identidad de servicio administrada para acceder a otros recursos

> [!NOTE]
> Actualmente, las identidades administradas se pueden utilizar para obtener certificados de Azure Key Vault para los nombres de dominio personalizados de API Management. Pronto se admitirán más escenarios.
>
>


### <a name="obtain-a-certificate-from-azure-key-vault"></a>Obtención de un certificado en Azure Key Vault

#### <a name="prerequisites"></a>Prerequisites
1. El almacén de claves que contiene los certificados pfx debe estar en la misma suscripción de Azure y el mismo grupo de recursos que el servicio API Management. Se trata de un requisito de la plantilla de Azure Resource Manager.
2. El tipo de contenido del secreto debe ser *application/x-pkcs12*. Puede usar el siguiente script para cargar el certificado:

```powershell
$pfxFilePath = "PFX_CERTIFICATE_FILE_PATH" # Change this path 
$pwd = "PFX_CERTIFICATE_PASSWORD" # Change this password 
$flag = [System.Security.Cryptography.X509Certificates.X509KeyStorageFlags]::Exportable 
$collection = New-Object System.Security.Cryptography.X509Certificates.X509Certificate2Collection 
$collection.Import($pfxFilePath, $pwd, $flag) 
$pkcs12ContentType = [System.Security.Cryptography.X509Certificates.X509ContentType]::Pkcs12 
$clearBytes = $collection.Export($pkcs12ContentType) 
$fileContentEncoded = [System.Convert]::ToBase64String($clearBytes) 
$secret = ConvertTo-SecureString -String $fileContentEncoded -AsPlainText –Force 
$secretContentType = 'application/x-pkcs12' 
Set-AzureKeyVaultSecret -VaultName KEY_VAULT_NAME -Name KEY_VAULT_SECRET_NAME -SecretValue $Secret -ContentType $secretContentType
```

> [!Important]
> Si no se proporciona la versión del objeto del certificado, API Management obtiene automáticamente la versión más reciente del certificado una vez cargado a Key Vault.

En el ejemplo siguiente se muestra una plantilla de Azure Resource Manager que contiene los siguientes pasos:

1. Creación de una instancia de API Management con una identidad administrada.
2. Actualización de las directivas de acceso de una instancia de Azure Key Vault y permiso para que la instancia de API Management obtenga secretos de ella.
3. Actualización de la instancia de API Management estableciendo un nombre de dominio personalizado mediante un certificado de la instancia de Key Vault.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "publisherEmail": {
            "type": "string",
            "minLength": 1,
            "metadata": {
                "description": "The email address of the owner of the service"
            }
        },
        "publisherName": {
            "type": "string",
            "defaultValue": "Contoso",
            "minLength": 1,
            "metadata": {
                "description": "The name of the owner of the service"
            }
        },
        "sku": {
            "type": "string",
            "allowedValues": ["Developer",
            "Standard",
            "Premium"],
            "defaultValue": "Developer",
            "metadata": {
                "description": "The pricing tier of this API Management service"
            }
        },
        "skuCount": {
            "type": "int",
            "defaultValue": 1,
            "metadata": {
                "description": "The instance size of this API Management service."
            }
        },
        "keyVaultName": {
            "type": "string",
            "metadata": {
                "description": "Name of the vault"
            }
        },
        "proxyCustomHostname1": {
            "type": "string",
            "metadata": {
                "description": "Proxy Custom hostname."
            }
        },
        "keyVaultIdToCertificate": {
            "type": "string",
            "metadata": {
                "description": "Reference to the KeyVault certificate. https://contoso.vault.azure.net/secrets/contosogatewaycertificate."
            }
        }
    },
    "variables": {
        "apiManagementServiceName": "[concat('apiservice', uniqueString(resourceGroup().id))]",
        "apimServiceIdentityResourceId": "[concat(resourceId('Microsoft.ApiManagement/service', variables('apiManagementServiceName')),'/providers/Microsoft.ManagedIdentity/Identities/default')]"
    },
    "resources": [{
        "apiVersion": "2017-03-01",
        "name": "[variables('apiManagementServiceName')]",
        "type": "Microsoft.ApiManagement/service",
        "location": "[resourceGroup().location]",
        "tags": {
        },
        "sku": {
            "name": "[parameters('sku')]",
            "capacity": "[parameters('skuCount')]"
        },
        "properties": {
            "publisherEmail": "[parameters('publisherEmail')]",
            "publisherName": "[parameters('publisherName')]"
        },
        "identity": {
            "type": "systemAssigned"
        }
    },
    {
        "type": "Microsoft.KeyVault/vaults/accessPolicies",
        "name": "[concat(parameters('keyVaultName'), '/add')]",
        "apiVersion": "2015-06-01",
        "dependsOn": [
            "[resourceId('Microsoft.ApiManagement/service', variables('apiManagementServiceName'))]"
        ],
        "properties": {
            "accessPolicies": [{
                "tenantId": "[reference(variables('apimServiceIdentityResourceId'), '2015-08-31-PREVIEW').tenantId]",
                "objectId": "[reference(variables('apimServiceIdentityResourceId'), '2015-08-31-PREVIEW').principalId]",
                "permissions": {
                    "secrets": ["get"]
                }
            }]
        }
    },
    {
        "apiVersion": "2017-05-10",
        "name": "apimWithKeyVault",
        "type": "Microsoft.Resources/deployments",
        "dependsOn": [
        "[resourceId('Microsoft.ApiManagement/service', variables('apiManagementServiceName'))]"
        ],
        "properties": {
            "mode": "incremental",
            "templateLink": {
                "uri": "https://raw.githubusercontent.com/solankisamir/arm-templates/master/basicapim.keyvault.json",
                "contentVersion": "1.0.0.0"
            },
            "parameters": {
                "publisherEmail": { "value": "[parameters('publisherEmail')]"},
                "publisherName": { "value": "[parameters('publisherName')]"},
                "sku": { "value": "[parameters('sku')]"},
                "skuCount": { "value": "[parameters('skuCount')]"},
                "proxyCustomHostname1": {"value" : "[parameters('proxyCustomHostname1')]"},
                "keyVaultIdToCertificate": {"value" : "[parameters('keyVaultIdToCertificate')]"}
            }
        }
    }]
}
```

## <a name="next-steps"></a>Pasos siguientes

Obtenga más información sobre las identidades administradas para recursos de Azure:

* [¿Qué son las identidades administradas de recursos de Azure?](../active-directory/managed-identities-azure-resources/overview.md)
* [Plantillas del Administrador de recursos de Azure](https://github.com/Azure/azure-quickstart-templates)
* [Autenticación con una identidad administrada en una directiva](./api-management-authentication-policies.md#ManagedIdentity)
