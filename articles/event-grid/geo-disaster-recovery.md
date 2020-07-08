---
title: Recuperación ante desastres geográfica en Azure Event Grid | Microsoft Docs
description: Describe cómo Azure Event Grid admite la recuperación ante desastres geográfica (GeoDR) automáticamente.
ms.topic: conceptual
ms.date: 07/07/2020
ms.openlocfilehash: ccb16971020a65932daa8f9adf4b7cd9008a9253
ms.sourcegitcommit: d7008edadc9993df960817ad4c5521efa69ffa9f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/08/2020
ms.locfileid: "86105853"
---
# <a name="server-side-geo-disaster-recovery-in-azure-event-grid"></a>Recuperación ante desastres geográfica del servidor en Azure Event Grid
Ahora, Event Grid incluye una recuperación ante desastres geográfica (GeoDR) automática de metadatos, en que no solo se recuperan las suscripciones a eventos, los temas y los dominios nuevos, sino también todos los existentes. Si deja de funcionar toda una región de Azure, Event Grid ya tendrá todos los metadatos de infraestructura relacionados con el evento sincronizados en una región emparejada. Los nuevos eventos empezarán a fluir de nuevo sin intervención del usuario. 

La recuperación ante desastres se mide con dos métricas:

- [Objetivo de punto de recuperación (RPO)](https://en.wikipedia.org/wiki/Disaster_recovery#Recovery_Point_Objective): minutos u horas de datos que se pueden perder.
- [Objetivo de tiempo de recuperación (RTO)](https://en.wikipedia.org/wiki/Disaster_recovery#Recovery_time_objective): minutos u horas que el servicio puede estar inactivo.

La conmutación automática por error de Event Grid tiene diferentes RPO y RTO para los metadatos (suscripciones a eventos, etc.) y los datos (eventos). Si necesita una especificación distinta a las siguientes, siempre puede implementar su propia [conmutación por error del lado cliente con las API de estado de temas](custom-disaster-recovery.md).

## <a name="recovery-point-objective-rpo"></a>Objetivo de punto de recuperación (RPO)
- **RPO de metadatos**: cero minutos. Cada vez que se crea un recurso en Event Grid, se replica al instante en otras regiones. Cuando se produce una conmutación por error, no se pierde ningún metadato.
- **RPO de datos**: si el sistema funciona correctamente y tenía controlado el tráfico en el momento de la conmutación por error regional, el RPO para eventos es de unos 5 minutos.

## <a name="recovery-time-objective-rto"></a>Objetivo de tiempo de recuperación (RTO)
- **RTO de metadatos**: aunque esto suele ocurrir con mucha más rapidez, en 60 minutos, Event Grid empezará a aceptar llamadas de creación, actualización y eliminación en los temas y las suscripciones.
- **RTO de datos**: al igual que con los metadatos, esto suele ocurrir mucho más rápido, pero, en un plazo de 60 minutos, Event Grid comenzará a aceptar el tráfico nuevo después de una conmutación por error regional.

> [!NOTE]
> El costo de GeoDR de metadatos en Event Grid es de 0 USD.


## <a name="next-steps"></a>Pasos siguientes
Si quiere implementar la lógica de conmutación por error del lado cliente, consulte [# Creación de una recuperación ante desastres propia para temas personalizados en Event Grid](custom-disaster-recovery.md).
