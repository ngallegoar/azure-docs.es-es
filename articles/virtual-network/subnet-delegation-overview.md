---
title: ¿Qué es la delegación de subred en una red virtual de Azure?
description: Información acerca de la delegación de subred en una red virtual de Azure
services: virtual-network
documentationcenter: na
author: KumudD
manager: mtillman
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 11/20/2019
ms.author: kumud
ms.openlocfilehash: 1fbb683754aed5b2a2e6e9c022713b7e87ad9ba9
ms.sourcegitcommit: 32c521a2ef396d121e71ba682e098092ac673b30
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/25/2020
ms.locfileid: "91329214"
---
# <a name="what-is-subnet-delegation"></a>¿Qué es la delegación de subred?

La delegación de subred permite designar una subred concreta para el servicio PaaS de Azure que se prefiera e insertarla en una red virtual. La delegación de subred proporciona al cliente control total al cliente de la administración de la integración de los servicios de Azure en sus redes virtuales.

Al delegar una subred en un servicio de Azure, se permite que el servicio establezca algunas reglas básicas de configuración de red para la subred, lo que facilita que las instancias del servicio de Azure funcionen de manera estable. Como resultado, el servicio de Azure puede establecer algunas condiciones previas o posteriores a la implementación, como:
- implementar el servicio en una subred compartida, en lugar de una dedicada.
- agregar al servicio un conjunto de directivas de intenciones de red después de la implementación que se requiere para que el servicio funcione correctamente.

##  <a name="advantages-of-subnet-delegation"></a>Ventajas de la delegación de subred

La delegación de una subred a servicios concretos aporta las siguientes ventajas:

- ayuda a designar una subred para uno o varios servicios de Azure y a administrar las instancias de la subred en función de los requisitos. Por ejemplo, el propietario de la red virtual puede definir lo siguientes elementos para una subred delegada, con el fin de administrar mejor los recursos y el acceso como se indica a continuación:
    - Directivas de tráfico de filtrado de red con grupos de seguridad de red.
    - Directivas de enrutamiento con rutas definidas por el usuario.
    - Integración de servicios con configuraciones de puntos de conexión de servicio.
- Ayuda a los servicios insertados a integrarse mejor con la red virtual mediante la definición de las condiciones previas de las implementaciones, en forma de directivas de intenciones de red. Esto garantiza que cualquier acción que pueda afectar al funcionamiento del servicio insertado se puede bloquear en PUT.


## <a name="who-can-delegate"></a>¿Quién puede delegar?
La delegación de subred es un ejercicio que los propietarios de redes virtuales deben realizar para designar una de las subredes para un servicio de Azure concreto. A su vez, el servicio de Azure implementa las instancias en esta subred para que las consuman las cargas de trabajo de los clientes.

## <a name="impact-of-subnet-delegation-on-your-subnet"></a>Impacto de la delegación de subred en una subred
Cada servicio de Azure define su propio modelo de implementación, en el que se puede especificar las propiedades que admite, o no, en una subred delegada a la hora de la inserción, tal como se indica a continuación:
- subred compartida con otros servicios de Azure o con un conjunto de escalado de máquinas virtuales o máquinas virtuales que estén en la misma subred, o bien solo admite una subred dedicada que tenga únicamente instancias de este servicio.
- Admite la asociación de grupos de seguridad de red con la subred delegada.
- Admite que un grupo de seguridad de red que esté asociado a la subred delegada también pueda estarlo a cualquier otra subred.
- Permite la asociación de la tabla de rutas con la subred delegada.
- Permite que la tabla de rutas asociada con la subred delegada se asocie con cualquier otra subred.
- Dicta el número mínimo de direcciones IP en la subred delegada.
- Dicta que el espacio de direcciones IP de la subred delegada sea del espacio de direcciones IP privadas (10.0.0.0/8, 192.168.0.0/16, 172.16.0.0/12).
- Dicta que la configuración de DNS personalizada tenga una entrada Azure DNS.
- Requiere quitar la delegación antes de que se pueda eliminar la subred o la red virtual.

Los servicios insertados también pueden agregar sus propias directivas, como las siguientes:
- **Directivas de seguridad**: colección de reglas de seguridad necesarias para el funcionamiento de un servicio determinado.
- **Directivas de rutas**: colección de rutas necesarias para el funcionamiento de un servicio dado.

## <a name="what-subnet-delegation-does-not-do"></a>Qué es lo que no hace la delegación de subred

Los servicios de Azure que se insertan en una subred delegada tienen el conjunto básico de propiedades, que está disponible para las subredes no delegadas, como:
-  Los servicios de Azure pueden insertar instancias en las subredes de los clientes, pero no pueden afectar a las cargas de trabajo existentes.
-  Las directivas o rutas que aplican estos servicios son flexibles y las pueden reemplazar el cliente.

## <a name="next-steps"></a>Pasos siguientes

- [Delegación de una subred](manage-subnet-delegation.md)
