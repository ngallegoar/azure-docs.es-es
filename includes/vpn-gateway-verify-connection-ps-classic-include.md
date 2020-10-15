---
title: archivo de inclusión
description: archivo de inclusión
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: include
ms.date: 10/17/2018
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: dd2dd84cbcd50fba48011e1836cdc64a6ad5855d
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "86040762"
---
Puede comprobar que la conexión se realizó correctamente mediante el cmdlet "Get-AzureVNetConnection".

1. Puede usar el siguiente ejemplo de cmdlet, configurando los valores para que coincidan con los tuyos. Si el nombre de la red virtual contiene espacios, debe estar entre comillas.

   ```azurepowershell
   Get-AzureVNetConnection "Group ClassicRG ClassicVNet"
   ```
2. Cuando el cmdlet haya finalizado, consulte los valores. En el ejemplo siguiente, en ConnectivityState aparece "Connected" y se pueden ver los bytes de entrada y salida.

```output
ConnectivityState         : Connected
EgressBytesTransferred    : 181664
IngressBytesTransferred   : 182080
LastConnectionEstablished : 1/7/2016 12:40:54 AM
LastEventID               : 24401
LastEventMessage          : The connectivity state for the local network site 'RMVNetLocal' changed from Connecting to
                            Connected.
LastEventTimeStamp        : 1/7/2016 12:40:54 AM
LocalNetworkSiteName      : RMVNetLocal
```
