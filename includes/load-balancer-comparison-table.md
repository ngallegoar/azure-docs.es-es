---
title: archivo de inclusión
description: archivo de inclusión
services: load balancer
author: KumudD
ms.service: load-balancer
ms.topic: include
ms.date: 02/21/2020
ms.author: kumud
ms.custom: include file
ms.openlocfilehash: 33e6fecafa2b9748a27794800365974ebc975d0e
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/26/2020
ms.locfileid: "78202532"
---
| | Standard Load Balancer | Versión Básico de Load Balancer |
| --- | --- | --- |
| [Tamaño de grupo de back-end](https://docs.microsoft.com/azure/azure-resource-manager/management/azure-subscription-service-limits#load-balancer) | Admite hasta 1000 instancias. | Admite hasta 300 instancias. |
| Puntos de conexión del grupo de back-end | Todas las máquinas virtuales o conjuntos de escalado de máquinas virtuales de una red virtual individual. | Máquinas virtuales en un único conjunto de disponibilidad o conjunto de escalado de máquinas virtuales. |
| [Sondeos de mantenimiento](../articles/load-balancer/load-balancer-custom-probe-overview.md#types) | TCP, HTTP, HTTPS | TCP, HTTP |
| [Comportamiento del sondeo de mantenimiento](../articles/load-balancer/load-balancer-custom-probe-overview.md#probedown) | Las conexiones TCP permanecen activas en el sondeo de la instancia __y__ en todos los sondeos. | Las conexiones TCP permanecen activas en un sondeo de instancia. Todas las conexiones TCP finalizan cuando todos los sondeos están inactivos. |
| Zonas de disponibilidad | Servidores front-end con redundancia de zona y zonales para el tráfico de entrada y salida. | No disponible |
| Diagnóstico | [Métricas multidimensionales de Azure Monitor](../articles/load-balancer/load-balancer-standard-diagnostics.md) | [Registros de Azure Monitor](../articles/load-balancer/load-balancer-monitor-log.md) |
| Puertos HA | [Disponibles para el equilibrador de carga interno](../articles/load-balancer/load-balancer-ha-ports-overview.md) | No disponible |
| Seguro de forma predeterminada | Cerrado a los flujos de entrada, a menos que lo permita un grupo de seguridad de red. Tenga en cuenta que se permite el tráfico interno de la red virtual al equilibrador de carga interno. | Abrir de forma predeterminada. Grupo de seguridad de red opcional. |
| Reglas de salida | [Configuración declarativa de NAT de salida](../articles/load-balancer/load-balancer-outbound-rules-overview.md) | No disponible |
| Restablecimiento de TCP en tiempo de inactividad | [Disponible en cualquier regla](../articles/load-balancer/load-balancer-tcp-reset.md) | No disponible |
| [Varios servidores front-end](../articles/load-balancer/load-balancer-multivip-overview.md) | Entrada y [salida](../articles/load-balancer/load-balancer-outbound-connections.md) | Solo de entrada |
| Operaciones de administración | La mayoría de las operaciones en menos de 30 segundos | Normalmente, entre 60 y 90 segundos |
| Contrato de nivel de servicio | [99.99%](https://azure.microsoft.com/support/legal/sla/load-balancer/v1_0/) | No disponible | 
