---
title: 'Acceso condicional: acceso condicional basado en el riesgo (Azure Active Directory)'
description: Creación de directivas de acceso condicional para permitir mejoras de Identity Protection en las directivas
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
ms.openlocfilehash: 8054d8985596095db32d9262322d7fb0f4aab8c8
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "80295153"
---
# <a name="conditional-access-risk-based-conditional-access"></a>Acceso condicional: Acceso condicional basado en el riesgo

Las organizaciones con licencias de Azure AD Premium P2 pueden crear directivas de acceso condicional que incorporen detecciones de riesgo de Azure AD Identity Protection. Hay tres directivas predeterminadas que se pueden habilitar inmediatamente. 

* Require all users to register for Azure Multi-Factor Authentication (Exigir que todos los usuarios se registren para la autenticación multifactor [MFA])
* Require a password change for users that are high risk (Exigir un cambio de contraseña para los usuarios que son de alto riesgo)
* Require multi-factor authentication for users with medium or high sign-in risk (Exigir la autenticación multifactor los usuarios con un riesgo medio o alto de inicio de sesión)

## <a name="require-all-users-to-register-for-azure-multi-factor-authentication"></a>Require all users to register for Azure Multi-Factor Authentication (Exigir que todos los usuarios se registren para la autenticación multifactor [MFA])

Al habilitar esta directiva, todos los usuarios deberán registrarse para Azure Multi-factor Authentication en el plazo de 14 días. 

1. Inicie sesión en **Azure Portal**.
1. Haga clic en **Todos los servicios** y, a continuación, vaya a **Azure AD Identity Protection**.
1. Haga clic en **Registro de MFA**.
1. En **Asignaciones**, seleccione **Usuarios**.
   1. En **Incluir**, seleccione **Todos los usuarios**.
   1. En **Excluir**, elija **Seleccionar usuarios excluidos**, elija las cuentas de acceso de emergencia de la organización y elija **Seleccionar**. 
   1. Seleccione **Listo**.
1. Establezca **Aplicar directiva** en **Activado**.
1. Haga clic en **Save**(Guardar).

## <a name="require-a-password-change-high-risk-users"></a>Require a password change high-risk users (Exigir el cambio de contraseña para usuarios de alto riesgo)

Microsoft trabaja con los investigadores, las autoridades judiciales, varios equipos de seguridad de Microsoft y otros orígenes de confianza para buscar pares de nombre de usuario y contraseña. Cuando uno de estos pares encaja con una cuenta de su entorno, se puede desencadenar un cambio de contraseña en función del riesgo mediante la siguiente directiva.

1. Inicie sesión en **Azure Portal**.
1. Haga clic en **Todos los servicios** y, a continuación, vaya a **Azure AD Identity Protection**.
1. Haga clic en **Directiva de riesgo de usuario**.
1. En **Asignaciones**, seleccione **Usuarios**.
   1. En **Incluir**, seleccione **Todos los usuarios**.
   1. En **Excluir**, elija **Seleccionar usuarios excluidos**, elija las cuentas de acceso de emergencia de la organización y elija **Seleccionar**.
   1. Seleccione **Listo**.
1. En **Condiciones**, seleccione **Riesgo de usuario** y, luego, elija **Alto**.
   1. Haga clic en **Seleccionar** y en **Listo**.
1. En **Controles** > **Acceso**, elija **Permitir acceso** y, luego, seleccione **Requerir cambio de contraseña**.
   1. Haga clic en **Seleccionar**.
1. Establezca **Aplicar directiva** en **Activado**.
1. Haga clic en **Save**(Guardar).

## <a name="require-mfa-medium-or-high-sign-in-risk-users"></a>Require MFA medium or high sign-in risk users (Exigir MFA para los usuarios con un riesgo de inicio de sesión medio o alto)

La mayoría de los usuarios tienen un comportamiento normal que puede seguirse, cuando se salen de esta norma, podría ser peligroso permitirles que simplemente inicien sesión. Es posible que sea conveniente bloquear a ese usuario o quizás simplemente puede pedirle que lleve a cabo la autenticación multifactor para demostrar que realmente es quien dice ser. Para habilitar una directiva que requiera MFA cuando se detecta un inicio de sesión peligroso, habilite la directiva siguiente.

1. Inicie sesión en **Azure Portal**.
1. Haga clic en **Todos los servicios** y, a continuación, vaya a **Azure AD Identity Protection**.
1. Haga clic en **Directiva de riesgo de inicio de sesión**.
1. En **Asignaciones**, seleccione **Usuarios**.
   1. En **Incluir**, seleccione **Todos los usuarios**.
   1. En **Excluir**, elija **Seleccionar usuarios excluidos**, elija las cuentas de acceso de emergencia de la organización y elija **Seleccionar**.
   1. Seleccione **Listo**.
1. En **Condiciones**, seleccione **Riesgo de inicio de sesión** y, luego, elija **Medio y superior**.
   1. Haga clic en **Seleccionar** y en **Listo**.
1. En **Controles** > **Acceso**, elija **Permitir acceso** y, luego, seleccione **Requerir autenticación multifactor**.
   1. Haga clic en **Seleccionar**.
1. Establezca **Aplicar directiva** en **Activado**.
1. Haga clic en **Save**(Guardar).

## <a name="next-steps"></a>Pasos siguientes

[Directivas de acceso condicional habituales](concept-conditional-access-policy-common.md)

[Determinación del impacto mediante el modo de solo informe de acceso condicional](howto-conditional-access-report-only.md)

[Simulación del comportamiento de inicio de sesión mediante la herramienta What If de acceso condicional](troubleshoot-conditional-access-what-if.md)

[Funcionamiento: Azure Multi-Factor Authentication](../authentication/concept-mfa-howitworks.md)

[¿Qué es Azure Active Directory Identity Protection?](../identity-protection/overview.md)
