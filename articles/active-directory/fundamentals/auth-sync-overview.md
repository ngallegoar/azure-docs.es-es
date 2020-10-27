---
title: Introducción al protocolo de autenticación y sincronización de Azure Active Directory
description: Guía arquitectónica para lograr este patrón de autenticación
services: active-directory
author: BarbaraSelden
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.subservice: fundamentals
ms.topic: conceptual
ms.date: 10/10/2020
ms.author: baselden
ms.reviewer: ajburnle
ms.custom: it-pro, seodec18
ms.collection: M365-identity-device-management
ms.openlocfilehash: 4d881dc3fe3e3caa1058cf97834735910b0de1d9
ms.sourcegitcommit: ae6e7057a00d95ed7b828fc8846e3a6281859d40
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/16/2020
ms.locfileid: "92114036"
---
# <a name="azure-active-directory-integrations-with-legacy-authentication-and-synchronization-protocols"></a>Integraciones de Azure Active Directory con protocolos de autenticación y sincronización heredados

Microsoft Azure Active Directory (Azure AD) permite la integración con muchos protocolos de autenticación y sincronización. Las integraciones de autenticación permiten usar Azure AD y sus características de seguridad y administración con pocos cambios o ninguno en las aplicaciones que usan métodos de autenticación heredados. Las integraciones de sincronización le permiten sincronizar los datos de usuarios y grupos con Azure AD y, a continuación, las capacidades de administración de usuarios de Azure AD. Algunos patrones de sincronización también permiten el aprovisionamiento automatizado.

## <a name="authentication-patterns"></a>Patrones de autenticación

En la tabla siguiente se presentan los patrones de autenticación y sus capacidades. Seleccione el nombre de un patrón de autenticación que quiera ver.

* Descripción detallada

* Cuándo se debe usar

* Diagrama de arquitectura

* Explicación de los componentes del sistema

* Vínculos para cómo implementar la integración

 

| Patrones de autenticación| Authentication| Authorization| Multi-Factor Authentication| Acceso condicional |
| - |- | - | - | - |
| [Autenticación basada en encabezados](auth-header-based.md)|![marca de verificación](./media/authentication-patterns/check.png)| ![marca de verificación](./media/authentication-patterns/check.png)| ![marca de verificación](./media/authentication-patterns/check.png)| ![marca de verificación](./media/authentication-patterns/check.png) |
| [Autenticación LDAP](auth-ldap.md)| ![marca de verificación](./media/authentication-patterns/check.png)| | |  |
| [Autenticación de OAuth 2.0](auth-oauth2.md)| ![marca de verificación](./media/authentication-patterns/check.png)| ![marca de verificación](./media/authentication-patterns/check.png)| ![marca de verificación](./media/authentication-patterns/check.png)| ![marca de verificación](./media/authentication-patterns/check.png) |
| [Autenticación de OIDC](auth-oidc.md)| ![marca de verificación](./media/authentication-patterns/check.png)| ![marca de verificación](./media/authentication-patterns/check.png)| ![marca de verificación](./media/authentication-patterns/check.png)| ![marca de verificación](./media/authentication-patterns/check.png) |
| [Autenticación SSO basada en contraseña](auth-password-based-sso.md )| ![marca de verificación](./media/authentication-patterns/check.png)| ![marca de verificación](./media/authentication-patterns/check.png)| ![marca de verificación](./media/authentication-patterns/check.png)| ![marca de verificación](./media/authentication-patterns/check.png) |
| [Autenticación RADIUS]( auth-radius.md)| ![marca de verificación](./media/authentication-patterns/check.png)| | ![marca de verificación](./media/authentication-patterns/check.png)| ![marca de verificación](./media/authentication-patterns/check.png) |
| [Servicios de puerta de enlace de Escritorio remoto](auth-remote-desktop-gateway.md)| ![marca de verificación](./media/authentication-patterns/check.png)| ![marca de verificación](./media/authentication-patterns/check.png)| ![marca de verificación](./media/authentication-patterns/check.png)| ![marca de verificación](./media/authentication-patterns/check.png) |
| [Autenticación SAML](auth-saml.md)| ![marca de verificación](./media/authentication-patterns/check.png)| ![marca de verificación](./media/authentication-patterns/check.png)| ![marca de verificación](./media/authentication-patterns/check.png)| ![marca de verificación](./media/authentication-patterns/check.png) |
| [Autenticación de Windows: delegación restringida de Kerberos](auth-kcd.md)| ![marca de verificación](./media/authentication-patterns/check.png)| ![marca de verificación](./media/authentication-patterns/check.png)| ![marca de verificación](./media/authentication-patterns/check.png)| ![marca de verificación](./media/authentication-patterns/check.png) |


 
## <a name="synchronization-patterns"></a>Patrones de sincronización

En la tabla siguiente se presentan los patrones de sincronización y sus capacidades. Seleccione el nombre de un patrón que quiera ver.

* Descripción detallada

* Cuándo se debe usar

* Diagrama de arquitectura

* Explicación de los componentes del sistema

* Vínculos para cómo implementar la integración



| Patrón de sincronización| Sincronización de directorios| Aprovisionamiento de usuarios |
| - | - | - |
| [Sincronización de directorios](sync-directory.md)| ![marca de verificación](./media/authentication-patterns/check.png)|  |
| [Sincronización LDAP](sync-ldap.md)| ![marca de verificación](./media/authentication-patterns/check.png)|  |
| [Sincronización SCIM](sync-scim.md)| ![marca de verificación](./media/authentication-patterns/check.png)| ![marca de verificación](./media/authentication-patterns/check.png) |

