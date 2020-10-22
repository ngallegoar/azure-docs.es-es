---
title: Solicitudes de aumento de cuota de vCPU de Azure Resource Manager
description: Solicitudes de aumento de cuota de vCPU de Azure Resource Manager
author: sowmyavenkat86
ms.author: svenkat
ms.date: 01/27/2020
ms.topic: how-to
ms.service: azure-supportability
ms.assetid: ce37c848-ddd9-46ab-978e-6a1445728a3b
ms.openlocfilehash: 7486cee351880dc0465a43b1259a5f07ef31c77b
ms.sourcegitcommit: 2989396c328c70832dcadc8f435270522c113229
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/19/2020
ms.locfileid: "92173298"
---
# <a name="quota-increase-requests"></a>Solicitudes de aumento de cuota

Las cuotas de vCPU de Resource Manager para máquinas virtuales y conjuntos de escalado de máquinas virtuales se aplican en dos niveles para cada suscripción en cada región.

El primer nivel es el límite de número total de vCPU regionales en todas las series de máquina virtual. El segundo nivel es el límite de vCPU por serie de máquina virtual, como las vCPU de la serie D. Cada vez que se va a implementar una nueva máquina virtual, la suma de las vCPU nuevas y existentes de esa serie de máquina virtual no debe superar la cuota de vCPU aprobada para la serie. Además, el número total de vCPU nuevas y existentes implementadas en todas las series de máquina virtual no debería superar la cuota del número total de vCPU regionales aprobada para la suscripción. Si se supera alguna de esas cuotas, no se permitirá la implementación de la máquina virtual.
Puede solicitar un aumento del límite de la cuota de vCPU para la serie de máquinas virtuales en Azure Portal. Al aumentar la cuota de la serie de máquina virtual, se aumenta automáticamente el límite del número total de vCPU regionales en la misma cantidad.

Cuando se crea una suscripción, el número total de vCPU regionales predeterminadas no puede ser igual a la suma de las cuotas de vCPU predeterminadas de todas las series individuales de máquina virtual. Este hecho puede dar lugar a una suscripción con suficiente cuota para cada serie individual de máquina virtual que quiera implementar. Pudiera ser que no hubiese cuota suficiente para el número total de vCPU regionales en todas las implementaciones. En este caso, deberá enviar una solicitud para aumentar explícitamente el límite del número total de vCPU regionales. El límite del número total de vCPU regionales no puede superar la suma de la cuota aprobada en todas las series de máquina virtual de la región.

> [!NOTE]
> Si desea aumentar el límite o la cuota por encima del límite predeterminado, [abra una solicitud de soporte técnico al cliente en línea sin cargo alguno](https://docs.microsoft.com/azure/azure-resource-manager/templates/error-resource-quota#solution).

Para más información sobre las cuotas, consulte [Cuotas de vCPU de máquinas virtuales](../../virtual-machines/windows/quotas.md) en [Límites, cuotas y restricciones de suscripción y servicios de Microsoft Azure](../../azure-resource-manager/management/azure-subscription-service-limits.md).

