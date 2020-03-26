---
title: Ejemplos de CLI de Azure para redes virtuales
description: Ejemplos de CLI de Azure para redes virtuales.
services: virtual-network
documentationcenter: virtual-network
author: KumudD
manager: twooley
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
ms.openlocfilehash: 5ce10cf37b61b0269e6c8f2279b8814d9dc4a4f9
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/24/2020
ms.locfileid: "78271217"
---
# <a name="azure-cli-samples-for-virtual-network"></a>Ejemplos de CLI de Azure para redes virtuales

En la tabla siguiente se incluyen vínculos a scripts de Bash con comandos de CLI de Azure:

| | |
|----|----|
| [Creación de una red virtual para aplicaciones de niveles múltiples](./scripts/virtual-network-cli-sample-multi-tier-application.md) | Creación de una red virtual con subredes de front-end y back-end. El tráfico a la subred de front-end está limitado a HTTP y SSH, mientras que el tráfico a la subred de back-end está limitado a MySQL, al puerto 3306. |
| [Emparejamiento de dos redes virtuales](./scripts/virtual-network-cli-sample-peer-two-virtual-networks.md) | Creación y conexión de dos redes virtuales en la misma región. |
| [Enrutamiento del tráfico por una aplicación virtual de red](./scripts/virtual-network-cli-sample-route-traffic-through-nva.md) | Creación de una red virtual con subredes de front-end y back-end y una máquina virtual que pueda enrutar el tráfico entre dos subredes. |
| [Filtrado del tráfico de red entrante y saliente de máquina virtual](./scripts/virtual-network-cli-sample-filter-network-traffic.md) | Creación de una red virtual con subredes de front-end y back-end. El tráfico de red entrante a la subred de front-end está limitado a HTTP, HTTPS y SSH. No se permite el tráfico saliente a Internet desde la subred de back-end. |
|[Configuración de una red virtual de doble pila de IPv4 + IPv6 con Basic Load Balancer](./scripts/virtual-network-cli-sample-ipv6-dual-stack.md)|Implementa la red virtual de doble pila (IPv4 + IPv6) con dos máquinas virtuales y una instancia de Azure Basic Load Balancer con las direcciones IP públicas IPv4 e IPv6. |
|[Configuración de una red virtual de doble pila de IPv4 + IPv6 con Standard Load Balancer](./scripts/virtual-network-cli-sample-ipv6-dual-stack-standard-load-balancer.md)|Implementa la red virtual de doble pila (IPv4 + IPv6) con dos máquinas virtuales y una instancia de Azure Standard Load Balancer con las direcciones IP públicas IPv4 e IPv6. |
|[Tutorial: Creación y prueba de una puerta de enlace de NAT: CLI de Azure](../virtual-network/tutorial-create-validate-nat-gateway-cli.md)|Cree y valide una puerta de enlace NAT mediante una máquina virtual de origen y de destino. |