---
title: Solución de problemas de suscripciones no aptas en Azure Portal
description: Este artículo le ayuda a solucionar los problemas del mensaje de error "No hay suscripciones aptas" de Azure Portal al intentar comprar una reserva.
author: bandersmsft
ms.service: cost-management-billing
ms.subservice: reservations
ms.author: banders
ms.reviewer: yashar
ms.topic: troubleshooting
ms.date: 10/14/2020
ms.openlocfilehash: fd7a2bde47f34a61390082a223409070275b64ce
ms.sourcegitcommit: ae6e7057a00d95ed7b828fc8846e3a6281859d40
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/16/2020
ms.locfileid: "92115233"
---
# <a name="troubleshoot-no-eligible-subscriptions"></a>Solución de problemas de suscripciones no aptas

Este artículo le ayuda a solucionar los problemas del mensaje de error *No hay suscripciones aptas* de Azure Portal al intentar comprar una reserva.

## <a name="symptoms"></a>Síntomas

1. Inicie sesión en [Azure Portal](https://portal.azure.com) y vaya a **Reservas** .
1. Seleccione **Agregar** y, luego, elija un servicio.
1. Se muestra el siguiente mensaje de error:
   ```
    No eligible subscriptions
    
    You do not have any eligible subscriptions to purchase reservations. To purchase a reservation, you should be an owner on at least one subscription of the following type: Pay-as-you-go, CSP, Microsoft Enterprise or Microsoft Customer Agreement.
    ```
1. En el área **Seleccione el producto que quiere comprar** , expanda la lista **Suscripción de facturación** para ver el motivo por el que una suscripción específica no es apta para comprar una instancia reservada. En la imagen siguiente se muestran ejemplos de por qué no se puede comprar una reserva.  
    :::image type="content" source="./media/troubleshoot-no-eligible-subscriptions/select-product-to-purchase.png" alt-text="Ejemplo que muestra por qué no se puede comprar una reserva" lightbox="./media/troubleshoot-no-eligible-subscriptions/select-product-to-purchase.png" :::

## <a name="cause"></a>Causa

Para comprar una instancia reservada de Azure, debe tener al menos una suscripción que cumpla los requisitos siguientes:

- La suscripción debe ser un tipo de oferta admitido. Los tipos de oferta admitidos son: Pago por uso, Proveedor de soluciones en la nube (CSP), Microsoft Azure Enterprise o Contrato de cliente de Microsoft.
- El usuario debe ser propietario de la suscripción.

Si no tiene una suscripción que cumpla los requisitos, recibirá el error `No eligible subscriptions`.

### <a name="cause-1"></a>Causa 1

La suscripción debe ser un tipo de oferta admitido. Los tipos de oferta admitidos son: Pago por uso, CSP, Microsoft Azure Enterprise o Contrato de cliente de Microsoft. El tipo de suscripción no pertenece a uno de los admitidos. Al seleccionar una suscripción que tiene un tipo de oferta que no admite reservas, verá el siguiente error.

```
Subscription not eligible for purchase

This subscription is not eligible for reservation benefit an cannot be used to purchase a reservation.
```

:::image type="content" source="./media/troubleshoot-no-eligible-subscriptions/subscription-not-eligible.png" alt-text="Ejemplo que muestra por qué no se puede comprar una reserva" :::

### <a name="cause-2"></a>Causa 2

El usuario debe ser propietario de la suscripción. El usuario no es propietario de la suscripción. Al seleccionar una suscripción de la que no es propietario, verá el siguiente error.

```
You do not have owner access on the subscription

You can only purchase reservations using subscriptions on which you have owner access.
```

:::image type="content" source="./media/troubleshoot-no-eligible-subscriptions/no-owner-access.png" alt-text="Ejemplo que muestra por qué no se puede comprar una reserva" :::

## <a name="solution"></a>Solución

- Si la oferta actual no admite reservas, debe crear otra suscripción de Azure.
- Si no tiene acceso a una reserva existente, puede obtener acceso a ella a través del propietario actual.

### <a name="solution-1"></a>Solución 1

Para comprar una reserva, debe crear una suscripción de Azure que admita reservas.

- Si tiene una evaluación gratuita de Azure, puede [actualizar la suscripción](../manage/upgrade-azure-subscription.md).
- Puede crear una suscripción de Azure con [tarifas de pago por uso](https://azure.microsoft.com/pricing/purchase-options/pay-as-you-go/).
- Regístrese con un [contrato de cliente de Microsoft](https://azure.microsoft.com/pricing/purchase-options/microsoft-customer-agreement/) y cree una suscripción.
- Compre una nueva suscripción con un [CSP](https://www.microsoft.com/solution-providers/home) y cree una suscripción.

### <a name="solution-2"></a>Solución 2

Para obtener acceso de propietario a una reserva, debe obtener acceso a:

- La orden de reserva con la que se compró la reserva
- La propia reserva

El propietario actual de la orden de reserva o de la reserva puede delegarle el acceso mediante estos pasos.

1. Inicie sesión en [Azure Portal](https://portal.azure.com).
1. Seleccione **Todos los servicios** > **Reservations** para enumerar las reservas a las que tiene acceso.
1. Seleccione la reserva cuyo acceso quiere delegar a otros usuarios.
1. Seleccione **Access Control (IAM)** .
1. Seleccione **Agregar asignación de roles** > **Rol** > **Propietario** . U otro rol si quiere conceder acceso limitado.
1. Escriba la dirección de correo electrónico del usuario al que desea agregar como propietario.
1. Seleccione el usuario y, después, **Guardar** .

Para más información, consulte [Agregar o cambiar los usuarios que pueden administrar una reserva](manage-reserved-vm-instance.md#add-or-change-users-who-can-manage-a-reservation).

## <a name="next-steps"></a>Pasos siguientes

- Revise [Agregar o cambiar los usuarios que pueden administrar una reserva](manage-reserved-vm-instance.md#add-or-change-users-who-can-manage-a-reservation) si necesita tener un propietario de reserva que le conceda acceso.