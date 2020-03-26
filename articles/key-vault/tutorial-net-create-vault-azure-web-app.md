---
title: 'Tutorial: Uso de Azure Key Vault con una aplicación web de Azure en .NET | Microsoft Docs'
description: En este tutorial, va a configurar una aplicación ASP.NET Core para que lea un secreto de su almacén de claves.
services: key-vault
author: msmbaldwin
manager: rajvijan
ms.service: key-vault
ms.subservice: general
ms.topic: tutorial
ms.date: 12/21/2018
ms.author: mbaldwin
ms.custom: mvc
ms.openlocfilehash: e636ab843a9801097bf770ca12c9d1e512750c91
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/24/2020
ms.locfileid: "78198123"
---
# <a name="tutorial-use-azure-key-vault-with-an-azure-web-app-in-net"></a>Tutorial: Uso de Azure Key Vault con una aplicación web de Azure en .NET

Azure Key Vault ayuda a proteger los secretos, como las claves de API y las cadenas de conexión de base de datos. Además, proporciona acceso a sus aplicaciones, servicios y recursos de TI.

En este tutorial, aprenderá a crear una aplicación web de Azure que puede leer información de un almacén de claves de Azure. El proceso usa Managed Identities for Azure Resources. Para más información sobre aplicaciones web de Azure, consulte [Azure App Service](../app-service/overview.md).

En este tutorial se muestra cómo realizar las siguientes acciones:

> [!div class="checklist"]
> * Cree un almacén de claves.
> * Agregue un secreto al almacén de claves.
> * Recuperar un secreto del almacén de claves.
> * Crear una aplicación web de Azure.
> * Habilitar una identidad administrada para la aplicación web.
> * Asignar permisos para la aplicación web.
> * Ejecutar la aplicación web en Azure.

Antes de empezar, lea los [conceptos básicos de Key Vault](basic-concepts.md). 

Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).

## <a name="prerequisites"></a>Prerrequisitos

* Para Windows: [SDK de .NET Core 2.1 o posterior](https://www.microsoft.com/net/download/windows)
* Para Mac: [Visual Studio para Mac](https://visualstudio.microsoft.com/vs/mac/)
* Para Windows, Mac y Linux:
  * [Git](https://git-scm.com/downloads)
  * Este tutorial requiere que se ejecute localmente la CLI de Azure. Debe tener instalada la versión 2.0.4 de la CLI de Azure o una versión posterior. Ejecute `az --version` para encontrar la versión. Si necesita instalarla o actualizarla, consulte [Instalación de la CLI de Azure 2.0](https://docs.microsoft.com/cli/azure/install-azure-cli).
  * [.NET Core](https://www.microsoft.com/net/download/dotnet-core/2.1)

## <a name="about-managed-service-identity"></a>Acerca de Managed Service Identity

Azure Key Vault almacena las credenciales de forma segura, de modo que no se muestran en el código. Sin embargo, debe autenticarse en Azure Key Vault para recuperar las claves. Para autenticarse en Key Vault, necesita una credencial. Se trata de un dilema de arranque clásico. Managed Service Identity (MSI) soluciona este problema al proporcionar una _identidad de arranque_ que simplifica el proceso.

Al habilitar MSI para un servicio de Azure, como Azure Virtual Machines, Azure App Service o Azure Functions, Azure crea una [entidad de servicio](basic-concepts.md). MSI hace esto para la instancia del servicio en Azure Active Directory (Azure AD) e inserta las credenciales de la entidad de servicio en esa instancia.

![Diagrama de MSI](media/MSI.png)

A continuación, para obtener un token de acceso, el código llama a un servicio de metadatos local disponible en el recurso de Azure. El código usa el token de acceso que obtiene del punto de conexión de MSI local para autenticarse en una instancia del servicio Azure Key Vault.

## <a name="log-in-to-azure"></a>Inicio de sesión en Azure

Para iniciar sesión en Azure mediante la CLI de Azure, escriba:

```azurecli
az login
```

## <a name="create-a-resource-group"></a>Crear un grupo de recursos

Un grupo de recursos de Azure es un contenedor lógico en el que se implementan y se administran los recursos de Azure.

Cree un grupo de recursos con el comando [az group create](/cli/azure/group#az-group-create).

Después, seleccione el nombre de un grupo de recursos y rellene el marcador de posición. En el siguiente ejemplo se crea un grupo de recursos en la ubicación Oeste de EE. UU:

   ```azurecli
   # To list locations: az account list-locations --output table
   az group create --name "<YourResourceGroupName>" --location "West US"
   ```

Usará este grupo de recursos a lo largo de este tutorial.

## <a name="create-a-key-vault"></a>Creación de un Almacén de claves

Para crear un almacén de claves en el grupo de recursos, proporcione la siguiente información:

* Nombre del almacén de claves: una cadena de entre 3 y 24 caracteres que puede contener solo números (0-9), letras (a-z, A-Z) y guiones (-).
* Definición de un nombre de grupo de recursos
* Ubicación: **Oeste de EE. UU.**

En la CLI de Azure, escriba el siguiente comando:

```azurecli
az keyvault create --name "<YourKeyVaultName>" --resource-group "<YourResourceGroupName>" --location "West US"
```

En este momento, su cuenta de Azure es la única autorizada para realizar operaciones en este nuevo almacén.

## <a name="add-a-secret-to-the-key-vault"></a>Incorporación de un secreto al almacén de claves

Ahora puede agregar un secreto. Podría ser una cadena de conexión de SQL o cualquier otra información que necesite mantener segura y disponible para la aplicación.

Para crear un secreto en el almacén de claves denominado **AppSecret**, escriba el siguiente comando: 

```azurecli
az keyvault secret set --vault-name "<YourKeyVaultName>" --name "AppSecret" --value "MySecret"
```

Este secreto almacena el valor **MySecret**.

Para ver el valor contenido en el secreto como texto sin formato, escriba el siguiente comando:

```azurecli
az keyvault secret show --name "AppSecret" --vault-name "<YourKeyVaultName>"
```

Este comando muestra la información secreta, incluido el identificador URI. 

Después de completar estos pasos debe tener un identificador URI para un secreto en un almacén de claves. Tome nota de esta información para utilizarla más adelante en este tutorial. 

## <a name="create-a-net-core-web-app"></a>Creación de una aplicación web .NET Core

Para crear una aplicación web .NET Core y publicarla en Azure, siga las instrucciones de [Creación de una aplicación web ASP.NET Core en Azure](../app-service/app-service-web-get-started-dotnet.md). 

También puede ver este vídeo:

>[!VIDEO https://www.youtube.com/embed/EdiiEH7P-bU]

## <a name="open-and-edit-the-solution"></a>Apertura y edición de la solución

1. Vaya a **Páginas** >  archivo **About.cshtml.cs**.

1. Instale estos paquetes NuGet:
   - [AppAuthentication](https://www.nuget.org/packages/Microsoft.Azure.Services.AppAuthentication)
   - [KeyVault](https://www.nuget.org/packages/Microsoft.Azure.KeyVault)

1. Importe el código siguiente en el archivo *About.cshtml.cs*:

   ```csharp
    using Microsoft.Azure.KeyVault;
    using Microsoft.Azure.KeyVault.Models;
    using Microsoft.Azure.Services.AppAuthentication;
   ```

   El código de la clase AboutModel debería ser similar a este:

   ```csharp
    public class AboutModel : PageModel
    {
        public string Message { get; set; }

        public async Task OnGetAsync()
        {
            Message = "Your application description page.";
            int retries = 0;
            bool retry = false;
            try
            {
                /* The next four lines of code show you how to use AppAuthentication library to fetch secrets from your key vault */
                AzureServiceTokenProvider azureServiceTokenProvider = new AzureServiceTokenProvider();
                KeyVaultClient keyVaultClient = new KeyVaultClient(new KeyVaultClient.AuthenticationCallback(azureServiceTokenProvider.KeyVaultTokenCallback));
                var secret = await keyVaultClient.GetSecretAsync("https://<YourKeyVaultName>.vault.azure.net/secrets/AppSecret")
                        .ConfigureAwait(false);
                Message = secret.Value;
            }
            /* If you have throttling errors see this tutorial https://docs.microsoft.com/azure/key-vault/tutorial-net-create-vault-azure-web-app */
            /// <exception cref="KeyVaultErrorException">
            /// Thrown when the operation returned an invalid status code
            /// </exception>
            catch (KeyVaultErrorException keyVaultException)
            {
                Message = keyVaultException.Message;
            }
        }

        // This method implements exponential backoff if there are 429 errors from Azure Key Vault
        private static long getWaitTime(int retryCount)
        {
            long waitTime = ((long)Math.Pow(2, retryCount) * 100L);
            return waitTime;
        }

        // This method fetches a token from Azure Active Directory, which can then be provided to Azure Key Vault to authenticate
        public async Task<string> GetAccessTokenAsync()
        {
            var azureServiceTokenProvider = new AzureServiceTokenProvider();
            string accessToken = await azureServiceTokenProvider.GetAccessTokenAsync("https://vault.azure.net");
            return accessToken;
        }
    }
    ```

## <a name="run-the-web-app"></a>Ejecución de la aplicación web

1. En el menú principal de Visual Studio 2019, seleccione **Depurar** > **Iniciar** con o sin depuración. 
1. En el explorador, vaya a la página **Acerca de**.  
    Aparecerá el valor de **AppSecret**.

## <a name="enable-a-managed-identity"></a>Habilitación de una entidad administrada

Azure Key Vault proporciona una manera de almacenar de forma segura credenciales y otros secretos, pero el código tiene que autenticarse en Key Vault para recuperarlos. La información del artículo [Acerca de Managed Identities for Azure Resources](../active-directory/managed-identities-azure-resources/overview.md) ayuda a resolver este problema al proporcionar a los servicios de Azure una identidad administrada automáticamente en Azure AD. Puede usar esta identidad para autenticar cualquier servicio que admita la autenticación de Azure AD, incluido Key Vault, sin necesidad de tener que mostrar las credenciales en el código.

En la CLI de Azure, ejecute el comando assign-identity para crear la identidad de esta aplicación:

```azurecli
az webapp identity assign --name "<YourAppName>" --resource-group "<YourResourceGroupName>"
```

Reemplace \<YourAppName> por el nombre de la aplicación publicada en Azure.  
    Por ejemplo, si el nombre de la aplicación publicada era **MyAwesomeapp.azurewebsites.net**, reemplace \<YourAppName> por **MyAwesomeapp**.

Anote el valor de `PrincipalId` cuando publique la aplicación en Azure. La salida del comando en el paso 1 debe tener el formato siguiente:

```json
{
  "principalId": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
  "tenantId": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
  "type": "SystemAssigned"
}
```

>[!NOTE]
>El comando de este procedimiento equivale a ir a [Azure Portal](https://portal.azure.com) y cambiar la configuración de **Identity /System assigned** a **On** en las propiedades de la aplicación web.

## <a name="assign-permissions-to-your-app"></a>Asignación de permisos a la aplicación

Reemplace \<YourKeyVaultName> por el nombre del almacén de claves y \<PrincipalId> por el valor de **PrincipalId** en el siguiente comando:

```azurecli
az keyvault set-policy --name '<YourKeyVaultName>' --object-id <PrincipalId> --secret-permissions get list
```

Este comando proporciona a la identidad (MSI) de App Service permisos para realizar operaciones **get** y **list** en el almacén de claves.

## <a name="publish-the-web-app-to-azure"></a>Publicación de la aplicación web en Azure

Publique la aplicación web en Azure una vez más para verificar que la aplicación web activa puede capturar el valor del secreto.

1. En Visual Studio, seleccione el proyecto **key-vault-dotnet-core-quickstart**.
2. Seleccione **Publicar** > **Iniciar**.
3. Seleccione **Crear**.

Cuando ejecute la aplicación, verá que puede recuperar el valor del secreto.

Ahora, ha creado correctamente una aplicación web en .NET que almacena y captura sus secretos del almacén de claves.

## <a name="clean-up-resources"></a>Limpieza de recursos
Cuando ya no son necesarios, puede eliminar la máquina virtual y el almacén de claves.

## <a name="next-steps"></a>Pasos siguientes

>[!div class="nextstepaction"]
>[Guía del desarrollador de Azure Key Vault](key-vault-developers-guide.md)
