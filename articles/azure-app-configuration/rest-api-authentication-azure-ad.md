---
title: 'API REST de Azure Active Directory: autenticación'
description: Use Azure Active Directory para autenticarse en Azure App Configuration mediante API REST
author: lisaguthrie
ms.author: lcozzens
ms.service: azure-app-configuration
ms.topic: reference
ms.date: 08/17/2020
ms.openlocfilehash: fb3d00fb79c55e29d578f5e068e4ae025414a935
ms.sourcegitcommit: 7cc10b9c3c12c97a2903d01293e42e442f8ac751
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/06/2020
ms.locfileid: "93423784"
---
# <a name="azure-active-directory-authentication"></a>Autenticación con Azure Active Directory

Las solicitudes HTTP se pueden autenticar mediante el esquema de autenticación de **portador** con un token adquirido en Azure Active Directory (Azure AD). Estas solicitudes se deben transmitir a través de TLS.

## <a name="prerequisites"></a>Requisitos previos

La entidad de seguridad que se va a usar para solicitar un token de Azure AD debe estar asignada a uno de los [roles de App Configuration](./rest-api-authorization-azure-ad.md) aplicables.

Proporcione a cada solicitud todos los encabezados HTTP necesarios para la autenticación. Los mínimos requeridos son:

|  Encabezado de solicitud | Descripción  |
| --------------- | ------------ |
| **Autorización** | Información de autenticación requerida por el esquema de **portador**. A continuación se explican el formato y los detalles. |

**Ejemplo**:

```http
Host: {myconfig}.azconfig.io
Authorization: Bearer {{AadToken}}
```

## <a name="azure-active-directory-token-acquisition"></a>Adquisición de token de Azure Active Directory

Antes de adquirir un token de Azure AD, uno debe identificar como qué usuario quiere autenticarse, para qué público se está solicitando el token y qué punto de conexión de Azure AD (autoridad) se debe usar.

### <a name="audience"></a>Público

El token de Azure AD debe solicitarse con un público adecuado. En Azure App Configuration, se debe especificar uno de los públicos siguientes al solicitar un token. También se puede hacer referencia al público como el "recurso" para el que se solicita el token.

- {configurationStoreName}.azconfig.io
- *.azconfig.io

> [!IMPORTANT]
> Si el público solicitado es {configurationStoreName}.azconfig.io, debe coincidir exactamente con el encabezado de solicitud "Host" (distingue mayúsculas de minúsculas) usado para enviar la solicitud.

### <a name="azure-ad-authority"></a>Autoridad de Azure AD

La autoridad de Azure AD es el punto de conexión que se usa para adquirir un token de Azure AD. Tiene el formato `https://login.microsoftonline.com/{tenantId}` . El segmento `{tenantId}` hace referencia al identificador de inquilino de Azure Active Directory al que pertenece el usuario o la aplicación que está intentando autenticarse.

### <a name="authentication-libraries"></a>Bibliotecas de autenticación

Azure proporciona un conjunto de bibliotecas denominado Biblioteca de autenticación de Azure Active Directory (ADAL) para simplificar el proceso de adquisición de un token de Azure AD. Estas bibliotecas se compilan en varios lenguajes. Puede encontrar documentación [aquí](https://docs.microsoft.com/azure/active-directory/develop/active-directory-authentication-libraries).

## <a name="errors"></a>**Errores**

```http
HTTP/1.1 401 Unauthorized
WWW-Authenticate: HMAC-SHA256, Bearer
```

**Motivo:** no se ha proporcionado un encabezado de solicitud de autorización con el esquema de portador.
**Solución:** proporcione un encabezado de solicitud HTTP ```Authorization``` válido.

```http
HTTP/1.1 401 Unauthorized
WWW-Authenticate: HMAC-SHA256, Bearer error="invalid_token", error_description="Authorization token failed validation"
```

**Motivo:** el token de Azure AD no es válido.
**Solución:** adquiera un token de Azure AD de la autoridad de Azure AD y asegúrese de que se use el público adecuado.

```http
HTTP/1.1 401 Unauthorized
WWW-Authenticate: HMAC-SHA256, Bearer error="invalid_token", error_description="The access token is from the wrong issuer. It must match the AD tenant associated with the subscription to which the configuration store belongs. If you just transferred your subscription and see this error message, please try back later."
```

**Motivo:** el token de Azure AD no es válido.
**Solución:** adquiera un token de Azure AD de la autoridad de Azure AD y asegúrese de que el inquilino de Azure AD sea el asociado a la suscripción a la que pertenece el almacén de configuración. Este error puede aparecer si la entidad de seguridad pertenece a más de un inquilino de Azure AD.
