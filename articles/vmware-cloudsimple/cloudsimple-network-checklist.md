---
title: 'Azure VMware Solution by CloudSimple: lista de comprobación de red'
description: Lista de comprobación para la asignación de CIDR de red en Azure VMware Solution by CloudSimple
author: sharaths-cs
ms.author: dikamath
ms.date: 09/25/2019
ms.topic: article
ms.service: azure-vmware-cloudsimple
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: bfb170036293dc9f519259dc92737f30380aa84a
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "77025017"
---
# <a name="networking-prerequisites-for-azure-vmware-solution-by-cloudsimple"></a>Requisitos previos de redes para Azure VMware Solution by CloudSimple

Azure VMware Solution by CloudSimple ofrece un entorno de nube privada de VMware al que los usuarios y las aplicaciones pueden acceder desde entornos locales, dispositivos administrados por la empresa y recursos de Azure. La conectividad se proporciona a través de servicios de red, como las VPN y las conexiones de Azure ExpressRoute. Algunos de estos servicios de red requieren que especifique los intervalos de direcciones de red para su habilitación. 

En las tablas de este artículo se describe el conjunto de intervalos de direcciones y los servicios correspondientes que usan las direcciones especificadas. Algunas de las direcciones son obligatorias y otras dependen de los servicios que desea implementar. Estos espacios de direcciones no deben superponerse con ninguna de las subredes locales, las subredes de Azure Virtual Network o las subredes de carga de trabajo de CloudSimple planeadas.

## <a name="network-address-ranges-required-for-creating-a-private-cloud"></a>Intervalos de direcciones de red necesarios para crear una nube privada

Durante la creación de un servicio CloudSimple y una nube privada, debe cumplir los intervalos de enrutamiento de interdominios sin clases (CIDR) de red especificados, como se indica a continuación.

| Nombre/usado para     | Descripción                                                                                                                            | Intervalo de direcciones            |
|-------------------|----------------------------------------------------------------------------------------------------------------------------------------|--------------------------|
| CIDR de puerta de enlace      | Se requiere para los servicios perimetrales (puertas de enlace de VPN).  Este CIDR es necesario durante la creación del servicio CloudSimple y debe proceder del espacio RFC 1918. | /28                      |
| CIDR de vSphere/vSAN | Se requiere para las redes de administración de VMware. Este CIDR debe especificarse durante la creación de una nube privada.                                    | /24 o /23 o /22 o /21 |

## <a name="network-address-range-required-for-azure-network-connection-to-an-on-premises-network"></a>Intervalo de direcciones de red necesario para la conexión de red de Azure a una red local

La conexión desde una [red local a la red de la nube privada mediante ExpressRoute](on-premises-connection.md) establece una conexión Global Reach.  La conexión usa el protocolo de puerta de enlace de borde (BGP) para intercambiar las rutas entre la red local, la red de la nube privada y las redes de Azure.

| Nombre/usado para             | Descripción                                                                                                                                                                             | Intervalo de direcciones |
|---------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|---------------|
| CIDR de emparejamiento de ExpressRoute | Se requiere cuando se usa Global Reach de ExpressRoute para la conectividad local. Este CIDR debe proporcionarse cuando se realiza una solicitud de conexión Global Reach a través de una incidencia de soporte técnico. | /29           |

## <a name="network-address-range-required-for-using-a-site-to-site-vpn-connection-to-an-on-premises-network"></a>Intervalo de direcciones de red necesario para usar una conexión VPN de sitio a sitio a una red local

La conexión desde la [red local a la red de la nube privada mediante una VPN de sitio a sitio](vpn-gateway.md) requiere las siguientes direcciones IP, una red local e identificadores. 

| Dirección o intervalo de direcciones | Descripción                                                                                                                                                                                                                                                           |
|-----------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Dirección IP de par               | Dirección IP pública de la puerta de enlace de VPN local. Se requiere para establecer una conexión VPN de sitio a sitio entre el centro de datos local y la región del servicio de CloudSimple. Esta dirección IP es necesaria durante la creación de una puerta de enlace de VPN de sitio a sitio.                                         |
| Identificador de par       | Identificador de par de la puerta de enlace de VPN local. Normalmente, es lo mismo que la **dirección IP de par**.  Si se especifica un identificador único en la puerta de enlace de VPN local, debe especificarse el identificador.  Este identificador es necesario durante la creación de la puerta de enlace de VPN de sitio a sitio.   |
| Redes locales   | Prefijos de redes locales que necesitan acceso a redes de CloudSimple de la región.  Indique todos los prefijos de una red local que accederán a la red de CloudSimple, incluida la red de cliente desde la que los usuarios accederán a la red.                                         |

## <a name="network-address-range-required-for-using-point-to-site-vpn-connections"></a>Intervalo de direcciones de red necesario para usar conexiones VPN de punto a sitio.

Una conexión VPN de punto a sitio permite el acceso a la red de CloudSimple desde una máquina cliente.  [Para configurar una VPN de punto a sitio](vpn-gateway.md), debe especificar el siguiente intervalo de direcciones de red.

| Dirección o intervalo de direcciones | Descripción                                                                                                                                                                                                                                                                                                  |
|-----------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Subred de cliente         | Las direcciones DHCP las proporciona la subred del cliente cuando se conecte mediante una VPN de punto a sitio. Esta subred se requiere al crear una puerta de enlace de VPN de punto a sitio en un portal de CloudSimple.  La red se divide en dos subredes: una para la conexión UDP y otra para las conexiones TCP. |

## <a name="next-steps"></a>Pasos siguientes

* [Configuración del firewall local para acceder a una nube privada](on-premises-firewall-configuration.md)
* [Inicio rápido: Creación del servicio CloudSimple](quickstart-create-cloudsimple-service.md)
* [Inicio rápido: Configuración de una nube privada](quickstart-create-private-cloud.md)
* Más información sobre las [conexiones de red de Azure](cloudsimple-azure-network-connection.md)
* Más información sobre las [puertas de enlace de VPN](cloudsimple-vpn-gateways.md)
