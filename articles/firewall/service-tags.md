---
title: Información general de las etiquetas del servicio Azure Firewall
description: Una etiqueta de servicio representa un grupo de prefijos de direcciones IP que ayudan a reducir la complejidad de la creación de reglas de seguridad.
services: firewall
author: vhorne
ms.service: firewall
ms.topic: article
ms.date: 11/19/2019
ms.author: victorh
ms.openlocfilehash: 47377817b62d33e8af79e4a0d2dceb68ba9dbdc5
ms.sourcegitcommit: 8e7316bd4c4991de62ea485adca30065e5b86c67
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/17/2020
ms.locfileid: "94658654"
---
# <a name="azure-firewall-service-tags"></a>Etiquetas de servicio de Azure Firewall

Una etiqueta de servicio representa un grupo de prefijos de direcciones IP que ayudan a reducir la complejidad de la creación de reglas de seguridad. No puede crear su propia etiqueta de servicio, ni especificar qué direcciones IP se incluyen dentro de una etiqueta. Microsoft administra los prefijos de direcciones que incluye la etiqueta de servicio y actualiza automáticamente esta a medida que las direcciones cambian.

Las etiquetas de servicio de Azure Firewall se pueden usar en el campo de destino de las reglas de red. Puede usarlas en lugar de direcciones IP específicas.

## <a name="supported-service-tags"></a>Etiquetas de servicio admitidas

Consulte los [grupos de seguridad](../virtual-network/network-security-groups-overview.md#service-tags) para obtener una lista de etiquetas de servicio que están disponibles para su uso en las reglas de red del firewall de Azure.

## <a name="next-steps"></a>Pasos siguientes

Para más información acerca de las reglas de Azure Firewall, consulte [Lógica de procesamiento de reglas de Azure Firewall](rule-processing.md).