---
title: Pago de suscripciones de Azure con factura
description: Describe cómo pagar las suscripciones de Azure mediante factura.
author: bandersmsft
ms.reviewer: judupont
tags: billing
ms.service: cost-management-billing
ms.topic: conceptual
ms.date: 02/13/2020
ms.author: banders
ms.openlocfilehash: 12ad1f09c988171b0d6212d79ca61d93acf92d80
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/24/2020
ms.locfileid: "77199609"
---
# <a name="pay-for-your-azure-subscription-by-invoice"></a>Pago de las suscripciones de Azure con factura

Si cambia al pago con factura, significa que pagará el importe en un plazo de 30 días después de la fecha de factura mediante cheque o transferencia bancaria. Para ser apto para pagar su suscripción de Azure mediante factura, envíe una solicitud a Soporte técnico de Azure. Una vez aprobada su solicitud, puede cambiar al pago con factura (cheque o transferencia bancaria) en [Azure Portal](https://portal.azure.com).

> [!IMPORTANT]
> * El pago (por cheque o transferencia bancaria) de la factura solo está disponible para los clientes que usan Azure en nombre de una empresa.
> * Pague todos los cargos pendientes antes de cambiar al pago con factura.
> * Actualmente, no se admite el pago de facturas para Azure global en China.

## <a name="request-to-pay-by-invoice"></a>Solicitud de pago con factura

1. Vaya a [Azure Portal](https://portal.azure.com) para enviar una solicitud de soporte técnico. Busque y seleccione **Ayuda y soporte técnico**.

    ![Busca de ayuda y soporte técnico, Microsoft Azure Portal](./media/pay-by-invoice/search-for-help-and-support.png)

2. Seleccione **Nueva solicitud de soporte técnico**.

    ![Vínculo a la nueva solicitud de soporte técnico, pantalla Ayuda y soporte técnico, Microsoft Azure Portal](./media/pay-by-invoice/help-and-support.png)

2. Seleccione **Facturación** como **tipo de incidencia**. El *tipo de incidencia* es la categoría de la solicitud de soporte técnico. Seleccione la suscripción que quiere pagar con factura, seleccione un plan de soporte técnico y, después, haga clic en **Siguiente**.

3. Seleccione **Pago** como el **tipo de problema**. El *tipo de problema* es la subcategoría de la solicitud de soporte técnico.

4. Seleccione **Switch to Pay by Invoice** (Cambiar a pago por factura) como el **subtipo de problema**.

5. Escriba la siguiente información en el cuadro **Detalles** y después haga clic en **Siguiente**.

         New or existing customer:
         If existing, current payment method:
         Order ID (requesting for invoice option):
         Account Admins Live ID (or Org ID) (should be company domain):
         Commerce Account ID:
         Company Name (as registered under VAT or Government Website):
         Company Address (as registered under VAT or Government Website):
         Company Website:
         Country:
         TAX ID/ VAT ID:
         Company Established on (Year):
         Any prior business with Microsoft:
         Contact Name:
         Contact Phone:
         Contact Email:
         Justification on why you prefer Invoice option over credit card:

         For cores increase, provide the following additional information:

         (Old quota) Existing Cores:
         (New quota) Requested cores:
         Specific region & series of Subscription:

    - El **nombre de la empresa** y la **dirección de la empresa** deben coincidir con la información que haya proporcionado para la cuenta de Azure. Para ver o actualizar la información, consulte [Cambio de la información de perfil de la cuenta de Azure](change-azure-account-profile.md).
    - Agregue la información de contacto de facturación en Azure Portal antes de que se pueda aprobar el límite de crédito. La información de contacto debe ser la del departamento de Contabilidad o Finanzas. Para actualizar la información de contacto de facturación, vaya al [Centro de cuentas de Azure](https://account.azure.com/Profile).

6. Compruebe la información de contacto y el método de contacto preferido, y seleccione **Crear**.

Si es necesario realizar una comprobación de crédito debido a la cantidad de crédito que necesita, le enviaremos una solicitud a tal fin.

## <a name="switch-to-invoice-pay-checkwire-transfer"></a>Cambio a pago con factura (cheque o transferencia bancaria)

Una vez aprobada su solicitud, puede cambiar al pago con factura (cheque o transferencia bancaria) en Azure Portal.

Si tiene una cuenta del programa de Microsoft Online Services, puede cambiar su suscripción de Azure para pagar mediante cheque o transferencia bancaria. Con un contrato de cliente de Microsoft, puede cambiar el perfil de facturación para pagar mediante cheque o transferencia bancaria. [Aprenda a comprobar el tipo de cuenta](#check-access-to-a-microsoft-customer-agreement).

### <a name="switch-azure-subscription-to-checkwire-transfer"></a>Cambio de la suscripción de Azure para pagar mediante cheque o transferencia bancaria

Siga los pasos siguientes para cambiar su suscripción de Azure para pagar mediante cheque o transferencia bancaria. *Una vez que cambie al pago por factura (cheque o transferencia bancaria), no podrá volver al pago con tarjeta de crédito*.

1. Vaya a [Azure Portal](https://portal.azure.com) para iniciar sesión como administrador de cuenta. Busque y seleccione **Administración de costos + facturación**.

    ![Buscar Administración de costos y facturación, Microsoft Azure Portal](./media/pay-by-invoice/search.png)

1. Seleccione la suscripción que le gustaría cambiar a pago con factura.
1. Seleccione **Métodos de pago**.
1. En la barra de comandos, seleccione el botón **Pagar por factura**.

    ![Botón Pagar por factura, métodos de pago, Microsoft Azure Portal](./media/pay-by-invoice/pay-by-invoice.png)

### <a name="switch-billing-profile-to-checkwire-transfer"></a>Cambio del perfil de facturación para pagar mediante cheque o transferencia bancaria

Siga los pasos siguientes para cambiar un perfil de facturación para pagar mediante cheque o transferencia bancaria. Solo la persona que se registró en Azure puede cambiar el método de pago predeterminado de un perfil de facturación.

1. Vaya a [Azure Portal](https://portal.azure.com) para ver la información de facturación. Busque y seleccione **Administración de costos + facturación**.
1. En el menú, elija **Perfiles de facturación**.

    ![Elemento de menú Perfiles de facturación, Administración de costos y facturación, Microsoft Azure Portal](./media/pay-by-invoice/billing-profile.png)

1. Seleccione un perfil de facturación.
1. En el menú **Perfil de facturación**, seleccione **Métodos de pago**.

   ![Elemento de menú Métodos de pago, Perfiles de facturación, Administración de costos, Microsoft Azure Portal](./media/pay-by-invoice/billing-profile-payment-methods.png)

1. Seleccione el banner azul que le informa que es apto para pagar mediante cheque o transferencia bancaria.

    ![Banner para cambiar a cheque o transferencia, métodos de pago, Microsoft Azure Portal](./media/pay-by-invoice/customer-led-switch-to-invoice.png)

## <a name="check-access-to-a-microsoft-customer-agreement"></a>Comprobación del acceso a un contrato de cliente de Microsoft
[!INCLUDE [billing-check-mca](../../../includes/billing-check-mca.md)]

## <a name="need-help-contact-us"></a>¿Necesita ayuda? Póngase en contacto con nosotros.

Si tiene alguna pregunta o necesita ayuda, [cree una solicitud de soporte técnico](https://go.microsoft.com/fwlink/?linkid=2083458).

## <a name="next-steps"></a>Pasos siguientes

- Si es necesario, actualice la información de contacto de facturación en el [Centro de cuentas de Azure](https://account.azure.com/Profile).
