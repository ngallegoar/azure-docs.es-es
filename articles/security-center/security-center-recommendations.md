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
ms.date: 07/29/2019
ms.author: memildin
ms.openlocfilehash: 4d65b43dad80cb130d582132d21e2d10bd8051dc
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/02/2020
ms.locfileid: "84791391"
---
# <a name="security-recommendations-in-azure-security-center"></a>Recomendaciones de seguridad en el Centro de seguridad de Azure 
En este tema se explica cómo ver y entender las recomendaciones de Azure Security Center para ayudar a proteger los recursos de Azure.

> [!NOTE]
> En este documento se presenta el servicio mediante una implementación de ejemplo.  Este documento no es una guía paso a paso.
>

## <a name="what-are-security-recommendations"></a>¿Cuáles son las recomendaciones de seguridad?

Las recomendaciones son acciones que se deben llevar a cabo para proteger los recursos.

Security Center analiza periódicamente el estado de seguridad de los recursos de Azure para identificar posibles vulnerabilidades de seguridad. A continuación, proporciona recomendaciones sobre cómo quitarlas.

Cada recomendación le proporciona:

- Una breve descripción de lo que se recomienda.
- Los pasos de corrección que se deben llevar a cabo para implementar la recomendación. <!-- In some cases, Quick Fix remediation is available. -->
- Los recursos necesarios para realizar la acción recomendada.
- El **impacto de la puntuación segura**, que es la cantidad que aumentará la puntuación de seguridad si se implementa esta recomendación.

## <a name="monitor-recommendations"></a>Supervisión de recomendaciones<a name="monitor-recommendations"></a>

Security Center analiza el estado de seguridad de los recursos para identificar posibles vulnerabilidades. El icono **Recomendaciones** de **Información general** permite conocer el número total de recomendaciones que identifica Security Center.

![Introducción a Security Center](./media/security-center-recommendations/asc-overview.png)

1. Seleccione el **icono de Recomendaciones** en **Información general**. Se abre la lista **Recomendaciones**.

      ![Ver recomendaciones](./media/security-center-recommendations/view-recommendations.png)

    Puede filtrar las recomendaciones. Para ello, seleccione **Filtro** en la hoja **Recomendaciones**. Se abrirá la hoja **Filtro** , donde podrá seleccionar los valores de gravedad y de estado que quiera ver.

   * **RECOMENDACIONES**: la recomendación.
   * **IMPACTO DE LA PUNTUACIÓN SEGURA**: puntuación que genera Security Center mediante sus recomendaciones de seguridad y la aplicación de algoritmos avanzados para determinar la importancia de cada recomendación. Para obtener más información, consulte [Cálculo de Puntuación segura](secure-score-security-controls.md#how-your-secure-score-is-calculated).
   * **RECURSO**: enumera los recursos a los que se aplica la recomendación.
   * **BARRAS DE ESTADO**:  describe la gravedad de una recomendación concreta:
       * **Alta (rojo)** : existe una vulnerabilidad en un recurso importante (como una aplicación, una máquina virtual o un grupo de seguridad de red) y requiere atención.
       * **Media (naranja)** : existe una vulnerabilidad y se requieren pasos adicionales o no críticos para eliminarla o completar un proceso.
       * **Baja (azul)** : existe una vulnerabilidad que se debe abordar, pero no requiere una atención inmediata. (De manera predeterminada no se muestran las recomendaciones bajas, pero si desea verlas, puede filtrar por ellas). 
       * **Correcto (verde)** :
       * **No disponible (gris)** :

1. Para ver los detalles de cada recomendación, haga clic en la recomendación.

    ![Detalles de recomendación](./media/security-center-recommendations/recommendation-details.png)

>[!NOTE] 
> Consulte los [modelos de implementación clásico y de Resource Manager](../azure-classic-rm.md) de los recursos de Azure.
 
## <a name="next-steps"></a>Pasos siguientes

En este documento, se han presentando las recomendaciones de seguridad del Centro de seguridad. Para obtener información sobre cómo corregir las recomendaciones:

* [Recomendaciones de corrección ](security-center-remediate-recommendations.md): aprenda a configurar directivas de seguridad para las suscripciones y los grupos de recursos de Azure.
