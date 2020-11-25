---
title: 'Streaming de audio comprimido con códec mediante el SDK de voz: servicio de voz'
titleSuffix: Azure Cognitive Services
description: Obtenga información sobre cómo hacer secuencias de audio comprimido al servicio de voz con el SDK de voz. Disponible para C++, C#y Java para Linux, Java en Android y Objective-C en iOS.
services: cognitive-services
author: amitkumarshukla
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 03/30/2020
ms.author: amishu
ms.custom: devx-track-csharp
zone_pivot_groups: programming-languages-set-twenty-two
ms.openlocfilehash: ace584b51f0aa457e30d8e48d97834aa735a1a57
ms.sourcegitcommit: 10d00006fec1f4b69289ce18fdd0452c3458eca5
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/21/2020
ms.locfileid: "95026580"
---
# <a name="use-codec-compressed-audio-input-with-the-speech-sdk"></a>Uso de entradas de audio comprimido con códec con el SDK de voz

La API **Compressed Audio Input Stream** del SDK del servicio de voz ofrece una forma de transmitir en secuencias audio comprimido al servicio de voz mediante `PullStream` o `PushStream`.

Plataforma | Idiomas | Versión de GStreamer compatible
| :--- | ---: | :---:
Windows (excepto UWP)  | C++, C#, Java, Python | [1.15.1](https://gstreamer.freedesktop.org/data/pkg/windows/1.15.1/)
Linux  | C++, C#, Java, Python | [Distribuciones y arquitecturas de destino de Linux admitidas](~/articles/cognitive-services/speech-service/speech-sdk.md)
Android  | Java | [1.14.4](https://gstreamer.freedesktop.org/data/pkg/android/1.14.4/)

## <a name="speech-sdk-version-required-for-compressed-audio-input"></a>Versión del SDK de voz necesaria para la entrada de audio comprimido
* Se requiere el SDK de Voz versión 1.10.0 o posterior para RHEL 8 y CentOS 8.
* Se requiere el SDK de Voz versión 1.11.0 o posterior para Windows.

[!INCLUDE [supported-audio-formats](includes/supported-audio-formats.md)]

## <a name="prerequisites"></a>Prerrequisitos

::: zone pivot="programming-language-csharp"
[!INCLUDE [prerequisites](includes/how-to/compressed-audio-input/csharp/prerequisites.md)]
::: zone-end

::: zone pivot="programming-language-cpp"
[!INCLUDE [prerequisites](includes/how-to/compressed-audio-input/cpp/prerequisites.md)]
::: zone-end

::: zone pivot="programming-language-java"
[!INCLUDE [prerequisites](includes/how-to/compressed-audio-input/java/prerequisites.md)]
::: zone-end

::: zone pivot="programming-language-python"
[!INCLUDE [prerequisites](includes/how-to/compressed-audio-input/python/prerequisites.md)]
::: zone-end

## <a name="example-code-using-codec-compressed-audio-input"></a>Ejemplo de código que usa una entrada de audio comprimido con códec

::: zone pivot="programming-language-csharp"
[!INCLUDE [prerequisites](includes/how-to/compressed-audio-input/csharp/examples.md)]
::: zone-end

::: zone pivot="programming-language-cpp"
[!INCLUDE [prerequisites](includes/how-to/compressed-audio-input/cpp/examples.md)]
::: zone-end

::: zone pivot="programming-language-java"
[!INCLUDE [prerequisites](includes/how-to/compressed-audio-input/java/examples.md)]
::: zone-end

::: zone pivot="programming-language-python"
[!INCLUDE [prerequisites](includes/how-to/compressed-audio-input/python/examples.md)]
::: zone-end

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Aprenda a reconocer voz](./get-started-speech-to-text.md)