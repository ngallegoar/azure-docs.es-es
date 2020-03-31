---
title: 'Verificación en dos pasos en Azure MFA y ADFS: Azure Active Directory'
description: En esta página de Azure Multi-Factor Authentication se describe cómo empezar a trabajar con Azure MFA y AD FS
services: multi-factor-authentication
ms.service: active-directory
ms.subservice: authentication
ms.topic: conceptual
ms.date: 11/21/2019
ms.author: iainfou
author: iainfoulds
manager: daveba
ms.reviewer: michmcla
ms.collection: M365-identity-device-management
ms.openlocfilehash: 1fbbba49ddf2252e22cb32a0b8adc6fa2070e999
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "74847140"
---
# <a name="getting-started-with-azure-multi-factor-authentication-and-active-directory-federation-services"></a>Introducción a Azure Multi-Factor Authentication y a los Servicios de federación de Active Directory

<center>

![Introducción a Azure MFA y AD FS](./media/multi-factor-authentication-get-started-adfs/adfs.png)</center>

Si su organización ha federado su Active Directory local con Azure Active Directory mediante AD FS, tiene a su disposición dos opciones para usar Azure Multi-Factor Authentication.

* Recursos de nube seguros a través del uso de Azure Multi-Factor Authentication o los Servicios de federación de Active Directory
* Protección de recursos en la nube y locales mediante Servidor Azure Multi-Factor Authentication

En la tabla siguiente se resume la experiencia de verificación entre la protección de recursos con Azure Multi-Factor Authentication y AD FS

| Experiencia de verificación: aplicaciones basadas en explorador | Experiencia de verificación: aplicaciones no basadas en explorador |
|:--- |:--- |
| Protección de recursos de Azure AD con Azure Multi-Factor Authentication |<li>El primer paso de comprobación se realiza localmente utilizando AD FS.</li> <li>El segundo factor es un método basado en teléfono que se lleva a cabo utilizando la autenticación en la nube.</li> |
| Protección de los recursos de Azure AD mediante los servicios de federación de Active Directory |<li>El primer paso de comprobación se realiza localmente utilizando AD FS.</li><li>El segundo paso es realiza localmente respondiendo a la notificación.</li> |

Advertencias sobre las contraseñas de aplicación para usuarios federados:

* Las contraseña de aplicación se comprueban mediante autenticación en la nube y, por lo tanto, omiten las federaciones. La federación solo se usa activamente al configurar una contraseña de aplicación.
* La configuración del control de acceso de cliente local no se cumple con las contraseñas de aplicación.
* Se pierde la capacidad de registro de autenticación local para las contraseñas de aplicación.
* La deshabilitación o eliminación de la cuenta puede tardar hasta 3 horas en la sincronización de directorios, retrasando la deshabilitación o eliminación de las contraseñas de aplicación en la identidad de nube.

Para información sobre cómo configurar Azure Multi-Factor Authentication o Servidor Azure Multi-Factor Authentication con AD FS, consulte los siguientes artículos:

* [Protección de recursos en la nube con Azure Multi-Factor Authentication y AD FS](howto-mfa-adfs.md)
* [Protección de recursos en la nube y locales mediante Servidor Azure Multi-Factor Authentication con Windows Server 2012 R2 AD FS](howto-mfaserver-adfs-2012.md)
* [Protección de recursos en la nube y locales mediante Servidor Azure Multi-Factor Authentication con AD FS 2.0](howto-mfaserver-adfs-2.md)
