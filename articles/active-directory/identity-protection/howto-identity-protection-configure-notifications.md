---
title: Notificaciones de Azure Active Directory Identity Protection
description: Obtenga información sobre cómo contribuyen las notificaciones a sus actividades de investigación.
services: active-directory
ms.service: active-directory
ms.subservice: identity-protection
ms.topic: conceptual
ms.date: 10/18/2019
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: sahandle
ms.collection: M365-identity-device-management
ms.openlocfilehash: 0c83aa6e476bbd898999fb6efe490c7847a809ff
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "77120118"
---
# <a name="azure-active-directory-identity-protection-notifications"></a>Notificaciones de Azure Active Directory Identity Protection

Azure AD Identity Protection envía dos tipos de correos electrónicos de notificación automatizados para ayudar a administrar el riesgo del usuario y las detecciones de riesgo:

- Correo electrónico de los usuarios en riesgo detectados
- Correo electrónico de resumen semanal

En este artículo se proporciona una introducción de ambos correos electrónicos de notificación.

## <a name="users-at-risk-detected-email"></a>Correo electrónico de los usuarios en riesgo detectados

En respuesta a una cuenta detectada en riesgo, Azure AD Identity Protection genera una alerta de correo electrónico con el asunto **Usuarios en riesgo detectados**. El correo electrónico incluye un vínculo al informe **[Usuarios marcados en riesgo](../reports-monitoring/concept-user-at-risk.md)** . Como práctica recomendada, debería investigar inmediatamente los usuarios en peligro.

La configuración de esta alerta permite especificar a qué nivel de riesgo del usuario desea que se genere la alerta. Se generará el correo electrónico al alcanzar el nivel de riesgo del usuario especificado. Sin embargo, no recibirá nuevas alertas por correo electrónico de usuarios en riesgo detectados para este usuario una vez que pase a este nivel de riesgo del usuario. Por ejemplo, si establece la directiva para alertar en el riesgo de usuario medio y el usuario John pasa a riesgo medio, recibirá el correo electrónico de usuarios en riesgo detectados para John. Pero no recibirá una segunda alerta de usuario en riesgo detectado si John pasa a riesgo alto o tiene detecciones de riesgo adicionales.

![Correo electrónico de los usuarios en riesgo detectados](./media/howto-identity-protection-configure-notifications/01.png)

### <a name="configure-users-at-risk-detected-alerts"></a>Configuración de alertas detectadas sobre usuarios en riesgo

Como administrador, puede establecer:

- **El nivel de riesgo de usuario que desencadena la generación de este correo electrónico**: de forma predeterminada, el nivel de riesgo se establece en "Alto" riesgo.
- **Los destinatarios de este correo electrónico**: de forma predeterminada, los destinatarios incluyen todos los administradores globales. Los administradores globales también pueden agregar otros administradores globales, administradores de seguridad y Lectores de seguridad como destinatarios.
   - Opcionalmente, puede **agregar correos electrónicos adicionales para recibir notificaciones de alerta**. Esta característica está en una versión preliminar y los usuarios definidos deben tener los permisos adecuados para ver los informes vinculados en Azure Portal.

Configure el correo electrónico de los usuarios en riesgo en **Azure Portal** en **Azure Active Directory** > **Seguridad** > **Identity Protectionl** > **Alertas detectadas sobre usuarios en riesgo**.

## <a name="weekly-digest-email"></a>Correo electrónico de resumen semanal

El correo electrónico de resumen semanal contiene un sumario de nuevas detecciones de riesgo.  
Incluye:

- Usuarios en riesgo
- Actividades sospechosas
- Puntos vulnerables detectados
- Vínculos a los informes relacionados en Identity Protection

![Correo electrónico de resumen semanal](./media/howto-identity-protection-configure-notifications/400.png)

De forma predeterminada, los destinatarios incluyen todos los administradores globales. Los administradores globales también pueden agregar otros administradores globales, administradores de seguridad y Lectores de seguridad como destinatarios.

### <a name="configure-weekly-digest-email"></a>Configuración de correo electrónico de resumen semanal

Como administrador, puede activar o desactivar el envío de un correo electrónico de resumen semanal y elegir a los usuarios asignados para recibir el correo electrónico.

Configure el correo electrónico de resumen semanal en **Azure Portal** bajo **Azure Active Directory** > **Seguridad** > **Identity Protection** > **Resumen semanal**.

## <a name="see-also"></a>Consulte también

- [Azure Active Directory Identity Protection](../active-directory-identityprotection.md)
