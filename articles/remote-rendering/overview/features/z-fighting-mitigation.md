---
title: Mitigación de Z-fighting
description: Describe técnicas para mitigar los artefactos de Z-fighting
author: florianborn71
ms.author: flborn
ms.date: 02/06/2020
ms.topic: article
ms.custom: devx-track-csharp
ms.openlocfilehash: 0e217676f5e1f4dcba24917cb140d9d4d8fcc422
ms.sourcegitcommit: 70ee014d1706e903b7d1e346ba866f5e08b22761
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/11/2020
ms.locfileid: "90024373"
---
# <a name="z-fighting-mitigation"></a>Mitigación de Z-fighting

Cuando se superponen dos superficies, no queda claro cuál se debe representar encima de la otra. El resultado varía incluso en cada píxel, lo que resulta en artefactos dependientes de la vista de cámara. Por lo tanto, cuando la cámara o la malla se mueven, estos patrones parpadean bastante. Este artefacto se denomina *Z-fighting*. En el caso de las aplicaciones de AR y VR, el problema se intensifica porque los cascos de realidad virtual siempre se mueven de forma natural. Para evitar la incomodidad del visor, Azure Remote Rendering dispone de una funcionalidad de mitigación de Z-fighting.

## <a name="z-fighting-mitigation-modes"></a>Modos de mitigación de Z-fighting

|Situación                        | Resultado                               |
|---------------------------------|:-------------------------------------|
|Z-fighting normal               |![No hay ninguna prioridad determinista entre los cuádruplos rojo y verde](./media/zfighting-0.png)|
|Mitigación de Z-fighting habilitada    |![El cuádruplo rojo tiene prioridad](./media/zfighting-1.png)|
|Resaltado de tablero de damas habilitado|![Prioridad de alternancia entre cuádruplos rojos y verdes en un patrón de tablero de ajedrez](./media/zfighting-2.png)|

El código siguiente habilita la mitigación de Z-fighting:

```cs
void EnableZFightingMitigation(AzureSession session, bool highlight)
{
    ZFightingMitigationSettings settings = session.Actions.ZFightingMitigationSettings;

    // enabling z-fighting mitigation
    settings.Enabled = true;

    // enabling checkerboard highlighting of z-fighting potential
    settings.Highlighting = highlight;
}
```

```cpp
void EnableZFightingMitigation(ApiHandle<AzureSession> session, bool highlight)
{
    ApiHandle<ZFightingMitigationSettings> settings = session->Actions()->GetZFightingMitigationSettings();

    // enabling z-fighting mitigation
    settings->SetEnabled(true);

    // enabling checkerboard highlighting of z-fighting potential
    settings->SetHighlighting(highlight);
}
```

> [!NOTE]
> la mitigación de Z-fighting es una configuración global que afecta a todas las mallas representadas.

## <a name="reasons-for-z-fighting"></a>Motivos para Z-fighting

Z-fighting se produce principalmente por dos motivos:

1. Cuando las superficies están lejos de la cámara, la precisión de sus valores de profundidad se degrada y los valores se vuelven indistinguibles.
1. Cuando las superficies de una malla se superponen físicamente.

El primer problema siempre puede producirse y es difícil de eliminar. Si esto ocurre en la aplicación, asegúrese de que la relación entre la distancia del *plano cercano* y la del *plano lejano* es la menor posible. Por ejemplo, un plano cercano a una distancia de 0,01 y un plano lejano a una distancia de 1000 creará este problema mucho antes que si el plano cercano está a 0,1 y el plano lejano a una distancia de 20.

El segundo problema es un indicador de que el contenido ha sido mal creado. En el mundo real, dos objetos no pueden estar en el mismo lugar al mismo tiempo. Según la aplicación, es posible que los usuarios deseen saber si existen superficies superpuestas y dónde se encuentran. Por ejemplo, una escena de CAD de un edificio que es la base para una construcción del mundo real no debe contener intersecciones de superficie físicamente imposibles. Para permitir la inspección visual, el modo de resaltado está disponible, que muestra un posible efecto de Z-fighting como un patrón de tablero de damas animado.

## <a name="limitations"></a>Limitaciones

La mitigación de Z-fighting proporcionada es un intento de mejora. No hay ninguna garantía de que quite todo el Z-fighting. También preferirá automáticamente una superficie sobre otra. Por lo tanto, si tiene superficies que están demasiado cerca entre sí, puede suceder que la superficie "equivocada" termine en la parte superior. Un problema común se produce cuando el texto y otras marcas se aplican a una superficie. Si se habilita la mitigación de Z-fighting, estos detalles podrían desaparecer.

## <a name="performance-considerations"></a>Consideraciones de rendimiento

* La habilitación de la mitigación de Z-fighting no ocasiona apenas sobrecarga del rendimiento.
* Además, la habilitación de la superposición de Z-fighting conlleva una sobrecarga de rendimiento considerable, aunque puede variar en función de la escena.

## <a name="api-documentation"></a>Documentación de la API

* [Propiedad RemoteManager.ZFightingMitigationSettings de C#](https://docs.microsoft.com/dotnet/api/microsoft.azure.remoterendering.remotemanager.zfightingmitigationsettings)
* [RemoteManager::ZFightingMitigationSettings() de C++](https://docs.microsoft.com/cpp/api/remote-rendering/remotemanager#zfightingmitigationsettings)

## <a name="next-steps"></a>Pasos siguientes

* [Modos de representación](../../concepts/rendering-modes.md)
* [Reproyección de fase tardía](late-stage-reprojection.md)
