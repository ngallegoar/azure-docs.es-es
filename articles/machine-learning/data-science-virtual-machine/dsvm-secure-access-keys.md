---
title: Almacenamiento seguro de credenciales de acceso
titleSuffix: Azure Data Science Virtual Machine
description: Obtenga información acerca de cómo almacenar las credenciales de acceso en Data Science Virtual Machine de forma segura. Aprenderá a usar las identidades de servicio administradas y Azure Key Vault para almacenar las credenciales de acceso.
keywords: aprendizaje profundo, AI, herramientas de ciencia de datos, data science virtual machine, análisis geoespacial, proceso de ciencia de datos en equipo
services: machine-learning
ms.service: machine-learning
ms.subservice: data-science-vm
author: vijetajo
ms.author: vijetaj
ms.topic: conceptual
ms.date: 05/08/2018
ms.openlocfilehash: 1cb0c5094d49eac5a1c8f63406a28d2927d8fa94
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "79477330"
---
# <a name="store-access-credentials-securely-on-an-azure-data-science-virtual-machine"></a>Almacenamiento seguro de las credenciales de acceso en Azure Data Science Virtual Machine

Es habitual que el código de las aplicaciones en la nube contenga las credenciales de autenticación en los servicios en la nube. La administración y la protección de estas credenciales es un reto conocido para la compilación de aplicaciones en la nube. Lo ideal sería que las credenciales nunca aparecieran en las estaciones de trabajo de los desarrolladores o que se controlaran en el código fuente.

La característica de las [identidades administradas para los recursos de Azure](https://docs.microsoft.com/azure/active-directory/managed-service-identity/overview) facilita la resolución de este problema, al ofrecer a los servicios de Azure una identidad administrada automáticamente en Azure Active Directory (Azure AD). Puede usar esta identidad para autenticarse en cualquier servicio que admita la autenticación de Azure AD, sin necesidad de tener credenciales en el código.

Una manera de proteger las credenciales es usar Windows Installer (MSI) en combinación con [Azure Key Vault](https://docs.microsoft.com/azure/key-vault/), un servicio de Azure administrado para almacenar las claves criptográficas y los secretos de forma segura. Puede acceder a un almacén de claves mediante la identidad administrada y recuperar los secretos autorizados y las claves criptográficas de este.

Las documentación sobre las identidades administradas para los recursos de Azure y Key Vault es un recurso integral de información detallada sobre estos servicios. El resto de este artículo ofrece una guía sobre el uso básico de MSI y Key Vault en Data Science Virtual Machine (DSVM) para acceder a los recursos de Azure. 

## <a name="create-a-managed-identity-on-the-dsvm"></a>Creación de una identidad administrada en DSVM

```azurecli-interactive
# Prerequisite: You have already created a Data Science VM in the usual way.

# Create an identity principal for the VM.
az vm assign-identity -g <Resource Group Name> -n <Name of the VM>
# Get the principal ID of the DSVM.
az resource list -n <Name of the VM> --query [*].identity.principalId --out tsv
```

## <a name="assign-key-vault-access-permissions-to-a-vm-principal"></a>Asignación de permisos de acceso a Key Vault a una entidad de seguridad de máquina virtual

```azurecli-interactive
# Prerequisite: You have already created an empty Key Vault resource on Azure by using the Azure portal or Azure CLI.

# Assign only get and set permissions but not the capability to list the keys.
az keyvault set-policy --object-id <Principal ID of the DSVM from previous step> --name <Key Vault Name> -g <Resource Group of Key Vault>  --secret-permissions get set
```

## <a name="access-a-secret-in-the-key-vault-from-the-dsvm"></a>Acceso a un secreto de Key Vault desde DSVM

```bash
# Get the access token for the VM.
x=`curl http://localhost:50342/oauth2/token --data "resource=https://vault.azure.net" -H Metadata:true`
token=`echo $x | python -c "import sys, json; print(json.load(sys.stdin)['access_token'])"`

# Access the key vault by using the access token.
curl https://<Vault Name>.vault.azure.net/secrets/SQLPasswd?api-version=2016-10-01 -H "Authorization: Bearer $token"
```

## <a name="access-storage-keys-from-the-dsvm"></a>Acceso a las claves de almacenamiento desde la DSVM

```bash
# Prerequisite: You have granted your VMs MSI access to use storage account access keys based on instructions at https://docs.microsoft.com/azure/active-directory/managed-service-identity/tutorial-linux-vm-access-storage. This article describes the process in more detail.

y=`curl http://localhost:50342/oauth2/token --data "resource=https://management.azure.com/" -H Metadata:true`
ytoken=`echo $y | python -c "import sys, json; print(json.load(sys.stdin)['access_token'])"`
curl https://management.azure.com/subscriptions/<SubscriptionID>/resourceGroups/<ResourceGroup of Storage account>/providers/Microsoft.Storage/storageAccounts/<Storage Account Name>/listKeys?api-version=2016-12-01 --request POST -d "" -H "Authorization: Bearer $ytoken"

# Now you can access the data in the storage account from the retrieved storage account keys.
```

## <a name="access-the-key-vault-from-python"></a>Acceso a Key Vault desde Python

```python
from azure.keyvault import KeyVaultClient
from msrestazure.azure_active_directory import MSIAuthentication

"""MSI Authentication example."""

# Get credentials.
credentials = MSIAuthentication(
    resource='https://vault.azure.net'
)

# Create a Key Vault client.
key_vault_client = KeyVaultClient(
    credentials
)

key_vault_uri = "https://<key Vault Name>.vault.azure.net/"

secret = key_vault_client.get_secret(
    key_vault_uri,  # Your key vault URL.
    # The name of your secret that already exists in the key vault.
    "SQLPasswd",
    ""              # The version of the secret; empty string for latest.
)
print("My secret value is {}".format(secret.value))
```

## <a name="access-the-key-vault-from-azure-cli"></a>Acceso a Key Vault desde la CLI de Azure

```azurecli-interactive
# With managed identities for Azure resources set up on the DSVM, users on the DSVM can use Azure CLI to perform the authorized functions. The following commands enable access to the key vault from Azure CLI without requiring login to an Azure account.
# Prerequisites: MSI is already set up on the DSVM as indicated earlier. Specific permissions, like accessing storage account keys, reading specific secrets, and writing new secrets, are provided to the MSI.

# Authenticate to Azure CLI without requiring an Azure account. 
az login --msi

# Retrieve a secret from the key vault. 
az keyvault secret show --vault-name <Vault Name> --name SQLPasswd

# Create a new secret in the key vault.
az keyvault secret set --name MySecret --vault-name <Vault Name> --value "Helloworld"

# List access keys for the storage account.
az storage account keys list -g <Storage Account Resource Group> -n <Storage Account Name>
```
