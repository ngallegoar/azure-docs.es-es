---
title: Materiales
description: Representación de las propiedades de material y la descripción de este
author: jakrams
ms.author: jakras
ms.date: 02/11/2020
ms.topic: conceptual
ms.openlocfilehash: 8551e17ddd71e76aca0c85b9768f564ae0e5f049
ms.sourcegitcommit: 642a297b1c279454df792ca21fdaa9513b5c2f8b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/06/2020
ms.locfileid: "80679402"
---
# <a name="materials"></a>Materiales

Los materiales son [recursos](../concepts/lifetime.md) que definen cómo se representan las [mallas](meshes.md). Los materiales se usan para especificar qué [texturas](textures.md) se van a aplicar, si se va a hacer que los objetos sean transparentes y cómo se calculará la iluminación.

Los materiales se crean automáticamente durante la [conversión de modelos](../how-tos/conversion/model-conversion.md) y son accesibles en tiempo de ejecución. También puede crear materiales personalizados a partir de código y reemplazar los existentes. Este escenario tiene sentido especialmente si desea compartir el mismo material en muchas mallas. Dado que las modificaciones de un material están visibles en cada malla que hace referencia al mismo, este método se puede usar para aplicar fácilmente los cambios.

> [!NOTE]
> Algunos casos de uso, como el resaltado de un objeto seleccionado, se pueden realizar mediante la modificación de materiales, pero se logran mucho más fácilmente a través del [HierarchicalStateOverrideComponent](../overview/features/override-hierarchical-state.md).

## <a name="material-types"></a>Tipos de material

Azure Remote Rendering tiene dos tipos de material distintos:

* Los [materiales de PBR](../overview/features/pbr-materials.md) se usan para las superficies que deben representarse tan físicamente correctas como sea posible. La iluminación realista se calcula para estos materiales mediante la *representación basada en la física* (PBR). Para sacar el máximo partido a este tipo de material, es importante proporcionar datos de entrada de alta calidad, como mapas normales y de rugosidad.

* Los [materiales de color](../overview/features/color-materials.md) se usan para los casos en los que no se desea iluminación adicional. Estos materiales siempre son totalmente brillantes y más fáciles de configurar. Los materiales de color se usan para los datos que no deben tener iluminación o que ya incorporan iluminación estática, como los modelos obtenidos a través de [fotogrametría](https://en.wikipedia.org/wiki/Photogrammetry).

## <a name="mesh-vs-meshcomponent-material-assignment"></a>Asignación de materiales a usar en la malla frente a MeshComponent

Las [mallas](meshes.md) tienen una o varias submallas. Cada submalla hace referencia a un material. Puede cambiar el material para usarlo directamente en la malla, o bien invalidar el material que se va a usar para una submalla en un [MeshComponent](meshes.md#meshcomponent).

Cuando se modifica un material directamente en el recurso de malla, este cambio afecta a todas las instancias de esa malla. Sin embargo, cambiarlo en el MeshComponent solo afecta a esa instancia de malla. El método más adecuado depende del comportamiento deseado, pero la modificación de un MeshComponent es el enfoque más común.

## <a name="material-classes"></a>Clases Material

Todos los materiales proporcionados por la API se derivan de la clase base `Material`. Su tipo se puede consultar a través de `Material.MaterialSubType` o mediante su conversión directa:

``` cs
void SetMaterialColorToGreen(Material material)
{
    if (material.MaterialSubType == MaterialType.Color)
    {
        ColorMaterial colorMaterial = material as ColorMaterial;
        colorMaterial.AlbedoColor = new Color4(0, 1, 0, 1);
        return;
    }

    PbrMaterial pbrMat = material as PbrMaterial;
    if( pbrMat!= null )
    {
        PbrMaterial pbrMaterial = material.PbrMaterial.Value;
        pbrMaterial.AlbedoColor = new Color4(0, 1, 0, 1);
        return;
    }
}
```

## <a name="next-steps"></a>Pasos siguientes

* [Materiales de PBR](../overview/features/pbr-materials.md)
* [Materiales de color](../overview/features/color-materials.md)
