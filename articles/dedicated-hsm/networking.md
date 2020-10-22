---
title: 'Consideraciones de redes: Azure Dedicated HSM | Microsoft Docs'
description: Introducción a las consideraciones de redes aplicables a las implementaciones de Azure Dedicated HSM
services: dedicated-hsm
author: msmbaldwin
manager: rkarlin
ms.custom: mvc, seodec18
ms.service: key-vault
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 12/06/2018
ms.author: mbaldwin
ms.openlocfilehash: 3764b261b491c660da16d7989be20742fead1fbf
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "91359161"
---
# <a name="azure-dedicated-hsm-networking"></a>Redes de Azure Dedicated HSM

Azure Dedicated HSM requiere un entorno de red muy seguro. Esto es así tanto desde la nube de Azure al entorno de TI del cliente (local) con aplicaciones distribuidas como para escenarios de alta disponibilidad. Las redes de Azure proporcionan esto y existen cuatro áreas distintas que deben abordarse.

- Creación de dispositivos HSM dentro de la red virtual (VNet) en Azure
- Conexión local a recursos basados en la nube para la configuración y administración de dispositivos HSM
- Creación y conexión de redes virtuales para la interconexión de recursos de aplicación y dispositivos HSM
- Conexión de redes virtuales entre regiones para la intercomunicación y también para habilitar escenarios de alta disponibilidad

## <a name="virtual-network-for-your-dedicated-hsms"></a>Red virtual para los HSM dedicados

Los HSM dedicados se integran en una red virtual y se colocan en la propia red privada de los clientes en Azure. Esto permite acceder a los dispositivos desde máquinas virtuales o recursos de proceso en la red virtual.  
Para más información sobre la integración de servicios de Azure en la red virtual y las funcionalidades que ofrece, consulte la documentación sobre [Red virtual para servicios de Azure](../virtual-network/virtual-network-for-azure-services.md).

### <a name="virtual-networks"></a>Redes virtuales

Antes de aprovisionar un dispositivo HSM dedicado, los clientes primero deberán crear una red virtual en Azure o utilizar una que ya exista en la suscripción de los clientes. La red virtual define el perímetro de seguridad para el dispositivo HSM dedicado. Para más información acerca de cómo crear redes virtuales, consulte la [documentación de redes virtuales](../virtual-network/virtual-networks-overview.md).

### <a name="subnets"></a>Subredes

Las subredes segmentan la red virtual en espacios de direcciones independientes utilizables por los recursos de Azure que coloque en ellos. Los HMS dedicados se implementan en una subred que se encuentra en la red virtual. Cada dispositivo HSM dedicado que se implementa en la subred del cliente recibirá una dirección IP privada de esta subred. La subred en la que se implementa el dispositivo HSM debe delegarse explícitamente al servicio: Microsoft.HardwareSecurityModules/dedicatedHSMs. Esto concede determinados permisos al servicio HSM para la implementación en la subred. La delegación a HSM dedicados impone ciertas restricciones de directiva en la subred. Actualmente no se admiten grupos de seguridad de red (NSG) ni rutas definidas por el usuario (UDR) en las subredes delegadas. En consecuencia, una vez que una subred se delega a HSM dedicados, solo puede usarse para implementar recursos HSM. Al implementar cualquier otro recurso de cliente en la subred se producirá un error.


### <a name="expressroute-gateway"></a>Puerta de enlace de ExpressRoute

Un requisito de la arquitectura actual es la configuración de una puerta de enlace de ExpressRoute en la subred de los clientes en la que debe colocarse un dispositivo HSM para permitir la integración del dispositivo HSM en Azure. Esta puerta de enlace de ExpressRoute no se puede utilizar para conectar ubicaciones locales a los dispositivos HSM de los clientes en Azure.

## <a name="connecting-your-on-premises-it-to-azure"></a>Conexión del entorno de TI local a Azure

Al crear recursos basados en la nube, este suele ser un requisito típico para una conexión privada a los recursos de TI locales. En el caso de un HSM dedicado, esto servirá principalmente para que el software cliente de HSM configure los dispositivos HSM y también para actividades como copias de seguridad y extracción de registros de HSM para su análisis. Una decisión clave aquí es la naturaleza de la conexión, ya que hay diversas opciones.  La opción más flexible es VPN de sitio a sitio, ya que probablemente habrá varios recursos locales que requieran una comunicación segura con los recursos (incluidos los HSM) de la nube de Azure. Esto requerirá que la organización del cliente disponga de un dispositivo VPN para facilitar la conexión. Es posible usar una conexión VPN de punto a sitio si hay solo un único punto de conexión en el entorno local, como una única estación de trabajo de administración.
Para más información sobre las opciones de conectividad, consulte [Opciones de planificación de puerta de enlace de VPN](../vpn-gateway/vpn-gateway-about-vpngateways.md?toc=%2fazure%2fvirtual-network%2ftoc.json#planningtable).

> [!NOTE]
> En este momento, ExpressRoute no es una opción para la conexión a los recursos del entorno local. Asimismo, debe tenerse en cuenta que la puerta de enlace de ExpressRoute usada como se describe anteriormente no sirve para conexiones a la infraestructura local.

### <a name="point-to-site-vpn"></a>VPN de punto a sitio

Una red privada virtual de punto a sitio es la forma más sencilla de establecer una conexión segura a un único punto de conexión en el entorno local. Esta puede ser la solución adecuada si piensa tener solo una estación de trabajo de administración para HSM dedicados basados en Azure.

### <a name="site-to-site-vpn"></a>VPN de sitio a sitio

Una red privada virtual de sitio a sitio permite una comunicación segura entre los HSM dedicados basados en Azure y su entorno de TI local. Una razón para optar por esta solución es si dispone de un centro de copia de seguridad para los HSM en el entorno local y necesita una conexión entre ambos para ejecutar la copia de seguridad.

## <a name="connecting-virtual-networks"></a>Conexión de redes virtuales

Una arquitectura típica de implementación de HSM dedicados empezará con una única red virtual y la subred correspondiente en la que se crearon y aprovisionaron los dispositivos HSM. Dentro de esa misma región, podría haber redes virtuales adicionales y subredes para componentes de aplicación que podrían hacer uso de los HSM dedicados. Para habilitar la comunicación a través de estas redes, usamos el emparejamiento de redes virtuales.

### <a name="virtual-network-peering"></a>Emparejamiento de redes virtuales de Azure

Cuando hay varias redes virtuales dentro de una región que necesitan acceso a los recursos de las demás, es posible usar el emparejamiento de redes virtuales para crear los canales de comunicación seguros entre ellas.  El emparejamiento de redes virtuales no solo proporciona comunicaciones seguras, sino que también garantiza conexiones de baja latencia y alto ancho de banda entre los recursos de Azure.

![emparejamiento de redes](media/networking/peering.png)

## <a name="connecting-across-azure-regions"></a>Conexión a través de las regiones de Azure

Los dispositivos HSM tienen la capacidad, mediante bibliotecas de software, de redirigir el tráfico a un HSM alternativo. El redireccionamiento del tráfico es útil si se produce un error en los dispositivos o se pierde el acceso a un dispositivo. Los escenarios de errores a nivel regional se pueden mitigar mediante la implementación de HSM en otras regiones y la habilitación de la comunicación entre redes virtuales entre regiones.

### <a name="cross-region-ha-using-vpn-gateway"></a>Alta disponibilidad entre regiones mediante VPN Gateway

Para aplicaciones distribuidas globalmente o para escenarios de conmutación por error regional de alta disponibilidad, es necesario conectar redes virtuales entre regiones. Con Azure Dedicated HSM, se puede lograr alta disponibilidad mediante el uso de VPN Gateway, que proporciona un túnel seguro entre las dos redes virtuales. Para más información sobre las conexiones de red virtual a red virtual mediante VPN Gateway, consulte el artículo titulado [¿Qué es VPN Gateway?](../vpn-gateway/design.md#V2V)

> [!NOTE]
> El emparejamiento de red virtual global no está disponible en escenarios de conectividad entre regiones con HSM dedicados en este momento y, en su lugar, debe utilizarse VPN Gateway. 

![En el diagrama se muestran dos regiones conectadas por dos puertas de enlace V P N. Cada región contiene redes virtuales emparejadas.](media/networking/global-vnet.png)

## <a name="next-steps"></a>Pasos siguientes

- [Preguntas más frecuentes](faq.md)
- [Compatibilidad](supportability.md)
- [Alta disponibilidad](high-availability.md)
- [Seguridad física](physical-security.md)
- [Supervisión](monitoring.md)
- [Arquitectura de implementación](deployment-architecture.md)