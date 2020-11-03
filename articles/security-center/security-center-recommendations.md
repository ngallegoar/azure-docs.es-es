---
title: Recomendaciones de seguridad en el Centro de seguridad de Azure
description: Este documento explica cómo las recomendaciones del Centro de seguridad de Azure ayudan a proteger los recursos de Azure y a cumplir con las directivas de seguridad.
services: security-center
documentationcenter: na
author: memildin
manager: rkarlin
ms.assetid: 86c50c9f-eb6b-4d97-acb3-6d599c06133e
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/22/2020
ms.author: memildin
ms.openlocfilehash: c0952e0f267f9c2511a2e80fa7106958d8dd0ee5
ms.sourcegitcommit: 400f473e8aa6301539179d4b320ffbe7dfae42fe
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/28/2020
ms.locfileid: "92791892"
---
# <a name="security-recommendations-in-azure-security-center"></a>Recomendaciones de seguridad en el Centro de seguridad de Azure 
En este tema se explica cómo ver y entender las recomendaciones de Azure Security Center para ayudar a proteger los recursos de Azure.


## <a name="what-are-security-recommendations"></a>¿Cuáles son las recomendaciones de seguridad?

Las recomendaciones son acciones que se deben llevar a cabo para proteger los recursos.

Security Center analiza periódicamente el estado de seguridad de los recursos de Azure para identificar posibles puntos vulnerables de la seguridad. A continuación, se proporcionan recomendaciones sobre cómo corregir dichos puntos vulnerables.

Cada recomendación le proporciona:

- Descripción breve del problema
- Pasos de corrección que se deben llevar a cabo para implementar la recomendación.
- Recursos afectados.

## <a name="monitor-recommendations"></a>Supervisión de recomendaciones<a name="monitor-recommendations"></a>

Security Center analiza el estado de seguridad de los recursos para identificar posibles vulnerabilidades. 

1. En el menú de Security Center, abra la página **Recomendaciones** para ver las recomendaciones aplicables a su entorno. Las recomendaciones se agrupan en controles de seguridad.

      ![Recomendaciones agrupadas por control de seguridad](./media/security-center-recommendations/view-recommendations.png)

1. Expanda un control y seleccione una recomendación concreta para ver la página de detalles de las recomendaciones.

    :::image type="content" source="./media/security-center-recommendations/recommendation-details-page.png" alt-text="Página de detalles de recomendación" lightbox="./media/security-center-recommendations/recommendation-details-page.png":::

    La página incluye:

    1. Los botones **Aplicar** y **Denegar** en recomendaciones admitidas (vea [Evitar configuraciones inactivas con las recomendaciones Aplicar o Denegar](prevent-misconfigurations.md))
    1. **Indicador de gravedad**
    1. **Intervalo de actualización** (si procede) 
    1. **Descripción** : una breve descripción del problema
    1. **Pasos de corrección** : una descripción de los pasos manuales necesarios para corregir el problema de seguridad en los recursos afectados Para obtener recomendaciones con "corrección rápida", puede seleccionar **Ver lógica de corrección** antes de aplicar la corrección sugerida a los recursos. 
    1. **Recursos afectados** : los recursos se agrupan en pestañas:
        - **Recursos con estado correcto** : recursos relevantes que no se ven afectados o en los que ya se ha corregido el problema.
        - **Recursos con estado incorrecto** : recursos que todavía se ven afectados por el problema identificado.
        - **Recursos no aplicables** : recursos para los que la recomendación no puede dar una respuesta definitiva. La pestaña no aplicable también incluye las razones de cada recurso. 

            :::image type="content" source="./media/security-center-recommendations/recommendations-not-applicable-reasons.png" alt-text="Los recursos no aplicables con motivos":::.
    1. Botones de acción para corregir la recomendación o desencadenar una aplicación lógica.

## <a name="preview-recommendations"></a>Recomendaciones de la versión preliminar

Las recomendaciones marcadas como **Versión preliminar** no se incluyen en los cálculos de la puntuación de seguridad.

Aun así, deben corregirse siempre que sea posible, de tal forma que, cuando finalice el período de versión preliminar, contribuyan a la puntuación.

Un ejemplo de una recomendación en versión preliminar:

:::image type="content" source="./media/secure-score-security-controls/example-of-preview-recommendation.png" alt-text="Recomendación con la marca de versión preliminar":::
 
## <a name="next-steps"></a>Pasos siguientes

En este documento, se han presentando las recomendaciones de seguridad del Centro de seguridad. Para obtener información relacionada, visite:

- [Recomendaciones de corrección ](security-center-remediate-recommendations.md): aprenda a configurar directivas de seguridad para las suscripciones y los grupos de recursos de Azure.
- [Evitar errores de configuración con las recomendaciones Aplicar o Denegar](prevent-misconfigurations.md).
- [Creación de respuestas automáticas a alertas y recomendaciones con automatización del flujo de trabajo](workflow-automation.md): automatice las respuestas a las recomendaciones
- [Exclusión de un recurso de una recomendación](exempt-resource.md)
- [Guía de referencia sobre las recomendaciones de seguridad](recommendations-reference.md)