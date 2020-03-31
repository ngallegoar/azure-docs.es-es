---
title: MFA o 2FA y Privileged Identity Management Azure AD | Microsoft Docs
description: Obtenga información sobre cómo Azure AD Privileged Identity Management (PIM) valida la autenticación multifactor (MFA).
services: active-directory
documentationcenter: ''
author: curtand
manager: mtillman
editor: ''
ms.service: active-directory
ms.topic: conceptual
ms.workload: identity
ms.subservice: pim
ms.date: 11/08/2019
ms.author: curtand
ms.custom: pim
ms.collection: M365-identity-device-management
ms.openlocfilehash: 6df593909c3ae5962e413eb84e64196fade0326b
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "74022153"
---
# <a name="multi-factor-authentication-and-privileged-identity-management"></a>Autenticación multifactor y Privileged Identity Management

Se recomienda exigir la autenticación multifactor (MFA) para todos los administradores. De esta forma, se reduce el riesgo de ataques debidos a que una contraseña se ha puesto en peligro.

Puede requerir que los usuarios completen un desafío de autenticación multifactor cuando inician sesión. También puede requerir que los usuarios completen un desafío de autenticación multifactor al activar un rol en Azure Active Directory (Azure AD) Privileged Identity Management (PIM). De este modo, si el usuario no ha completado un desafío de autenticación multifactor al iniciar sesión, se le pedirá que lo haga de Privileged Identity Management.

> [!IMPORTANT]
> En este momento, Azure Multi-Factor Authentication solo funciona con cuentas profesionales o educativas, no con cuentas personales de Microsoft (normalmente una cuenta personal que se usa para iniciar sesión en servicios de Microsoft como Skype, Xbox o Outlook.com). Por este motivo, cualquier persona que use una cuenta personal no puede ser un administrador válido porque no puede usar autenticación multifactor para activar sus roles. Si estos usuarios tienen que seguir administrando cargas de trabajo con una cuenta de Microsoft, conviértalos en administradores permanentes por el momento.

## <a name="how-pim-validates-mfa"></a>Cómo PIM valida MFA

Hay dos opciones para validar autenticación multifactor cuando un usuario activa un rol.

La opción más sencilla consiste en confiar en Azure Multi-Factor Authentication para los usuarios que activan un rol con privilegios. Para ello, primero compruebe que esos usuarios tienen licencia, si es necesario, y que se han registrado para Azure Multi-Factor Authentication. Para más información sobre cómo implementar Azure Multi-Factor Authentication, consulte [Implementar Azure Multi-Factor Authentication basado en la nube](../authentication/howto-mfa-getstarted.md). Se recomienda, pero no es necesario, configurar Azure AD para aplicar la autenticación multifactor a estos usuarios cuando inician sesión. Esto se debe a que las comprobaciones de autenticación multifactor se realizarán por Privileged Identity Management.

Como alternativa, si los usuarios se autentican de forma local, puede hacer que su proveedor de identidades sea responsable de la autenticación multifactor. Por ejemplo, si ha configurado Servicios de federación de AD para exigir la autenticación basada en tarjeta inteligente antes de acceder a Azure AD, el artículo [Protección de recursos en la nube con Azure Multi-Factor Authentication y AD FS](../authentication/howto-mfa-adfs.md) incluye instrucciones para configurar AD FS para enviar notificaciones a Azure AD. Cuando un usuario intenta activar un rol, Privileged Identity Management aceptará que ya se ha validado la autenticación multifactor para el usuario una vez que reciba las notificaciones adecuadas.

## <a name="next-steps"></a>Pasos siguientes

- [Configuración del rol de Azure AD en Privileged Identity Management](pim-how-to-change-default-settings.md)
- [Configuración de las opciones de rol de recursos de Azure en Privileged Identity Management](pim-resource-roles-configure-role-settings.md)
