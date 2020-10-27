---
title: Alta disponibilidad y recuperación ante desastres
titleSuffix: Azure Digital Twins
description: Describe las características de Azure y Azure Digital Twins que lo ayudarán a crear soluciones de Azure IoT de alta disponibilidad con funcionalidades de recuperación ante desastres.
author: baanders
ms.author: baanders
ms.date: 10/14/2020
ms.topic: conceptual
ms.service: digital-twins
ms.openlocfilehash: 85fd5a4246e891ef6640438b07e12a9c32ad12fa
ms.sourcegitcommit: 30505c01d43ef71dac08138a960903c2b53f2499
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/15/2020
ms.locfileid: "92094202"
---
# <a name="azure-digital-twins-high-availability-and-disaster-recovery"></a>Alta disponibilidad y recuperación ante desastres de Azure Digital Twins

Un área clave que hay que considerar para tener soluciones resistentes de IoT es la continuidad empresarial y recuperación ante desastres. El diseño de **alta disponibilidad (HA)** y **recuperación ante desastres (DR)** puede ayudarlo a definir y alcanzar los objetivos de tiempo de actividad adecuados para su solución.

En este artículo se describen las características de alta disponibilidad y recuperación ante desastres que ofrece específicamente el servicio Azure Digital Twins.

Azure Digital Twins admite estas opciones de características:
* *Alta disponibilidad dentro de una región* : redundancia integrada para cumplir con el tiempo de actividad del servicio.
* *Recuperación ante desastres entre regiones* : conmutación por error en una región de Azure emparejada geográficamente en caso de un error inesperado en el centro de datos.

También puede consultar la sección [*Procedimientos recomendados*](#best-practices) para ver instrucciones generales de Azure sobre el diseño de alta disponibilidad y recuperación ante desastres.

## <a name="intra-region-ha"></a>Alta disponibilidad dentro de una región
 
Azure Digital Twins ofrece alta disponibilidad dentro de una región mediante la implementación de redundancias en el servicio. **No es necesario que los desarrolladores de una solución de Azure Digital Twins hagan ningún trabajo adicional para poder beneficiarse de estas características de alta disponibilidad.** Aunque Azure Digital Twins ofrece una garantía de tiempo de actividad razonablemente elevada, todavía es posible que ocurran errores transitorios, al igual que sucede con cualquier plataforma de informática distribuida. Se deben integrar directivas de reintento adecuadas en los componentes que interactúan con una aplicación en la nube para solucionar errores transitorios.

## <a name="cross-region-dr"></a>Recuperación ante desastres entre regiones

Podría haber algunas situaciones excepcionales cuando un centro de datos sufre interrupciones prolongadas debido a problemas con el suministro eléctrico u otros eventos en la región. Estos eventos son poco frecuentes, durante los cuales es posible que no sea útil la funcionalidad de alta disponibilidad dentro de una región que se describió anteriormente. Azure Digital Twins aborda esta situación con la conmutación por error iniciada por Microsoft.

La **conmutación por error iniciada por Microsoft** la ejecuta Microsoft en situaciones excepcionales para conmutar por error todas las instancias de Azure Digital Twins desde una región afectada hasta la región emparejada geográficamente correspondiente. Este proceso es una opción predeterminada (los usuarios no pueden omitirla) y no requiere ninguna intervención por parte del usuario. Microsoft se reserva el derecho a tomar una determinación sobre cuándo ejecutar esta opción. Este mecanismo no precisa de consentimiento del usuario antes de realizar la conmutación por error de la instancia del usuario.

>[!NOTE]
> Algunos servicios de Azure también proporcionan una opción adicional denominada **conmutación por error iniciada por el cliente** , que permite que los clientes inicien una conmutación por error solo para esa instancia, como la ejecución de un simulacro de recuperación ante desastres. Actualmente, Azure Digital Twins **no admite** este mecanismo. 

## <a name="best-practices"></a>Procedimientos recomendados

Para conocer los procedimientos recomendados de alta disponibilidad y recuperación ante desastres, consulte estas instrucciones de Azure sobre el tema: 
* En el artículo de [*orientación técnica de la continuidad empresarial de Azure*](/azure/architecture/framework/resiliency/overview) se describe un marco general que ayuda a reflexionar sobre la continuidad empresarial y recuperación ante desastres. 
* El documento [*Recuperación ante desastres y alta disponibilidad para aplicaciones de Azure*](/azure/architecture/framework/resiliency/backup-and-recovery) proporciona una guía de arquitectura enfocada a estrategias para que las aplicaciones de Azure logren alta disponibilidad y recuperación ante desastres.

## <a name="next-steps"></a>Pasos siguientes 

Obtenga más información sobre cómo empezar a utilizar las soluciones de Azure Digital Twins:
 
* [*¿Qué es Azure Digital Twins?*](overview.md)
* [*Guía de inicio rápido: Exploración de un escenario de ejemplo*](quickstart-adt-explorer.md)