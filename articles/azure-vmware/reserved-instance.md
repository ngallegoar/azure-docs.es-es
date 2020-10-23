---
title: Ahorro de costos con la instancia reservada de Azure VMware Solution
description: Obtenga información sobre cómo comprar una instancia reservada para Azure VMware Solution.
ms.topic: how-to
ms.date: 10/02/2020
ms.openlocfilehash: fa354d4fbfef868ea1e6783656be7871669f200d
ms.sourcegitcommit: a2d8acc1b0bf4fba90bfed9241b299dc35753ee6
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/12/2020
ms.locfileid: "91951424"
---
# <a name="save-costs-with-azure-vmware-solution"></a>Ahorro de costos con Azure VMware Solution

Al confirmar una instancia reservada de Azure VMware Solution, puede ahorrar dinero. El descuento de la reserva se aplica automáticamente al número de hosts de Azure VMware Solution en ejecución que coinciden con el ámbito y los atributos de la reserva. No es necesario asignar una reserva a un host dedicado para obtener los descuentos. La compra de una instancia reservada cubre solo la parte de proceso de su uso e incluye los costos de licencia de software. Consulte la  [información general sobre Azure VMware Solution](introduction.md).

## <a name="purchase-restriction-considerations"></a>Consideraciones sobre restricciones de compra

Las instancias reservadas están disponibles con algunas excepciones:

-   **Nubes**: las reservas solo están disponibles en las regiones enumeradas en la página [Productos disponibles por región](https://azure.microsoft.com/global-infrastructure/services/?products=azure-vmware).

-   **Cuota insuficiente**: una reserva cuyo ámbito sea de una suscripción única o compartida debe tener cuota de hosts disponible en la suscripción para la nueva instancia reservada. Puede  [cursar una solicitud de aumento de la cuota](enable-azure-vmware-solution.md) para resolver este problema.

-   **Idoneidad de la oferta**: necesitará un  [Contrato Enterprise (EA) de Azure](../cost-management-billing/manage/ea-portal-agreements.md) con Microsoft.

-   **Restricciones de capacidad**: en algunas circunstancias poco frecuentes, Azure limita la compra de nuevas reservas a los SKU de hosts de Azure VMware Solution debido a que la capacidad en una región es baja.

## <a name="buy-a-reservation"></a>Adquisición de una reserva

Puede comprar una instancia reservada de una instancia de host de Azure VMware Solution en  [Azure Portal](https://portal.azure.com/#blade/Microsoft_Azure_Reservations/CreateBlade/referrer/documentation/filters/%7B%22reservedResourceType%22%3A%22VirtualMachines%22%7D).

Pague la reserva [ por adelantado o de manera mensual](../cost-management-billing/reservations/prepare-buy-reservation.md).

Estos requisitos se aplican a la compra de una instancia reservada de host dedicado:

-   Debe tener un rol de propietario en al menos una suscripción de EA o en una suscripción con una tarifa de pago por uso.

-   En el caso de las suscripciones de EA, la opción  **Agregar instancias reservadas** debe estar habilitada en el  [portal de EA](https://ea.azure.com/). O bien, si esa opción está deshabilitada, debe ser un administrador de EA en la suscripción.

Para comprar una instancia:

1. Inicie sesión en  [Azure Portal](https://portal.azure.com/).

2. Seleccione  **Todos los servicios** > **Reservas**.

3. Seleccione **Agregar** para comprar una nueva reserva y, luego, haga clic en**Azure VMware Solution**.

4. Rellene los campos obligatorios. Los hosts de Azure VMware Solution en ejecución que coinciden con los atributos seleccionados cumplen los requisitos para obtener el descuento de la reserva. El número real de hosts de Azure VMware Solution que obtienen el descuento depende del ámbito y la cantidad que se seleccionen.

   Si tiene un Contrato Enterprise, puede usar la opción  **Agregar más** para agregar rápidamente instancias adicionales. La opción no está disponible para otros tipos de suscripciones.

   | Campo        |  Descripción |
   | ------------ | ------------ |
   | Subscription | Suscripción que se usa para pagar la reserva. Los costos de la reserva se cobran en el método de pago de la suscripción. El tipo de suscripción debe ser Contrato Enterprise (números de oferta: MS-AZR-0017P o MS-AZR-0148P) o Contrato de cliente de Microsoft o una suscripción individual con tarifas de pago por uso (números de oferta: MS-AZR-0003P o MS-AZR-0023P). Los cargos se deducen del saldo de compromiso monetario, si está disponible, o se cobran como uso por encima del límite. En una suscripción con tarifas de pago por uso, los cargos se cobran con el método de pago de factura o la tarjeta de crédito de la suscripción. |
   | Ámbito        | El ámbito de la reserva puede cubrir una o varias suscripciones (ámbito compartido). Si selecciona:<br><ul><li><b>Single resource group scope (Ámbito de grupo de recursos único): el descuento por reserva se aplica a los recursos coincidentes solo del grupo de recursos seleccionado.</li><li><b>Single subscription scope (Ámbito de suscripción única): el descuento por reserva se aplica a los recursos coincidentes de la suscripción seleccionada.</li><li><b>Ámbito compartido: el descuento por reserva se aplica a los recursos coincidentes en suscripciones válidas que están en el contexto de facturación. Para los clientes de EA, el contexto de facturación es la inscripción. En el caso de suscripciones individuales con tarifas de pago por uso, el ámbito de facturación son todas las suscripciones aptas creadas por el administrador de la cuenta.</li></ul>       |
   | Region       | Región de Azure que está cubierta por la reserva.   |
   | Tamaño de host    | AV36    |
   | Término         | Un año o tres años.  |
   | Cantidad     | Número de instancias que se compran dentro de la reserva. La cantidad es el número de hosts de Azure VMware Solution en ejecución a los que se aplica el descuento de facturación.    |

## <a name="usage-data-and-reservation-utilization"></a>Datos de uso y utilización de la reserva

Los datos de uso que obtienen un descuento de reserva tienen un precio efectivo de cero. Puede ver qué instancia de Azure VMware Solution recibió el descuento de reserva para cada reserva.

Para obtener más información sobre cómo aparecen los descuentos de reserva en los datos de uso, y si es un cliente de EA, consulte  [Obtención del uso y los costos de reservas de Contrato Enterprise](../cost-management-billing/reservations/understand-reserved-instance-usage-ea.md). Si tiene una suscripción individual, consulte  [Descripción del uso de reservas para su suscripción individual de pago por uso](../cost-management-billing/reservations/understand-reserved-instance-usage.md).

## <a name="change-a-reservation-after-purchase"></a>Cambiar una reserva después de la compra

Puede realizar los siguientes tipos de cambios en una reserva después de haberla comprado:

-   Actualización del ámbito de la reserva

-   Flexibilidad del tamaño de instancia (si procede)

-   Propiedad

También puede dividir una reserva en fragmentos más pequeños y combinar reservas. Ninguno de estos cambios conlleva una nueva transacción comercial ni un cambio en la fecha de finalización de la reserva.

>[!NOTE]
>Una vez que haya adquirido la reserva, no podrá realizar los siguientes tipos de cambios directamente:
>
> - La región de una reserva existente
> - SKU
> - Cantidad
> - Duration
>
>Pero sí se puede  *intercambiar* una reserva si quiere realizar cambios.

## <a name="cancel-exchange-or-refund-reservations"></a>Cancelación, intercambio o reembolso de reservas

Puede cancelar, intercambiar o reembolsar reservas con ciertas limitaciones. Para más información, consulte  [Autoservicio de intercambios y reembolsos de reservas de Azure](../cost-management-billing/reservations/exchange-and-refund-azure-reservations.md).