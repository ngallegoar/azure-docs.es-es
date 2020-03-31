---
title: Introducción a las redes de Azure Service Fabric
description: Obtenga información sobre redes, puertas de enlace y enrutamiento del tráfico inteligente en Service Fabric Mesh.
author: dkkapur
ms.topic: conceptual
ms.date: 11/26/2018
ms.author: dekapur
ms.custom: mvc, devcenter
ms.openlocfilehash: dc793e2991783cc9b7b46d92fcc8e0267feb529b
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "75459135"
---
# <a name="introduction-to-networking-in-service-fabric-mesh-applications"></a>Introducción a las redes en las aplicaciones de Service Fabric Mesh
En este artículo se describen los diferentes tipos de equilibradores de carga, cómo se conectan las puertas de enlace a la red con sus aplicaciones a otras redes y cómo se enruta el tráfico entre los servicios en sus aplicaciones.

## <a name="layer-4-vs-layer-7-load-balancers"></a>Equilibradores de carga de capa 4 frente a capa 7
El equilibrio de carga de carga puede realizarse en diferentes capas en el [modelo OSI](https://en.wikipedia.org/wiki/OSI_model) para las redes, a menudo en la capa 4 (L4) y en la capa 7 (L7).  Normalmente, hay dos tipos de equilibradores de carga:

- Un equilibrador de carga L4 funciona en el nivel de transporte de red, que se ocupa de la entrega de paquetes sin tener en cuenta el contenido de los mismos. El equilibrador de carga solo inspecciona los encabezados de los paquetes, por lo que los criterios de equilibrio se limitan a los puertos y las direcciones IP. Por ejemplo, un cliente realiza una conexión TCP con el equilibrador de carga. El equilibrador de carga termina la conexión (al responder directamente a SYN), selecciona un back-end y realiza una conexión TCP nueva con el back-end (envía un nuevo SYN). Un equilibrador de carga L4 generalmente funciona solo en el nivel de la sesión o conexión L4 TCP/UDP. Por lo tanto, el equilibrio de carga redirige los bytes y se asegura de que los bytes de la misma sesión terminan en el mismo back-end. El equilibrador de carga L4 no reconoce los detalles de los bytes de la aplicación que se va a mover. Los bytes pueden ser cualquier protocolo de aplicación.

- Un equilibrador de carga L7 funciona en el nivel de aplicación, que se ocupa del contenido de cada paquete. Inspecciona el contenido de los paquetes porque reconoce los protocolos, como HTTP, HTTPS o WebSockets. Esto permite al equilibrador de carga realizar un enrutamiento avanzado. Por ejemplo, un cliente realiza una única conexión TCP HTTP/2 con el equilibrador de carga. El equilibrador de carga entonces procede a realizar dos conexiones de back-end. Cuando el cliente envía dos secuencias HTTP/2 al equilibrador de carga, la primera secuencia se envía al primer back-end y la segunda secuencia se envía al segundo back-end. Por tanto, incluso la multiplexación de clientes que tienen cargas de solicitud muy diferentes se equilibrará eficazmente en los back-ends. 

## <a name="networks-and-gateways"></a>Redes y puertas de enlace
En el [modelo de recursos de Service Fabric](service-fabric-mesh-service-fabric-resources.md), un recurso de red es un recurso que se puede implementar individualmente y es independiente de un recurso de aplicación o servicio al que pueda hacer referencia como dependencia. Sirve para crear una red para las aplicaciones que está abierta a Internet. Varios servicios de distintas aplicaciones pueden formar parte de la misma red. Esta red privada la crea y administra Service Fabric y no se trata de una red virtual (VNET) de Azure. Las aplicaciones se pueden agregar y quitar de forma dinámica en el recurso de red para habilitar y deshabilitar la conectividad a la red virtual. 

Una puerta de enlace se utiliza como puente entre dos redes. El recurso de puerta de enlace implementa un [proxy Envoy](https://www.envoyproxy.io/) que proporciona enrutamiento L4 para cualquier protocolo y enrutamiento L7 para el enrutamiento de aplicaciones HTTP(S) avanzado. La puerta de enlace enruta el tráfico a la red desde una red externa y determina a qué servicio enrutar el tráfico.  La red externa puede ser una red abierta (básicamente, la red pública de Internet) o una red virtual de Azure, lo que le permite conectarse con sus otras aplicaciones y recursos de Azure. 

![Red y puerta de enlace][Image1]

Cuando se crea el recurso de red con `ingressConfig`, se asigna una dirección IP pública al recurso de red. La dirección IP pública estará vinculada a la duración del recurso de red.

Cuando se crea una aplicación de malla, debe hacer referencia a un recurso de red existente. Se pueden agregar nuevos puertos públicos o se pueden quitar puertos existentes de la configuración de entrada. Se producirá un error al eliminar un recurso de red en caso de que un recurso de aplicación haga referencia a él. Cuando se elimina la aplicación, se quita el recurso de red.

## <a name="next-steps"></a>Pasos siguientes 
Para obtener más información sobre Service Fabric mesh, lea la introducción:
- [Service Fabric Mesh overview](service-fabric-mesh-overview.md) (Introducción a Service Fabric Mesh)

[Image1]: media/service-fabric-mesh-networks-and-gateways/NetworkAndGateway.png