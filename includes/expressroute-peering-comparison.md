---
author: cherylmc
ms.date: 12/13/2019
ms.service: expressroute
ms.topic: include
ms.author: cherylmc
ms.openlocfilehash: b6ab3e80c7a27263039de3d371adea3d49ddfcc9
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "75437020"
---
|  | **Emparejamiento privado** | **Emparejamiento de Microsoft** |  **Emparejamiento público** (en desuso para circuitos nuevos) |
| --- | --- | --- | --- |
| **Número máximo de prefijos admitidos por emparejamiento** |4000 de forma predeterminada, 10.000 con ExpressRoute Premium |200 |200 |
| **Intervalos de direcciones IP admitidas** |Cualquier dirección IP válida de la WAN. |Direcciones IP públicas propiedad suya o de su proveedor de conectividad. |Direcciones IP públicas propiedad suya o de su proveedor de conectividad. |
| **Requisitos del número de sistema autónomo (AS)** |Números de sistema autónomo (AS) públicos y privados Debe poseer el número de AS público si opta por utilizar uno. |Números de sistema autónomo (AS) públicos y privados Sin embargo, debe comprobar la titularidad de las direcciones IP públicas. |Números de sistema autónomo (AS) públicos y privados Sin embargo, debe comprobar la titularidad de las direcciones IP públicas. |
| **Protocolos IP admitidos**| IPv4 |  IPv4, IPv6 | IPv4 |
| **Direcciones IP de la interfaz de enrutamiento** |Direcciones IP públicas y de RFC1918 |Direcciones IP públicas registradas para usted en los registros de enrutamiento. |Direcciones IP públicas registradas para usted en los registros de enrutamiento. |
| **Compatibilidad con Hash MD5** |Sí |Sí |Sí |