---
title: Recuperación ante desastres geográfica de Azure Spring Cloud | Microsoft Docs
description: Más información sobre cómo proteger su aplicación de Spring Cloud ante interrupciones regionales
author: bmitchell287
ms.service: spring-cloud
ms.topic: conceptual
ms.date: 10/24/2019
ms.author: brendm
ms.openlocfilehash: 4961e5a63e5bc1933cf19b1f291b521d89cbda0e
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "76279153"
---
# <a name="azure-spring-cloud-disaster-recovery"></a>Recuperación ante desastres de Azure Spring Cloud

En este artículo se explican algunas estrategias que puede usar para proteger las aplicaciones de Azure Spring Cloud contra tiempos de inactividad.  Cualquier centro de datos o región puede experimentar tiempos de inactividad causados por desastres regionales, pero una planeación cuidadosa puede mitigar el impacto en los clientes.

## <a name="plan-your-application-deployment"></a>Planeamiento de la implementación de la aplicación

Las aplicaciones de Azure Spring Cloud se ejecutan en una región específica.  Azure funciona en varias ubicaciones geográficas del mundo. Una ubicación geográfica de Azure es un área definida del mundo que contiene al menos una región de Azure. Una región de Azure es un área dentro de una ubicación geográfica que contiene uno o varios centros de datos.  Cada región de Azure se empareja con otra región de la misma zona geográfica, que juntas forman un emparejamiento regional. Azure serializa las actualizaciones de plataforma (mantenimiento planeado) entre pares regionales, lo que garantiza que solo se actualice una región de cada par cada vez. Si se produce una interrupción que afecte a varias regiones, se dará prioridad a la recuperación de al menos una región de cada par.

Para garantizar la alta disponibilidad y la protección ante desastres, tiene que implementar las aplicaciones de Spring Cloud en varias regiones.  Azure proporciona una lista de [regiones emparejadas](../best-practices-availability-paired-regions.md) para que se puedan planear las implementaciones de Spring Cloud en pares regionales.  Se recomienda tener en cuenta tres factores clave al diseñar la arquitectura de microservicio: disponibilidad en regiones, regiones emparejadas de Azure y disponibilidad del servicio.

*  Disponibilidad en regiones:  Elija un área geográfica cercana a sus usuarios para minimizar el retraso de la red y el tiempo de transmisión.
*  Regiones emparejadas de Azure:  Elija regiones emparejadas dentro del área geográfica elegida para garantizar las actualizaciones coordinadas de la plataforma y los esfuerzos de recuperación prioritarios si es necesario.
*  Disponibilidad del servicio:   Decida si los niveles de acceso de las regiones emparejadas deben ser frecuente/frecuente, frecuente/normal o frecuente/poco frecuente.

## <a name="use-azure-traffic-manager-to-route-traffic"></a>Uso de Azure Traffic Manager para enrutar el tráfico

[Azure Traffic Manager](../traffic-manager/traffic-manager-overview.md) ofrece un equilibrio de carga de tráfico basado en DNS y puede distribuir el tráfico de red entre varias regiones.  Use Azure Traffic Manager para dirigir a los clientes a la instancia del servicio Azure Spring Cloud más cercana.  Para conseguir los máximos niveles de rendimiento y redundancia, dirija todo el tráfico de aplicaciones a través de Azure Traffic Manager antes de enviarlo al servicio Azure Spring Cloud.

Si tiene aplicaciones de Azure Spring Cloud en varias regiones, use Azure Traffic Manager para controlar el flujo de tráfico a las aplicaciones en cada una de las regiones.  Defina un punto de conexión de Azure Traffic Manager para cada servicio con la dirección IP del servicio. Los clientes deben conectarse a un nombre DNS de Azure Traffic Manager que apunte al servicio Azure Spring Cloud.  Azure Traffic Manager equilibra el tráfico a través de los puntos de conexión definidos.  Si se produce un desastre en un centro de datos, Azure Traffic Manager dirigirá el tráfico de esa región a su par, lo que garantiza la continuidad del servicio.