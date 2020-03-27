---
title: ¿Qué es Azure Reservations?
description: Obtenga información sobre Azure Reservations y precios para ahorrar en máquinas virtuales, SQL Database, Azure Cosmos DB y otros costos de recursos.
author: yashesvi
ms.reviewer: yashar
ms.service: cost-management-billing
ms.topic: conceptual
ms.date: 03/24/2020
ms.author: banders
ms.openlocfilehash: 6277a7e7dc5891a3bc67c298a31344284c92e31d
ms.sourcegitcommit: 253d4c7ab41e4eb11cd9995190cd5536fcec5a3c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/25/2020
ms.locfileid: "80235644"
---
# <a name="what-are-azure-reservations"></a>¿Qué es Azure Reservations?

Las reservas de Azure le ayudan a ahorrar dinero, ya que se compromete a planes de uno o tres años para varios productos. La confirmación le permite obtener un descuento en los recursos que utiliza. Reservations puede reducir significativamente los costos de los recursos hasta un 72 % en los precios de pago por uso. Reservations ofrece un descuento en la facturación y no afecta al estado del entorno de ejecución de los recursos. Después de adquirir una reserva, el descuento se aplica automáticamente a los recursos coincidentes.

Puede pagar una reserva por adelantado o mensualmente. El costo total de las reservas por adelantado y mensuales es el mismo y no se pagan cargos adicionales por elegir el pago mensual. El pago mensual está disponible para las reservas de Azure, no para los productos de terceros.

Puede comprar una reserva en [Azure Portal](https://portal.azure.com/#blade/Microsoft_Azure_Reservations/ReservationsBrowseBlade).

## <a name="why-buy-a-reservation"></a>¿Por qué comprar una reserva?

Si tiene un uso constante de recursos que admiten reservas, la compra de una reserva le ofrece la opción de reducir los costos. Por ejemplo, si ejecuta continuamente instancias de un servicio sin una reserva, se le cobra según las tarifas de pago por uso. Cuando compre una reserva, obtendrá inmediatamente el descuento por reserva. Ya no se aplicarán a los recursos las tarifas de pago por uso.

## <a name="how-reservation-discount-is-applied"></a>Aplicación del descuento por reserva

Tras la compra, el descuento por reserva se aplica automáticamente al uso de recursos que coincida con los atributos seleccionados al comprar la reserva. Los atributos incluyen la SKU, las regiones (si procede) y el ámbito. El ámbito de la reserva determina dónde se aplica el ahorro de la reserva.

Para más información sobre cómo se aplica el descuento, consulte [Aplicación del descuento de instancia reservada](reservation-discount-application.md).

Para más información sobre cómo funciona el ámbito de la reserva, consulte [Ámbito de las reservas](prepare-buy-reservation.md#scope-reservations).


## <a name="flexibility-with-azure-reservations"></a>Flexibilidad con las reservas de Azure

Las reservas de Azure proporcionan flexibilidad para ayudar a satisfacer sus necesidades en constante cambio. Puede intercambiar una reserva por otra del mismo tipo. Si ya no necesita una reserva, también puede devolverla, hasta 50 000 USD en un período renovable de 12 meses. El límite máximo de reembolso se aplica a todas las reservas en el ámbito del contrato con Microsoft.

Para más información, consulte [Autoservicio de intercambios y reembolsos de reservas de Azure](exchange-and-refund-azure-reservations.md).


## <a name="charges-covered-by-reservation"></a>Cargos cubiertos por la reserva

- **Instancia reservada de máquina virtual**: una reserva solo cubre el costo de proceso de máquina virtual. No cubre los cargos por software adicional, redes de Windows ni almacenamiento.
- **Capacidad reservada de Azure Storage**: una reserva cubre la capacidad de almacenamiento de las cuentas de almacenamiento estándar para el almacenamiento de blobs o el almacenamiento de Azure Data Lake Gen2. La reserva no cubre las tasas de ancho de banda o de transacción.
- **Capacidad reservada de Azure Cosmos DB**: una reserva abarca el rendimiento aprovisionado de los recursos. No cubre los cargos de almacenamiento y redes.
- **Núcleo virtual reservado de SQL Database**: solo se incluyen con una reserva los costos de proceso. La licencia de SQL se factura por separado.
- **SQL Data Warehouse**: una reserva cubre el uso de cDWU. No cubre los cargos de almacenamiento o de red asociados con el uso de SQL Data Warehouse.
- **Azure Databricks**: una reserva solo cubre el uso de DBU. Otros cargos, tales como proceso, almacenamiento y redes, se aplican por separado.
- **Impuesto sobre el timbre de App Service**: una reserva cubre el uso del timbre. No se aplica a los trabajos, por lo que los demás recursos asociados con el timbre se cobran por separado.
- **Azure Database for MySQL**: solo se incluyen con una reserva los costos de proceso. La reserva no cubre los cargos por software, redes o almacenamiento asociados al servidor de bases de datos de MySQL.
- **Azure Database for PostgreSQL**: solo se incluyen con una reserva los costos de proceso. La reserva no cubre los cargos por software, redes o almacenamiento asociados al servidor de bases de datos de PostgreSQL.
- **Azure Database for MariaDB**: solo se incluyen con una reserva los costos de proceso. La reserva no cubre los cargos por software, redes o almacenamiento asociados al servidor de bases de datos de MariaDB.
- **Azure Data Explorer**: una reserva cubre los cargos de margen de beneficio. Una reserva no cubre los cargos de proceso, de red o de almacenamiento asociados con los clústeres.
- **Azure Cache for Redis**: solo se incluyen los costos de proceso con una reserva. La reserva no cubre los cargos de red o de almacenamiento asociados con las instancias de cache de Redis.
- **Azure Dedicated Host**: solo se incluyen los costos de proceso con el host dedicado.
- **Reservas de Azure Disk Storage**: Una reserva solo cubre las SSD Premium de tamaño P30 o superior. No cubre ningún otro tipo de disco ni tamaños menores que P30.

Planes de software:

- **SUSE Linux**: una reserva cubre los costos de los planes de software. Los descuentos solo se aplican a los medidores SUSE y no en al uso de la máquina virtual.
- **Planes de Red Hat**: una reserva cubre los costos del plan de software. Los descuentos solo se aplican a los medidores RedHat, y no al uso de la máquina virtual.
- **Azure VMware Solution by CloudSimple**: una reserva cubre los nodos de CloudSimple de VMWare. Es posible que todavía se apliquen costos de software adicionales.
- **Red Hat OpenShift en Azure**: una reserva se aplica a los costos de OpenShift, no a los costos de infraestructura de Azure.

En el caso de máquinas virtuales Windows y SQL Database, el descuento por reserva no se aplica a los costos de software. Puede cubrir los costos de licencia con [Ventaja híbrida de Azure](https://azure.microsoft.com/pricing/hybrid-benefit/).


## <a name="need-help-contact-us"></a>¿Necesita ayuda? Póngase en contacto con nosotros.

Si tiene alguna pregunta o necesita ayuda, [cree una solicitud de soporte técnico](https://go.microsoft.com/fwlink/?linkid=2083458).

## <a name="next-steps"></a>Pasos siguientes

- Para más información acerca de Azure Reservations, consulte los siguientes artículos:
    - [Administración de Azure Reservations](manage-reserved-vm-instance.md)
    - [Información sobre el uso de reservas de Azure para suscripciones de pago por uso](understand-reserved-instance-usage.md)
    - [Información sobre el uso de reservas para la inscripción Enterprise](understand-reserved-instance-usage-ea.md)
    - [Costos de software de Windows no incluidos con reservas](reserved-instance-windows-software-costs.md)
    - [Azure Reservations en el programa del Proveedor de soluciones en la nube (CSP) del Centro de partners](/partner-center/azure-reservations)

- Más información sobre las reservas para los planes de servicio:
    - [Máquinas virtuales con Azure Reserved VM Instances](../../virtual-machines/windows/prepay-reserved-vm-instances.md)
    - [Recursos de Azure Cosmos DB con capacidad reservada de Azure Cosmos DB](../../cosmos-db/cosmos-db-reserved-capacity.md)
    - [Recursos de proceso de SQL Database con capacidad reservada de Azure SQL Database](../../sql-database/sql-database-reserved-capacity.md) Más información sobre las reservas para planes de software:
    - [Planes de software de Red Hat con reservas de Azure](../../virtual-machines/linux/prepay-rhel-software-charges.md)
    - [Planes de software SUSE con reservas de Azure](../../virtual-machines/linux/prepay-suse-software-charges.md)
