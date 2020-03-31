---
title: Ahorro de costos
description: Aprenda a comprar capacidad reservada de Azure SQL Database para ahorrar en los costos de proceso.
services: sql-database
ms.service: sql-database
ms.subservice: service
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: anosov1960
ms.author: sashan
ms.reviewer: sstein, carlrab
ms.date: 08/29/2019
ms.openlocfilehash: 0713e1ed98cc87b95cad1f84672148cd062e0b58
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "75979989"
---
# <a name="save-costs-for-sql-database-compute-resources-with-azure-sql-database-reserved-capacity"></a>Ahorro de costos de los recursos de proceso de SQL Database con capacidad reservada de Azure SQL Database

Ahorre dinero con Azure SQL Database al comprometerse a una reserva de recursos de proceso en lugar de elegir el pago por uso. Con la capacidad reservada, se compromete a usar Azure SQL Database durante un período de uno a tres años para obtener un descuento considerable en los costos de proceso. Para comprar capacidad reservada de SQL Database, debe especificar la región de Azure, el tipo de implementación, el nivel de servicio y el período.


No es necesario asignar la reserva a instancias específicas de SQL Database (bases de datos únicas, grupos elásticos o instancias administradas). Al igualarse las instancias de SQL Database en ejecución o las recién implementadas se obtendrá la ventaja automáticamente. Al comprar una reserva, se compromete a usar el servicio por los costos de proceso durante un período de uno a tres años. En cuando se compra una reserva, los costos de proceso de SQL Database que coincidan con los atributos de reserva dejan de pagarse según las tarifas de pago por uso. La reserva no cubre los cargos por software, redes o almacenamiento asociados a la instancia de SQL Database. Al final del plazo de reserva, la ventaja en la facturación expira y las instancias de SQL Database se facturan según los precios de pago por uso. Las reservas no se renuevan automáticamente. Para información sobre precios, consulte el artículo sobre la [oferta de capacidad reservada de SQL Database](https://azure.microsoft.com/pricing/details/sql-database/managed/).

Puede comprar capacidad reservada de Azure SQL Database en [Azure Portal](https://portal.azure.com). Pague la reserva [por adelantado o mensualmente](../cost-management-billing/reservations/monthly-payments-reservations.md). Para comprar capacidad reservada de SQL Database:

- Debe tener el rol de propietario al menos en una suscripción Enterprise o individual con tarifas de pago por uso.
- En el caso de las suscripciones Enterprise, la opción **Agregar instancias reservadas** debe estar habilitada en el [portal de EA](https://ea.azure.com). O bien, si esa opción está deshabilitada, debe ser un administrador de EA en la suscripción.
- En el caso del programa del Proveedor de soluciones en la nube (CSP), los únicos que pueden comprar la capacidad reservada de SQL Database son los agentes de administración o de ventas.

Para información sobre cómo se les cobra a los clientes de empresa y a los de pago por uso las compras de reservas, consulte [Información sobre el uso de reservas de Azure para la inscripción Enterprise](../cost-management-billing/reservations/understand-reserved-instance-usage-ea.md) y [Información sobre el uso de reservas de Azure para suscripciones de pago por uso](../cost-management-billing/reservations/understand-reserved-instance-usage.md).

## <a name="determine-the-right-sql-size-before-purchase"></a>Determinación del tamaño adecuado de SQL antes de la compra

El tamaño de la reserva debe basarse en el proceso total de las bases de datos únicas existentes o que se van a implementar, de los grupos elásticos o de instancias administradas de una región específica y con el mismo nivel de rendimiento y la misma generación de hardware.

Por ejemplo, supongamos que está ejecutando un grupo elástico de propósito general Gen5 de 16 núcleos virtuales y dos bases de datos únicas Gen5 de 4 núcleos virtuales críticas para la empresa. Además, supongamos que planea implementar en el próximo mes un grupo elástico Gen5 de propósito general y 16 núcleos virtuales adicional, y otro de 32 núcleos virtuales crítico para la empresa. Y también supongamos que sabe que necesitará estos recursos durante al menos 1 año. En este caso debe comprar una reserva de 1 año de una instancia de Gen5 con 32 núcleos virtuales (2 × 16) para una base de datos única y un grupo elástico de uso general y otra reserva de 1 año de una instancia de Gen 5 con 40 núcleos virtuales (2 × 4 + 32) para una base de datos única y un grupo elástico críticos para la empresa.

## <a name="buy-sql-database-reserved-capacity"></a>Compra de capacidad reservada de SQL Database

1. Inicie sesión en [Azure Portal](https://portal.azure.com).
2. Seleccione **Todos los servicios** > **Reservations**.
3. Seleccione **Agregar** y, en el panel Comprar reservas, seleccione **SQL Database** para comprar una nueva reserva para SQL Database.
4. Rellene todos los campos obligatorios. Las bases de datos únicas, los grupos elásticos o las instancias administradas nuevas que coincidan con los atributos seleccionados serán aptos para el descuento en la capacidad reservada. El número real de instancias de SQL Database que obtienen el descuento depende el ámbito y la cantidad seleccionados.
    ![Captura de pantalla antes del envío de la compra de la capacidad reservada de SQL Database](./media/sql-database-reserved-vcores/sql-reserved-vcores-purchase.png)

En la siguiente tabla se describen los campos obligatorios.

| Campo      | Descripción|
|------------|--------------|
|Subscription|La usada para pagar la reserva de capacidad reservada de SQL Database. Los costos anticipados por la capacidad reservada de SQL Database se cobran mediante el método de pago de la suscripción. El tipo de suscripción debe ser Contrato Enterprise (números de oferta: MS-AZR-0017P o MS-AZR-0148P) o un contrato individual con precios de pago por uso (números de oferta: MS-AZR-0003P o MS-AZR-0023P). Para una suscripción Enterprise, los cargos se deducen del saldo de compromiso monetario de la inscripción o se cobran como uso por encima del límite. Para una suscripción individual con precios de pago por uso, los cargos se cobran en el método de pago de la factura o la tarjeta de crédito de la suscripción.|
|Ámbito       |El ámbito de la reserva de núcleos virtuales puede cubrir una suscripción o varias (ámbito compartido). Si selecciona: <br/><br/>**Compartido**: el descuento por la reserva de núcleos virtuales se aplica a las instancias de SQL Database en ejecución en cualquiera de las suscripciones en el contexto de facturación. Para los clientes Enterprise, el ámbito compartido es la inscripción e incluye todas las suscripciones que esta contiene. Para los clientes de Pago por uso, el ámbito compartido incluye todas las suscripciones de Pago por uso creadas por el administrador de la cuenta.<br/><br/>**Suscripción única**: el descuento por la reserva de núcleos virtuales se aplica a las instancias de SQL Database de esta suscripción. <br/><br/>**Grupo de recursos único**: el descuento de reserva se aplica a las instancias de SQL Database de la suscripción seleccionada y al grupo de recursos seleccionado de esa suscripción.|
|Region      |La región de Azure que abarca la capacidad reservada de SQL Database.|
|Tipo de implementación|El tipo de recurso de SQL para el que desea adquirir la reserva.|
|Nivel de rendimiento|El nivel de servicio de las instancias de SQL Database.
|Término        |Un año o tres años.|
|Cantidad    |Número de recursos de proceso que se compran dentro de la reserva de capacidad reservada de SQL Database. La cantidad es un número de núcleos virtuales de la región de Azure y el nivel de rendimiento seleccionados que se están reservando y obtendrán el descuento de facturación. Por ejemplo, si ejecuta o planea ejecutar instancias de SQL Database con la capacidad total de proceso de 16 núcleos virtuales Gen5 en la región Este de EE. UU., deberá especificar la cantidad como 16 para maximizar las ventajas de todas las instancias. |

1. Revise el costo de la reserva de capacidad reservada de SQL Database en la sección **Costos**.
1. Seleccione **Comprar**.
1. Haga clic en **Ver esta reserva** para conocer el estado de la compra.

## <a name="cancel-exchange-or-refund-reservations"></a>Cancelación, intercambio o reembolso de reservas

Puede cancelar, intercambiar o reembolsar reservas con ciertas limitaciones. Para más información, consulte [Autoservicio de intercambios y reembolsos de reservas de Azure](../cost-management-billing/reservations/exchange-and-refund-azure-reservations.md).

## <a name="vcore-size-flexibility"></a>Flexibilidad de tamaño del núcleo virtual

La flexibilidad de tamaño del núcleo virtual le ayuda a escalar o reducir verticalmente dentro de un nivel de rendimiento y una región, sin perder los beneficios de la capacidad reservada. La capacidad reservada de SQL Database también le proporciona la flexibilidad para mover temporalmente las bases de datos de acceso frecuente entre grupos y bases de datos únicas como parte de las operaciones normales (dentro de la misma región y el mismo nivel de rendimiento) sin perder las ventajas de la capacidad reservada. Si se mantiene un búfer no aplicado en la reserva, puede administrar de manera eficaz los picos de rendimiento sin sobrepasar el presupuesto.

## <a name="limitation"></a>Limitación

No se pueden reservar bases de datos SQL basadas en DTU (básicas, estándar o prémium).

## <a name="need-help-contact-us"></a>¿Necesita ayuda? Ponerse en contacto con nosotros

Si tiene alguna pregunta o necesita ayuda, [cree una solicitud de soporte técnico](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest).

## <a name="next-steps"></a>Pasos siguientes

El descuento por la reserva de núcleos virtuales se aplica automáticamente al número de instancias de SQL Database que coincidan con el ámbito y los atributos de la capacidad reservada de SQL Database. Puede actualizar el ámbito de la capacidad reservada de SQL Database a través de [Azure Portal](https://portal.azure.com), PowerShell, la CLI o la API.

Para saber cómo administrar la capacidad reservada de SQL Database, consulte el artículo sobre la [administración de capacidad reservada de SQL Database](../cost-management-billing/reservations/manage-reserved-vm-instance.md).

Para obtener más información acerca de Azure Reservations, consulte los siguientes artículos:

- [¿Qué es Azure Reservations?](../cost-management-billing/reservations/save-compute-costs-reservations.md)
- [Administración de Azure Reservations](../cost-management-billing/reservations/manage-reserved-vm-instance.md)
- [Información sobre el descuento de Azure Reservations](../cost-management-billing/reservations/understand-reservation-charges.md)
- [Información sobre el uso de reservas para suscripciones de pago por uso](../cost-management-billing/reservations/understand-reserved-instance-usage.md)
- [Información sobre el uso de reservas para la inscripción Enterprise](../cost-management-billing/reservations/understand-reserved-instance-usage-ea.md)
- [Azure Reservations en el programa del Proveedor de soluciones en la nube (CSP) del Centro de partners](https://docs.microsoft.com/partner-center/azure-reservations)
