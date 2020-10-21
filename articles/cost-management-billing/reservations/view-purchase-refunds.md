---
title: Visualización de las transacciones de compra y reembolso de Reserva de Azure
description: Aprenda a ver las transacciones de compra y reembolso de Reserva de Azure.
author: yashesvi
ms.service: cost-management-billing
ms.subservice: reservations
ms.topic: how-to
ms.date: 07/24/2020
ms.author: banders
ms.openlocfilehash: b986aa2bfce203be85adbcde8e2966c167bf7ca1
ms.sourcegitcommit: dbe434f45f9d0f9d298076bf8c08672ceca416c6
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/17/2020
ms.locfileid: "92151780"
---
# <a name="view-reservation-purchase-and-refund-transactions"></a>Visualización de las transacciones de compra y reembolso de las reservas

Hay varias maneras de ver las transacciones de compra y reembolso de reserva. Se pueden usar Azure Portal, Power BI y las API REST.

## <a name="view-reservation-transactions-in-the-azure-portal"></a>Visualización de transacciones de reserva en Azure Portal

Los administradores de facturación de una inscripción Enterprise o de un Contrato de cliente de Microsoft pueden ver las transacciones de reserva en Cost Management y facturación.

1. Inicie sesión en [Azure Portal](https://portal.azure.com).
1. Busque **Administración de costos + facturación**.
1. Seleccione **Transacciones de reserva**.
1. Para filtrar los resultados, seleccione **Intervalo de tiempo**, **Tipo** o **Descripción**.
1. Seleccione **Aplicar**.

[![Captura de pantalla que muestra transacciones de reserva en Azure Portal](./media/view-purchase-refunds/azure-portal-reservation-transactions.png)](./media/view-purchase-refunds/azure-portal-reservation-transactions.png#lightbox)

## <a name="view-reservation-transactions-in-power-bi"></a>Visualización de transacciones de reserva en Power BI

Los administradores de facturación de una inscripción Enterprise o de un Contrato de cliente de Microsoft pueden ver las transacciones de reserva con la aplicación Cost Management para Power BI.

1. Obtenga la [aplicación Azure Cost Management para Power BI](https://appsource.microsoft.com/product/power-bi/costmanagement.azurecostmanagementapp).
1. Vaya al informe de compras de RI.

[![Ejemplo que muestra las transacciones de reserva](./media/view-purchase-refunds/power-bi-reservation-transactions.png)](./media/view-purchase-refunds/power-bi-reservation-transactions.png#lightbox)

Para más información, consulte [Azure Cost Management para Power BI para Contratos Enterprise](../costs/analyze-cost-data-azure-cost-management-power-bi-template-app.md).

## <a name="use-apis-to-get-reservation-transactions"></a>Uso de las API para obtener transacciones de reserva

Los usuarios de Contrato Enterprise (EA) y Contrato de cliente de Microsoft pueden obtener datos de transacciones de reserva mediante [Transacciones de reserva: List API](/rest/api/consumption/reservationtransactions/list).

## <a name="need-help-contact-us"></a>¿Necesita ayuda? Póngase en contacto con nosotros.

Si tiene alguna pregunta o necesita ayuda, [cree una solicitud de soporte técnico](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest).

## <a name="next-steps"></a>Pasos siguientes

- Para más información sobre cómo administrar una reserva, consulte [Administración de Azure Reservations](manage-reserved-vm-instance.md).
- Para obtener más información acerca de Azure Reservations, consulte los siguientes artículos:
  - [¿Qué es Azure Reservations?](save-compute-costs-reservations.md)
  - [Administración de reservas en Azure](manage-reserved-vm-instance.md)