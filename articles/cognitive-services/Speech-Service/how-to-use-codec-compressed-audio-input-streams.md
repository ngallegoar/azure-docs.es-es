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
zone_pivot_groups: programming-languages-set-twelve
ms.openlocfilehash: bd6b500a823bde654da4442704b75451806d6a46
ms.sourcegitcommit: 0450ed87a7e01bbe38b3a3aea2a21881f34f34dd
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/03/2020
ms.locfileid: "80637299"
---
# <a name="use-codec-compressed-audio-input-with-the-speech-sdk"></a>Uso de entradas de audio comprimido con códec con el SDK de voz

La API **Compressed Audio Input Stream** del SDK del servicio de voz ofrece una forma de transmitir en secuencias audio comprimido al servicio de voz mediante `PullStream` o `PushStream`.

El streaming de audio de entrada comprimido actualmente se admite para C#, C++ y Java en Windows (las aplicaciones para UWP no se admiten) Linux (Ubuntu 16.04, Ubuntu 18.04, Debian 9, RHEL 8 y CentOS 8). También se admite para Java en Android y Objective-C en la plataforma iOS.
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

::: zone pivot="programming-language-objectivec"
[!INCLUDE [prerequisites](includes/how-to/compressed-audio-input/objectivec/prerequisites.md)]
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

::: zone pivot="programming-language-objectivec"
[!INCLUDE [prerequisites](includes/how-to/compressed-audio-input/objectivec/examples.md)]
::: zone-end

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Aprenda a reconocer voz](quickstarts/speech-to-text-from-microphone.md)
