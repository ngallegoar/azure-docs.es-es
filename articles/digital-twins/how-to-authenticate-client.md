---
title: Escritura de código de autenticación de aplicación
titleSuffix: Azure Digital Twins
description: Vea cómo escribir código de autenticación en una aplicación cliente
author: baanders
ms.author: baanders
ms.date: 10/7/2020
ms.topic: how-to
ms.service: digital-twins
ms.openlocfilehash: d71a7535c40d240b6c9bf53cff906f12b4b8b5df
ms.sourcegitcommit: 957c916118f87ea3d67a60e1d72a30f48bad0db6
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/19/2020
ms.locfileid: "92204306"
---
# <a name="write-client-app-authentication-code"></a>Escritura de código de autenticación de aplicación cliente

Después de [configurar una instancia y la autenticación de Azure Digital Twins](how-to-set-up-instance-portal.md), puede crear una aplicación cliente para usarla para interactuar con la instancia. Una vez configurado un proyecto cliente de inicio, necesitará **escribir código en esa aplicación cliente para autenticarla** en la instancia de Azure Digital Twins.

Azure Digital Twins realiza la autenticación mediante [tokens de seguridad de Azure AD basados en OAUTH 2.0](../active-directory/develop/security-tokens.md#json-web-tokens-jwts-and-claims). Para autenticar el SDK, deberá obtener un token de portador con los permisos correctos para Azure Digital Twins y pasarlo junto con las llamadas de API. 

En este artículo se describe cómo obtener credenciales mediante la biblioteca de cliente `Azure.Identity`. Aunque en este artículo se muestran ejemplos de código en C#, como lo que escribiría para el [SDK de .NET (C#)](https://www.nuget.org/packages/Azure.DigitalTwins.Core), puede usar una versión de `Azure.Identity` independientemente del SDK que use (para obtener más información sobre los SDKs disponibles para Azure Digital Twins, consulte [*Procedimiento: uso de las API y los SDK de Azure Digital Twins*](how-to-use-apis-sdks.md)).

## <a name="prerequisites"></a>Requisitos previos

En primer lugar, realice los pasos de configuración de [*Configuración de una instancia y autenticación*](how-to-set-up-instance-portal.md). Así se asegura de tener una instancia de Azure Digital Twins, de que el usuario tiene permisos de acceso y de que ha configurado permisos para las aplicaciones cliente. Después de toda esta configuración, está listo para escribir el código de la aplicación cliente.

Para continuar, necesita un proyecto de aplicación cliente en el que escribir el código. Si aún no tiene un proyecto de aplicación cliente configurado, cree un proyecto básico en el lenguaje que prefiera para usarlo con este tutorial.

## <a name="common-authentication-methods-with-azureidentity"></a>Métodos de autenticación comunes con Azure.Identity

`Azure.Identity` es una biblioteca de cliente que proporciona varios métodos para obtener credenciales que puede usar para obtener un token de portador y autenticarse con el SDK. Aunque en este artículo se proporcionan ejemplos en C# , puede ver `Azure.Identity` para varios idiomas, incluido...
* [.NET (C#)](/dotnet/api/azure.identity?preserve-view=true&view=azure-dotnet)
* [Java](/java/api/overview/azure/identity-readme?preserve-view=true&view=azure-java-stable)
* [JavaScript](/javascript/api/overview/azure/identity-readme?preserve-view=true&view=azure-node-latest)
* [Python](/python/api/overview/azure/identity-readme?preserve-view=true&view=azure-python)

Tres métodos comunes de obtención de credenciales en `Azure.Identity` son:
* [DefaultAzureCredential](/dotnet/api/azure.identity.defaultazurecredential?preserve-view=true&view=azure-dotnet) proporciona un flujo de autenticación `TokenCredential` predeterminado para las aplicaciones que se implementarán en Azure y es **la opción recomendada para el desarrollo local** . También se puede habilitar para probar los otros dos métodos recomendados en este artículo: contiene `ManagedIdentityCredential` y puede tener acceso a `InteractiveBrowserCredential` con una variable de configuración.
* [ManagedIdentityCredential](/dotnet/api/azure.identity.managedidentitycredential?preserve-view=true&view=azure-dotnet) funciona bien en los casos en los que necesita [identidades administradas (MSI)](../active-directory/managed-identities-azure-resources/overview.md) y es un buen candidato para trabajar con Azure Functions y realizar implementaciones en servicios de Azure.
* [InteractiveBrowserCredential](/dotnet/api/azure.identity.interactivebrowsercredential?preserve-view=true&view=azure-dotnet) está diseñado para aplicaciones interactivas y se puede usar para crear un cliente de SDK autenticado.

En los ejemplos siguientes se muestra cómo usar cada uno de estos con el SDK de .NET (C#).

## <a name="authentication-examples-net-c-sdk"></a>Ejemplos de autenticación: SDK de .NET (C#)

En esta sección se muestra un ejemplo en C# sobre el uso del SDK para .NET proporcionado para escribir el código de autenticación.

En primer lugar, incluya el paquete del SDK `Azure.DigitalTwins.Core` y el paquete `Azure.Identity` en el proyecto. En función de las herramientas que elija, puede incluir los paquetes con el administrador de paquetes de Visual Studio o con la herramienta de línea de comandos `dotnet`. 

También deberá agregar las siguientes instrucciones using al código del proyecto:

```csharp
using Azure.Identity;
using Azure.DigitalTwins.Core;
```

A continuación, agregue código para obtener credenciales mediante uno de los métodos de `Azure.Identity`.

### <a name="defaultazurecredential-method"></a>Método DefaultAzureCredential

[DefaultAzureCredential](/dotnet/api/azure.identity.defaultazurecredential?preserve-view=true&view=azure-dotnet) proporciona un flujo de autenticación `TokenCredential` predeterminado para las aplicaciones que se implementarán en Azure y es **la opción recomendada para el desarrollo local** .

Para usar las credenciales predeterminadas de Azure, necesitará la dirección URL de la instancia de Azure Digital Twins ([instrucciones para encontrarla](how-to-set-up-instance-portal.md#verify-success-and-collect-important-values)).

A continuación se muestra un ejemplo de código para agregar `DefaultAzureCredential` al proyecto:

```csharp
// The URL of your instance, starting with the protocol (https://)
private static string adtInstanceUrl = "https://<your-Azure-Digital-Twins-instance-URL>";

//...

DigitalTwinsClient client;
try
{
    var credential = new DefaultAzureCredential();
    client = new DigitalTwinsClient(new Uri(adtInstanceUrl), credential);
} catch(Exception e)
{
    Console.WriteLine($"Authentication or client creation error: {e.Message}");
    Environment.Exit(0);
}
```

### <a name="managedidentitycredential-method"></a>Método ManagedIdentityCredential

El método [ManagedIdentityCredential](/dotnet/api/azure.identity.managedidentitycredential?preserve-view=true&view=azure-dotnet) funciona muy bien en los casos en los que necesita [identidades administradas (MSI)](../active-directory/managed-identities-azure-resources/overview.md), por ejemplo, al trabajar con Azure Functions.

Esto significa que puede usar `ManagedIdentityCredential` en el mismo proyecto que `DefaultAzureCredential` o `InteractiveBrowserCredential` para autenticar otra parte del proyecto.

Para usar las credenciales predeterminadas de Azure, necesitará la dirección URL de la instancia de Azure Digital Twins ([instrucciones para encontrarla](how-to-set-up-instance-portal.md#verify-success-and-collect-important-values)).

En una función de Azure, puede usar las credenciales de identidad administrada de la siguiente manera:

```csharp
ManagedIdentityCredential cred = new ManagedIdentityCredential(adtAppId);
DigitalTwinsClientOptions opts = 
    new DigitalTwinsClientOptions { Transport = new HttpClientTransport(httpClient) });
client = new DigitalTwinsClient(new Uri(adtInstanceUrl), cred, opts);
```

### <a name="interactivebrowsercredential-method"></a>Método InteractiveBrowserCredential

El método [InteractiveBrowserCredential](/dotnet/api/azure.identity.interactivebrowsercredential?preserve-view=true&view=azure-dotnet) está pensado para aplicaciones interactivas y abrirá un explorador web para la autenticación. Puede utilizarlo en lugar de `DefaultAzureCredential` en los casos en los que necesite una autenticación interactiva.

Para usar las credenciales interactivas del explorador, necesitará un **registro de la aplicación** que tenga permisos para las API de Azure Digital Twins. Para conocer los pasos para configurar el registro de esta aplicación, consulte [*Guía paso a paso: creación de un registro de aplicación*](how-to-create-app-registration.md). Una vez configurado el registro de la aplicación, necesitará...
* El *id. de la aplicación (cliente)* del registro de la aplicación ([instrucciones para buscarlo](how-to-create-app-registration.md#collect-client-id-and-tenant-id))
* El *id. del directorio (inquilino)* del registro de la aplicación ([instrucciones para buscarlo](how-to-create-app-registration.md#collect-client-id-and-tenant-id))
* la dirección URL de la instancia de Azure Digital Twins ([instrucciones para encontrarla](how-to-set-up-instance-portal.md#verify-success-and-collect-important-values)).

Este es un ejemplo del código para crear un cliente de SDK autenticado mediante `InteractiveBrowserCredential`.

```csharp
// Your client / app registration ID
private static string clientId = "<your-client-ID>"; 
// Your tenant / directory ID
private static string tenantId = "<your-tenant-ID>";
// The URL of your instance, starting with the protocol (https://)
private static string adtInstanceUrl = "https://<your-Azure-Digital-Twins-instance-URL>";

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

#### <a name="other-notes-about-authenticating-azure-functions"></a>Otras notas sobre la autenticación de Azure Functions

Vea [*Configuración de una función de Azure para procesar datos*](how-to-create-azure-function.md) para obtener un ejemplo más completo en el que se explican algunas de las opciones de configuración importantes en el contexto de las funciones.

Además, para usar la autenticación en una función, recuerde:
* [Habilitar una entidad administrada](../app-service/overview-managed-identity.md?tabs=dotnet).
* Usar [variables de entorno](/sandbox/functions-recipes/environment-variables?tabs=csharp) según sea necesario
* Asignar permisos a la aplicación de funciones que le permiten acceder a las API de Digital Twins. Para obtener más información sobre los procesos de Azure Functions, vea [ *Configuración de una función de Azure para procesar datos*](how-to-create-azure-function.md).

## <a name="other-credential-methods"></a>Otros métodos de credenciales

Si los escenarios de autenticación resaltados anteriores no cubren las necesidades de la aplicación, puede explorar otros tipos de autenticación que se ofrecen en la [**plataforma de Microsoft Identity**](../active-directory/develop/v2-overview.md#getting-started). La documentación de esta plataforma abarca escenarios de autenticación adicionales organizados por tipo de aplicación.

## <a name="next-steps"></a>Pasos siguientes

Obtenga más información sobre cómo funciona la seguridad en Azure Digital Twins:
* [*Conceptos: Seguridad para las soluciones de Azure Digital Twins*](concepts-security.md)

O bien, ahora que la autenticación está configurada, continúe con la creación de modelos en la instancia:
* [*Procedimiento: Administración de modelos personalizados*](how-to-manage-model.md)