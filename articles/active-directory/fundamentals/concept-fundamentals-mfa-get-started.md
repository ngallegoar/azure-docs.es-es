---
title: 'Habilitación de Multi-Factor Authentication en una organización: Azure Active Directory'
description: Conozca las características de Azure Multi-Factor Authentication disponibles para su organización según el modelo de licencia.
services: multi-factor-authentication
ms.service: active-directory
ms.subservice: authentication
ms.topic: conceptual
ms.date: 03/18/2020
ms.author: iainfou
author: iainfoulds
manager: daveba
ms.reviewer: michmcla
ms.collection: M365-identity-device-management
ms.openlocfilehash: 13bb7782f6396466200fbb3e2df77158bb75d8a4
ms.sourcegitcommit: 7374b41bb1469f2e3ef119ffaf735f03f5fad484
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/16/2020
ms.locfileid: "90705408"
---
# <a name="overview-of-azure-multi-factor-authentication-for-your-organization"></a>Introducción a Azure Multi-Factor Authentication en una organización

Hay varias maneras de habilitar Azure Multi-Factor Authentication para los usuarios de Azure Active Directory (AD) en función de las licencias que posea una organización. 

![Investigación de las señales y aplicación de MFA si es necesario](./media/concept-fundamentals-mfa-get-started/verify-signals-and-perform-mfa-if-required.png)

Según nuestros estudios, es muy improbable que una cuenta se vea comprometida si se usa la autenticación multifactor (MFA).

Así pues, ¿cómo puede activar su organización MFA incluso de forma gratuita antes de convertirse en una estadística?

## <a name="free-option"></a>Opción gratuita

Los clientes que usen las ventajas gratuitas de Azure AD pueden emplear [valores predeterminados de seguridad](../fundamentals/concept-fundamentals-security-defaults.md) para habilitar la autenticación multifactor en su entorno.

## <a name="microsoft-365-business-e3-or-e5"></a>Microsoft 365 Empresa, E3 o E5

En el caso de los clientes con Microsoft 365, hay dos opciones:

* Azure Multi-Factor Authentication está o bien habilitado o bien deshabilitado para todos los usuarios, para todos los eventos de inicio de sesión. No se puede habilitar la autenticación multifactor solo para un subconjunto de usuarios o solo en determinados escenarios. La administración se realiza a través del portal de Office 365. 
* Para mejorar la experiencia del usuario, actualice a Azure AD Premium P1 o P2 y use el acceso condicional. Para obtener más información, consulte el artículo sobre la protección de los recursos de Microsoft 365 con la autenticación multifactor.

## <a name="azure-ad-premium-p1"></a>Azure AD Premium P1

Para clientes con Azure AD Premium P1 o licencias similares que incluyen esta funcionalidad, como Enterprise Mobility + Security E3, Microsoft 365 F1 o Microsoft 365 E3: 

Utilice el [acceso condicional de Azure AD](../authentication/tutorial-enable-azure-mfa.md) si desea solicitar la autenticación multifactor a los usuarios en determinados escenarios o eventos, lo que le permitirá adaptarse a los requisitos empresariales.

## <a name="azure-ad-premium-p2"></a>Azure AD Premium P2

Para clientes con Azure AD Premium P2 o licencias similares que incluyen esta funcionalidad, como Enterprise Mobility + Security E5 o Microsoft 365 E5: 

Proporciona la opción de seguridad más potente y una experiencia de usuario mejorada. Agrega [acceso condicional basado en riesgos](../conditional-access/howto-conditional-access-policy-risk.md) a las características de Azure AD Premium P1; se adapta a los patrones del usuario y reduce el número de solicitudes de la autenticación multifactor.

## <a name="authentication-methods"></a>Métodos de autenticación

| Método | Valores predeterminados de seguridad | Todos los demás métodos |
| --- | --- | --- |
| Notificación a través de aplicación móvil | X | X |
| Código de verificación de aplicación móvil o token de hardware |   | X |
| Mensaje de texto al teléfono |   | X |
| Llamada al teléfono |   | X |

## <a name="next-steps"></a>Pasos siguientes

Para empezar, consulte el tutorial para [eventos de inicio de sesión de usuario seguro con Azure Multi-Factor Authentication](../authentication/tutorial-enable-azure-mfa.md).

Para más información sobre las licencias, consulte este artículo sobre las [características y licencias de Azure Multi-Factor Authentication](../authentication/concept-mfa-licensing.md).
