---
title: ¿Mi página de inicio de sesión de Azure AD acepta cuentas de Microsoft? | Microsoft Docs
description: Cómo los mensajes en pantalla reflejan la búsqueda del nombre de usuario durante el inicio de sesión
services: active-directory
author: curtand
manager: daveba
ms.service: active-directory
ms.subservice: enterprise-users
ms.workload: identity
ms.topic: overview
ms.date: 12/02/2020
ms.author: curtand
ms.reviewer: kexia
ms.custom: it-pro
ms.collection: M365-identity-device-management
ms.openlocfilehash: 2c3b1c8d35c24d78abb8a519ddc8790649eb5f2b
ms.sourcegitcommit: 65db02799b1f685e7eaa7e0ecf38f03866c33ad1
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/03/2020
ms.locfileid: "96547872"
---
# <a name="sign-in-options-for-microsoft-accounts-in-azure-active-directory"></a>Opciones de inicio de sesión para cuentas Microsoft en Azure Active Directory

La página de inicio de sesión de Microsoft 365 para Azure Active Directory (Azure AD) admite cuentas educativas o profesionales y cuentas Microsoft, pero según la situación del usuario, podría ser una, la otra o ambas. Por ejemplo, la página de inicio de sesión de Azure AD admite:

* Aplicaciones que aceptan inicios de sesión con ambos tipos de cuenta
* Organizaciones que aceptan invitados

## <a name="identification"></a>Identificación
Para saber si la página de inicio de sesión que usa su organización admite cuentas Microsoft, examine el texto de sugerencia en el campo de nombre de usuario. Si en el texto de sugerencia pone "Correo electrónico, teléfono o Skype", la página de inicio de sesión admite cuentas Microsoft.

![Diferencia entre las páginas de inicio de sesión de las cuentas](./media/signin-account-support/ui-prompt.png)

Las [opciones de inicio de sesión adicionales solo funcionan con cuentas Microsoft personales](https://azure.microsoft.com/updates/microsoft-account-signin-options/ ), no se pueden usar para iniciar sesión en recursos de cuentas profesionales o educativas.

## <a name="next-steps"></a>Pasos siguientes

[Personalización de marca del inicio de sesión](../fundamentals/add-custom-domain.md)