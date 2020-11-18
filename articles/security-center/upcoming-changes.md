---
title: Próximos cambios importantes en Azure Security Center
description: Próximos cambios en Azure Security Center que debe tener en cuenta y para los que puede que necesite un plan
services: security-center
documentationcenter: na
author: memildin
manager: rkarlin
ms.service: security-center
ms.devlang: na
ms.topic: overview
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/09/2020
ms.author: memildin
ms.openlocfilehash: df863372cbf7abfb6fee145b7d13bb00d8deb074
ms.sourcegitcommit: 8a1ba1ebc76635b643b6634cc64e137f74a1e4da
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/09/2020
ms.locfileid: "94380172"
---
# <a name="important-upcoming-changes-to-azure-security-center"></a>Próximos cambios importantes en Azure Security Center

> [!IMPORTANT]
> La información de esta página está relacionada con productos o características en versión preliminar que pueden modificarse sustancialmente antes de lanzarse comercialmente, si es que lo hacen finalmente. Microsoft no ofrece ningún compromiso o garantía, ni expresa ni implícita, con respecto a la información que se proporciona aquí.

En esta página, obtendrá información sobre los cambios planeados para Security Center. Describe las modificaciones planeadas en el producto que pueden afectar a aspectos como los flujos de trabajo o la puntuación segura.

Si busca las notas de la versión más recientes, puede encontrarlas en [Novedades de Azure Security Center](release-notes.md).


## <a name="planned-changes"></a>Cambios planeados

### <a name="system-updates-should-be-installed-on-your-machines-recommendation-getting-sub-recommendations"></a>La recomendación "Las actualizaciones del sistema deben estar instaladas en las máquinas" incluirá recomendaciones secundarias

#### <a name="summary"></a>Resumen

| Aspecto | Detalles |
|---------|---------|
|Fecha del anuncio | 9 de noviembre de 2020  |
|Fecha de este cambio  |  Entre mediados y finales de noviembre de 2020 |
|Impacto     | Durante la transición de la versión actual de esta recomendación a la versión de sustitución, la puntuación de seguridad podría cambiar. |
|  |  |

Vamos a publicar una versión mejorada de la recomendación **Las actualizaciones del sistema deben estar instaladas en las máquinas**. La nueva versión *reemplaza* la versión actual en el control de seguridad de aplicación de actualizaciones del sistema y ofrece las siguientes mejoras:

- Recomendaciones secundarias para las actualizaciones que faltan
- Una nueva experiencia en las páginas Azure Security Center de Azure Portal
- Datos enriquecidos para la recomendación de Azure Resource Graph

#### <a name="transition-period"></a>Período de transición

Habrá un período de transición de 36 horas (aproximadamente). Para minimizar cualquier posible interrupción, hemos programado la actualización para que tenga lugar durante un fin de semana. Durante la transición, las puntuaciones de seguridad podrían verse afectadas.

#### <a name="redesigned-portal-experience"></a>Nueva experiencia del portal

La página de detalles de la recomendación **Las actualizaciones del sistema deben estar instaladas en las máquinas** incluye la lista de conclusiones que se muestran a continuación. Al seleccionar una única búsqueda, se abre el panel de detalles con un vínculo a la información de corrección y una lista de los recursos afectados.

:::image type="content" source="./media/upcoming-changes/system-updates-should-be-installed-subassessment.png" alt-text="Apertura de una de las recomendaciones secundarias en la experiencia del portal para la recomendación actualizada":::


#### <a name="richer-data-from-azure-resource-graph"></a>Datos enriquecidos de Azure Resource Graph

Azure Resource Graph (ARG) es un servicio de Azure diseñado para facilitar la exploración eficaz de los recursos. Puede usar ARG para realizar consultas a escala para un conjunto determinado de suscripciones a fin de controlar eficazmente su entorno. 

Para Azure Security Center, puede usar ARG y el [lenguaje de consulta Kusto (KQL)](https://docs.microsoft.com/azure/data-explorer/kusto/query/) para consultar una amplia variedad de datos de posición de seguridad.

Si consulta la versión actual de **Las actualizaciones del sistema deben estar instaladas en las máquinas**, la única información disponible en ARG es que la recomendación necesita corregirse en una máquina. Cuando se publique la versión actualizada, la siguiente consulta devolverá las actualizaciones del sistema que falten agrupadas en función de la máquina.

```kusto
securityresources
| where type =~ "microsoft.security/assessments/subassessments"
| where extract(@"(?i)providers/Microsoft.Security/assessments/([^/]*)", 1, id) == "4ab6e3c5-74dd-8b35-9ab9-f61b30875b27"
| where properties.status.code == "Unhealthy"
```

## <a name="next-steps"></a>Pasos siguientes

Para ver todos los cambios recientes realizados en el producto, consulte [Novedades de Azure Security Center](release-notes.md).