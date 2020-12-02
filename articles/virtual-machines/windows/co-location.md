---
title: Colocación de máquinas virtuales para mejorar la latencia
description: Obtenga información sobre cómo la colocación de recursos de máquina virtual de Azure puede mejorar la latencia.
author: cynthn
ms.service: virtual-machines
ms.topic: conceptual
ms.workload: infrastructure-services
ms.date: 10/30/2019
ms.author: zivr
ms.openlocfilehash: eb7b5d3b477a511432dbd3ee8a6b382002aaab44
ms.sourcegitcommit: d60976768dec91724d94430fb6fc9498fdc1db37
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/02/2020
ms.locfileid: "96488479"
---
# <a name="co-locate-resource-for-improved-latency"></a>Colocación de un recurso para mejorar la latencia

Al implementar la aplicación en Azure, la propagación de instancias entre regiones o zonas de disponibilidad crea una latencia de red, lo que puede afectar al rendimiento general de la aplicación. 


## <a name="proximity-placement-groups"></a>Grupos de selección de ubicación de proximidad 

[!INCLUDE [virtual-machines-common-ppg-overview](../../../includes/virtual-machines-common-ppg-overview.md)]

## <a name="next-steps"></a>Pasos siguientes

Implemente una máquina virtual en un [grupo de selección de ubicación de proximidad](proximity-placement-groups.md) mediante Azure PowerShell.

Aprenda a [probar la latencia de red](../../virtual-network/virtual-network-test-latency.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

Aprenda a [optimizar el rendimiento de la red](../../virtual-network/virtual-network-optimize-network-bandwidth.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).  

Aprenda a [usar grupos de selección de ubicación de proximidad con las aplicaciones de SAP](../workloads/sap/sap-proximity-placement-scenarios.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).