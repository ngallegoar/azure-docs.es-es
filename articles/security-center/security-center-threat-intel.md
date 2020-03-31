---
title: 'Inteligencia sobre amenazas y mapa de alertas de seguridad: Azure Security Center'
description: Aprenda a usar la funcionalidad de mapa de alertas e inteligencia sobre amenazas de seguridad de Azure Security Center para identificar posibles amenazas en las máquinas virtuales y los equipos.
services: security-center
documentationcenter: na
author: rkarlin
manager: barbkess
editor: ''
ms.assetid: a771a3a1-2925-46ca-8e27-6f6a0746f58b
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/3/2018
ms.author: rkarlin
ms.openlocfilehash: 39835d9d73adcbe474d3b70dfced313e18d1a3b3
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "77603421"
---
# <a name="security-alerts-map-and-threat-intelligence"></a>Mapa de alertas de seguridad e información sobre amenazas
En este artículo obtendrá ayuda para usar el mapa de alertas de seguridad y el mapa de inteligencia sobre amenazas basada en eventos de seguridad de Azure Security Center para solucionar problemas relacionados con la seguridad.

> [!NOTE]
> El botón de asignación de *eventos* de seguridad se ha retirado el 31 de julio de 2019. Para obtener más información y servicios alternativos, consulte [Retirada de las características de Security Center (julio de 2019)](security-center-features-retirement-july2019.md#menu_securityeventsmap).


## <a name="how-the-security-alerts-map-works"></a>Cómo funciona el mapa de alertas de seguridad
Security Center le proporciona un mapa que le ayuda a identificar las amenazas de seguridad en el entorno. Por ejemplo, pueden identificar si un determinado equipo forma parte de una red de robots (botnet) y de dónde proviene la amenaza. Los equipos pueden convertirse en nodos en una de estas redes cuando los atacantes instalan de forma ilegal malware que interactúa en secreto con comandos y controles que administran la red de robots. 

Para crear este mapa, Security Center usa datos procedentes de varios orígenes dentro de Microsoft y aprovecha estos datos para crear un mapa de posibles amenazas en su entorno. 

Uno de los pasos de un [proceso de respuesta a incidentes de seguridad](https://docs.microsoft.com/azure/security-center/security-center-planning-and-operations-guide#incident-response) consiste en identificar la gravedad de los sistemas en peligro. En esta fase debe realizar las siguientes tareas:

- Determinar la naturaleza del ataque
- Determinar el punto de origen del ataque.
- Determinar la intención del ataque ¿Estaba el ataque dirigido a su organización para conseguir información concreta o fue aleatorio?
- Identificar los sistemas que se pusieron en peligro
- Identificar los archivos a los que se ha obtenido acceso y determinar la confidencialidad de esos archivos

Puede usar el mapa de alertas de seguridad de Security Center para ayudarle con estas tareas.

## <a name="access-the-security-alerts-map"></a>Acceso al mapa de alertas de seguridad
Para visualizar las amenazas actuales en su entorno, abra el mapa de alertas de seguridad:

1. Abra el panel **Security Center**.
2. En el panel izquierdo, en **Protección contra amenazas**, haga clic en **Mapa de alertas de seguridad**. Se abre el mapa.
3. Para obtener más información acerca de la alerta y recibir los pasos de corrección, haga clic en el punto de alerta en el mapa y siga las instrucciones. 
 
El mapa de alertas de seguridad se basa en alertas. Estas alertas se basan en las actividades para las que la comunicación de red estaba asociada con una dirección IP que se resolvió correctamente, así la dirección IP sea una dirección IP de riesgo conocida o no (por ejemplo, un criptominero conocido) o una dirección IP que anteriormente no se reconocía como de riesgo. El mapa proporciona alertas a través de cualquier suscripción que haya seleccionado previamente en Azure. 

Las alertas en el mapa se muestran según la ubicación geográfica de donde se detecte su origen, y se codifican por color según la gravedad. 
    ![Información de inteligencia sobre amenazas](./media/security-center-threat-intel/security-center-alert-map.png)



## <a name="see-also"></a>Consulte también
En este artículo, ha aprendido a utilizar la inteligencia sobre amenazas de Security Center para ayudarle a identificar cualquier actividad sospechosa. Para más información sobre Security Center, consulte los siguientes artículos:

* [Administración y respuesta a las alertas de seguridad en Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-managing-and-responding-alerts) Aprenda a administrar las alertas y a responder a incidentes de seguridad en Security Center.
* [Supervisión del estado de seguridad en Azure Security Center](security-center-monitoring.md). Aprenda a supervisar el estado de los recursos de Azure.
* [Comprensión de las alertas de seguridad en Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-alerts-type). Obtenga información acerca de los distintos tipos de alertas de seguridad.
* [Guía de solución de problemas de Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-troubleshooting-guide). Obtenga información acerca de cómo solucionar problemas comunes en Security Center.