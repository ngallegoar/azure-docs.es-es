---
title: archivo de inclusión
description: archivo de inclusión
services: azure-communication-services
author: tomaschladek
manager: nmurav
ms.service: azure-communication-services
ms.subservice: azure-communication-services
ms.date: 08/20/2020
ms.topic: include
ms.custom: include file
ms.author: tchladek
ms.openlocfilehash: 6be69a1ec20ed859769c944a2f66de1310c09507
ms.sourcegitcommit: 230d5656b525a2c6a6717525b68a10135c568d67
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/19/2020
ms.locfileid: "94915188"
---
## <a name="prerequisites"></a>Prerrequisitos

- Una cuenta de Azure con una suscripción activa. [Cree una cuenta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).
- [Kit de desarrollo de Java (JDK)](/java/azure/jdk/?preserve-view=true&view=azure-java-stable), versión 8 o posterior.
- [Apache Maven](https://maven.apache.org/download.cgi).
- Un recurso de Communication Services implementado y una cadena de conexión. [Cree un recurso de Communication Services](../create-communication-resource.md).

## <a name="setting-up"></a>Instalación

### <a name="create-a-new-java-application"></a>Creación de una aplicación Java

Abra el terminal o la ventana de comandos. Vaya al directorio en el que quiere crear la aplicación Java. Ejecute el siguiente comando para generar el proyecto de Java a partir de la plantilla maven-archetype-quickstart.

```console
mvn archetype:generate -DgroupId=com.communication.quickstart -DartifactId=communication-quickstart -DarchetypeArtifactId=maven-archetype-quickstart -DarchetypeVersion=1.4 -DinteractiveMode=false
```

Observará que la tarea "generar" creó un directorio con el mismo nombre que el objeto `artifactId`. En este directorio, el directorio src/main/java contiene el código fuente del proyecto, el directorio `src/test/java directory` contiene el origen de la prueba y el archivo `pom.xml` es el modelo de objetos del proyecto o POM.

### <a name="install-the-package"></a>Instalar el paquete

Abra el archivo **pom.xml** en el editor de texto. Agregue el siguiente elemento de dependencia al grupo de dependencias.

```xml
<dependency>
    <groupId>com.azure</groupId>
    <artifactId>azure-communication-administration</artifactId>
    <version>1.0.0-beta.3</version> 
</dependency>
```

### <a name="set-up-the-app-framework"></a>Instalación del marco de la aplicación

Desde el directorio del proyecto:

1. Vaya al directorio */src/main/java/com/communication/quickstart*.
1. Abra el archivo *App.java* en el editor.
1. Reemplace la instrucción `System.out.println("Hello world!");`.
1. Agregue directivas `import`.

Use el código siguiente para empezar:

```java
import com.azure.communication.administration.*;
import com.azure.communication.common.*;
import java.io.*;
import java.util.*;
import java.time.*;

import com.azure.core.http.*;

public class App
{
    public static void main( String[] args ) throws IOException
    {
        System.out.println("Azure Communication Services - Access Tokens Quickstart");
        // Quickstart code goes here
    }
}
```

## <a name="authenticate-the-client"></a>Autenticar el cliente

Cree una instancia de `CommunicationIdentityClient` con la clave de acceso y el punto de conexión del recurso. Obtenga información sobre cómo [administrar la cadena de conexión del recurso](../create-communication-resource.md#store-your-connection-string).

Agregue el código siguiente al método `main`:

```java
// Your can find your endpoint and access key from your resource in the Azure Portal
String endpoint = "https://<RESOURCE_NAME>.communication.azure.com";
String accessKey = "SECRET";

// Create an HttpClient builder of your choice and customize it
// Use com.azure.core.http.netty.NettyAsyncHttpClientBuilder if that suits your needs
// -> Add "import com.azure.core.http.netty.*;"
// -> Add azure-core-http-netty dependency to file pom.xml

HttpClient httpClient = new NettyAsyncHttpClientBuilder().build();

CommunicationIdentityClient communicationIdentityClient = new CommunicationIdentityClientBuilder()
    .endpoint(endpoint)
    .accessKey(accessKey)
    .httpClient(httpClient)
    .buildClient();
```

Puede inicializar el cliente con cualquier cliente HTTP personalizado que implemente la interfaz `com.azure.core.http.HttpClient`. En el código anterior se muestra el uso del [cliente HTTP Netty de Azure Core](/java/api/overview/azure/core-http-netty-readme?preserve-view=true&view=azure-java-stable) proporcionado por `azure-core`.

También puede proporcionar toda la cadena de conexión mediante la función connectionString() en lugar de proporcionar el punto de conexión y la clave de acceso. 
```java
// Your can find your connection string from your resource in the Azure Portal
String connectionString = "<connection_string>";
CommunicationIdentityClient communicationIdentityClient = new CommunicationIdentityClientBuilder()
    .connectionString(connectionString)
    .httpClient(httpClient)
    .buildClient();
```

## <a name="create-an-identity"></a>Creación de una identidad

Azure Communication Services mantiene un directorio de identidades ligero. Use el método `createUser` para crear una nueva entrada en el directorio con un `Id` único. Almacene la identidad recibida con la asignación a los usuarios de la aplicación. Por ejemplo, almacenándolos en la base de datos del servidor de aplicaciones. La identidad es necesaria más adelante para emitir tokens de acceso.

```java
CommunicationUser identity = communicationIdentityClient.createUser();
System.out.println("\nCreated an identity with ID: " + identity.getId());
```

## <a name="issue-access-tokens"></a>Emitir tokens de acceso

Use el método `issueToken` para emitir un token de acceso para la identidad de Communication Services existente. El parámetro `scopes` define un conjunto de primitivas que autorizará este token de acceso. Consulte la [lista de plataformas admitidas](../../concepts/authentication.md). Se puede crear una nueva instancia del parámetro `user` en función de la representación de cadena de la identidad de Azure Communication Services.

```java
// Issue an access token with the "voip" scope for an identity
List<String> scopes = new ArrayList<>(Arrays.asList("voip"));
CommunicationUserToken response = communicationIdentityClient.issueToken(identity, scopes);
OffsetDateTime expiresOn = response.getExpiresOn();
String token = response.getToken();
String identityId = response.getUser().getId();
System.out.println("\nIssued a access token with 'voip' scope for identity with ID: " + identityId + ": " + token);
```

Los tokens de acceso son credenciales de corta duración que deben volver a emitirse. No hacerlo puede provocar una interrupción de la experiencia de los usuarios respecto a la aplicación. La propiedad de respuesta `expiresAt` indica la duración del token de acceso.

## <a name="refresh-access-tokens"></a>Tokens de acceso de actualización

Para actualizar un token de acceso, use el objeto `CommunicationUser` para volver a emitir:

```java  
// Value existingIdentity represents identity of Azure Communication Services stored during identity creation
CommunicationUser identity = new CommunicationUser(existingIdentity);
response = communicationIdentityClient.issueToken(identity, scopes);
```

## <a name="revoke-access-tokens"></a>Revocación de los tokens de acceso

En algunos casos, puede revocar explícitamente los tokens de acceso. Por ejemplo, cuando el usuario de una aplicación cambia la contraseña que usa para autenticarse en el servicio. El método `revokeTokens` invalida todos los tokens de acceso activos emitidos para la identidad.

```java  
communicationIdentityClient.revokeTokens(identity, OffsetDateTime.now());
System.out.println("\nRevoked access tokens for the user with ID: " + identity.getId());
```

## <a name="delete-an-identity"></a>Eliminación de una identidad

La eliminación de una identidad revoca todos los tokens de acceso activos e impide que se emitan tokens de acceso posteriores para la identidad. También quita todo el contenido conservado asociado a la identidad.

```java
communicationIdentityClient.deleteUser(identity);
System.out.println("\nSuccessfully deleted the identity with ID: " + identity.getId());
```

## <a name="run-the-code"></a>Ejecución del código

Navegue hasta el directorio que contiene el archivo *pom.xml* y compile el proyecto mediante el siguiente comando `mvn`.

```console
mvn compile
```

A continuación, compile el paquete.

```console
mvn package
```

Ejecute el siguiente comando `mvn` para ejecutar la aplicación.

```console
mvn exec:java -Dexec.mainClass="com.communication.quickstart.App" -Dexec.cleanupDaemonThreads=false
```