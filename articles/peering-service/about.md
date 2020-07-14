---
title: Introducción a Azure Peering Service
description: Conozca la información general de Azure Peering Service.
services: peering-service
author: derekolo
ms.service: peering-service
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: Infrastructure-services
ms.date: 05/18/2020
ms.author: derekol
ms.openlocfilehash: 548d4f90bd9632e4807547120ac1be589668e8fa
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/02/2020
ms.locfileid: "84871345"
---
# <a name="azure-peering-service-overview"></a>Introducción a Azure Peering Service

Azure Peering Service es un servicio de red que mejora la conectividad de los clientes a los servicios en la nube de Microsoft, como Office 365, Dynamics 365, servicios de software como servicio (SaaS), Azure o cualquier otro servicio de Microsoft accesible a través de la red pública de Internet. Microsoft se ha asociado con proveedores de acceso a Internet (ISP), asociados de intercambio de Internet (IXP) y proveedores de interconexión de nube definidos por software (SDCI) de todas las partes del mundo con el objeto de proporcionar conectividad pública confiable y de alto rendimiento con un enrutamiento óptimo del cliente a la red de Microsoft.

Con Peering Service, los clientes pueden seleccionar un proveedor de servicios asociado bien conectado en una región determinada. La conectividad pública está optimizada para conseguir alta confiabilidad y mínima latencia desde los servicios en la nube hasta la ubicación del usuario final.

![Conectividad distribuida a la nube de Microsoft](./media/peering-service-about/peering-service-what.png)

Los clientes también pueden optar a la telemetría de Peering Service, como las medidas de latencia del usuario hasta la red de Microsoft, la supervisión de rutas BGP y las alertas frente a fugas y secuestros mediante el registro de la conexión de Peering Service en Azure Portal. 

Para usar Peering Service, no es necesario que los clientes se registren en Microsoft. El único requisito es ponerse en contacto con un [asociado de Peering Service](location-partners.md) para obtener el servicio. Para optar a la telemetría de Peering Service, los clientes deben registrarse en Azure Portal.

Puede encontrar instrucciones sobre cómo registrar Peering Service en [Registro de Peering Service mediante Azure Portal](azure-portal.md). 

> [!NOTE]
> Este artículo está destinado a los arquitectos de red que se encargan de la conectividad empresarial a la nube y a Internet.


## <a name="what-is-peering-service"></a>¿Qué es Peering Service?

Peering Service es:

- Un servicio IP que usa la red pública de Internet. 
- Una plataforma de colaboración con proveedores de servicios y un servicio de valor agregado pensado para ofrecer un enrutamiento óptimo y confiable al cliente a través de los proveedores de servicios asociados a la nube de Microsoft a través de la red pública.

Peering Service no es un producto de conectividad privada como Azure ExpressRoute ni un producto de VPN.

> [!NOTE]
> Para más información sobre ExpressRoute, consulte [Documentación de ExpressRoute](https://docs.microsoft.com/azure/expressroute/).
>

## <a name="background"></a>Información previa

Office 365, Dynamics 365 y cualquier otro servicio SaaS de Microsoft se hospedan en varios centros de datos de Microsoft y se puede acceder a ellos desde cualquier ubicación geográfica. La red global de Microsoft tiene ubicaciones de punto de presencia (PoP) de Microsoft Edge en todo el mundo donde pueda conectar a un usuario final a través de sus proveedores de servicios. 

Microsoft y los proveedores de servicios asociados garantizan que el tráfico de los prefijos registrados con una conexión de Peering Service entra y sale de las ubicaciones PoP de Microsoft Edge más próximas de la red global de Microsoft. Microsoft garantiza que el tráfico de red que sale de los prefijos registrados con conexiones de Peering Service toma las ubicaciones PoP de Microsoft Edge más próximas de la red global de Microsoft.

![Conectividad pública y la red de Microsoft](./media/peering-service-about/peering-service-background-final.png)

> [!NOTE]
> Para más información sobre la red global de Microsoft, consulte [Red global de Microsoft](https://docs.microsoft.com/azure/networking/microsoft-global-network).
>

## <a name="why-use-peering-service"></a>¿Por qué usar Peering Service?

Las empresas que quieran acceso a la nube a través de Internet como primer requisito o que consideren la arquitectura SD-WAN, o que hagan un uso elevado de los servicios SaaS de Microsoft, necesitan una conectividad a Internet sólida y de alto rendimiento. Los clientes pueden lograr esa transición con Peering Service. Microsoft y los proveedores de servicios se han asociado para ofrecer conectividad pública confiable y centrada en el rendimiento a la nube de Microsoft. A continuación, se enumeran algunas de las características clave de los clientes:

- El mejor enrutamiento público a través de Internet hacia Azure Cloud Services para conseguir un rendimiento y una confiabilidad óptimos.
- Posibilidad de seleccionar el proveedor de servicios que prefiera para conectarse a la nube de Microsoft.
- Información sobre el tráfico como, por ejemplo, notificaciones sobre la latencia y supervisión de prefijos.
- Saltos de red óptimos (saltos AS) desde la nube de Microsoft.
- Análisis de rutas y estadísticas: Eventos de anomalías de rutas ([BGP](https://en.wikipedia.org/wiki/Border_Gateway_Protocol)) (detección de fugas o secuestros) y enrutamiento por debajo del nivel óptimo.

### <a name="robust-reliable-peering"></a>Emparejamiento sólido y confiable

Peering Service usa dos tipos de redundancia:

- **Redundancia local**

   Microsoft y los proveedores de servicios se interconectan a través de varias ubicaciones PoP de Microsoft Edge para ofrecer el servicio Peering Service. En cada ubicación, la interconexión debe admitir la conmutación por error entre dos enrutadores.

   Cada ubicación de emparejamiento se aprovisiona con vínculos de emparejamiento redundantes y diversos.

- **Redundancia geográfica**

   Microsoft se ha interconectado con proveedores de servicios en varias áreas metropolitanas, de modo que, si el rendimiento de uno de los nodos perimetrales se ha degradado, el tráfico se enruta hacia y desde Microsoft por medio de sitios alternativos. Para obtener un rendimiento óptimo, Microsoft enruta el tráfico de su red global mediante directivas de enrutamiento basadas en SDN.

    En este tipo de redundancia se usa la ruta de acceso de enrutamiento más corta, para lo cual se selecciona siempre el PoP de Microsoft Edge más cercano al usuario final, y se garantiza que el cliente está a un salto de red (saltos AS) de Microsoft.

   ![Redundancia geográfica](./media/peering-service-about/peering-service-geo-shortest.png)

### <a name="optimal-routing"></a>Enrutamiento óptimo

La siguiente técnica de enrutamiento es la preferida:

-  **Enrutamiento de "patata fría"**

   La técnica de enrutamiento de "patata fría" definida por software ofrece control sobre el tráfico de red que se origina en la nube de Microsoft. Esta técnica garantiza que el tráfico permanece en la red global de Microsoft de gran capacidad, baja latencia y alta confiabilidad hasta que esté lo más cerca posible del destino.
   
   El enrutamiento que no usa la técnica de "patata fría" se conoce como enrutamiento de "patata caliente". Con el enrutamiento de "patata caliente", el tráfico que se origina en la nube de Microsoft pasa a través de Internet.

   ![Enrutamiento de "patata fría"](./media/peering-service-about/peering-service-cold-potato.png)

### <a name="monitoring-platform"></a>Supervisión de la plataforma

   Se ofrece la supervisión del servicio para analizar el tráfico y el enrutamiento de los clientes, y proporciona las siguientes funcionalidades: 

-  **Detección de anomalías de ruta BGP de Internet**
          
   Este servicio se usa para detectar los eventos de anomalía de ruta, como el secuestro de rutas a los prefijos del cliente, y alertar sobre ellos.

-  **Latencia del cliente**

   Este servicio supervisa el rendimiento del enrutamiento entre la ubicación del cliente y Microsoft. 
   
   El rendimiento del enrutamiento se mide mediante la validación del tiempo de ida y vuelta que tarda el cliente en llegar al PoP de Microsoft Edge. Los clientes pueden ver los informes de latencia de diferentes ubicaciones geográficas.

   Durante la supervisión se capturan los eventos por si se diera el caso de una degradación del servicio.

   ![Supervisión de la plataforma de Peering Service](media/peering-service-about/peering-service-latency-report.png)

### <a name="traffic-protection"></a>Protección del tráfico

El enrutamiento se produce solo a través de una ruta de acceso preferida que se define cuando el cliente se registra en Peering Service.

Microsoft garantiza que, en caso de detectarse actividad malintencionada, el tráfico se enrutará a través de las rutas de acceso preferidas.

Las anomalías de ruta BGP se muestran en Azure Portal, si las hay.

## <a name="next-steps"></a>Pasos siguientes

- Para más información sobre las conexiones de Peering Service, consulte [Conexiones de Peering Service](connection.md).
- Para más información sobre la telemetría de la conexión de Peering Service, consulte [Telemetría de la conexión de Peering Service](connection-telemetry.md).
- Para encontrar un proveedor de servicios asociado, consulte [Proveedores de servicios asociados y ubicaciones](location-partners.md).
- Para incorporar una conexión de Peering Service, consulte [Modelo de incorporación de Azure Peering Service](onboarding-model.md).
- Para registrar una conexión mediante Azure Portal, consulte [Registro de una conexión de Peering Service mediante Azure Portal](azure-portal.md).
- Para medir la telemetría, consulte [Medición de la telemetría de la conexión](measure-connection-telemetry.md).
