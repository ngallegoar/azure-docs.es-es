---
title: Estadísticas de latencia de ida y vuelta de red de Azure | Microsoft Docs
description: Obtenga información sobre las estadísticas de latencia de ida y vuelta entre regiones de Azure.
services: networking
author: nayak-mahesh
ms.service: virtual-network
ms.topic: article
ms.date: 08/05/2020
ms.author: kumud
ms.openlocfilehash: 72168a56bfb4e08c7f44c84c773d9f6599cfa607
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "91848896"
---
# <a name="azure-network-round-trip-latency-statistics"></a>Estadísticas de latencia de ida y vuelta de red de Azure

Azure supervisa continuamente la latencia (velocidad) de las áreas principales de su red mediante herramientas de supervisión internas, así como las mediciones recopiladas por [ThousandEyes](https://thousandeyes.com), un servicio de supervisión sintética de terceros.

## <a name="how-are-the-measurements-collected"></a>¿Cómo se recopilan las medidas?

Las medidas de latencia se recopilan de agentes de ThousandEyes hospedados en regiones en la nube de Azure de todo el mundo, que envían continuamente sondeos de red entre ellos, en intervalos de 1 minuto. Las estadísticas de latencia mensuales se derivan de calcular el promedio de las muestras recopiladas del mes.

## <a name="september-2020-round-trip-latency-figures"></a>Cifras de latencia de recorrido de ida y vuelta de septiembre de 2020

A continuación se muestra el promedio mensual de los tiempos de ida y vuelta entre las regiones de Azure durante los últimos 30 días (que terminan el 30 de septiembre de 2020). Las siguientes medidas cuentan con la tecnología de [ThousandEyes](https://thousandeyes.com).

[![Estadísticas de latencia interregionales de Azure](media/azure-network-latency/azure-network-latency.png)](media/azure-network-latency/azure-network-latency.png#lightbox)

## <a name="next-steps"></a>Pasos siguientes

Más información sobre las [regiones de Azure](https://azure.microsoft.com/global-infrastructure/regions/).
