---
title: Opciones de arquitectura de Azure Firewall Manager
description: Compare y contraste el empleo de arquitecturas de red virtual de centro o de centro virtual protegido con Azure Firewall Manager.
author: vhorne
ms.service: firewall-manager
services: firewall-manager
ms.topic: article
ms.date: 02/18/2020
ms.author: victorh
ms.openlocfilehash: b946a360ced05500a4ef89cda7c623d8ae16658e
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "77444869"
---
# <a name="what-are-the-azure-firewall-manager-architecture-options"></a>Opciones de arquitectura de Azure Firewall Manager

Azure Firewall Manager puede proporcionar administración de seguridad para dos tipos de arquitectura de red:

- **Centro virtual protegido**

   Un [centro de conectividad de Azure Virtual WAN](../virtual-wan/virtual-wan-about.md#resources) es un recurso administrado por Microsoft que permite crear fácilmente arquitecturas en estrella tipo hub-and-spoke. Cuando las directivas de seguridad y enrutamiento están asociadas a un centro de ese tipo, se conoce como *[centro virtual protegido](secured-virtual-hub.md)* . 
- **Red virtual de centro de conectividad**

   Se trata de una red virtual estándar de Azure que crea y administra uno mismo. Cuando las directivas de seguridad están asociadas con este tipo de centro, se conoce como *red virtual de centro*. En este momento, solo se admite la directiva de Azure Firewall. Puede emparejar las redes virtuales de radio que contienen los servidores y servicios de carga de trabajo. También puede administrar los firewalls de redes virtuales independientes que no están emparejadas con ningún radio.

## <a name="comparison"></a>De comparación

En la tabla siguiente se comparan estas dos opciones de arquitectura, lo que puede ayudarle a decidir cuál es la adecuada para los requisitos de seguridad de la organización:


|  |**Red virtual de centro**|**Centro virtual protegido**  |
|---------|---------|---------|
|**Recurso subyacente**     |Virtual network|Centro de Virtual WAN|
|**Centro y radio**     |Usa emparejamiento de red virtual|Automatizado mediante conexión de red virtual de centro|
|**Conectividad local**     |VPN Gateway de hasta 10 Gbps y 30 conexiones S2S; ExpressRoute|VPN Gateway más escalable de hasta 20 Gbps y 1000 conexiones S2S; ExpressRoute|
|**Conectividad de rama automatizada mediante SDWAN**      |No compatible|Compatible|
|**Centros por región**     |Varias redes virtuales por región|Único centro virtual por región. Varios centros posibles con varias WAN virtuales|
|**Azure Firewall: varias direcciones IP públicas**      |Proporcionado por el cliente|Generado automáticamente. Va a estar disponible mediante GA.|
|**Availability Zones de Azure Firewall**     |Compatible|No disponible en versión preliminar. Va a estar disponible mediante GA|
|**Seguridad avanzada de Internet con seguridad de terceros como asociados de servicio**     |Conectividad VPN administrada y establecida por el cliente con el servicio asociado que se prefiera|Automatizada a través de la experiencia de administración de asociados y el flujo de asociado de seguridad de confianza|
|**Administración de rutas centralizada para enrutar el tráfico al centro**     |Ruta definida por el usuario administrada por el cliente|Se admite con BGP|
|**Firewall de aplicaciones web en Application Gateway** |Se admite en Virtual Network|Actualmente se admite en redes de radio|
|**Aplicación virtual de red**|Se admite en Virtual Network|Actualmente se admite en redes de radio|

## <a name="next-steps"></a>Pasos siguientes

- Consulte [Información general sobre la implementación de Azure Firewall Manager (versión preliminar)](deployment-overview.md)
- Información sobre [centros de conectividad virtuales protegidos](secured-virtual-hub.md).