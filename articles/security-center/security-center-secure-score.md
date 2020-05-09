---
title: Puntuación de seguridad en Azure Security Center | Microsoft Docs
description: " Dé prioridad a las recomendaciones de seguridad mediante la puntuación segura de Azure Security Center. "
services: security-center
documentationcenter: na
author: memildin
manager: rkarlin
ms.assetid: c42d02e4-201d-4a95-8527-253af903a5c6
ms.service: security-center
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/15/2019
ms.author: memildin
ms.openlocfilehash: 30405ce5cc875144fcd1cf83d4a3f883a0304989
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/28/2020
ms.locfileid: "79415767"
---
# <a name="improve-your-secure-score-in-azure-security-center"></a>Mejorar la puntuación segura de Azure Security Center

> [!NOTE]
> En este artículo se trata la versión anterior de la puntuación segura. Esta experiencia de puntuación segura todavía está disponible desde la interfaz de usuario, pero se retirará paulatinamente con el tiempo. Las dos experiencias de puntuación segura se ejecutan en paralelo para permitir una transición más fluida.
>
> Puede obtener más información sobre la puntuación segura más reciente [aquí](secure-score-security-controls.md).
>

Con tantos servicios que ofrecen ventajas de seguridad, a menudo es difícil saber qué pasos dar primero para asegurar y proteger su carga de trabajo. La puntuación de seguridad revisa las recomendaciones de seguridad y les asigna una prioridad, para que usted sepa qué recomendaciones realizar primero. De esta manera, puede encontrar las vulnerabilidades de seguridad más graves y así dar prioridad a la investigación. La puntuación de seguridad es una herramienta que le ayuda a evaluar la postura de seguridad de las cargas de trabajo.

## <a name="secure-score-calculation"></a>Cálculo de la puntuación de seguridad

Security Center imita el trabajo de un analista de seguridad: revisa sus recomendaciones de seguridad y aplica algoritmos avanzados para determinar la importancia de cada recomendación.
Azure Security Center revisa constantemente las recomendaciones activas y calcula la puntuación de seguridad en función de ellas. La puntuación de una recomendación se deriva de su gravedad y de los procedimientos recomendados de seguridad que afecten más a la seguridad de la carga de trabajo.

Security Center también proporciona una **puntuación de seguridad general**. 

La **puntuación de seguridad general** es una acumulación de todas las puntuaciones de recomendación. Puede ver la puntuación de seguridad general de sus suscripciones o grupos de administración, según lo que seleccione. La puntuación variará en función de la suscripción seleccionada y las recomendaciones activas de esas suscripciones.

Para comprobar qué recomendaciones afectan más a la puntuación de seguridad, puede ver las tres recomendaciones con más impacto en el panel de Security Center, o puede ordenar las recomendaciones en la hoja de la lista de recomendaciones mediante la columna **Impacto de la puntuación segura**.

Para consultar su puntuación de seguridad general:

1. En el panel de Azure, haga clic en **Security Center** y, después, en **Puntuación de seguridad**.

2. En la parte superior puede ver los aspectos destacados de la puntuación de seguridad:
   - **Puntuación de seguridad general** representa la puntuación por directivas, por la suscripción seleccionada.
   - **Puntuación de seguridad por categoría** muestra los recursos que necesitan más atención.
   - **Recomendaciones principales por impacto de Puntuación de seguridad** le proporciona una lista de las recomendaciones que mejorarán en mayor medida su puntuación de seguridad si las implementa.
 
   ![Puntuación segura](./media/security-center-secure-score/secure-score-dashboard.png)

3. En la tabla siguiente puede ver cada una de las suscripciones y la puntuación de seguridad general para cada una.

   > [!NOTE]
   > La suma de la puntuación de seguridad de cada suscripción no equivale a la puntuación de seguridad general. La puntuación de seguridad es un cálculo basado en la relación entre los recursos en buen estado y los recursos totales por recomendación, no una suma de las puntuaciones de seguridad entre las suscripciones. 
   >
4. Haga clic en **Ver recomendaciones** para ver las recomendaciones para esa suscripción, que puede corregir para mejorar su puntuación de seguridad.
4. En la lista de recomendaciones, puede ver que para cada recomendación hay una columna que representa el **Impacto de la puntuación segura**. Este número representa cuánto mejorará la puntuación de seguridad general si sigue las recomendaciones. Por ejemplo, en la pantalla siguiente, **Corregir vulnerabilidades en las configuraciones de seguridad de contenedor** aumentará su puntuación de seguridad en 35 puntos.

   ![Puntuación segura](./media/security-center-secure-score/security-center-secure-score1.png)



## <a name="individual-secure-score"></a>Puntuación de seguridad individual

Además, puede consultar las puntuaciones de seguridad individuales en la hoja de recomendaciones individuales.  

**Puntuación de la recomendación** es un cálculo basado en la relación entre los recursos en buen estado y los recursos totales. Si el número de recursos en buen estado es igual al número total de recursos, obtendrá la máxima puntuación de seguridad de la recomendación de 50. Para intentar obtener la puntuación de seguridad más cercana a la máxima puntuación, siga las recomendaciones para corregir los recursos en mal estado.

**Impacto de la recomendación** le permite saber cuánto mejorará su puntuación de seguridad si aplica los pasos de la recomendación. Por ejemplo, si la puntuación de seguridad es de 42 y el **Impacto de la recomendación** es de +3, al realizar los pasos descritos en la recomendación mejorará su puntuación segura hasta 45.

La recomendación muestra las amenazas a las que se expone la carga de trabajo si no se realizan los pasos para la corrección.

![puntuación segura de recomendación individual](./media/security-center-secure-score/indiv-recommendation-secure-score.png)


## <a name="next-steps"></a>Pasos siguientes
En este artículo se muestra cómo mejorar su postura de seguridad mediante **Puntuación de seguridad** de Azure Security Center. Para más información sobre Security Center, consulte:

* [Preguntas más frecuentes sobre Azure Security Center](security-center-faq.md): encuentre las preguntas más frecuentes sobre el uso del servicio.
* [Supervisión del estado de seguridad en Azure Security Center](security-center-monitoring.md): aprenda a supervisar el estado de los recursos de Azure.
