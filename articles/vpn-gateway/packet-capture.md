---
title: 'Azure VPN Gateway: Configuración de captura de paquetes'
description: Más información sobre las funcionalidades de capturas de paquetes que pueden usar en las VPN Gateways.
services: vpn-gateway
author: radwiv
ms.service: vpn-gateway
ms.topic: conceptual
ms.date: 10/15/2019
ms.author: radwiv
ms.openlocfilehash: 2429a8d08baa34aed120cffa069abae1fb9a3df9
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "75353518"
---
# <a name="configure-packet-captures-for-vpn-gateways"></a>Configuración de captura de paquetes para VPN Gateways

Los problemas relacionados con la conectividad y el rendimiento suelen ser complejos y tardan mucho tiempo y esfuerzo solo en limitar la causa del problema. La capacidad de las capturas de paquetes ayuda a reducir considerablemente el tiempo en limitar el ámbito del problema a determinadas partes de la red, por ejemplo, si el problema se encuentra en el lado de la red del cliente, en el lado de la red de Azure o en algún punto entre ellos. Una vez que el problema se ha reducido, es mucho más eficaz depurar y tomar acciones correctivas.

Hay algunas herramientas disponibles comúnmente para la capturas de paquetes. Sin embargo, la obtención de capturas de paquetes relevantes con estas herramientas suele resultar complicada especialmente cuando se trabaja con escenarios de tráfico de gran volumen. Las capacidades de filtrado que proporciona una captura de paquetes de VPN Gateway se convierten en un importante diferenciador. Puede usar una captura de paquetes de VPN Gateway además de las herramientas de captura de paquetes disponibles habitualmente.

## <a name="vpn-gateway-packet-capture-filtering-capabilities"></a>Capacidades de filtrado de captura de paquetes de VPN Gateway

Las capturas de paquetes de VPN Gateway se pueden ejecutar en la puerta de enlace o en una conexión específica según las necesidades del cliente. También puede ejecutar capturas de paquetes en varios túneles al mismo tiempo. Puede capturar el tráfico único o bidireccional, el tráfico de IKE y ESP y los paquetes internos junto con el filtrado en una VPN Gateway.

El uso de 5 filtros de tupla (subred de origen, subred de destino, puerto de origen, puerto de destino, protocolo) y marcas TCP (SYN, ACK, FIN, URG, PSH, RST) resulta útil al aislar problemas en un tráfico de gran volumen.

Puede usar solo una opción por propiedad mientras ejecuta la captura de paquetes.

## <a name="setup-packet-capture-using-powershell"></a>Configuración de la captura de paquetes con PowerShell

Consulte los siguientes ejemplos de comandos de PowerShell para iniciar y detener capturas de paquetes. Para más información sobre las opciones de parámetros (por ejemplo, cómo crear un filtro), consulte este [documento](https://docs.microsoft.com/powershell/module/az.network/start-azvirtualnetworkgatewaypacketcapture) de PowerShell.

### <a name="start-packet-capture-for-a-vpn-gateway"></a>Iniciar la captura de paquetes para una VPN Gateway

```azurepowershell-interactive
Start-AzVirtualnetworkGatewayPacketCapture -ResourceGroupName "YourResourceGroupName" -Name "YourVPNGatewayName"
```

Se puede usar el parámetro opcional **-FilterData** para aplicar el filtro.

### <a name="stop-packet-capture-for-a-vpn-gateway"></a>Detener la captura de paquetes para una VPN Gateway

```azurepowershell-interactive
Stop-AzVirtualNetworkGatewayPacketCapture -ResourceGroupName "YourResourceGroupName" -Name "YourVPNGatewayName" -SasUrl "YourSASURL"
```

### <a name="start-packet-capture-for-a-vpn-gateway-connection"></a>Iniciar la captura de paquetes para una conexión de VPN Gateway

```azurepowershell-interactive
Start-AzVirtualNetworkGatewayConnectionPacketCapture -ResourceGroupName "YourResourceGroupName" -Name "YourVPNGatewayConnectionName"
```

Se puede usar el parámetro opcional **-FilterData** para aplicar el filtro.

### <a name="stop-packet-capture-on-a-vpn-gateway-connection"></a>Detener la captura de paquetes en una conexión de VPN Gateway

```azurepowershell-interactive
Stop-AzVirtualNetworkGatewayConnectionPacketCapture -ResourceGroupName "YourResourceGroupName" -Name "YourVPNGatewayConnectionName" -SasUrl "YourSASURL"
```

## <a name="key-considerations"></a>Consideraciones clave

- La ejecución de capturas de paquetes puede afectar al rendimiento. Recuerde detener la captura de paquetes cuando no sea necesaria.
- La duración de la captura de paquetes mínima sugerida es de 600 segundos. Una duración de captura de paquetes más corta no proporciona datos completos debido a la sincronización de problemas entre varios componentes de la ruta de acceso.
- Los archivos de datos de captura de paquetes se generan en formato PCAP. Use Wireshark u otras aplicaciones disponibles habitualmente para abrir archivos PCAP.

## <a name="next-steps"></a>Pasos siguientes

Para más información sobre VPN Gateway, consulte [Acerca de VPN Gateway](vpn-gateway-about-vpngateways.md)
