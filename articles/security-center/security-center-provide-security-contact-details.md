---
title: Configuración de las notificaciones por correo electrónico para las alertas de Azure Defender
description: Obtenga información sobre cómo ajustar los tipos de los correos electrónicos enviados por Azure Defender para las alertas de seguridad.
services: security-center
documentationcenter: na
author: memildin
manager: rkarlin
ms.assetid: 26b5dcb4-ce3f-4f22-8d56-d2bf743cfc90
ms.service: security-center
ms.devlang: na
ms.topic: quickstart
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 12/01/2020
ms.author: memildin
ms.openlocfilehash: 85dffd4d96a78bab9dd890d9ad37572f3e524f06
ms.sourcegitcommit: d60976768dec91724d94430fb6fc9498fdc1db37
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/02/2020
ms.locfileid: "96487939"
---
# <a name="configure-email-notifications-for-security-alerts"></a>Configuración de notificaciones de alertas de seguridad por correo electrónico 

Las alertas de seguridad deben llegar a las personas adecuadas de la organización. De forma predeterminada, Defender envía correos electrónicos a los propietarios de la suscripción cada vez que se desencadena una alerta de alta gravedad para su suscripción. En esta página se explica cómo personalizar estas notificaciones.

Para definir sus propias preferencias para los correos electrónicos de notificación, la página de configuración **Notificaciones por correo electrónico** de Azure Defender le permite elegir:

- **_A quién_ se debe notificar**: se pueden enviar mensajes de correo electrónico a usuarios individuales o a cualquier persona con un rol de Azure especificado para una suscripción. 
- **_Qué_ se les debe notificar**: modifique los niveles de gravedad para los que Defender debe enviar notificaciones.

Para evitar un exceso de alertas, Security Center limita el volumen de correos salientes. Para cada suscripción, Security Center envía:

- un máximo de un correo electrónico cada **6 horas** (4 correos electrónicos al día) para alertas de **gravedad alta**.
- un máximo de un correo electrónico cada **12 horas** (2 correos electrónicos al día) para alertas de **gravedad media**.
- un máximo de un correo electrónico cada **24 horas** (1 correo electrónico al día) para alertas de **gravedad baja**.

:::image type="content" source="./media/security-center-provide-security-contacts/email-notification-settings.png" alt-text="Configuración de los detalles de los contactos que recibirán correos electrónicos sobre las alertas de seguridad" :::
 
## <a name="availability"></a>Disponibilidad

|Aspecto|Detalles|
|----|:----|
|Estado de la versión:|Disponible con carácter general|
|Precios:|Gratuito|
|Roles y permisos necesarios:|**Administrador de seguridad**<br>**Propietario de la suscripción** |
|Nubes:|![Sí](./media/icons/yes-icon.png) Nubes comerciales<br>![Sí](./media/icons/yes-icon.png) Nacionales o soberanas (US Gov, China Gov, otros gobiernos)|
|||


## <a name="customize-the-security-alerts-email-notifications"></a>Personalización de notificaciones de alertas de seguridad por correo electrónico<a name="email"></a>

Puede enviar notificaciones por correo electrónico a individuos o a todos los usuarios con roles de Azure específicos.

1. En el área **Precios y configuración** de Defender, seleccione la suscripción correspondiente y **Notificaciones de correo electrónico**.

1. Defina los destinatarios de las notificaciones con una o ambas de estas opciones:

    - En la lista desplegable, seleccione entre los roles disponibles.
    - Escriba direcciones de correo electrónico específicas, separadas por comas. No hay ningún límite en el número de direcciones de correo electrónico que se pueden escribir.

1. Seleccione **Guardar** para aplicar la información de contacto de seguridad a su suscripción.


## <a name="see-also"></a>Consulte también
Para más información sobre las alertas de seguridad, consulte las siguientes páginas:

- [Alertas de seguridad: una guía de referencia](alerts-reference.md): obtenga información sobre las alertas de seguridad que puede ver en el módulo de protección contra amenazas de Azure Defender.
- [Administración y respuesta a las alertas de seguridad en Azure Defender](security-center-managing-and-responding-alerts.md): aprenda a administrar y responder a alertas de seguridad.
- [Automatización de flujos de trabajo](workflow-automation.md): automatice respuestas a alertas con lógica de notificación personalizada