---
title: Solución de problemas con la descarga de detalles de uso de reserva de Azure
description: Este artículo le ayuda a comprender por qué la descarga de detalles de uso de la instancia reservada no está disponible en Azure Portal y a solucionar los problemas relacionados.
author: bandersmsft
ms.service: cost-management-billing
ms.subservice: reservations
ms.author: banders
ms.reviewer: yashar
ms.topic: troubleshooting
ms.date: 10/30/2020
ms.openlocfilehash: 85584626b050be8052f59c80ab294cc62747daed
ms.sourcegitcommit: 4b76c284eb3d2b81b103430371a10abb912a83f4
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/01/2020
ms.locfileid: "93147344"
---
# <a name="troubleshoot-azure-reservation-download-usage-details"></a>Solución de problemas con la descarga de detalles de uso de reserva de Azure

Este artículo le ayuda a comprender por qué la descarga de detalles de uso de la instancia reservada no está disponible en Azure Portal y a solucionar los problemas relacionados.

## <a name="symptoms"></a>Síntomas

1. Inicie sesión en [Azure Portal](https://portal.azure.com/) y vaya a **Reservas**.
1. Seleccione una reserva.
1. En la página de información general de la reserva, la vista predeterminada muestra el uso de los siete últimos días. Puede seleccionar **Descargar como CSV** para descargar los detalles de uso de la reserva.
1. Al cambiar el intervalo de tiempo, la opción **Descargar como CSV** deja de estar disponible.
    :::image type="content" source="./media/troubleshoot-download-usage/download-csv-unavailable.png" alt-text="Ejemplo que muestra Descargar como CSV no disponible" lightbox="./media/troubleshoot-download-usage/download-csv-unavailable.png" :::

## <a name="cause"></a>Causa

Debido a las limitaciones técnicas, Azure no admite la descarga de datos para un período superior a siete días. Los períodos prolongados para clientes con grandes cantidades de reservas generan grandes cantidades de datos. La devolución de datos a través de las API impone una presión adicional en los recursos de Azure.

## <a name="solution"></a>Solución

Somos conscientes de que los clientes quieren descargar datos para períodos más largos. Sin embargo, actualmente no hay ninguna forma de descargar datos de uso de reserva para períodos prolongados.

## <a name="next-steps"></a>Pasos siguientes

- Para más información sobre las reservas, consulte [¿Qué es Azure Reservations?](save-compute-costs-reservations.md).