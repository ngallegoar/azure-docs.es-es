---
title: 'Tutorial: Uso de una máquina virtual Linux y una aplicación de consola de ASP.NET para almacenar secretos en Azure Key Vault | Microsoft Docs'
description: En este tutorial, aprenderá a configurar una aplicación de consola de ASP.NET para leer un secreto de Azure Key Vault.
services: key-vault
author: msmbaldwin
manager: rajvijan
ms.service: key-vault
ms.subservice: secrets
ms.topic: tutorial
ms.date: 12/21/2018
ms.author: mbaldwin
ms.custom: mvc
ms.openlocfilehash: 50810dd1fbf2d97989df47b537ef5c574be059d2
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/16/2020
ms.locfileid: "81426174"
---
# <a name="tutorial-use-a-linux-vm-and-a-net-app-to-store-secrets-in-azure-key-vault"></a>Tutorial: Uso de una máquina virtual Linux y una aplicación .NET para almacenar secretos en Azure Key Vault

Azure Key Vault ayuda a proteger secretos como las claves de API y las cadenas de conexión de base de datos necesarias para acceder a las aplicaciones, los servicios y los recursos de TI.

En este tutorial, configurará una aplicación de consola .NET para leer información de Azure Key Vault mediante identidades administradas para recursos de Azure. Aprenderá a:

> [!div class="checklist"]
> * Creación de un Almacén de claves
> * Almacenar un secreto en Key Vault
> * Crear una máquina virtual Linux de Azure
> * Habilitar una [identidad administrada](../../active-directory/managed-identities-azure-resources/overview.md) para la máquina virtual
> * Conceder los permisos necesarios para que la aplicación de consola lea datos de Key Vault
> * Recuperar un secreto del almacén de claves

Antes de avanzar, lea acerca de los [conceptos básicos de Key Vault](../general/basic-concepts.md).

## <a name="prerequisites"></a>Prerrequisitos

* [Git](https://git-scm.com/downloads).
* Suscripción a Azure. Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de empezar.
* [CLI de Azure 2.0 o posterior](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest) o Azure Cloud Shell.

[!INCLUDE [Azure Cloud Shell](../../../includes/cloud-shell-try-it.md)]

## <a name="understand-managed-service-identity"></a>Managed Service Identity

Azure Key Vault puede almacenar credenciales de forma segura para que no estén en el código, pero para recuperarlas debe autenticarse en Azure Key Vault. Sin embargo, para autenticarse en Key Vault, necesita una credencial. Se trata de un problema clásico de arranque. Con Azure y Azure Active Directory (Azure AD), Managed Service Identity (MSI) puede proporcionar una identidad de arranque que permite comenzar fácilmente.

Cuando se habilita MSI para un servicio de Azure como Virtual Machines, App Service o Functions, Azure crea una entidad de servicio para la instancia del servicio en Azure Active Directory. Inserta las credenciales de la entidad de servicio en la instancia del servicio.

![MSI](../media/MSI.png)

A continuación, el código llama a un servicio local de metadatos disponible en el recurso de Azure para obtener un token de acceso.
El código usa el token de acceso que obtiene del MSI_ENDPOINT local para autenticar una instancia del servicio Azure Key Vault.

## <a name="sign-in-to-azure"></a>Inicio de sesión en Azure

Para iniciar sesión en Azure mediante la CLI de Azure, escriba:

```azurecli-interactive
az login
```

## <a name="create-a-resource-group"></a>Crear un grupo de recursos

Cree un grupo de recursos mediante el comando `az group create`. Un grupo de recursos de Azure es un contenedor lógico en el que se implementan y se administran los recursos de Azure.

Cree un grupo de recursos en la ubicación de oeste de EE. UU. Elija un nombre para el grupo de recursos y reemplace `YourResourceGroupName` en el ejemplo siguiente:

```azurecli-interactive
# To list locations: az account list-locations --output table
az group create --name "<YourResourceGroupName>" --location "West US"
```

Usará este grupo de recursos a lo largo de este tutorial.

## <a name="create-a-key-vault"></a>Creación de un Almacén de claves

A continuación, cree un almacén de claves en el grupo de recursos. Proporcione la siguiente información:

* Nombre del almacén de claves: una cadena de entre 3 y 24 caracteres que puede contener solo números, letras y guiones ( 0-9, a-z, A-Z y \- ).
* Definición de un nombre de grupo de recursos
* Ubicación: **Oeste de EE. UU.**

```azurecli-interactive
az keyvault create --name "<YourKeyVaultName>" --resource-group "<YourResourceGroupName>" --location "West US"
```

En este momento, su cuenta de Azure es la única autorizada para realizar operaciones en este nuevo almacén.

## <a name="add-a-secret-to-the-key-vault"></a>Incorporación de un secreto al almacén de claves

Ahora puede agregar un secreto. En un escenario del mundo real, se puede almacenar una cadena de conexión de SQL o cualquier otra información que necesite guardar de forma segura, pero que desea que esté disponible para la aplicación.

Para este tutorial, escriba los siguientes comandos para crear un secreto en el almacén de claves. El secreto se denomina **AppSecret** y su valor es **MySecret**.

```azurecli
az keyvault secret set --vault-name "<YourKeyVaultName>" --name "AppSecret" --value "MySecret"
```

## <a name="create-a-linux-virtual-machine"></a>Creación de una máquina virtual con Linux

Cree una máquina virtual con el comando `az vm create`.

En el ejemplo siguiente se crea una máquina virtual llamada **myVM** y se agrega una cuenta de usuario llamada **azureuser**. El parámetro `--generate-ssh-keys` se usa para generar automáticamente una clave SSH y colocarla en la ubicación de clave predeterminada ( **~/.ssh**). Para utilizar un conjunto de claves concreto, utilice la opción `--ssh-key-value`.

```azurecli-interactive
az vm create \
  --resource-group myResourceGroup \
  --name myVM \
  --image UbuntuLTS \
  --admin-username azureuser \
  --generate-ssh-keys
```

La creación de la máquina virtual y los recursos auxiliares tarda unos minutos en realizarse. En la salida de ejemplo siguiente se muestra que la operación de creación de la máquina virtual se realizó correctamente.

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

Anote el valor de `publicIpAddress` en la salida de la máquina virtual. Esta dirección se utiliza para acceder a la VM en los pasos posteriores.

## <a name="assign-an-identity-to-the-vm"></a>Asignación de una identidad a la máquina virtual

Cree una identidad asignada por el sistema para la máquina virtual; para ello, ejecute el siguiente comando:

```azurecli
az vm identity assign --name <NameOfYourVirtualMachine> --resource-group <YourResourceGroupName>
```

La salida del comando debe ser:

```output
{
  "systemAssignedIdentity": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
  "userAssignedIdentities": {}
}
```

Anote el `systemAssignedIdentity`. La usará en el paso siguiente.

## <a name="give-the-vm-identity-permission-to-key-vault"></a>Concesión del permiso de identidad de máquina virtual al almacén de claves

Ahora puede dar permiso de Key Vault a la identidad que creó. Ejecute el siguiente comando:

```azurecli
az keyvault set-policy --name '<YourKeyVaultName>' --object-id <VMSystemAssignedIdentity> --secret-permissions get list
```

## <a name="log-in-to-the-vm"></a>Inicio de sesión en la máquina virtual

Ahora inicie sesión en la máquina virtual mediante un terminal.

```terminal
ssh azureuser@<PublicIpAddress>
```

## <a name="install-net-core-on-linux"></a>Instalación de .NET Core en Linux

En la máquina virtual Linux:

Registre la clave de producto de Microsoft como de confianza mediante la ejecución de los siguientes comandos:

   ```console
   curl https://packages.microsoft.com/keys/microsoft.asc | gpg --dearmor > microsoft.gpg
   sudo mv microsoft.gpg /etc/apt/trusted.gpg.d/microsoft.gpg
   ```

Configure la fuente del paquete de host de la versión deseada en función del sistema operativo:

```console
   # Ubuntu 17.10
   sudo sh -c 'echo "deb [arch=amd64] https://packages.microsoft.com/repos/microsoft-ubuntu-artful-prod artful main" > /etc/apt/sources.list.d/dotnetdev.list'
   sudo apt-get update
   
   # Ubuntu 17.04
   sudo sh -c 'echo "deb [arch=amd64] https://packages.microsoft.com/repos/microsoft-ubuntu-zesty-prod zesty main" > /etc/apt/sources.list.d/dotnetdev.list'
   sudo apt-get update
   
   # Ubuntu 16.04 / Linux Mint 18
   sudo sh -c 'echo "deb [arch=amd64] https://packages.microsoft.com/repos/microsoft-ubuntu-xenial-prod xenial main" > /etc/apt/sources.list.d/dotnetdev.list'
   sudo apt-get update
   
   # Ubuntu 14.04 / Linux Mint 17
   sudo sh -c 'echo "deb [arch=amd64] https://packages.microsoft.com/repos/microsoft-ubuntu-trusty-prod trusty main" > /etc/apt/sources.list.d/dotnetdev.list'
   sudo apt-get update
```

Instale .NET y compruebe la versión:

   ```console
   sudo apt-get install dotnet-sdk-2.1.4
   dotnet --version
   ```

## <a name="create-and-run-a-sample-net-app"></a>Creación y ejecución de una aplicación de .NET de ejemplo

Ejecute los comandos siguientes: Debería ver que se imprime "Hola mundo" en la consola.

```console
dotnet new console -o helloworldapp
cd helloworldapp
dotnet run
```

## <a name="edit-the-console-app-to-fetch-your-secret"></a>Edición de la aplicación de consola para capturar el secreto

Abra el archivo Program.cs y agregue estos paquetes:

   ```csharp
   using System;
   using System.IO;
   using System.Net;
   using System.Text;
   using Newtonsoft.Json;
   using Newtonsoft.Json.Linq;
   ```

Es un proceso de dos pasos para cambiar el archivo de clase para permitir a la aplicación acceder al secreto del almacén de claves.

1. Captura un token del punto de conexión de MSI local de la máquina virtual que, a su vez, captura un token de Azure Active Directory.
1. Pase el token a Key Vault y capture el secreto.

   Edite el archivo de clase para que contenga el código siguiente:

   ```csharp
    class Program
       {
           static void Main(string[] args)
           {
               // Step 1: Get a token from local (URI) Managed Service Identity endpoint which in turn fetches it from Azure Active Directory
               var token = GetToken();

               // Step 2: Fetch the secret value from Key Vault
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
               WebRequest kvRequest = WebRequest.Create("https://prashanthwinvmvault.vault.azure.net/secrets/RandomSecret?api-version=2016-10-01");
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

Ahora ha aprendido a realizar operaciones con Azure Key Vault en una aplicación .NET que se ejecuta en una máquina virtual Linux de Azure.

## <a name="clean-up-resources"></a>Limpieza de recursos

Elimine el grupo de recursos, la máquina virtual y todos los recursos relacionados Cuando ya no los necesite. Para ello, seleccione el grupo de recursos de la máquina virtual y seleccione **Eliminar**.

Elimine el almacén de claves mediante el comando `az keyvault delete`:

```azurecli-interactive
az keyvault delete --name
                   [--resource group]
                   [--subscription]
```

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [API REST de Azure Key Vault](https://docs.microsoft.com/rest/api/keyvault/)
