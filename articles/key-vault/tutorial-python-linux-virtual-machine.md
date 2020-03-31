---
title: 'Tutorial: Uso de una máquina virtual Linux y una aplicación Python para almacenar secretos en Azure Key Vault | Microsoft Docs'
description: En este tutorial, aprenderá a configurar una aplicación Python para leer un secreto de Azure Key Vault.
services: key-vault
author: msmbaldwin
manager: rajvijan
ms.service: key-vault
ms.subservice: secrets
ms.topic: tutorial
ms.date: 09/05/2018
ms.author: mbaldwin
ms.custom: mvc
ms.openlocfilehash: 59b8abf59212d9cfb0719b6b76e9542249ee4c41
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/24/2020
ms.locfileid: "79472697"
---
# <a name="tutorial-use-a-linux-vm-and-a-python-app-to-store-secrets-in-azure-key-vault"></a>Tutorial: Uso de una máquina virtual Linux y una aplicación Python para almacenar secretos en Azure Key Vault

Azure Key Vault ayuda a proteger secretos como las claves de API y las cadenas de conexión de base de datos necesarias para acceder a las aplicaciones, los servicios y los recursos de TI.

En este tutorial, configurará una aplicación web de Azure para leer información de Azure Key Vault mediante identidades administradas para recursos de Azure. Aprenderá a:

> [!div class="checklist"]
> * Creación de un Almacén de claves
> * Almacenar un secreto en el almacén de claves
> * Creación de una máquina virtual con Linux
> * Habilitar una [identidad administrada](../active-directory/managed-identities-azure-resources/overview.md) para la máquina virtual
> * Conceder los permisos necesarios para que la aplicación de consola lea datos del almacén de claves
> * Recuperar un secreto del almacén de claves

Antes de continuar, asegúrese de conocer los [conceptos básicos sobre Key Vault](basic-concepts.md).

## <a name="prerequisites"></a>Prerrequisitos

* [Git](https://git-scm.com/downloads).
* Suscripción a Azure. Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de empezar.
* [CLI de Azure versión 2.0.4 o posterior](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest) o Azure Cloud Shell.

[!INCLUDE [Azure Cloud Shell](../../includes/cloud-shell-try-it.md)]

## <a name="understand-managed-service-identity"></a>Managed Service Identity

Azure Key Vault puede almacenar credenciales de forma segura, con el fin de que estén en el código. Para recuperarlas, deberá autenticarse en Azure Key Vault. Sin embargo, para autenticarse en Key Vault, necesita una credencial. Se trata de un problema clásico de arranque. Mediante Azure y Azure Active Directory (Azure AD), Managed Service Identity (MSI) proporciona una identidad de arranque que permite comenzar fácilmente.

Cuando se habilita MSI para un servicio de Azure como Virtual Machines, App Service o Functions, Azure crea una entidad de servicio para la instancia del servicio en Azure AD. Inserta las credenciales de la entidad de servicio en la instancia del servicio.

![MSI](media/MSI.png)

A continuación, el código llama a un servicio local de metadatos disponible en el recurso de Azure para obtener un token de acceso. El código usa el token de acceso que obtiene del punto de conexión de MSI local para autenticarse en una instancia del servicio Azure Key Vault.

## <a name="sign-in-to-azure"></a>Inicio de sesión en Azure

Para iniciar sesión en Azure mediante la CLI de Azure, escriba:

```azurecli-interactive
az login
```

## <a name="create-a-resource-group"></a>Crear un grupo de recursos

Un grupo de recursos de Azure es un contenedor lógico en el que se implementan y se administran los recursos de Azure.

Cree un grupo de recursos mediante el comando `az group create` en la ubicación Oeste de EE. UU. con el código siguiente. Reemplace `YourResourceGroupName` por el nombre que prefiera.

```azurecli-interactive
# To list locations: az account list-locations --output table
az group create --name "<YourResourceGroupName>" --location "West US"
```

Usará este grupo de recursos a lo largo de este tutorial.

## <a name="create-a-key-vault"></a>Creación de un Almacén de claves

A continuación, creará un almacén de claves en el grupo de recursos creado en el paso anterior. Proporcione la siguiente información:

* Nombre del almacén de claves: el nombre debe ser una cadena de entre 3 y 24 caracteres y solo debe contener estos caracteres: 0-9, a-z, A-Z y guiones (-).
* Nombre del grupo de recursos.
* Ubicación: **Oeste de EE. UU.**

```azurecli-interactive
az keyvault create --name "<YourKeyVaultName>" --resource-group "<YourResourceGroupName>" --location "West US"
```

En este momento, su cuenta de Azure es la única autorizada para realizar operaciones en este nuevo almacén.

## <a name="add-a-secret-to-the-key-vault"></a>Incorporación de un secreto al almacén de claves

Estamos agregando un secreto para ayudar a ilustrar cómo funciona. Podría querer almacenar una cadena de conexión de SQL o cualquier otra información que necesite mantener segura y disponible para la aplicación.

Escriba los siguientes comandos para crear un secreto en el almacén de claves denominado *AppSecret*. Este secreto almacenará el valor **MySecret**.

```azurecli-interactive
az keyvault secret set --vault-name "<YourKeyVaultName>" --name "AppSecret" --value "MySecret"
```

## <a name="create-a-linux-virtual-machine"></a>Creación de una máquina virtual con Linux

Cree una máquina virtual con el comando `az vm create`.

En el ejemplo siguiente se crea una máquina virtual llamada **myVM** y se agrega una cuenta de usuario llamada **azureuser**. El parámetro `--generate-ssh-keys` genera automáticamente una clave SSH y la coloca en la ubicación de la clave predeterminada ( **~/.ssh**). Para crear un conjunto de claves concreto, utilice la opción `--ssh-key-value`.

```azurecli-interactive
az vm create \
  --resource-group myResourceGroup \
  --name myVM \
  --image UbuntuLTS \
  --admin-username azureuser \
  --generate-ssh-keys
```

La creación de la máquina virtual y los recursos auxiliares tarda unos minutos en realizarse. En la salida de ejemplo siguiente se muestra que la creación de la máquina virtual se realizó correctamente:

```output
{
  "fqdns": "",
  "id": "/subscriptions/<guid>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachines/myVM",
  "location": "westus",
  "macAddress": "00-00-00-00-00-00",
  "powerState": "VM running",
  "privateIpAddress": "XX.XX.XX.XX",
  "publicIpAddress": "XX.XX.XXX.XXX",
  "resourceGroup": "myResourceGroup"
}
```

Anote su propio valor de `publicIpAddress` en la salida de la máquina virtual. Esta dirección se utiliza para acceder a la VM en los pasos posteriores.

## <a name="assign-an-identity-to-the-vm"></a>Asignación de una identidad a la máquina virtual

Cree una identidad asignada por el sistema para la máquina virtual; para ello, ejecute el siguiente comando:

```azurecli-interactive
az vm identity assign --name <NameOfYourVirtualMachine> --resource-group <YourResourceGroupName>
```

La salida del comando es la siguiente.

```output
{
  "systemAssignedIdentity": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
  "userAssignedIdentities": {}
}
```

Anote el `systemAssignedIdentity`. Se usa en el paso siguiente.

## <a name="give-the-vm-identity-permission-to-key-vault"></a>Concesión del permiso de identidad de máquina virtual al almacén de claves

Ahora puede dar permiso de Key Vault a la identidad que creó. Ejecute el siguiente comando:

```azurecli-interactive
az keyvault set-policy --name '<YourKeyVaultName>' --object-id <VMSystemAssignedIdentity> --secret-permissions get list
```

## <a name="log-in-to-the-vm"></a>Inicio de sesión en la máquina virtual

Ahora inicie sesión en la máquina virtual mediante un terminal.

```terminal
ssh azureuser@<PublicIpAddress>
```

## <a name="install-python-library-on-the-vm"></a>Instalación de la biblioteca de Python en la máquina virtual

Descargue e instale la biblioteca Python de [solicitudes](https://pypi.org/project/requests/2.7.0/) Python para realizar llamadas HTTP GET.

## <a name="create-edit-and-run-the-sample-python-app"></a>Creación, edición y ejecución de la aplicación Python de ejemplo

Cree un archivo de Python llamado **Sample.py**.

Abra Sample.py y modifíquelo para que contenga el código siguiente:

```python
# importing the requests library
import requests

# Step 1: Fetch an access token from an MSI-enabled Azure resource      
# Note that the resource here is https://vault.azure.net for the public cloud, and api-version is 2018-02-01
MSI_ENDPOINT = "http://169.254.169.254/metadata/identity/oauth2/token?api-version=2018-02-01&resource=https%3A%2F%2Fvault.azure.net"
r = requests.get(MSI_ENDPOINT, headers = {"Metadata" : "true"})

# Extracting data in JSON format 
# This request gets an access token from Azure Active Directory by using the local MSI endpoint
data = r.json()

# Step 2: Pass the access token received from the previous HTTP GET call to the key vault
KeyVaultURL = "https://prashanthwinvmvault.vault.azure.net/secrets/RandomSecret?api-version=2016-10-01"
kvSecret = requests.get(url = KeyVaultURL, headers = {"Authorization": "Bearer " + data["access_token"]})

print(kvSecret.json()["value"])
```

El código anterior lleva a cabo un proceso de dos pasos:

   1. Captura un token del punto de conexión MSI local en la máquina virtual. El punto de conexión captura a continuación un token de Azure Active Directory.
   1. Pasa el token al almacén de claves y captura el secreto.

Ejecute el siguiente comando: Debería ver el valor del secreto.

```console
python Sample.py
```

En este tutorial, ha obtenido información sobre cómo usar Azure Key Vault con una aplicación Python que se ejecuta en una máquina virtual Linux.

## <a name="clean-up-resources"></a>Limpieza de recursos

Elimine el grupo de recursos, la máquina virtual y todos los recursos relacionados Cuando ya no los necesite. Para ello, seleccione el grupo de recursos de la máquina virtual y seleccione **Eliminar**.

Elimine el almacén de claves mediante el comando `az keyvault delete`:

```azurecli-interactive
az keyvault delete --name
                   [--resource-group]
                   [--subscription]
```

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [API REST de Azure Key Vault](https://docs.microsoft.com/rest/api/keyvault/)
