---
title: archivo de inclusión
description: archivo de inclusión
services: virtual-wan
author: cherylmc
ms.service: virtual-wan
ms.topic: include
ms.date: 10/06/2020
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: 5a3b9160f1fb28c68bdbaa540c78b2890dbcab82
ms.sourcegitcommit: 5abc3919a6b99547f8077ce86a168524b2aca350
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/07/2020
ms.locfileid: "91812789"
---
* Tiene una red virtual a la que quiere conectarse. Compruebe que ninguna de las subredes de sus redes locales se superpone a las redes virtuales a las que quiere conectarse. Para crear una red virtual en Azure Portal, consulte este [Inicio rápido](../articles/virtual-network/quick-create-portal.md).

* Su red virtual no tiene ninguna puerta de enlace de red virtual. Si la red virtual tiene alguna puerta de enlace (ya sea VPN o ExpressRoute), tiene que quitarla. Esta configuración requiere que las redes virtuales estén conectadas a la puerta de enlace del centro de conectividad de Virtual WAN.

* Obtenga un intervalo de direcciones IP para la región del concentrador. El centro de conectividad es una red virtual que Virtual WAN crea y usa. El intervalo de direcciones que especifique para el centro de conectividad no se puede superponer a ninguna de las redes virtuales existentes a las que ya esté conectado. Igualmente no se puede superponer a los intervalos de direcciones con las que esté conectadas en el entorno local. Si no está familiarizado con los intervalos de direcciones IP ubicados en la configuración de la red local, póngase de acuerdo con alguien que pueda proporcionarle estos detalles.

* Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).
