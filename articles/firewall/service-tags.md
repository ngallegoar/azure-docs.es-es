---
title: Información general de las etiquetas del servicio Azure Firewall
description: Una etiqueta de servicio representa un grupo de prefijos de direcciones IP que ayudan a reducir la complejidad de la creación de reglas de seguridad.
services: firewall
author: vhorne
ms.service: firewall
ms.topic: article
ms.date: 11/19/2019
ms.author: victorh
ms.openlocfilehash: ec9fb3c9b27d21ac65888379f59d51ecc8042eb0
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "74168692"
---
# <a name="azure-firewall-service-tags"></a>Etiquetas de servicio de Azure Firewall

Una etiqueta de servicio representa un grupo de prefijos de direcciones IP que ayudan a reducir la complejidad de la creación de reglas de seguridad. No puede crear su propia etiqueta de servicio, ni especificar qué direcciones IP se incluyen dentro de una etiqueta. Microsoft administra los prefijos de direcciones que incluye la etiqueta de servicio y actualiza automáticamente esta a medida que las direcciones cambian.

Las etiquetas de servicio de Azure Firewall se pueden usar en el campo de destino de las reglas de red. Puede usarlas en lugar de direcciones IP específicas.

## <a name="supported-service-tags"></a>Etiquetas de servicio admitidas

Consulte los [grupos de seguridad](../virtual-network/security-overview.md#service-tags) para obtener una lista de etiquetas de servicio que están disponibles para su uso en las reglas de red del firewall de Azure.

## <a name="next-steps"></a>Pasos siguientes

Para más información acerca de las reglas de Azure Firewall, consulte [Lógica de procesamiento de reglas de Azure Firewall](rule-processing.md).