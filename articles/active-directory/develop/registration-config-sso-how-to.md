---
title: Configuración del inicio de sesión único de la aplicación | Microsoft Docs
description: Aprenda a configurar el inicio de sesión único para una aplicación personalizada desarrollada y registrada con Azure AD.
services: active-directory
documentationcenter: ''
author: rwike77
manager: CelesteDG
ms.assetid: ''
ms.service: active-directory
ms.subservice: develop
ms.custom: aaddev
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 07/15/2019
ms.author: ryanwi
ms.openlocfilehash: bb77f376e22428e9259ff3efc84cf6f1cb3491fe
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "76702648"
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

[Consentimiento y permisos para las aplicaciones convergentes v2.0 de Azure AD](https://docs.microsoft.com/azure/active-directory/develop/active-directory-v2-scopes)<br>

[Azure AD en StackOverflow](https://stackoverflow.com/questions/tagged/azure-active-directory)
