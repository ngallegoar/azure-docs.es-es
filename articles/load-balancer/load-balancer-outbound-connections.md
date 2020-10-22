---
title: Azure Load Balancer como proxy de salida
description: En este artículo se describe cómo se usa Azure Load Balancer como proxy para la conectividad saliente de Internet.
services: load-balancer
author: asudbring
ms.service: load-balancer
ms.topic: conceptual
ms.custom: contperfq1
ms.date: 10/13/2020
ms.author: allensu
ms.openlocfilehash: 422f8106ac52c85f0680d54e420d0f1b4d326910
ms.sourcegitcommit: 2c586a0fbec6968205f3dc2af20e89e01f1b74b5
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/14/2020
ms.locfileid: "92017699"
---
# <a name="outbound-proxy-azure-load-balancer"></a>Azure Load Balancer como proxy de salida

Se puede usar un equilibrador de carga de Azure Load Balancer como proxy para la conectividad saliente de Internet. El equilibrador de carga proporciona conectividad saliente para las instancias de back-end. 

Esta configuración usa **traducción de direcciones de red de origen (SNAT)** . SNAT reescribe la dirección IP del back-end en la IP pública del equilibrador de carga. 

SNAT habilita el **enmascaramiento de IP** de la instancia de back-end. Esta enmascaramiento impide que los orígenes externos tengan una dirección directa a las instancias de back-end. 

El hecho de compartir una dirección IP entre instancias de back-end reduce el costo de las direcciones IP públicas estáticas y admite escenarios como la simplificación de listas de direcciones IP permitidas con tráfico procedente de direcciones IP públicas conocidas. 

## <a name="sharing-ports-across-resources"></a><a name ="snat"></a> Uso compartido de puertos entre recursos

Si los recursos de back-end de un equilibrador de carga no tienen direcciones IP públicas a nivel de instancia (ILPIP), establecen conectividad saliente a través de la IP de front-end del equilibrador de carga público.

Se usan puertos para generar identificadores únicos que se emplean para mantener flujos distintos. Internet usa una tupla de cinco elementos para proporcionar esta distinción.

La tupla de cinco elementos consta de:

* IP de destino
* Puerto de destino
* IP de origen
* Puerto y protocolo de origen para proporcionar esta distinción.

Si se usa un puerto para las conexiones entrantes, tendrá un **agente de escucha** para las solicitudes de conexión entrantes en ese puerto y no se puede usar para las conexiones salientes. 

Para establecer una conexión saliente, se debe usar un **puerto efímero** para proporcionar al destino un puerto con el que comunicarse y mantener un flujo de tráfico distinto. 

Cada dirección IP tiene 65 535 puertos. Los primeros 1024 puertos se reservan como **puertos del sistema**. Cada puerto puede usarse para conexiones entrantes o salientes para TCP y UDP. 

De los restantes puertos, Azure ofrece 64 000 para su uso como **puertos efímeros**. Cuando se agrega una dirección IP como configuración de IP de front-end, estos puertos efímeros se pueden usar para SNAT.

A través de reglas de salida, estos puertos SNAT se pueden distribuir a instancias de back-end para permitirles compartir las IP públicas del equilibrador de carga para las conexiones salientes.

Las redes del host para cada instancia de back-end realizarán la traducción SNAT en los paquetes que forman parte de una conexión saliente. El host reescribe la dirección IP de origen en una de las direcciones IP públicas. El host reescribe el puerto de origen de cada paquete saliente en uno de los puertos SNAT.

## <a name="exhausting-ports"></a><a name="scenarios"></a> Agotamiento de puertos

Cada conexión a la misma dirección IP de destino y puerto de destino usará un puerto SNAT. Esta conexión mantiene un **flujo de tráfico** distinto desde la instancia de back-end o **cliente** a un **servidor**. Este proceso proporciona al servidor un puerto distinto en el que dirigir el tráfico. Sin este proceso, el equipo cliente no sabe a qué flujo pertenece un paquete.

Imagine que varios exploradores van a https://www.microsoft.com, que tiene los valores siguientes:

* Dirección IP de destino = 23.53.254.142
* Puerto de destino = 443
* Protocolo = TCP

Sin puertos de destino diferentes para el tráfico de retorno (el puerto SNAT usado para establecer la conexión), el cliente no tendrá forma de separar el resultado de una consulta de otro.

Las conexiones salientes pueden aumentar. Una instancia de back-end podría tener asignados puertos insuficientes. Si la **reutilización de conexiones** no está habilitada, aumenta el riesgo de **agotamiento de puertos** SNAT.

Se producirá un error en las nuevas conexiones salientes a una dirección IP de destino cuando se agoten los puertos. Las conexiones se realizarán correctamente cuando esté disponible un puerto. Este agotamiento se produce cuando los 64 000 puertos de una dirección IP se distribuyen entre muchas instancias de back-end. Para obtener instrucciones sobre la mitigación del agotamiento de puertos SNAT, consulte la [guía de solución de problemas](https://docs.microsoft.com/azure/load-balancer/troubleshoot-outbound-connection).  

En el caso de las conexiones TCP, el equilibrador de carga usará un solo puerto SNAT para cada IP y puerto de destino. Este uso múltiple permite varias conexiones a la misma dirección IP de destino con el mismo puerto SNAT, pero se limita si la conexión no es a puertos de destino diferentes.

En el caso de las conexiones UDP, el equilibrador de carga usa un algoritmo de **NAT de cono con restricción de puerto**, que consume un puerto SNAT por cada IP de destino, sea cual sea el puerto de destino. 

Un puerto se reutiliza para un número ilimitado de conexiones. El puerto solo se reutiliza si la dirección IP o puerto de destino son diferentes.

## <a name="port-allocation"></a><a name="preallocatedports"></a> Asignación de puertos

A cada dirección IP pública asignada como IP de front-end del equilibrador de carga se le proporcionan 64 000 puertos SNAT para sus miembros del grupo de back-end. Los puertos no se pueden compartir con miembros del grupo de back-end. Un intervalo de puertos SNAT solo lo puede usar una única instancia de back-end para garantizar que los paquetes devueltos se enruten correctamente. 

Se recomienda usar una regla de salida explícita para configurar la asignación de puertos SNAT. Esta regla maximizará el número de puertos SNAT que cada instancia de back-end tiene disponible para las conexiones salientes. 

Si usa la asignación automática de SNAT saliente a través de una regla de equilibrio de carga, la tabla de asignación definirá la asignación de los puertos.

En la <a name="snatporttable"></a>tabla siguiente se muestran las asignaciones previas de puertos SNAT para los niveles de tamaño del grupo de back-end:

| Tamaño del grupo (instancias de máquina virtual) | Puertos SNAT asignados previamente por configuración IP |
| --- | --- |
| 1-50 | 1024 |
| 51-100 | 512 |
| 101-200 | 256 |
| 201-400 | 128 |
| 401-800 | 64 |
| 801-1000 | 32 | 

>[!NOTE]
> Si tiene un grupo de back-end con un tamaño máximo de 6, cada instancia puede tener 64 000/10 = 6400 puertos si define una regla de salida explícita. Según la tabla anterior, cada uno solo tendrá 1024 si elige la asignación automática.

## <a name="outbound-rules-and-virtual-network-nat"></a><a name="outboundrules"></a> Reglas de salida y Virtual Network NAT

Las reglas de salida de Azure Load Balancer y Virtual Network NAT son dos opciones disponibles para la salida desde una red virtual.

Para obtener más información sobre las reglas de salida, consulte [Reglas de salida](outbound-rules.md).

Para obtener más información sobre Azure Virtual Network NAT, consulte [¿Qué es Azure Virtual Network NAT?](../virtual-network/nat-overview.md)

## <a name="constraints"></a>Restricciones

*   Los puertos se liberarán después de 15 segundos si se recibe o se envía **TCP RST**.
*   Los puertos se liberarán después de 240 segundos si se recibe o se envía **FINACK**.
*   Cuando una conexión está inactiva y no se envían paquetes nuevos, los puertos se liberarán después de un intervalo entre 4 y 120 minutos.
  * Este umbral se puede configurar a través de reglas de salida.
*   Cada dirección IP proporciona 64 000 puertos que se pueden usar para SNAT.
*   Cada puerto se puede usar para conexiones TCP y UDP con una dirección IP de destino.
  * Se necesita un puerto SNAT UDP tanto si el puerto de destino es único como si no. Para cada conexión UDP con una dirección IP de destino, se usa un puerto SNAT UDP.
  * Se puede usar un puerto SNAT TCP para varias conexiones con la misma dirección IP de destino, siempre que los puertos de destino sean diferentes.
*   El agotamiento de SNAT se produce cuando una instancia de back-end se ejecuta fuera de los puertos SNAT establecidos. Un equilibrador de carga puede tener puertos SNAT sin usar. Si los puertos SNAT usados de una instancia de back-end superan sus puertos SNAT, no podrá establecer nuevas conexiones salientes.

## <a name="next-steps"></a>Pasos siguientes

*   [Solución de errores de conexiones salientes debidos a un agotamiento de SNAT](https://docs.microsoft.com/azure/load-balancer/troubleshoot-outbound-connection)
*   [Revise las métricas de SNAT](https://docs.microsoft.com/azure/load-balancer/load-balancer-standard-diagnostics#how-do-i-check-my-snat-port-usage-and-allocation) y familiarícese con la manera correcta de filtrarlas, dividirlas y visualizarlas.

