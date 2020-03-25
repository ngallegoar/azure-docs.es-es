---
title: Cómo se aplican descuentos de reserva a Azure SQL Data Warehouse | Microsoft Docs
description: Obtenga información sobre cómo se aplican descuentos de reserva a Azure SQL Data Warehouse para ayudarle a ahorrar dinero.
author: yashesvi
ms.reviewer: yashar
ms.service: cost-management-billing
ms.topic: conceptual
ms.date: 02/12/2020
ms.author: banders
ms.openlocfilehash: b08f11799c1471af22138fefcd57ed1eb951a1a6
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/24/2020
ms.locfileid: "77199388"
---
# <a name="how-reservation-discounts-apply-to-azure-sql-data-warehouse"></a>Cómo se aplican descuentos de reserva a Azure SQL Data Warehouse

Después de comprar capacidad reservada de Azure SQL Data Warehouse, el descuento de reserva se aplica automáticamente a los almacenes de datos que existen en esa región. Se aplica el descuento de reserva para la utilización que emite el medidor de cDWU de SQL Data Warehouse. El almacenamiento y las redes se cargan con tarifas de pago por uso.

## <a name="reservation-discount-application"></a>Aplicación del descuento de reserva

El descuento sobre la capacidad reservada de SQL Data Warehouse se aplica a los almacenes en ejecución por hora. Si no tiene un almacén implementado durante una hora, se pierde la capacidad reservada para esa hora. No se transfiere.

Después de la compra, la reserva que ha adquirido se asocia al uso de SQL Data Warehouse emitido al ejecutar los almacenes en cualquier momento. Si apaga algunos almacenes, los descuentos de reserva se aplican automáticamente a otros almacenes que coincidan.

Para los almacenes que no se ejecuten durante una hora completa, la reserva se aplica automáticamente a otras instancias que coincidan en esa hora.

## <a name="discount-examples"></a>Ejemplos de descuento

En los ejemplos siguientes se muestra cómo se aplica el descuento sobre la capacidad reservada de SQL Data Warehouse en función de las implementaciones.

- **Ejemplo 1**: compra 5 unidades de capacidad de reserva de 100 cDWU. Ejecuta una instancia de SQL Data Warehouse de DW1500c durante una hora. En este caso, se emite el uso de 15 unidades de uso de 100 cDWU. El descuento de reserva se aplica a las 5 unidades que se han usado. Se le cargará con tarifas de pago por uso para las restantes 10 unidades de uso de 100 cDWU que se han usado. En otras palabras, la cobertura parcial es posible para varias reservas.

- **Ejemplo 2**: compra 5 unidades de capacidad de reserva de 100 cDWU. Ejecuta dos instancias de SQL Data Warehouse de DW100c durante una hora. En este caso, se emiten dos eventos de uso para 1 unidad de utilización de 100 cDWU. Ambos eventos de uso obtienen descuentos de capacidad reservada. Las 3 unidades de capacidad de reserva de 100 cDWU restantes se pierden y no se transfieren para un uso futuro. En otras palabras, una reserva única puede coincidir con varias instancias de SQL Data Warehouse.

- **Ejemplo 3**: compra 1 unidad de capacidad de reserva de 100 cDWU. Ejecuta dos instancias de SQL Data Warehouse de DW100c. Cada una de ellas se ejecuta durante 30 minutos. En este caso, ambos eventos de uso obtienen descuentos de capacidad reservada. No se carga ninguna utilización con tarifas de pago por uso.

## <a name="need-help-contact-us"></a>¿Necesita ayuda? Ponerse en contacto con nosotros

- Si tiene alguna pregunta o necesita ayuda, [cree una solicitud de soporte técnico](https://go.microsoft.com/fwlink/?linkid=2083458).

## <a name="next-steps"></a>Pasos siguientes

Para obtener más información acerca de Azure Reservations, consulte los siguientes artículos:

- [¿Qué es Azure Reservations?](save-compute-costs-reservations.md)
- [Ver transacciones de reserva](view-reservations.md)
- [Obtener las transacciones y el uso de reserva a través de la API](reservation-apis.md)
- [Administrar reservas](manage-reserved-vm-instance.md)
