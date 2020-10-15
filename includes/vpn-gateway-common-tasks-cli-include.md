---
title: archivo de inclusión
description: archivo de inclusión
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: include
ms.date: 03/21/2018
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: 61082333afc88abef0a8d8a57d1f1b1d893b6148
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "67186173"
---
### <a name="to-view-local-network-gateways"></a>Para ver las puertas de enlace de la red local

Para ver una lista de las puertas de enlace de red local, use el comando [az network local-gateway list](https://docs.microsoft.com/cli/azure/network/local-gateway).

```azurecli
az network local-gateway list --resource-group TestRG1
```

[!INCLUDE [modify-prefix](vpn-gateway-modify-ip-prefix-cli-include.md)]

[!INCLUDE [modify-gateway-IP](vpn-gateway-modify-lng-gateway-ip-cli-include.md)]

### <a name="to-verify-the-shared-key-values"></a>Para comprobar los valores de la clave compartida

Compruebe que el valor de la clave compartida es el mismo valor que utilizó para la configuración del dispositivo VPN. Si no es así, ejecute de nuevo la conexión con el valor del dispositivo, o actualice el dispositivo con el valor devuelto. Los valores deben coincidir. Para ver la clave compartida, use [az network vpn-connection-list](https://docs.microsoft.com/cli/azure/network/vpn-connection).

```azurecli
az network vpn-connection shared-key show --connection-name VNet1toSite2 --resource-group TestRG1
```
### <a name="to-view-the-vpn-gateway-public-ip-address"></a>Para ver la dirección IP pública de la puerta de enlace de VPN

Para buscar la dirección IP pública de la puerta de enlace de red virtual, use el comando [az network public-ip list](https://docs.microsoft.com/cli/azure/network/public-ip). Para facilitar la lectura, la salida de este ejemplo tiene el formato preciso para mostrar la lista de direcciones IP públicas en formato de tabla.

```azurecli
az network public-ip list --resource-group TestRG1 --output table
```
