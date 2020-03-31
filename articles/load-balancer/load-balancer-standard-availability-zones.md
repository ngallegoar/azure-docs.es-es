---
title: Azure Standard Load Balancer y zonas de disponibilidad
titleSuffix: Azure Load Balancer
description: Con esta ruta de aprendizaje, empiece a usar Azure Standard Load Balancer y Availability Zones.
services: load-balancer
documentationcenter: na
author: asudbring
ms.custom: seodec18
ms.service: load-balancer
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 08/07/2019
ms.author: allensu
ms.openlocfilehash: 5a65982c5c13eb4e4273efcfd8d14910b0f35572
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "78197154"
---
# <a name="standard-load-balancer-and-availability-zones"></a>Load Balancer Estándar y zonas de disponibilidad

Azure Standard Load Balancer admite escenarios de [zonas de disponibilidad](../availability-zones/az-overview.md). Puede usar Standard Load Balancer para optimizar la disponibilidad en un escenario de un extremo a otro mediante la alineación de recursos con zonas y su distribución entre ellas.  Revise el documento sobre [zonas de disponibilidad](../availability-zones/az-overview.md) para ver qué son las zonas de disponibilidad, qué regiones las admiten actualmente y otros productos y conceptos relacionados. Las zonas de disponibilidad, combinadas con Standard Load Balancer, son un conjunto de características ampliable y flexible que posibilita la creación de diferentes escenarios.  Revise este documento para conocer estos [conceptos](#concepts) y la [guía de diseño](#design) de un escenario básico.

## <a name="availability-zones-concepts-applied-to-load-balancer"></a><a name="concepts"></a> Conceptos de las zonas de disponibilidad aplicados a Load Balancer

Un recurso de Load Balancer en sí mismo es regional y nunca zonal. La granularidad de lo que se puede configurar está restringida por cada configuración de front-end, regla y definición del grupo de back-end.
En el contexto de las zonas de disponibilidad, el comportamiento y las propiedades de una regla de Load Balancer se describen como con redundancia de zona o zonales.  Con redundancia de zona y zonal describen la zonalidad de una propiedad.  En el contexto de Load Balancer, con redundancia de zona siempre significa *varias zonas* y zonal significa aislamiento del servicio para una *sola zona*.
El uso de Load Balancer, tanto público como interno, admite escenarios con redundancia de zona y escenarios zonales, y ambos pueden dirigir el tráfico entre las zonas según sea necesario (*equilibrio de carga entre zonas*). 

### <a name="frontend"></a>Front-end

Un front-end de Load Balancer es una configuración de IP de front-end que hace referencia a un recurso de dirección IP pública o a una dirección IP privada dentro de la subred de un recurso de red virtual.  Conforma el punto de conexión con equilibrio de carga en el que se expone su servicio.
Un recurso de Load Balancer puede contener reglas con front-end zonales y con redundancia de zona al mismo tiempo. Cuando se garantiza un recurso de dirección IP pública o una dirección IP privada a una zona, la zonalidad (o la falta de ella) no es mutable.  Si desea cambiar u omitir la zonalidad de un front-end con dirección IP pública o privada, debe volver a crear la dirección IP pública en la zona que corresponda.  Las zonas de disponibilidad no cambian las restricciones para varios front-end, revise [varios front-ends para Load Balancer](load-balancer-multivip-overview.md) para más información sobre esta capacidad.

#### <a name="zone-redundant"></a>Redundancia de zona 

En una región con zonas de disponibilidad, un front-end de Standard Load Balancer tiene redundancia de zona.  La redundancia de zona significa que todos los flujos de entrada o de salida son atendidos por todas las zonas de disponibilidad de una región de forma simultánea con una única dirección IP. No son necesarios esquemas de redundancia de DNS. Una dirección IP de front-end única puede sobrevivir a un error en la zona y se puede utilizar para llegar a todos los miembros del grupo de back-end (no afectados) con independencia de la zona. Se puede producir un error en una o más zonas de disponibilidad y la ruta de acceso de datos sobrevive siempre que una zona de la región permanezca correcta. La dirección IP única del front-end se suministra a la vez en varias implementaciones de infraestructura independientes de varias zonas de disponibilidad.  Esto no significa que sea una ruta de acceso de datos sin incidencias, sino que los reintentos o el restablecimiento se realizarán correctamente en otras zonas no afectadas por el error zonal.   

#### <a name="optional-zone-isolation"></a>Aislamiento de zona opcional

Puede optar por tener un front-end garantizado para una sola zona, que se conoce como un *front-end zonal*.  Esto significa que cualquier flujo de entrada o de salida es atendido por una sola zona en una región.  El front-end comparte destino con el estado de la zona.  La ruta de acceso de datos no se ve afectada por errores en zonas distintas a la garantizada. Puede utilizar front-end zonales para exponer una dirección IP por cada zona de disponibilidad.  

Además, puede consumir servidores front-end zonales directamente para puntos de conexión con equilibrio de carga dentro de cada zona. También puede utilizar esto para exponer puntos de conexión con equilibrio de carga por zona para supervisar de forma individual cada zona.  O bien, para los puntos de conexión públicos, puede integrarlos con un producto de equilibrio de carga de DNS como [Traffic Manager](../traffic-manager/traffic-manager-overview.md) y usar un nombre DNS único. Después, el cliente resolverá este nombre DNS en varias direcciones IP zonales.  

Si desea combinar estos conceptos (con redundancia de zona y zonal para el mismo back-end), consulte [varios front-end en Azure Load Balancer](load-balancer-multivip-overview.md).

Para un front-end de Load Balancer público, se agrega un parámetro *zones* al recurso de dirección IP pública al que hace referencia la configuración de IP del front-end que usa cada regla correspondiente.

Para un front-end de Load Balancer interno, se agrega un parámetro de *zonas* a la configuración IP del front-end del recurso de Load Balancer interno. El front-end zonal hace que Load Balancer garantice una dirección IP en una subred para una zona específica.

### <a name="cross-zone-load-balancing"></a>Equilibrio de carga entre zonas

El equilibrio de carga entre zonas es la capacidad de Load Balancer para alcanzar un punto de conexión de back-end en cualquier zona y es independiente del front-end y su zonalidad.  Cualquier regla de equilibrio de carga puede tener como destino una instancia de back-end de cualquier zona de disponibilidad o instancia regional.

Debe tener cuidado para construir el escenario de forma que exprese una noción de zonas de disponibilidad. Por ejemplo, debe garantizar la implementación de la máquina virtual en una sola zona o en varias y alinear los recursos de front-end y de back-end zonales en la misma zona.  Si cruza zonas de disponibilidad solo con recursos de zona, el escenario funcionará, pero es posible que no tenga un modo de recuperación de errores con respecto a las zonas de disponibilidad. 

### <a name="backend"></a>Back-end

Load Balancer funciona con máquinas virtuales.  Pueden ser independientes, conjuntos de disponibilidad o conjuntos de escalado de máquinas virtuales.  Cualquier máquina virtual de una red virtual individual puede formar parte del grupo de back-end con independencia de si están garantizadas para una zona o no, o a qué zona están garantizadas.

Si desea alinear y garantizar el front-end y el back-end con una sola zona, basta con colocar las máquinas virtuales dentro de la misma zona en el grupo de back-end respectivo.

Si desea ubicar las máquinas virtuales a través de varias zonas, basta con colocar las máquinas virtuales de varias zonas en el mismo grupo de back-end.  Cuando se utilizan conjuntos de escalado de máquinas virtuales, puede ubicar uno o más conjuntos de escalado de máquinas virtuales en el mismo grupo de back-end.  Y cada uno de estos conjuntos de escalado de máquinas virtuales puede estar en una o varias zonas.

### <a name="outbound-connections"></a>Conexiones de salida

Las mismas propiedades de zonas y con redundancia de zona se aplican a las [conexiones salientes](load-balancer-outbound-connections.md).  Todas las zonas proporcionan una dirección IP pública con redundancia de zona para las conexiones salientes. Una dirección IP pública zonal solo sirve para la zona en la que se garantiza.  Las asignaciones del puerto SNAT de conexión saliente sobreviven a los errores de zona y el escenario seguirá proporcionando conectividad SNAT saliente si no se ve afectada por un error de zona.  Esto puede requerir que las transmisiones o las conexiones se vuelvan a establecer para escenarios con redundancia de zona si un flujo se sirvió mediante una zona afectada.  Los flujos en zonas distintas de las zonas afectadas no resultan afectados.

El algoritmo de asignación previa de puerto SNAT es el mismo, con o sin zonas de disponibilidad.

### <a name="health-probes"></a>Sondeos de estado

Las definiciones de sondeos de mantenimiento existentes permanecen tal cual estaban sin zonas de disponibilidad.  Pero hemos ampliado el modelo de mantenimiento hasta un nivel de infraestructura. 

Cuando se usan front-end con redundancia de zona, Load Balancer expande su modelo de mantenimiento interno para sondear el alcance de una máquina virtual de cada zona de disponibilidad por separado y cerrar las rutas de acceso entre zonas que puedan haber tenido errores sin la intervención del usuario.  Si una ruta de acceso determinada no está disponible desde la infraestructura de Load Balancer de una zona para una máquina virtual de otra zona, Load Balancer puede detectar y evitar este error. Otras zonas que puedan llegar a esta máquina virtual pueden continuar sirviendo la máquina virtual desde sus front-end respectivos.  Como resultado, es posible que, durante los eventos de error, cada zona pueda tener distribuciones de nuevos flujos ligeramente diferentes al proteger el mantenimiento general de su servicio de un extremo a otro.

## <a name="design-considerations"></a><a name="design"></a> Consideraciones de diseño

Load Balancer es deliberadamente flexible en el contexto de las zonas de disponibilidad. Puede optar por alinearse con zonas y puede optar por la redundancia de zona para cada zona.  Aumentar la disponibilidad puede conllevar una mayor complejidad y debe diseñar la disponibilidad para un rendimiento óptimo.  Revise algunas consideraciones de diseño importantes.

### <a name="automatic-zone-redundancy"></a>Redundancia de zona automática

Load Balancer hace sencillo tener una dirección IP única como un front-end con redundancia de zona. Una dirección IP con redundancia de zona puede servir de forma segura un recurso zonal de cualquier zona y puede sobrevivir a uno o más errores de zona mientras una zona funcione correctamente en la región. Por el contrario, un front-end zonal es una reducción del servicio a una única zona y comparte el destino con la zona correspondiente.

La redundancia de zona no implica una ruta de acceso de datos sin incidencias ni un plano de control; es expresamente un plano de datos. Los flujos con redundancia de zona pueden usar cualquier zona y los flujos de un cliente utilizarán todas las zonas correctas de una región. En caso de error de zona, el tráfico fluye por las zonas correctas que en ese momento del tiempo no se ven afectadas.  Los flujos de tráfico que están usando una zona en el momento en que se produce un error en esta se pueden ver afectados pero las aplicaciones se pueden recuperar. Estos flujos pueden continuar en las restantes zonas correctas de la región después de la retransmisión o el restablecimiento una vez que Azure ha detectado el error en la zona.

### <a name="cross-zone-boundaries"></a><a name="xzonedesign"></a> Límites entre zonas

Es importante comprender que cuando un servicio de un extremo a otro cruza zonas, comparte su destino no con una zona, sino potencialmente con varias zonas.  Como resultado, el servicio de un extremo a otro podría no obtener ninguna disponibilidad en implementaciones no zonales.

Cuando use zonas de disponibilidad, evite la introducción de dependencias entre zonas no deseadas que anulen las mejoras de disponibilidad.  Cuando la aplicación consta de varios componentes y desea que sean resistentes a los errores de zona, debe tener cuidado para asegurar la supervivencia de los componentes fundamentales suficientes si se produce un error en la zona.  Por ejemplo, un único componente crítico de la aplicación puede afectar a toda la aplicación si solo se encuentra en una zona que no sea de las zonas supervivientes.  Además, considere también la restauración de la zona y cómo se restablecerá la aplicación. Debe comprender el motivo por el que se producen errores en algunas partes de la aplicación. Vamos a revisar algunos puntos clave y usarlos como ideas para preguntas al pensar sobre su escenario concreto.

- Si la aplicación tiene dos componentes, como una dirección IP y una máquina virtual con un disco administrado, y están garantizados en las zonas 1 y 2, cuando se produce un error en la zona 1, el servicio de un extremo a otro de la zona no sobrevive.  En los escenarios zonales no se deben cruzar las zonas, a menos que entienda perfectamente que se va a crear un modo de error potencialmente perjudicial.  Este escenario puede proporcionar flexibilidad.

- Si la aplicación tiene dos componentes, como una dirección IP y una máquina virtual con un disco administrado, y tienen garantizadas la redundancia de zona y la zona 1 respectivamente, el servicio de un extremo a otro sobrevivirá a los errores en las zonas 2 y 3, o ambas, a menos que se produzcan errores en la zona 1.  No obstante, perderá cierta capacidad para razonar sobre el mantenimiento del servicio si todo lo que se observa es si se puede llegar al front-end.  Considere la posibilidad de desarrollar un modelo de mantenimiento y capacidad más amplio.  Podría usar los conceptos zonal y con redundancia de zona juntos para ampliar su visión y capacidad de administración.

- Si la aplicación tiene dos componentes, como un front-end de Load Balancer con redundancia de zona y un conjunto de escalado de máquinas virtuales distribuido en tres zonas, los recursos de las zonas no afectadas por el error estarán disponibles, pero el servicio de un extremo a otro puede verse degradado en términos de capacidad durante el error en la zona. Desde una perspectiva de infraestructura, la implementación puede sobrevivir a uno o más errores de zona y esto nos lleva a las siguientes preguntas:
  - ¿Conoce la lógica de la aplicación ante estos errores y ante una capacidad degradada?
  - ¿Necesita contar con medidas de seguridad en el servicio para forzar la conmutación por error a un par de regiones, si es necesario?
  - ¿Cómo serán la supervisión, la detección y la mitigación ante este escenario? Puede usar los diagnósticos de Load Balancer Estándar para aumentar la supervisión del rendimiento del servicio de un extremo a otro. Tenga en cuenta lo que está disponible y lo que puede necesitar de un aumento para tener una imagen completa.

- Las zonas pueden hacer que los errores sean más fáciles de entender y contener.  Sin embargo, un error en la zona no es diferente a otros errores en cuanto a conceptos como tiempos de espera, reintentos y algoritmos de retroceso. Aunque Azure Load Balancer proporciona rutas de acceso con redundancia de zona e intenta una rápida recuperación, en un nivel de paquete en tiempo real, las retransmisiones o los restablecimientos pueden producirse durante la aparición de un error y es importante conocer cómo controla la aplicación los errores. El esquema de equilibrio de carga sobrevivirá, pero debe planear lo siguiente:
  - Cuando se produce un error en una zona, ¿el servicio de un extremo a otro comprende esto y si se pierde el estado, cómo se recuperará?
  - Cuando vuelve la disponibilidad de una zona, ¿la aplicación sabe cómo realizar una recuperación de forma segura?

Revise [los patrones de diseño en la nube de Azure](https://docs.microsoft.com/azure/architecture/patterns/) para mejorar la resistencia de la aplicación a los escenarios de error.

## <a name="next-steps"></a>Pasos siguientes
- Aprenda más sobre [zonas de disponibilidad](../availability-zones/az-overview.md).
- Más información sobre [Load Balancer Estándar](load-balancer-standard-overview.md)
- Obtenga información sobre cómo [equilibrar la carga de las máquinas virtuales dentro de una zona con Load Balancer estándar con un front-end de zona](load-balancer-standard-public-zonal-cli.md)
- Obtenga información sobre cómo [equilibrar la carga de las máquinas virtuales en distintas zonas con Load Balancer estándar con un front-end con redundancia de zona](load-balancer-standard-public-zone-redundant-cli.md)
- Más información sobre [los patrones de diseño en la nube de Azure](https://docs.microsoft.com/azure/architecture/patterns/) para mejorar la resistencia de la aplicación a los escenarios de error.
