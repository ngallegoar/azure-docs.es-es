---
title: 'Aspectos básicos de la síntesis de voz: Servicio de voz'
titleSuffix: Azure Cognitive Services
description: Aprenda a usar el SDK de voz para convertir texto a voz. En este artículo, obtendrá información sobre la construcción de objetos, los formatos de salida de audio admitidos y las opciones de configuración personalizadas de la síntesis de voz.
services: cognitive-services
author: trevorbye
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: quickstart
ms.date: 04/14/2020
ms.author: trbye
zone_pivot_groups: programming-languages-set-sixteen
ms.openlocfilehash: 74f762dd0004bf72b703d0f611a16762b1ff028d
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/16/2020
ms.locfileid: "81399543"
---
# <a name="learn-the-basics-of-speech-synthesis"></a>Aspectos básicos de la síntesis de voz

En este artículo aprenderá patrones de diseño comunes para realizar la síntesis de texto a voz mediante el SDK de voz. Para empezar, puede realizar una configuración y síntesis básicas y, después, pasar a ejemplos más avanzados para el desarrollo de aplicaciones personalizadas, entre las que se incluyen:

* Obtención de respuestas como flujos de datos en memoria
* Personalización de la frecuencia de muestreo y la velocidad de bits de salida
* Envío de solicitudes de síntesis mediante SSML (lenguaje de marcado de síntesis de voz)
* Uso de voces neuronales

> [!TIP]
> Si no ha tenido la oportunidad de completar alguno de nuestros inicios rápidos, le animamos a que lo haga ahora y pruebe el reconocimiento de voz.
> * [Reconocimiento de voz a través de un micrófono](quickstarts/text-to-speech.md)

::: zone pivot="programming-language-csharp"
[!INCLUDE [C# Basics include](includes/how-to/text-to-speech-basics/text-to-speech-basics-csharp.md)]
::: zone-end

::: zone pivot="programming-language-cpp"
[!INCLUDE [C++ Basics include](includes/how-to/text-to-speech-basics/text-to-speech-basics-cpp.md)]
::: zone-end

::: zone pivot="programming-language-java"
[!INCLUDE [Java Basics include](includes/how-to/text-to-speech-basics/text-to-speech-basics-java.md)]
::: zone-end

::: zone pivot="programming-language-javascript"
[!INCLUDE [JavaScript Basics include](includes/how-to/text-to-speech-basics/text-to-speech-basics-javascript.md)]
::: zone-end

::: zone pivot="programming-language-python"
[!INCLUDE [Python Basics include](includes/how-to/text-to-speech-basics/text-to-speech-basics-python.md)]
::: zone-end

::: zone pivot="programming-language-more"
[!INCLUDE [More languages include](./includes/how-to/speech-to-text-basics/more.md)]
::: zone-end

## <a name="next-steps"></a>Pasos siguientes

* [Introducción a Voz personalizada](how-to-custom-voice.md)
* [Mejora de síntesis con SSML](speech-synthesis-markup.md)