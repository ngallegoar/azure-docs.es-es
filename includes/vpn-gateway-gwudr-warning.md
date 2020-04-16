---
title: archivo de inclusión
description: archivo de inclusión
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: include
ms.date: 09/28/2019
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: 97fde67c3ac7649418ed0239a2c7aa4f1a4b3f96
ms.sourcegitcommit: 8dc84e8b04390f39a3c11e9b0eaf3264861fcafc
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/13/2020
ms.locfileid: "81275039"
---
Las rutas definidas por el usuario con un destino 0.0.0.0/0 y NSG en GatewaySubnet **no se admiten**. Se bloqueará la creación de puertas de enlace creadas con esta configuración. Las puertas de enlace requieren acceso a los controladores de administración para que funcionen correctamente. [Propagación de rutas BGP](https://docs.microsoft.com/azure/virtual-network/virtual-networks-udr-overview#border-gateway-protocol) debe establecerse en "Habilitado" en GatewaySubnet para garantizar la disponibilidad de la puerta de enlace. Si se establece en deshabilitado, la puerta de enlace no funcionará.
