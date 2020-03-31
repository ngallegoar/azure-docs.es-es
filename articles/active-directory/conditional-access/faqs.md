---
title: Preguntas más frecuentes sobre el acceso condicional de Azure Active Directory | Microsoft Docs
description: Obtenga respuestas a las preguntas más frecuentes sobre el acceso condicional en Azure Active Directory.
services: active-directory
ms.service: active-directory
ms.subservice: conditional-access
ms.topic: article
ms.date: 01/15/2018
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: calebb
ms.collection: M365-identity-device-management
ms.openlocfilehash: 6842338bd27e4bea3436f0b249380ab773d60de6
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "77368089"
---
# <a name="azure-active-directory-conditional-access-faqs"></a>Preguntas más frecuentes sobre el acceso condicional de Azure Active Directory

## <a name="which-applications-work-with-conditional-access-policies"></a>¿Qué aplicaciones funcionan con directivas de acceso condicional?

Para obtener información sobre las aplicaciones que funcionan con directivas de acceso condicional, consulte [Aplicaciones y navegadores que usan reglas de acceso condicional en Azure Active Directory](concept-conditional-access-cloud-apps.md).

## <a name="are-conditional-access-policies-enforced-for-b2b-collaboration-and-guest-users"></a>¿Se aplican directivas de acceso condicional a usuarios de colaboración B2B o invitados?

Se exigen directivas a los usuarios de la colaboración de negocio a negocio (B2B), pero en algunos casos un usuario podría no satisfacer los requisitos de las directivas. Por ejemplo, la organización de un usuario invitado podría no admitir la autenticación multifactor. 

## <a name="does-a-sharepoint-online-policy-also-apply-to-onedrive-for-business"></a>¿Se aplica la directiva de SharePoint Online también a OneDrive para la Empresa?

Sí. Las directivas de SharePoint Online también se aplican a OneDrive para la Empresa.

## <a name="why-cant-i-set-a-policy-directly-on-client-apps-like-word-or-outlook"></a>¿Por qué no se puede establecer una directiva directamente en las aplicaciones cliente, como Word o Outlook?

Una directiva de acceso condicional establece los requisitos para obtener acceso a un servicio. Se aplica cuando se produce la autenticación con ese servicio. La directiva no se establece directamente en una aplicación cliente. En su lugar, se aplica cuando un cliente llama a un servicio. Por ejemplo, una directiva establecida en SharePoint se aplicará a los clientes que llamen a SharePoint. Una directiva establecida en Exchange se aplica a Outlook.

## <a name="does-a-conditional-access-policy-apply-to-service-accounts"></a>¿Se aplica una directiva de acceso condicional a las cuentas de servicio?

Las directivas de acceso condicional se aplican a todas las cuentas de usuario. Aquí se incluyen las cuentas de usuario que se usan como cuentas de servicio. A menudo sucede que una cuenta de servicio que se ejecuta de forma desatendida no puede satisfacer los requisitos de una directiva de acceso condicional. Por ejemplo, se podría requerir una autenticación multifactor. Las cuentas de servicio se pueden excluir de una directiva mediante la configuración de administración de directivas de acceso condicional. 

## <a name="are-microsoft-graph-apis-available-for-configuring-conditional-access-policies"></a>¿Está disponible Microsoft Graph API para configurar directivas de acceso condicional?

Actualmente no. 

## <a name="what-is-the-default-exclusion-policy-for-unsupported-device-platforms"></a>¿Qué es la directiva de exclusión predeterminada para plataformas de dispositivos no compatibles?

Actualmente, las directivas de acceso condicional se aplican de forma selectiva a los usuarios de dispositivos iOS y Android. De forma predeterminada, las aplicaciones de otras plataformas de dispositivos no se ven afectadas por la directiva de acceso condicional para dispositivos iOS y Android. Un administrador de inquilinos puede optar por invalidar la directiva global para impedir el acceso a los usuarios en las plataformas no compatibles.

## <a name="how-do-conditional-access-policies-work-for-microsoft-teams"></a>¿Cómo funcionan las directivas de acceso condicional para Microsoft Teams?

Microsoft Teams se basa principalmente en Exchange Online y SharePoint Online para los principales escenarios de productividad, como las reuniones, los calendarios y el uso compartido de archivos. Las directivas de acceso condicional que se establecen para estas aplicaciones de nube se aplican a Microsoft Teams cuando un usuario inicia sesión directamente en este.

Microsoft Teams también se admite por separado como aplicación de nube en las directivas de acceso condicional de Azure Active Directory. Las directivas de acceso condicional que se establecen para una aplicación en la nube se aplican a Microsoft Teams cuando un usuario inicia sesión. No obstante, los usuarios podrán acceder a esos recursos directamente aún sin las directivas correctas de otras aplicaciones como Exchange Online y SharePoint Online.

Los clientes de escritorio de Microsoft Teams para Windows y Mac admiten la autenticación moderna. La autenticación moderna proporciona un inicio de sesión basado en la biblioteca de autenticación de Azure Active Directory (ADAL) para las aplicaciones cliente de Microsoft Office en distintas plataformas.

## <a name="next-steps"></a>Pasos siguientes

- Para configurar directivas de acceso condicional para su entorno, consulte [Procedimientos recomendados para el acceso condicional en Azure Active Directory](best-practices.md). 
