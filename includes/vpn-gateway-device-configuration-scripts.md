---
title: archivo de inclusión
description: archivo de inclusión
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: include
ms.date: 02/07/2020
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: e84a77629026bb8885a48b8ed928699825f07115
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "77111230"
---
| **Proveedor** | **Familia de dispositivos** | **Versión de firmware** |
| --- | --- | --- |
|Cisco | ISR| IOS 15.1 (versión preliminar)|
|Cisco | ASA | ASA (*) RouteBased (IKEv2: sin BGP) para versión de ASA anterior a 9.8 |
|Cisco | ASA | ASA RouteBased (IKEv2: sin BGP) para versión de ASA superior a 9.8 |
|Juniper | SRX_GA | 12.x|
|Juniper | SSG_GA | ScreenOS 6.2.x|
|Juniper | JSeries_GA | JunOS 12.x|
|Juniper | SRX | JunOS 12.x RouteBased BGP |
|Ubiquiti| EdgeRouter| EdgeOS v1.10x RouteBased VTI|
|Ubiquiti| EdgeRouter| EdgeOS v1.10x RouteBased BGP|

> [!NOTE]
> (*) Obligatorio: NarrowAzureTrafficSelectors (habilitar la opción UsePolicyBasedTrafficSelectors) y CustomAzurePolicies (IKE/IPsec)
>
