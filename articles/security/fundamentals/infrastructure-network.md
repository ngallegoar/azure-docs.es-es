---
title: Arquitectura de red de Azure
description: En este artículo se proporciona una descripción general de la red de infraestructura de Microsoft Azure.
services: security
documentationcenter: na
author: TerryLanfear
manager: rkarlin
ms.assetid: 61e95a87-39c5-48f5-aee6-6f90ddcd336e
ms.service: security
ms.subservice: security-fundamentals
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/08/2020
ms.author: terrylan
ms.openlocfilehash: 3b047489f9cfa3623c11e324cf58114b707c10b7
ms.sourcegitcommit: d0541eccc35549db6381fa762cd17bc8e72b3423
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/09/2020
ms.locfileid: "89567881"
---
# <a name="azure-network-architecture"></a>Arquitectura de red de Azure
La arquitectura de red de Azure proporciona conectividad de Internet a los centros de datos de Azure. Todas las cargas de trabajo implementadas (IaaS, PaaS y SaaS) en Azure aprovechan la red de centro de datos de Azure.

## <a name="network-topology"></a>Topología de red
La arquitectura de red de un centro de datos de Azure consta de los siguientes componentes:

- Red perimetral
- Red de área extensa
- Red de puertas de enlace regionales
- Red del centro de datos

![Diagrama de red de Azure](./media/infrastructure-network/network-arch.png)

## <a name="network-components"></a>Componentes de red
Breve descripción de los componentes de la red.

- Red perimetral

   - Punto de demarcación entre las redes de Microsoft y otras redes (por ejemplo, Internet, red empresarial).
   - Proporciona conexión a Internet y emparejamiento de [ExpressRoute](../../expressroute/expressroute-introduction.md) a Azure.

- Red de área extensa

   - Red troncal inteligente de Microsoft que abarca todo el mundo.
   - Proporciona conectividad entre [recursos de Azure](https://azure.microsoft.com/global-infrastructure/geographies/).

- Puerta de enlace regional

   - Punto de agregación de todos los centros de datos de una región de Azure.
   - Proporciona conectividad masiva entre centros de datos dentro de una región de Azure (por ejemplo, varios cientos de terabits por centro de datos).

- Red del centro de datos

   - Proporciona conectividad entre servidores en el centro de datos con un exceso de demanda de ancho de banda bajo.

Los componentes de red anteriores están diseñados para proporcionar la máxima disponibilidad a fin de admitir un negocio de nube siempre disponible y siempre activo. La redundancia está diseñada e integrada en la red desde el aspecto físico hasta el modo de control del protocolo.

## <a name="datacenter-network-resiliency"></a>Resistencia de red de centro de datos
Vamos a ilustrar el principio de diseño de resistencia mediante una red de centro de datos.

La red del centro de datos es una versión modificada de una [red Clos](https://en.wikipedia.org/wiki/Clos_network), lo que proporciona un ancho de banda de dos secciones elevado para el tráfico de escala en la nube. La red se construye mediante un gran número de dispositivos de mercancía para reducir el impacto que causa un error de hardware individual. Estos dispositivos se ubican estratégicamente en diferentes ubicaciones físicas con dominios de alimentación y enfriamiento independientes para reducir el impacto de un evento del entorno.  En el plano de control, todos los dispositivos de red se ejecutan como el modo de enrutamiento de nivel 3 del modelo OSI, lo que elimina el problema histórico del bucle de tráfico. Todas las rutas de acceso entre los distintos niveles están activas para proporcionar una redundancia y ancho de banda altos mediante el enrutamiento multiruta de costo equivalente (ECMP).

En el diagrama siguiente se muestra que la red del centro de datos se construye mediante diferentes niveles de dispositivos de red. Las barras del diagrama representan grupos de dispositivos de red que proporcionan redundancia y conectividad de ancho de banda elevado.

![Red del centro de datos](./media/infrastructure-network/datacenter-network.png)

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
