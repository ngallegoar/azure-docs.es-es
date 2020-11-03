---
title: Introducción al protocolo de autenticación y sincronización de Azure Active Directory
description: Guía de arquitectura sobre la integración de Azure AD en protocolos de autenticación y patrones de sincronización heredados
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
ms.openlocfilehash: ab63bc5bd2819a239741da525eebb2404a47bbf9
ms.sourcegitcommit: 9b8425300745ffe8d9b7fbe3c04199550d30e003
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/23/2020
ms.locfileid: "92441205"
---
# <a name="azure-active-directory-integrations-with-authentication-and-synchronization-protocols"></a>Integraciones de Azure Active Directory en protocolos de autenticación y sincronización

Microsoft Azure Active Directory (Azure AD) permite la integración con muchos protocolos de autenticación y sincronización. Las integraciones de autenticación permiten usar Azure AD y sus características de seguridad y administración con pocos cambios o ninguno en las aplicaciones que usan métodos de autenticación heredados. Las integraciones de sincronización le permiten sincronizar los datos de usuarios y grupos con Azure AD y, a continuación, las capacidades de administración de usuarios de Azure AD. Algunos patrones de sincronización también permiten el aprovisionamiento automatizado.

## <a name="legacy-authentication-protocols"></a>Protocolos de autenticación heredados

En la tabla siguiente se muestra la integración de Azure AD de autenticación en protocolos de autenticación heredados y sus funcionalidades. Seleccione el nombre de un protocolo de autenticación que quiera ver.

* Descripción detallada

* Cuándo se debe usar

* Diagrama de arquitectura

* Explicación de los componentes del sistema

* Vínculos para cómo implementar la integración

 

| Protocolo de autenticación| Authentication| Authorization| Multi-Factor Authentication| Acceso condicional |
| - |- | - | - | - |
| [Autenticación basada en encabezados](auth-header-based.md)|![marca de verificación](./media/authentication-patterns/check.png)| ![marca de verificación](./media/authentication-patterns/check.png)| ![marca de verificación](./media/authentication-patterns/check.png)| ![marca de verificación](./media/authentication-patterns/check.png) |
| [Autenticación LDAP](auth-ldap.md)| ![marca de verificación](./media/authentication-patterns/check.png)| | |  |
| [Autenticación de OAuth 2.0](auth-oauth2.md)| ![marca de verificación](./media/authentication-patterns/check.png)| ![marca de verificación](./media/authentication-patterns/check.png)| ![marca de verificación](./media/authentication-patterns/check.png)| ![marca de verificación](./media/authentication-patterns/check.png) |
| [Autenticación de OIDC](auth-oidc.md)| ![marca de verificación](./media/authentication-patterns/check.png)| ![marca de verificación](./media/authentication-patterns/check.png)| ![marca de verificación](./media/authentication-patterns/check.png)| ![marca de verificación](./media/authentication-patterns/check.png) |
| [Autenticación SSO basada en contraseña](auth-password-based-sso.md )| ![marca de verificación](./media/authentication-patterns/check.png)| ![marca de verificación](./media/authentication-patterns/check.png)| ![marca de verificación](./media/authentication-patterns/check.png)| ![marca de verificación](./media/authentication-patterns/check.png) |
| [Autenticación RADIUS]( auth-radius.md)| ![marca de verificación](./media/authentication-patterns/check.png)| | ![marca de verificación](./media/authentication-patterns/check.png)| ![marca de verificación](./media/authentication-patterns/check.png) |
| [Servicios de puerta de enlace de Escritorio remoto](auth-remote-desktop-gateway.md)| ![marca de verificación](./media/authentication-patterns/check.png)| ![marca de verificación](./media/authentication-patterns/check.png)| ![marca de verificación](./media/authentication-patterns/check.png)| ![marca de verificación](./media/authentication-patterns/check.png) |
| [Secure Shell (SSH)](auth-ssh.md) |  ![marca de verificación](./media/authentication-patterns/check.png)| | ![marca de verificación](./media/authentication-patterns/check.png)| ![marca de verificación](./media/authentication-patterns/check.png) |
| [Autenticación SAML](auth-saml.md)| ![marca de verificación](./media/authentication-patterns/check.png)| ![marca de verificación](./media/authentication-patterns/check.png)| ![marca de verificación](./media/authentication-patterns/check.png)| ![marca de verificación](./media/authentication-patterns/check.png) |
| [Autenticación de Windows: delegación restringida de Kerberos](auth-kcd.md)| ![marca de verificación](./media/authentication-patterns/check.png)| ![marca de verificación](./media/authentication-patterns/check.png)| ![marca de verificación](./media/authentication-patterns/check.png)| ![marca de verificación](./media/authentication-patterns/check.png) |


 
## <a name="synchronization-patterns"></a>Patrones de sincronización

En la tabla siguiente se presenta la integración de Azure AD con los patrones de sincronización y sus funcionalidades. Seleccione el nombre de un patrón que quiera ver.

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

