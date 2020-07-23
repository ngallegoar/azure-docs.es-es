---
title: 'Conceptos: interconectividad de red'
description: Conozca los aspectos clave y los casos de uso de redes e interconectividad en la solución de VMware en Azure (AVS)
ms.topic: conceptual
ms.date: 05/04/2020
ms.openlocfilehash: 2378ad56e2754b2d2fde7f895f6673e7d7d561c9
ms.sourcegitcommit: 3543d3b4f6c6f496d22ea5f97d8cd2700ac9a481
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/20/2020
ms.locfileid: "86539149"
---
# <a name="azure-vmware-solution-avs-preview-networking-and-interconnectivity-concepts"></a>Conceptos de interconectividad y redes de la versión preliminar de la solución de VMware en Azure (AVS)

La interconectividad de red entre las nubes privadas de la solución de VMware en Azure (AVS) y los entornos locales o las redes virtuales de Azure le permite acceder a su nube privada y utilizarla. En este artículo se describen algunos conceptos fundamentales de redes y de interconectividad que establecen la base de la interconectividad.

Una perspectiva útil sobre la interconectividad es tener en cuenta los dos tipos de implementaciones de la nube privada de AVS: implementaciones con interconectividad básica solo de Azure e implementaciones con interconectividad completa del entorno local a la nube privada.

Los casos de uso de nubes privadas de AVS incluyen:
- nuevas cargas de trabajo de máquinas virtuales de VMware en la nube
- ráfagas de carga de trabajo de máquinas virtuales a la nube
- migración de carga de trabajo de máquinas virtuales a la nube
- recuperación ante desastres
- consumo de servicios de Azure

 Todos los casos de uso del servicio AVS se habilitan con conectividad del entorno local a la nube privada. El modelo de interconectividad básica es más adecuado para las evaluaciones o implementaciones de AVS que no requieren acceso desde entornos locales.

Los dos tipos de interconectividad de nube privada de AVS se describen en las secciones siguientes.  La interconectividad más básica se muestra en "Conectividad de la red virtual de Azure" y le permite administrar y usar su nube privada con una sola red virtual en Azure. La interconectividad descrita en "Interconectividad local" amplía la conectividad de la red virtual para incluir igualmente la interconectividad entre los entornos locales y las nubes privadas de AVS.

## <a name="azure-virtual-network-interconnectivity"></a>Conectividad de la red virtual de Azure

En el diagrama siguiente se muestra la interconectividad de red básica que se establece al implementar una nube privada. Muestra las redes lógicas basadas en ExpressRoute entre una red virtual de Azure y una nube privada. La interconectividad cumple tres de los casos de uso principales:
- Acceso de entrada a las redes de administración donde se encuentran vCenter Server y NSX-T Manager.
    - Accesible desde las máquinas virtuales de su suscripción de Azure y no desde sus sistemas locales.
- Acceso de salida desde las máquinas virtuales a los servicios de Azure.
- Acceso de entrada y consumo de cargas de trabajo que ejecutan una nube privada.

![Conexión de una red virtual básica a una nube privada](./media/concepts/adjacency-overview-drawing-single.png)

El circuito de ExpressRoute en este escenario de red virtual en la nube privada se establece cuando se crea una conexión desde una red virtual de su suscripción con el circuito de ExpressRoute de la nube privada. El emparejamiento usa una clave de autorización y un identificador de circuito que solicita en Azure Portal. La conexión de ExpressRoute que se establece mediante el emparejamiento es una conexión privada de uno a uno entre su nube privada y la red virtual. Puede administrar su nube privada, consumir cargas de trabajo en la nube privada y acceder a los servicios de Azure mediante esta conexión de ExpressRoute.

Al implementar una nube privada de AVS, se requiere un espacio de direcciones de red privada única /22. Este espacio de direcciones no debe superponerse sobre los espacios de direcciones usados en otras redes virtuales de la suscripción. Dentro de este espacio de direcciones, la administración, el aprovisionamiento y las redes de vMotion se aprovisionan automáticamente. El enrutamiento se basa en BGP, y se aprovisiona y habilita automáticamente de forma predeterminada para cada implementación de nube privada.

Cuando se implementa una nube privada, se le proporcionarán las direcciones IP de vCenter y NSX-T Manager. Para acceder a estas interfaces de administración, creará recursos adicionales en una red virtual de su suscripción. En los tutoriales se proporcionan los procedimientos para crear esos recursos y establecer el emparejamiento privado de ExpressRoute.

Diseña la red lógica de la nube privada y la implementa con NSX-T. La nube privada incluye NSX-T previamente aprovisionado. Se le han aprovisionado previamente una puerta de enlace de nivel 0 y una puerta de enlace de nivel 1. Puede crear un segmento y asociarlo a la puerta de enlace de nivel 1 existente o asociarla a una nueva puerta de enlace de nivel 1 que puede definir. Los componentes de redes lógicas NSX-T proporcionan conectividad horizontal de derecha a izquierda entre cargas de trabajo y también proporcionan conectividad vertical de arriba abajo a Internet y a los servicios de Azure. 

## <a name="on-premises-interconnectivity"></a>Interconectividad local

Puede conectar igualmente entornos locales a sus nubes privadas de AVS. Este tipo de interconectividad es una extensión de la interconectividad básica descrita en la sección anterior.

![red virtual y conectividad de nube privada local completa](./media/concepts/adjacency-overview-drawing-double.png)

Para establecer una interconectividad completa con una nube privada, use Azure Portal para habilitar ExpressRoute Global Reach entre un circuito de ExpressRoute de la nube privada y un circuito de ExpressRoute local. Esta configuración amplía la conectividad básica e incluye el acceso a nubes privadas desde entornos locales.

Se requiere un circuito de ExpressRoute desde el entorno local a la red virtual de Azure para conectarse desde entornos locales a su nube privada de Azure. Este circuito de ExpressRoute está en su suscripción y no forma parte de una implementación de nube privada. El circuito de ExpressRoute local está fuera del ámbito de este documento. Si necesita una conectividad desde el entorno local a su nube privada, puede utilizar uno de los circuitos de ExpressRoute existentes o comprar uno en Azure Portal.

Una vez vinculados con Global Reach, los dos circuitos de ExpressRoute enrutarán el tráfico de red entre los entornos locales y su nube privada. La interconectividad desde entornos locales a una nube privada se muestra en el diagrama anterior. La interconectividad representada en el diagrama habilita los siguientes casos de uso:

- Cross-vCenter vMotion frecuente/poco frecuente
- Acceso de administración desde los entornos locales a la nube privada de AVS

Para habilitar la conectividad completa, se puede solicitar una clave de autorización y un identificador de emparejamiento privado para Global Reach en Azure Portal. Utilice la clave y el identificador para establecer Global Reach entre un circuito de ExpressRoute en su suscripción y el circuito de ExpressRoute para su nueva nube privada. En el [tutorial para crear una nube privada](tutorial-create-private-cloud.md) se indican los procedimientos para solicitar y usar la clave y el identificador.

Los requisitos de enrutamiento de la solución requieren la planeación de espacios de direcciones de red de la nube privada para evitar superposiciones con otras redes virtuales y redes locales. Las nubes privadas AVS requieren un mínimo de un bloque de direcciones de red CIDR `/22` para las subredes, que se muestran a continuación. Esta red complementa las redes locales. Para conectarse a entornos locales y redes virtuales, debe ser un bloque de direcciones de red no superpuestas.

Un ejemplo de bloque de direcciones de red CIDR `/22` es `10.10.0.0/22`

Las subredes:

| Uso de red             | Subnet | Ejemplo        |
| ------------------------- | ------ | -------------- |
| Administración de la nube privada            | `/24`    | `10.10.0.0/24`   |
| Red vMotion       | `/24`    | `10.10.1.0/24`   |
| Cargas de trabajo de máquinas virtuales | `/24`   | `10.10.2.0/24`   |
| Emparejamiento de ExpressRoute | `/24`    | `10.10.3.8/30`   |

## <a name="next-steps"></a>Pasos siguientes 

El siguiente paso es obtener información sobre los [conceptos de almacenamiento de nubes privadas](concepts-storage.md).

<!-- LINKS - external -->
[enable Global Reach]: ../expressroute/expressroute-howto-set-global-reach.md

<!-- LINKS - internal -->
