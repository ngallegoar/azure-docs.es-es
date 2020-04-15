---
title: Representación en una sola cara
description: En este artículo se describen los casos de uso y la configuración de representación en una sola cara
author: florianborn71
ms.author: flborn
ms.date: 02/06/2020
ms.topic: article
ms.openlocfilehash: 34ee5d4978c6476da407cde33598a5713177078e
ms.sourcegitcommit: 642a297b1c279454df792ca21fdaa9513b5c2f8b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/06/2020
ms.locfileid: "80679454"
---
# <a name="single-sided-rendering"></a>Representación en una sola cara

La mayoría de los representadores usan la [selección de la cara posterior](https://en.wikipedia.org/wiki/Back-face_culling) para mejorar el rendimiento. Sin embargo, cuando se cortan las mallas abiertas con [planos de corte](cut-planes.md), los usuarios suelen ver la cara posterior de los triángulos. Si esos triángulos se seleccionan, el resultado no parecerá convincente.

La manera de evitar este problema de forma confiable es representar triángulos de *dos caras*. Como no usar la selección de la cara posterior tiene implicaciones en el rendimiento, de forma predeterminada, Azure Remote Rendering solo cambia a la renderización de dos caras para las mallas que forman intersección con un plano de corte.

El valor de *representación en una sola cara* permite personalizar este comportamiento.

> [!CAUTION]
> La configuración de representación en una sola cara es una característica experimental. Puede volver a quitarse en el futuro. No cambie la configuración predeterminada, a menos que resuelva realmente un problema crítico en la aplicación.

## <a name="prerequisites"></a>Prerrequisitos

La configuración de representación en una sola cara solo tiene efecto para las mallas que se han [convertido](../../how-tos/conversion/configure-model-conversion.md) con la opción `opaqueMaterialDefaultSidedness` establecida en `SingleSided`. De forma predeterminada, el valor de esta opción es `DoubleSided`.

## <a name="single-sided-rendering-setting"></a>Configuración de representación en una sola cara

Hay tres modos diferentes:

**Normal:** en este modo, las mallas siempre se representan a medida que se convierten. Esto significa que las mallas convertidas con el valor `opaqueMaterialDefaultSidedness` establecido en `SingleSided` siempre se representarán con la selección de la cara posterior habilitada, incluso cuando forman intersección con un plano de corte.

**DynamicDoubleSiding:** en este modo, cuando un plano de corte forma una intersección con una malla, se cambia automáticamente a la representación en dos caras. Este modo es el predeterminado.

**AlwaysDoubleSided:** obliga a que todas las geometrías de una sola cara se representen siempre. Este modo se expone principalmente para que pueda comparar fácilmente el impacto en el rendimiento entre la representación en una sola cara y en dos.

Para cambiar la configuración de representación en una sola cara, siga estos pasos:

``` cs
void ChangeSingleSidedRendering(AzureSession session)
{
    SingleSidedSettings settings = session.Actions.SingleSidedSettings;

    // Single-sided geometry is rendered as is
    settings.Mode = SingleSidedMode.Normal;

    // Single-sided geometry is always rendered double-sided
    settings.Mode = SingleSidedMode.AlwaysDoubleSided;
}
```

## <a name="next-steps"></a>Pasos siguientes

* [Planos de corte](cut-planes.md)
* [Configuración de la conversión de modelos](../../how-tos/conversion/configure-model-conversion.md)
