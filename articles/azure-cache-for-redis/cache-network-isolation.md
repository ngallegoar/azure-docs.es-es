---
title: Opciones de aislamiento de red de Azure Cache for Redis
description: En este artículo, obtendrá información sobre cómo determinar la mejor solución de aislamiento de red para sus necesidades. Analizaremos los aspectos básicos de Azure Private Link, la inserción de Azure Virtual Network (VNet) y las reglas de Azure Firewall con sus ventajas y limitaciones.
author: curib
ms.author: cauribeg
ms.service: cache
ms.topic: conceptual
ms.date: 10/15/2020
ms.openlocfilehash: b01e7ca9ff05b6eed51e1c454b8064ab28bda0d5
ms.sourcegitcommit: 8d8deb9a406165de5050522681b782fb2917762d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/20/2020
ms.locfileid: "92221894"
---
# <a name="azure-cache-for-redis-network-isolation-options"></a>Opciones de aislamiento de red de Azure Cache for Redis 
En este artículo, obtendrá información sobre cómo determinar la mejor solución de aislamiento de red para sus necesidades. Analizaremos los aspectos básicos de Azure Private Link, la inserción de Azure Virtual Network (VNet) y las reglas de Azure Firewall con sus ventajas y limitaciones.  

## <a name="azure-private-link-public-preview"></a>Azure Private Link (versión preliminar pública) 
Azure Private Link proporciona conectividad privada desde una red virtual a los servicios PaaS de Azure. Simplifica la arquitectura de red y protege la conexión entre los puntos de conexión de Azure mediante la eliminación de la exposición de los datos a la red pública de Internet. 

### <a name="advantages"></a>Ventajas
* Se admite en las instancias Básica, Estándar y Premium de Azure Cache for Redis. 
* Mediante el uso de [Azure Private Link](/azure/private-link/private-link-overview), puede conectarse a una instancia de Azure Cache desde la red virtual a través de un punto de conexión privado, al que se le asigna una dirección IP privada en una subred dentro de la red virtual. Con esto, las instancias de caché están disponibles tanto dentro de VNet como de forma pública.  
* Una vez creado un punto de conexión privado, el acceso a la red pública se puede restringir a través de la marca `publicNetworkAccess`. Esta marca se establece en `Enabled` de forma predeterminada, lo que le brinda la opción de permitir el acceso de vínculo público y privado a la memoria caché. Si se establece en `Disabled`, solo permitirá el acceso de vínculo privado. Puede establecer el valor en `Disabled` con una solicitud PATCH. Para más información, consulte [Azure Cache for Redis con Azure Private Link (versión preliminar)](cache-private-link.md). 
* Ninguna dependencia de caché externa afectará a las reglas de NSG de VNet.

### <a name="limitations"></a>Limitaciones 
* Los grupos de seguridad de red (NSG) se deshabilitan para los puntos de conexión privados. Sin embargo, si hay otros recursos en la subred, la obligatoriedad de NSG se aplicará a esos recursos.
* No se admite todavía la replicación geográfica, las reglas de firewall, la compatibilidad con la consola del portal, varios puntos de conexión por caché en clúster, la persistencia en el firewall ni las cachés insertadas en VNet. 
* Para conectarse a una caché en clúster, `publicNetworkAccess` debe establecerse en `Disabled`, y solo puede haber una conexión de punto de conexión privado.

> [!NOTE]
> Al agregar un punto de conexión privado a una instancia de caché, todo el tráfico de Redis se mueve al punto de conexión privado debido al DNS.
> Asegúrese de que se ajustan antes las reglas de firewall anteriores.  
>
>

## <a name="azure-virtual-network-injection"></a>Inserción de Azure Virtual Network 
VNet es el bloque de creación básico de una red privada en Azure. VNet permite que muchos recursos de Azure se puedan comunicar de forma segura entre ellos, con Internet y con redes en el entorno local. VNet es como una red tradicional que funcionaría en su propio centro de datos, pero con las ventajas de la infraestructura de Azure, la escala, la disponibilidad y el aislamiento. 

### <a name="advantages"></a>Ventajas
* Cuando una instancia de Azure Cache for Redis se configura con una VNet, no será posible acceder a ella públicamente, solo se podrá acceder desde máquinas virtuales y aplicaciones de dentro de VNet.  
* Cuando se combina VNet con directivas de NSG restringidas, ayuda a reducir el riesgo de la exfiltración de datos. 
* La implementación de VNet aporta mayor seguridad y aislamiento a su instancia de Azure Cache for Redis, así como a subredes, directivas de control de acceso y otras características para restringir aún más el acceso. 
* Se admite la replicación geográfica. 

### <a name="limitations"></a>Limitaciones
* Las cachés insertadas en VNet solo están disponibles para Azure Cache for Redis Premium. 
* Al usar una caché insertada en VNet, tendrá que abrir VNet para almacenar en caché las dependencias, como CRL/PKI, AKV, Azure Storage, Azure Monitor, etc.  


## <a name="azure-firewall-rules"></a>Reglas de Azure Firewall
[Azure Firewall](/azure/firewall/overview) es un servicio de seguridad de red administrado y basado en la nube que protege los recursos de Azure VNet. Se trata de un firewall como servicio con estado completo que incorpora alta disponibilidad y escalabilidad en la nube sin restricciones. Puede crear, aplicar y registrar directivas de aplicaciones y de conectividad de red a nivel central en suscripciones y redes virtuales.  

### <a name="advantages"></a>Ventajas
* Cuando se configuran las reglas de firewall, solo las conexiones de cliente de los intervalos de direcciones IP especificados pueden conectarse a la memoria caché. Siempre se permiten las conexiones de los sistemas de supervisión de Azure Cache for Redis, incluso si se configuran reglas de firewall. También se permiten las reglas de NSG que defina.  

### <a name="limitations"></a>Limitaciones
* Las reglas de firewall se pueden usar junto con memorias caché insertadas en VNet, pero actualmente no con puntos de conexión privados. 


## <a name="next-steps"></a>Pasos siguientes
* Obtenga información sobre cómo configurar una [memoria caché insertada de VNet para una instancia Premium de Azure Cache for Redis](cache-how-to-premium-vnet.md).  
* Obtenga información sobre cómo [configurar reglas de firewall para todos los niveles de Azure Cache for Redis](cache-configure.md#firewall). 
* Obtenga información sobre cómo [configurar puntos de conexión privados para todos los niveles de Azure Cache for Redis](cache-private-link.md). 
