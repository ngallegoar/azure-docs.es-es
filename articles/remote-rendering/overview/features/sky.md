---
title: Reflejos del cielo
description: Describe cómo configurar mapas de entorno para reflejos del cielo
author: florianborn71
ms.author: flborn
ms.date: 02/07/2020
ms.topic: article
ms.custom: devx-track-csharp
ms.openlocfilehash: a193fdf82ed5e9a785c6c01db4931fb05e9aaac7
ms.sourcegitcommit: 957c916118f87ea3d67a60e1d72a30f48bad0db6
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/19/2020
ms.locfileid: "92205751"
---
# <a name="sky-reflections"></a>Reflejos del cielo

En Azure Remote Rendering, se usa una textura de cielo para iluminar los objetos de forma realista. En el caso de las aplicaciones de realidad aumentada, esta textura debe ser similar a su entorno real para que los objetos parezcan convincentes. En este artículo se describe cómo cambiar la textura del cielo.

> [!NOTE]
> La textura del cielo también se conoce como *mapa de entorno*. Estos términos se usan indistintamente.

## <a name="object-lighting"></a>Iluminación de objetos

Azure Remote Rendering emplea la *representación basada en la física* (PBR) para los cálculos de iluminación realistas. Aunque puede agregar [fuentes de luz](lights.md) a la escena, el uso de una buena textura de cielo tiene el mayor efecto.

En las imágenes siguientes se muestran los resultados de las distintas superficies de iluminación solo con una textura de cielo:

| Rugosidad  | 0                                        | 0,25                                          | 0.5                                          | 0,75                                          | 1                                          |
|:----------:|:----------------------------------------:|:---------------------------------------------:|:--------------------------------------------:|:---------------------------------------------:|:------------------------------------------:|
| No metal  | ![Dielectric, rugosidad=0](media/dielectric-0.png)   | ![Dielectric, rugosidad=0,25](media/dielectric-0.25.png)  | ![Dielectric, rugosidad=0,5](media/dielectric-0.5.png)  | ![Dielectric, rugosidad=0,75](media/dielectric-0.75.png)  | ![Dielectric, rugosidad=1](media/dielectric-1.png)  |
| Metal      | ![Metal, rugosidad=0](media/metallic-0.png)  | ![Metal, rugosidad=0,25](media/metallic-0.25.png)    | ![Metal, rugosidad=0,5](media/metallic-0.5.png)    | ![Metal, rugosidad=0,75](media/metallic-0.75.png)    | ![Metal, rugosidad=1](media/metallic-1.png)    |

Para más información sobre el modelo de iluminación, vea el capítulo de [materiales](../../concepts/materials.md).

> [!IMPORTANT]
> Azure Remote Rendering usa la textura de cielo solo para los modelos de iluminación. No representa el cielo como fondo, ya que las aplicaciones de realidad aumentada ya tienen un fondo adecuado: el mundo real.

## <a name="changing-the-sky-texture"></a>Cambio de la textura del cielo

Para cambiar el mapa de entorno, lo único que tiene que hacer es [cargar una textura](../../concepts/textures.md) y cambiar el valor de `SkyReflectionSettings` de la sesión:

```cs
LoadTextureAsync _skyTextureLoad = null;
void ChangeEnvironmentMap(AzureSession session)
{
    _skyTextureLoad = session.Actions.LoadTextureFromSASAsync(new LoadTextureFromSASParams("builtin://VeniceSunset", TextureType.CubeMap));

    _skyTextureLoad.Completed += (LoadTextureAsync res) =>
        {
            if (res.IsRanToCompletion)
            {
                try
                {
                    session.Actions.SkyReflectionSettings.SkyReflectionTexture = res.Result;
                }
                catch (RRException exception)
                {
                    System.Console.WriteLine($"Setting sky reflection failed: {exception.Message}");
                }
            }
            else
            {
                System.Console.WriteLine("Texture loading failed!");
            }
        };
}
```

```cpp
void ChangeEnvironmentMap(ApiHandle<AzureSession> session)
{
    LoadTextureFromSASParams params;
    params.TextureType = TextureType::CubeMap;
    params.TextureUrl = "builtin://VeniceSunset";
    ApiHandle<LoadTextureAsync> skyTextureLoad = *session->Actions()->LoadTextureFromSASAsync(params);

    skyTextureLoad->Completed([&](ApiHandle<LoadTextureAsync> res)
        {
            if (res->GetIsRanToCompletion())
            {
                ApiHandle<SkyReflectionSettings> settings = session->Actions()->GetSkyReflectionSettings();
                settings->SetSkyReflectionTexture(res->GetResult());
            }
            else
            {
                printf("Texture loading failed!\n");
            }
        });
}

```

Tenga en cuenta que la variante `LoadTextureFromSASAsync` se usa porque se carga una textura integrada. Si la carga se realiza desde [almacenamientos de blobs vinculados](../../how-tos/create-an-account.md#link-storage-accounts), utilice la variante `LoadTextureAsync`.

## <a name="sky-texture-types"></a>Tipos de textura de cielo

Puede usar *[mapas de cubo](https://en.wikipedia.org/wiki/Cube_mapping)* y *texturas 2D* como mapas de entorno.

Todas las texturas tienen que estar en un [formato de textura compatible](../../concepts/textures.md#supported-texture-formats). No es necesario proporcionar mapas MIP para las texturas de cielo.

### <a name="cube-environment-maps"></a>Mapas de entorno de cubo

Como referencia, este es un mapa de cubo desempaquetado:

![Mapa de cubo desempaquetado](media/Cubemap-example.png)

Use `AzureSession.Actions.LoadTextureAsync`/ `LoadTextureFromSASAsync` con `TextureType.CubeMap` para cargar texturas de mapa de cubo.

### <a name="sphere-environment-maps"></a>Mapas de entorno de esfera

Cuando se usa una textura 2D como mapa de entorno, la imagen tiene que estar en un [espacio de coordenadas esférico](https://en.wikipedia.org/wiki/Spherical_coordinate_system).

![Una imagen de cielo en coordenadas esféricas](media/spheremap-example.png)

Use `AzureSession.Actions.LoadTextureAsync` con `TextureType.Texture2D` para cargar mapas de entorno esféricos.

## <a name="built-in-environment-maps"></a>Mapas de entorno integrados

Azure Remote Rendering proporciona una serie de mapas de entorno integrados que siempre están disponibles. Todas las asignaciones de entorno integradas son mapas de cubo.

|Identificador                         | Descripción                                              | Ilustración                                                      |
|-----------------------------------|:---------------------------------------------------------|:-----------------------------------------------------------------:|
|builtin://Autoshop                 | Diversas iluminación base de interior brillante y bandas de luz    | ![Skybox Autoshop usado para iluminar un objeto](media/autoshop.png)
|builtin://BoilerRoom               | Configuración de luz interior brillante, varias luces de ventana      | ![Skybox BoilerRoom usado para iluminar un objeto](media/boiler-room.png)
|builtin://ColorfulStudio           | Iluminación de variaciones de color en la opción de iluminación media de interior  | ![Skybox ColorfulStudio usado para iluminar un objeto](media/colorful-studio.png)
|builtin://Hangar                   | Iluminación ambiental de sala moderadamente brillante                     | ![Skybox SmallHangar usado para iluminar un objeto](media/hangar.png)
|builtin://IndustrialPipeAndValve   | Configuración interior de DIM con contraste claro-oscuro              | ![Skybox IndustrialPipeAndValve usado para iluminar un objeto](media/industrial-pipe-and-valve.png)
|builtin://Lebombo                  | Iluminación de sala ambiental diurna, iluminación de área de ventana brillante     | ![Skybox Lebombo usado para iluminar un objeto](media/lebombo.png)
|builtin://SataraNight              | Cielo nocturno oscuro y suelo con muchas luces circundantes   | ![Skybox SataraNight usado para iluminar un objeto](media/satara-night.png)
|builtin://SunnyVondelpark          | Luz solar brillante y contraste de sombra                      | ![Skybox SunnyVondelpark usado para iluminar un objeto](media/sunny-vondelpark.png)
|builtin://Syferfontein             | Iluminación de cielo clara con iluminación de suelo moderada            | ![Skybox Syferfontein usado para iluminar un objeto](media/syferfontein.png)
|builtin://TearsOfSteelBridge       | Tono y sombreado de variación moderada                         | ![Skybox TearsOfSteelBridge usado para iluminar un objeto](media/tears-of-steel-bridge.png)
|builtin://VeniceSunset             | Luz de puesta de sol que se aproxima a la del anochecer                    | ![Skybox VeniceSunset usado para iluminar un objeto](media/venice-sunset.png)
|builtin://WhippleCreekRegionalPark | Suelo atenuado con tonos de luz verde, blanco y brillante | ![Skybox WhippleCreekRegionalPark usado para iluminar un objeto](media/whipple-creek-regional-park.png)
|builtin://WinterRiver              | Día con luz de tierra en ambiente brillante                 | ![Skybox WinterRiver usado para iluminar un objeto](media/winter-river.png)
|builtin://DefaultSky               | Igual que TearsOfSteelBridge                               | ![Skybox DefaultSky usado para iluminar un objeto](media/tears-of-steel-bridge.png)

## <a name="api-documentation"></a>Documentación de la API

* [Propiedad RemoteManager.SkyReflectionSettings de C#](/dotnet/api/microsoft.azure.remoterendering.remotemanager.skyreflectionsettings)
* [RemoteManager::SkyReflectionSettings() de C++](/cpp/api/remote-rendering/remotemanager#skyreflectionsettings)

## <a name="next-steps"></a>Pasos siguientes

* [Luces](../../overview/features/lights.md)
* [Materiales](../../concepts/materials.md)
* [Texturas](../../concepts/textures.md)
* [La herramienta de línea de comandos TexConv](../../resources/tools/tex-conv.md)