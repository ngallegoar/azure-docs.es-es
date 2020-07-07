---
title: 'Ejemplos de la CLI de Azure: Redes'
description: Ejemplos de la CLI de Azure
services: virtual-network
documentationcenter: virtual-network
author: KumudD
manager: timlt
tags: ''
ms.assetid: ''
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: ''
ms.workload: infrastructure
ms.date: 04/25/2017
ms.author: kumud
ms.openlocfilehash: 84754a61bfe9537e928759aefbcb5dcddce33089
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/02/2020
ms.locfileid: "81457967"
---
# <a name="azure-cli-samples-for-networking"></a>Ejemplos de la CLI de Azure para redes

En la tabla siguiente se incluyen vínculos a scripts de Bash creados con la CLI de Azure.

| | |
|-|-|
|**Conectividad entre recursos de Azure**||
| [Creación de una red virtual para aplicaciones de niveles múltiples](./scripts/virtual-network-cli-sample-multi-tier-application.md?toc=%2fazure%2fnetworking%2ftoc.json) | Creación de una red virtual con subredes de front-end y back-end. El tráfico a la subred de front-end está limitado a HTTP y SSH, mientras que el tráfico a la subred de back-end está limitado a MySQL, al puerto 3306. |
| [Emparejamiento de dos redes virtuales](./scripts/virtual-network-cli-sample-peer-two-virtual-networks.md?toc=%2fazure%2fnetworking%2ftoc.json) | Creación y conexión de dos redes virtuales en la misma región. |
| [Enrutamiento del tráfico por una aplicación virtual de red](./scripts/virtual-network-cli-sample-route-traffic-through-nva.md?toc=%2fazure%2fnetworking%2ftoc.json) | Creación de una red virtual con subredes de front-end y back-end y una máquina virtual que pueda enrutar el tráfico entre dos subredes. |
| [Filtrado del tráfico de red entrante y saliente de máquina virtual](./scripts/virtual-network-filter-network-traffic.md?toc=%2fazure%2fnetworking%2ftoc.json) | Creación de una red virtual con subredes de front-end y back-end. El tráfico de red entrante a la subred de front-end está limitado a HTTP, HTTPS y SSH. No se permite el tráfico saliente a Internet desde la subred de back-end. |
|**Equilibrio de carga y dirección del tráfico**||
| [Equilibrio de carga entre varios sitios web en máquinas virtuales](./scripts/load-balancer-linux-cli-load-balance-multiple-websites-vm.md?toc=%2fazure%2fnetworking%2ftoc.json) | Creación de dos máquinas virtuales con varias configuraciones de IP, unidas a un conjunto de disponibilidad de Azure, accesibles a través de una instancia de Azure Load Balancer. |
| [Dirección del tráfico entre varias regiones para conseguir alta disponibilidad de las aplicaciones](./scripts/traffic-manager-cli-websites-high-availability.md?toc=%2fazure%2fnetworking%2ftoc.json) |  Creación de dos planes de App Service, dos aplicaciones web, un perfil de Traffic Manager y dos puntos de conexión de Traffic Manager. |
| | |
