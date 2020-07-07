---
title: 'Configuración de la directiva de registro de MFA: Azure Active Directory Identity Protection'
description: Aprenda cómo configurar la directiva de registro de autenticación multifactor en Azure AD Identity Protection.
services: active-directory
ms.service: active-directory
ms.subservice: identity-protection
ms.topic: how-to
ms.date: 06/05/2020
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: sahandle
ms.collection: M365-identity-device-management
ms.openlocfilehash: d4c6b57eaa2a68906053faade48dd0e63fbf0db7
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/02/2020
ms.locfileid: "84464341"
---
# <a name="how-to-configure-the-azure-multi-factor-authentication-registration-policy"></a>Instrucciones: Configuración de la directiva de registro de Azure Multi-Factor Authentication

Azure AD Identity Protection le ayuda a administrar la puesta en servicio del registro de Azure Multi-Factor Authentication (MFA) mediante la configuración de una directiva de acceso condicional para requerir el registro de MFA, independientemente de la aplicación de autenticación moderna en que inicie sesión.

## <a name="what-is-the-azure-multi-factor-authentication-registration-policy"></a>¿Qué es la directiva de registro de la autenticación multifactor de Azure?

Azure Multi-factor Authentication proporciona un medio para verificar su identidad más allá del nombre de usuario y la contraseña. Proporciona una segunda capa de seguridad a los inicios de sesión de los usuarios. Para que los usuarios puedan responder a las solicitudes MFA, primero debe registrarse para Azure Multi-Factor Authentication.

Se recomienda requerir Azure Multi-Factor Authentication en los inicios de sesión de usuario por los siguientes motivos:

- Ofrece autenticación segura en una variedad de opciones de verificación.
- Desempeña un papel fundamental en la preparación de su autocorrección en caso de detecciones de riesgo en Identity Protection.

Para más información sobre Azure Multi-Factor Authentication, consulte [¿Qué es Azure Multi-Factor Authentication?](../authentication/howto-mfa-getstarted.md)

## <a name="policy-configuration"></a>Configuración de directivas

1. Acceda a [Azure Portal](https://portal.azure.com).
1. Vaya a **Azure Active Directory** > **Seguridad** > **Identity Protection** > **Directiva de registro de MFA**.
   1. En **Asignaciones**
      1. **Usuarios**: elija **Todos los usuarios** o **Seleccionar individuos y grupos** si limita el lanzamiento.
         1. Opcionalmente, puede optar por excluir usuarios de la directiva.
   1. En **Controles**
      1. Asegúrese de que la casilla **Requerir registro de Azure MFA** está activada y elija **Seleccionar**.
   1. **Aplicar directiva** - **Activado**
   1. **Guardar**

## <a name="user-experience"></a>Experiencia del usuario

Azure Active Directory Identity Protection solicitará a los usuarios que se registren la próxima vez que inicien sesión de forma interactiva y tendrán 14 días para completar el registro. Durante este período de 14 días, pueden omitir el registro, pero al final del período se les pedirá que se registren para poder completar el proceso de inicio de sesión.

Para obtener información general de la experiencia de usuario relacionada, consulte:

- [Experiencias de inicio de sesión con Azure AD Identity Protection](concept-identity-protection-user-experience.md).  

## <a name="next-steps"></a>Pasos siguientes

- [Habilitación de las directivas de riesgo de inicios de sesión y de usuarios](howto-identity-protection-configure-risk-policies.md)

- [Habilitación del autoservicio de restablecimiento de contraseña de Azure AD](../authentication/howto-sspr-deployment.md)

- [Habilitación de Azure Multi-Factor Authentication](../authentication/howto-mfa-getstarted.md)
