---
title: 'Paso a producción de una aplicación de página única: Plataforma de identidad de Microsoft | Azure'
description: Aprenda a compilar una aplicación de página única (paso a producción)
services: active-directory
author: navyasric
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: conceptual
ms.workload: identity
ms.date: 05/07/2019
ms.author: nacanuma
ms.custom: aaddev
ms.openlocfilehash: 0a51442870fb72e2b3cd93d9f03736d2c679ed06
ms.sourcegitcommit: 6109f1d9f0acd8e5d1c1775bc9aa7c61ca076c45
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/10/2020
ms.locfileid: "94442827"
---
# <a name="single-page-application-move-to-production"></a>Aplicación de página única: Paso a producción

Ahora que sabe cómo adquirir un token para llamar a las API web, conozca cómo pasarlo a producción.

## <a name="improve-your-app"></a>Mejorar la aplicación

[Habilite el registro](msal-logging.md) para tener la aplicación lista para producción.

## <a name="test-your-integration"></a>Probar la integración

Pruebe la integración siguiendo la [lista de comprobación de integración de la Plataforma de identidad de Microsoft](identity-platform-integration-checklist.md).

## <a name="deploy-your-app"></a>Implementación de la aplicación

Vea un [ejemplo de implementación](https://github.com/Azure-Samples/ms-identity-javascript-angular-spa-aspnet-webapi-multitenant/tree/master/Chapter3) para aprender a implementar los proyectos de SPA y API web con Azure Storage y Azure App Services, respectivamente. 

## <a name="next-steps"></a>Pasos siguientes

- Profundice en el ejemplo del inicio rápido, donde se explica el código para iniciar la sesión de los usuarios y obtener un token de acceso para llamar a **Microsoft Graph API** mediante **MSAL.js**: [Tutorial de SPA de JavaScript](./tutorial-v2-javascript-spa.md).

- Ejemplo en el que se muestra cómo obtener tokens para una API web de back-end (ASP.NET Core) propia mediante **MSAL.js**: [SPA con un back-end de ASP.NET](https://github.com/Azure-Samples/ms-identity-javascript-angular-spa-aspnetcore-webapi).

- Ejemplo en el que se muestra cómo validar tokens de acceso para la API web de back-end (Node.js) mediante **passport-azure-ad**: [API web de Node.js (Azure AD)](https://github.com/Azure-Samples/active-directory-javascript-nodejs-webapi-v2).

- Ejemplo en el que se muestra cómo usar **MSAL.js** para iniciar la sesión de los usuarios en una aplicación registrada en **Azure Active Directory B2C** (Azure AD B2C): [SPA con Azure AD B2C](https://github.com/Azure-Samples/active-directory-b2c-javascript-msal-singlepageapp).

- Ejemplo en el que se muestra cómo usar **passport-azure-ad** para validar tokens de acceso para aplicaciones registradas con **Azure Active Directory B2C** (Azure AD B2C): [API web de Node.js (Azure AD B2C)](https://github.com/Azure-Samples/active-directory-b2c-javascript-nodejs-webapi).
