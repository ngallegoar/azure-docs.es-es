---
title: 'Incidentes de Azure Security Center: correlaciones inteligentes de alertas'
description: En este tema se explica cómo la fusión usa la correlación de alertas inteligentes en la nube para generar incidentes de seguridad en Azure Security Center.
services: security-center
documentationcenter: na
author: memildin
manager: rkarlin
ms.assetid: e9d5a771-bfbe-458c-9a9b-a10ece895ec1
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/02/2019
ms.author: memildin
ms.openlocfilehash: b26f0bab073ce248ca23bb8a815fa3e293ddba51
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "73686493"
---
# <a name="cloud-smart-alert-correlation-in-azure-security-center-incidents"></a>Correlación de alertas inteligentes en la nube en Azure Security Center (incidentes)

Azure Security Center analiza continuamente las cargas de trabajo de la nube híbrida mediante el análisis avanzado e información sobre amenazas para alertarle de cualquier actividad malintencionada.

La magnitud de la cobertura de las amenazas está creciendo. La necesidad de detectar incluso el peligro más ligero es importante y puede ser difícil para los analistas de seguridad evaluar las distintas alertas e identificar un ataque real. Security Center ayuda a los analistas a afrontar este exceso de alertas. Ayuda a diagnosticar los ataques a medida que se producen, al correlacionar las distintas alertas y señales de baja fidelidad en incidentes de seguridad.

El análisis de fusión es la tecnología y el back-end analítico que permite generar los incidentes de Security Center, al poner en correlación diferentes alertas y señales contextuales entre sí. La fusión examina las distintas señales informadas en una suscripción en todos los recursos. La fusión encuentra patrones que revelan la progresión de los ataques o las señales con información contextual compartida, lo que indica que debe utilizar un procedimiento de respuesta unificado para ellos.

El análisis de fusión combina el conocimiento de dominios de seguridad con inteligencia artificial para analizar las alertas y detectar nuevos patrones de ataque cuando se producen. 

Security Center utiliza la matriz de ataque MITRE para asociar alertas con su intención percibida, lo que ayuda a formalizar el conocimiento de dominios de seguridad. Además, mediante el uso de la información recopilada para cada paso de un ataque, Security Center puede descartar actividades que parecen ser pasos de un ataque, pero en realidad no lo son.

Dado que los ataques se producen a menudo a través de distintos inquilinos, Security Center puede combinar algoritmos de inteligencia artificial para analizar las secuencias de ataque que se notifican en cada suscripción. Esta técnica identifica las secuencias de ataque como patrones de alerta más frecuentes, en lugar de asociarse simplemente entre sí.

Durante la investigación de un incidente, los analistas a menudo necesitan más contexto para llegar a un veredicto sobre la naturaleza de la amenaza y cómo mitigarla. Por ejemplo, incluso cuando se detecta una anomalía de red, sin saber qué más sucede en la red o con respecto al recurso objetivo, es difícil determinar las medidas que se deben tomar a continuación. En estos casos, pueden ser de utilidad los artefactos, los eventos relacionados y la información que un incidente de seguridad puede incluir. La información adicional disponible de los incidentes de seguridad variará según el tipo de amenaza detectada y la configuración de su entorno. 

![Captura de pantalla del informe de incidentes de seguridad detectados](./media/security-center-alerts-cloud-smart/security-incident.png)

Para comprender mejor qué son los incidentes de seguridad, consulte [Control de incidentes de seguridad en Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-incident).

