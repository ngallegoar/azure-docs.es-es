---
title: 'Escenario de aplicaciones de página única de JavaScript: plataforma de identidad de Microsoft | Azure'
description: Aprenda a compilar una aplicación de página única (información general del escenario) mediante la plataforma de identidad de Microsoft.
services: active-directory
author: navyasric
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: conceptual
ms.workload: identity
ms.date: 05/07/2019
ms.author: nacanuma
ms.custom: aaddev, identityplatformtop40
ms.openlocfilehash: 3ead0ea58c6860519f027eb6a7450df37396bd89
ms.sourcegitcommit: d187fe0143d7dbaf8d775150453bd3c188087411
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80885182"
---
# <a name="scenario-single-page-application"></a>Escenario: Aplicación de una sola página

Aprenda lo necesario para crear una aplicación de página única (SPA)

## <a name="prerequisites"></a>Prerrequisitos

[!INCLUDE [Prerequisites](../../../includes/active-directory-develop-scenarios-prerequisites.md)]

## <a name="getting-started"></a>Introducción

Puede crear su primera aplicación siguiendo la guía de inicio rápido de SPA de JavaScript:

> [!div class="nextstepaction"]
> [Inicio rápido: Aplicación de página única](./quickstart-v2-javascript.md)

## <a name="overview"></a>Información general

Muchas aplicaciones web modernas se compilan como aplicaciones de página única del lado cliente. Los desarrolladores las escriben mediante JavaScript o un marco SPA, como Angular, Vue.js y React.js. Estas aplicaciones se ejecutan en un explorador web y tienen características de autenticación diferentes a las de las aplicaciones web del lado servidor tradicionales. 

La plataforma de identidad de Microsoft permite que las aplicaciones de página única inicien la sesión de los usuarios y obtengan tokens para acceder a los servicios de back-end o a las API web mediante el [flujo implícito de OAuth 2.0](./v2-oauth2-implicit-grant-flow.md). El flujo implícito permite a la aplicación obtener los tokens de identificador para representar al usuario autenticado y también acceder a los tokens que se necesitan para llamar a las API protegidas.

![Aplicación de página única](./media/scenarios/spa-app.svg)

Este flujo de autenticación no incluye escenarios de aplicaciones que usan marcos de JavaScript multiplataforma, como Electron y React Native, ya que requieren más funcionalidades para la interacción con las plataformas nativas.

## <a name="specifics"></a>Características específicas

Para habilitar este escenario en la aplicación, necesita lo siguiente:

* Registro de aplicaciones con Azure Active Directory (Azure AD). Este registro implica habilitar el flujo implícito y establecer un URI de redirección al que se devuelven los tokens.
* La configuración de la aplicación con las propiedades de aplicación registradas, como el identificador de aplicación.
* Uso de la biblioteca de autenticación de Microsoft (MSAL) para realizar el flujo de autenticación a fin de iniciar sesión y adquirir tokens.

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Registro de aplicaciones](scenario-spa-app-registration.md)
