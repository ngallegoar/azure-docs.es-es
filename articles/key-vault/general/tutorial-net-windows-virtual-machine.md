---
title: 'Tutorial: Uso de Azure Key Vault con una máquina virtual Windows en .NET | Microsoft Docs'
description: En este tutorial, va a configurar una aplicación ASP.NET Core para que lea un secreto de su almacén de claves.
services: key-vault
author: msmbaldwin
manager: rajvijan
ms.service: key-vault
ms.subservice: general
ms.topic: tutorial
ms.date: 01/02/2019
ms.author: mbaldwin
ms.custom: mvc
ms.openlocfilehash: 6ba78a44af7beb9b5b79aa1a87e08f5a82589cce
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/16/2020
ms.locfileid: "81425764"
---
# <a name="tutorial-use-azure-key-vault-with-a-windows-virtual-machine-in-net"></a>Tutorial: Uso de Azure Key Vault con una máquina virtual Windows en .NET

Azure Key Vault ayuda a proteger secretos como las claves de API, las cadenas de conexión de base de datos necesarias para acceder a las aplicaciones, los servicios y los recursos de TI.

En este tutorial, aprenderá cómo obtener una aplicación de consola para leer información de Azure Key Vault. Para ello, use identidades administradas para recursos de Azure. 

En este tutorial se muestra cómo realizar las siguientes acciones:

> [!div class="checklist"]
> * Cree un grupo de recursos.
> * Cree un almacén de claves.
> * Agregue un secreto al almacén de claves.
> * Recuperar un secreto del almacén de claves.
> * Cree una máquina virtual de Azure.
> * Habilitar una [identidad administrada](../../active-directory/managed-identities-azure-resources/overview.md) para la máquina virtual.
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

![MSI](../media/MSI.png)

A continuación, para obtener un token de acceso, el código llama a un servicio de metadatos local disponible en el recurso de Azure. Para autenticarse en una instancia del servicio Azure Key Vault, el código usa el token de acceso que obtiene del punto de conexión de MSI local. 

## <a name="create-resources-and-assign-permissions"></a>Creación de recursos y asignación de permisos

Antes de empezar a codificar, necesita crear algunos recursos, colocar un secreto en el almacén de claves y asignar los permisos.

### <a name="sign-in-to-azure"></a>Inicio de sesión en Azure

Para iniciar sesión en Azure mediante la CLI de Azure, escriba:

```azurecli
az login
```

### <a name="create-a-resource-group"></a>Crear un grupo de recursos

Un grupo de recursos de Azure es un contenedor lógico en el que se implementan y se administran los recursos de Azure. Cree un grupo de recursos con el comando [az group create](/cli/azure/group#az-group-create). 

Este ejemplo crea un grupo de recursos en la ubicación Oeste de EE. UU.:

```azurecli
# To list locations: az account list-locations --output table
az group create --name "<YourResourceGroupName>" --location "West US"
```

El grupo de recursos recién creado se usará en todo este tutorial.

### <a name="create-a-key-vault-and-populate-it-with-a-secret"></a>Creación de un almacén de claves y su rellenado con un secreto

Cree un almacén de claves en el grupo de recursos mediante el comando [az keyvault create](/cli/azure/keyvault?view=azure-cli-latest#az-keyvault-create) con la información siguiente:

* Nombre del almacén de claves: una cadena de entre 3 y 24 caracteres que puede contener solo números (0-9), letras (a-z, A-Z) y guiones (-).
* Definición de un nombre de grupo de recursos
* Ubicación: **Oeste de EE. UU.**

```azurecli
az keyvault create --name "<YourKeyVaultName>" --resource-group "<YourResourceGroupName>" --location "West US"
```
En este momento, su cuenta de Azure es la única autorizada para realizar operaciones en este nuevo almacén de claves.

Ahora agregue un secreto al almacén de claves con el comando [az keyvault secret set](/cli/azure/keyvault/secret?view=azure-cli-latest#az-keyvault-secret-set).


Para crear un secreto en el almacén de claves denominado **AppSecret**, escriba el siguiente comando:

```azurecli
az keyvault secret set --vault-name "<YourKeyVaultName>" --name "AppSecret" --value "MySecret"
```

Este secreto almacena el valor **MySecret**.

### <a name="create-a-virtual-machine"></a>Creación de una máquina virtual
Cree una máquina virtual mediante uno de los métodos siguientes:

* [La CLI de Azure](../../virtual-machines/windows/quick-create-cli.md)
* [PowerShell](../../virtual-machines/windows/quick-create-powershell.md)
* [Portal de Azure](../../virtual-machines/windows/quick-create-portal.md)

### <a name="assign-an-identity-to-the-vm"></a>Asignación de una identidad a la máquina virtual
Cree una identidad asignada por el sistema para la máquina virtual con el comando [az vm identity assign](/cli/azure/vm/identity?view=azure-cli-latest#az-vm-identity-assign):

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

### <a name="assign-permissions-to-the-vm-identity"></a>Asignación de permisos a la identidad de máquina virtual
Asigne los permisos de la identidad creada anteriormente al almacén de claves con el comando[az keyvault set-policy](/cli/azure/keyvault?view=azure-cli-latest#az-keyvault-set-policy):

```azurecli
az keyvault set-policy --name '<YourKeyVaultName>' --object-id <VMSystemAssignedIdentity> --secret-permissions get list
```

### <a name="sign-in-to-the-virtual-machine"></a>Inicio de sesión en la máquina virtual

Para iniciar sesión en la máquina virtual, siga las instrucciones que aparecen en [Conexión a una máquina virtual de Azure donde se ejecuta Windows e inicio de sesión en ella](../../virtual-machines/windows/connect-logon.md).

## <a name="set-up-the-console-app"></a>Configuración de la aplicación de consola

Cree una aplicación de consola e instale los paquetes necesarios con el comando `dotnet`.

### <a name="install-net-core"></a>Instalar .NET Core

Para instalar .NET Core, vaya a la [página de descargas de .NET](https://www.microsoft.com/net/download).

### <a name="create-and-run-a-sample-net-app"></a>Creación y ejecución de una aplicación de .NET de ejemplo

Abra un símbolo del sistema.

Puede imprimir "Hola mundo" en la consola mediante la ejecución de los siguientes comandos:

```console
dotnet new console -o helloworldapp
cd helloworldapp
dotnet run
```

### <a name="install-the-packages"></a>Instalación de los paquetes

En la ventana de la consola, instale los paquetes .NET necesarios para este inicio rápido:

```console
dotnet add package System.IO;
dotnet add package System.Net;
dotnet add package System.Text;
dotnet add package Newtonsoft.Json;
dotnet add package Newtonsoft.Json.Linq;
```

## <a name="edit-the-console-app"></a>Edición de la aplicación de consola

Abra el archivo *Program.cs* y agregue estos paquetes:

```csharp
using System;
using System.IO;
using System.Net;
using System.Text;
using Newtonsoft.Json;
using Newtonsoft.Json.Linq;
```

Edite el archivo de clase para que contenga el código en el proceso de tres pasos siguiente:

1. Capturar un token del punto de conexión MSI local en la máquina virtual. Si lo hace, también captura un token de Azure AD.
2. Pase el token al almacén de claves y, después, capture el secreto. 
3. Agregue tanto el nombre del almacén como el nombre del secreto a la solicitud.

```csharp
 class Program
    {
        static void Main(string[] args)
        {
            // Step 1: Get a token from the local (URI) Managed Service Identity endpoint, which in turn fetches it from Azure AD
            var token = GetToken();

            // Step 2: Fetch the secret value from your key vault
            System.Console.WriteLine(FetchSecretValueFromKeyVault(token));
        }

        static string GetToken()
        {
            WebRequest request = WebRequest.Create("http://169.254.169.254/metadata/identity/oauth2/token?api-version=2018-02-01&resource=https%3A%2F%2Fvault.azure.net");
            request.Headers.Add("Metadata", "true");
            WebResponse response = request.GetResponse();
            return ParseWebResponse(response, "access_token");
        }
        
        static string FetchSecretValueFromKeyVault(string token)
        {
            //Step 3: Add the vault name and secret name to the request.
            WebRequest kvRequest = WebRequest.Create("https://<YourVaultName>.vault.azure.net/secrets/<YourSecretName>?api-version=2016-10-01");
            kvRequest.Headers.Add("Authorization", "Bearer "+  token);
            WebResponse kvResponse = kvRequest.GetResponse();
            return ParseWebResponse(kvResponse, "value");
        }

        private static string ParseWebResponse(WebResponse response, string tokenName)
        {
            string token = String.Empty;
            using (Stream stream = response.GetResponseStream())
            {
                StreamReader reader = new StreamReader(stream, Encoding.UTF8);
                String responseString = reader.ReadToEnd();

                JObject joResponse = JObject.Parse(responseString);    
                JValue ojObject = (JValue)joResponse[tokenName];             
                token = ojObject.Value.ToString();
            }
            return token;
        }
    }
```

El código anterior muestra cómo realizar operaciones con Azure Key Vault en una máquina virtual Windows.

## <a name="clean-up-resources"></a>Limpieza de recursos

Cuando ya no son necesarios, elimine la máquina virtual y el almacén de claves.

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [API REST de Azure Key Vault](https://docs.microsoft.com/rest/api/keyvault/)
