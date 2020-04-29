---
title: Notificaciones por correo para Azure AD Domain Services | Microsoft Docs
description: Aprenda a configurar las notificaciones por correo para enviar alertas sobre los problemas en un dominio administrado de Azure Active Directory Domain Services
services: active-directory-ds
author: iainfoulds
manager: daveba
ms.assetid: b9af1792-0b7f-4f3e-827a-9426cdb33ba6
ms.service: active-directory
ms.subservice: domain-services
ms.workload: identity
ms.topic: how-to
ms.date: 03/30/2020
ms.author: iainfou
ms.openlocfilehash: 8c6d59889e572893877f2178cade57e07aa91413
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/28/2020
ms.locfileid: "80654787"
---
# <a name="configure-email-notifications-for-issues-in-azure-active-directory-domain-services"></a>Configuración de las notificaciones por correo electrónico para los problemas de Azure Active Directory Domain Services

La plataforma de Azure supervisa el estado de los dominios administrados de Azure Active Directory Domain Services (Azure AD DS). En la página estado de mantenimiento de Azure Portal se muestran las alertas del dominio administrado. Para asegurarse de que los problemas se abordan diligentemente, se pueden configurar notificaciones por correo para enviar las alertas de estado en cuanto se detecten en el dominio administrado de Azure AD DS.

En este artículo se muestra cómo configurar los destinatarios de las notificaciones por correo para un dominio administrado de Azure AD DS.

## <a name="email-notification-overview"></a>Introducción a las notificaciones por correo

Para avisarle de los problemas de un dominio administrado de Azure AD DS, puede configurar notificaciones por correo. En estas notificaciones por correo se especifica el dominio administrado de Azure AD DS al que hace referencia la alerta, la hora a la que se ha detectado el problema y un vínculo a la página de estado en Azure Portal. Después, puede seguir los consejos para la solución de problemas.

La notificación de correo de ejemplo siguiente indica que se ha generado una advertencia crítica o una alerta en el dominio administrado de Azure AD DS:

![Notificación por correo electrónico de ejemplo](./media/active-directory-domain-services-alerts/email-alert.png)

> [!WARNING]
> Asegúrese siempre de que el correo procede de un remitente comprobado de Microsoft antes de hacer clic en los vínculos del mensaje. Las notificaciones de correo siempre proceden de la dirección `azure-noreply@microsoft.com`.

### <a name="why-would-i-receive-email-notifications"></a>¿Por qué recibiré notificaciones por correo electrónico?

Azure AD DS envía notificaciones por correo de actualizaciones importantes en el dominio administrado. Estas notificaciones solo informan de problemas urgentes que afectan al servicio y que deben solucionarse de inmediato. Las notificaciones por correo las desencadena una alerta en el dominio administrado de Azure AD DS. Estas alertas también aparecen en Azure Portal y pueden verse en la [página de estado de Azure AD DS][check-health].

Azure AD DS no envía correos con anuncios, actualizaciones o ventas.

### <a name="when-will-i-receive-email-notifications"></a>¿Cuándo recibiré notificaciones por correo electrónico?

Se envía una notificación inmediatamente cuando una [nueva alerta][troubleshoot-alerts] se genera en el dominio administrado de Azure AD DS. Si no se resuelve la alerta, se envían notificaciones por correo adicionales como recordatorio cada cuatro días.

### <a name="who-should-receive-the-email-notifications"></a>¿Quién recibe las notificaciones por correo electrónico?

La lista de destinatarios de correo de Azure AD DS debe estar formada por las personas que puedan administrar y realizar cambios en el dominio administrado. Debe considerar a las personas de esta lista de correo como los primeros en responder ante cualquier alerta y problema.

Puede agregar hasta cinco destinatarios adicionales para las notificaciones por correo. Si desea más de cinco, en cambio, debe crear una lista de distribución y agregarla a la lista de notificaciones.

Además, puede elegir que todos los *administradores globales* del directorio de Azure AD y los miembros del grupo *Administradores de controladores de dominio de AAD* reciban las notificaciones por correo. Azure AD DS solo envía notificaciones a un máximo de 100 direcciones de correo, incluida la lista de los administradores globales y de los administradores de controladores de dominio de AAD.

## <a name="configure-email-notifications"></a>Configuración de notificaciones de correo electrónico

Para revisar los destinatarios de las notificaciones por correo existentes o agregar destinatarios adicionales, siga estos pasos:

1. En Azure Portal, busque y seleccione **Azure AD Domain Services**.
1. Seleccione el dominio administrado de Azure AD DS, por ejemplo, *aaddscontoso.com*.
1. En el lado izquierdo de la ventana de recursos de Azure AD DS, seleccione **Configuración de notificaciones**. Se muestran los destinatarios existentes para las notificaciones por correo.
1. Para agregar un destinatario de correo, escriba la dirección de correo en la tabla de destinatarios adicionales.
1. Cuando haya terminado, seleccione **Guardar** en el panel de navegación superior.

> [!WARNING]
> Al cambiar la configuración de notificación, se modifica también la de todo el dominio administrado de Azure AD DS, no solo la suya.

## <a name="frequently-asked-questions"></a>Preguntas más frecuentes

### <a name="i-received-an-email-notification-for-an-alert-but-when-i-logged-on-to-the-azure-portal-there-was-no-alert-what-happened"></a>He recibido una notificación por correo electrónico de una alerta, pero cuando inicié sesión en Azure Portal no había ninguna. ¿Qué ha ocurrido?

Si una alerta se resuelve, esta desaparece de Azure Portal. El motivo más probable de esto es que alguien que también recibe notificaciones por correo hubiera resuelto la alerta en el dominio administrado de Azure AD DS, o bien que la plataforma de Azure la hubiera solucionado automáticamente.

### <a name="why-can-i-not-edit-the-notification-settings"></a>¿Por qué no puedo editar la configuración de notificaciones?

Si no puede acceder a la página de configuración de notificación en Azure Portal, es porque no tiene permiso para editar el dominio administrado de Azure AD DS. Debe ponerse en contacto con un administrador global y que le otorgue permiso para editar el recurso de Azure AD DS o que le elimine de la lista de destinatarios.

### <a name="i-dont-seem-to-be-receiving-email-notifications-even-though-i-provided-my-email-address-why"></a>Parece que no recibo notificaciones por correo electrónico, a pesar de que he proporcionado mi dirección de correo electrónico. ¿Por qué?

Compruebe la carpeta de correo no deseado para buscar la notificación y asegúrese de permitir el correo de `azure-noreply@microsoft.com`.

## <a name="next-steps"></a>Pasos siguientes

Para más información sobre la solución de algunos problemas que se pueden notificar, consulte el artículo de [Solución de alertas en un dominio administrado de Azure AD DS][troubleshoot-alerts].

<!-- INTERNAL LINKS -->
[check-health]: check-health.md
[troubleshoot-alerts]: troubleshoot-alerts.md
