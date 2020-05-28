---
title: archivo de inclusión
description: archivo de inclusión
services: firewall
author: vhorne
ms.service: firewall
ms.topic: include
ms.date: 05/14/2020
ms.author: victorh
ms.custom: include file
ms.openlocfilehash: 4e00b0c46c79dc058d19076a396d055ba1d41b87
ms.sourcegitcommit: fdec8e8bdbddcce5b7a0c4ffc6842154220c8b90
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/19/2020
ms.locfileid: "83673533"
---
| Resource | Límite |
| --- | --- |
| Rendimiento de los datos |30 Gbps<sup>1</sup> |
|Reglas|10 000. Todos los tipos de reglas combinados.|
|Número máximo de reglas DNAT|298<br>Si el protocolo de una regla está configurado tanto para TCP como para UDP, cuenta como dos reglas.|
|Tamaño mínimo de AzureFirewallSubnet |/26|
|Intervalo de puertos en reglas de red y aplicación|1 - 65535|
|Direcciones IP públicas|250 máximo. Todas las IP públicas se pueden usar en las reglas de DNAT y todas ellas contribuyen a los puertos SNAT disponibles.|
|Direcciones IP de grupos de IP|50 grupos de IP o menos: cada uno puede tener un máximo de 5000 direcciones IP individuales por instancia de firewall.<br>Entre 51 y 100 grupos de IP: cada uno puede tener 500 direcciones IP individuales por instancia de firewall.<br><br>Para más información, consulte [IP Groups (versión preliminar) en Azure Firewall](../articles/firewall/ip-groups.md#ip-address-limits).
|Tabla de rutas|De forma predeterminada, AzureFirewallSubnet tiene una ruta 0.0.0.0/0 con el valor de NextHopType establecido en **Internet**.<br><br>Azure Firewall debe tener conectividad directa a Internet. Si AzureFirewallSubnet aprende una ruta predeterminada a la red local mediante BGP, debe reemplazarla por una UDR 0.0.0.0/0 con el valor **NextHopType** establecido como **Internet** para mantener la conectividad directa a Internet. De forma predeterminada, Azure Firewall no admite la tunelización forzada a una red local.<br><br>Sin embargo, si la configuración requiere la tunelización forzada a una red local, Microsoft proporcionará soporte según el caso. Póngase en contacto con soporte técnico para que podamos revisar su caso. Si acepta, permitiremos su suscripción y nos aseguraremos de que se mantenga la conectividad del firewall a Internet requerida.|

<sup>1</sup>Si necesita aumentar estos límites, póngase en contacto con el soporte técnico de Azure.
