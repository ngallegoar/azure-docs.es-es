---
title: Estadísticas de latencia de recorrido de ida y vuelta de red de Azure | Microsoft Docs
description: Conozca las estadísticas de latencia de recorrido de ida y vuelta entre regiones de Azure.
services: networking
author: nayak-mahesh
ms.service: virtual-network
ms.topic: article
ms.date: 03/10/2020
ms.author: kumud
ms.openlocfilehash: 3469daa8b81b20d5d0052a23ce3236fc4fde75de
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "79082952"
---
# <a name="azure-network-round-trip-latency-statistics"></a>Estadísticas de latencia de recorrido de ida y vuelta de red de Azure

Azure supervisa continuamente la latencia (velocidad) de las áreas principales de su red mediante herramientas de supervisión internas, así como las mediciones recopiladas por [ThousandEyes](https://thousandeyes.com), un servicio de supervisión sintética de terceros.

## <a name="how-are-the-measurements-collected"></a>¿Cómo se recopilan las medidas?

Las medidas de latencia se recopilan de agentes de ThousandEyes hospedados en regiones en la nube de Azure de todo el mundo, que envían continuamente sondeos de red entre ellos, en intervalos de 1 minuto. Las estadísticas de latencia mensuales se derivan de calcular el promedio de las muestras recopiladas del mes.

## <a name="february-2020-round-trip-latency-figures"></a>Cifras de latencia de recorrido de ida y vuelta de febrero de 2020

A continuación se muestra el promedio mensual de los tiempos de recorrido de ida y vuelta entre las regiones de Azure durante los últimos 29 días (que terminan el 29 de febrero de 2020). Las siguientes medidas cuentan con la tecnología de [ThousandEyes](https://thousandeyes.com).

[![Estadísticas de latencia interregionales de Azure](media/azure-network-latency/azure-network-latency.png)](media/azure-network-latency/azure-network-latency.png#lightbox)

## <a name="next-steps"></a>Pasos siguientes

Más información sobre las [regiones de Azure](https://azure.microsoft.com/global-infrastructure/regions/).
