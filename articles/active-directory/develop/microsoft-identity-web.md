---
title: Introducción a la biblioteca de autenticación de Microsoft Identity Web
titleSuffix: Microsoft identity platform
description: Obtenga información acerca de Microsoft Identity Web, una biblioteca de autenticación y autorización para las aplicaciones de ASP.NET Core que se integran con Azure Active Directory, Azure AD B2C, Microsoft Graph y otras API web.
services: active-directory
author: jmprieur
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: conceptual
ms.workload: identity
ms.date: 10/09/2020
ms.author: jmprieur
ms.reviewer: marsma
ms.custom: devx-track-csharp, aaddev
ms.openlocfilehash: b82e300128a41f8315132e1ff93af33c853edb15
ms.sourcegitcommit: d22a86a1329be8fd1913ce4d1bfbd2a125b2bcae
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/26/2020
ms.locfileid: "96173522"
---
# <a name="microsoft-identity-web-authentication-library"></a>Biblioteca de autenticación de Microsoft Identity Web

Microsoft Identity Web es un conjunto de bibliotecas de ASP.NET Core que simplifican la compatibilidad con la autenticación y la autorización de aplicaciones web y API web que se integran con la plataforma de identidad de Microsoft. Asimismo, proporciona una cómoda capa de API de una sola superficie que une ASP.NET Core, su middleware de autenticación y la [biblioteca de autenticación de Microsoft (MSAL) para .NET](https://github.com/azuread/microsoft-authentication-library-for-dotnet).

## <a name="supported-application-scenarios"></a>Escenarios de aplicación admitidos

Si va a compilar las aplicaciones web de ASP.NET Core o las API web y quiere usar Azure Active Directory (Azure AD) o Azure AD B2C para administrar las identidades y el acceso (IAM), se recomienda usar Microsoft Identity Web para todos estos escenarios:

- [Aplicación web que permite iniciar sesión a los usuarios](scenario-web-app-sign-user-overview.md)
- [Aplicación web que inicia la sesión de los usuarios y llama a una API web en su nombre](scenario-web-app-call-api-overview.md)
- [API web protegida a la que solo pueden acceder los usuarios autenticados](scenario-protected-web-api-overview.md)
- [API web protegida que llama a otra API web (de bajada) en nombre del usuario con sesión iniciada](scenario-web-api-call-api-overview.md)

## <a name="get-the-library"></a>Obtención de la biblioteca

Puede obtener Microsoft Identity Web en [NuGet](#nuget), las [plantillas de proyecto de .NET Core](#project-templates)y [GitHub](#github).

#### <a name="nuget"></a>NuGet

Microsoft Identity Web está disponible en NuGet como un conjunto de paquetes que proporcionan funcionalidad modular en función de las necesidades de la aplicación. Use el comando `dotnet add` de la CLI de .NET o el **administrador de paquetes de NuGet** de Visual Studio para instalar los paquetes adecuados para su proyecto:

- [Microsoft.Identity.Web](https://www.nuget.org/packages/Microsoft.Identity.Web): el paquete principal. Es necesario en todas las aplicaciones que usan Microsoft Identity Web.
- [Microsoft.Identity.Web.UI](https://www.nuget.org/packages/Microsoft.Identity.Web.UI): opcional. Agrega la interfaz de usuario en el inicio y cierre de sesión de los usuarios y un controlador asociado para las aplicaciones web.
- [Microsoft.Identity.Web.MicrosoftGraph](https://www.nuget.org/packages/Microsoft.Identity.Web.MicrosoftGraph): opcional. Proporciona una interacción simplificada con Microsoft Graph API.
- [Microsoft.Identity.Web.MicrosoftGraphBeta](https://www.nuget.org/packages/Microsoft.Identity.Web.MicrosoftGraphBeta): opcional. Proporciona una interacción simplificada con el [punto de conexión beta](/graph/api/overview?view=graph-rest-beta&preserve-view=true) de Microsoft Graph API.

#### <a name="project-templates"></a>Plantillas de proyecto

Las plantillas de proyecto de Microsoft Identity Web se incluyen en .NET 5.0 y están disponibles para su descarga en los proyectos de ASP.NET Core 3.1.

Si utiliza ASP.NET Core 3.1, instale las plantillas con la CLI de .NET:

```dotnetcli
dotnet new --install Microsoft.Identity.Web.ProjectTemplates::1.0.0
```

En el diagrama siguiente se muestra una vista de alto nivel de los tipos de aplicación admitidos y sus argumentos pertinentes:

:::image type="content" source="media/microsoft-identity-web-overview/diagram-microsoft-identity-web-templates.png" lightbox="media/microsoft-identity-web-overview/diagram-microsoft-identity-web-templates.png" alt-text="Diagrama de las plantillas de proyecto disponibles en la CLI de .NET para Microsoft Identity Web.":::
<br /><sup><b>*</b></sup> `MultiOrg` no es compatible con `webapi2`, pero se puede habilitar en *appsettings.json* estableciendo el inquilino en `common` o `organizations`.
<br /><sup><b>**</b></sup> `--calls-graph` no se admite en Azure AD B2C

Este comando de ejemplo de la CLI de .NET que se ha tomado del [tutorial de Blazor Server](tutorial-blazor-server.md), genera un nuevo proyecto de Blazor Server que incluye los paquetes correctos y el código de inicio (valores de marcador de posición que se muestran):

```dotnetcli
dotnet new blazorserver2 --auth SingleOrg --calls-graph --client-id "00000000-0000-0000-0000-000000000000" --tenant-id "11111111-1111-1111-1111-111111111111" --output my-blazor-app
```

#### <a name="github"></a>GitHub

Microsoft Identity Web es un proyecto de código abierto hospedado en GitHub: <a href="https://github.com/AzureAD/microsoft-identity-web" target="_blank">AzureAD/microsoft-identity-web<span class="docon docon-navigate-external x-hidden-focus"></span></a>

La [wiki del repositorio](https://github.com/AzureAD/microsoft-identity-web/wiki) contiene documentación adicional y, si necesita ayuda o detecta un error, puede [notificar un problema](https://github.com/AzureAD/microsoft-identity-web/issues).

## <a name="features"></a>Características

Microsoft Identity Web incluye varias características que no se proporcionan si usa las plantillas de proyecto predeterminadas de ASP.NET 3.1.

| Característica                                                                                  | ASP.NET Core 3.1                                                     | Web de Microsoft Identity                                                                                  |
|------------------------------------------------------------------------------------------|----------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------|
| [Inicio de sesión de usuarios](scenario-web-app-sign-user-app-configuration.md) en aplicaciones web             | <li>Cuentas profesionales o educativas<li>Identidades sociales (con Azure AD B2C) | <li>Cuentas profesionales o educativas<li>Cuentas personales de Microsoft<li>Identidades sociales (con Azure AD B2C)     |
| [Proteger las API web](scenario-protected-web-api-app-configuration.md#microsoftidentityweb) | <li>Cuentas profesionales o educativas<li>Identidades sociales (con Azure AD B2C) | <li>Cuentas profesionales o educativas<li>Cuentas personales de Microsoft<li>Identidades sociales (con Azure AD B2C)     |
| Validación de emisores en aplicaciones de varios inquilinos                                                   | No                                                                   | Sí, para [todas las nubes](authentication-national-cloud.md) y [Azure AD B2C](../../active-directory-b2c/index.yml) |
| Aplicación web o API [llama a Microsoft Graph][scenario-api-call-graph]                             | No                                                                   | Sí                                                                                                     |
| Aplicación web o API [llama a API web][scenario-api-call-api]                                       | No                                                                   | Sí                                                                                                     |
| Admite credenciales de certificado                                                         | No                                                                   | Sí, incluyendo Azure Key Vault                                                                          |
| Compatibilidad con el consentimiento incremental y el acceso condicional en aplicaciones web                           | No                                                                   | Sí, en MVC, páginas de Razor y Blazor                                                                    |
| Certificados de cifrado de tokens en API web                                                | No                                                                   | Sí                                                                                                     |
| [Validación de roles de ámbitos o aplicaciones][scenario-api-validation] en las API web                        | No                                                                   | Sí                                                                                                     |
| Generación de encabezados `WWW-Authenticate` en las API web                                         | No                                                                   | Sí                                                                                                     |

## <a name="next-steps"></a>Pasos siguientes

Para ver cómo funciona Microsoft Identity Web, pruebe nuestro tutorial de Blazor Server:

[Tutorial: creación de una aplicación Blazor Server que usa la plataforma de identidad de Microsoft para la autenticación.](tutorial-blazor-server.md)

La wiki de Microsoft Identity Web en GitHub contiene una amplia documentación de referencia acerca de diversos aspectos de la biblioteca. Por ejemplo, el uso de certificados, el consentimiento incremental y la referencia de acceso condicional se pueden encontrar aquí:

- <a href="https://github.com/AzureAD/microsoft-identity-web/wiki/Using-certificates" target="_blank">Uso de certificados con Microsoft.Identity.Web<span class="docon docon-navigate-external x-hidden-focus"></span></a> (GitHub)
- <a href="https://github.com/AzureAD/microsoft-identity-web/wiki/Managing-incremental-consent-and-conditional-access" target="_blank">Consentimiento incremental y acceso condicional<span class="docon docon-navigate-external x-hidden-focus"></span></a> (GitHub)

<!-- LINKS -->
<!--  [miw-certs]: microsoft-identity-web-certificates.md  -->
<!--  [miw-certs-decrypt]: microsoft-identity-web-certificates.md#decryption-certificates  -->
<!--  [miw-inc-consent-ca-header]: microsoft-identity-web-consent-conditional-access.md#handling-incremental-consent-or-conditional-access-in-web-apis  -->
<!--  [miw-inc-consent-ca]: microsoft-identity-web-consent-conditional-access.md  -->
[scenario-api-call-api]: scenario-web-api-call-api-call-api.md#option-1-call-microsoft-graph-with-the-sdk [scenario-api-call-graph]: scenario-web-api-call-api-call-api.md#option-1-call-microsoft-graph-with-the-sdk [scenario-api-validation]: scenario-protected-web-api-verification-scope-app-roles.md