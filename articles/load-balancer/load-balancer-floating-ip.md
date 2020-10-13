---
title: Configuración de la dirección IP flotante de Azure Load Balancer
description: Información general de la dirección IP flotante de Azure Load Balancer
services: load-balancer
documentationcenter: na
author: asudbring
ms.service: load-balancer
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/13/2020
ms.author: allensu
ms.openlocfilehash: e15aab3ebfe82fa97f5716769b5ff9675b4b3dc4
ms.sourcegitcommit: d479ad7ae4b6c2c416049cb0e0221ce15470acf6
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/01/2020
ms.locfileid: "91637025"
---
# <a name="azure-load-balancer-floating-ip-configuration"></a>Configuración de la dirección IP flotante de Azure Load Balancer

Load Balancer proporciona varias funcionalidades para las aplicaciones UDP y TCP.

## <a name="floating-ip"></a>Dirección IP flotante

Algunos escenarios de aplicación prefieren o requieren que varias instancias de la aplicación usen el mismo puerto en una sola máquina virtual en el grupo back-end. Entre los ejemplos comunes de reutilización de puertos se incluyen la agrupación en clústeres para alta disponibilidad, dispositivos de red virtuales y la exposición de varios puntos de conexión TLS sin volver a cifrar. Si desea reutilizar el puerto back-end en varias reglas, debe habilitar la IP flotante en la definición de la regla.

La **IP flotante** es el término de Azure para referirse a una parte de lo que se conoce como Direct Server Return (DSR). DSR consta de dos partes:

- Topología de flujo
- Esquema de asignación de direcciones IP

En un nivel de plataforma, Azure Load Balancer siempre funciona en una topología de flujo DSR independientemente de si la dirección IP flotante está habilitada o no. Esto significa que la parte de salida de un flujo siempre se reescribe correctamente para que se dirija de nuevo al origen.
Sin una IP flotante, Azure expone un esquema de asignación de direcciones IP de equilibrio para facilitar el uso (la IP de las instancias de máquina virtual). Habilitar la dirección IP flotante cambia el esquema de asignación de direcciones IP a la IP de servidor front-end del equilibrador de carga para permitir una flexibilidad adicional. Obtenga más información [aquí](load-balancer-multivip-overview.md).

## <a name="limitations"></a><a name = "limitations"></a>Limitaciones

- La dirección IP flotante no se admite actualmente en las configuraciones de IP secundarias para los escenarios de equilibrio de carga.

## <a name="next-steps"></a>Pasos siguientes

- Consulte el artículo sobre cómo [crear una instancia de Standard Load Balancer pública](quickstart-load-balancer-standard-public-portal.md) para empezar a usar un equilibrador de carga.
- Información sobre [Conexiones salientes de Azure Load Balancer](load-balancer-outbound-connections.md).
- Más información sobre [Azure Load Balancer](load-balancer-overview.md).
- Más información sobre [sondeos de mantenimiento](load-balancer-custom-probe-overview.md).
- Más información acerca de los [diagnósticos de Load Balancer Estándar](load-balancer-standard-diagnostics.md).
- Más información sobre los [grupos de seguridad de red](../virtual-network/security-overview.md).
