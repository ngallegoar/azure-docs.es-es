---
title: Configuración del inicio de sesión único de la aplicación
description: Aprenda a configurar el inicio de sesión único para una aplicación personalizada desarrollada y registrada con Azure AD.
services: active-directory
author: rwike77
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.custom: aaddev
ms.workload: identity
ms.topic: conceptual
ms.date: 07/15/2019
ms.author: ryanwi
ms.openlocfilehash: 9b6451d29a3a874e09d74cbe664662a395b647f9
ms.sourcegitcommit: b396c674aa8f66597fa2dd6d6ed200dd7f409915
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/07/2020
ms.locfileid: "82890466"
---
# <a name="how-to-configure-single-sign-on-for-an-application"></a>Para configurar el inicio de sesión único para una aplicación

El inicio de sesión único federado (SSO) de la aplicación se habilita automáticamente cuando la federación se realiza a través de Azure AD para OpenID Connect, SAML 2.0, o WS-Fed. Si los usuarios finales tienen que iniciar sesión aunque ya tengan una sesión con Azure AD, es probable que la aplicación no esté configurada correctamente.

* Si utiliza ADAL/MSAL, asegúrese de que **PromptBehavior** está establecido en **Auto** y no en **Always**.

* Si está creando una aplicación móvil, tendrá que configurar otras opciones para habilitar el SSO con y sin intermediación.

En el caso de Android, consulte [Habilitación de SSO entre aplicaciones en Android](https://docs.microsoft.com/azure/active-directory/develop/active-directory-sso-android).<br>

En el caso de iOS, consulte [Habilitación del inicio de sesión único entre aplicaciones en iOS ](https://docs.microsoft.com/azure/active-directory/develop/active-directory-sso-ios).

## <a name="next-steps"></a>Pasos siguientes

[SSO en Azure AD](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)<br>

[Habilitación de SSO entre aplicaciones en Android](https://docs.microsoft.com/azure/active-directory/develop/active-directory-sso-android)<br>

[Habilitación del inicio de sesión único entre aplicaciones en iOS](https://docs.microsoft.com/azure/active-directory/develop/active-directory-sso-ios)<br>

[Integración de aplicaciones con Azure Active Directory](https://docs.microsoft.com/azure/active-directory/develop/active-directory-integrating-applications)<br>

[Permisos y consentimiento en el punto de conexión de la Plataforma de identidad de Microsoft](https://docs.microsoft.com/azure/active-directory/develop/active-directory-v2-scopes)<br>

[Azure AD en StackOverflow](https://stackoverflow.com/questions/tagged/azure-active-directory)
