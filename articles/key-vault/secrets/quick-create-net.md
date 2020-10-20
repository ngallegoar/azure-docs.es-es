---
title: 'Inicio rápido: biblioteca cliente de Azure Key Vault para .NET (v4)'
description: Obtenga información sobre cómo crear, recuperar y eliminar secretos desde una instancia de Azure Key Vault mediante la biblioteca cliente de .NET (v4)
author: msmbaldwin
ms.author: mbaldwin
ms.date: 09/23/2020
ms.service: key-vault
ms.subservice: secrets
ms.topic: quickstart
ms.custom: devx-track-csharp
ms.openlocfilehash: 8d60c604ecde8607c0da8a125108e13683bdf6c8
ms.sourcegitcommit: 1b47921ae4298e7992c856b82cb8263470e9e6f9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/14/2020
ms.locfileid: "92058546"
---
# <a name="quickstart-azure-key-vault-secret-client-library-for-net-sdk-v4"></a>Inicio rápido: Biblioteca cliente de secretos de Azure Key Vault para .NET (SDK v4)

Introducción a la biblioteca cliente de secretos de Azure Key Vault para .NET. Siga estos pasos para instalar el paquete y probar el código de ejemplo para realizar tareas básicas.

[Documentación de referencia de la API](/dotnet/api/azure.security.keyvault.secrets?view=azure-dotnet&preserve-view=true) | [Código fuente de la biblioteca](https://github.com/Azure/azure-sdk-for-net/tree/master/sdk/keyvault) | [Paquete (NuGet)](https://www.nuget.org/packages/Azure.Security.KeyVault.Secrets/)

## <a name="prerequisites"></a>Prerrequisitos

* Una suscripción a Azure: [cree una cuenta gratuita](https://azure.microsoft.com/free/dotnet).
* [SDK de .NET Core 3.1 o una versión posterior](https://dotnet.microsoft.com/download/dotnet-core)
* [CLI de Azure](/cli/azure/install-azure-cli)

En este inicio rápido se usa `dotnet` y la CLI de Azure.

## <a name="setup"></a>Configurar

En este inicio rápido se usa la biblioteca de identidades de Azure con la CLI de Azure para autenticar al usuario en los servicios de Azure. Los desarrolladores también pueden usar Visual Studio o Visual Studio Code para autenticar sus llamadas. Para más información, consulte [Autenticación del cliente mediante la biblioteca cliente Azure Identity](https://docs.microsoft.com/dotnet/api/overview/azure/identity-readme?#authenticate-the-client&preserve-view=true).

### <a name="sign-in-to-azure"></a>Inicio de sesión en Azure

1. Ejecute el comando `login`.

    ```azurecli-interactive
    az login
    ```

    Si la CLI puede abrir el explorador predeterminado, lo hará y cargará una página de inicio de sesión de Azure.

    En caso contrario, abra una página del explorador en [https://aka.ms/devicelogin](https://aka.ms/devicelogin) y escriba el código de autorización que se muestra en el terminal.

2. Inicie sesión con las credenciales de su cuenta en el explorador.


### <a name="create-new-net-console-app"></a>Creación de una aplicación de consola de .NET

1. En un shell de comandos, ejecute el siguiente comando para crear un proyecto llamado `key-vault-console-app`:

    ```dotnetcli
    dotnet new console --name key-vault-console-app
    ```

1. Cambie al directorio *key-vault-console-app* recién creado y ejecute el comando siguiente para compilar el proyecto:

    ```dotnetcli
    dotnet build
    ```

    La salida de la compilación no debe contener advertencias ni errores.
    
    ```console
    Build succeeded.
     0 Warning(s)
     0 Error(s)
    ```

### <a name="install-the-packages"></a>Instalación de los paquetes

En el shell de comandos, instale la biblioteca cliente de secretos de Azure Key Vault para .NET:

```dotnetcli
dotnet add package Azure.Security.KeyVault.Secrets
```

Para este inicio rápido también deberá instalar la biblioteca cliente del SDK de Azure SDK para Azure Identity:

```dotnetcli
dotnet add package Azure.Identity
```

### <a name="create-a-resource-group-and-key-vault"></a>Creación de un grupo de recursos y de un almacén de claves

[!INCLUDE[Create a resource group and key vault](../../../includes/key-vault-rg-kv-creation.md)]

#### <a name="grant-access-to-your-key-vault"></a>Concesión de acceso al almacén de claves

Creación de una directiva de acceso para el almacén de claves que conceda permiso mediante secreto a la cuenta de usuario

```console
az keyvault set-policy --name <YourKeyVaultName> --upn user@domain.com --secret-permissions delete get list set
```

#### <a name="set-environment-variables"></a>Establecimiento de variables de entorno

Esta aplicación también usa el nombre del almacén de claves como variable de entorno llamada `KEY_VAULT_NAME`.

Windows
```cmd
set KEY_VAULT_NAME=<your-key-vault-name>
````
Windows PowerShell
```powershell
$Env:KEY_VAULT_NAME=<your-key-vault-name>
```

macOS o Linux
```bash
export KEY_VAULT_NAME=<your-key-vault-name>
```

## <a name="object-model"></a>Modelo de objetos

La biblioteca cliente de secretos de Azure Key Vault para .NET permite administrar los secretos. En la sección [Ejemplos de código](#code-examples) se muestra cómo crear un cliente y cómo establecer, recuperar y eliminar un secreto.

## <a name="code-examples"></a>Ejemplos de código

### <a name="add-directives"></a>Adición de directivas

Agregue las directivas siguientes al principio de *Program.cs*:

[!code-csharp[](~/samples-key-vault-dotnet-quickstart/key-vault-console-app/Program.cs?name=directives)]

### <a name="authenticate-and-create-a-client"></a>Autenticación y creación de un cliente

En este inicio rápido se emplea el usuario que ha iniciado sesión para autenticarlo en el almacén de claves, que es el método preferido para el desarrollo local. Para las aplicaciones implementadas en Azure, la identidad administrada debe asignarse a App Service o la máquina virtual. Para más información, consulte [Introducción a la identidad administrada](https://docs.microsoft.com/azure/active-directory/managed-identities-azure-resources/overview).

En el ejemplo siguiente, el nombre del almacén de claves se expande al URI del almacén de claves, con el formato "https://\<your-key-vault-name\>.vault.azure.net". En este ejemplo se usa la clase ["DefaultAzureCredential()"](/dotnet/api/azure.identity.defaultazurecredential), que permite usar el mismo código en entornos diferentes con distintas opciones para proporcionar la identidad. Para más información, consulte [Autenticación mediante las credenciales predeterminadas de Azure](https://docs.microsoft.com/dotnet/api/overview/azure/identity-readme?#defaultazurecredential). 

[!code-csharp[](~/samples-key-vault-dotnet-quickstart/key-vault-console-app/Program.cs?name=authenticate)]

### <a name="save-a-secret"></a>Almacenamiento de un secreto

Ahora que la aplicación de consola se ha autenticado, agregue un secreto al almacén de claves. Para esta tarea, use el método [SetSecretAsync](/dotnet/api/azure.security.keyvault.secrets.secretclient.setsecretasync). El primer parámetro del método acepta un nombre para el secreto&mdash;"mySecret" en este ejemplo.

```csharp
await client.SetSecretAsync(secretName, secretValue);
``````

Puede comprobar que el secreto se ha establecido con el comando [az keyvault secret show](/cli/azure/keyvault/secret?view=azure-cli-latest#az-keyvault-secret-show&preserve-view=true):

```azurecli
az keyvault secret show --vault-name <your-unique-keyvault-name> --name mySecret
```

```azurepowershell
(Get-AzKeyVaultSecret -VaultName <your-unique-keyvault-name> -Name mySecret).SecretValueText
```

### <a name="retrieve-a-secret"></a>Recuperación de un secreto

El valor establecido previamente ahora se puede recuperar con el método [GetSecretAsync](/dotnet/api/azure.security.keyvault.secrets.secretclient.getsecretasync).

```csharp
var secret = await client.GetSecretAsync(secretName);
``````

El secreto se guarda ahora como `secret.Value`.

### <a name="delete-a-secret"></a>Eliminación de un secreto

Por último, vamos a eliminar el secreto del almacén de claves con el método [StartDeleteSecretAsync](/dotnet/api/azure.security.keyvault.secrets.secretclient.startdeletesecretasync).

```csharp
await client.StartDeleteSecretAsync(secretName);
``````

Puede comprobar que el secreto ya no está con el comando [az keyvault secret show](/cli/azure/keyvault/secret?view=azure-cli-latest#az-keyvault-secret-show&preserve-view=true):

```azurecli
az keyvault secret show --vault-name <your-unique-keyvault-name> --name mySecret
```

```azurepowershell
(Get-AzKeyVaultSecret -VaultName <your-unique-keyvault-name> -Name mySecret).SecretValueText
```

## <a name="clean-up-resources"></a>Limpieza de recursos

Cuando ya no lo necesite, puede usar la CLI de Azure o Azure PowerShell para quitar el almacén de claves y el grupo de recursos correspondiente.

### <a name="delete-a-key-vault"></a>Eliminación de un almacén de claves

```azurecli
az keyvault delete --name <your-unique-keyvault-name>
```

```azurepowershell
Remove-AzKeyVault -VaultName <your-unique-keyvault-name>
```

### <a name="purge-a-key-vault"></a>Purga de un almacén de claves

```azurecli
az keyvault purge --location eastus --name <your-unique-keyvault-name>
```

```azurepowershell
Remove-AzKeyVault -VaultName <your-unique-keyvault-name> -InRemovedState -Location eastus
```

### <a name="delete-a-resource-group"></a>Eliminación de un grupo de recursos

```azurecli
az group delete -g "myResourceGroup"
```

```azurepowershell
Remove-AzResourceGroup -Name "myResourceGroup"
```

## <a name="sample-code"></a>Código de ejemplo

Modifique la aplicación de consola de .NET Core para interactuar con el almacén de claves; para ello, complete los pasos siguientes:

1. Reemplace el código de *Program.cs* por el código siguiente:

    ```csharp
    using System;
    using Azure.Identity;
    using Azure.Security.KeyVault.Secrets;
    
    namespace key_vault_console_app
    {
        class Program
        {
            static async Task Main(string[] args)
            {
                const string secretName = "mySecret";
                var keyVaultName = Environment.GetEnvironmentVariable("KEY_VAULT_NAME");
                var kvUri = $"https://{keyVaultName}.vault.azure.net";
    
                var client = new SecretClient(new Uri(kvUri), new DefaultAzureCredential());
    
                Console.Write("Input the value of your secret > ");
                var secretValue = Console.ReadLine();
    
                Console.Write($"Creating a secret in {keyVaultName} called '{secretName}' with the value '{secretValue}' ...");
                await client.SetSecretAsync(secretName, secretValue);
                Console.WriteLine(" done.");
    
                Console.WriteLine("Forgetting your secret.");
                secretValue = string.Empty;
                Console.WriteLine($"Your secret is '{secretValue}'.");
    
                Console.WriteLine($"Retrieving your secret from {keyVaultName}.");
                var secret = await client.GetSecretAsync(secretName);
                Console.WriteLine($"Your secret is '{secret.Value}'.");
    
                Console.Write($"Deleting your secret from {keyVaultName} ...");
                DeleteSecretOperation operation = await client.StartDeleteSecretAsync(secretName);
                // You only need to wait for completion if you want to purge or recover the secret.
                await operation.WaitForCompletionAsync();

                await client.PurgeDeletedSecret(secretName);
                Console.WriteLine(" done.");
            }
        }
    }
    ```

1. Ejecute el siguiente comando para ejecutar la aplicación.

    ```dotnetcli
    dotnet run
    ```

1. Cuando se le pida, escriba un secreto. Por ejemplo, mySecretPassword.

    Aparece una variación del resultado siguiente:

    ```console
    Input the value of your secret > mySecretPassword
    Creating a secret in <your-unique-keyvault-name> called 'mySecret' with the value 'mySecretPassword' ... done.
    Forgetting your secret.
    Your secret is ''.
    Retrieving your secret from <your-unique-keyvault-name>.
    Your secret is 'mySecretPassword'.
    Deleting your secret from <your-unique-keyvault-name> ... done.    
    ```

## <a name="next-steps"></a>Pasos siguientes

En este inicio rápido, ha creado un almacén de claves, ha almacenado un secreto en él y, posteriormente, lo ha recuperado. Consulte la [aplicación de consola completa en GitHub](https://github.com/Azure-Samples/key-vault-dotnet-core-quickstart/tree/master/key-vault-console-app).

Para más información sobre Key Vault y cómo integrarlo con las aplicaciones, consulte los artículos siguientes:

- Lea una [introducción a Azure Key Vault](../general/overview.md).
- Vea un [Tutorial sobre el acceso a Key Vault desde una aplicación de App Service](../general/tutorial-net-create-vault-azure-web-app.md).
- Consulte un [Tutorial sobre el acceso a Key Vault desde una máquina virtual](../general/tutorial-net-virtual-machine.md).
- Consulte la [guía del desarrollador de Azure Key Vault](../general/developers-guide.md).
- Consulte los [procedimientos recomendados de Azure Key Vault](../general/best-practices.md).
