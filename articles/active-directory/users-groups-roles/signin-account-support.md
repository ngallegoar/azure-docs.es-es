---
title: ¿Mi página de inicio de sesión de Azure AD acepta cuentas de Microsoft? | Microsoft Docs
description: Cómo los mensajes en pantalla reflejan la búsqueda del nombre de usuario durante el inicio de sesión
services: active-directory
author: curtand
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.subservice: users-groups-roles
ms.topic: overview
ms.date: 11/08/2019
ms.author: curtand
ms.reviewer: kexia
ms.custom: it-pro
ms.collection: M365-identity-device-management
ms.openlocfilehash: 81ba7f77bb3396f49c3cf41d2266567aa48a8257
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/02/2020
ms.locfileid: "84731365"
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