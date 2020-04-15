---
title: Límites de objetos
description: Se explica cómo se pueden consultar los límites de objetos espaciales.
author: florianborn71
ms.author: flborn
ms.date: 02/03/2020
ms.topic: conceptual
ms.openlocfilehash: 1d2dfdb203b05f2f6b7de740718d7407bd88066c
ms.sourcegitcommit: 642a297b1c279454df792ca21fdaa9513b5c2f8b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/06/2020
ms.locfileid: "80679362"
---
# <a name="object-bounds"></a>Límites de objetos

Los límites de objetos representan el volumen que ocupa una [entidad](entities.md) y sus elementos secundarios. En Azure Remote Rendering, los límites de objetos siempre se dan como *cuadros de límite alineados por el eje* (AABB). Los objetos enlazados pueden estar en el *espacio local* o en el *espacio universal*. En cualquier caso, siempre están alineados por el eje, lo que significa que las extensiones y el volumen pueden diferir entre la representación del espacio local y el espacio universal.

## <a name="querying-object-bounds"></a>Consulta de los límites de objetos

El AABB local de una [malla](meshes.md) se puede consultar directamente desde el recurso de malla. Estos límites se pueden transformar en el espacio local o en el espacio universal de una entidad mediante la transformación de la entidad.

Es posible calcular los límites de una jerarquía de objetos completa de esta manera, pero para ello es necesario recorrer la jerarquía, consultar los límites de cada malla y combinarlos manualmente. Esta operación es tanto tediosa como ineficaz.

Una manera mejor es llamar a `QueryLocalBoundsAsync` o `QueryWorldBoundsAsync` en una entidad. El cálculo se descarga luego en el servidor y se devuelve con un retardo mínimo.

``` cs
private BoundsQueryAsync _boundsQuery = null;

public void GetBounds(Entity entity)
{
    _boundsQuery = entity.QueryWorldBoundsAsync();
    _boundsQuery.Completed += (BoundsQueryAsync bounds) =>
    {
        if (bounds.IsRanToCompletion)
        {
            Double3 aabbMin = bounds.Result.min;
            Double3 aabbMax = bounds.Result.max;
            // ...
        }
    };
}
```

## <a name="next-steps"></a>Pasos siguientes

* [Consultas espaciales](../overview/features/spatial-queries.md)
