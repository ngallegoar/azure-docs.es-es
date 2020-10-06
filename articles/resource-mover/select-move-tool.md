---
title: Elección de una herramienta para el traslado de recursos de Azure entre regiones
description: Revisión de las opciones y herramientas para el traslado de recursos de Azure entre regiones
author: rayne-wiselman
manager: evansma
ms.service: resource-move
ms.topic: overview
ms.date: 09/09/2020
ms.author: raynew
ms.custom: mvc
ms.openlocfilehash: 1b233028b52175842c73660ff116ac592d8296e0
ms.sourcegitcommit: 80b9c8ef63cc75b226db5513ad81368b8ab28a28
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/16/2020
ms.locfileid: "90603363"
---
# <a name="choose-a-tool-for-moving-azure-resources"></a>Elección de una herramienta para el traslado de recursos de Azure

Puede trasladar recursos dentro de Azure de la siguiente manera:

- **Traslado de recursos entre regiones**: traslade el recurso desde el centro de Resource Mover o un grupo de recursos. 
- **Traslado de recursos entre grupos de recursos o suscripciones**: traslade el recurso desde un grupo de recursos. 
- **Traslado de recursos entre nubes de Azure**: use el servicio Azure Site Recovery para trasladar recursos entre nubes públicas y gubernamentales.
- **Traslado de recursos entre zonas de disponibilidad de misma región**: use el servicio Azure Site Recovery para trasladar recursos entre zonas de disponibilidad de la misma región de Azure.


## <a name="compare-move-tools"></a>Comparación de las herramientas de traslado

**Herramienta** | **Cuándo se deben usar** | **Más información**
--- | --- | ---
**Traslado dentro del grupo de recursos** | Traslade recursos a otro grupo de recursos o suscripción, o entre regiones.<br/><br/> Si va a realizar un traslado entre regiones, seleccione los recursos que desea trasladar en el grupo de recursos y, a continuación, vaya al centro de Resource Mover para comprobar las dependencias y mover los recursos a la región de destino. | [Traslado de los recursos a un nuevo grupo de recursos o a una nueva suscripción](../azure-resource-manager/management/move-resource-group-and-subscription.md).<br/><br/> [Traslado de recursos entre regiones (desde un grupo de recursos)](move-region-within-resource-group.md).
**Traslado desde el centro de Resource Mover** | Traslade recursos entre regiones. <br/><br/> Puede realizar el traslado a una región de destino, o a una zona de disponibilidad específica, o a un conjunto de disponibilidad, dentro de la región de destino. | [Traslado de recursos entre regiones en el centro de Resource Mover]().
**Traslado de máquinas virtuales con Site Recovery** | Se usa para trasladar máquinas virtuales de Azure entre las nubes gubernamentales y públicas.<br/><br/> Utilícelo si desea trasladar máquinas virtuales entre zonas de disponibilidad de la misma región. |[Mover las VM de Azure entre Azure Government y las regiones públicas](../site-recovery/region-move-cross-geos.md), [Habilitación de la recuperación ante desastres de máquinas virtuales de Azure](../site-recovery/azure-to-azure-how-to-enable-zone-to-zone-disaster-recovery.md).

## <a name="next-steps"></a>Pasos siguientes

[Más información](about-move-process.md) sobre los componentes de Resource Mover y el proceso de traslado.
