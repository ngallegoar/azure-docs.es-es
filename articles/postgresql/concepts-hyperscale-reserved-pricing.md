---
title: Precios de procesos reservados de Hiperescala (Citus) para Azure Database for PostgreSQL
description: Pago por adelantado de recursos de proceso de Hiperescala (Citus) para Azure Database for PostgreSQL con capacidad reservada
author: jonels-msft
ms.author: jonels
ms.service: postgresql
ms.subservice: hyperscale-citus
ms.topic: conceptual
ms.date: 06/15/2020
ms.openlocfilehash: dcf38a3bcd41ba41bb7cf3b16d022a7a45734c4a
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/02/2020
ms.locfileid: "85217971"
---
# <a name="prepay-for-azure-database-for-postgresql---hyperscale-citus-compute-resources-with-reserved-capacity"></a>Pago por adelantado de recursos de proceso de Hiperescala (Citus) para Azure Database for PostgreSQL con capacidad reservada

Ahora Hiperescala (Citus) para Azure Database for PostgreSQL le ayuda a ahorrar mediante el pago por adelantado de los recursos de proceso en comparación con los precios de pago por uso. Con la capacidad reservada de Hiperescala (Citus), se realiza un compromiso inicial en el grupo de servidores de Hiperescala (Citus) durante un periodo de uno o tres años con el fin de obtener un descuento importante en los costos de proceso. Para comprar capacidad reservada de Hiperescala (Citus), debe especificar la región de Azure, el plazo de reserva y la frecuencia de facturación.

> [!IMPORTANT]
> Esta página trata sobre la capacidad reservada para Hiperescala (Citus) de Azure Database for PostgreSQL. Consulte [esta página](/azure/postgresql/concept-reserved-pricing) para obtener información sobre la capacidad reservada de un servidor único de Azure Database for PostgreSQL.

No es necesario asignar la reserva a grupos de servidores específicos de Hiperescala (Citus). Un grupo de servidores de Hiperescala (Citus) que ya está en ejecución, o los que se han implementado recientemente, obtendrán de forma automática la ventaja de los precios reservados. Al comprar una reserva, se adelanta el pago de los costos de proceso durante un período de uno a tres años. Tan pronto como se compra una reserva, los costos de proceso de Hiperescala (Citus) que coinciden con los atributos de la reserva dejan de pagarse según las tarifas de pago por uso. La reserva no cubre los cargos por software, redes o almacenamiento asociados a los grupos de servidor de Hiperescala (Citus). Al final del plazo de reserva, la ventaja en la facturación expira y los grupos de servidor de Hiperescala (Citus) se facturan según los precios de pago por uso. Las reservas no se renuevan automáticamente. Para obtener información sobre precios, consulte [Oferta de capacidad reservada de Hiperescala (Citus) para Azure Database for PostgreSQL](https://azure.microsoft.com/pricing/details/postgresql/hyperscale-citus/).

Puede comprar la capacidad reservada de Hiperescala (Citus) en [Azure Portal](https://portal.azure.com/). Pague la reserva [por adelantado o mensualmente](https://docs.microsoft.com/azure/cost-management-billing/reservations/monthly-payments-reservations). Para comprar capacidad reservada:

* Debe tener el rol de propietario al menos en una suscripción Enterprise o individual con tarifas de pago por uso.
* En el caso de las suscripciones Enterprise, la opción **Agregar instancias reservadas** debe estar habilitada en el [portal de EA](https://ea.azure.com/). O bien, si esa opción está deshabilitada, debe ser un administrador de EA en la suscripción.
* En el caso del programa del Proveedor de soluciones en la nube (CSP), los únicos que pueden comprar la capacidad reservada de Hiperescala (Citus) son los agentes de administración o de ventas.

Si quiere conocer detalles sobre cómo se les cobra a los clientes de empresa y a los de pago por uso las compras de reservas, consulte [Información sobre el uso de reservas de Azure para la inscripción Enterprise](https://docs.microsoft.com/azure/billing/billing-understand-reserved-instance-usage-ea) e [Información sobre el uso de reservas de Azure para suscripciones de pago por uso](https://docs.microsoft.com/azure/billing/billing-understand-reserved-instance-usage).

## <a name="determine-the-right-server-group-size-before-purchase"></a>Determinación del tamaño de grupo de servidor adecuado antes de la compra

El tamaño de la reserva se debe basar en la cantidad total de proceso que van a usar los nodos de trabajo y coordinación existentes o que se van a implementar pronto en los grupos de servidores de Hiperescala (Citus) de una región específica.

Por ejemplo, supongamos que está ejecutando un grupo de servidores de Hiperescala (Citus) con un nodo de coordinación de 16 núcleos virtuales y tres nodos de trabajo de 8 núcleos virtuales. Además, supongamos que planea implementar en el siguiente mes un grupo de servidores de Hiperescala (Citus) con un nodo de coordinación de 32 núcleos virtuales y dos nodos de trabajo de 4 núcleos virtuales. Vamos a suponer que necesitará estos recursos durante al menos un año.

En este caso, debe adquirir la reserva de un año para lo siguiente

* Total de 16 núcleos virtuales + 32 núcleos virtuales = 48 núcleos virtuales para nodos de coordinación
* Total de tres nodos x 8 núcleos virtuales + 2 nodos x 4 núcleos virtuales = 24 + 8 = 32 núcleos virtuales para nodos de trabajo

## <a name="buy-azure-database-for-postgresql-reserved-capacity"></a>Compra de capacidad reservada de Azure Database for PostgreSQL

1. Inicie sesión en [Azure Portal](https://portal.azure.com/).
2. Seleccione **Todos los servicios** > **Reservations**.
3. Seleccione **Agregar** y, en el panel Comprar reservas, seleccione **Azure Database for PostgreSQL** para comprar una nueva reserva para las bases de datos de PostgreSQL.
4. Seleccione el tipo de proceso de Hiperescala (Citus) y haga clic en el botón **Seleccionar**.
5. Revise la cantidad del tipo de proceso seleccionado en la pestaña **Productos**.
4. Vaya a la pestaña **Comprar y revisar** para completar la compra.

En la siguiente tabla se describen los campos obligatorios.

| Campo        | Descripción                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                        |
|--------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Suscripción | Suscripción usada para pagar la reserva de capacidad reservada de Azure Database for PostgreSQL. Los costos anticipados por la reserva de capacidad reservada de Azure Database for PostgreSQL se cobran mediante el método de pago de la suscripción. El tipo de suscripción debe ser Contrato Enterprise (números de oferta: MS-AZR-0017P o MS-AZR-0148P) o un contrato individual con precios de pago por uso (números de oferta: MS-AZR-0003P o MS-AZR-0023P). Para una suscripción Enterprise, los cargos se deducen del saldo de compromiso monetario de la inscripción o se cobran como uso por encima del límite. Para una suscripción individual con precios de pago por uso, los cargos se cobran en el método de pago de la factura o la tarjeta de crédito de la suscripción.                                                                                  |
| Ámbito        | El ámbito de la reserva de núcleos virtuales puede cubrir una suscripción o varias (ámbito compartido). Si selecciona: <br><br> **Compartido**: el descuento por la reserva de núcleos virtuales se aplica a los grupos de servidores de Hiperescala (Citus) en cualquiera de las suscripciones en el contexto de facturación. Para los clientes Enterprise, el ámbito compartido es la inscripción e incluye todas las suscripciones que esta contiene.  Para los clientes de Pago por uso, el ámbito compartido incluye todas las suscripciones de Pago por uso creadas por el administrador de la cuenta. <br><br> **Suscripción única**: el descuento por la reserva de núcleos virtuales se aplica a los grupos de servidores de Hiperescala (Citus) de esta suscripción. <br><br> **Grupo de recursos único**: el descuento de reserva se aplica a los grupos de servidores de Hiperescala (Citus) de la suscripción seleccionada y al grupo de recursos seleccionado de esa suscripción. |
| Region       | La región de Azure que abarca la reserva de capacidad reservada de Hiperescala (Citus) para Azure Database for PostgreSQL.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                           |
| Término         | Un año o tres años.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                           |
| Cantidad     | Número de recursos de proceso que se compran dentro de la reserva de capacidad reservada de Hiperescala (Citus). En concreto, el número de núcleos virtuales de los nodos de trabajo o coordinación en la región de Azure seleccionada que se están reservando y obtendrán el descuento de facturación. Por ejemplo, si ejecuta (o planea ejecutar) los grupos de servidores de Hiperescala (Citus) con la capacidad de proceso total de 64 núcleos virtuales de nodo de coordinación y 32 núcleos virtuales de nodo de trabajo en la región Este de EE. UU., debe especificar la cantidad como 64 y 32 para los nodos de coordinación y de trabajo, respectivamente, para maximizar el beneficio de todos los servidores.                                                                                                                                                                                                                                                     |



## <a name="cancel-exchange-or-refund-reservations"></a>Cancelación, intercambio o reembolso de reservas

Puede cancelar, intercambiar o reembolsar reservas con ciertas limitaciones. Para más información, consulte [Autoservicio de intercambios y reembolsos de reservas de Azure](https://docs.microsoft.com/azure/billing/billing-azure-reservations-self-service-exchange-and-refund).

## <a name="vcore-size-flexibility"></a>Flexibilidad de tamaño del núcleo virtual

La flexibilidad de tamaño del núcleo virtual le ayuda a escalar o reducir verticalmente los nodos de trabajo y coordinación dentro de una región, sin perder los beneficios de la capacidad reservada.

## <a name="need-help-contact-us"></a>¿Necesita ayuda? Ponerse en contacto con nosotros

Si tiene alguna pregunta o necesita ayuda, [cree una solicitud de soporte técnico](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest).

## <a name="next-steps"></a>Pasos siguientes

El descuento por la reserva de núcleos virtuales se aplica automáticamente al número de grupos de servidores de Hiperescala (Citus) que coinciden con el ámbito y los atributos de la reserva de capacidad reservada de Azure Database for PostgreSQL. Se puede actualizar el ámbito de la reserva de capacidad reservada de Hiperescala (Citus) para Azure Database for PostgreSQL a través de Azure Portal, PowerShell, la CLI o la API.

Para obtener más información acerca de Azure Reservations, consulte los siguientes artículos:

* [¿Qué es Azure Reservations?](https://docs.microsoft.com/azure/billing/billing-save-compute-costs-reservations)
* [Administración de Azure Reservations](https://docs.microsoft.com/azure/billing/billing-manage-reserved-vm-instance)
* [Información sobre el descuento de Azure Reservations](https://docs.microsoft.com/azure/billing/billing-understand-reservation-charges)
* [Información sobre el uso de reservas para suscripciones de pago por uso](https://docs.microsoft.com/azure/billing/billing-understand-reservation-charges-postgresql)
* [Información sobre el uso de reservas para la inscripción Enterprise](https://docs.microsoft.com/azure/billing/billing-understand-reserved-instance-usage-ea)
* [Azure Reservations en el programa del Proveedor de soluciones en la nube (CSP) del Centro de partners](https://docs.microsoft.com/partner-center/azure-reservations)
