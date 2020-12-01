---
title: 'API REST de Azure Active Directory: autenticación'
description: Use Azure Active Directory para autenticarse en Azure App Configuration mediante la API REST.
author: lisaguthrie
ms.author: lcozzens
ms.service: azure-app-configuration
ms.topic: reference
ms.date: 08/17/2020
ms.openlocfilehash: 78344bd3896ca7d00c9f761c586b6f5142dc1e58
ms.sourcegitcommit: 30906a33111621bc7b9b245a9a2ab2e33310f33f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/22/2020
ms.locfileid: "95253412"
---
# <a name="azure-active-directory-authentication"></a>Autenticación con Azure Active Directory

Para autenticar las solicitudes HTTP, puede usar el esquema de autenticación `Bearer` con un token adquirido de Azure Active Directory (Azure AD). Debe transmitir estas solicitudes a través de la seguridad de la capa de transporte (TLS).

## <a name="prerequisites"></a>Requisitos previos

Debe asignar la entidad de seguridad que se usa para solicitar un token de Azure AD a uno de los [roles de Azure App Configuration](./rest-api-authorization-azure-ad.md) aplicables.

Proporcione a cada solicitud todos los encabezados HTTP necesarios para la autenticación. Estos son los requisitos mínimos:

|  Encabezado de solicitud | Descripción  |
| --------------- | ------------ |
| `Authorization` | Información de autenticación requerida por el esquema `Bearer`. |

**Ejemplo**:

```http
Host: {myconfig}.azconfig.io
Authorization: Bearer {{AadToken}}
```

## <a name="azure-ad-token-acquisition"></a>Obtención de tokens de Azure AD

Antes de adquirir un token de Azure AD, debe identificar como qué usuario quiere autenticarse, para qué público solicita el token y qué punto de conexión de Azure AD (autoridad) se debe usar.

### <a name="audience"></a>Público

Solicite el token de Azure AD con un público adecuado. Para Azure App Configuration, use uno de los públicos siguientes. También se puede hacer referencia al público como el *recurso* para el que se solicita el token.

- {configurationStoreName}.azconfig.io
- *.azconfig.io

> [!IMPORTANT]
> Si el público solicitado es `{configurationStoreName}.azconfig.io`, debe coincidir exactamente con el encabezado de solicitud `Host` (distingue mayúsculas de minúsculas) usado para enviar la solicitud.

### <a name="azure-ad-authority"></a>Autoridad de Azure AD

La autoridad de Azure AD es el punto de conexión que se usa para obtener un token de Azure AD. Tiene el formato `https://login.microsoftonline.com/{tenantId}`. El segmento `{tenantId}` hace referencia al identificador de inquilino de Azure AD al que pertenece el usuario o la aplicación que está intentando autenticarse.

### <a name="authentication-libraries"></a>Bibliotecas de autenticación

Azure proporciona un conjunto de bibliotecas denominado Biblioteca de autenticación de Azure Active Directory para simplificar el proceso de adquisición de un token de Azure AD. Azure compila estas bibliotecas para varios lenguajes. Para más información, consulte la [documentación](https://docs.microsoft.com/azure/active-directory/develop/active-directory-authentication-libraries).

## <a name="errors"></a>Errors

Puede que encuentre los siguientes errores.

```http
HTTP/1.1 401 Unauthorized
WWW-Authenticate: HMAC-SHA256, Bearer
```

**Motivo:** No proporcionó el encabezado de solicitud de autorización con el esquema `Bearer`.

**Solución:** Proporcione un encabezado de solicitud HTTP `Authorization` válido.

```http
HTTP/1.1 401 Unauthorized
WWW-Authenticate: HMAC-SHA256, Bearer error="invalid_token", error_description="Authorization token failed validation"
```

**Motivo:** El token de Azure AD no es válido.

**Solución:** Obtenga un token de Azure AD de la autoridad de Azure AD y asegúrese de haber usado el público adecuado.

```http
HTTP/1.1 401 Unauthorized
WWW-Authenticate: HMAC-SHA256, Bearer error="invalid_token", error_description="The access token is from the wrong issuer. It must match the AD tenant associated with the subscription to which the configuration store belongs. If you just transferred your subscription and see this error message, please try back later."
```

**Motivo:** El token de Azure AD no es válido.

**Solución:** Obtenga un token de Azure AD de la autoridad de Azure AD. Asegúrese de que el inquilino de Azure AD sea el asociado a la suscripción a la que pertenece el almacén de configuración. Este error puede aparecer si la entidad de seguridad pertenece a más de un inquilino de Azure AD.
