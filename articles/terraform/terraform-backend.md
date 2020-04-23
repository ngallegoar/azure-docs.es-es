---
title: 'Tutorial: Almacenamiento del estado de Terraform en Azure Storage'
description: Introducción al almacenamiento del estado de Terraform en Azure Storage.
ms.topic: tutorial
ms.date: 11/07/2019
ms.openlocfilehash: c94c66969c517bd0e51117ab7c6ed6a889149518
ms.sourcegitcommit: b55d7c87dc645d8e5eb1e8f05f5afa38d7574846
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/16/2020
ms.locfileid: "81459022"
---
# <a name="tutorial-store-terraform-state-in-azure-storage"></a>Tutorial: Estado de Terraform en Azure Storage

El estado de Terraform se usa para conciliar los recursos implementados con configuraciones de Terraform. El estado permite que Terraform sepa qué recursos de Azure debe agregar, actualizar o eliminar. De forma predeterminada, el estado de Terraform se almacena localmente cuando se ejecuta el comando `terraform apply`. Esta configuración no es ideal por los siguientes motivos:

- El estado local no funciona bien en un equipo ni en un entorno de colaboración.
- El estado de Terraform puede incluir información confidencial.
- Almacenar el estado localmente aumenta la posibilidad de eliminación accidental.

Terraform admite la persistencia del estado en el almacenamiento remoto. Uno de estos back-end compatible es Azure Storage. En este documento se muestra cómo configurar y usar Azure Storage con este fin.

## <a name="configure-storage-account"></a>Configurar la cuenta de almacenamiento

Para poder usar Azure Storage como back-end, es preciso crear antes una cuenta de almacenamiento. La cuenta de almacenamiento puede crearse con Azure Portal, PowerShell, la CLI de Azure o Terraform. Use el ejemplo siguiente para configurar la cuenta de almacenamiento con la CLI de Azure.

```azurecli
#!/bin/bash

RESOURCE_GROUP_NAME=tstate
STORAGE_ACCOUNT_NAME=tstate$RANDOM
CONTAINER_NAME=tstate

# Create resource group
az group create --name $RESOURCE_GROUP_NAME --location eastus

# Create storage account
az storage account create --resource-group $RESOURCE_GROUP_NAME --name $STORAGE_ACCOUNT_NAME --sku Standard_LRS --encryption-services blob

# Get storage account key
ACCOUNT_KEY=$(az storage account keys list --resource-group $RESOURCE_GROUP_NAME --account-name $STORAGE_ACCOUNT_NAME --query [0].value -o tsv)

# Create blob container
az storage container create --name $CONTAINER_NAME --account-name $STORAGE_ACCOUNT_NAME --account-key $ACCOUNT_KEY

echo "storage_account_name: $STORAGE_ACCOUNT_NAME"
echo "container_name: $CONTAINER_NAME"
echo "access_key: $ACCOUNT_KEY"
```

Anote el nombre de la cuenta de almacenamiento, el nombre del contenedor y la clave de acceso de almacenamiento. Estos valores son necesarios al configurar el estado remoto.

## <a name="configure-state-back-end"></a>Configuración del back-end de estado

El back-end de estado de Terraform se configura al ejecutar el comando `terraform init`. Para configurar el back-end de estado, se requieren los siguientes datos:

- **storage_account_name**: nombre de la cuenta de Azure Storage.
- **container_name**: nombre del contenedor de blobs.
- **key**: nombre del archivo de almacenamiento de estados que se va a crear.
- **access_key**: clave de acceso de almacenamiento.

Cada uno de estos valores se puede especificar en el archivo de configuración de Terraform o en la línea de comandos. Se recomienda usar una variable de entorno para el valor `access_key`. El uso de una variable de entorno evita que la clave se escriba en el disco.

Cree una variable de entorno denominada `ARM_ACCESS_KEY` con el valor de la clave de acceso de Azure Storage.

```bash
export ARM_ACCESS_KEY=<storage access key>
```

Para proteger aún más la clave de acceso de la cuenta de Azure Storage, almacénela en Azure Key Vault. A continuación, se puede establecer la variable de entorno mediante el uso de un comando similar al siguiente. Para más información sobre Azure Key Vault, consulte la [documentación de Azure Key Vault](../key-vault/secrets/quick-create-cli.md).

```bash
export ARM_ACCESS_KEY=$(az keyvault secret show --name terraform-backend-key --vault-name myKeyVault --query value -o tsv)
```

Para configurar Terraform para que use el back-end, deben realizarse los siguientes pasos:
- Incluya un bloque de configuración de `backend` con un tipo de `azurerm`.
- Agregue el valor `storage_account_name` al bloque de configuración.
- Agregue el valor `container_name` al bloque de configuración.
- Agregue el valor `key` al bloque de configuración.

En el ejemplo siguiente se configura un back-end de Terraform y se crea un grupo de recursos de Azure.

```hcl
terraform {
  backend "azurerm" {
    resource_group_name   = "tstate"
    storage_account_name  = "tstate09762"
    container_name        = "tstate"
    key                   = "terraform.tfstate"
  }
}

resource "azurerm_resource_group" "state-demo-secure" {
  name     = "state-demo"
  location = "eastus"
}
```

Inicialice el archivo de configuración, para lo que debe seguir estos pasos:

1. Ejecute el comando `terraform init`.
1. Ejecute el comando `terraform apply`.

Ya puede encontrar el archivo de estado en Azure Storage Blob.

## <a name="state-locking"></a>Estado bloqueado

Los blobs de Azure Storage Blob se bloquean automáticamente antes de cualquier operación que escriba el estado. Este patrón evita operaciones de estado simultáneas que pueden causar daños. 

Para más información, consulte el artículo acerca del [bloqueo del estado](https://www.terraform.io/docs/state/locking.html) en la documentación de Terraform.

El bloqueo se puede ver al examinar el blob desde Azure Portal, o cualquier otra herramienta de administración de Azure.

![Blob de Azure con bloqueo](media/terraform-backend/lock.png)

## <a name="encryption-at-rest"></a>Cifrado en reposo

Los datos almacenados en un blob de Azure se cifran antes de enviarse. Cuando es necesario, Terraform recupera el estado del back-end y lo almacena en la memoria local. Con este patrón, el estado nunca se escribe en el disco local.

Para más información acerca del cifrado de Azure Storage, consulte [Cifrado de Azure Storage para datos en reposo](../storage/common/storage-service-encryption.md).

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"] 
> [Más información sobre el uso de Terraform en Azure](/azure/terraform)
