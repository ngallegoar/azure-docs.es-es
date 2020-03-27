---
title: Prepararse para la compra de una reserva de Azure
description: Antes de comprar una reserva de Azure, es importante que conozca algunos aspectos importantes.
author: bandersmsft
ms.reviewer: yashar
ms.service: cost-management-billing
ms.topic: conceptual
ms.date: 03/24/2020
ms.author: banders
ms.openlocfilehash: 1f5ca2d43356eab98cffe8414c00d97e5744739a
ms.sourcegitcommit: 253d4c7ab41e4eb11cd9995190cd5536fcec5a3c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/25/2020
ms.locfileid: "80235648"
---
# <a name="prepare-to-buy-a-reservation"></a>Prepararse para la compra de una reserva

Las reservas de Azure le ayudan a ahorrar dinero, ya que se compromete a planes de uno o tres años para muchos productos de Azure. Antes de entrar en un compromiso para comprar una reserva, asegúrese de revisar las secciones siguientes para prepararse para ello.

## <a name="who-can-buy-a-reservation"></a>Quién puede comprar una reserva

Para comprar un plan, debe tener un rol de propietario de la suscripción en una suscripción Enterprise (MS-AZR-0017P o MS-AZR-0148P) o de pago por uso (MS-AZR-0003P o MS-AZR-0023P), o bien una suscripción de Microsoft Customer Agreement. Los proveedores de soluciones en la nube pueden usar Azure Portal o el  [Centro de partners](/partner-center/azure-reservations) para comprar reservas de Azure.

Los clientes de Contrato Enterprise (EA) pueden limitar sus compras a los administradores de EA mediante la desactivación de la opción **Add Reserved Instances** (Agregar instancias reservadas) en EA Portal. Para poder comprar una reserva, los administradores de Contrato Enterprise deben ser propietarios de la suscripción para al menos una suscripción de Contrato Enterprise. La opción es útil para empresas que requieren un equipo centralizado para comprar reservas para distintos centros de coste. Después de la compra, los equipos centralizados pueden agregar propietarios de centros de coste a las reservas. Los propietarios pueden entonces ampliar la reserva a sus suscripciones. El equipo central no necesita tener acceso al propietario de la suscripción donde se compra la reserva.

Los descuentos por reserva se aplican solo a los recursos asociados con las suscripciones compradas a través de un contrato Enterprise, Proveedor de soluciones en la nube (CSP), Microsoft Customer Agreement y de planes individuales con tarifas de pago por uso.

## <a name="scope-reservations"></a>Ámbito de las reservas

Puede limitar el ámbito de una reserva a una suscripción o a grupos de recursos. Al limitar el ámbito de una reserva, se selecciona el lugar en el que se aplican los ahorros de la reserva. Al limitar el ámbito de la reserva a un grupo de recursos, los descuentos por reserva solo se aplican al grupo de recursos, no a la suscripción completa.

### <a name="reservation-scoping-options"></a>Opciones de limitación del ámbito de la reserva

Tiene tres opciones para limitar el ámbito de una reserva, según sus necesidades:

- **Single resource group scope** (Ámbito de grupo de recursos único): el descuento por reserva se aplica a los recursos coincidentes solo del grupo de recursos seleccionado.
- **Single subscription scope** (Ámbito de suscripción única): el descuento por reserva se aplica a los recursos coincidentes de la suscripción seleccionada.
- **Shared scope** (Ámbito compartido): el descuento por reserva se aplica a los recursos coincidentes en suscripciones válidas que están en el contexto de facturación. Para los clientes con Contrato Enterprise, el contexto de facturación es la inscripción. En el caso de los clientes con contrato Microsoft Customer Agreement, el ámbito de facturación es el perfil de facturación. En el caso de suscripciones individuales con tarifas de pago por uso, el ámbito de facturación son todas las suscripciones aptas creadas por el administrador de la cuenta.

Al aplicar los descuentos por reserva sobre su uso, Azure procesa la reserva en el orden siguiente:

1. Reservas cuyo ámbito es un grupo de recursos
2. Reservas de ámbito único
3. Reservas de ámbito compartido

Un único grupo de recursos puede obtener descuentos por reserva de varias reservas, en función de cómo se establezca el ámbito de las reservas.

También puede actualizar el ámbito después de comprar una reserva. Para ello, vaya a la reserva, haga clic en **Configuración** y limite de nuevo el ámbito de la reserva. Volver a limitar el ámbito de una reserva no es una transacción comercial. No se cambian las condiciones de la reserva. Para más información sobre cómo actualizar el ámbito, consulte [Update the scope after you purchase a reservation](manage-reserved-vm-instance.md#change-the-reservation-scope) (Actualización del ámbito después de comprar una reserva).

![Ejemplo que muestra un cambio en el ámbito de la reserva](./media/prepare-buy-reservation/rescope-reservation-resource-group.png)

## <a name="purchase-reservations"></a>Compra de reservas

Puede comprar reservas en Azure Portal, las API, PowerShell o la CLI. Lea los siguientes artículos que correspondan a su caso cuando esté preparado para hacer una compra de reservas:

- [App Service](prepay-app-service-isolated-stamp.md)
- [Azure Cache for Redis](../../azure-cache-for-redis/cache-reserved-pricing.md)
- [Cosmos DB](../../cosmos-db/cosmos-db-reserved-capacity.md)
- [Databricks](prepay-databricks-reserved-capacity.md)
- [Data Explorer](../../data-explorer/pricing-reserved-capacity.md)
- [Disk Storage](../../virtual-machines/linux/disks-reserved-capacity.md)
- [Host dedicado](../../virtual-machines/prepay-dedicated-hosts-reserved-instances.md)
- [Planes de software](../../virtual-machines/linux/prepay-suse-software-charges.md)
- [Storage](../../storage/blobs/storage-blob-reserved-capacity.md)
- [SQL Database](../../sql-database/sql-database-reserved-capacity.md)
- [SQL Data Warehouse](prepay-sql-data-warehouse-charges.md)
- [Máquinas virtuales](../../virtual-machines/windows/prepay-reserved-vm-instances.md)

## <a name="next-steps"></a>Pasos siguientes

- [Administración de reservas para los recursos de Azure](manage-reserved-vm-instance.md)
