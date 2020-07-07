---
title: Colocación de máquinas virtuales Linux
description: Obtenga información sobre cómo la colocación de recursos de máquina virtual de Azure puede mejorar la latencia.
ms.service: virtual-machines
ms.topic: article
ms.date: 10/30/2019
ms.author: zivr
ms.openlocfilehash: d2fd8a2cd7dac7b1d3c78691c84a861d924005ce
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/02/2020
ms.locfileid: "79226796"
---
# <a name="co-locate-resources-for-improved-latency"></a>Colocación de recursos para mejorar la latencia

Al implementar la aplicación en Azure, la propagación de instancias entre regiones o zonas de disponibilidad crea una latencia de red, lo que puede afectar al rendimiento general de la aplicación. 

## <a name="proximity-placement-groups"></a>Grupos de selección de ubicación de proximidad

[!INCLUDE [virtual-machines-common-ppg-overview](../../../includes/virtual-machines-common-ppg-overview.md)]

## <a name="next-steps"></a>Pasos siguientes

Implemente una máquina virtual en un [grupo de selección de ubicación de proximidad](proximity-placement-groups.md) mediante la CLI de Azure.

Aprenda a [probar la latencia de red](https://aka.ms/TestNetworkLatency?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

Aprenda a [optimizar el rendimiento de la red](https://docs.microsoft.com/azure/virtual-network/virtual-network-optimize-network-bandwidth?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).  

Aprenda a [usar grupos de selección de ubicación de proximidad con las aplicaciones de SAP](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-proximity-placement-scenarios?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
