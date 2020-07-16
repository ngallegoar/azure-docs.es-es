---
title: Autenticación de una aplicación cliente
titleSuffix: Azure Digital Twins
description: Vea cómo autenticar una aplicación cliente en el servicio Azure Digital Twins.
author: baanders
ms.author: baanders
ms.date: 4/22/2020
ms.topic: how-to
ms.service: digital-twins
ms.openlocfilehash: e52307c92d9371af6479f64841c6f269ed10e4b4
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/02/2020
ms.locfileid: "85390829"
---
# <a name="authenticate-a-client-application-with-azure-digital-twins"></a>Autenticación de una aplicación cliente con Azure Digital Twins

Después de [crear una instancia de Azure Digital Twins](how-to-set-up-instance.md), puede crear una aplicación cliente que usará para interactuar con la instancia. Una vez que haya configurado un proyecto de cliente de inicio, en este artículo se muestra cómo autenticar correctamente la aplicación cliente con la instancia de Azure Digital Twins.

Para ello, debe realizar dos pasos:
1. Crear un registro de aplicación
2. Escribir código de autenticación en una aplicación cliente

[!INCLUDE [Cloud Shell for Azure Digital Twins](../../includes/digital-twins-cloud-shell.md)]

## <a name="create-an-app-registration"></a>Crear un registro de aplicación

Para realizar la autenticación en Azure Digital Twins desde una aplicación cliente, debe configurar un **registro de la aplicación** en [Azure Active Directory](../active-directory/fundamentals/active-directory-whatis.md).

Este registro de la aplicación es donde se configuran los permisos de acceso a las [API de Azure Digital Twins](how-to-use-apis-sdks.md). La aplicación cliente se autentica en el registro de la aplicación y, como resultado, se le conceden los permisos de acceso configurados a las API.

Para crear un registro de la aplicación, debe proporcionar los identificadores de recursos de las API de Azure Digital Twins y los permisos de línea de base para la API. En el directorio de trabajo, abra un archivo nuevo y escriba el siguiente fragmento de código JSON para configurar estos detalles: 

```json
[{
    "resourceAppId": "0b07f429-9f4b-4714-9392-cc5e8e80c8b0",
    "resourceAccess": [
     {
       "id": "4589bd03-58cb-4e6c-b17f-b580e39652f8",
       "type": "Scope"
     }
    ]
}]
``` 

Guarde este archivo como *manifest.json*.

> [!NOTE] 
> Hay algunos lugares en los que se puede usar una cadena `https://digitaltwins.azure.net` legible y "descriptiva" para el identificador de la aplicación de recursos de Azure Digital Twins en lugar del GUID `0b07f429-9f4b-4714-9392-cc5e8e80c8b0`. En particular, muchos ejemplos a lo largo de este conjunto de documentación usan la autenticación con la biblioteca MSAL y, para ello, se puede usar la cadena descriptiva. Sin embargo, durante este paso de creación del registro de la aplicación, es necesario el formato de GUID del identificador, tal y como se muestra arriba. 

En la ventana de Cloud Shell, haga clic en el icono "Cargar/Descargar archivos" y elija "Cargar".

:::image type="content" source="media/how-to-authenticate-client/upload-extension.png" alt-text="Ventana de Cloud Shell que muestra la selección de la opción Cargar":::
Vaya al archivo *manifest.json* que acaba de crear y seleccione "Abrir".

A continuación, ejecute el siguiente comando para crear un registro de la aplicación (reemplazando los marcadores de posición según sea necesario):

```azurecli
az ad app create --display-name <name-for-your-app> --native-app --required-resource-accesses manifest.json --reply-url http://localhost
```

La salida de este comando tendrá un aspecto similar al siguiente.

:::image type="content" source="media/how-to-authenticate-client/new-app-registration.png" alt-text="Nuevo registro de aplicación de AAD":::

Una vez creado el registro de la aplicación, siga [este vínculo](https://portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/RegisteredApps) para ir a la página de información general del registro de la aplicación de AAD en Azure Portal.

En esta información general, seleccione en la lista el registro de la aplicación que acaba de crear. Se abrirán sus detalles en una página como esta:

:::image type="content" source="media/how-to-authenticate-client/get-authentication-ids.png" alt-text="Azure Portal: identificadores de autenticación":::

Tome nota del *Id. de la aplicación (cliente)* y el *Id. de directorio (inquilino)* , como se muestra en **su** página. Usará estos valores más adelante para autenticar una aplicación cliente en las API de Azure Digital Twins.

> [!NOTE]
> En función de su escenario, puede que tenga que realizar más cambios en el registro de la aplicación. Estos son algunos de los requisitos comunes que puede tener que cumplir:
> * Activar el acceso de cliente público
> * Establecer direcciones URL de respuesta específicas para el acceso web y de escritorio
> * Permitir flujos de autenticación de OAuth2 implícitos
> * Si su suscripción a Azure se crea con una cuenta de Microsoft como Live, Xbox o Hotmail, debe establecer *signInAudience* en el registro de la aplicación para admitir cuentas personales.
> La forma más fácil de configurar estas opciones es usar [Azure Portal](https://portal.azure.com/). Para más información sobre este proceso, vea [Registro de una aplicación con la plataforma de identidad de Microsoft](https://docs.microsoft.com/graph/auth-register-app-v2).

## <a name="write-client-app-authentication-code-net-c-sdk"></a>Escritura del código de autenticación de la aplicación cliente: SDK de .NET (C#)

En esta sección se describe el código que se debe incluir en la aplicación cliente para completar el proceso de autenticación mediante el SDK de .NET (C#).
El SDK de C# para Azure Digital Twins forma parte del SDK de Azure para .NET. Se encuentra aquí: [Biblioteca de cliente de Digital Twins de Azure IoT para .NET](https://github.com/Azure/azure-sdk-for-net/tree/master/sdk/digitaltwins/Azure.DigitalTwins.Core).

### <a name="prerequisites"></a>Requisitos previos

Si no tiene un proyecto de aplicación cliente de inicio ya configurado, cree un proyecto de .NET básico para usarlo con este tutorial.

Para poder usar el SDK de .NET, debe incluir los siguientes paquetes en el proyecto:
* `Azure.DigitalTwins.Core` (versión `1.0.0-preview.2`)
* `Azure.Identity`

En función de las herramientas que elija, puede hacerlo con el administrador de paquetes de Visual Studio o con la herramienta de línea de comandos `dotnet`. 

### <a name="authentication-and-client-creation-net"></a>Autenticación y creación de cliente: .NET

Para autenticarse con el SDK de .NET, use uno de los métodos de obtención de credenciales que se definen en la biblioteca [Azure.Identity](https://docs.microsoft.com/dotnet/api/azure.identity?view=azure-dotnet).

A continuación, se muestran dos de uso común: 
* [InteractiveBrowserCredential](https://docs.microsoft.com/dotnet/api/azure.identity.interactivebrowsercredential?view=azure-dotnet). Este método está pensado para aplicaciones interactivas y abrirá un explorador web para la autenticación.
* [ManagedIdentityCredential](https://docs.microsoft.com/dotnet/api/azure.identity.managedidentitycredential?view=azure-dotnet). Este método funciona muy bien en los casos en los que necesita [identidades administradas (MSI)](https://docs.microsoft.com/azure/active-directory/managed-identities-azure-resources/overview), por ejemplo, al trabajar con Azure Functions. 

También necesitará las siguientes instrucciones using:

```csharp
using Azure.Identity;
using Azure.DigitalTwins.Core;
```

Para usar las credenciales interactivas del explorador para crear un cliente de SDK autenticado, agregue este código:

```csharp
// Your client / app registration ID
private static string clientId = "<your-client-ID>"; 
// Your tenant / directory ID
private static string tenantId = "<your-tenant-ID>";
// The URL of your instance, starting with the protocol (https://)
private static string adtInstanceUrl = "<your-Azure-Digital-Twins-instance-URL>";

//...

DigitalTwinsClient client;
try
{
    var credential = new InteractiveBrowserCredential(tenantId, clientId);
    client = new DigitalTwinsClient(new Uri(adtInstanceUrl), credential);
} catch(Exception e)
{
    Console.WriteLine($"Authentication or client creation error: {e.Message}");
    Environment.Exit(0);
}
```

>[!NOTE]
> Aunque puede especificar el identificador de cliente, el identificador de inquilino y la dirección URL de la instancia directamente en el código, como se muestra arriba, se recomienda que, en su lugar, el código obtenga estos valores de un archivo de configuración o una variable de entorno.

En una función de Azure, puede usar las credenciales de identidad administrada de la siguiente manera:

```csharp
ManagedIdentityCredential cred = new ManagedIdentityCredential(adtAppId);
DigitalTwinsClientOptions opts = 
    new DigitalTwinsClientOptions { Transport = new HttpClientTransport(httpClient) });
client = new DigitalTwinsClient(new Uri(adtInstanceUrl), cred, opts);
```

Consulte [ Configure una función de Azure para procesar los datos](how-to-create-azure-function.md), a fin de obtener un ejemplo más completo en el que se explican algunas de las opciones de configuración importantes en el contexto de las funciones.

Además, para usar la autenticación en una función, recuerde:
* [Habilitar una entidad administrada](https://docs.microsoft.com/azure/app-service/overview-managed-identity?tabs=dotnet).
* [Variables de entorno](https://docs.microsoft.com/sandbox/functions-recipes/environment-variables?tabs=csharp)
* Asignar permisos a la aplicación de funciones que le permiten acceder a las API de Digital Twins. Consulte [ Configuración de una función de Azure para procesar datos](how-to-create-azure-function.md) para obtener más información.

## <a name="authentication-in-an-autorest-generated-sdk"></a>Autenticación en un SDK generado por AutoRest

Si no usa .NET, puede optar por crear una biblioteca SDK en el lenguaje que prefiera, como se describe en [ Creación de SDK personalizados para Azure Digital Twins con AutoRest](how-to-create-custom-sdks.md).

En esta sección se explica cómo realizar la autenticación en ese caso.

### <a name="prerequisites"></a>Requisitos previos

En este ejemplo, se usa un SDK de Typescript generado con AutoRest. Como resultado, también requiere:
* [msal-js](https://github.com/AzureAD/microsoft-authentication-library-for-js)
* [ms-rest-js](https://github.com/Azure/ms-rest-js)

### <a name="minimal-authentication-code-sample"></a>Ejemplo de código de autenticación mínimo

Para autenticar una aplicación .NET con los servicios de Azure, puede usar el siguiente código mínimo dentro de la aplicación cliente.

Necesitará el *Id. de la aplicación (cliente)* y el *Id. de directorio (inquilino)* anteriores, así como la dirección URL de la instancia de Azure Digital Twins.

> [!TIP]
> La dirección URL de la instancia de Azure Digital Twins se crea con la adición de *https://* al principio del *nombre de host* de la instancia de Azure Digital Twins. Para ver el *nombre de host*, junto con todas las propiedades de la instancia, puede ejecutar `az dt show --dt-name <your-Azure-Digital-Twins-instance>`. Puede usar el comando `az account show --query tenantId` para ver el *Id. de directorio (inquilino)* . 

```javascript
import * as Msal from "msal";
import { TokenCredentials } from "@azure/ms-rest-js";
// Autorest-generated SDK
import { AzureDigitalTwinsAPI } from './azureDigitalTwinsAPI';

// Client / app registration ID
var ClientId = "<your-client-ID>";
// Azure tenant / directory ID
var TenantId = "<your-tenant-ID>";
// URL of the Azure Digital Twins instance
var AdtInstanceUrl = "<your-instance-URL>"; 

var AdtAppId = "https://digitaltwins.azure.net";

let client = null;

export async function login() {

    const msalConfig = {
        auth: {
            clientId: ClientId,
            redirectUri: "http://localhost:3000",
            authority: "https://login.microsoftonline.com/"+TenantId
        }
    };

    const msalInstance = new Msal.UserAgentApplication(msalConfig);

    msalInstance.handleRedirectCallback((error, response) => {
        // handle redirect response or error
    });

    var loginRequest = {
        scopes: [AdtAppId + "/.default"] 
    };

    try {
        await msalInstance.loginPopup(loginRequest)
        var accessToken;
        // if the user is already logged in you can acquire a token
        if (msalInstance.getAccount()) {
            var tokenRequest = {
                scopes: [AdtAppId + "/.default"]
            };
            try {
                const response = await msalInstance.acquireTokenSilent(tokenRequest);
                accessToken = response.accessToken;
            } catch (err) {
                if (err.name === "InteractionRequiredAuthError") {
                    const response = await msalInstance.acquireTokenPopup(tokenRequest)
                    accessToken = response.accessToken;
                }
            }
        }
        if (accessToken!=null)
        {
            var tokenCredentials = new TokenCredentials(accessToken);
                
            // Add token and server URL to service instance
            const clientConfig = {
                baseUri: AdtInstanceUrl
            };
            client = new AzureDigitalTwinsAPI(tokenCredentials, clientConfig);
            appDataStore.client = client;
        }
    } catch (err) {
        ...
    }
}
```

Tenga en cuenta una vez más que donde el código anterior especifica el identificador de cliente, el identificador de inquilino y la dirección URL de la instancia directamente en el código para simplificar, se recomienda que, en su lugar, el código obtenga estos valores de un archivo de configuración o una variable de entorno.

MSAL tiene muchas más opciones que puede usar para implementar elementos, como el almacenamiento en caché y otros flujos de autenticación. Para más información, vea [Introducción a la Biblioteca de autenticación de Microsoft (MSAL)](../active-directory/develop/msal-overview.md).

## <a name="next-steps"></a>Pasos siguientes

Obtenga más información sobre cómo funciona la seguridad en Azure Digital Twins:
* [Conceptos: Seguridad para las soluciones de Azure Digital Twins](concepts-security.md)

O bien, ahora que la autenticación está configurada, continúe con la creación de modelos en la instancia:
* [Procedimiento: Administración de modelos personalizados](how-to-manage-model.md)