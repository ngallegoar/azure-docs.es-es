---
title: Creación de recomendaciones de reservas de Azure
description: Obtenga información sobre cómo se crean las recomendaciones de reservas de Azure para máquinas virtuales.
author: banders
ms.reviewer: yashar
ms.service: cost-management-billing
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/28/2020
ms.author: banders
ms.openlocfilehash: 2f7b09c14553fdb5d642080d286ce123176b997f
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/24/2020
ms.locfileid: "76991055"
---
# <a name="how-reservation-recommendations-are-created"></a>Creación de recomendaciones de reservas
Las recomendaciones de compra de instancias reservadas de Azure (RI) se proporcionan mediante la [API de recomendación de reservas](/rest/api/consumption/reservationrecommendations) de Azure Consumption, [Azure Advisor](../..//advisor/advisor-cost-recommendations.md#buy-reserved-virtual-machine-instances-to-save-money-over-pay-as-you-go-costs) y a través de la experiencia de compra de reservas de Azure Portal.

En los pasos siguientes se define cómo se calculan las recomendaciones: 
1. El motor de recomendaciones evalúa el uso por hora de los recursos en el ámbito determinado de los últimos 7, 30 y 60 días.
2. Según los datos de uso, el motor simula los costos con y sin reservas.
3. Los costos se simulan para distintas cantidades y se recomienda la cantidad que maximiza el ahorro.
4. Si los recursos se cierran con regularidad, la simulación no encontrará ningún ahorro y no se proporcionará ninguna recomendación de compra.

## <a name="recommendations-in-azure-advisor"></a>Recomendaciones en Azure Advisor
Las recomendaciones de compra de reservas para máquinas virtuales están disponibles en Azure Advisor. Tenga en cuenta los siguientes puntos: 
- Advisor solo tiene recomendaciones de ámbito de una sola suscripción.
- Las recomendaciones que se calculan con un período retrospectivo de 30 días están disponibles en Advisor.
- Si adquiere una reserva de ámbito compartido, las recomendaciones de compra de reservas de Advisor pueden tardar hasta 30 días en desaparecer.

## <a name="other-expected-api-behavior"></a>Otro comportamiento esperado de la API
- Si usa un período retrospectivo de siete días, es posible que no obtenga recomendaciones cuando las máquinas virtuales estén apagadas durante más de un día.

## <a name="next-steps"></a>Pasos siguientes
- Más información sobre la [Aplicación del descuento por reserva de Azure en las máquinas virtuales](../manage/understand-vm-reservation-charges.md).
