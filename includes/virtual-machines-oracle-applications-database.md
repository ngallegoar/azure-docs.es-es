---
author: dlepow
ms.service: virtual-machines-linux
ms.topic: include
ms.date: 07/10/2019
ms.author: danlep
ms.openlocfilehash: 8106b1a77f11874b1b13f41bfc9e62ea800afc98
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "68361264"
---
### <a name="database-tier"></a>Nivel de base de datos

El nivel de base de datos contiene las instancias de base de datos para la aplicación. La base de datos puede ser una instancia de Oracle DB, Oracle RAC o un sistema de Oracle Exadata Database. 

Si la opción es usar Oracle DB, la instancia de base de datos se puede implementar en Azure a través de las imágenes de Oracle DB disponibles en Azure Marketplace. Como alternativa, puede usar la interconexión entre Azure y OCI para implementar Oracle DB en un modelo de PaaS en OCI.

Para Oracle RAC, puede implementar Oracle RAC en Azure CloudSimple en el modelo de IaaS o en OCI en el modelo de PaaS. Se recomienda usar un sistema de RAC de dos nodos. 

Por último, en el caso de los sistemas de Exadata, use la interconexión de OCI e implemente el sistema de Exadata en OCI. En el diagrama de arquitectura anterior se muestra un sistema de Exadata implementado en OCI en dos subredes.

En el caso de los escenarios de producción, implemente varias instancias de la base de datos en dos zonas de disponibilidad (si se implementa en Azure) o en dos dominios de disponibilidad (en OCI). Use Oracle Active Data Guard para sincronizar las base de datos principal y la que está en espera.

El nivel de base de datos solo recibe solicitudes del nivel intermedio. Se recomienda configurar un grupo de seguridad de red (lista de seguridad si la base de datos se implementa en OCI) para permitir solo solicitudes en el puerto 1521 del nivel intermedio y en el puerto 22 del servidor bastión por motivos administrativos.

En el caso de las bases de datos implementadas en OCI, se debe configurar una red de nube virtual independiente con una puerta de enlace de enrutamiento dinámico (DRG) que esté conectada al circuito de FastConnect.