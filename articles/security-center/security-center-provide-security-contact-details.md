---
title: Provisión de detalles de contacto de seguridad en Azure Security Center | Microsoft Docs
description: En este documento se muestra cómo proporcionar detalles de contacto de seguridad en Azure Security Center.
services: security-center
documentationcenter: na
author: memildin
manager: rkarlin
ms.assetid: 26b5dcb4-ce3f-4f22-8d56-d2bf743cfc90
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/09/2019
ms.author: memildin
ms.openlocfilehash: 64a9600a3014f7e85a7f924d38882bfadaf631db
ms.sourcegitcommit: d0fd35f4f0f3ec71159e9fb43fcd8e89d653f3f2
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/30/2020
ms.locfileid: "80387825"
---
# <a name="provide-security-contact-details-in-azure-security-center"></a>Proporcionar detalles de contacto de seguridad en Azure Security Center
Azure Security Center recomendará que proporcione los detalles de contacto de seguridad para su suscripción de Azure si no lo ha hecho ya. Esta información la utilizará Microsoft para ponerse en contacto con usted si Microsoft Security Response Center (MSRC) detecta que un tercero no autorizado o ilegal ha accedido a los datos de clientes. MSRC lleva a cabo una selecta supervisión de seguridad de la red e infraestructura de Azure y recibe información sobre amenazas y quejas sobre abusos de terceros.

Se envía una notificación de correo electrónico en la primera repetición diaria de una alerta y solo en aquellas con un nivel de gravedad elevado. Las preferencias de correo electrónico solo pueden configurarse para las directivas de suscripción. Los grupos de recursos de una suscripción heredan esta configuración. Las alertas solo están disponibles en el nivel Estándar de Azure Security Center.

Se envían las notificaciones de alerta por correo electrónico:
- A un único destinatario de correo electrónico por cada tipo de alerta por día  
- Se envían no más de 3 mensajes de correo electrónico a un destinatario único en un solo día
- Cada mensaje de correo electrónico contiene una única alerta, no una agregación de alertas
- Solo para alertas de gravedad alta

> [!TIP]
> Para alertas con otros niveles de gravedad, cree una [automatización de flujos de trabajo](workflow-automation.md) para usar una aplicación lógica que envíe correos electrónicos al personal pertinente.
 
Por ejemplo, si ya se ha enviado un mensaje de correo electrónico para avisarle de un ataque de RDP, no recibirá otro mensaje de correo electrónico acerca de un ataque de RDP en el mismo día, incluso si se desencadena otra alerta. 

> [!NOTE]
> En este documento se presenta el servicio mediante una implementación de ejemplo.  No se trata de una guía paso a paso.

## <a name="set-up-email-notifications-for-alerts"></a>Configuración de notificaciones de correo electrónico para alertas <a name="email"></a>

1. Abra la página **Notificaciones de correo electrónico**:

    - En el caso de las alertas, abra **Precios y configuración**, seleccione la suscripción correspondiente y **Notificaciones de correo electrónico**.

    - Si va a implementar una recomendación, en **Recomendaciones**, seleccione **Proporcionar detalles de contacto de seguridad** y elija la suscripción de Azure de la que quiere proporcionar información de contacto. Así se abren las **Notificaciones por correo electrónico**.

   ![Proporcionar datos de los contactos de seguridad][2]

1. Escriba la dirección de correo electrónico del contacto de seguridad o direcciones separadas por comas. No hay ningún límite en el número de direcciones de correo electrónico que se pueden escribir.

1. Para recibir correos electrónicos de alertas de gravedad alta, active la opción **Send me emails about alert**(Enviar correos electrónicos de alertas). Para el resto de niveles de gravedad, use una aplicación lógica, tal como se explica en el artículo [Automatización de flujos de trabajo](workflow-automation.md).

1. Puede enviar notificaciones por correo electrónico a los propietarios de la suscripción (Administrador de servicios clásico y Coadministradores, además del rol Propietario de RBAC en el ámbito de la suscripción).

1. Seleccione **Guardar** para aplicar la información de contacto de seguridad a su suscripción.

## <a name="see-also"></a>Consulte también
Para más información sobre el Centro de seguridad, consulte los siguientes recursos:

* [Establecimiento de directivas de seguridad en Azure Security Center](tutorial-security-policy.md): aprenda a configurar directivas de seguridad para las suscripciones y los grupos de recursos de Azure.
* [Administración de recomendaciones de seguridad en Azure Security Center](security-center-recommendations.md): recomendaciones que le ayudan a proteger los recursos de Azure.
* [Supervisión del estado de seguridad en Azure Security Center](security-center-monitoring.md): obtenga información sobre cómo supervisar el mantenimiento de los recursos de Azure.
* [Administración y respuesta a las alertas de seguridad en Azure Security Center](security-center-managing-and-responding-alerts.md) : obtenga información sobre cómo administrar y responder a alertas de seguridad.
* [Supervisión de las soluciones de asociados con Azure Security Center](security-center-partner-solutions.md): aprenda a supervisar el estado de mantenimiento de las soluciones de asociados.

<!--Image references-->
[1]: ./media/security-center-provide-security-contacts/provide-contacts.png
[2]:./media/security-center-provide-security-contacts/provide-contact-details.png
