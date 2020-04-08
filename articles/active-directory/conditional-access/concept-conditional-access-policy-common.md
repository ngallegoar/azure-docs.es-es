---
title: 'Directivas comunes de acceso condicional: Azure Active Directory'
description: Directivas comunes de acceso condicional para organizaciones
services: active-directory
ms.service: active-directory
ms.subservice: conditional-access
ms.topic: conceptual
ms.date: 03/25/2020
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: calebb, rogoya
ms.collection: M365-identity-device-management
ms.openlocfilehash: 20b699f0672b49dd2f947e0cf00d0ffcef7961e6
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "80295301"
---
# <a name="common-conditional-access-policies"></a>Directivas de acceso condicional habituales

Los [valores predeterminados de seguridad](../fundamentals/concept-fundamentals-security-defaults.md) son ideales para algunas organizaciones, pero muchas otras necesitan más flexibilidad que la que ofrecen. Por ejemplo, muchas organizaciones necesitan poder excluir cuentas específicas, como las cuentas de administración de acceso de emergencia, de las directivas de acceso condicional que requieren autenticación multifactor. En esas organizaciones, se pueden usar las directivas comunes a las que se hace referencia en este artículo.

![Directivas de acceso condicional en Azure Portal](./media/concept-conditional-access-policy-common/conditional-access-policies-azure-ad-listing.png)

## <a name="emergency-access-accounts"></a>Cuentas de acceso de emergencia

En los siguientes artículos encontrará más información sobre las cuentas de acceso de emergencia y por qué son importantes: 

* [Administración de cuentas de acceso de emergencia en Azure AD](../users-groups-roles/directory-emergency-access.md)
* [Crear una estrategia de administración de control de acceso resistente con Azure Active Directory](../authentication/concept-resilient-controls.md)

## <a name="typical-policies-deployed-by-organizations"></a>Directivas habituales implementadas por organizaciones

* [Bloquear la autenticación heredada](howto-conditional-access-policy-block-legacy.md)\*
* [Exigir autenticación multifactor para administradores](howto-conditional-access-policy-admin-mfa.md)\*
* [Exigir autenticación multifactor para la administración de Azure](howto-conditional-access-policy-azure-management.md)\*
* [Exigir autenticación multifactor para todos los usuarios](howto-conditional-access-policy-all-users-mfa.md)\*

\* Cuando estas cuatro directivas se configuran de forma conjunta, imitan la funcionalidad habilitada por los [valores predeterminados de seguridad](../fundamentals/concept-fundamentals-security-defaults.md).

## <a name="additional-policies"></a>Directivas adicionales

* [Acceso condicional basado en el riesgo (se necesita Azure AD Premium P2)](howto-conditional-access-policy-risk.md)
* [Exigir ubicaciones de confianza para el registro de la autenticación multifactor](howto-conditional-access-policy-registration.md)
* [Bloquear el acceso por ubicación](howto-conditional-access-policy-location.md)
* [Requerir dispositivo compatible](howto-conditional-access-policy-compliant-device.md)
* [Bloquear el acceso, excepto para aplicaciones específicas](howto-conditional-access-policy-block-access.md)

## <a name="next-steps"></a>Pasos siguientes

- [Simulación del comportamiento de inicio de sesión mediante la herramienta What If de acceso condicional.](troubleshoot-conditional-access-what-if.md)

- [Uso del modo de solo informe de acceso condicional para determinar el impacto de las nuevas decisiones de directiva.](concept-conditional-access-report-only.md)
