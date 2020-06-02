---
title: Preguntas frecuentes sobre Azure Load Balancer
description: Respuestas a las preguntas frecuentes sobre Azure Load Balancer.
services: load-balancer
author: erichrt
ms.service: load-balancer
ms.topic: article
ms.date: 04/22/2020
ms.author: errobin
ms.openlocfilehash: 3be8ce241817b3b2fa03976eebe3147c1dc9c877
ms.sourcegitcommit: ac4a365a6c6ffa6b6a5fbca1b8f17fde87b4c05e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/10/2020
ms.locfileid: "83005150"
---
# <a name="frequently-asked-questions"></a>Preguntas más frecuentes

## <a name="what-types-of-load-balancer-exist"></a>¿Qué tipos de Load Balancer existen?
Equilibradores de carga internos que equilibran el tráfico dentro de una red virtual y equilibradores de carga externos que equilibran el tráfico hacia y desde un punto de conexión conectado a Internet. Para más información, consulte [Tipos de Load Balancer](components.md#frontend-ip-configurations). 

Para ambos tipos, Azure ofrece una SKU básica y una SKU estándar que tienen diferentes capacidades funcionales, de rendimiento, de seguridad y de seguimiento de estado. Estas diferencias se explican en el artículo [Comparación de SKU](skus.md).

 ## <a name="how-can-i-upgrade-from-a-basic-to-a-standard-load-balancer"></a>¿Cómo puedo actualizar de la versión Basic de Load Balancer a la versión Standard?
Consulte el artículo [Actualización de Basic a Standard](upgrade-basic-standard.md) para obtener una secuencia de comandos automatizada e instrucciones sobre la actualización de una SKU de Load Balancer.

 ## <a name="what-are-the-different-load-balancing-options-in-azure"></a>¿Cuáles son las diferentes opciones de equilibrio de carga en Azure?
Consulte la [Guía de tecnología de Load Balancer](https://docs.microsoft.com/azure/architecture/guide/technology-choices/load-balancing-overview) para conocer los servicios de equilibrio de carga disponibles y los usos recomendados para cada uno.

## <a name="where-can-i-find-load-balancer-arm-templates"></a>¿Dónde puedo encontrar plantillas de Resource Manager para Load Balancer?
Consulte la [lista de plantillas de inicio rápido de Azure Load Balancer](https://docs.microsoft.com/azure/templates/microsoft.network/loadbalancers#quickstart-templates) para conocer las plantillas de Resource Manager de las implementaciones comunes.

## <a name="how-are-inbound-nat-rules-different-from-load-balancing-rules"></a>¿En qué se diferencian las reglas NAT de entrada de las reglas de equilibrio de carga?
Las reglas NAT se usan para especificar un recurso de back-end al que enrutar el tráfico. Por ejemplo, la configuración de un puerto de equilibrador de carga específico para enviar el tráfico RDP a una máquina virtual específica. Las reglas de equilibrio de carga se usan para especificar un grupo de recursos de back-end para enrutar el tráfico y equilibrar la carga en cada instancia. Por ejemplo, una regla de equilibrador de carga puede enrutar los paquetes TCP en el puerto 80 del equilibrador de carga en un grupo de servidores web.

## <a name="next-steps"></a>Pasos siguientes
Si su pregunta no aparece en la lista anterior, envíe sus comentarios sobre esta página junto con su pregunta. Esto creará un problema de GitHub para que el equipo del producto se asegure de que las preguntas de todos nuestros valiosos clientes se respondan.
