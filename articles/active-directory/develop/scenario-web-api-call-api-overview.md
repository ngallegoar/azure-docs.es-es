---
title: 'Compilación de una API web que llama a otras API web: Plataforma de identidad de Microsoft | Azure'
description: Obtenga información sobre cómo compilar una API web que llame a API web de bajada (información general).
services: active-directory
author: jmprieur
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: conceptual
ms.workload: identity
ms.date: 05/07/2019
ms.author: jmprieur
ms.custom: aaddev, identityplatformtop40
ms.openlocfilehash: 88a0177755fbd913bdaaf0ecf3e12c62dee294c1
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/28/2020
ms.locfileid: "80885079"
---
# <a name="scenario-a-web-api-that-calls-web-apis"></a>Escenario: API web que llama a API web

Aprenda a compilar una API web que llame a las API web.

## <a name="prerequisites"></a>Prerrequisitos

Este escenario, en el que una API web protegida llama a otras API web, se basa en el escenario de "Proteger una API web". Para más información sobre este escenario básico, consulte [Escenario: API web protegida](scenario-protected-web-api-overview.md).

## <a name="overview"></a>Información general

- Un cliente de aplicación web, de escritorio, móvil o de página única (no se representa en el diagrama siguiente) llama a una API web protegida y proporciona un token de portador JSON Web Token (JWT) en el encabezado HTTP "Autorización".
- La API web protegida valida el token y usa el método `AcquireTokenOnBehalfOf` de la biblioteca de autenticación de Microsoft (MSAL) para solicitar otro token de Azure Active Directory (Azure AD) para que la API web protegida pueda llamar a una segunda API web o a una API web de bajada en nombre del usuario.
- La API web protegida también puede llamar `AcquireTokenSilent`más tarde para solicitar tokens para otras API de bajada en nombre del mismo usuario. `AcquireTokenSilent` actualiza el token cuando sea necesario.

![Diagrama de una API web que llama a una API web](media/scenarios/web-api.svg)

## <a name="specifics"></a>Características específicas

La parte del registro de aplicación que está relacionada con los permisos de la API es clásica. La configuración de la aplicación implica el uso del flujo con derechos delegados OAuth 2.0 para intercambiar el token de portador JWT con un token para una API de bajada. Este token se agrega a la caché de tokens, que está disponible en los controladores de la API web, y puede adquirir un token de forma silenciosa para llamar a las API de bajada.

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Registro de aplicaciones](scenario-web-api-call-api-app-registration.md)
