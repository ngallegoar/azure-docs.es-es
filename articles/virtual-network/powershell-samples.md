---
title: Ejemplos de Azure PowerShell para red virtual
description: Ejemplos de Azure PowerShell para red virtual
services: virtual-network
documentationcenter: virtual-network
author: KumudD
manager: mtillman
editor: ''
tags: ''
ms.assetid: ''
ms.service: virtual-network
ms.devlang: na
ms.topic: sample
ms.tgt_pltfrm: ''
ms.workload: infrastructure
ms.date: 07/15/2019
ms.author: kumud
ms.openlocfilehash: f4ba3754467efda852573989aa89bc8a4a99c469
ms.sourcegitcommit: 537c539344ee44b07862f317d453267f2b7b2ca6
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/11/2020
ms.locfileid: "84688271"
---
# <a name="azure-powershell-samples-for-virtual-network"></a>Ejemplos de Azure PowerShell para red virtual

En la tabla siguiente se incluyen vínculos a scripts de Azure PowerShell:

| | |
|----|----|
| [Creación de una red virtual para aplicaciones de niveles múltiples](./scripts/virtual-network-powershell-sample-multi-tier-application.md) | Creación de una red virtual con subredes de front-end y back-end. El tráfico a la subred de front-end está limitado a HTTP, mientras que el tráfico a la subred de back-end está limitado a SQL, al puerto 1433. |
| [Emparejamiento de dos redes virtuales](./scripts/virtual-network-powershell-sample-peer-two-virtual-networks.md) | Creación y conexión de dos redes virtuales en la misma región. |
| [Enrutamiento del tráfico por una aplicación virtual de red](./scripts/virtual-network-powershell-sample-route-traffic-through-nva.md) | Creación de una red virtual con subredes de front-end y back-end y una máquina virtual que pueda enrutar el tráfico entre dos subredes. |
| [Filtrado del tráfico de red entrante y saliente de máquina virtual](./scripts/virtual-network-powershell-sample-filter-network-traffic.md) | Creación de una red virtual con subredes de front-end y back-end. El tráfico de red entrante a la subred de front-end está limitado a HTTP y HTTPS. No se permite el tráfico saliente a Internet desde la subred de back-end. |
|[Configuración de una red virtual de doble pila de IPv4 + IPv6 con Basic Load Balancer](./scripts/virtual-network-powershell-sample-ipv6-dual-stack.md)|Implementa la red virtual de doble pila (IPv4 + IPv6) con dos máquinas virtuales y una instancia de Azure Basic Load Balancer con las direcciones IP públicas IPv4 e IPv6. |
|[Configuración de una red virtual de doble pila de IPv4 + IPv6 con Standard Load Balancer](./scripts/virtual-network-powershell-sample-ipv6-dual-stack-standard-load-balancer.md)|Implementa la red virtual de doble pila (IPv4 + IPv6) con dos máquinas virtuales y una instancia de Azure Standard Load Balancer con las direcciones IP públicas IPv4 e IPv6. |
