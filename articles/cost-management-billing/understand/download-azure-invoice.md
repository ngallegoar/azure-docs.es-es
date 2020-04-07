---
title: Visualización y descarga de la factura de Azure
description: Describe cómo ver y descargar su factura de Azure.
keywords: factura, facturación, descarga de factura, factura de Azure, uso de Azure
author: bandersmsft
ms.reviewer: amberb
tags: billing
ms.service: cost-management-billing
ms.topic: conceptual
ms.date: 03/27/2020
ms.author: banders
ms.openlocfilehash: 16534343a831f0802a60a9214f567742153360e0
ms.sourcegitcommit: efefce53f1b75e5d90e27d3fd3719e146983a780
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/01/2020
ms.locfileid: "80478995"
---
# <a name="view-and-download-your-microsoft-azure-invoice"></a>Visualización y descarga de la factura de Microsoft Azure

También puede descargar la factura en [Azure Portal](https://portal.azure.com/) o recibirla por correo electrónico. Si es cliente de Azure con un Contrato Enterprise (cliente de EA), no puede descargar la factura de su organización. En su lugar, las facturas se envían a la persona establecida para recibir las facturas de la inscripción.

[!INCLUDE [GDPR-related guidance](../../../includes/gdpr-intro-sentence.md)]

## <a name="when-invoices-are-generated"></a>Cuándo se generan las facturas

Una factura se genera en función del tipo de cuenta de facturación. Las facturas se crean para las cuentas de facturación de Microsoft Online Service Program (MOSP), Contrato de cliente de Microsoft (MCA) y Microsoft Partner Agreement (MPA). También se generan facturas para cuentas de facturación de Contrato Enterprise (EA). Sin embargo, las facturas para cuentas de facturación de EA no se muestran en Azure Portal.

Para más información sobre las cuentas de facturación y la identificación del tipo de cuenta de facturación, consulte [Visualización de las cuentas de facturación en Azure Portal](../manage/view-all-accounts.md).

## <a name="invoices-for-mosp-billing-accounts"></a>Facturas de cuentas de facturación de MOSP

Al registrarse en Azure mediante el sitio web de Azure, se crea una cuenta de facturación de MOSP. Por ejemplo, cuando se registra para obtener una [cuenta gratuita de Azure](https://azure.microsoft.com/offers/ms-azr-0044p/), como una [cuenta con tarifas de pago por uso](https://azure.microsoft.com/offers/ms-azr-0003p/) o como un [suscriptor de Visual Studio](https://azure.microsoft.com/pricing/member-offers/credit-for-visual-studio-subscribers/).

Los clientes de algunas regiones seleccionadas, que se registran en el sitio web de Azure para una [cuenta con tarifas de pago por uso](https://azure.microsoft.com/offers/ms-azr-0003p/) o una [cuenta gratuita de Azure](https://azure.microsoft.com/offers/ms-azr-0044p/), pueden tener una cuenta de facturación para un Contrato de cliente de Microsoft.

Si no está seguro de su tipo de cuenta de facturación, consulte [Comprobación del tipo de cuenta de facturación](../manage/view-all-accounts.md#check-the-type-of-your-account) antes de seguir las instrucciones de este artículo. 

Una cuenta de facturación de MOSP puede tener las siguientes facturas:

**Cargos por servicios de Azure**: se genera una factura para cada suscripción de Azure que contenga recursos de Azure usados por la suscripción. La factura contiene los cargos de un período de facturación. El período de facturación viene determinado por el día del mes en que se creó la suscripción.

Por ejemplo, Juan crea *Azure sub 01* el 5 de marzo y *Azure sub 02* el 10 de marzo. La factura de *Azure sub 01* tendrá cargos desde el quinto día de un mes al cuarto día del mes siguiente. La factura de *Azure sub 02* tendrá cargos desde el décimo día de un mes al noveno día del mes siguiente. Las facturas para todas las suscripciones de Azure se generan normalmente el día del mes en que se creó la cuenta, pero puede ser hasta dos días más tarde. En este ejemplo, si Juan creó su cuenta el 2 de febrero, las facturas para *Azure sub 01* y *Azure sub 02* se generarán normalmente el segundo día de cada mes, pero también podrían crearse hasta dos días más tarde.

**Azure Marketplace, reservas y máquinas virtuales de Spot**: se genera una factura para las reservas, los productos de Marketplace y las máquinas virtuales de Spot adquiridos mediante una suscripción. La factura muestra los cargos correspondientes del mes anterior. Por ejemplo, Juan compró una reserva el 1 de marzo y otra reserva el 30 de marzo. Se generará una sola factura para ambas reservas en abril. Las facturas de Azure Marketplace, reservas y máquinas virtuales de Spot siempre se generan aproximadamente en el noveno día del mes.

Si paga por Azure con una tarjeta de crédito y compra una reserva, Azure genera una factura inmediata. Sin embargo, cuando se facture, se le cobrará la reserva en la siguiente factura mensual.

**Plan de soporte técnico de Azure**: se genera una factura cada mes para la suscripción del plan de soporte técnico. La primera factura se genera el día de la compra o hasta dos días más tarde. Las facturas del plan de soporte técnico posteriores se generan normalmente el mismo día del mes en que se creó la cuenta, pero puede ser hasta dos días más tarde.

## <a name="download-your-mosp-azure-subscription-invoice"></a>Descarga de la factura de suscripción de Azure MOSP

Solo se genera una factura para una suscripción que pertenece a una cuenta de facturación de MOSP. [Compruebe si tiene acceso a una cuenta de MOSP](../manage/view-all-accounts.md#check-the-type-of-your-account). 

Debe tener un rol de administrador de cuenta de una suscripción para descargar su factura. Los usuarios con roles de propietario, colaborador o lector pueden descargar su factura si el administrador de la cuenta les ha concedido permiso. Para más información, consulte [Permitir que los usuarios descarguen facturas](../manage/manage-billing-access.md#opt-in).

1. Seleccione su suscripción en la [página Suscripciones](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade) de Azure Portal.
1. Seleccione **Facturas** en la sección de facturación.  
    ![Captura de pantalla que muestra un usuario que selecciona la opción Facturas para una suscripción](./media/download-azure-invoice/select-subscription-invoice.png)
1. Seleccione **Descargar** para descargar una versión en PDF de la factura y, a continuación, seleccione **Descargar** en la sección de factura.  
    [![Captura de pantalla que muestra los períodos de facturación, la opción de descarga y los cargos totales para cada período de facturación](./media/download-azure-invoice/download-invoice-subscription.png)](./media/download-azure-invoice/download-invoice-subscription-zoomed.png#lightbox)
1. También puede descargar un desglose diario de las cantidades consumidas y los gastos estimados seleccionando **Descargar** en la sección de detalles de uso. La preparación del archivo CSV puede tardar unos minutos.  
    ![Captura de pantalla que muestra la página Descarga de factura y uso](./media/download-azure-invoice/usage-and-invoice-subscription.png)

Para más información sobre la factura, consulte [Comprender la factura de Microsoft Azure](../understand/review-individual-bill.md). Para ayudar a administrar los costos, consulte [Prevención de costos inesperados con la administración de costos y facturación de Azure](../manage/getting-started.md).

## <a name="download-your-mosp-support-plan-invoice"></a>Descarga de la factura del plan de soporte técnico de MOSP

Solo se genera una factura para una suscripción de plan de soporte técnico que pertenece a una cuenta de facturación de MOSP. [Compruebe si tiene acceso a una cuenta de MOSP](../manage/view-all-accounts.md#check-the-type-of-your-account). 

Debe tener un rol de administrador de cuenta en la suscripción de plan de soporte técnico para descargar su factura.

1.  Inicie sesión en [Azure Portal](https://portal.azure.com).
1.  Busque **Administración de costos + facturación**.  
    ![Captura de pantalla que muestra la búsqueda en el portal para la administración de costos y facturación](./media/download-azure-invoice/search-cmb.png)
1.  Seleccione **Facturas** en el lado izquierdo.
1.  Seleccione su suscripción de plan de soporte técnico y luego seleccione **Descargar**.  
    [![Captura de pantalla que muestra la lista de perfiles de facturación](./media/download-azure-invoice/cmb-invoices.png)](./media/download-azure-invoice/cmb-invoices-zoomed-in.png#lightbox)
1.  Seleccione **Descargar** para descargar una versión en PDF de la factura.  
    ![Captura de pantalla que muestra los períodos de facturación, la opción de descarga y los cargos totales para cada período de facturación](./media/download-azure-invoice/download-invoice-support-plan.png)

## <a name="allow-others-to-download-an-mosp-subscription-invoice"></a>Permitir a otros usuarios descargar una factura de suscripción de MOSP

La versión en PDF de la factura contiene información personal sobre el administrador de la cuenta, por lo que el administrador de la cuenta de esta suscripción debe conceder permiso a otros usuarios para descargar la factura. Una vez que se concede el permiso, las siguientes personas y usuarios con roles adecuados podrán descargar facturas de suscripción:

- Usuarios
- Grupos
- Entidades de servicio con un propietario
- Colaborador
- Lector
- Administrador de acceso de usuario
- Lector de facturación
- Coadministrador
- Administrador de servicios

Para descargar una factura:

1.  Inicie sesión en [Azure Portal](https://portal.azure.com) como administrador de cuenta para la suscripción.
1.  Busque **Administración de costos + facturación**.  
1.  Seleccione **Facturas** en el lado izquierdo.
1.  Seleccione su suscripción de Azure y, a continuación, seleccione **Acceder a la factura**.  
    [![Captura de pantalla que muestra la selección del acceso a la factura](./media/download-azure-invoice/cmb-select-access-to-invoice.png)](./media/download-azure-invoice/cmb-select-access-to-invoice-zoomed-in.png#lightbox)
1.  En la parte superior de la página, seleccione **Activado** y, a continuación, **Guardar**.  
    ![Captura de pantalla que muestra la selección del acceso a la factura](./media/download-azure-invoice/cmb-access-to-invoice.png)

## <a name="get-mosp-subscription-invoice-in-email"></a>Recepción de la factura de suscripción de MOSP en el correo electrónico

Debe tener un rol de administrador de cuenta en una suscripción o un plan de soporte técnico para poder recibir su factura por correo electrónico. Las facturas por correo electrónico solo están disponibles para las suscripciones y los planes de soporte técnico, no para reservas o compras de Azure Marketplace. Tras seleccionar esta opción, puede agregar destinatarios adicionales, que también recibirán la factura por correo electrónico.

1.  Inicie sesión en [Azure Portal](https://portal.azure.com).
1.  Busque **Administración de costos + facturación**.  
1.  Seleccione **Facturas** en el lado izquierdo.
1.  Seleccione la suscripción a Azure o del plan de soporte técnico y, a continuación, seleccione **Enviar factura por correo electrónico**.  
    [![Captura de pantalla que muestra la lista de perfiles de facturación](./media/download-azure-invoice/cmb-email-invoice.png)](./media/download-azure-invoice/cmb-email-invoice-zoomed-in.png#lightbox)
1. Seleccione **Habilitar envío** y acepte los términos.  
    ![Captura de pantalla que muestra el paso 2 del flujo de participación](./media/download-azure-invoice/invoice-article-step02.png)
1. La factura se enviará a su dirección de correo electrónico de comunicación preferida. Puede actualizar la dirección de correo electrónico en la [información de contacto de su cuenta de facturación](https://portal.azure.com/#blade/Microsoft_Azure_GTM/ModernBillingMenuBlade/Properties).  
    ![Captura de pantalla que muestra el paso 3 del flujo de participación](./media/download-azure-invoice/invoice-article-step03-verify-email.png)

## <a name="share-your-mosp-invoices-with-others-by-email"></a>Compartir facturas de MOSP por correo electrónico

Es posible que desee compartir las facturas de su suscripción y su plan de soporte técnico cada mes con el equipo de contabilidad o enviarlas a una de sus otras direcciones de correo electrónico.

1. Siga los pasos de [Recepción de facturas de suscripción y plan de soporte técnico por correo electrónico](#get-mosp-subscription-invoice-in-email) y seleccione **Configurar destinatarios**.  
    ![Captura de pantalla que muestra a un usuario seleccionando Configurar destinatarios](./media/download-azure-invoice/invoice-article-step03.png)
1. Escriba una dirección de correo electrónico y seleccione **Agregar destinatario**. Puede agregar varias direcciones de correo electrónico.  
    ![Captura de pantalla que muestra a un usuario agregando destinatarios adicionales](./media/download-azure-invoice/invoice-article-step04.png)
1. Una vez agregadas todas las direcciones de correo electrónico, seleccione **Listo** en la parte inferior de la pantalla.

## <a name="invoices-for-mca-and-mpa-billing-accounts"></a>Facturas de cuentas de facturación de MCA y MPA

Cuando su organización trabaja con un representante de Microsoft para firmar un Contrato de cliente de Microsoft (MCA), se crea una cuenta de facturación de MCA. Algunos clientes de regiones seleccionadas, que se registran en el sitio web de Azure para una [cuenta con tarifas de pago por uso](https://azure.microsoft.com/offers/ms-azr-0003p/) o una [cuenta gratuita de Azure](https://azure.microsoft.com/offers/ms-azr-0044p/), también pueden tener una cuenta de facturación para un Contrato de cliente de Microsoft. Para más información, consulte [Introducción a la cuenta de facturación de MCA](../understand/mca-overview.md).

Para los asociados del programa Proveedor de soluciones en la nube (CSP), se crea una cuenta de facturación de MCA con el fin de administrar sus clientes en la nueva experiencia comercial. Los asociados deben tener al menos un cliente con un [plan de Azure](https://docs.microsoft.com/partner-center/purchase-azure-plan) para administrar su cuenta de facturación en Azure Portal. Para más información, consulte [Introducción a la cuenta de facturación de MPA](../understand/mpa-overview.md).

Una factura mensual se genera al comienzo del mes para cada perfil de facturación de la cuenta. La factura incluye los cargos respectivos de todas las suscripciones de Azure y otras compras realizadas el mes anterior. Por ejemplo, Juan creó *Azure sub 01* el 5 de marzo y *Azure sub 02* el 10 de marzo. Ha adquirido la suscripción de *Soporte técnico de Azure 01* el 28 de marzo mediante *Perfil de facturación 01*. Juan recibirá una sola factura al principio de abril que contendrá los cargos de las suscripciones de Azure y del plan de soporte técnico.

##  <a name="download-an-mca-or-mpa-billing-profile-invoice"></a>Descarga de una factura de perfil de facturación de MCA o MPA

Debe tener un rol de propietario, colaborador, lector o administrador de facturación del perfil de facturación para descargar su factura en Azure Portal. Los usuarios con un rol de propietario, colaborador o lector en una cuenta de facturación pueden descargar facturas para todos los perfiles de facturación de la cuenta.

1.  Inicie sesión en [Azure Portal](https://portal.azure.com).
1.  Busque **Administración de costos + facturación**.  
1. Seleccione **Facturas** en el lado izquierdo.
    [![Captura de pantalla que muestra la página Facturas de una cuenta de facturación de MCA](./media/download-azure-invoice/mca-billing-profile-invoices.png)](./media/download-azure-invoice/mca-billing-profile-invoices-zoomed.png#lightbox)
1. En la tabla de facturas, seleccione la factura que desea descargar.
1. Seleccione **Descargar PDF de factura** en la parte superior de la página.  
    ![Captura de pantalla que muestra la descarga del archivo PDF de una factura](./media/download-azure-invoice/mca-billing-profile-download-invoice.png)
1. También puede descargar un archivo CSV que contiene un desglose diario de las cantidades consumidas y los gastos estimados seleccionando **Descargar uso de Azure**. La preparación del archivo CSV puede tardar unos minutos.

## <a name="get-your-billing-profiles-invoice-in-email"></a>Recepción de la factura del perfil de facturación por correo electrónico

Debe tener un rol de propietario o colaborador en el perfil de facturación o en su cuenta de facturación para actualizar su preferencia de factura por correo electrónico. Tras habilitar el envío, todos los usuarios con un rol de propietario, colaborador, lector o administrador de facturas de un perfil de facturación recibirán su factura por correo electrónico. 

1.  Inicie sesión en [Azure Portal](https://portal.azure.com).
1.  Busque **Administración de costos + facturación**.  
1.  Seleccione **Facturas** en el panel izquierdo y, a continuación, **Enviar factura por correo electrónico** en la parte superior de la página.  
    [![Captura de pantalla que muestra la página Facturas de una cuenta de facturación de MCA](./media/download-azure-invoice/mca-billing-profile-select-email-invoice.png)](./media/download-azure-invoice/mca-billing-profile-select-email-invoice-zoomed.png#lightbox)
1.  Si tiene varios perfiles de facturación, seleccione un perfil de facturación y, a continuación, seleccione **Habilitar envío**.  
    ![Captura de pantalla que muestra la página Facturas de una cuenta de facturación de MCA](./media/download-azure-invoice/mca-billing-profile-email-invoice.png)
1.  Seleccione **Actualizar**.

## <a name="give-others-access-to-mca-or-mpa-invoices"></a>Conceder a otros usuarios acceso a facturas de MCA o MPA

Puede conceder acceso a otros usuarios para ver, descargar y pagar facturas si les asigna el rol de administrador de facturas para un perfil de facturación de MCA o MPA. Si ha optado por recibir la factura por correo electrónico, los usuarios también recibirán las facturas en el correo electrónico.

1. Inicie sesión en [Azure Portal](https://portal.azure.com).
1. Busque **Administración de costos + facturación**.  
1. Seleccione **Perfiles de facturación** en el lado izquierdo. En la lista de perfiles de facturación, seleccione uno para el que quiera asignar un rol de administrador de facturas.  
   ![Captura de pantalla que muestra la lista de perfiles de facturación](./media/download-azure-invoice/mca-select-profile-zoomed-in.png)
1. Seleccione **Access Control (IAM)** en el lado izquierdo y, a continuación, **Agregar** en la parte superior de la página.  
    ![Captura de pantalla que muestra la página de control de acceso](./media/download-azure-invoice/mca-select-access-control-zoomed-in.png)
1. En la lista desplegable Rol, seleccione **Administrador de facturación**. Escriba la dirección de correo electrónico del usuario al que desea dar acceso. Seleccione **Guardar** para asignar el rol.  
   ![Captura de pantalla que muestra cómo agregar un usuario como administrador de facturas](./media/download-azure-invoice/mca-added-invoice-manager.png)

## <a name="why-you-might-not-see-an-invoice"></a><a name="noinvoice"></a> Por qué no puede ver una factura

Pueden existir varias razones por las que no ve una factura:

- Han transcurrido menos de 30 días desde el día que se suscribió a Azure. 
- Azure le factura unos días después del final del período de facturación. Por lo tanto, es posible que todavía no se haya generado una factura.
- No tiene permiso para ver facturas.
    Si tiene una cuenta de facturación de MCA o MPA, debe tener un rol de propietario, colaborador, lector o administrador de facturación en un perfil de facturación o el rol de propietario, colaborador o lector en la cuenta de facturación para ver facturas. Para otras suscripciones, es posible que no vea las facturas si no es el administrador de cuenta. Para obtener más información sobre cómo obtener acceso a la información de facturación, vea [Manage access to Azure billing using roles](../manage/manage-billing-access.md) (Administrar el acceso a la facturación de Azure mediante roles).
- Si tiene una cuenta de facturación de MOSP y se registró para obtener una cuenta gratuita de Azure o una suscripción con un importe de crédito mensual, solo recibirá una factura cuando supere el importe de crédito mensual. Si tiene una cuenta de facturación para un MCA o MPA, siempre recibirá una factura.

## <a name="need-help-contact-us"></a>¿Necesita ayuda? Póngase en contacto con nosotros.

Si tiene alguna pregunta o necesita ayuda, [cree una solicitud de soporte técnico](https://go.microsoft.com/fwlink/?linkid=2083458).

## <a name="next-steps"></a>Pasos siguientes

Para obtener más información acerca de la factura y los cargos, consulte:

- [Visualización y descarga de los datos de uso y los cargos de Microsoft Azure](download-azure-daily-usage.md)
- [Comprender la factura de Microsoft Azure](review-individual-bill.md)
- [Descripción de los términos en su factura de Microsoft Azure](understand-invoice.md)

Si tiene un MCA, consulte:

- [Comprender los cargos de facturación de su contrato de cliente de Microsoft](review-customer-agreement-bill.md)
- [Descripción de los términos en la factura del contrato de cliente de Microsoft](mca-understand-your-invoice.md)
- [Descripción de los términos en los CSV de cargos y el uso de Azure para un contrato de cliente de Microsoft](mca-understand-your-usage.md)


