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
ms.openlocfilehash: 04a900c98311f4e7dcccbfca93ea41e212308759
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/28/2020
ms.locfileid: "80882104"
---
# <a name="single-page-application-move-to-production"></a>Aplicación de página única: Paso a producción

Ahora que sabe cómo adquirir un token para llamar a las API web, conozca cómo pasarlo a producción.

## <a name="improve-your-app"></a>Mejorar la aplicación

[Habilite el registro](msal-logging.md) para tener la aplicación lista para producción.

## <a name="test-your-integration"></a>Probar la integración

Pruebe la integración siguiendo la [lista de comprobación de integración de la Plataforma de identidad de Microsoft](identity-platform-integration-checklist.md).

## <a name="next-steps"></a>Pasos siguientes

Profundice en el inicio rápido de ejemplo, que explica el código para iniciar la sesión de los usuarios y obtener un token de acceso para llamar a Microsoft Graph API mediante MSAL.js:

> [!div class="nextstepaction"]
> [Tutorial de SPA de JavaScript](./tutorial-v2-javascript-spa.md)

Ejemplo que demuestra cómo obtener tokens para su propia API web de back-end mediante MSAL.js:

> [!div class="nextstepaction"]
> [SPA con un back-end de ASP.NET](https://github.com/Azure-Samples/active-directory-javascript-singlepageapp-dotnet-webapi-v2)

Ejemplo que muestra cómo usar MSAL.js para iniciar la sesión de los usuarios en una aplicación registrada en Azure Active Directory B2C (Azure AD B2C):

> [!div class="nextstepaction"]
> [SPA con Azure AD B2C](https://github.com/Azure-Samples/active-directory-b2c-javascript-msal-singlepageapp)
