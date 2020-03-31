---
title: 'Descripción de la autenticación de las API: Azure Digital Twins | Microsoft Docs'
description: Aprenda a conectarse y autenticarse con las API mediante Azure Digital Twins.
ms.author: alinast
author: alinamstanciu
manager: bertvanhoof
ms.service: digital-twins
services: digital-twins
ms.topic: conceptual
ms.date: 01/21/2020
ms.openlocfilehash: d950d41186d578702343645875dd7c565002d5a5
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "76513012"
---
# <a name="connect-to-and-authenticate-with-apis"></a>Conexión y autenticación con las API

Azure Digital Twins utiliza Azure Active Directory (Azure AD) para autenticar a los usuarios y proteger a las aplicaciones. Azure AD admite la autenticación de diversas arquitecturas modernas. Todas ellas se basan en los protocolos estándar del sector OAuth 2.0 u OpenID Connect. Además, los desarrolladores pueden usar Azure AD para compilar las aplicaciones de inquilino único y línea de negocio (LOB). Los desarrolladores también pueden usar Azure AD para desarrollar [aplicaciones para varios inquilinos](how-to-multitenant-applications.md).

Para obtener información general de Azure AD, visite la [página de aspectos básicos](https://docs.microsoft.com/azure/active-directory/fundamentals/) para acceder a guías paso a paso, conceptos y guías de inicio rápido.

> [!TIP]
> Siga el [tutorial](tutorial-facilities-setup.md) para configurar y ejecutar una aplicación de ejemplo de Azure Digital Twins.

Para integrar una aplicación o un servicio con Azure AD, el desarrollador debe registrar primero la aplicación en Azure AD. Para obtener instrucciones detalladas y capturas de pantalla, lea [este inicio rápido](../active-directory/develop/quickstart-register-app.md).

Azure AD admite [cinco escenarios principales de aplicación](../active-directory/develop/v2-app-types.md):

* Aplicación de una sola página (SPA): un usuario tiene que iniciar sesión en una aplicación de una sola página protegida por Azure AD.
* Explorador web para aplicación web: un usuario tiene que iniciar sesión en una aplicación web protegida por Azure AD.
* Aplicación nativa para API web: una aplicación nativa que se ejecuta en teléfonos, tabletas o equipos tiene que autenticar a un usuario para obtener recursos de una API web protegida por Azure AD.
* Aplicación web para API web: una aplicación web tiene que obtener recursos de una API web protegida por Azure AD.
* Aplicación de servidor o demonio para API web: una aplicación de demonio o de servidor sin interfaz de usuario web tiene que obtener recursos de una API web protegida por Azure AD.

> [!IMPORTANT]
> Azure Digital Twins admite las dos bibliotecas de autenticación siguientes:
> * La [Biblioteca de autenticación de Microsoft (MSAL)](https://docs.microsoft.com/azure/active-directory/develop/msal-overview) más reciente
> * La [Biblioteca de autenticación de Azure Active Directory (ADAL)](https://docs.microsoft.com/azure/active-directory/develop/active-directory-authentication-libraries)

## <a name="call-digital-twins-from-a-middle-tier-web-api"></a>Llamada a Digital Twins desde una API web de nivel intermedio

Cuando los desarrolladores diseñan soluciones de Digital Twins, normalmente crean una aplicación o API web de nivel intermedio. La aplicación o la API, a continuación, llama a la API de Digital Twins de un nivel inferior. Para admitir la arquitectura de la solución web estándar, asegúrese de que los usuarios primero:

1. Se autentiquen con la aplicación de nivel intermedio.

1. Adquieran un token en nombre de OAuth 2.0 durante la autenticación.

1. A continuación, el token adquirido se usa para autenticarse con API o para llamar a API que están en niveles inferiores con el flujo "en nombre de".

Para obtener instrucciones sobre cómo coordinar el flujo con derechos delegados , lea [Flujo con derechos delegados de OAuth 2.0](https://docs.microsoft.com/azure/active-directory/develop/v2-oauth2-on-behalf-of-flow). También puede ver ejemplos de código en [Calling a downstream web API](https://github.com/Azure-Samples/active-directory-dotnet-webapi-onbehalfof) (Llamada a una API web de bajada).

## <a name="next-steps"></a>Pasos siguientes

Para configurar y probar Azure Digital Twins mediante el flujo de concesión implícita de OAuth 2.0, lea [Configure Postman](./how-to-configure-postman.md) (Configurar Postman).

Para obtener información acerca de la seguridad en Azure Digital Twins, consulte [Create and manage role assignments](./security-create-manage-role-assignments.md) (Creación y administración de las asignaciones de roles).
