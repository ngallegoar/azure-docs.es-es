---
title: Iluminación de escenas
description: Descripción y propiedades de la fuente de luz
author: florianborn71
ms.author: flborn
ms.date: 02/10/2020
ms.topic: article
ms.openlocfilehash: 49027899d66a2192cc311fb4dba66e441155b527
ms.sourcegitcommit: 957c916118f87ea3d67a60e1d72a30f48bad0db6
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/19/2020
ms.locfileid: "92206856"
---
# <a name="scene-lighting"></a>Iluminación de escenas

De forma predeterminada, los objetos representados de forma remota se iluminan con un color [celeste claro](sky.md). Para la mayoría de las aplicaciones, esto ya es suficiente, pero puede agregar más fuentes de luz a la escena.

> [!IMPORTANT]
> Solo los [materiales de PBR](pbr-materials.md) afectan a las fuentes de luz. Los [materiales de color](color-materials.md) siempre aparecen totalmente brillantes.

> [!NOTE]
> Actualmente no se admite la proyección de sombras. Azure Remote Rendering está optimizado para representar enormes cantidades de geometría, mediante el uso de varias GPU si es necesario. Los enfoques tradicionales para la proyección de sombras no funcionan bien en estos escenarios.

## <a name="common-light-component-properties"></a>Propiedades comunes de componentes de luz

Todos los tipos de luz derivan de la clase base abstracta `LightComponent` y comparten estas propiedades:

* **Color:** color de la luz en el [espacio Gamma](https://en.wikipedia.org/wiki/SRGB). Se omite Alpha.

* **Intensity:** brillo de la luz. En el caso de las luces focales y puntuales, la intensidad también define la distancia a la que la luz llega.

## <a name="point-light"></a>Luz puntual

En Azure Remote Rendering, el `PointLightComponent` no solo puede emitir luz desde un único punto, sino también desde una esfera pequeña o un tubo pequeño, para simular fuentes de luz más suaves.

### <a name="pointlightcomponent-properties"></a>Propiedades de PointLightComponent

* **Radius:** el radio predeterminado es cero, en cuyo caso la luz actúa como una luz puntual. Si el radio es mayor que cero, actúa como una fuente de luz esférica, que cambia la apariencia de los reflejos especulares.

* **Length:** si tanto `Length` como `Radius` son distintos de cero, la luz actúa como una luz de tubo. Se puede usar para simular tubos de neón.

* **AttenuationCutoff:** si se deja en (0,0), la atenuación de la luz solo depende de su `Intensity` (intensidad). Sin embargo, puede proporcionar distancias mínimas y máximas personalizadas sobre las que la intensidad de la luz se escale linealmente a 0. Esta característica se puede usar para aplicar un intervalo menor de influencia de una luz concreta.

* **ProjectedCubemap:** si se establece en un [mapa de cubo](../../concepts/textures.md) válido, la textura se proyecta en la geometría circundante de la luz. El color del mapa de cubo se modula con el color de la luz.

## <a name="spot-light"></a>Luz focal

El valor de `SpotLightComponent` es similar al de `PointLightComponent`, pero la luz está restringida a la forma de un cono. La orientación del cono se define mediante el *eje z negativo de la entidad propietaria*.

### <a name="spotlightcomponent-properties"></a>Propiedades de SpotLightComponent

* **Radius:** igual que para `PointLightComponent`.

* **SpotAngleDeg:** este intervalo define el ángulo interior y exterior del cono, medido en grados. Todo el contenido dentro del ángulo interior se ilumina con un brillo completo. Se aplica un descenso hacia el ángulo exterior que genera un efecto similar a la penumbra.

* **FalloffExponent:** define la nitidez de las transiciones de descenso entre el ángulo de cono interior y el exterior. Un valor mayor produce una transición más nítida. El valor predeterminado de 1,0 da como resultado una transición lineal.

* **AttenuationCutoff:** igual que para `PointLightComponent`.

* **Projected2dTexture:** si se establece en una [textura 2D](../../concepts/textures.md) válida, la imagen se proyecta en la geometría en la que destaca la luz. El color de la textura se modula con el color de la luz.

## <a name="directional-light"></a>Luz direccional

El valor de `DirectionalLightComponent` simula una fuente de luz que está infinitamente lejos. La luz destaca en la dirección del *eje Z negativo de la entidad propietaria*. Se omite la posición de la entidad.

No hay propiedades adicionales.

## <a name="performance-considerations"></a>Consideraciones de rendimiento

Las fuentes de luz tienen un efecto significativo en el rendimiento de la representación. Úselas con cuidado y solo si la aplicación lo requiere. Cualquier condición de iluminación global estática, incluido un componente direccional estático, se puede lograr con una [textura de cielo personalizada](sky.md), sin ningún costo de representación adicional.

## <a name="api-documentation"></a>Documentación de la API

* [Clase LightComponentBase de C#](/dotnet/api/microsoft.azure.remoterendering.lightcomponentbase)
* [Clase PointLightComponent de C#](/dotnet/api/microsoft.azure.remoterendering.pointlightcomponent)
* [Clase SpotLightComponent de C#](/dotnet/api/microsoft.azure.remoterendering.spotlightcomponent)
* [Clase DirectionalLightComponent de C#](/dotnet/api/microsoft.azure.remoterendering.directionallightcomponent)
* [Clase LightComponentBase de C++](/cpp/api/remote-rendering/lightcomponentbase)
* [Clase PointLightComponent de C++](/cpp/api/remote-rendering/pointlightcomponent)
* [Clase SpotLightComponent de C++](/cpp/api/remote-rendering/spotlightcomponent)
* [Clase DirectionalLightComponent de C++](/cpp/api/remote-rendering/directionallightcomponent)

## <a name="next-steps"></a>Pasos siguientes

* [Materiales](../../concepts/materials.md)
* [Cielo](sky.md)