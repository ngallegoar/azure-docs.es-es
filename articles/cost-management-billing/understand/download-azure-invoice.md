---
title: Visualización y descarga de la factura de Microsoft Azure
description: Describe cómo ver y descargar su factura de Microsoft Azure.
keywords: factura, facturación, descarga de factura, factura de Azure, uso de Azure
author: bandersmsft
ms.reviewer: judupont
tags: billing
ms.service: cost-management-billing
ms.topic: conceptual
ms.date: 02/13/2020
ms.author: banders
ms.openlocfilehash: 691d27acebf238e84265870e8c01976bfc2412b2
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/24/2020
ms.locfileid: "77200272"
---
# <a name="view-and-download-your-microsoft-azure-invoice"></a>Visualización y descarga de la factura de Microsoft Azure

En la mayoría de las suscripciones, puede descargar la factura desde [Azure Portal](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade) o recibirla por correo electrónico. Si es cliente de Azure con un Contrato Enterprise (cliente de EA), no puede descargar las facturas de su organización. Las facturas se envían a la persona que se configuró para recibir las facturas de la inscripción.

Solo ciertos roles tienen permiso para ver las facturas. Por ejemplo, el administrador de cuenta o el administrador de empresa. Para más información sobre cómo obtener acceso a la información de facturación, consulte el artículo sobre la [administración del acceso a la facturación de Azure mediante roles](../manage/manage-billing-access.md).

Si dispone de un contrato de cliente de Microsoft (MCA), debe tener uno de los roles siguientes para obtener las facturas:

- Propietario del perfil de facturación
- Colaborador
- Lector
- Administrador de facturación

Si tiene un contrato Microsoft Partner Agreement (MPA), debe ser el administrador global o el agente de administrador de la organización asociada para ver y descargar las facturas de Azure. [Compruebe el tipo de cuenta de facturación ](#check-your-billing-account-type) para ver qué permisos necesita.

<!-- For more information about billing roles for Microsoft Customer Agreements, see [Billing profile roles and tasks](../manage/understand-mca-roles.md#billing-profile-roles-and-tasks). -->

## <a name="why-you-might-not-get-an-invoice"></a><a name="noinvoice"></a> ¿Por qué no puede obtener una factura?

Pueden existir varias razones por las que no ve una factura:

- Han transcurrido menos de 30 días desde el día que se suscribió a Azure.

- Azure le cobra al final del período de facturación. Por lo tanto, es posible que todavía no se haya generado una factura. Espere hasta el final del período de facturación.

- No tiene permiso para ver facturas. Si tiene un MCA o un MPA, debe ser el propietario del perfil de facturación, colaborador, lector o administrador de facturación. Para otras suscripciones, es posible que no vea las facturas antiguas si no es el administrador de cuenta. Para obtener más información sobre cómo obtener acceso a la información de facturación, vea [Manage access to Azure billing using roles](../manage/manage-billing-access.md) (Administrar el acceso a la facturación de Azure mediante roles).

- Si tiene una evaluación gratuita o un importe de crédito mensual con la suscripción, solo recibirá una factura cuando exceda el importe de crédito mensual. Si tiene un contrato de cliente de Microsoft o un contrato Microsoft Partner Agreement, siempre recibirá una factura.

## <a name="download-invoices-in-the-azure-portal"></a>Descarga de facturas en Azure Portal

1. Inicie sesión en [Azure Portal](https://portal.azure.com).
1. Busque *Administración de costos + facturación*.
1. Dependiendo de su acceso, es posible que tenga que seleccionar una cuenta de facturación o un perfil de facturación.
1. En el menú de la izquierda, seleccione **Facturas** en **Facturación**.
1. En la cuadrícula de la factura, busque la fila de la factura que desea descargar.
1. Haga clic en el icono de descargas o los puntos suspensivos (`...`) al final de la fila.
1. En el menú de descargas, seleccione **Factura**.

Para más información sobre la factura, consulte [Comprender la factura de Microsoft Azure](review-individual-bill.md). Para ayudar a administrar los costos, consulte [Prevención de costos inesperados con la administración de costos y facturación de Azure](../manage/getting-started.md).

## <a name="get-your-subscriptions-invoices-in-email"></a>Obtención de las facturas de su suscripción por correo electrónico

Puede optar a destinatarios adicionales y configurarlos para recibir la factura de Azure en un correo electrónico. Esta característica puede no estar disponible para determinadas suscripciones, como ofertas de soporte técnico, contratos Enterprise o Azure bajo licencia Open.

1. Seleccione su suscripción en la [página Suscripciones](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade). Tendrá que habilitar la opción en cada suscripción que posea. Haga clic en **Facturas** y luego en **Email my invoice** (Enviar mi factura por correo electrónico).

    ![Captura de pantalla que muestra el flujo de participación](./media/download-azure-invoice/invoicesdeeplink01.png)

2. Haga clic en **Recibir notificaciones** y acepte los términos.

    ![Captura de pantalla que muestra el paso 2 del flujo de participación](./media/download-azure-invoice/invoicearticlestep02.png)

3. Una vez que ha aceptado el contrato, puede configurar destinatarios adicionales. Cuando se quita un destinatario, ya no se almacena la dirección de correo electrónico. Si cambia de opinión, deberá volver a agregarlos.

    ![Captura de pantalla que muestra el paso 3 del flujo de participación](./media/download-azure-invoice/invoicearticlestep03.png)

Si no recibe un correo electrónico después de seguir los pasos, asegúrese de que su dirección de correo electrónico es correcta en las [preferencias de comunicación de su perfil](https://account.windowsazure.com/profile).

## <a name="opt-out-of-getting-your-subscriptions-invoices-in-email"></a>Opción de no recibir las facturas de su suscripción por correo electrónico

Puede optar por no recibir la factura por correo electrónico; para ello, siga los pasos anteriores y haga clic en **Deshabilitar el envío de facturas por correo electrónico**. Esta opción eliminará todas las direcciones de correo electrónico configuradas para recibir las facturas por correo electrónico. Si cambia de opinión, tendrá que volver a configurar los destinatarios.

 ![Captura de pantalla que muestra el flujo para deshabilitar](./media/download-azure-invoice/invoicearticlestep04.png)

<!-- Does following section apply to MPA too? -->
## <a name="get-your-microsoft-customer-agreement-invoices-in-email"></a>Obtención de las facturas del contrato de cliente de Microsoft por correo electrónico

Si tiene una cuenta de facturación del Contrato de cliente de Microsoft, puede optar por recibir la factura por correo electrónico. Todos los usuarios con un rol de propietario, colaborador, lector o administrador de facturas de un perfil de facturación recibirán su factura por correo electrónico.

1. Inicie sesión en [Azure Portal](https://portal.azure.com).

1. Busque **Administración de costos + facturación**.

   ![Captura de pantalla que muestra la búsqueda de suscripciones en el portal](./media/download-azure-invoice/search-cmb.png)

1. Seleccione **Perfiles de facturación** en el lado izquierdo. En la lista de perfiles de facturación, seleccione un perfil de facturación para recibir sus facturas por correo electrónico.

   [![Captura de pantalla que muestra la lista de perfiles de facturación](./media/download-azure-invoice/mca-select-profile.png)](./media/download-azure-invoice/mca-select-profile-zoomed-in.png#lightbox)

1. Seleccione **Propiedades** en el lado izquierdo y luego seleccione **Actualizar la preferencia de factura por correo electrónico**.

   [![Captura de pantalla que muestra la lista de perfiles de facturación](./media/download-azure-invoice/mca-select-update-email-preferences.png)](./media/download-azure-invoice/mca-select-update-email-preferences.png#lightbox)

1. Seleccione **Habilitar envío** y, después, haga clic en **Actualizar**.

   [![Captura de pantalla que muestra la lista de perfiles de facturación](./media/download-azure-invoice/mca-select-email-opt-in.png)](./media/download-azure-invoice/mca-select-email-opt-in.png#lightbox)

## <a name="opt-out-of-getting-your-microsoft-customer-agreement-invoices-in-email"></a>Dejar de recibir las facturas del contrato de cliente de Microsoft por correo electrónico

Puede optar por no recibir la factura por correo electrónico; para ello, siga los pasos anteriores y haga clic en **No participar**. Todos los usuarios con un rol de propietario, colaborador, lector o administrador de facturas dejarán de recibir la factura por correo electrónico.

## <a name="give-others-access-to-your-microsoft-customer-agreement-invoices"></a>Concesión de acceso a otros usuarios a las facturas del Contrato de cliente de Microsoft

Puede conceder acceso a otros usuarios para ver, descargar y pagar facturas si les asigna el rol de administrador de facturas para un perfil de facturación. Si ha optado por recibir la factura por correo electrónico, estos usuarios también recibirán las facturas en el correo electrónico.

1. Inicie sesión en [Azure Portal](https://portal.azure.com).

1. Busque **Administración de costos + facturación**.

   ![Captura de pantalla que muestra la búsqueda de suscripciones en el portal](./media/download-azure-invoice/search-cmb.png)

1. Seleccione **Perfiles de facturación** en el lado izquierdo. En la lista de perfiles de facturación, seleccione uno para el que quiera asignar un rol de administrador de facturas.

   [![Captura de pantalla que muestra la lista de perfiles de facturación](./media/download-azure-invoice/mca-select-profile.png)](./media/download-azure-invoice/mca-select-profile-zoomed-in.png#lightbox)

1. Seleccione **Access Control (IAM)** en el lado izquierdo y, a continuación, **Agregar** en la parte superior de la página.

   [![Captura de pantalla que muestra la página de control de acceso](./media/download-azure-invoice/mca-select-access-control.png)](./media/download-azure-invoice/mca-select-access-control-zoomed-in.png#lightbox)

1. En la lista desplegable Rol, seleccione **Administrador de facturación**. Escriba la dirección de correo electrónico del usuario al que desea dar acceso. Seleccione **Guardar** para asignar el rol.

   [![Captura de pantalla que muestra cómo agregar un usuario como administrador de facturas](./media/download-azure-invoice/mca-added-invoice-manager.png)](./media/download-azure-invoice/mca-added-invoice-manager.png#lightbox)

## <a name="check-your-billing-account-type"></a>Comprobación del tipo de cuenta de facturación
[!INCLUDE [billing-check-account-type](../../../includes/billing-check-account-type.md)]

## <a name="need-help-contact-us"></a>¿Necesita ayuda? Póngase en contacto con nosotros.

Si tiene alguna pregunta o necesita ayuda, [cree una solicitud de soporte técnico](https://go.microsoft.com/fwlink/?linkid=2083458).

## <a name="next-steps"></a>Pasos siguientes

Para obtener más información acerca de la factura y los cargos, consulte:

- [Visualización y descarga de los datos de uso y los cargos de Microsoft Azure](download-azure-daily-usage.md)
- [Comprender la factura de Microsoft Azure](review-individual-bill.md)
- [Descripción de los términos en su factura de Microsoft Azure](understand-invoice.md)
- [Descripción de los términos de uso detallados de Microsoft Azure](understand-usage.md)
- [Visualización de los precios de Azure de su organización](../manage/ea-pricing.md)

Si tiene un contrato de cliente de Microsoft, vea:

- [Comprender los cargos de facturación de su contrato de cliente de Microsoft](review-customer-agreement-bill.md)
- [Descripción de los términos en la factura del contrato de cliente de Microsoft](mca-understand-your-invoice.md)
- [Descripción de los términos en los CSV de cargos y el uso de Azure para un contrato de cliente de Microsoft](mca-understand-your-usage.md)
- [Ver los documentos de impuestos del contrato de cliente de Microsoft](mca-download-tax-document.md)
- [Visualización de los precios de Azure de su organización](../manage/ea-pricing.md)
