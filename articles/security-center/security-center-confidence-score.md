---
title: Puntuación de confianza en Azure Security Center | Microsoft Docs
description: Security Center genera puntuaciones de confianza para ayudar a su equipo a determinar si una amenaza es legítima y cómo evaluar y clasificar por orden de prioridad las alertas.
services: security-center
documentationcenter: na
author: memildin
manager: rkarlin
ms.assetid: e88198f8-2e16-409d-a0b0-a62e68c2f999
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/23/2018
ms.author: memildin
ms.openlocfilehash: 8aa17f473c550c99d91862754e6a746575aca202
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "77604385"
---
# <a name="alert-confidence-score-preview"></a>Puntuación de confianza de la alerta (versión preliminar)

Azure Security Center le proporciona visibilidad sobre los recursos que se ejecutan en Azure y le avisa cuando detecta posibles problemas. El volumen de alertas puede suponer un reto para que un equipo de operaciones de seguridad las aborde de manera individual, por lo que es necesario dar prioridad a las alertas que debe investigar. La investigación de alertas puede resultar compleja y lenta y, como consecuencia, algunas alertas se ignoran.

La puntuación de confianza (actualmente en versión preliminar) de Security Center puede ayudar a su equipo a evaluar las prioridades y clasificar las alertas. Security Center aplica automáticamente procedimientos recomendados del sector, algoritmos inteligentes y procesos usados por analistas para determinar si una amenaza es legítima y le proporciona conclusiones significativas en forma de una puntuación de confianza.

## <a name="how-the-confidence-score-is-triggered"></a>¿Cómo se desencadena la puntuación de confianza?

Se generan alertas cuando se detectan procesos sospechosos que se ejecutan en las máquinas virtuales. Security Center revisa y analiza estas alertas en las máquinas virtuales Windows que se ejecutan en Azure. Realiza comprobaciones automatizadas y correlaciones con algoritmos avanzados en varios orígenes de datos y entidades a través de la organización y los recursos de Azure, y se presenta con una puntuación de confianza que permite valorar que Security Center proporciona tal confianza que la alerta es genuina y debe investigarse.

## <a name="understanding-the-confidence-score"></a>Descripción de la puntuación de confianza

La puntuación de confianza va de 1 a 100 y representa el grado de confianza de Security Center en que una alerta se tiene que investigar. Cuanto mayor sea la puntuación, más seguro está Security Center de que la alerta denota una actividad malintencionada auténtica. La puntuación de confianza incluye una lista de las principales razones de por qué la alerta ha recibido esa puntuación de confianza. La puntuación de confianza facilita a los analistas de seguridad la clasificación por orden de prioridad de su respuesta a las alertas, abordando en primer lugar los ataques más acuciantes y reduciendo en última instancia la cantidad de tiempo que se tarda en responder a los ataques y vulneraciones de seguridad.

Para ver la puntuación de confianza:
- En el portal de Security Center, abra la hoja Alertas de seguridad.
-  Las alertas e incidentes se organizan de mayor a menor, lo cual significa que cuanto mayor es el grado de confianza de Security Center en que una alerta representa una amenaza, más cerca está la alerta de la parte superior de la página. 


 ![Puntuación de confianza][1]

Para ver los datos que han contribuido a la confianza de Security Center en una alerta:
- En la hoja de Alertas de seguridad, en **Confianza**, consulte las observaciones que contribuyeron a aumentar la puntuación de confianza y obtenga más información relacionada con la alerta. Esto le brinda más información sobre la naturaleza de las actividades que provocaron la alerta.

  ![Puntuación de confianza sospechosa][2]

Use la puntuación de confianza de Security Center para clasificar por orden de prioridad las alertas del entorno. La puntuación de confianza le permite ahorrar tiempo y esfuerzos mediante la investigación automática de las alertas, la aplicación de procedimientos recomendados del sector y algoritmos inteligentes y la actuación como analista virtual para determinar qué amenazas son reales y en cuáles debe centrar su atención.


<!--Image references-->
[1]: ./media/security-center-confidence-score/confidence-score.png
[2]: ./media/security-center-confidence-score/suspicious-confidence-score.png
