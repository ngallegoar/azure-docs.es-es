---
title: archivo de inclusión
description: archivo de inclusión
services: networking
author: anzaman
ms.service: VPN Gateway
ms.topic: include
ms.date: 08/25/2020
ms.author: alzam
ms.custom: include file
ms.openlocfilehash: 9fe9ef5549ced3b73d18d553fa0b62ec019684fe
ms.sourcegitcommit: c95e2d89a5a3cf5e2983ffcc206f056a7992df7d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/24/2020
ms.locfileid: "95557768"
---
| Resource                                | Límite        |
|-----------------------------------------|------------------------------|
| Prefijos de dirección de red virtual                   | 600 por puerta de enlace de VPN          |
| Agregar rutas BGP                    | 4000 por puerta de enlace de VPN        |
| Prefijos de direcciones de puerta de enlace de red local  | 1000 por puerta de enlace de red local               |
| de conexiones de sitio a sitio                         | [Depende de la SKU de puerta de enlace](../articles/vpn-gateway/vpn-gateway-about-vpngateways.md#gwsku)|
| Conexiones de punto a sitio                         | [Depende de la SKU de puerta de enlace](../articles/vpn-gateway/vpn-gateway-about-vpngateways.md#gwsku) |
| Límite de ruta de P2S: IKEv2                 | 256 si no es de Windows **/** 25 si es de Windows           |
| Límite de ruta de P2S: OpenVPN               | 1000                         |
| Máx. flows                              | 100 000 para VpnGw1/AZ  **/** 512 000 para VpnGw2-4/AZ|