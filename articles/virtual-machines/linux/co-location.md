---
title: Colocación de máquinas virtuales Linux
description: Obtenga información sobre cómo la colocación de recursos de máquina virtual de Azure para Linux puede mejorar la latencia.
ms.service: virtual-machines
ms.topic: conceptual
ms.date: 10/30/2019
ms.author: zivr
ms.openlocfilehash: 6bf01f4da6ce96b413c87b81c14a998603cebd6b
ms.sourcegitcommit: 32c521a2ef396d121e71ba682e098092ac673b30
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/25/2020
ms.locfileid: "91279730"
---
# <a name="co-locate-resources-for-improved-latency"></a>Colocación de recursos para mejorar la latencia

Al implementar la aplicación en Azure, la propagación de instancias entre regiones o zonas de disponibilidad crea una latencia de red, lo que puede afectar al rendimiento general de la aplicación. 

## <a name="proximity-placement-groups"></a>Grupos de selección de ubicación de proximidad

[!INCLUDE [virtual-machines-common-ppg-overview](../../../includes/virtual-machines-common-ppg-overview.md)]

## <a name="next-steps"></a>Pasos siguientes

Implemente una máquina virtual en un [grupo de selección de ubicación de proximidad](proximity-placement-groups.md) mediante la CLI de Azure.

Aprenda a [probar la latencia de red](https://aka.ms/TestNetworkLatency?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

Aprenda a [optimizar el rendimiento de la red](../../virtual-network/virtual-network-optimize-network-bandwidth.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).  

Aprenda a [usar grupos de selección de ubicación de proximidad con las aplicaciones de SAP](../workloads/sap/sap-proximity-placement-scenarios.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
