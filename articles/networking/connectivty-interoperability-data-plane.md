---
title: 'Interoperabilidad en Azure: Análisis del plano de datos'
description: En este artículo se proporciona el análisis del plano de datos de la configuración de prueba que puede usar para analizar la interoperabilidad entre ExpressRoute, una VPN de sitio a sitio y el emparejamiento de redes virtuales en Azure.
documentationcenter: na
services: networking
author: rambk
manager: tracsman
ms.service: virtual-network
ms.topic: article
ms.workload: infrastructure-services
ms.date: 10/18/2018
ms.author: rambala
ms.openlocfilehash: 635a8fc5409e18da9529763b06e4a531a36d0156
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "86169211"
---
# <a name="interoperability-in-azure--data-plane-analysis"></a>Interoperabilidad en Azure: Análisis del plano de datos

En este artículo se describe el análisis del plano de datos de la [configuración de prueba][Setup]. También puede revisar la [configuración de la prueba][Configuration] y el [análisis del plano de datos][Control-Analysis] de la configuración de prueba.

En el análisis del plano de datos se examina la ruta de acceso de los paquetes que se desplazan desde una red local (LAN o red virtual) a otra dentro de una topología. Es posible que la ruta de acceso de datos entre dos redes locales no sea necesariamente simétrica. Por tanto, en este artículo se va a analizar la ruta de reenvío desde una red local a otra de forma independiente a la ruta de acceso inversa.

## <a name="data-path-from-the-hub-vnet"></a>Ruta de acceso de datos desde el centro de conectividad

### <a name="path-to-the-spoke-vnet"></a>Ruta a la red virtual de radio

El emparejamiento de redes virtuales emula la funcionalidad de puente de red entre las dos redes virtuales emparejadas. A continuación se muestra la salida de traceroute desde un centro de conectividad a una máquina virtual de la red virtual de radio:

```console
C:\Users\rb>tracert 10.11.30.4

Tracing route to 10.11.30.4 over a maximum of 30 hops

  1     2 ms     1 ms     1 ms  10.11.30.4

Trace complete.
```

En la siguiente ilustración se muestra la vista de conexión gráfica del centro de conectividad y la red virtual de radio desde la perspectiva de Azure Network Watcher:


![1][1]

### <a name="path-to-the-branch-vnet"></a>Ruta de acceso a la red virtual de sucursal

A continuación se muestra la salida de traceroute desde un centro de conectividad a una máquina virtual de la red virtual de sucursal:

```console
C:\Users\rb>tracert 10.11.30.68

Tracing route to 10.11.30.68 over a maximum of 30 hops

  1     1 ms     1 ms     1 ms  10.10.30.142
  2     *        *        *     Request timed out.
  3     2 ms     2 ms     2 ms  10.11.30.68

Trace complete.
```

En este comando traceroute, el primer salto es la puerta de enlace de VPN en la instancia de Azure VPN Gateway del centro de conectividad. El segundo salto es la puerta de enlace de VPN de la red virtual de sucursal. La dirección IP de la puerta de enlace de VPN de la red virtual de sucursal no se anuncia en el centro de conectividad. El tercer salto es la máquina virtual de la red virtual de sucursal.

En la siguiente ilustración se muestra la vista de conexión gráfica del centro de conectividad y la red virtual de sucursal desde la perspectiva de Network Watcher:

![2][2]

Para la misma conexión, en la siguiente ilustración se muestra la vista de cuadrícula de Network Watcher:

![3][3]

### <a name="path-to-on-premises-location-1"></a>Ruta de acceso a la ubicación 1 local

A continuación se muestra la salida de traceroute desde un centro de conectividad a una máquina virtual de la ubicación 1 local:

```console
C:\Users\rb>tracert 10.2.30.10

Tracing route to 10.2.30.10 over a maximum of 30 hops

  1     2 ms     2 ms     2 ms  10.10.30.132
  2     *        *        *     Request timed out.
  3     *        *        *     Request timed out.
  4     2 ms     2 ms     2 ms  10.2.30.10

Trace complete.
```

En este comando traceroute, el primer salto es el punto de conexión del túnel de puerta de enlace de Azure ExpressRoute a un enrutador de Microsoft Enterprise Edge (MSEE). El segundo y tercer salto son las direcciones IP del enrutador del lado del cliente y la LAN de la ubicación 1 local. Estas direcciones IP no se anuncian en el centro de conectividad. El cuarto salto es la máquina virtual de la ubicación 1 local.


### <a name="path-to-on-premises-location-2"></a>Ruta de acceso a la ubicación 2 local

A continuación se muestra la salida de traceroute desde un centro de conectividad a una máquina virtual de la ubicación 2 local:

```console
C:\Users\rb>tracert 10.1.31.10

Tracing route to 10.1.31.10 over a maximum of 30 hops

  1    76 ms    75 ms    75 ms  10.10.30.134
  2     *        *        *     Request timed out.
  3     *        *        *     Request timed out.
  4    75 ms    75 ms    75 ms  10.1.31.10

Trace complete.
```

En este comando traceroute, el primer salto es el punto de conexión del túnel de puerta de enlace de ExpressRoute a un enrutador MSEE. El segundo y tercer salto son las direcciones IP del enrutador del lado del cliente y la LAN de la ubicación 2 local. Estas direcciones IP no se anuncian en el centro de conectividad. El cuarto salto es la máquina virtual de la ubicación 2 local.

### <a name="path-to-the-remote-vnet"></a>Ruta de acceso a la red virtual remota

A continuación se muestra la salida de traceroute desde un centro de conectividad a una máquina virtual de la red virtual remota:

```console
C:\Users\rb>tracert 10.17.30.4

Tracing route to 10.17.30.4 over a maximum of 30 hops

  1     2 ms     2 ms     2 ms  10.10.30.132
  2     *        *        *     Request timed out.
  3    69 ms    68 ms    69 ms  10.17.30.4

Trace complete.
```

En este comando traceroute, el primer salto es el punto de conexión del túnel de puerta de enlace de ExpressRoute a un enrutador MSEE. El segundo salto es la dirección IP de la puerta de enlace de la red virtual remota. El intervalo IP del segundo salto no se anuncia en el centro de conectividad. El tercer salto es la máquina virtual de la red virtual remota.

## <a name="data-path-from-the-spoke-vnet"></a>Ruta de acceso de datos de la red virtual de radio

La red virtual de radio comparte la vista de red del centro de conectividad. Mediante el emparejamiento de redes virtuales, la red virtual de radio usa la conectividad de puerta de enlace remota del centro de conectividad como si estuviera conectada directamente a la red virtual de radio.

### <a name="path-to-the-hub-vnet"></a>Ruta de acceso al centro de conectividad

A continuación se muestra la salida de traceroute desde una red virtual de radio a una máquina virtual del centro de conectividad:

```console
C:\Users\rb>tracert 10.10.30.4

Tracing route to 10.10.30.4 over a maximum of 30 hops

  1    <1 ms    <1 ms    <1 ms  10.10.30.4

Trace complete.
```

### <a name="path-to-the-branch-vnet"></a>Ruta de acceso a la red virtual de sucursal

A continuación se muestra la salida de traceroute desde una red virtual de radio a una máquina virtual de la red virtual de sucursal:

```console
C:\Users\rb>tracert 10.11.30.68

Tracing route to 10.11.30.68 over a maximum of 30 hops

  1     1 ms    <1 ms    <1 ms  10.10.30.142
  2     *        *        *     Request timed out.
  3     3 ms     2 ms     2 ms  10.11.30.68

Trace complete.
```

En este comando traceroute, el primer salto es la puerta de enlace de VPN del centro de conectividad. El segundo salto es la puerta de enlace de VPN de la red virtual de sucursal. La dirección IP de la puerta de enlace de VPN de la red virtual de sucursal no se anuncia en el centro de conectividad ni en la red virtual de radio. El tercer salto es la máquina virtual de la red virtual de sucursal.

### <a name="path-to-on-premises-location-1"></a>Ruta de acceso a la ubicación 1 local

A continuación se muestra la salida de traceroute desde la red virtual de radio a una máquina virtual de la ubicación 1 local:

```console
C:\Users\rb>tracert 10.2.30.10

Tracing route to 10.2.30.10 over a maximum of 30 hops

  1    24 ms     2 ms     3 ms  10.10.30.132
  2     *        *        *     Request timed out.
  3     *        *        *     Request timed out.
  4     3 ms     2 ms     2 ms  10.2.30.10

Trace complete.
```

En este comando traceroute, el primer salto es el punto de conexión del túnel de puerta de enlace de ExpressRoute del centro de conectividad a un enrutador MSEE. El segundo y tercer salto son las direcciones IP del enrutador del lado del cliente y la LAN de la ubicación 1 local. Estas direcciones IP no se anuncian en el centro de conectividad ni en la red virtual de radio. El cuarto salto es la máquina virtual de la ubicación 1 local.

### <a name="path-to-on-premises-location-2"></a>Ruta de acceso a la ubicación 2 local

A continuación se muestra la salida de traceroute desde la red virtual de radio a una máquina virtual de la ubicación 2 local:

```console
C:\Users\rb>tracert 10.1.31.10

Tracing route to 10.1.31.10 over a maximum of 30 hops

  1    76 ms    75 ms    76 ms  10.10.30.134
  2     *        *        *     Request timed out.
  3     *        *        *     Request timed out.
  4    75 ms    75 ms    75 ms  10.1.31.10

Trace complete.
```

En este comando traceroute, el primer salto es el punto de conexión del túnel de puerta de enlace de ExpressRoute del centro de conectividad a un enrutador MSEE. El segundo y tercer salto son las direcciones IP del enrutador del lado del cliente y la LAN de la ubicación 2 local. Estas direcciones IP no se anuncian en el centro de conectividad ni en la red virtual de radio. El cuarto salto es la máquina virtual de la ubicación 2 local.

### <a name="path-to-the-remote-vnet"></a>Ruta de acceso a la red virtual remota

A continuación se muestra la salida de traceroute desde una red virtual de radio a una máquina virtual de la red virtual remota:

```console
C:\Users\rb>tracert 10.17.30.4

Tracing route to 10.17.30.4 over a maximum of 30 hops

  1     2 ms     1 ms     1 ms  10.10.30.133
  2     *        *        *     Request timed out.
  3    71 ms    70 ms    70 ms  10.17.30.4

Trace complete.
```

En este comando traceroute, el primer salto es el punto de conexión del túnel de puerta de enlace de ExpressRoute del centro de conectividad a un enrutador MSEE. El segundo salto es la dirección IP de la puerta de enlace de la red virtual remota. El intervalo IP del segundo salto no se anuncia en el centro de conectividad ni en la red virtual de radio. El tercer salto es la máquina virtual de la red virtual remota.

## <a name="data-path-from-the-branch-vnet"></a>Ruta de acceso de datos desde la red virtual de sucursal

### <a name="path-to-the-hub-vnet"></a>Ruta de acceso al centro de conectividad

A continuación se muestra la salida de traceroute desde la red virtual de sucursal a una máquina virtual del centro de conectividad:

```console
C:\Windows\system32>tracert 10.10.30.4

Tracing route to 10.10.30.4 over a maximum of 30 hops

  1    <1 ms    <1 ms    <1 ms  10.11.30.100
  2     *        *        *     Request timed out.
  3     4 ms     3 ms     3 ms  10.10.30.4

Trace complete.
```

En este comando traceroute, el primer salto es la puerta de enlace de VPN de la red virtual de sucursal. El segundo salto es la puerta de enlace de VPN del centro de conectividad. La dirección IP de la puerta de enlace de VPN de centro de conectividad no se anuncia en la red virtual remota. El tercer salto es la máquina virtual del centro de conectividad.

### <a name="path-to-the-spoke-vnet"></a>Ruta a la red virtual de radio

A continuación se muestra la salida de traceroute desde una red virtual de sucursal a una máquina virtual de la red virtual de radio:

```console
C:\Users\rb>tracert 10.11.30.4

Tracing route to 10.11.30.4 over a maximum of 30 hops

  1     1 ms    <1 ms     1 ms  10.11.30.100
  2     *        *        *     Request timed out.
  3     4 ms     3 ms     2 ms  10.11.30.4

Trace complete.
```

En este comando traceroute, el primer salto es la puerta de enlace de VPN de la red virtual de sucursal. El segundo salto es la puerta de enlace de VPN del centro de conectividad. La dirección IP de la puerta de enlace de VPN de centro de conectividad no se anuncia en la red virtual remota. El tercer salto es la máquina virtual de la red virtual de radio.

### <a name="path-to-on-premises-location-1"></a>Ruta de acceso a la ubicación 1 local

A continuación se muestra la salida de traceroute desde la red virtual de radio a una máquina virtual de la ubicación 1 local:

```console
C:\Users\rb>tracert 10.2.30.10

Tracing route to 10.2.30.10 over a maximum of 30 hops

  1     1 ms    <1 ms    <1 ms  10.11.30.100
  2     *        *        *     Request timed out.
  3     3 ms     2 ms     2 ms  10.2.30.125
  4     *        *        *     Request timed out.
  5     3 ms     3 ms     3 ms  10.2.30.10

Trace complete.
```

En este comando traceroute, el primer salto es la puerta de enlace de VPN de la red virtual de sucursal. El segundo salto es la puerta de enlace de VPN del centro de conectividad. La dirección IP de la puerta de enlace de VPN de centro de conectividad no se anuncia en la red virtual remota. El tercer salto es el punto de terminación de túnel VPN en el enrutador CE principal. El cuarto salto es una dirección IP interna de la ubicación 1 local. Esta dirección IP de LAN no se anuncia fuera del enrutador del lado del cliente. El quinto salto es la máquina virtual de destino de la ubicación 1 local.

### <a name="path-to-on-premises-location-2-and-the-remote-vnet"></a>Ruta de acceso a la ubicación 2 local y a la red virtual remota

Como se explicó al hablar del análisis del plano de control, la red virtual de sucursal no tiene visibilidad de la ubicación 2 local ni de la red virtual remota según la configuración de red. Los siguientes resultados de ping lo confirman: 

```console
C:\Users\rb>ping 10.1.31.10

Pinging 10.1.31.10 with 32 bytes of data:

Request timed out.
...
Request timed out.

Ping statistics for 10.1.31.10:
    Packets: Sent = 4, Received = 0, Lost = 4 (100% loss),

C:\Users\rb>ping 10.17.30.4

Pinging 10.17.30.4 with 32 bytes of data:

Request timed out.
...
Request timed out.

Ping statistics for 10.17.30.4:
    Packets: Sent = 4, Received = 0, Lost = 4 (100% loss),
```

## <a name="data-path-from-on-premises-location-1"></a>Ruta de acceso de datos desde la ubicación 1 local

### <a name="path-to-the-hub-vnet"></a>Ruta de acceso al centro de conectividad

A continuación se muestra la salida de traceroute desde la ubicación 1 local a una máquina virtual del centro de conectividad:

```console
C:\Users\rb>tracert 10.10.30.4

Tracing route to 10.10.30.4 over a maximum of 30 hops

  1    <1 ms    <1 ms    <1 ms  10.2.30.3
  2    <1 ms    <1 ms    <1 ms  192.168.30.0
  3    <1 ms    <1 ms    <1 ms  192.168.30.18
  4     *        *        *     Request timed out.
  5     2 ms     2 ms     2 ms  10.10.30.4

Trace complete.
```

En el comando traceroute, los dos primeros saltos forman parte de la red local. El tercer salto es la interfaz MSEE principal que se expone al enrutador del lado del cliente. El cuarto salto es la puerta de enlace de ExpressRoute del centro de conectividad. El intervalo IP de la puerta de enlace de ExpressRoute del centro de conectividad no se anuncia en la red local. El quinto salto es la máquina virtual de destino.

Network Watcher solo proporciona una vista centrada en Azure. Para una perspectiva local se usa Azure Network Performance Monitor. Network Performance Monitor proporciona agentes que se pueden instalar en servidores de redes fuera de Azure para el análisis de la ruta de acceso de datos.

En la siguiente ilustración se muestra la vista de topología de la conectividad de la máquina virtual de la ubicación 1 local con la máquina virtual del centro de conectividad mediante ExpressRoute:

![4][4]

Como se comentó anteriormente, en la configuración de prueba se usa VPN de sitio a sitio como conectividad de copia de seguridad para ExpressRoute entre la ubicación 1 local y el centro de conectividad. Para probar la ruta de acceso de datos de copia de seguridad se va a inducir un error de vinculación de ExpressRoute entre el enrutador CE principal de la ubicación 1 local y el MSEE correspondiente. Para inducir un error de vinculación de ExpressRoute, apague la interfaz del lado del cliente que se expone al MSEE:

```console
C:\Users\rb>tracert 10.10.30.4

Tracing route to 10.10.30.4 over a maximum of 30 hops

  1    <1 ms    <1 ms    <1 ms  10.2.30.3
  2    <1 ms    <1 ms    <1 ms  192.168.30.0
  3     3 ms     2 ms     3 ms  10.10.30.4

Trace complete.
```

En la siguiente ilustración se muestra la vista de topología de la conectividad de la máquina virtual en la ubicación 1 local con la máquina virtual con el centro de conectividad mediante la conectividad VPN de sitio a sitio cuando la de ExpressRoute está inactiva:

![5][5]

### <a name="path-to-the-spoke-vnet"></a>Ruta a la red virtual de radio

A continuación se muestra la salida de traceroute desde la ubicación 1 local a una máquina virtual de la red virtual de radio:

Volvamos a la conectividad de ExpressRoute principal para realizar el análisis de la ruta de datos hacia la red virtual de radio:

```console
C:\Users\rb>tracert 10.11.30.4

Tracing route to 10.11.30.4 over a maximum of 30 hops

  1    <1 ms    <1 ms    <1 ms  10.2.30.3
  2    <1 ms    <1 ms    <1 ms  192.168.30.0
  3    <1 ms    <1 ms    <1 ms  192.168.30.18
  4     *        *        *     Request timed out.
  5     3 ms     2 ms     2 ms  10.11.30.4

Trace complete.
```

Recuperemos la conectividad de ExpressRoute 1 principal para el resto del análisis de la ruta acceso de datos.

### <a name="path-to-the-branch-vnet"></a>Ruta de acceso a la red virtual de sucursal

A continuación se muestra la salida de traceroute desde la ubicación 1 local a una máquina virtual de la red virtual de sucursal:

```console
C:\Users\rb>tracert 10.11.30.68

Tracing route to 10.11.30.68 over a maximum of 30 hops

  1    <1 ms    <1 ms    <1 ms  10.2.30.3
  2    <1 ms    <1 ms    <1 ms  192.168.30.0
  3     3 ms     2 ms     2 ms  10.11.30.68

Trace complete.
```

### <a name="path-to-on-premises-location-2"></a>Ruta de acceso a la ubicación 2 local

Como se explicó en el [análisis del plano de control][Control-Analysis], la ubicación 1 local no tiene visibilidad de la ubicación 2 local según la configuración de red. Los siguientes resultados de ping lo confirman: 

```console
C:\Users\rb>ping 10.1.31.10

Pinging 10.1.31.10 with 32 bytes of data:

Request timed out.
...
Request timed out.

Ping statistics for 10.1.31.10:
    Packets: Sent = 4, Received = 0, Lost = 4 (100% loss),
```

### <a name="path-to-the-remote-vnet"></a>Ruta de acceso a la red virtual remota

A continuación se muestra la salida de traceroute desde la ubicación 1 local a una máquina virtual de la red virtual remota:

```console
C:\Users\rb>tracert 10.17.30.4

Tracing route to 10.17.30.4 over a maximum of 30 hops

  1    <1 ms    <1 ms    <1 ms  10.2.30.3
  2     2 ms     5 ms     7 ms  192.168.30.0
  3    <1 ms    <1 ms    <1 ms  192.168.30.18
  4     *        *        *     Request timed out.
  5    69 ms    70 ms    69 ms  10.17.30.4

Trace complete.
```

## <a name="data-path-from-on-premises-location-2"></a>Ruta de acceso de datos desde la ubicación 2 local

### <a name="path-to-the-hub-vnet"></a>Ruta de acceso al centro de conectividad

A continuación se muestra la salida de traceroute desde la ubicación 2 local a una máquina virtual del centro de conectividad:

```console
C:\Windows\system32>tracert 10.10.30.4

Tracing route to 10.10.30.4 over a maximum of 30 hops

  1    <1 ms    <1 ms    <1 ms  10.1.31.3
  2    <1 ms    <1 ms    <1 ms  192.168.31.4
  3    <1 ms    <1 ms    <1 ms  192.168.31.22
  4     *        *        *     Request timed out.
  5    75 ms    74 ms    74 ms  10.10.30.4

Trace complete.
```

### <a name="path-to-the-spoke-vnet"></a>Ruta a la red virtual de radio

A continuación se muestra la salida de traceroute desde la ubicación 2 local a una máquina virtual de la red virtual de radio:

```console
C:\Windows\system32>tracert 10.11.30.4

Tracing route to 10.11.30.4 over a maximum of 30 hops
  1    <1 ms    <1 ms     1 ms  10.1.31.3
  2    <1 ms    <1 ms    <1 ms  192.168.31.0
  3    <1 ms    <1 ms    <1 ms  192.168.31.18
  4     *        *        *     Request timed out.
  5    75 ms    74 ms    74 ms  10.11.30.4

Trace complete.
```

### <a name="path-to-the-branch-vnet-on-premises-location-1-and-the-remote-vnet"></a>Ruta de acceso a la red virtual de sucursal, la ubicación 1 local y la red virtual remota

Como se explicó en el [análisis del plano de control][Control-Analysis], la ubicación 1 local no tiene visibilidad de la red virtual de sucursal, la ubicación 1 local y la red virtual remota según la configuración de red. 

## <a name="data-path-from-the-remote-vnet"></a>Ruta de acceso de datos desde la red virtual remota

### <a name="path-to-the-hub-vnet"></a>Ruta de acceso al centro de conectividad

A continuación se muestra la salida de traceroute desde la red virtual remota a una máquina virtual del centro de conectividad:

```console
C:\Users\rb>tracert 10.10.30.4

Tracing route to 10.10.30.4 over a maximum of 30 hops

  1    65 ms    65 ms    65 ms  10.17.30.36
  2     *        *        *     Request timed out.
  3    69 ms    68 ms    68 ms  10.10.30.4

Trace complete.
```

### <a name="path-to-the-spoke-vnet"></a>Ruta a la red virtual de radio

A continuación se muestra la salida de traceroute desde una red virtual remota a una máquina virtual de la red virtual de radio:

```console
C:\Users\rb>tracert 10.11.30.4

Tracing route to 10.11.30.4 over a maximum of 30 hops

  1    67 ms    67 ms    67 ms  10.17.30.36
  2     *        *        *     Request timed out.
  3    71 ms    69 ms    69 ms  10.11.30.4

Trace complete.
```

### <a name="path-to-the-branch-vnet-and-on-premises-location-2"></a>Ruta de acceso a la red virtual de sucursal y la ubicación 2 local

Como se explicó en el [análisis del plano de control][Control-Analysis], la red virtual remota no tiene visibilidad de la red virtual de sucursal ni de la ubicación 2 local según la configuración de red. 

### <a name="path-to-on-premises-location-1"></a>Ruta de acceso a la ubicación 1 local

A continuación se muestra la salida de traceroute desde la red virtual remota a una máquina virtual de la ubicación 1 local:

```console
C:\Users\rb>tracert 10.2.30.10

Tracing route to 10.2.30.10 over a maximum of 30 hops

  1    67 ms    67 ms    67 ms  10.17.30.36
  2     *        *        *     Request timed out.
  3     *        *        *     Request timed out.
  4    69 ms    69 ms    69 ms  10.2.30.10

Trace complete.
```

## <a name="expressroute-and-site-to-site-vpn-connectivity-in-tandem"></a>Uso simultáneo de ExpressRoute y la conectividad VPN de sitio a sitio

###  <a name="site-to-site-vpn-over-expressroute"></a>VPN de sitio a sitio con ExpressRoute

Puede configurar una VPN de sitio a sitio mediante el emparejamiento de Microsoft con ExpressRoute para intercambiar datos de forma privada entre la red local y las redes virtuales de Azure. Esta configuración le asegura un intercambio de datos con confidencialidad, autenticidad, integridad y antirreproducción. Para más información acerca de cómo configurar una VPN IPsec de sitio a sitio en modo de túnel con emparejamiento de Microsoft de ExpressRoute, consulte [Configuración de una VPN de sitio a sitio a través del emparejamiento de Microsoft de ExpressRoute][S2S-Over-ExR]. 

La limitación principal de la configuración de una VPN de sitio a sitio que use el emparejamiento de Microsoft es el rendimiento. El rendimiento a través del túnel IPsec está limitado por la capacidad de la puerta de enlace de VPN. El rendimiento de la puerta de enlace de VPN es menor que el de ExpressRoute. En este escenario, el uso del túnel IPsec para el tráfico de seguridad alta y con emparejamiento privado para el tráfico restante ayuda a optimizar el uso de ancho de banda de ExpressRoute.

### <a name="site-to-site-vpn-as-a-secure-failover-path-for-expressroute"></a>VPN de sitio a sitio como ruta de acceso de conmutación por error para ExpressRoute

ExpressRoute sirve como par de circuito redundante para garantizar la alta disponibilidad. Puede configurar la conectividad de ExpressRoute con redundancia geográfica en diferentes regiones de Azure. Además, como se ha demostrado en la configuración de prueba, dentro de una región de Azure puede usar una VPN de sitio a sitio para crear una ruta de acceso de conmutación por error para la conectividad de ExpressRoute. Cuando se anuncian los mismos prefijos a través de ExpressRoute y VPN de sitio a sitio, Azure da prioridad a ExpressRoute. Para evitar el enrutamiento asimétrico entre ExpressRoute y VPN de sitio a sitio, la configuración de la red local también debe tener prioridad mediante el uso de la conectividad ExpressRoute antes de VPN de sitio a sitio.

Para más información acerca de cómo configurar conexiones coexistentes de ExpressRoute y una VPN de sitio a sitio, consulte el artículo sobre la [coexistencia de conexiones de ExpressRoute y de sitio a sitio][ExR-S2S-CoEx].

## <a name="extend-back-end-connectivity-to-spoke-vnets-and-branch-locations"></a>Ampliación de la conectividad de back-end a redes virtuales de radio y ubicaciones de sucursal

### <a name="spoke-vnet-connectivity-by-using-vnet-peering"></a>Conectividad de red virtual de radio mediante el emparejamiento de redes virtuales

La arquitectura de red virtual de tipo hub-and-spoke se usa ampliamente. El centro de conectividad es una red virtual de Azure que actúa como punto central de conectividad entre las redes virtuales de radio y la red local. Los radios son redes virtuales que se emparejan con el centro de conectividad y sirven para aislar las cargas de trabajo. El tráfico fluye entre el centro de datos local y el concentrador a través de una conexión de ExpressRoute o VPN. Para más información acerca de la arquitectura, consulte [Implementación de una topología de red en estrella tipo hub-and-spoke en Azure][Hub-n-Spoke].

En el emparejamiento de redes virtuales dentro de una región, las redes virtuales de radio pueden usar las puertas de enlace del centro de conectividad (tanto de VPN como de ExpressRoute) para comunicarse con las redes remotas.

### <a name="branch-vnet-connectivity-by-using-site-to-site-vpn"></a>Conectividad de red virtual de sucursal mediante VPN de sitio a sitio

Es posible que quiera redes virtuales en rama, que están en regiones diferentes, y redes locales para comunicarse entre sí a través de una red virtual de centro de conectividad. La solución de Azure nativa para esta configuración es la conectividad VPN de sitio a sitio mediante una VPN. Una alternativa es usar una aplicación virtual de red (NVA) para el enrutamiento en el centro de conectividad.

Para más información, consulte [¿Qué es VPN Gateway?][VPN] e [Implementación de aplicaciones virtuales de red de alta disponibilidad][Deploy-NVA].


## <a name="next-steps"></a>Pasos siguientes

Consulte las [preguntas más frecuentes de ExpressRoute][ExR-FAQ] para información acerca de:
-   Cuántos circuitos de ExpressRoute puede conectar a una puerta de enlace de ExpressRoute.
-   Cuántas puertas de enlace de ExpressRoute puede conectar a un circuito de ExpressRoute.
-   Los límites de escala de ExpressRoute.


<!--Image References-->
[1]: ./media/backend-interoperability/HubVM-SpkVM.jpg "Vista de Network Watcher de la conectividad desde el centro de conectividad a una red virtual de radio"
[2]: ./media/backend-interoperability/HubVM-BranchVM.jpg "Vista de Network Watcher de la conectividad desde el centro de conectividad a una red virtual de sucursal"
[3]: ./media/backend-interoperability/HubVM-BranchVM-Grid.jpg "Vista de la cuadrícula de Network Watcher de la conectividad desde el centro de conectividad a una red virtual de sucursal"
[4]: ./media/backend-interoperability/Loc1-HubVM.jpg "Vista de Network Performance Monitor de la conectividad desde la máquina virtual de la ubicación 1 al centro de conectividad mediante ExpressRoute 1"
[5]: ./media/backend-interoperability/Loc1-HubVM-S2S.jpg "Vista de Network Performance Monitor de la conectividad desde la máquina virtual de la ubicación 1 al centro de conectividad mediante VPN de sitio a sitio"

<!--Link References-->
[Setup]: https://docs.microsoft.com/azure/networking/connectivty-interoperability-preface
[Configuration]: https://docs.microsoft.com/azure/networking/connectivty-interoperability-configuration
[ExpressRoute]: https://docs.microsoft.com/azure/expressroute/expressroute-introduction
[VPN]: https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-about-vpngateways
[VNet]: https://docs.microsoft.com/azure/virtual-network/tutorial-connect-virtual-networks-portal
[Configuration]: https://docs.microsoft.com/azure/networking/connectivty-interoperability-configuration
[Control-Analysis]:https://docs.microsoft.com/azure/networking/connectivty-interoperability-control-plane
[Data-Analysis]: https://docs.microsoft.com/azure/networking/connectivty-interoperability-data-plane
[ExR-FAQ]: https://docs.microsoft.com/azure/expressroute/expressroute-faqs
[S2S-Over-ExR]: https://docs.microsoft.com/azure/expressroute/site-to-site-vpn-over-microsoft-peering
[ExR-S2S-CoEx]: https://docs.microsoft.com/azure/expressroute/expressroute-howto-coexist-resource-manager
[Hub-n-Spoke]: https://docs.microsoft.com/azure/architecture/reference-architectures/hybrid-networking/hub-spoke
[Deploy-NVA]: https://docs.microsoft.com/azure/architecture/reference-architectures/dmz/nva-ha
[VNet-Config]: https://docs.microsoft.com/azure/virtual-network/virtual-network-manage-peering


