---
title: Prefijo de dirección IPv6 pública en Azure Virtual Network
titlesuffix: Azure Virtual Network
description: Obtenga información sobre el prefijo de dirección IPv6 pública en Azure Virtual Network.
services: virtual-network
documentationcenter: na
author: KumudD
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/31/2020
ms.author: kumud
ms.openlocfilehash: ff33bf771065e7af209934a5c54b6f8f2e34835e
ms.sourcegitcommit: 67e8e1caa8427c1d78f6426c70bf8339a8b4e01d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/02/2020
ms.locfileid: "91666790"
---
# <a name="reserved-public-ipv6-address-prefix"></a>Prefijo de dirección IPv6 pública reservada

En Azure, las máquinas virtuales (VM) y las redes virtuales (VNet) de pila dual (IPv4+IPv6) son seguras de manera predeterminada, ya que no tienen conectividad a Internet. Puede agregar fácilmente una conectividad a Internet IPv6 a sus máquinas virtuales e instancias de Azure Load Balancer con direcciones IPv6 públicas que obtiene de Azure.

Todas las direcciones IP públicas que reserva están asociadas a una región de Azure de su preferencia y a su suscripción de Azure. Puede trasladar una IP pública IPv6 (estática) reservada entre cualquiera de las instancias de Azure Load Balancer o las máquinas virtuales de Azure de su suscripción. Puede desasociar la IP pública IPv6 por completo y se conservará para su uso cuando esté listo.

> [!WARNING]
> Tenga cuidado de no eliminar por accidente las IP públicas. La eliminación de una IP pública la quita de su suscripción y no podrá recuperarla (ni siquiera con la ayuda del Soporte técnico de Azure).

Además de reservar direcciones IPv6 individuales, puede reservar intervalos contiguos de direcciones IPv6 de Azure (conocidos como prefijo IP) para su uso.  De manera similar a las direcciones IP individuales, los prefijos reservados se asocian con una región de Azure de su preferencia y con su suscripción de Azure. La reserva de un intervalo de direcciones contiguo y predecible tiene muchos usos. Por ejemplo, puede simplificar en gran medida el *filtrado* de IP de las aplicaciones hospedadas en Azure por la empresa y los clientes, ya que los intervalos de direcciones IP estáticas se pueden programar fácilmente en los firewalls locales.  Puede crear IP públicas individuales a partir del prefijo IP según sea necesario y, cuando elimine esas IP públicas individuales, se *devuelven* al intervalo reservado para que pueda volver a usarlas más adelante. Todas las direcciones IP del prefijo IP están reservadas para su uso exclusivo hasta que se elimine el prefijo.



## <a name="ipv6-prefix-sizes"></a>Tamaños del prefijo IPv6
Están disponibles los tamaños de prefijo de IP pública siguientes:

-  Tamaño mínimo del prefijo IPv6: /127 = 2 direcciones
-  Tamaño máximo del prefijo IPv6: /124 = 16 direcciones

El tamaño del prefijo se especifica como un tamaño de máscara de enrutamiento de interdominios sin clases (CIDR). Por ejemplo, una máscara de /128 representa una dirección IPv6 individual, ya que las direcciones IPv6 están compuestas de 128 bits.

## <a name="pricing"></a>Precios
 
Para los costos asociados con el uso de direcciones IP públicas de Azure, tanto direcciones IP individuales como intervalos IP, consulte [Precios de las direcciones IP públicas](https://azure.microsoft.com/pricing/details/ip-addresses/).

## <a name="limitations"></a>Limitaciones
IPv6 solo se admite en IP públicas básicas con asignación "dinámica", lo que significa que la dirección IPv6 cambiará si elimina y vuelve a implementar la aplicación (máquinas virtuales o equilibradores de carga) en Azure. Las IP públicas estándar IPv6 admiten únicamente la asignación estática (reservada), aunque los equilibradores de carga internos estándar también pueden admitir la asignación dinámica desde dentro de la subred a la que están asignadas.  

Como procedimiento recomendado, se recomienda usar IP públicas estándar e instancias de Standard Load Balancer para las aplicaciones IPv6.

## <a name="next-steps"></a>Pasos siguientes
- Reserve un [prefijo de dirección IPv6](ipv6-reserve-public-ip-address-prefix.md) pública.
- Obtenga más información sobre las [direcciones IPv6](ipv6-overview.md).
- Obtenga información sobre [cómo crear y usar IP públicas](virtual-network-public-ip-address.md) (tanto IPv4 como IPv6) en Azure.
