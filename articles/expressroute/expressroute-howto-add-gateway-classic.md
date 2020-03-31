---
title: 'Azure ExpressRoute: Incorporación de una puerta de enlace a una red virtual: clásico'
description: Configure una puerta de enlace de red virtual para un modelo de implementación clásica con PowerShell para una configuración de ExpressRoute.
services: expressroute
author: charwen
ms.service: expressroute
ms.topic: article
ms.date: 12/06/2019
ms.author: charwen
ms.custom: seodec18
ms.openlocfilehash: d7927af5b831dbe3294e1abc3a878e323bf1867e
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "74928062"
---
# <a name="configure-a-virtual-network-gateway-for-expressroute-using-powershell-classic"></a>Configuración de una puerta de enlace de red virtual para ExpressRoute mediante PowerShell (clásica)
> [!div class="op_single_selector"]
> * [Resource Manager - PowerShell](expressroute-howto-add-gateway-resource-manager.md)
> * [Clásico: PowerShell](expressroute-howto-add-gateway-classic.md)
> * [Vídeo: Azure Portal](https://azure.microsoft.com/documentation/videos/azure-expressroute-how-to-create-a-vpn-gateway-for-your-virtual-network)
> 
> 

Este artículo le guiará por los pasos para agregar, cambiar el tamaño y quitar una puerta de enlace de red virtual (VNet) para una red virtual existente. Los pasos de esta configuración son específicos para las redes virtuales que se crearon con el **modelo de implementación clásica** y que se utilizarán en una configuración ExpressRoute. 

[!INCLUDE [expressroute-classic-end-include](../../includes/expressroute-classic-end-include.md)]

**Información sobre los modelos de implementación de Azure**

[!INCLUDE [vpn-gateway-classic-rm](../../includes/vpn-gateway-classic-rm-include.md)]

## <a name="before-beginning"></a>Antes de comenzar
Compruebe que ha instalado los cmdlets de Azure PowerShell necesarios para esta configuración.

[!INCLUDE [classic powershell install instructions](../../includes/expressroute-poweshell-classic-install-include.md)]

[!INCLUDE [expressroute-gateway-classic-ps](../../includes/expressroute-gateway-classic-ps-include.md)]

## <a name="next-steps"></a>Pasos siguientes
Después de crear la puerta de enlace de red virtual, puede vincular la red virtual a un circuito ExpressRoute. Consulte el artículo [Vinculación de la red virtual a circuitos ExpressRoute](expressroute-howto-linkvnet-classic.md).

