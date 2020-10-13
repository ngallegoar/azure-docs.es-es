---
title: Información general de hosts dedicados de Azure para máquinas virtuales
description: Obtenga más información sobre cómo se pueden usar instancias de Azure Dedicated Host para implementar máquinas virtuales de Linux.
author: cynthn
ms.service: virtual-machines
ms.topic: article
ms.date: 07/28/2020
ms.author: cynthn
ms.openlocfilehash: acf79448c0dcb81bbe644be822414a7e51b0ee36
ms.sourcegitcommit: 32c521a2ef396d121e71ba682e098092ac673b30
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/25/2020
ms.locfileid: "91328163"
---
# <a name="azure-dedicated-hosts-for-virtual-machines"></a>Hosts dedicados de Azure para máquinas virtuales

Azure Dedicated Host es un servicio que proporciona servidores físicos (capaces de hospedar una o varias máquinas virtuales) dedicados a una suscripción a Azure. Los hosts dedicados son los mismos servidores físicos que se usan en nuestros centros de datos y se proporcionan como un recurso. Puede aprovisionar hosts dedicados dentro de una región, zona de disponibilidad y dominio de error. Después, puede colocar las máquinas virtuales directamente en los hosts aprovisionados, en la configuración que más se ajuste a sus necesidades.



[!INCLUDE [virtual-machines-common-dedicated-hosts](../../../includes/virtual-machines-common-dedicated-hosts.md)]


## <a name="next-steps"></a>Pasos siguientes

- Para implementar un host dedicado se pueden usar la [CLI de Azure](dedicated-hosts-cli.md), el [portal](dedicated-hosts-portal.md) y [PowerShell](../windows/dedicated-hosts-powershell.md).

- Para obtener más detalles, consulte la introducción a los [hosts dedicados](dedicated-hosts.md).

- [Aquí](https://github.com/Azure/azure-quickstart-templates/blob/master/201-vm-dedicated-hosts/README.md) encontrará una plantilla de ejemplo en la que se usan zonas y dominios de error para obtener la máxima resistencia en una región.

- También puede ahorrar costos con una [instancia reservada de Azure Dedicated Host](../prepay-dedicated-hosts-reserved-instances.md).
