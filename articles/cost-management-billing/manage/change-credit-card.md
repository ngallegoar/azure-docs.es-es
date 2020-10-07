---
title: Cambio de la tarjeta de crédito para Azure
description: Describe cómo cambiar la tarjeta de crédito que se usa para pagar una suscripción de Azure.
author: bandersmsft
ms.reviewer: judupont
tags: billing
ms.service: cost-management-billing
ms.subservice: billing
ms.topic: conceptual
ms.date: 09/15/2020
ms.author: banders
ms.openlocfilehash: 6b4b5fe5d7dffbcacc804500cc1f3f8f07f0090d
ms.sourcegitcommit: 6a4687b86b7aabaeb6aacdfa6c2a1229073254de
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/06/2020
ms.locfileid: "91758547"
---
# <a name="add-or-update-a-credit-card-for-azure"></a>Adición o actualización de una tarjeta de crédito para Azure

Este documento se aplica a los clientes que se han suscrito a Azure online con una tarjeta de crédito.

En Azure Portal, no solo puede cambiar el método de pago predeterminado a una nueva tarjeta de crédito, sino también actualizar los detalles de la tarjeta de crédito. Debe ser [administrador de la cuenta](../understand/subscription-transfer.md#whoisaa) para realizar estos cambios.

Si desea eliminar una tarjeta de crédito, consulte [Eliminación de un método de pago de la facturación de Azure (versión preliminar)](delete-azure-payment-method.md).

Los métodos de pago admitidos para Microsoft Azure son tarjetas de crédito y cheques o transferencias bancarias. Para obtener la aprobación para pagar por cheque o transferencia bancaria, consulte [Pago de las suscripciones de Azure con factura](pay-by-invoice.md).

Con un Contrato de cliente de Microsoft, los métodos de pago se asocian a perfiles de facturación. Aprenda a [comprobar el acceso a un Contrato de cliente de Microsoft](#check-the-type-of-your-account). Si tiene un Contrato de cliente de Microsoft, consulte el tema sobre cómo [administrar tarjetas de crédito para un Contrato de cliente de Microsoft](#manage-credit-cards-for-a-microsoft-customer-agreement).

<a id="addcard"></a>

## <a name="manage-credit-cards-for-an-azure-subscription"></a>Administración de tarjetas de crédito para una suscripción de Azure

Las secciones siguientes se aplican a los clientes que tienen una cuenta de facturación del programa Microsoft Online Services. Aprenda a [comprobar el tipo de cuenta de facturación](#check-the-type-of-your-account). Si el tipo de cuenta de facturación es el programa de Microsoft Online Services, los métodos de pago se asocian a las suscripciones individuales de Azure. Si se produce un error después de agregar la tarjeta de crédito, consulte [Tarjeta de crédito rechazada al suscribirse a Azure](/azure/cost-management-billing/manage/troubleshoot-declined-card).

### <a name="change-credit-card-for-a-subscription-by-adding-a-new-credit-card"></a>Cambio de la tarjeta de crédito de una suscripción mediante la adición de una nueva tarjeta de crédito

Puede cambiar el crédito predeterminado de la suscripción de Azure a una nueva tarjeta de crédito o a una tarjeta de crédito guardada previamente en Azure Portal. Para cambiar la tarjeta de crédito, debe ser el administrador de cuenta. Si se tiene el mismo método de pago activo en varias suscripciones, el cambio del método de pago activo en cualquiera de las suscripciones también actualizará el método de pago activo en las demás.

Puede cambiar la tarjeta de crédito predeterminada de la suscripción a una nueva siguiendo estos pasos:

1. Inicie sesión en [Azure Portal](https://portal.azure.com) como administrador de la cuenta.
1. Busque **Administración de costos + facturación**.  
    ![Captura de pantalla que muestra la búsqueda.](./media/change-credit-card/search.png)
1. Seleccione la suscripción a la que quiere agregar la tarjeta de crédito.
1. Seleccione **Métodos de pago**.  
    ![Captura de pantalla que muestra la opción Administrar métodos de pago seleccionada](./media/change-credit-card/payment-methods-blade-x.png)
1. En la esquina superior izquierda, seleccione "+" para agregar una tarjeta. Aparecerá un formulario de tarjeta de crédito a la derecha.
1. Escriba los detalles de la tarjeta de crédito.  
    ![Captura de pantalla que muestra la adición de una nueva tarjeta](./media/change-credit-card/sub-add-new-x.png)
1. Para que esta tarjeta sea su forma de pago activa, active la casilla situada junto a **Convertir en mi método de pago activo** en la parte superior del formulario. Esta tarjeta se convertirá en el instrumento de pago activo para todas las suscripciones que utilicen la misma tarjeta que la suscripción seleccionada.
1. Seleccione **Next** (Siguiente).

### <a name="change-credit-card-for-a-subscription-to-a-previously-saved-credit-card"></a>Cambio de la tarjeta de crédito de una suscripción por una tarjeta de crédito que se ha guardado previamente

También puede cambiar la tarjeta de crédito predeterminada de la suscripción a una que ya esté guardada en la cuenta siguiendo estos pasos:

1. Inicie sesión en [Azure Portal](https://portal.azure.com) como administrador de la cuenta.
1. Busque **Administración de costos + facturación**.  
    ![Captura de pantalla que muestra la búsqueda.](./media/change-credit-card/search.png)
1. Seleccione la suscripción a la que quiere agregar la tarjeta de crédito.
1. Seleccione **Métodos de pago**.
    ![Captura de pantalla que muestra la opción Administrar métodos de pago seleccionada](./media/change-credit-card/payment-methods-blade-x.png)
1. Seleccione la casilla junto a la tarjeta que quiere convertir en el método de pago activo.
1. Seleccione **Establecer como activo**.
    ![Captura de pantalla que muestra la tarjeta de crédito seleccionada y establecida como activa](./media/change-credit-card/sub-change-active-x.png)

### <a name="edit-credit-card-details"></a>Edición de los detalles de la tarjeta de crédito

Si se renueva la tarjeta de crédito y el número sigue siendo el mismo, actualice la información de la tarjeta de crédito existente, como la fecha de expiración. Si su número de tarjeta de crédito cambia porque la tarjeta se pierda, se la roben o caduque, siga los pasos de la sección [Adición de una tarjeta de crédito como método de pago](#addcard). No es necesario actualizar el CVV.

1. Inicie sesión en [Azure Portal](https://portal.azure.com) como administrador de la cuenta.
1. Busque **Administración de costos + facturación**.
    ![Captura de pantalla que muestra la búsqueda.](./media/change-credit-card/search.png)
1. Seleccione **Métodos de pago**.
    ![Captura de pantalla que muestra la opción Administrar métodos de pago seleccionada](./media/change-credit-card/payment-methods-blade-x.png)
1. Seleccione la tarjeta de crédito que desea editar. Aparecerá un formulario de tarjeta de crédito a la derecha.
    ![Captura de pantalla que muestra la tarjeta de crédito seleccionada](./media/change-credit-card/edit-card-x.png)
1. Actualice los detalles de la tarjeta de crédito.
1. Seleccione **Guardar**.

## <a name="manage-credit-cards-for-a-microsoft-customer-agreement"></a>Administración de tarjetas de crédito para un Contrato de cliente de Microsoft

Las secciones siguientes se aplican a los clientes que tienen un Contrato de cliente de Microsoft y se han suscrito a Azure Online con una tarjeta de crédito. [Aprenda a comprobar si tiene un Contrato de cliente de Microsoft](#check-the-type-of-your-account).

### <a name="change-default-credit-card"></a>Cambio de la tarjeta de crédito predeterminada

Si tiene un Contrato de cliente de Microsoft, la tarjeta de crédito están asociada a un perfil de facturación. Para cambiar el método de pago de un perfil de facturación, debe ser la persona que se suscribió a Azure y que creó la cuenta de facturación.

Si quiere cambiar el método de pago predeterminado del perfil de facturación al pago mediante cheque o transferencia bancaria, consulte [Pago de las suscripciones de Azure con factura](pay-by-invoice.md).

Para cambiar la tarjeta de crédito, siga estos pasos:

1. Inicie sesión en [Azure Portal](https://portal.azure.com).
1. Busque en **Administración de costos + facturación**.
1. En el menú de la izquierda, seleccione **Perfiles de facturación**.
1. Seleccione un perfil de facturación.
1. En el menú de la izquierda, seleccione **Métodos de pago**.  
   ![Captura de pantalla que muestra los métodos de pago del menú](./media/change-credit-card/payment-methods-tab-mca.png)
1. En la sección **Método de pago predeterminado**, seleccione **Reemplazar**.  
    :::image type="content" source="./media/change-credit-card/change-payment-method-mca.png" alt-text="Captura de pantalla que muestra la opción de reemplazar" :::
1. En la nueva área de la derecha, seleccione una tarjeta existente en la lista desplegable o agregue una nueva seleccionando el vínculo azul **Add new payment method** (Agregar nuevo método de pago).

### <a name="edit-a-credit-card"></a>Edición de una tarjeta de crédito

Puede editar los detalles de la tarjeta de crédito (como actualizar la fecha de expiración) en Azure Portal. 

Para editar una tarjeta de crédito, siga estos pasos:

1. Inicie sesión en [Azure Portal](https://portal.azure.com).
1. Busque en **Administración de costos + facturación**.
1. En el menú de la izquierda, seleccione **Perfiles de facturación**.
1. Seleccione un perfil de facturación.
1. En el menú de la izquierda, seleccione **Métodos de pago**.  
   ![Captura de pantalla que muestra los métodos de pago del menú](./media/change-credit-card/payment-methods-tab-mca.png)
1. En la sección **Tarjetas de crédito del usuario**, busque la tarjeta que desea editar.
1. Seleccione los puntos suspensivos (`...`) al final de la fila.  
    :::image type="content" source="./media/change-credit-card/edit-delete-credit-card-mca.png" alt-text="Captura de pantalla que muestra la opción de reemplazar" :::
1. Para editar los detalles de la tarjeta de crédito, seleccione **Editar** en el menú contextual.

## <a name="troubleshooting"></a>Solución de problemas

Azure no admite tarjetas prepago ni virtuales. Si aparecen errores al agregar o actualizar una tarjeta de crédito válida, pruebe a abrir el explorador en modo privado.

## <a name="frequently-asked-questions"></a>Preguntas más frecuentes

Las siguientes secciones responden a las preguntas más frecuentes sobre cómo cambiar la información de la tarjeta de crédito.

### <a name="why-do-i-keep-getting-your-login-session-has-expired-please-click-here-to-log-back-in"></a>¿Por qué sigo recibiendo un mensaje similar a "Su sesión de inicio de sesión ha expirado. Haga clic aquí para volver a iniciar sesión"?

Si sigue apareciendo este mensaje de error incluso si ya cerró la sesión y volvió a iniciarla, inténtelo de nuevo con una sesión de exploración privada.

### <a name="how-do-i-use-a-different-card-for-each-subscription-i-have"></a>¿Cómo se usa una tarjeta diferente para cada suscripción que tengo?

Desgraciadamente, si sus suscripciones ya están usando la misma tarjeta, no es posible separarlas para utilizar varias tarjetas. Sin embargo, cuando se registra con una nueva suscripción, puede elegir usar un nuevo método de pago para esa suscripción.

### <a name="how-do-i-make-payments"></a>¿Cómo puedo realizar los pagos?

Si ha configurado una tarjeta de crédito como método de pago, se le cobra automáticamente en su tarjeta después de finalizar cada período de facturación. No es necesario hacer nada más.

Si desea [pagar con factura](pay-by-invoice.md), envíe el pago a la ubicación indicada en la parte inferior de la factura.

### <a name="how-do-i-change-the-tax-id"></a>¿Cómo se cambia el número de identificación tributaria?

Para agregar o actualizar el número de identificación fiscal, actualice el perfil en [Azure Portal](https://portal.azure.com) y seleccione **Registro fiscal**. Este número de identificación tributaria se utiliza para los cálculos de exención fiscal y aparece en la factura.

## <a name="check-the-type-of-your-account"></a>Comprobación del tipo de la cuenta

[!INCLUDE [billing-check-mca](../../../includes/billing-check-account-type.md)]

## <a name="need-help-contact-us"></a>¿Necesita ayuda? Póngase en contacto con nosotros.

Si tiene alguna pregunta o necesita ayuda, [cree una solicitud de soporte técnico](https://go.microsoft.com/fwlink/?linkid=2083458).

## <a name="next-steps"></a>Pasos siguientes

- Obtenga información sobre [reservas de Azure](../reservations/save-compute-costs-reservations.md) para ver si pueden hacerle ahorrar dinero.
- Si desea eliminar una tarjeta de crédito, consulte [Eliminación de un método de pago de la facturación de Azure (versión preliminar)](delete-azure-payment-method.md).