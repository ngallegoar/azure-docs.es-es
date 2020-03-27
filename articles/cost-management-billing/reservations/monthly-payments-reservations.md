---
title: Compra de reservas de Azure con pagos por adelantado o mensuales
description: Aprenda a comprar reservas de Azure con pagos por adelantado o mensuales.
author: bandersmsft
ms.reviewer: yashar
ms.service: cost-management-billing
ms.topic: conceptual
ms.date: 03/24/2020
ms.author: banders
ms.openlocfilehash: ede60adc13dadc38e18ee5ade468e01b16523f4f
ms.sourcegitcommit: 253d4c7ab41e4eb11cd9995190cd5536fcec5a3c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/25/2020
ms.locfileid: "80235739"
---
# <a name="purchase-reservations-with-monthly-payments"></a>Compra de reservas con pagos mensuales

Ahora puede pagar las reservas mensualmente. A diferencia de la compra por adelantado (importe total), la opción de pago mensual divide el costo de la reserva en partes iguales cada mes del período. El costo total de las reservas por adelantado y mensuales es el mismo y no se pagan cargos adicionales por elegir el pago mensual.

Si la reserva se adquiere mediante un contrato de cliente de Microsoft (MCA), el importe del pago mensual puede variar en función de la tasa de cambio actual del mercado correspondiente a su moneda local.

Los pagos mensuales están disponibles para: Databricks, reservas de SUSE Linux, planes de Red Hat y proceso de Azure Red Hat OpenShift.

Realice reservas en [Azure Portal](https://ms.portal.azure.com/#blade/Microsoft_Azure_Reservations/CreateBlade/referrer/Docs).

![Ejemplo que muestra una reserva](./media/monthly-payments-reservations/purchase-reservation.png)

Al realizar una reserva puede ver el calendario de pagos. Haga clic en **Ver el calendario de pagos completo**.

![Ejemplo que muestra el calendario de pagos de las reservas](./media/monthly-payments-reservations/prepurchase-schedule.png)

Para ver el calendario de pagos después de la reserva, selecciónela, y haga clic en el **id. de pedido de reserva** y en la pestaña **Pagos**.

## <a name="view-payments-made"></a>Visualización de los pagos realizados

Puede ver los pagos realizados mediante las API, los datos de uso y el análisis de costos. En el caso de las reservas de pago mensual, el valor de la frecuencia aparece como **periódico** en los datos de uso y en Reservation Charges API. En el caso de las reservas pagadas por adelantado, el valor se muestra como **OneTime**.

El análisis de costos muestra las compras mensuales en la vista predeterminada. Aplique el filtro **compra** en **Charge type** (Tipo de cargo) y **periódico** en **Frecuencia** para ver todas las compras. Para ver solo las reservas, aplique un filtro de **Reserva**.

![Ejemplo que muestra los costos de las reservas en el análisis de costos](./media/monthly-payments-reservations/cost-analysis.png)

## <a name="switch-to-monthly-payments-at-renewal"></a>Cambio al pago mensual en la renovación

Al renovar una reserva, puede cambiar la frecuencia de facturación a mensualmente.

## <a name="exchange-and-refunds"></a>Cambios y reembolsos

Al igual que otras reservas, con la facturación mensual se pueden reembolsar o cambiar. 

Al cambiar una reserva pagada mensualmente, el costo total de la nueva compra debe ser mayor que los pagos restantes que se cancelan de la reserva devuelta. No hay ningún otro límite ni tarifa por los cambios. Puede cambiar una reserva pagada por adelantado para comprar una nueva que se facture mensualmente. Sin embargo, el valor de la duración de la nueva reserva debe ser mayor que el prorrateado de la reserva que se va a devolver.

Si cancela una reserva que se paga mensualmente, los pagos futuros cancelados se acumularán hasta el límite de reembolso de 50 000 USD.

Para más información sobre los cambios y los reembolsos, consulte [Autoservicio de cambios y reembolsos de reservas de Azure](exchange-and-refund-azure-reservations.md).

## <a name="next-steps"></a>Pasos siguientes

- Para más información sobre las reservas, consulte [¿Qué es Azure Reservations?](save-compute-costs-reservations.md)
- Para información sobre las tareas que debe realizar antes de adquirir una reserva, consulte [Determinación del tamaño adecuado de una máquina virtual antes de su adquisición](../../virtual-machines/windows/prepay-reserved-vm-instances.md#determine-the-right-vm-size-before-you-buy).
