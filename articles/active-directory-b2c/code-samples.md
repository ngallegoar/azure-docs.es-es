---
title: Ejemplos de código de Azure Active Directory B2C | Microsoft Docs
description: Ejemplos de código para aplicaciones móviles, de escritorio, web y de una sola página de Azure Active Directory B2C.
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.author: mimart
ms.date: 10/02/2020
ms.custom: mvc
ms.topic: sample
ms.service: active-directory
ms.subservice: B2C
ms.openlocfilehash: 63a79df8e540e88d0f1e3f4c5b8d339ff03c877f
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "91854297"
---
# <a name="azure-active-directory-b2c-code-samples"></a>Ejemplos de código de Azure Active Directory B2C

En las tablas siguientes se proporcionan vínculos a ejemplos para aplicaciones iOS, Android, .NET y Node.js.

## <a name="mobile-and-desktop-apps"></a>Aplicaciones móviles y de escritorio

| Muestra | Descripción |
|--------| ----------- |
| [ios-swift-native-msal](https://github.com/Azure-Samples/active-directory-b2c-ios-swift-native-msal) | Un ejemplo de iOS en Swift que autentica a los usuarios de Azure AD B2C y llama a una API mediante OAuth 2.0 |
| [android-native-msal](https://github.com/Azure-Samples/ms-identity-android-java#b2cmodefragment-class) | Una aplicación Android sencilla que muestra cómo usar MSAL para autenticar a los usuarios mediante Azure Active Directory B2C y acceder a una API web con los tokens resultantes. |
| [ios-native-appauth](https://github.com/Azure-Samples/active-directory-b2c-ios-native-appauth) | Un ejemplo que muestra cómo se puede usar una biblioteca de terceros para compilar una aplicación de iOS en Objective-C que autentica a los usuarios de identidades de Microsoft en nuestro servicio de identidad de Azure AD B2C. |
| [android-native-appauth](https://github.com/Azure-Samples/active-directory-b2c-android-native-appauth) | Un ejemplo que muestra cómo se puede usar una biblioteca de terceros para compilar una aplicación Android que autentica a los usuarios de identidades de Microsoft en nuestro servicio de identidad B2C y llama a una API web mediante tokens de acceso de OAuth 2.0. |
| [dotnet-desktop](https://github.com/Azure-Samples/active-directory-b2c-dotnet-desktop) | Un ejemplo que muestra cómo una aplicación .NET de Windows Desktop (WPF) puede iniciar la sesión de un usuario mediante Azure AD B2C, obtener un token de acceso mediante MSAL.NET y llamar a una API. |
| [xamarin-native](https://github.com/Azure-Samples/active-directory-b2c-xamarin-native) | Una aplicación de Xamarin Forms sencilla que muestra cómo usar MSAL para autenticar a los usuarios a través de Azure Active Directory B2C y acceder a una API web con los tokens resultantes. |

## <a name="web-apps-and-apis"></a>Web Apps y API

| Muestra | Descripción |
|--------| ----------- |
| [dotnet-webapp-and-webapi](https://github.com/Azure-Samples/active-directory-b2c-dotnet-webapp-and-webapi) | Un ejemplo combinado de una aplicación web .NET que llama a una API web. NET, ambas protegidas mediante Azure AD B2C. |
| [dotnetcore-webapp-openidconnect](https://github.com/Azure-Samples/active-directory-aspnetcore-webapp-openidconnect-v2/tree/master/1-WebApp-OIDC/1-5-B2C) | Una aplicación web ASP.NET Core que usa OpenID Connect para iniciar la sesión de los usuarios en Azure AD B2C. |
| [dotnetcore-webapp-msal-api](https://github.com/Azure-Samples/active-directory-aspnetcore-webapp-openidconnect-v2/tree/master/4-WebApp-your-API/4-2-B2C) | Una aplicación web ASP.NET Core que puede iniciar la sesión de un usuario mediante Azure AD B2C, obtener un token de acceso mediante MSAL.NET y llamar a una API. |
| [openidconnect-nodejs](https://github.com/AzureADQuickStarts/B2C-WebApp-OpenIDConnect-NodeJS) | Una aplicación Node.js que proporciona una manera rápida y sencilla de configurar una aplicación web con Express mediante OpenID Connect. |
| [javascript-nodejs-webapi](https://github.com/Azure-Samples/active-directory-b2c-javascript-nodejs-webapi) | Una pequeña API web node.js para Azure AD B2C que muestra cómo proteger la API web y aceptar tokens de acceso B2C mediante passport.js. |
| [ms-identity-python-webapp](https://github.com/Azure-Samples/ms-identity-python-webapp/blob/master/README_B2C.md) | Se muestra cómo integrar B2C de la Plataforma de identidad de Microsoft con una aplicación web en Python.  |

## <a name="single-page-apps"></a>Aplicaciones de una sola página

| Muestra | Descripción |
|--------| ----------- |
| [javascript-msal-singlepageapp](https://github.com/Azure-Samples/active-directory-b2c-javascript-msal-singlepageapp) | Una aplicación de una sola página (SPA) que llama a una API web. La autenticación se realiza con Azure AD B2C mediante MSAL.js. |

## <a name="saml-test-application"></a>Aplicación de prueba SAML

| Muestra | Descripción |
|--------| ----------- |
| [saml-sp-tester](https://github.com/azure-ad-b2c/saml-sp-tester/tree/master/source-code) | Aplicación de prueba SAML para probar Azure AD B2C configurado para que actúe como proveedor de identidades SAML. |

## <a name="api-connectors"></a>Conectores de API

En las tablas siguientes se proporcionan vínculos a ejemplos de código para aprovechar las API web en los flujos de usuario con [conectores de API](api-connectors-overview.md).

## <a name="api-connector-azure-function-quickstarts"></a>Guías de inicio rápido de Azure Functions para conectores de API

| Muestra                                                                                                                          | Descripción                                                                                                                                               |
| ------------------------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------- |
| [.NET Core](https://github.com/Azure-Samples/active-directory-dotnet-external-identities-api-connector-azure-function-validate) | Este ejemplo de Azure Functions con .NET Core muestra cómo limitar los registros a dominios de correo electrónico específicos y validar la información proporcionada por el usuario. |
| [Node.js](https://github.com/Azure-Samples/active-directory-nodejs-external-identities-api-connector-azure-function-validate)   | Este ejemplo de Azure Functions con Node.js muestra cómo limitar los registros a dominios de correo electrónico específicos y validar la información proporcionada por el usuario.  |
| [Python](https://github.com/Azure-Samples/active-directory-python-external-identities-api-connector-azure-function-validate)    | Este ejemplo de Azure Functions con Python muestra cómo limitar los registros a dominios de correo electrónico específicos y validar la información proporcionada por el usuario.    |