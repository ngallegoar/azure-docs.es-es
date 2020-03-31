---
title: Introducción al modelo de recursos de Azure Service Fabric
description: Obtenga información sobre el modelo de recursos de Service Fabric, un enfoque simplificado para definir aplicaciones de Service Fabric mesh.
author: vturecek
ms.topic: conceptual
ms.date: 10/23/2018
ms.author: vturecek
ms.custom: mvc, devcenter
ms.openlocfilehash: 0ae2ed163560aee4c0c3525ab31910e37afaa5b9
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "79229520"
---
# <a name="introduction-to-service-fabric-resource-model"></a>Introducción al modelo de recursos de Service Fabric

El modelo de recursos de Service Fabric describe un enfoque simple para definir los recursos que componen una aplicación de Service Fabric Mesh. Los recursos individuales se pueden implementar en cualquier entorno de Service Fabric.  El modelo de recursos de Service Fabric también es compatible con el modelo de Azure Resource Manager. Actualmente, se admiten los siguientes tipos de recursos en este modelo:

- Aplicaciones y servicios
- Redes
- Puertas de enlace
- Secretos y secretos/valores
- Volúmenes

Cada recurso se describe mediante declaración en un archivo de recursos, que es un documento YAML o JSON simple que describe la aplicación mesh y que aprovisiona la plataforma Service Fabric.

## <a name="applications-and-services"></a>Aplicaciones y servicios

Un recurso de aplicación es la unidad de implementación, control de versiones y duración de una aplicación mesh. Se compone de uno o varios recursos de servicio que representan un microservicio. Cada recurso de servicio, a su vez, se compone de uno o más paquetes de código que describen todo lo necesario para ejecutar la imagen de contenedor asociada con el paquete de código.

![Aplicaciones y servicios][Image1]

Un recurso de servicio declara lo siguiente:

- Nombre, versión y registro del contenedor
- Recursos de CPU y memoria necesarios para cada contenedor
- Puntos de conexión de red
- Referencias a otros recursos como redes, volúmenes y secretos 

Todos los paquetes de código definidos como parte de un recurso de servicio se implementan y activan juntos como grupo. El recurso de servicio también describe cuántas instancias de servicio se deben ejecutar y también hace referencia a otros recursos (por ejemplo, el recurso de red) de los que depende.

Si una aplicación mesh se compone de más de un servicio, no se garantiza que se ejecuten conjuntamente en el mismo nodo. Además, durante una actualización de la aplicación, un error al actualizar un único servicio provoca que se reviertan todos los servicios a su versión anterior.

Como se ha mencionado antes, el ciclo de vida de cada instancia de aplicación se puede administrar de forma independiente. Por ejemplo, una instancia de aplicación se puede actualizar independientemente de las otras instancias de la aplicación. Normalmente, se mantiene un número reducido de servicios en una aplicación, ya que, cuantos más servicios se agregan a una aplicación, más difícil resulta administrar cada servicio de manera independiente.

## <a name="networks"></a>Redes

Un recurso de red es un recurso que se puede implementar individualmente y es independiente de un recurso de aplicación o servicio al que pueda hacer referencia como dependencia. Se usa para crear una red para las aplicaciones. Varios servicios de distintas aplicaciones pueden formar parte de la misma red.  Para más información, lea sobre las [redes en aplicaciones de Service Fabric Mesh](service-fabric-mesh-networks-and-gateways.md).

> [!NOTE]
> La versión preliminar actual solo admite una asignación de uno a uno entre aplicaciones y redes.

![Red y puerta de enlace][Image2]

## <a name="gateways"></a>Puertas de enlace
Un recurso de puerta de enlace conecta dos redes y enruta el tráfico.  Una puerta de enlace permite a los servicios comunicarse con los clientes externos y proporciona una entrada en los servicios.  También se puede usar una puerta de enlace para conectar la aplicación de malla con su propia red virtual existente. Para más información, lea sobre las [redes en aplicaciones de Service Fabric Mesh](service-fabric-mesh-networks-and-gateways.md).

![Red y puerta de enlace][Image2]

## <a name="secrets"></a>Secretos

Los recursos de secretos se implementan de forma independiente a una aplicación o un recurso de servicio que puede hacer referencia a él como su dependencia. Se utiliza para distribuir secretos de forma segura a sus aplicaciones. Varios servicios de distintas aplicaciones pueden hacer referencia a valores del mismo secreto.

## <a name="volumes"></a>Volúmenes

Los contenedores suelen habilitar discos temporales. Sin embargo, los discos temporales son efímeros, por lo que puede obtener un nuevo disco temporal y perder la información cuando un contenedor se bloquea. También es difícil compartir información de los discos temporales con otros contenedores. Los volúmenes son directorios que se montan dentro de las instancias de contenedor que puede usar para conservar el estado. Los volúmenes proporcionan almacenamiento de archivos de propósito general y permiten leer y escribir archivos mediante las API de archivos de E/S de disco normal. El recurso de volumen es una manera declarativa de describir cómo se monta un directorio y su almacenamiento de respaldo (un volumen de Azure Files o un volumen confiable de Service Fabric).  Para más información, vea el [estado de almacenamiento](service-fabric-mesh-storing-state.md#volumes).

![Volúmenes][Image3]

## <a name="programming-models"></a>Modelos de programación
Un recurso de servicio solo requiere una imagen de contenedor para ejecutarse, a la que se hace referencia en los paquetes de código asociados con el recurso. Puede ejecutar cualquier código, escrito en cualquier lenguaje, con cualquier marco, dentro del contenedor sin necesidad de conocer ni usar API específicas de Service Fabric mesh. 

El código de aplicación sigue siendo portátil, incluso fuera de Service Fabric mesh, y las implementaciones de aplicaciones siguen siendo coherentes, independientemente del lenguaje o marco que se use para implementar los servicios. Independientemente de si la aplicación es ASP.NET Core, Go, o, simplemente, un conjunto de procesos y scripts, el modelo de implementación de recursos de Service Fabric mesh es el mismo. 

## <a name="packaging-and-deployment"></a>Empaquetado e implementación

Las aplicaciones de Service Fabric Mesh basadas en el modelo de recursos se empaquetan como contenedores de Docker.  Service Fabric Mesh es un entorno compartido multiempresa, y los contenedores ofrecen un alto nivel de aislamiento.  Estas aplicaciones se describen mediante un formato JSON o un formato YAML (que, a continuación, se convierte en JSON). Al implementar una aplicación de malla en Azure Service Fabric Mesh, el formato JSON que se usa para describir la aplicación es una plantilla de Azure Resource Manager. Los recursos se asignan a los recursos de Azure.  Al implementar una aplicación de malla en un clúster de Service Fabric (independiente u hospedado en Azure), el formato JSON que se usa para describir la aplicación es un formato similar a una plantilla de Azure Resource Manager.  Una vez implementadas, las aplicaciones de malla pueden administrarse mediante interfaces HTTP o la CLI de Azure. 


## <a name="next-steps"></a>Pasos siguientes 
Para obtener más información sobre Service Fabric mesh, lea la introducción:
- [Service Fabric Mesh overview](service-fabric-mesh-overview.md) (Introducción a Service Fabric Mesh)

[Image1]: media/service-fabric-mesh-service-fabric-resources/AppsAndServices.png
[Image2]: media/service-fabric-mesh-service-fabric-resources/NetworkAndGateway.png
[Image3]: media/service-fabric-mesh-service-fabric-resources/volumes.png
