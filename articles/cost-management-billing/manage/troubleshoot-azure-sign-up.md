---
title: Solución de problemas al registrar una cuenta nueva en Azure Portal o en el centro de cuentas de Azure
description: Resolución de problemas al intentar registrar una nueva cuenta en el centro de cuentas de Microsoft Azure Portal.
services: cost-management-billing
author: v-miegge
manager: dcscontentpm
tags: billing
ms.service: cost-management-billing
ms.topic: conceptual
ms.date: 02/12/2020
ms.author: v-miegge
ms.openlocfilehash: 7bcf3197ae218c469983200814cecf551dc42a6b
ms.sourcegitcommit: edccc241bc40b8b08f009baf29a5580bf53e220c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/24/2020
ms.locfileid: "82133182"
---
# <a name="troubleshoot-issues-when-you-sign-up-for-a-new-account-in-azure-portal-or-azure-account-center"></a>Solución de problemas al registrar una cuenta nueva en Azure Portal o en el centro de cuentas de Azure

Puede surgir algún problema al intentar registrar una nueva cuenta en Microsoft Azure Portal o en el Centro de cuentas de Azure. Antes de solucionar el problema, confirme lo siguiente:

- La información de perfil de cuenta de Azure que ha facilitado (incluidos la dirección de correo electrónico de contacto, la dirección postal y el número de teléfono) es correcta.
- La información de la tarjeta de crédito es correcta.
- No tiene ninguna cuenta Microsoft con la misma información.

> [!NOTE]
> Si ya tiene una cuenta existente y busca instrucciones para solucionar problemas de inicio de sesión, consulte [Solución de problemas de inicio de sesión en la suscripción de Azure](https://docs.microsoft.com/azure/cost-management-billing/manage/troubleshoot-sign-in-issue).

## <a name="resolutions"></a>Soluciones

Para resolver cualquier error, seleccione el problema que sufra al intentar suscribirse a Azure.

### <a name="error-we-cannot-proceed-with-sign-up-due-to-an-issue-with-your-account-please-contact-billing-support"></a>Error: *No se puede continuar con el registro debido a un problema con su cuenta. Póngase en contacto con el servicio de soporte técnico de facturación.*

Para solucionar el problema, siga estos pasos:

1. Inicie sesión en el [Centro de cuentas de Azure](https://account.azure.com/Profile) con la credencial de administrador de cuenta.

2. Seleccione **Editar detalles**.

3. Asegúrese de que todos los campos de dirección están completados y son válidos.

4. Cuando se registre para obtener una suscripción de Azure, asegúrese de que la dirección de facturación del registro de la tarjeta de crédito coincide con la de los datos bancarios.

Si sigue recibiendo el mensaje de error, pruebe a usar otro explorador para registrarse.

### <a name="progress-bar-hangs-in-identity-verification-by-card-section"></a>La barra de progreso se bloquea en la sección *Verificación de identidad mediante tarjeta*.

Para completar la verificación de identidad mediante tarjeta, se deben permitir las cookies de terceros en explorador.

![Verificación de identidad mediante tarjeta](./media/troubleshoot-azure-sign-up/identify-verification-by-card.png)

Utilice los pasos siguientes para actualizar la configuración de las cookies del explorador.

1. Si usa Chrome, seleccione **Configuración** > **Mostrar configuración avanzada** > **Privacidad** > **Configuración de contenido**. Desactive la opción **Bloquear los datos de sitios y las cookies de terceros**.

2. Si usa Microsoft Edge, seleccione **Configuración** > **Ver configuración avanzada** > **Cookies** > **No bloquear cookies**.

3. Actualice la página de registro en Azure y compruebe si el problema se ha resuelto.

4. Si la actualización no soluciona el problema, cierre y reinicie el explorador e inténtelo de nuevo.

### <a name="credit-card-form-doesnt-support-my-billing-address"></a>Incompatibilidad del formulario de tarjeta de crédito con la dirección de facturación

La dirección de facturación debe estar en el país que seleccionó en la sección **Acerca de usted**. Asegúrese de seleccionar el país correcto.

### <a name="no-text-messages-or-calls-during-sign-up-account-verification"></a>No hay mensajes de texto o llamadas durante la comprobación de la cuenta de suscripción

Aunque el proceso suele ser rápido, un código de verificación puede tardar hasta cuatro minutos en enviarse. El número de teléfono especificado a efectos de verificación no se guarda como un número de contacto para la cuenta.

Estas son algunas sugerencias adicionales:

- En el proceso de verificación telefónica no se puede usar un número de teléfono de voz sobre IP (VoIP).
- Revise el número de teléfono especificado, incluido el código de país seleccionado en el menú desplegable.
- Si el teléfono no recibe mensajes de texto (SMS), pruebe la opción **Llamarme**.
- Asegúrese de que el teléfono puede recibir llamadas o mensajes SMS desde un número de teléfono de Estados Unidos.

Cuando reciba el mensaje de texto o la llamada telefónica, escriba en el cuadro de texto el código proporcionado.

### <a name="credit-card-declined-or-not-accepted"></a>Tarjeta de crédito rechazada o no aceptada

Las tarjetas de crédito o débito virtuales o de prepago no se aceptan como opciones de pago para suscripciones de Azure. Para saber qué otros motivos pueden dar lugar a que se rechace la tarjeta, vea [La tarjeta de débito o crédito se rechazó al suscribirse a Azure](https://support.microsoft.com/help/4042960).

### <a name="free-trial-is-not-available"></a>La prueba gratuita no está disponible

¿Ha utilizado una suscripción de Azure en el pasado? El contrato de términos de uso de Azure limita la activación de la evaluación gratuita solo para un usuario que es nuevo en Azure. Si tiene cualquier otro tipo de suscripción de Azure, no puede activar una evaluación gratuita. Considere la posibilidad de suscribirse a una [suscripción de pago por uso](https://azure.microsoft.com/offers/ms-azr-0003p/).

### <a name="i-saw-a-charge-on-my-free-trial-account"></a>Hay un cargo reflejado en la cuenta de evaluación gratuita

Es posible que vea reflejado un pequeño cargo de verificación en la tarjeta de crédito después de registrarse, que desaparecerá en un plazo de entre tres y cinco días. Si le preocupan los costos de administración, lea más información en la página sobre [prevención de costos inesperados](getting-started.md).

### <a name="cant-activate-azure-benefit-plan-like-msdn-bizspark-bizsparkplus-or-mpn"></a>No se puede activar un plan de prestaciones de Azure como MSDN, BizSpark, BizSparkPlus o MPN

Asegúrese de usar las credenciales de inicio de sesión correctas. Después, compruebe el programa de beneficios para asegurarse de que puede optar a él.

- MSDN
  - Compruebe su estado de idoneidad en la [página de la cuenta de MSDN](https://msdn.microsoft.com/subscriptions/manage/default.aspx).
  - Si no puede verificar su estado, póngase en contacto con los [Centros de servicio al cliente de Suscripciones a MSDN](https://msdn.microsoft.com/library/aa493452.aspx).
- Microsoft for Startups
  - Inicie sesión en el [portal de Microsoft for Startups](https://startups.microsoft.com/#start-two) para verificar su estado de idoneidad para Microsoft for Startups.
  - Si no puede verificar el estado, puede obtener ayuda en los [foros de Microsoft for Startups](https://www.microsoftpartnercommunity.com/t5/Microsoft-for-Startups/ct-p/Microsoft_Startups).
- MPN
  - Inicie sesión en el [portal de MPN](https://mspartner.microsoft.com/Pages/Locale.aspx) y compruebe su estado de idoneidad. Si tiene las [competencias de la plataforma de nube](https://mspartner.microsoft.com/pages/membership/cloud-platform-competency.aspx) pertinentes, puede ser apto para obtener más ventajas.
  - Si no puede verificar el estado, póngase en contacto con el [soporte técnico de MPN](https://mspartner.microsoft.com/Pages/Support/Premium/contact-support.aspx).

### <a name="cant-activate-new-azure-in-open-subscription"></a>No se puede activar una nueva suscripción de Azure bajo licencia Open

Para crear una suscripción de Azure bajo licencia Open, debe contar con una clave válida de Activación del servicio en línea (OSA) que tenga asociado al menos un token de Azure bajo licencia Open. Si no tiene una clave OSA, póngase en contacto con alguno de los asociados de Microsoft que aparecen en [Microsoft Pinpoint](https://pinpoint.microsoft.com/).

### <a name="error-you-are-not-eligible-for-an-azure-subscription"></a>Error: *No es apto para una suscripción de Azure*

Para resolver este problema, compruebe si se cumplen las siguientes condiciones:

- La información de perfil de cuenta de Azure que ha facilitado (incluidos la dirección de correo electrónico de contacto, la dirección postal y el número de teléfono) es correcta.
- La información de la tarjeta de crédito es correcta.
- No tiene ninguna cuenta Microsoft en la que se use la misma información.

### <a name="error-your-current-account-type-is-not-supported"></a>Error: *Tipo de cuenta actual no admitido*

Este problema puede producirse si la cuenta está registrada en [un directorio de Azure AD no administrado](../../active-directory/users-groups-roles/directory-self-service-signup.md) y no está en el directorio de Azure AD de la organización.

Para solucionar este problema, inicie sesión en la cuenta de Azure usando otra cuenta o tome el control del directorio de AD no administrado. Para más información, vea [Adquisición de un directorio no administrado como administrador en Azure Active Directory](../../active-directory/users-groups-roles/domains-admin-takeover.md).

## <a name="additional-help-resources"></a>Más recursos de ayuda

Otros artículos de solución de problemas relativos a la facturación y las suscripciones de Azure

- [Tarjeta rechazada](troubleshoot-declined-card.md)
- [Problemas de inicio de sesión de suscripción](troubleshoot-sign-in-issue.md)
- [No se han encontrado suscripciones](no-subscriptions-found.md)
- [Se ha deshabilitado la vista de costos empresariales](enterprise-mgmt-grp-troubleshoot-cost-view.md)

## <a name="contact-us-for-help"></a>Póngase en contacto con nosotros para obtener ayuda

Si tiene alguna pregunta o necesita ayuda, [cree una solicitud de soporte técnico](https://ms.portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest).

## <a name="next-steps"></a>Pasos siguientes

- [Documentación de Facturación de Azure](../../billing/index.md)
