---
title: VMware Solution by CloudSimple - Conexiones de red de Azure
description: 'Aprenda a conectar su red virtual de Azure con la red de su región de CloudSimple:'
author: sharaths-cs
ms.author: dikamath
ms.date: 04/10/2019
ms.topic: article
ms.service: azure-vmware-cloudsimple
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: cfd4d65b07cf255ac2b60d6bf8376723a997374e
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "77025085"
---
# <a name="azure-network-connections-overview"></a>Introducción a las conexiones de red de Azure

Cuando se crea un servicio CloudSimple en una región y se crean nodos, se puede:

* Solicitar un circuito de Azure ExpressRoute y asociarlo a la red CloudSimple de esa región.
* Conectar la red de la región de CloudSimple a la red virtual de Azure o la red local mediante Azure ExpressRoute.
* Proporcionar acceso a servicios que se ejecutan en la suscripción de Azure o en la red local desde el entorno de nube privada.

La conexión de ExpressRoute es de ancho de banda alto con baja latencia.

## <a name="benefits"></a>Ventajas

La conexión de red de Azure le permite:

* Uso de Azure como destino de copia de seguridad para máquinas virtuales en su nube privada.
* Implementar servidores KMS en su suscripción de Azure para cifrar el almacén de datos vSAN de la nube privada.
* Usar aplicaciones híbridas donde el nivel de web de la aplicación se ejecuta en la nube pública mientras la aplicación y los niveles de base de datos se ejecutan en su nube privada.

## <a name="azure-virtual-network-connection"></a>Conexión de red virtual de Azure

Las nubes privadas se pueden conectar a los recursos de Azure mediante ExpressRoute.  La conexión de ExpressRoute permite acceder a recursos que se ejecutan en la suscripción de Azure desde la nube privada.  Esta conexión permite extender la red de nube privada a la red virtual de Azure.  Las rutas de la red CloudSimple se intercambian con la red virtual de Azure a través de BGP.  Si tiene configurado el emparejamiento de red virtual, se puede acceder a todas las redes virtuales emparejadas desde la red CloudSimple.

![Conexión de Azure ExpressRoute a la red virtual](media/cloudsimple-azure-network-connection.png)

## <a name="expressroute-connection-to-on-premises-network"></a>Conexión de ExpressRoute a la red local

Puede conectar el circuito de Azure ExpressRoute existente a su región de CloudSimple. La característica Global Reach de ExpressRoute se utiliza para conectar los dos circuitos entre sí.  Se establece una conexión entre el circuito local y el circuito de ExpressRoute de CloudSimple.  Esta conexión le permite ampliar su red local a la red de nube privada. Las rutas de la red CloudSimple se intercambian a través de BGP con la red local.

![Conexión de ExpressRoute local - Global Reach](media/cloudsimple-global-reach-connection.png)

## <a name="connection-to-on-premises-network-and-azure-virtual-network"></a>Conexión a la red local y a la red virtual de Azure

Las conexiones a la red local y a la red virtual de Azure pueden coexistir desde la red CloudSimple.  La conexión usa BGP para intercambiar rutas entre la red local, la red virtual de Azure y la red CloudSimple.  Al conectar la red CloudSimple a la red virtual de Azure en presencia de la conexión de Global Reach, las rutas de la red virtual de Azure están visibles en la red local.  El intercambio de ruta se produce en Azure entre los enrutadores perimetrales.

![Conexión local de ExpressRoute con conexión de red virtual de Azure](media/cloudsimple-global-reach-and-vnet-connection.png)

### <a name="important-considerations"></a>Consideraciones importantes

La conexión a la red CloudSimple desde la red local y desde la red virtual de Azure permite el intercambio de rutas entre todas las redes.

* La red virtual de Azure es visible desde la red local y la red CloudSimple.
* Si se ha conectado a la red virtual de Azure desde la red local, la conexión a la red CloudSimple mediante Global Reach permite el acceso a redes virtuales desde la red CloudSimple.
* Las direcciones de subred **no deben** superponerse entre ninguna de las redes conectadas.
* CloudSimple **no** anuncia la ruta predeterminada para las conexiones de ExpressRoute
* Si el enrutador local anuncia la ruta predeterminada, el tráfico de la red CloudSimple y de la red virtual de Azure usa la ruta predeterminada anunciada.  Como resultado, no se puede acceder a las máquinas virtuales de Azure mediante direcciones IP públicas.

## <a name="next-steps"></a>Pasos siguientes

* [Conexión de red virtual de Azure a CloudSimple mediante ExpressRoute](virtual-network-connection.md)
* [Conexión desde una red local a CloudSimple mediante ExpressRoute](on-premises-connection.md)
