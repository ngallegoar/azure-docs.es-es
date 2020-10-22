---
title: Límites de objetos
description: Se explica cómo se pueden consultar los límites de objetos espaciales.
author: florianborn71
ms.author: flborn
ms.date: 02/03/2020
ms.topic: conceptual
ms.custom: devx-track-csharp
ms.openlocfilehash: 421265bf1ee488c8e7d0c41e3ec9a250392d6f3d
ms.sourcegitcommit: 957c916118f87ea3d67a60e1d72a30f48bad0db6
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/19/2020
ms.locfileid: "92202793"
---
# <a name="object-bounds"></a>Límites de objetos

Los límites de objetos representan el volumen que ocupa una [entidad](entities.md) y sus elementos secundarios. En Azure Remote Rendering, los límites de objetos siempre se dan como *cuadros de límite alineados por el eje* (AABB). Los objetos enlazados pueden estar en el *espacio local* o en el *espacio universal*. En cualquier caso, siempre están alineados por el eje, lo que significa que las extensiones y el volumen pueden diferir entre la representación del espacio local y el espacio universal.

## <a name="querying-object-bounds"></a>Consulta de los límites de objetos

El AABB local de una [malla](meshes.md) se puede consultar directamente desde el recurso de malla. Estos límites se pueden transformar en el espacio local o en el espacio universal de una entidad mediante la transformación de la entidad.

Es posible calcular los límites de una jerarquía de objetos completa de esta manera, pero para ello es necesario recorrer la jerarquía, consultar los límites de cada malla y combinarlos manualmente. Esta operación es tanto tediosa como ineficaz.

Una manera mejor es llamar a `QueryLocalBoundsAsync` o `QueryWorldBoundsAsync` en una entidad. El cálculo se descarga luego en el servidor y se devuelve con un retardo mínimo.

```cs
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

```cpp
void GetBounds(ApiHandle<Entity> entity)
{
    ApiHandle<BoundsQueryAsync> boundsQuery = *entity->QueryWorldBoundsAsync();
    boundsQuery->Completed([](ApiHandle<BoundsQueryAsync> bounds)
    {
        if (bounds->GetIsRanToCompletion())
        {
            Double3 aabbMin = bounds->GetResult().min;
            Double3 aabbMax = bounds->GetResult().max;
            // ...
        }
    });
}
```

## <a name="api-documentation"></a>Documentación de la API

* [C# Entity.QueryLocalBoundsAsync](/dotnet/api/microsoft.azure.remoterendering.entity.querylocalboundsasync)
* [C++ Entity::QueryLocalBoundsAsync](/cpp/api/remote-rendering/entity#querylocalboundsasync)

## <a name="next-steps"></a>Pasos siguientes

* [Consultas espaciales](../overview/features/spatial-queries.md)