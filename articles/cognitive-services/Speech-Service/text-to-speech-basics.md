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
ms.custom: tracking-python, devx-track-javascript
zone_pivot_groups: programming-languages-set-two-with-js
ms.openlocfilehash: b571c5ee64e12021acf699f979d85d37af7bac7d
ms.sourcegitcommit: 42107c62f721da8550621a4651b3ef6c68704cd3
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/29/2020
ms.locfileid: "87407647"
---
# <a name="learn-the-basics-of-speech-synthesis"></a>Aspectos básicos de la síntesis de voz

En este artículo aprenderá patrones de diseño comunes para realizar la síntesis de texto a voz mediante el SDK de voz. Para empezar, puede realizar una configuración y síntesis básicas y, después, pasar a ejemplos más avanzados para el desarrollo de aplicaciones personalizadas, entre las que se incluyen:

* Obtención de respuestas como flujos de datos en memoria
* Personalización de la frecuencia de muestreo y la velocidad de bits de salida
* Envío de solicitudes de síntesis mediante SSML (lenguaje de marcado de síntesis de voz)
* Uso de voces neuronales

> [!TIP]
> Si no ha tenido la oportunidad de completar ninguno de nuestros inicios rápidos, le animamos a que lo haga ahora y pruebe la conversión de texto a voz.
> * [Síntesis de voz en un altavoz](quickstarts/text-to-speech.md)

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
