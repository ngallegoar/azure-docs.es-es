---
title: Restablecimiento de TCP inactivo en Load Balancer en Azure
titleSuffix: Azure Load Balancer
description: En este artículo, obtendrá información sobre Azure Load Balancer con paquetes de TCP RST bidireccionales en tiempo de espera de inactividad.
services: load-balancer
documentationcenter: na
author: asudbring
ms.custom: seodec18
ms.service: load-balancer
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/03/2019
ms.author: allensu
ms.openlocfilehash: d3d836ddea8d07a25ad09e6f19d9f17a680decd6
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "80294400"
---
# <a name="load-balancer-with-tcp-reset-on-idle"></a>Load Balancer con restablecimiento de TCP inactivo

Puede usar [Standard Load Balancer](load-balancer-standard-overview.md) para crear un comportamiento de aplicación más predecible para los escenarios si habilita el restablecimiento de TCP inactivo para una regla determinada. El comportamiento predeterminado de Load Balancer es descartar silenciosamente los flujos cuando se alcanza el tiempo de inactividad de un flujo.  Si habilita esta característica, Load Balancer enviará restablecimientos de TCP bidireccionales (paquete TCP RST) cuando se agote el tiempo de espera de inactividad.  Esto le informará a los puntos de conexión de la aplicación que el tiempo de espera se agotó y ya no se puede usar.  De ser necesario, los puntos de conexión pueden establecer una conexión nueva inmediatamente.

![Restablecimiento de TCP en Load Balancer](media/load-balancer-tcp-reset/load-balancer-tcp-reset.png)
 
Puede cambiar este comportamiento predeterminado y habilitar el envío de restablecimientos de TCP al agotarse el tiempo de espera de inactividad en las reglas NAT de entrada, en las reglas de equilibrio de carga y en las [reglas de salida](https://aka.ms/lboutboundrules).  Cuando se habilita en cada regla, Load Balancer envía restablecimientos TCP bidireccionales (paquetes RST de TCP) tanto a los puntos de conexión del cliente como a los del servidor en el momento en que se agota el tiempo de espera de inactividad para todos los flujos que correspondan.

Los puntos de conexión que reciben los paquetes RST de TCP cierran inmediatamente el socket correspondiente. De este modo, se proporciona una notificación inmediata a los puntos de conexión para indicar que se ha liberado la conexión y cualquier comunicación posterior en la misma conexión TCP generará un error.  Las aplicaciones pueden purgar las conexiones cuando el socket se cierra y restablecer las conexiones según sea necesario sin tener que esperar que la conexión TCP, finalmente, agote el tiempo de espera.

En muchos escenarios, esto puede evitar que haya que enviar paquetes keepalive de TCP (o en la capa de la aplicación) para actualizar el tiempo de inactividad de un flujo. 

Si la duración de la inactividad supera la permitida por la configuración o la aplicación muestra un comportamiento no deseado con los restablecimientos de TCP habilitados, es posible que siga teniendo que usar paquetes keepalive de TCP (o de la capa de la aplicación) para supervisar el estado de las conexiones TCP.  Además, los paquetes keepalive también pueden seguir siendo útiles cuando la conexión atraviesa un proxy en algún lugar de la ruta de acceso, especialmente en el caso de los paquetes keepalive de la capa de la aplicación.  

Examine con cuidado todo el escenario completo para decidir si le beneficia habilitar los restablecimientos de TCP y ajustar el tiempo de espera de inactividad, y si es posible que se requieran pasos adicionales para garantizar el comportamiento deseado de la aplicación.

## <a name="enabling-tcp-reset-on-idle-timeout"></a>Habilitación del restablecimiento de TCP al agotarse el tiempo de espera de inactividad

Con la API versión 2018-07-01, puede habilitar el envío de restablecimientos de TCP bidireccionales al agotarse el tiempo de espera de inactividad en cada regla:

```json
      "loadBalancingRules": [
        {
          "enableTcpReset": true | false,
        }
      ]
```

```json
      "inboundNatRules": [
        {
          "enableTcpReset": true | false,
        }
      ]
```

```json
      "outboundRules": [
        {
          "enableTcpReset": true | false,
        }
      ]
```

## <a name="region-availability"></a><a name="regions"></a> Disponibilidad en regiones

Disponible en todas las regiones.

## <a name="limitations"></a>Limitaciones

- TCP RST solo se envía durante la conexión TCP en el estado ESTABLECIDO.

## <a name="next-steps"></a>Pasos siguientes

- Más información sobre [Standard Load Balancer](load-balancer-standard-overview.md).
- Más información sobre [reglas de salida](load-balancer-outbound-rules-overview.md).
- [Configurar TCP RST en tiempo de espera de inactividad](load-balancer-tcp-idle-timeout.md)
