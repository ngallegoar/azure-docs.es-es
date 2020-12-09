---
title: Configuración de la continuidad empresarial y la recuperación ante desastres (BCDR) en una red privada virtual (VPN) de Azure Stack Edge
description: Se describe cómo configurar BCDR en una VPN de Azure Stack Edge.
services: databox
author: alkohli
ms.service: databox
ms.subservice: edge
ms.topic: article
ms.date: 04/17/2020
ms.author: alkohli
ms.openlocfilehash: 4d735c623a6dffe24108d06d00caa7fba987c7df
ms.sourcegitcommit: 6a350f39e2f04500ecb7235f5d88682eb4910ae8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/01/2020
ms.locfileid: "96465804"
---
# <a name="configure-business-continuity-and-disaster-recovery-for-azure-stack-edge-vpn"></a>Configuración de la continuidad empresarial y recuperación ante desastres para una VPN de Azure Stack Edge

[!INCLUDE [applies-to-r-skus](../../includes/azure-stack-edge-applies-to-r-sku.md)]

En este artículo se describe cómo configurar la continuidad empresarial y la recuperación ante desastres (BCDR) en una red privada virtual (VPN) configurada en un dispositivo de Azure Stack Edge.

Este artículo es aplicable a los dispositivos de Azure Stack Edge Pro R y Azure Stack Edge Mini R.

## <a name="configure-failover-to-a-paired-region"></a>Configuración de la conmutación por error a una región emparejada

El dispositivo de Azure Stack Edge usa otros servicios de Azure; por ejemplo, Azure Storage. Puede configurar BCDR en cualquier servicio de Azure específico que use el dispositivo de Azure Stack Edge. Si uno de los servicios de Azure que usa Azure Stack Edge conmuta por error a su región emparejada, el dispositivo de Azure Stack Edge se conectará ahora a las nuevas direcciones IP y la comunicación no se cifrará dos veces. 

El dispositivo de Azure Stack Edge usa la tunelización dividida y todos los datos y servicios que están configurados en la región principal (la región asociada con el dispositivo de Azure Stack Edge) pasan por el túnel de VPN. Si los servicios de Azure conmutan por error a una región emparejada que está fuera de la región principal, los datos ya no pasarán a través de la VPN y, por lo tanto, no se cifrarán dos veces. 

En este escenario, normalmente solo se ven afectados algunos servicios de Azure. Para solucionar este problema, se deben realizar los siguientes cambios en la configuración de la VPN de Azure Stack Edge:

1. Agregue los intervalos IP de conmutación por error del servicio de Azure en las rutas inclusivas para VPN de Azure Stack Edge. Los servicios comenzarán a enrutarse a través de la VPN.

    Para agregar las rutas inclusivas, debe descargar el archivo JSON que tiene las rutas específicas del servicio. Asegúrese de actualizar este archivo con las rutas nuevas.
2. Agregue los intervalos IP correspondientes del servicio de Azure en la tabla de enrutamiento de Azure.
3. Agregue las rutas al firewall.

> [!NOTE]
>
> 1. La conmutación por error de una instancia de Azure VPN Gateway y Azure Virtual Network (VNET) se trata en la sección [Recuperación de una región de Azure que produjo un error debido a un desastre](#recover-from-a-failed-azure-region).
> 2. Los intervalos IP agregados en la tabla de enrutamiento de Azure podrían superar el límite de 400. Si esto sucede, tendrá que seguir las instrucciones de la sección [Pasar de una región de Azure a otra](#move-from-an-azure-region-to-another).

## <a name="recover-from-a-failed-azure-region"></a>Recuperación de una región de Azure con errores

En caso de que se produzca una conmutación por error de toda una región de Azure debido a un evento catastrófico como un terremoto, todos los servicios de Azure de esa región, incluido el servicio de Azure Stack Edge, conmutarán por error. Dado que hay varios servicios, las rutas inclusivas pueden fácilmente llegar a unos cuantos cientos. Azure tiene un límite de 400 rutas. 

Cuando la región conmuta por error, la red virtual también conmuta por error a la nueva región, al igual que la puerta de enlace de red virtual (VPN Gateway). Para responder a este cambio, realice los siguientes cambios en la configuración de la VPN de Azure Stack Edge:

1. Cambie la red virtual a la región de destino. Para más información, consulte: [Traslado de una red virtual de Azure a otra región mediante Azure Portal](../virtual-network/move-across-regions-vnet-portal.md).
2. Implemente una nueva instancia de Azure VPN Gateway en la región de destino a la que trasladó la red virtual. Para obtener más información, consulte [Creación de una puerta de enlace de red virtual](../vpn-gateway/vpn-gateway-howto-point-to-site-resource-manager-portal.md#creategw).
3. Actualice la configuración de VPN de Azure Stack Edge para que use la puerta de enlace de VPN anterior en la conexión VPN y, luego, seleccione la región de destino para agregar las rutas que usarán la puerta de enlace de VPN.
4. Actualice la tabla de enrutamiento de Azure entrante si también cambia el grupo de direcciones de cliente. 

## <a name="move-from-an-azure-region-to-another"></a>Traslado de una región de Azure a otra

Puede trasladar el dispositivo de Azure Stack Edge de una ubicación a otra. Para usar una región más cercana a la ubicación en la que se implementó el dispositivo, debe configurar el dispositivo para una nueva región principal. Se han realizado los siguientes cambios:

1. Puede actualizar la configuración de VPN de Azure Stack Edge para usar la puerta de enlace de VPN de una nueva región y seleccionar la nueva región para agregar rutas que usen la puerta de enlace de VPN.

## <a name="next-steps"></a>Pasos siguientes

[Copia de seguridad del dispositivo de Azure Stack Edge](azure-stack-edge-gpu-prepare-device-failure.md).