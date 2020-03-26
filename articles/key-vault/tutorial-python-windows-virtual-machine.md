---
title: 'Tutorial: Uso de Azure Key Vault con una máquina virtual Windows en Python | Microsoft Docs'
description: En este tutorial, va a configurar una aplicación ASP.NET Core para que lea un secreto de su almacén de claves.
services: key-vault
author: msmbaldwin
manager: rajvijan
ms.service: key-vault
ms.subservice: general
ms.topic: tutorial
ms.date: 09/05/2018
ms.author: mbaldwin
ms.custom: mvc
ms.openlocfilehash: c0bb391348548ecca595fd1a6472bafcb22ed4ee
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/24/2020
ms.locfileid: "79472663"
---
# <a name="tutorial-use-azure-key-vault-with-a-windows-virtual-machine-in-python"></a>Tutorial: Uso de Azure Key Vault con una máquina virtual Windows en Python

Azure Key Vault ayuda a proteger secretos como las claves de API, las cadenas de conexión de base de datos necesarias para acceder a las aplicaciones, los servicios y los recursos de TI.

En este tutorial, aprenderá cómo obtener una aplicación de consola para leer información de Azure Key Vault. Para ello, use identidades administradas para recursos de Azure. 

En este tutorial se muestra cómo realizar las siguientes acciones:

> [!div class="checklist"]
> * Cree un almacén de claves.
> * Agregue un secreto al almacén de claves.
> * Recuperar un secreto del almacén de claves.
> * Cree una máquina virtual de Azure.
> * Habilite una entidad administrada.
> * Asigne permisos a la identidad de máquina virtual.

Antes de empezar, lea los [conceptos básicos de Key Vault](basic-concepts.md). 

Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).

## <a name="prerequisites"></a>Prerrequisitos

Para Windows, Mac y Linux:
  * [Git](https://git-scm.com/downloads)
  * Este tutorial requiere que se ejecute localmente la CLI de Azure. Debe tener instalada la versión 2.0.4 de la CLI de Azure o una versión posterior. Ejecute `az --version` para encontrar la versión. Si necesita instalarla o actualizarla, consulte [Instalación de la CLI de Azure 2.0](/cli/azure/install-azure-cli).

## <a name="about-managed-service-identity"></a>Acerca de Managed Service Identity

Azure Key Vault almacena las credenciales de forma segura, de modo que no se muestran en el código. Sin embargo, debe autenticarse en Azure Key Vault para recuperar las claves. Para autenticarse en Key Vault, necesita una credencial. Se trata de un dilema de arranque clásico. Managed Service Identity (MSI) soluciona este problema al proporcionar una _identidad de arranque_ que simplifica el proceso.

Al habilitar MSI para un servicio de Azure, como Azure Virtual Machines, Azure App Service o Azure Functions, Azure crea una [entidad de servicio](basic-concepts.md). MSI hace esto para la instancia del servicio en Azure Active Directory (Azure AD) e inserta las credenciales de la entidad de servicio en esa instancia. 

![MSI](media/MSI.png)

A continuación, para obtener un token de acceso, el código llama a un servicio de metadatos local disponible en el recurso de Azure. Para autenticarse en una instancia del servicio Azure Key Vault, el código usa el token de acceso que obtiene del punto de conexión de MSI local. 

## <a name="log-in-to-azure"></a>Inicio de sesión en Azure

Para iniciar sesión en Azure mediante la CLI de Azure, escriba:

```azurecli
az login
```

## <a name="create-a-resource-group"></a>Crear un grupo de recursos

Un grupo de recursos de Azure es un contenedor lógico en el que se implementan y se administran los recursos de Azure.

Cree un grupo de recursos con el comando [az group create](/cli/azure/group#az-group-create). 

Seleccione el nombre de un grupo de recursos y rellene el marcador de posición. En el siguiente ejemplo se crea un grupo de recursos en la ubicación Oeste de EE. UU:

```azurecli
# To list locations: az account list-locations --output table
az group create --name "<YourResourceGroupName>" --location "West US"
```

Usará el grupo de recursos que acaba de crear en este tutorial.

## <a name="create-a-key-vault"></a>Creación de un Almacén de claves

Para crear un almacén de claves en el grupo de recursos que creó en el paso anterior, proporcione la siguiente información:

* Nombre del almacén de claves: una cadena de entre 3 y 24 caracteres que puede contener solo números (0-9), letras (a-z, A-Z) y guiones (-).
* Definición de un nombre de grupo de recursos
* Ubicación: **Oeste de EE. UU.**

```azurecli
az keyvault create --name "<YourKeyVaultName>" --resource-group "<YourResourceGroupName>" --location "West US"
```
En este momento, su cuenta de Azure es la única autorizada para realizar operaciones en este nuevo almacén de claves.

## <a name="add-a-secret-to-the-key-vault"></a>Incorporación de un secreto al almacén de claves

Estamos agregando un secreto para ayudar a ilustrar cómo funciona. El secreto podría ser una cadena de conexión SQL o cualquier otra información que necesite mantener segura y disponible para la aplicación.

Para crear un secreto en el almacén de claves denominado **AppSecret**, escriba el siguiente comando:

```azurecli
az keyvault secret set --vault-name "<YourKeyVaultName>" --name "AppSecret" --value "MySecret"
```

Este secreto almacena el valor **MySecret**.

## <a name="create-a-virtual-machine"></a>Creación de una máquina virtual
Puede crear una máquina virtual mediante uno de los métodos siguientes:

* [La CLI de Azure](../virtual-machines/windows/quick-create-cli.md)
* [PowerShell](../virtual-machines/windows/quick-create-powershell.md)
* [Portal de Azure](../virtual-machines/windows/quick-create-portal.md)

## <a name="assign-an-identity-to-the-vm"></a>Asignación de una identidad a la máquina virtual
En este paso, va a crear una identidad asignada por el sistema para la máquina virtual mediante la ejecución del siguiente comando en la CLI de Azure:

```azurecli
az vm identity assign --name <NameOfYourVirtualMachine> --resource-group <YourResourceGroupName>
```

Tenga en cuenta la identidad asignada por el sistema que se muestra en el código siguiente. La salida del comando anterior sería: 

```output
{
  "systemAssignedIdentity": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
  "userAssignedIdentities": {}
}
```

## <a name="assign-permissions-to-the-vm-identity"></a>Asignación de permisos a la identidad de máquina virtual
Ahora puede asignar los permisos de la identidad creada anteriormente al almacén de claves mediante la ejecución del comando siguiente:

```azurecli
az keyvault set-policy --name '<YourKeyVaultName>' --object-id <VMSystemAssignedIdentity> --secret-permissions get list
```

## <a name="log-on-to-the-virtual-machine"></a>Iniciar sesión en la nueva máquina virtual

Para iniciar sesión en la máquina virtual, siga las instrucciones de [Conexión a una máquina virtual de Azure donde se ejecuta Windows e inicio de sesión en ella](../virtual-machines/windows/connect-logon.md).

## <a name="create-and-run-a-sample-python-app"></a>Creación y ejecución de una aplicación de Python de ejemplo

En la siguiente sección, se muestra un archivo de ejemplo denominado *Sample.py*. Usa una biblioteca [requests](https://2.python-requests.org/en/master/) para realizar llamadas HTTP GET.

## <a name="edit-samplepy"></a>Edición de Sample.py

Después de crear *Sample.py*, abra el archivo y, después, copie el código en esta sección. 

El código presenta un proceso de dos pasos:
1. Capturar un token del punto de conexión MSI local en la máquina virtual.  
  Si lo hace, también captura un token de Azure AD.
1. Pase el token al almacén de claves y, después, capture el secreto. 

```python
    # importing the requests library 
    import requests 

    # Step 1: Fetch an access token from a Managed Identity enabled azure resource.      
    # Note that the resource here is https://vault.azure.net for public cloud and api-version is 2018-02-01
    MSI_ENDPOINT = "http://169.254.169.254/metadata/identity/oauth2/token?api-version=2018-02-01&resource=https%3A%2F%2Fvault.azure.net"
    r = requests.get(MSI_ENDPOINT, headers = {"Metadata" : "true"}) 
      
    # extracting data in json format 
    # This request gets an access_token from Azure AD by using the local MSI endpoint.
    data = r.json() 
    
    # Step 2: Pass the access_token received from previous HTTP GET call to your key vault.
    KeyVaultURL = "https://prashanthwinvmvault.vault.azure.net/secrets/RandomSecret?api-version=2016-10-01"
    kvSecret = requests.get(url = KeyVaultURL, headers = {"Authorization": "Bearer " + data["access_token"]})
    
    print(kvSecret.json()["value"])
```

Puede mostrar el valor del secreto al ejecutar el código siguiente: 

```console
python Sample.py
```

El código anterior muestra cómo realizar operaciones con Azure Key Vault en una máquina virtual Windows. 

## <a name="clean-up-resources"></a>Limpieza de recursos

Cuando ya no son necesarios, elimine la máquina virtual y el almacén de claves.

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [API REST de Azure Key Vault](https://docs.microsoft.com/rest/api/keyvault/)
