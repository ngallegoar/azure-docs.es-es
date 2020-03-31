---
title: Arquitectura de red de Azure
description: En este artículo se proporciona una descripción general de la red de infraestructura de Microsoft Azure.
services: security
documentationcenter: na
author: TerryLanfear
manager: barbkess
editor: TomSh
ms.assetid: 61e95a87-39c5-48f5-aee6-6f90ddcd336e
ms.service: security
ms.subservice: security-fundamentals
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/20/2019
ms.author: terrylan
ms.openlocfilehash: c4756c36c2243840df69f3696e7ddac3628f3a00
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "68727175"
---
# <a name="azure-network-architecture"></a>Arquitectura de red de Azure
La arquitectura de red de Azure sigue una versión modificada del modelo estándar de núcleo/distribución/acceso del sector, con distintas capas de hardware. Las capas incluyen:

- Núcleo (enrutadores de centros de datos)
- Distribución (enrutadores de acceso y agregación L2). La capa de distribución separa el enrutamiento L3 de la conmutación L2.
- Acceso (conmutadores de host L2)

La arquitectura de red tiene dos capas de conmutadores de nivel 2. Una capa agrega el tráfico de la otra capa. La segunda capa crea un bucle para incorporar redundancia. Esta arquitectura proporciona una superficie de VLAN más flexible y mejora el escalado de los puertos. La arquitectura mantiene la distinción entre L2 y L3, lo que permite usar hardware en cada una de las distintas capas de la red y minimiza que los errores en una capa afecten a las otras capas. El uso de troncos permite compartir recursos, como la conectividad con la infraestructura de L3.

## <a name="network-configuration"></a>Network configuration (Configuración de red)
La arquitectura de red de un clúster de Azure en un centro de datos consta de los siguientes dispositivos:

- Enrutadores (centro de datos, enrutador de acceso y enrutadores de hoja de borde)
- Conmutadores (conmutadores de agregación y parte superior del bastidor)
- Digi CM
- Unidades de distribución de energía

Azure tiene dos arquitecturas diferentes. Algunos clientes y servicios compartidos existentes de Azure residen en la arquitectura LAN predeterminada (DLA), mientras que las nuevas regiones y clientes virtuales residen en la arquitectura de Quantum 10 (Q10). La arquitectura DLA es un diseño de árbol tradicional con enrutadores de acceso activo-pasivo y listas de control de acceso (ACL) de seguridad aplicadas en los enrutadores de acceso. La arquitectura de Quantum 10 es un diseño de enrutadores en malla o cerrado, donde no se aplican ACL a los enrutadores. En su lugar, las ACL se aplican debajo del enrutamiento, mediante equilibrio de carga de (SLB) o redes VLAN definidas por software.

En el siguiente diagrama se proporciona una descripción general de la arquitectura de red dentro de un clúster de Azure:

![Diagrama de red de Azure](./media/infrastructure-network/network-arch.png)

### <a name="quantum-10-devices"></a>Dispositivos Quantum 10
En el diseño de Quantum 10, la conmutación de nivel 3 se extiende por varios dispositivos en un diseño de malla o CLOS. Entre las ventajas del diseño Q10 se incluyen una mayor capacidad y una mayor posibilidad de escalar la infraestructura de red existente. El diseño emplea enrutadores de hoja de borde, conmutadores de tallo y enrutadores de la parte superior del bastidor para pasar el tráfico a los clústeres a través de varias rutas, lo que permite la tolerancia a errores. Los servicios de seguridad, como la traducción de direcciones de red, se controlan mediante el equilibrio de carga de software, en lugar de dispositivos de hardware.

### <a name="access-routers"></a>Enrutadores de acceso
Los enrutadores de distribución/acceso L3 (AR) realizan la funcionalidad de enrutamiento principal para las capas de distribución y acceso. Estos dispositivos se implementan en pareja, y son la puerta de enlace predeterminada para las subredes. Cada par de AR puede admitir varios pares de conmutador de agregación L2, según la capacidad. El número máximo depende de la capacidad del dispositivo, así como de los dominios de error. Un número típico es tres pares de conmutadores de agregación L2 por cada par de AR.

### <a name="l2-aggregation-switches"></a>Conmutadores de agregación L2  
Estos dispositivos actúan como un punto de agregación para el tráfico de L2. Son la capa de distribución para el tejido de L2 y pueden controlar una gran cantidad de tráfico. Dado que estos dispositivos agregan tráfico, requieren la funcionalidad 802.1q y tecnologías de alto ancho de banda, como la agregación de puertos y 10GE.

### <a name="l2-host-switches"></a>Conmutadores de host L2
Los hosts se conectan directamente a estos conmutadores. Pueden ser conmutadores montados en bastidor o implementaciones en chasis. El estándar 802.1q permite la designación de una VLAN como VLAN nativa, por lo que esa VLAN se trata como una trama de Ethernet normal (sin etiquetar). En circunstancias normales, las tramas en la VLAN nativa se transmiten y reciben sin etiquetar en un puerto de enlace 802.1q. Esta característica se diseñó para la migración a 802.1q y compatibilidad con dispositivos no aptos para 802.1q. En esta arquitectura, solo la infraestructura de red utiliza la VLAN nativa.

Esta arquitectura especifica un estándar para la selección de la VLAN nativa. El estándar garantiza, siempre que sea posible, que los dispositivos de AR tengan una VLAN nativa única para cada tronco y los troncos de L2Aggregation a L2Aggregation. Los troncos del conmutador de L2Aggregation a L2Host tienen una VLAN nativa no predeterminada.

### <a name="link-aggregation-8023ad"></a>Agregación de vínculos (802.3ad)
La agregación de vínculos permite agrupar varios vínculos individuales y tratarlos como un único vínculo lógico. Para facilitar la depuración operativa, el número que se usa para designar las interfaces canal-puerto debe normalizarse. El resto de la red utiliza el mismo número en ambos extremos de un canal-puerto.

Los números especificados para el conmutador de L2Agg a L2Host son los números de puerto-canal utilizados en el lado L2Agg. Dado que el intervalo de números es más limitado en el lado L2Host, el estándar es usar los números 1 y 2 en el lado L2Host. Hacen referencia al puerto-canal que va a la parte "a" y la parte "b", respectivamente.

### <a name="vlans"></a>VLAN
La arquitectura de red utiliza redes VLAN para agrupar servidores en un único dominio de difusión. Los números de VLAN cumplen el estándar 802.1q, que admite redes VLAN con numeración de 1 a 4094.

### <a name="customer-vlans"></a>Redes VLAN de clientes
Hay varias opciones para implementar redes VLAN en Azure Portal para satisfacer las necesidades de separación y arquitectura de su solución. Estas soluciones se implementan mediante máquinas virtuales. Para ver ejemplos de la arquitectura de referencia de los clientes, consulte [Arquitecturas de referencia de Azure](https://docs.microsoft.com/azure/architecture/reference-architectures/).

### <a name="edge-architecture"></a>Arquitectura de borde
Los centros de datos de Azure se basan en infraestructuras de red altamente redundantes y bien aprovisionadas. Microsoft implementa redes en los centros de datos de Azure con arquitecturas de redundancia de "necesidad más uno" (N+1) o superior. Las características de conmutación por error completas dentro de los centros de datos y entre estos ayudan a garantizar la disponibilidad de las redes y de los servicios. Externamente, los centros de datos se atienden con circuitos de red dedicados de alto ancho de banda. Estos circuitos conectan repetidamente las propiedades con más de 1200 proveedores de servicios de Internet en todo el mundo, en varios puntos de emparejamiento. Esto proporciona más de 2000 Gbps de capacidad de borde potencial en toda la red.

Los enrutadores de filtrado en el borde y la capa de acceso de la red de Azure proporcionan seguridad bien establecida a nivel de paquete y ayuda a evitar intentos no autorizados de conexión a Azure. Los enrutadores ayudan a asegurar que el contenido real de los paquetes incluye datos que tienen el formato esperado y se ajustan al esquema de comunicación de cliente/servidor esperado. Azure implementa una arquitectura en capas que consta de los siguientes componentes de segregación de redes y de control de acceso:

- **Enrutadores perimetrales**. Separan el entorno de las aplicaciones de Internet. Los enrutadores perimetrales están diseñados para proporcionar protección contra la suplantación de identidad y limitar el acceso mediante listas de control de acceso (ACL).
- **Enrutadores de distribución (acceso).** Solo permiten las direcciones IP aprobadas por Microsoft, proporcionan protección contra la suplantación de identidad y establecen conexiones mediante ACL.

### <a name="ddos-mitigation"></a>Mitigación de DDoS
Los ataques por denegación de servicio distribuido (DDoS) siguen siendo una amenaza real para la confiabilidad de los servicios en línea de Microsoft. Los ataques son cada vez más específicos y sofisticados y los servicios de Microsoft abarcan geografías cada vez más diversas, lo que hace que detectar y minimizar el impacto de estos ataques sea una prioridad alta.

[Azure DDoS Protection Estándar](../../virtual-network/ddos-protection-overview.md) proporciona una defensa contra los ataques por DDoS. Para más información, consulte [Azure DDoS Protection: procedimientos recomendados y arquitecturas de referencia](ddos-best-practices.md).

> [!NOTE]
> Microsoft ofrece protección contra DDoS de forma predeterminada para todos los clientes de Azure.
>
>

## <a name="network-connection-rules"></a>Reglas de conexión de red
Azure implementa enrutadores perimetrales en su red para proporcionar seguridad en el nivel de paquete con el fin de evitar intentos no autorizados de conectarse a Azure. Los enrutadores perimetrales aseguran que el contenido real de los paquetes incluye datos que tienen el formato esperado y se ajustan al esquema de comunicación de cliente/servidor esperado.

Los enrutadores perimetrales separan el entorno de aplicaciones de Internet. Estos enrutadores están diseñados para proporcionar protección contra la suplantación de identidad y limitar el acceso mediante listas de control de acceso (ACL). Microsoft configura estos enrutadores perimetrales mediante un enfoque de ACL en capas para limitar los protocolos de red que tienen permitido transitar por los enrutadores perimetrales y los enrutadores de acceso.

Microsoft coloca los dispositivos de red en las ubicaciones de acceso y de borde, y funcionan como puntos limítrofes donde se aplican los filtros de entrada y salida.

## <a name="next-steps"></a>Pasos siguientes
Para más información sobre lo que Microsoft hace para ayudar a proteger la infraestructura de Azure, consulte:

- [Instalaciones de Azure, entornos locales y seguridad física](physical-security.md)
- [Disponibilidad de la infraestructura de Azure](infrastructure-availability.md)
- [Componentes y límites del sistema de información de Azure](infrastructure-components.md)
- [Red de producción de Azure](production-network.md)
- [Características de seguridad de Azure SQL Database](infrastructure-sql.md)
- [Operaciones de producción y administración de Azure](infrastructure-operations.md)
- [Supervisión de la infraestructura de Azure](infrastructure-monitoring.md)
- [Integridad de la infraestructura de Azure](infrastructure-integrity.md)
- [Protección de datos de cliente de Azure](protection-customer-data.md)


