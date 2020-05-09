---
title: Guía de migración de Python ADAL a MSAL | Azure
description: Obtenga información sobre cómo migrar la aplicación para Python de la Biblioteca de autenticación de Azure Active Directory (ADAL) a la Biblioteca de autenticación de Microsoft (MSAL).
services: active-directory
titleSuffix: Microsoft identity platform
author: rayluo
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: conceptual
ms.tgt_pltfrm: Python
ms.workload: identity
ms.date: 11/11/2019
ms.author: rayluo
ms.reviewer: rayluo, nacanuma, twhitney
ms.custom: aaddev
ms.openlocfilehash: a3f95383979fd47b3baaec946f724533461729b8
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/28/2020
ms.locfileid: "82128041"
---
# <a name="adal-to-msal-migration-guide-for-python"></a>Guía de migración de ADAL a MSAL para Python

En este artículo se resaltan los cambios que debe realizar para migrar una aplicación que usa la biblioteca de autenticación de Azure Active Directory (ADAL) para que use la biblioteca de autenticación de Microsoft (MSAL).

## <a name="difference-highlights"></a>Diferencias destacadas

ADAL funciona con el punto de conexión de Azure Active Directory (Azure AD) v1.0. La biblioteca de autenticación de Microsoft (MSAL) funciona con la Plataforma de identidad de Microsoft, que anteriormente se conocía como punto de conexión de Azure Active Directory v2.0. La Plataforma de identidad de Microsoft se diferencia de Azure AD v1.0 en lo siguiente:

Es compatible con:
  - Cuentas profesionales y educativas (cuentas aprovisionadas por Azure AD).
  - Cuentas personales (como Outlook.com o Hotmail.com)
  - Los clientes que traen su propio correo electrónico o identidad social (como LinkedIn, Facebook, Google) a través de la oferta de Azure AD B2C

- Es compatibles con las normas:
  - OAuth v2.0
  - OpenID Connect (OIDC)

Consulte [¿Qué es diferente del punto de conexión de la Plataforma de identidad de Microsoft (v2.0)?](https://docs.microsoft.com/azure/active-directory/develop/azure-ad-endpoint-comparison) para más detalles.

### <a name="scopes-not-resources"></a>Ámbitos, no recursos

ADAL para Python adquiere tokens para los recursos, pero MSAL para Python adquiere tokens para los ámbitos. La superficie de API en MSAL para Python ya no tiene el parámetro de recurso. Tendría que proporcionar ámbitos como una lista de cadenas que declaran los permisos y recursos deseados que se solicitan. Para ver algunos ejemplo de ámbitos, consulte [Ámbitos de Microsoft Graph](https://docs.microsoft.com/graph/permissions-reference).

Puede agregar el sufijo del ámbito `/.default` al recurso para ayudar a migrar las aplicaciones del punto de conexión v1.0 (ADAL) al punto de conexión de la plataforma de identidad de Microsoft (MSAL). Por ejemplo, para el valor de recurso de `https://graph.microsoft.com`, el valor de ámbito equivalente es `https://graph.microsoft.com/.default`.  Si el recurso no está en el formato de dirección URL, sino en el del identificador de recurso `XXXXXXXX-XXXX-XXXX-XXXXXXXXXXXX`, todavía puede usar el valor de ámbito como `XXXXXXXX-XXXX-XXXX-XXXXXXXXXXXX/.default`.

Para más información sobre los diferentes tipos de ámbitos, consulte los artículos [Permisos y consentimiento en la plataforma de identidad de Microsoft](https://docs.microsoft.com/azure/active-directory/develop/v2-permissions-and-consent) y [Ámbitos para una API web que acepta tokens de la versión 1.0](https://docs.microsoft.com/azure/active-directory/develop/msal-v1-app-scopes).

### <a name="error-handling"></a>Control de errores

La Biblioteca de autenticación de Azure Active Directory (ADAL) para Python usa la excepción `AdalError` para indicar que ha habido un problema. MSAL para Python normalmente usa códigos de error en su lugar. Para más información, consulte [Control de errores de MSAL para Python](https://docs.microsoft.com/azure/active-directory/develop/msal-handling-exceptions?tabs=python).

### <a name="api-changes"></a>Cambios de API

En la tabla siguiente se muestra una API en ADAL para Python y la que se usa en su lugar en MSAL para Python:

| API de ADAL para Python  | API de MSAL para Python |
| ------------------- | ---------------------------------- |
| [AuthenticationContext](https://adal-python.readthedocs.io/en/latest/#adal.AuthenticationContext)  | [PublicClientApplication o ConfidentialClientApplication](https://msal-python.readthedocs.io/en/latest/#msal.ClientApplication.__init__)  |
| N/D  | [get_authorization_request_url()](https://msal-python.readthedocs.io/en/latest/#msal.ClientApplication.get_authorization_request_url)  |
| [acquire_token_with_authorization_code()](https://adal-python.readthedocs.io/en/latest/#adal.AuthenticationContext.acquire_token_with_authorization_code) | [acquire_token_by_authorization_code()](https://msal-python.readthedocs.io/en/latest/#msal.ClientApplication.acquire_token_by_authorization_code) |
| [acquire_token()](https://adal-python.readthedocs.io/en/latest/#adal.AuthenticationContext.acquire_token) | [acquire_token_silent()](https://msal-python.readthedocs.io/en/latest/#msal.ClientApplication.acquire_token_silent) |
| [acquire_token_with_refresh_token()](https://adal-python.readthedocs.io/en/latest/#adal.AuthenticationContext.acquire_token_with_refresh_token) | N/D |
| [acquire_user_code()](https://adal-python.readthedocs.io/en/latest/#adal.AuthenticationContext.acquire_user_code) | [initiate_device_flow()](https://msal-python.readthedocs.io/en/latest/#msal.PublicClientApplication.initiate_device_flow) |
| [acquire_token_with_device_code()](https://adal-python.readthedocs.io/en/latest/#adal.AuthenticationContext.acquire_token_with_device_code) and [cancel_request_to_get_token_with_device_code()](https://adal-python.readthedocs.io/en/latest/#adal.AuthenticationContext.cancel_request_to_get_token_with_device_code) | [acquire_token_by_device_flow()](https://msal-python.readthedocs.io/en/latest/#msal.PublicClientApplication.acquire_token_by_device_flow) |
| [acquire_token_with_username_password()](https://adal-python.readthedocs.io/en/latest/#adal.AuthenticationContext.acquire_token_with_username_password) | [acquire_token_by_username_password()](https://msal-python.readthedocs.io/en/latest/#msal.PublicClientApplication.acquire_token_by_username_password) |
| [acquire_token_with_client_credentials()](https://adal-python.readthedocs.io/en/latest/#adal.AuthenticationContext.acquire_token_with_client_credentials) y [acquire_token_with_client_certificate()](https://adal-python.readthedocs.io/en/latest/#adal.AuthenticationContext.acquire_token_with_client_certificate) | [acquire_token_for_client()](https://msal-python.readthedocs.io/en/latest/#msal.ConfidentialClientApplication.acquire_token_for_client) |
| N/D | [acquire_token_on_behalf_of()](https://msal-python.readthedocs.io/en/latest/#msal.ConfidentialClientApplication.acquire_token_on_behalf_of) |
| [TokenCache()](https://adal-python.readthedocs.io/en/latest/#adal.TokenCache) | [SerializableTokenCache()](https://msal-python.readthedocs.io/en/latest/#msal.SerializableTokenCache) |
| N/D | Almacenar en caché con persistencia, disponible desde [Extensiones de MSAL](https://github.com/marstr/original-microsoft-authentication-extensions-for-python) |

## <a name="migrate-existing-refresh-tokens-for-msal-python"></a>Migración de tokens de actualización existentes para MSAL para Python

La Biblioteca de autenticación de Microsoft (MSAL) abstrae el concepto de tokens de actualización. MSAL para Python proporciona una memoria caché de tokens en memoria de forma predeterminada para que no sea necesario almacenar, buscar o actualizar tokens de actualización. Los usuarios también verán menos solicitudes de inicio de sesión porque los tokens de actualización normalmente se pueden actualizar sin la intervención del usuario. Para más información acerca de la memoria caché de tokens, consulte [Serialización de la memoria caché de tokens personalizada en MSAL para Python](msal-python-token-cache-serialization.md).

El código siguiente le ayudará a migrar los tokens de actualización administrados por otra biblioteca de OAuth2 (incluida, pero sin limitarse a ADAL para Python), para ser administrados por MSAL para Python. Una razón para migrar esos tokens de actualización es evitar que los usuarios existentes tengan que iniciar sesión de nuevo al migrar la aplicación a MSAL para Python.

El método para migrar un token de actualización es usar MSAL para Python con el fin de adquirir un nuevo token de acceso con el token de actualización anterior. Cuando se devuelva el nuevo token de actualización, MSAL para Python lo almacenará en la memoria caché. Aquí se muestra un ejemplo de cómo se hace:

```python
from msal import PublicClientApplication

def get_preexisting_rt_and_their_scopes_from_elsewhere(...):
    raise NotImplementedError("You will need to implement this by yourself")

app = PublicClientApplication(..., token_cache=...)

for old_rt, old_scope in get_preexisting_rt_and_their_scopes_from_elsewhere(...):
    # Assuming the old scope could be a space-delimited string.
    # MSAL expects a list, like ["scope1", "scope2"].
    scopes = old_scope.split()
        # If your old refresh token came from ADAL for Python, which uses a resource rather than a scope,
        # you need to convert your v1 resource into v2 scopes
        # See https://docs.microsoft.com/azure/active-directory/develop/azure-ad-endpoint-comparison#scopes-not-resources
        # You may be able to append "/.default" to your v1 resource to form a scope
        # See https://docs.microsoft.com/azure/active-directory/develop/v2-permissions-and-consent#the-default-scope

    result = app.client.obtain_token_by_refresh_token(old_rt, scope=scopes)
    # When this call returns the new token(s), a new refresh token is issued by the Microsoft identity platform and MSAL for Python
    # stores it in the token cache.
```

## <a name="next-steps"></a>Pasos siguientes

Para más información, consulte la [comparación entre la versión 1 y la versión 2](active-directory-v2-compare.md).
