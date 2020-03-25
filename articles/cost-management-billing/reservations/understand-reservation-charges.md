---
title: Descripción del descuento por reserva para bases de datos Azure SQL Database | Microsoft Docs
description: Aprenda cómo se aplica un descuento en la reserva a las bases de datos de SQL Database en ejecución.
author: yashesvi
ms.reviewer: yashar
ms.service: cost-management-billing
ms.topic: conceptual
ms.date: 02/13/2020
ms.author: banders
ms.openlocfilehash: f0b093950b7f8fbbfbb0ca8ceb48cb0aa6341e10
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/24/2020
ms.locfileid: "77200289"
---
# <a name="how-a-reservation-discount-is-applied-to-azure-sql-databases"></a>Aplicación de un descuento en la reserva a las bases de datos Azure SQL Database

Después de comprar capacidad reservada en Azure SQL Database, el descuento en la reserva se aplica automáticamente a las instancias que coincidan con los atributos y la cantidad de la reserva. Una reserva cubre los costos de proceso de la instancia de SQL Database. Se le cobra por el software, el almacenamiento y la administración de redes según las tarifas normales. Puede cubrir los costos de licencia de las instancias de SQL Database con [Ventaja híbrida de Azure](https://azure.microsoft.com/pricing/hybrid-benefit/).

Para las instancias reservadas de máquina virtual, consulte el artículo de [información sobre el descuento en las instancias reservadas de máquina virtual de Azure](../manage/understand-vm-reservation-charges.md).

## <a name="how-reservation-discount-is-applied"></a>Aplicación del descuento por reserva

Un descuento de reserva significa "*usarlo o perderlo*". Por lo tanto, si no tiene recursos coincidentes para ninguna hora, perderá una cantidad de reserva para esa hora. No se pueden arrastrar las horas reservadas no utilizadas.

Al cerrar un recurso, el descuento por reserva se aplica automáticamente a otro recurso que coincida con el ámbito especificado. Si no se encuentran recursos coincidentes en el ámbito especificado, las horas reservadas se *pierden*.

## <a name="discount-applied-to-sql-databases"></a>Descuento reserva aplicado a las base de datos SQL Database

 El descuento sobre la capacidad reservada de SQL Database se aplica a las instancias en ejecución por hora. La reserva que compra coincide con el uso de procesos que emiten las instancias de SQL Database en ejecución. Para las instancias de SQL Database que no se ejecutan durante una hora entera, la reserva se aplica automáticamente a otras instancias que coincidan con los atributos de reserva. El descuento se puede aplicar a instancias de SQL Database en ejecución simultáneamente. Si no tiene instancias de SQL Database que se ejecuten durante toda la hora que coincidan con los atributos de reserva, no obtendrá todas las ventajas del descuento en la reserva para esa hora.

En los ejemplos siguientes se muestra cómo se aplica el descuento en la capacidad reservada de SQL Database en función del número de núcleos adquiridos y su momento de ejecución.

- Escenario 1: compra capacidad reservada de SQL Database para una instancia de 8 núcleos. Ejecuta una instancia de SQL Database con 16 núcleos que coincide con el resto de los atributos de la reserva. Se le cobra el precio de pago por uso de 8 núcleos por el proceso de SQL Database. Obtiene el descuento en la reserva para una hora de uso de proceso para una instancia de SQL Database de 8 núcleos.

En el resto de estos ejemplos, supongamos que la capacidad reservada de SQL Database que compra es para una instancia de 16 núcleos y el resto de atributos de la reserva coinciden con las instancias de SQL Database en ejecución.

- Escenario 2: ejecuta dos instancias de SQL Database con 8 núcleos cada una durante una hora. Se aplica el descuento en la reserva para 16 núcleos al uso de proceso para ambas instancias de SQL Database de 8 núcleos.
- Escenario 3: ejecuta una instancia de SQL Database de 16 núcleos de las 13:00 a las 13:30. Ejecuta otra instancia de SQL Database de 16 núcleos de las 13:30 a las 14:00. Ambas están cubiertas por el descuento en la reserva.
- Escenario 4: ejecuta una instancia de SQL Database de 16 núcleos de las 13:00 a las 13:45. Ejecuta otra instancia de SQL Database de 16 núcleos de las 13:30 a las 14:00. Se le cobra el precio de pago por uso por el solapamiento de 15 minutos. El descuento en la reserva se aplica al uso de proceso por el resto del tiempo.

Para obtener información sobre la aplicación de Azure Reservations en informes de uso de facturación, consulte el artículo [Información sobre el uso de Azure Reservations](understand-reserved-instance-usage-ea.md).

## <a name="need-help-contact-us"></a>¿Necesita ayuda? Ponerse en contacto con nosotros

Si tiene alguna pregunta o necesita ayuda, [cree una solicitud de soporte técnico](https://go.microsoft.com/fwlink/?linkid=2083458).

## <a name="next-steps"></a>Pasos siguientes

Para obtener más información acerca de Azure Reservations, consulte los siguientes artículos:

- [¿Qué es Azure Reservations?](save-compute-costs-reservations.md)
- [Pago por adelantado de máquinas virtuales con Azure Reserved VM Instances](../../virtual-machines/windows/prepay-reserved-vm-instances.md)
- [Pago por adelantado de los recursos de proceso de SQL Database con capacidad reservada de Azure SQL Database](../../sql-database/sql-database-reserved-capacity.md)
- [Administración de Azure Reservations](manage-reserved-vm-instance.md)
- [Información sobre el uso de reservas para suscripciones de pago por uso](understand-reserved-instance-usage.md)
- [Información sobre el uso de reservas para la inscripción Enterprise](understand-reserved-instance-usage-ea.md)
- [Información sobre el uso de reservas para suscripciones de CSP](/partner-center/azure-reservations)
