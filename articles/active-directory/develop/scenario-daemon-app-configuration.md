---
title: 'Configuración de aplicaciones de demonio que llaman a las API web: Plataforma de identidad de Microsoft | Azure'
description: Aprenda a configurar el código para la aplicación de demonio que llama a las API web (configuración de aplicaciones).
services: active-directory
author: jmprieur
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: conceptual
ms.workload: identity
ms.date: 09/19/2020
ms.author: jmprieur
ms.custom: aaddev, devx-track-python
ms.openlocfilehash: 6864502a9d338a786e1e77dbf9888a7818bb94e9
ms.sourcegitcommit: 1bf144dc5d7c496c4abeb95fc2f473cfa0bbed43
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/24/2020
ms.locfileid: "95748655"
---
# <a name="daemon-app-that-calls-web-apis---code-configuration"></a>Aplicación de demonio que llama a las API web: configuración de código

Aprenda a configurar el código para la aplicación de demonio que llama a las API web.

## <a name="msal-libraries-that-support-daemon-apps"></a>Bibliotecas MSAL que admiten aplicaciones de demonio

Estas bibliotecas de Microsoft son compatibles con las aplicaciones de demonio:

  Biblioteca MSAL | Descripción
  ------------ | ----------
  ![MSAL.NET](media/sample-v2-code/logo_NET.png) <br/> MSAL.NET  | Se admiten las plataformas .NET Framework y .NET Core para compilar aplicaciones de demonio. (UWP, Xamarin.iOS y Xamarin.Android no se admiten porque esas plataformas se usan para crear aplicaciones cliente públicas).
  ![Python](media/sample-v2-code/logo_python.png) <br/> Python de MSAL | Compatibilidad con aplicaciones de demonio en Python.
  ![Java](media/sample-v2-code/logo_java.png) <br/> Java de MSAL | Compatibilidad con aplicaciones de demonio en Java.

## <a name="configure-the-authority"></a>Configuración de la autoridad

Las aplicaciones de demonio usan permisos de aplicación en lugar de permisos delegados. Por lo tanto, el tipo de cuenta admitido no puede ser una cuenta en ningún directorio de la organización ni una cuenta de Microsoft personal (por ejemplo, Skype, Xbox, Outlook.com). No hay ningún administrador de inquilinos para otorgar consentimiento a una aplicación de demonio para una cuenta de Microsoft personal. Debe elegir *cuentas de mi organización* o *cuentas de cualquier organización*.

La autoridad especificada en la configuración de la aplicación debe tener inquilinos (con identificador o nombre de dominio asociado a la organización).

Incluso si quiere proporcionar una herramienta para varios inquilinos, debe usar un id. de inquilino o un nombre de dominio y **no** debe usar `common` o `organizations` con este flujo, ya que el servicio no puede deducir de manera confiable qué inquilino debe usarse.

## <a name="configure-and-instantiate-the-application"></a>Configuración y creación de instancias de la aplicación

En las bibliotecas MSAL, las credenciales del cliente (secreto o certificado) se pasan como parámetro de la construcción de la aplicación cliente confidencial.

> [!IMPORTANT]
> Aunque se trate de una aplicación de consola que se ejecuta como servicio, si es una aplicación de demonio debe ser una aplicación cliente confidencial.

### <a name="configuration-file"></a>Archivo de configuración

El archivo de configuración define:

- La instancia de nube y el identificador de inquilino, que forman la *autoridad*.
- El identificador de cliente que recibió del registro de la aplicación.
- Un secreto de cliente o un certificado.

# <a name="net"></a>[.NET](#tab/dotnet)

A continuación se muestra un ejemplo de definición de la configuración en un archivo [*appsettings.json*](https://github.com/Azure-Samples/active-directory-dotnetcore-daemon-v2/blob/master/1-Call-MSGraph/daemon-console/appsettings.json). Este ejemplo se ha tomado del ejemplo de código [Demonio de consola de .NET Core](https://github.com/Azure-Samples/active-directory-dotnetcore-daemon-v2) de GitHub.

```json
{
  "Instance": "https://login.microsoftonline.com/{0}",
  "Tenant": "[Enter here the tenantID or domain name for your Azure AD tenant]",
  "ClientId": "[Enter here the ClientId for your application]",
  "ClientSecret": "[Enter here a client secret for your application]",
  "CertificateName": "[Or instead of client secret: Enter here the name of a certificate (from the user cert store) as registered with your application]"
}
```

Proporcione `ClientSecret` o `CertificateName`. Estos valores son excluyentes.

# <a name="python"></a>[Python](#tab/python)

Al crear un cliente confidencial con secretos de cliente, el archivo de configuración [parameters.json](https://github.com/Azure-Samples/ms-identity-python-daemon/blob/master/1-Call-MsGraph-WithSecret/parameters.json) en el ejemplo de [demonio de Python](https://github.com/Azure-Samples/ms-identity-python-daemon) es el siguiente:

```Json
{
  "authority": "https://login.microsoftonline.com/<your_tenant_id>",
  "client_id": "your_client_id",
  "scope": [ "https://graph.microsoft.com/.default" ],
  "secret": "The secret generated by AAD during your confidential app registration",
  "endpoint": "https://graph.microsoft.com/v1.0/users"
}
```

Al crear un cliente confidencial con certificados, el archivo de configuración [parameters.json](https://github.com/Azure-Samples/ms-identity-python-daemon/blob/master/2-Call-MsGraph-WithCertificate/parameters.json) en el ejemplo de [demonio de Python](https://github.com/Azure-Samples/ms-identity-python-daemon) es el siguiente:

```Json
{
  "authority": "https://login.microsoftonline.com/<your_tenant_id>",
  "client_id": "your_client_id",
  "scope": [ "https://graph.microsoft.com/.default" ],
  "thumbprint": "790E... The thumbprint generated by AAD when you upload your public cert",
  "private_key_file": "server.pem",
  "endpoint": "https://graph.microsoft.com/v1.0/users"
}
```

# <a name="java"></a>[Java](#tab/java)

```Java
 private final static String CLIENT_ID = "";
 private final static String AUTHORITY = "https://login.microsoftonline.com/<tenant>/";
 private final static String CLIENT_SECRET = "";
 private final static Set<String> SCOPE = Collections.singleton("https://graph.microsoft.com/.default");
```

---

### <a name="instantiate-the-msal-application"></a>Creación de una instancia de la aplicación MSAL

Para crear una instancia de la aplicación MSAL, debe agregar o importar el paquete MSAL (dependiendo del lenguaje), o agregar una referencia a él.

La construcción es diferente dependiendo de si usa secretos de cliente o certificados (o, como un escenario avanzado, aserciones firmadas).

#### <a name="reference-the-package"></a>Haga referencia al paquete

Haga referencia al paquete MSAL en el código de la aplicación.

# <a name="net"></a>[.NET](#tab/dotnet)

Agregue el paquete NuGet [Microsoft.Identity.Client](https://www.nuget.org/packages/Microsoft.Identity.Client) a la aplicación y, a continuación, agregue una directiva `using` en el código para hacer referencia a él.

En MSAL.NET, la aplicación cliente confidencial está representada por la interfaz `IConfidentialClientApplication`.

```csharp
using Microsoft.Identity.Client;
IConfidentialClientApplication app;
```

# <a name="python"></a>[Python](#tab/python)

```python
import msal
import json
import sys
import logging
```

# <a name="java"></a>[Java](#tab/java)

```java
import com.microsoft.aad.msal4j.ClientCredentialFactory;
import com.microsoft.aad.msal4j.ClientCredentialParameters;
import com.microsoft.aad.msal4j.ConfidentialClientApplication;
import com.microsoft.aad.msal4j.IAuthenticationResult;
import com.microsoft.aad.msal4j.IClientCredential;
import com.microsoft.aad.msal4j.MsalException;
import com.microsoft.aad.msal4j.SilentParameters;
```

---

#### <a name="instantiate-the-confidential-client-application-with-a-client-secret"></a>Creación de una instancia de aplicación cliente confidencial con un secreto de cliente

Este es el código para crear una instancia de aplicación cliente confidencial con un secreto de cliente:

# <a name="net"></a>[.NET](#tab/dotnet)

```csharp
app = ConfidentialClientApplicationBuilder.Create(config.ClientId)
           .WithClientSecret(config.ClientSecret)
           .WithAuthority(new Uri(config.Authority))
           .Build();
```

El elemento `Authority` es una concatenación de la instancia de nube y el identificador de inquilino, por ejemplo `https://login.microsoftonline.com/contoso.onmicrosoft.com` o `https://login.microsoftonline.com/eb1ed152-0000-0000-0000-32401f3f9abd`. En el archivo *appsettings.json* que se muestra en la sección [Archivo de configuración](#configuration-file), estos se representan mediante los valores `Instance` y `Tenant`, respectivamente.

En el ejemplo de código del que se tomó el fragmento de código anterior, `Authority` es una propiedad de la clase [AuthenticationConfig](https://github.com/Azure-Samples/active-directory-dotnetcore-daemon-v2/blob/ffc4a9f5d9bdba5303e98a1af34232b434075ac7/1-Call-MSGraph/daemon-console/AuthenticationConfig.cs#L61-L70) y se define como tal:

```csharp
/// <summary>
/// URL of the authority
/// </summary>
public string Authority
{
    get
    {
        return String.Format(CultureInfo.InvariantCulture, Instance, Tenant);
    }
}
```

# <a name="python"></a>[Python](#tab/python)

```Python
# Pass the parameters.json file as an argument to this Python script. E.g.: python your_py_file.py parameters.json
config = json.load(open(sys.argv[1]))

# Create a preferably long-lived app instance that maintains a token cache.
app = msal.ConfidentialClientApplication(
    config["client_id"], authority=config["authority"],
    client_credential=config["secret"],
    # token_cache=...  # Default cache is in memory only.
                       # You can learn how to use SerializableTokenCache from
                       # https://msal-python.rtfd.io/en/latest/#msal.SerializableTokenCache
    )
```

# <a name="java"></a>[Java](#tab/java)

```Java
IClientCredential credential = ClientCredentialFactory.createFromSecret(CLIENT_SECRET);

ConfidentialClientApplication cca =
        ConfidentialClientApplication
                .builder(CLIENT_ID, credential)
                .authority(AUTHORITY)
                .build();
```

---

#### <a name="instantiate-the-confidential-client-application-with-a-client-certificate"></a>Creación de una instancia de aplicación cliente confidencial con un certificado de cliente

Este es el código para compilar una aplicación con un certificado:

# <a name="net"></a>[.NET](#tab/dotnet)

```csharp
X509Certificate2 certificate = ReadCertificate(config.CertificateName);
app = ConfidentialClientApplicationBuilder.Create(config.ClientId)
    .WithCertificate(certificate)
    .WithAuthority(new Uri(config.Authority))
    .Build();
```

# <a name="python"></a>[Python](#tab/python)

```Python
# Pass the parameters.json file as an argument to this Python script. E.g.: python your_py_file.py parameters.json
config = json.load(open(sys.argv[1]))

# Create a preferably long-lived app instance that maintains a token cache.
app = msal.ConfidentialClientApplication(
    config["client_id"], authority=config["authority"],
    client_credential={"thumbprint": config["thumbprint"], "private_key": open(config['private_key_file']).read()},
    # token_cache=...  # Default cache is in memory only.
                       # You can learn how to use SerializableTokenCache from
                       # https://msal-python.rtfd.io/en/latest/#msal.SerializableTokenCache
    )
```

# <a name="java"></a>[Java](#tab/java)

En MSAL Java hay dos generadores para crear una instancia de la aplicación cliente confidencial con certificados:

```Java

InputStream pkcs12Certificate = ... ; /* Containing PCKS12-formatted certificate*/
string certificatePassword = ... ;    /* Contains the password to access the certificate */

IClientCredential credential = ClientCredentialFactory.createFromCertificate(pkcs12Certificate, certificatePassword);

ConfidentialClientApplication cca =
        ConfidentialClientApplication
                .builder(CLIENT_ID, credential)
                .authority(AUTHORITY)
                .build();
```

or

```Java
PrivateKey key = getPrivateKey(); /* RSA private key to sign the assertion */
X509Certificate publicCertificate = getPublicCertificate(); /* x509 public certificate used as a thumbprint */

IClientCredential credential = ClientCredentialFactory.createFromCertificate(key, publicCertificate);

ConfidentialClientApplication cca =
        ConfidentialClientApplication
                .builder(CLIENT_ID, credential)
                .authority(AUTHORITY)
                .build();
```

---

#### <a name="advanced-scenario-instantiate-the-confidential-client-application-with-client-assertions"></a>Escenario avanzado: Creación de una instancia de aplicación cliente confidencial con aserciones de cliente

# <a name="net"></a>[.NET](#tab/dotnet)

En lugar de un secreto de cliente o un certificado, la aplicación cliente confidencial también puede demostrar su identidad mediante aserciones de cliente.

MSAL.NET tiene dos métodos para proporcionar aserciones firmadas a la aplicación cliente confidencial:

- `.WithClientAssertion()`
- `.WithClientClaims()`

Cuando use `WithClientAssertion`, tiene que proporcionar un JWT firmado. Este escenario avanzado se detalla en las [aserciones de cliente](msal-net-client-assertions.md).

```csharp
string signedClientAssertion = ComputeAssertion();
app = ConfidentialClientApplicationBuilder.Create(config.ClientId)
                                          .WithClientAssertion(signedClientAssertion)
                                          .Build();
```

Cuando utilice `WithClientClaims`, MSAL .NET producirá una aserción firmada que contiene las notificaciones esperadas por Azure AD más las notificaciones de cliente adicionales que se quieren enviar.
Este código muestra cómo hacerlo:

```csharp
string ipAddress = "192.168.1.2";
var claims = new Dictionary<string, string> { { "client_ip", ipAddress } };
X509Certificate2 certificate = ReadCertificate(config.CertificateName);
app = ConfidentialClientApplicationBuilder.Create(config.ClientId)
                                          .WithAuthority(new Uri(config.Authority))
                                          .WithClientClaims(certificate, claims)
                                          .Build();
```

De nuevo, para una información más detallada, consulte [aserciones de cliente](msal-net-client-assertions.md).

# <a name="python"></a>[Python](#tab/python)

En MSAL Python, puede proporcionar notificaciones de cliente mediante las notificaciones que se firmarán con esta clave privada de `ConfidentialClientApplication`.

```Python
# Pass the parameters.json file as an argument to this Python script. E.g.: python your_py_file.py parameters.json
config = json.load(open(sys.argv[1]))

# Create a preferably long-lived app instance that maintains a token cache.
app = msal.ConfidentialClientApplication(
    config["client_id"], authority=config["authority"],
    client_credential={"thumbprint": config["thumbprint"], "private_key": open(config['private_key_file']).read()},
    client_claims = {"client_ip": "x.x.x.x"}
    # token_cache=...  # Default cache is in memory only.
                       # You can learn how to use SerializableTokenCache from
                       # https://msal-python.rtfd.io/en/latest/#msal.SerializableTokenCache
    )
```

Para más información, consulte la documentación de referencia de MSAL Python para [ConfidentialClientApplication](https://msal-python.readthedocs.io/en/latest/#msal.ClientApplication.__init__).

# <a name="java"></a>[Java](#tab/java)

```Java
IClientCredential credential = ClientCredentialFactory.createFromClientAssertion(assertion);

ConfidentialClientApplication cca =
        ConfidentialClientApplication
                .builder(CLIENT_ID, credential)
                .authority(AUTHORITY)
                .build();
```

---

## <a name="next-steps"></a>Pasos siguientes

# <a name="net"></a>[.NET](#tab/dotnet)

Avance al siguiente artículo de este escenario, [Obtención de un token para la aplicación](./scenario-daemon-acquire-token.md?tabs=dotnet).

# <a name="python"></a>[Python](#tab/python)

Avance al siguiente artículo de este escenario, [Obtención de un token para la aplicación](./scenario-daemon-acquire-token.md?tabs=python).

# <a name="java"></a>[Java](#tab/java)

Avance al siguiente artículo de este escenario, [Obtención de un token para la aplicación](./scenario-daemon-acquire-token.md?tabs=java).

---
