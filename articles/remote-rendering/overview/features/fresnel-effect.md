---
title: Efecto Fresnel
description: Página de explicación de la característica del efecto Fresnel en el material
author: jumeder
ms.author: jumeder
ms.date: 11/09/2020
ms.topic: article
ms.custom: devx-track-csharp
ms.openlocfilehash: c8ecd3afc95cf4b8054cd7db3530c15afebea3ce
ms.sourcegitcommit: d60976768dec91724d94430fb6fc9498fdc1db37
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/02/2020
ms.locfileid: "96498241"
---
# <a name="fresnel-effect"></a>Efecto Fresnel

La característica de efecto Fresnel en el material es un efecto ad hoc que no es físicamente correcto. Se basa en la observación física de los objetos que se vuelven más reflectantes en estos ángulos. La propia reflectancia de Fresnel ya está incorporada físicamente en el [modelo de material de PBR](../../overview/features/pbr-materials.md) usado en Azure Remote Rendering. Por el contrario, la característica de efecto Fresnel en el material es simplemente un efecto de color aditivo que no depende de las [luces](../../overview/features/lights.md) ni del [entorno del cielo](../../overview/features/sky.md).

El efecto Fresnel proporciona a los objetos a los que afecta un brillo de color alrededor de los bordes. Puede encontrar información sobre la personalización del efecto y ejemplos de los resultados de la representación en las secciones siguientes.

## <a name="enabling-the-fresnel-effect"></a>Habilitación del efecto Fresnel

Para usar la característica de efecto Fresnel, debe estar habilitada en los materiales en cuestión. Puede habilitarla estableciendo el bit FresnelEffect de [PbrMaterialFeatures](/dotnet/api/microsoft.azure.remoterendering.pbrmaterialfeatures) en el [material de PBR](../../overview/features/pbr-materials.md). El mismo patrón se aplica a [ColorMaterialFeatures](/dotnet/api/microsoft.azure.remoterendering) y el [material de color](../../overview/features/color-materials.md). Consulte en la sección de ejemplos de código una demostración de su uso.

Después de habilitar el efecto Fresnel, será visible inmediatamente. De forma predeterminada, el brillo será blanco (1, 1, 1, 1) y tendrá un exponente de 1. Puede personalizar esta configuración mediante los establecedores de parámetros siguientes.

## <a name="customizing-the-effect-appearance"></a>Personalización de la apariencia del efecto

Actualmente, el efecto Fresnel se puede personalizar por material con las siguientes propiedades:

| Propiedad de material | Tipo | Explicación |
|-------------------|------|-------------|
| FresnelEffectColor | Color4 | El color que se agrega como máximo como brillo de Fresnel. Actualmente se omite el canal alfa. |
| FresnelEffectExponent | FLOAT | La distribución del brillo de Fresnel. Abarca desde 0,01 (se distribuye por todo el objeto) hasta 10 (solo en los ángulos más oblicuos). |

En la práctica, se pueden ver diferentes configuraciones de color y exponente del siguiente modo:

![Ejemplos del efecto Fresnel](./media/fresnel-effect-examples.png)

El exponente del efecto Fresnel se incrementa progresivamente de 1 a 10 en cada fila de color. Al hacerlo, se lleva progresivamente el brillo de Fresnel a los bordes de los objetos vistos. El efecto Fresnel tampoco se ve afectado por la transparencia, como puede ver en el ejemplo siguiente:

![Ejemplos de transparencia en el efecto Fresnel](./media/fresnel-effect-transparent-examples.png)

Como se muestra, los objetos de la diagonal son completamente transparentes, pero el brillo de Fresnel permanece. En este sentido, el efecto imita al efecto Fresnel basado en la representación física, que también se encuentra en estas capturas de pantallas.

## <a name="code-samples"></a>Ejemplos de código

Los siguientes ejemplos de código muestran cómo habilitar y personalizar el efecto Fresnel para un [material de PBR](../../overview/features/pbr-materials.md) y un [material de color](../../overview/features/color-materials.md):

```cs
    void SetFresnelEffect(AzureSession session, Material material)
    {
        if (material.MaterialSubType == MaterialType.Pbr)
        {
            var pbrMaterial = material as PbrMaterial;
            pbrMaterial.PbrFlags |= PbrMaterialFeatures.FresnelEffect;
            pbrMaterial.FresnelEffectColor = new Color4(1.0f, 0.5f, 0.1f, 1.0f);
            pbrMaterial.FresnelEffectExponent = 3.141592f;
        }
        else if (material.MaterialSubType == MaterialType.Color)
        {
            var colorMaterial = material as ColorMaterial;
            colorMaterial.ColorFlags |= ColorMaterialFeatures.FresnelEffect;
            colorMaterial.FresnelEffectColor = new Color4(0.25f, 1.0f, 0.25f, 1.0f);
            colorMaterial.FresnelEffectExponent = 7.654321f;
        }
    }
```

```cpp
void SetFresnelEffect(ApiHandle<AzureSession> session, ApiHandle<Material> material)
{
    if (material->GetMaterialSubType() == MaterialType::Pbr)
    {
        auto pbrMaterial = material.as<PbrMaterial>();
        auto featureFlags = PbrMaterialFeatures((int32_t)pbrMaterial->GetPbrFlags() | (int32_t)PbrMaterialFeatures::FresnelEffect);
        pbrMaterial->SetPbrFlags(featureFlags);
        pbrMaterial->SetFresnelEffectColor(Color4{ 1.f, 0.5f, 0.1f, 1.f });
        pbrMaterial->SetFresnelEffectExponent(3.141592f);

    }
    else if (material->GetMaterialSubType() == MaterialType::Color)
    {
        auto colorMaterial = material.as<ColorMaterial>();
        auto featureFlags = ColorMaterialFeatures((int32_t)colorMaterial->GetColorFlags() | (int32_t)ColorMaterialFeatures::FresnelEffect);
        colorMaterial->SetColorFlags(featureFlags);
        colorMaterial->SetFresnelEffectColor(Color4{ 0.25f, 1.f, 0.25f, 1.f });
        colorMaterial->SetFresnelEffectExponent(7.654321f);
    }
}
```

## <a name="api-documentation"></a>Documentación de la API

* [PbrMaterialFeatures para C#](/dotnet/api/microsoft.azure.remoterendering.pbrmaterialfeatures)
* [PbrMaterialFeatures para C++](/cpp/api/remote-rendering/pbrmaterialfeatures)
* [ColorMaterialFeatures para C#](/dotnet/api/microsoft.azure.remoterendering.colormaterialfeatures)
* [ColorMaterialFeatures para C++](/cpp/api/remote-rendering/colormaterialfeatures)

## <a name="next-steps"></a>Pasos siguientes

* [Materiales](../../concepts/materials.md)
* [Materiales de PBR](../../overview/features/pbr-materials.md)
* [Materiales de color](../../overview/features/color-materials.md)