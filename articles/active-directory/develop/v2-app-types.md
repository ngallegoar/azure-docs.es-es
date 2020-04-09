---
title: Tipos de aplicaciones para la Plataforma de identidad de Microsoft | Azure
description: Los tipos de aplicaciones y escenarios que admite el punto de conexión de la Plataforma de identidad de Microsoft (versión 2.0).
services: active-directory
author: rwike77
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.workload: identity
ms.topic: conceptual
ms.date: 04/06/2019
ms.author: ryanwi
ms.reviewer: saeeda, jmprieur
ms.custom: aaddev
ms.openlocfilehash: 34ccddff3e42f69a50b44f4bfff391070063a5a1
ms.sourcegitcommit: d187fe0143d7dbaf8d775150453bd3c188087411
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80886371"
---
# <a name="application-types-for-microsoft-identity-platform"></a>Tipos de aplicaciones para la Plataforma de identidad de Microsoft

El punto de conexión de la Plataforma de identidad de Microsoft (versión 2.0) admite la autenticación de una variedad de arquitecturas de aplicaciones modernas, todas ellas basadas en los protocolos estándar del sector [OAuth 2.0 u OpenID Connect](active-directory-v2-protocols.md). En este artículo se describen los tipos de aplicaciones que puede compilar mediante la Plataforma de identidad de Microsoft, independientemente de su plataforma o idioma preferidos. La información está diseñada para ayudarlo a entender los escenarios de alto nivel antes de [empezar a trabajar con el código](v2-overview.md#getting-started).

> [!NOTE]
> No todas las características y escenarios de Azure Active Directory (Azure AD) son compatibles con el punto de conexión de la Plataforma de identidad de Microsoft. Para determinar si debe usar el punto de conexión de la Plataforma de identidad de Microsoft, conozca las [limitaciones de esta plataforma](active-directory-v2-limitations.md).

## <a name="the-basics"></a>Conceptos básicos

Debe registrar cada aplicación que usa el punto de conexión de la Plataforma de identidad de Microsoft en el nuevo [portal de Registros de aplicaciones](https://go.microsoft.com/fwlink/?linkid=2083908). El proceso de registro de la aplicación recopila y asigna algunos valores a la aplicación:

* Un **identificador de la aplicación (cliente)** que identifica la aplicación de manera única.
* Un **URI de redirección** que puede usarse para dirigir las respuestas de nuevo a la aplicación
* Algunos otros valores específicos para el escenario, como los tipos de cuentas compatibles.

Para más información, aprenda a [registrar una aplicación](quickstart-register-app.md).

Una vez que la aplicación se registra, se comunica con la Plataforma de identidad de Microsoft mediante el envío de solicitudes al punto de conexión. Proporcionamos bibliotecas y marcos de código abierto que controlan los detalles de estas solicitudes. También tiene la opción de implementar la lógica de autenticación por su cuenta mediante la creación de solicitudes a estos puntos de conexión:

```HTTP
https://login.microsoftonline.com/common/oauth2/v2.0/authorize
https://login.microsoftonline.com/common/oauth2/v2.0/token
```

## <a name="single-page-apps-javascript"></a>Aplicaciones de una página (JavaScript)

Muchas aplicaciones modernas tienen un front-end de aplicación de una página escrito principalmente en JavaScript. A menudo, se escribe mediante un marco como Angular, React o Vue. El punto de conexión de la Plataforma de identidad de Microsoft admite estas aplicaciones a través del uso del [flujo implícito de OAuth 2.0](v2-oauth2-implicit-grant-flow.md).

En este flujo, la aplicación recibe tokens directamente del punto de conexión de autorización de la Plataforma de identidad de Microsoft, sin necesidad de realizar ningún intercambio de servidor a servidor. Toda la lógica de autenticación y el control de sesiones tiene lugar por completo en el cliente de JavaScript, sin redireccionamientos de página adicionales.

![Muestra el flujo de autenticación implícita](./media/v2-app-types/convergence-scenarios-implicit.svg)

Para ver este escenario en acción, pruebe uno de los ejemplos de código de aplicación de una única página en nuestra sección de [introducción a la Plataforma de identidad de Microsoft](v2-overview.md#getting-started).

## <a name="web-apps"></a>Aplicaciones web

En las aplicaciones web (. NET, PHP, Java, Ruby, Python, Node) a las que el usuario accede a través de un explorador, puede usar [OpenID Connect](active-directory-v2-protocols.md) para el inicio de sesión de usuario. En OpenID Connect, la aplicación web recibe un identificador de token. Un token de id. es un token de seguridad que comprueba la identidad del usuario y proporciona información sobre el usuario en forma de notificaciones:

```JSON
// Partial raw ID token
eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6ImtyaU1QZG1Cd...

// Partial content of a decoded ID token
{
    "name": "John Smith",
    "email": "john.smith@gmail.com",
    "oid": "d9674823-dffc-4e3f-a6eb-62fe4bd48a58"
    ...
}
```

Encontrará más información sobre los diferentes tipos de token que se usan en el punto de conexión de la Plataforma de identidad de Microsoft en la [referencia de token de acceso](access-tokens.md) y en la [referencia de id_token](id-tokens.md).

En las aplicaciones de servidor web, el flujo de autenticación de inicio de sesión realiza estos pasos de alto nivel:

![Muestra el flujo de autenticación de aplicaciones web](./media/v2-app-types/convergence-scenarios-webapp.svg)

Puede confirmar la identidad del usuario mediante la validación del token de id. con una clave de firma pública recibida por el punto de conexión de la Plataforma de identidad de Microsoft. Se establece una cookie de sesión, que puede usarse para identificar al usuario en las sucesivas solicitudes de página.

Para ver este escenario en acción, pruebe uno de los ejemplos de código de inicio de sesión de aplicaciones web en la sección de [introducción a la Plataforma de identidad de Microsoft](v2-overview.md#getting-started).

Además del inicio de sesión sencillo, una aplicación web de servidor podría tener la necesidad de acceder a otros servivio web, como una API de REST. En este caso, la aplicación de servidor web participa en un flujo combinado de OpenID Connect y OAuth 2.0, mediante el [flujo de código de autorización de OAuth 2.0](active-directory-v2-protocols.md). Para más información sobre este escenario, lea acerca de cómo [comenzar con aplicaciones web y API web](active-directory-v2-devquickstarts-webapp-webapi-dotnet.md).

## <a name="web-apis"></a>API web

Puede usar el punto de conexión de la Plataforma de identidad de Microsoft para proteger los servicios web, como la API web RESTful de la aplicación. Las API web se pueden implementar en numerosas plataformas y lenguajes. También se pueden implementar mediante desencadenadores HTTP en Azure Functions. En lugar de tokens de id. y cookies de sesión, una API web usa un token de acceso de OAuth 2.0 para proteger sus datos y para autenticar las solicitudes entrantes. El llamador de una API web anexa un token de acceso en el encabezado de autorización de una solicitud HTTP, como esta:

```HTTP
GET /api/items HTTP/1.1
Host: www.mywebapi.com
Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6...
Accept: application/json
...
```

La API web usa el token de acceso para comprobar la identidad del llamador de API y extraer información sobre el llamador de notificaciones que se codifican en dicho token. Hay más detalles sobre los diferentes tipos de token que se usan en el punto de conexión de la Plataforma de identidad de Microsoft en la referencia [Token de acceso](access-tokens.md) y en la referencia [id_token](id-tokens.md).

Una API web puede ofrecer a los usuarios la capacidad para administrar la participación/no participación de ciertas funcionalidades o datos mediante la exposición de permisos, conocidos también como [ámbitos](v2-permissions-and-consent.md). Para que una aplicación de llamada adquiera permiso para un ámbito, el usuario debe consentir el ámbito durante un flujo. El punto de conexión de la Plataforma de identidad de Microsoft solicita al usuario permiso y luego registra los permisos en todos los tokens de acceso que recibe la API web. La API web valida los tokens de acceso que recibe en cada llamada y realiza comprobaciones de autorización.

Una API web puede recibir tokens de acceso de todos los tipos de aplicaciones, incluidas las aplicaciones de servidor web, aplicaciones móviles y de escritorio, aplicaciones de una página, demonios del lado del servidor e incluso otras API web. El flujo de alto nivel de una API web se parece a este:

![Muestra el flujo de autenticación de API web](./media/v2-app-types/convergence-scenarios-webapi.svg)

Para aprender a proteger una API web con tokens de acceso de OAuth2, consulte los ejemplos de código de API web en nuestra sección de [introducción a la Plataforma de identidad de Microsoft](v2-overview.md#getting-started).

En muchos casos, las API web también tienen que realizar solicitudes salientes a otras API web de bajada protegidas por la Plataforma de identidad de Microsoft. Para ello, las API web pueden aprovechar las ventajas del flujo **con derechos delegados**, que permite a la API web intercambiar un token de acceso entrante por otro token de acceso que se usará en las solicitudes salientes. Para más información, consulte [Plataforma de identidad de Microsoft y flujo con derechos delegados de OAuth 2.0](v2-oauth2-on-behalf-of-flow.md).

## <a name="mobile-and-native-apps"></a>Aplicaciones móviles y nativas

Las aplicaciones instaladas en un dispositivo, como las aplicaciones móviles y de escritorio, suelen necesitar el acceso a servicios back-end o a las API web que almacenan datos y realizan varias funciones en nombre del usuario. Estas aplicaciones pueden agregar el inicio de sesión y la autorización a los servicios back-end mediante el [flujo de código de autorización de OAuth 2.0](v2-oauth2-auth-code-flow.md).

En este flujo, la aplicación recibe un código de autorización del punto de conexión de la Plataforma de identidad de Microsoft cuando el usuario inicia sesión. El código de autorización representa el permiso de la aplicación para llamar a servicios de back-end en nombre del usuario que inició la sesión. La aplicación podrá intercambiar el código de autorización en segundo plano para un token de acceso de OAuth 2.0 y un token de actualización. La aplicación puede usar el token de acceso para autenticar las API web en las solicitudes HTTP y utilizar el token de actualización para obtener nuevos tokens de acceso cuando expiren los antiguos.

![Muestra el flujo de autenticación de la aplicación nativa](./media/v2-app-types/convergence-scenarios-native.svg)

## <a name="daemons-and-server-side-apps"></a>Demonios y aplicaciones de servidor

Las aplicaciones que contienen procesos de larga duración o que funcionan sin la interacción con un usuario también necesitan un modo de acceder a recursos protegidos, como las API web. Estas aplicaciones pueden autenticarse y obtener tokens mediante la identidad de la aplicación, en lugar de una identidad delegada del usuario, con el flujo de credenciales de cliente de OAuth 2.0. Puede demostrar la identidad de la aplicación mediante un certificado o secreto de cliente. Para más información, consulte este artículo sobre la [aplicación de consola de demonio de .NET Core con la plataforma de identidad de Microsoft](https://github.com/Azure-Samples/active-directory-dotnetcore-daemon-v2).

En este flujo, la aplicación interactúa directamente con el punto de conexión `/token` para obtener acceso:

![Muestra el flujo de autenticación de la aplicación de demonio](./media/v2-app-types/convergence-scenarios-daemon.svg)

Para compilar una aplicación demonio, consulte la [documentación de credenciales de cliente](v2-oauth2-client-creds-grant-flow.md) en nuestra sección de introducción o pruebe una [aplicación de ejemplo de .NET](https://github.com/Azure-Samples/active-directory-dotnet-daemon-v2).
