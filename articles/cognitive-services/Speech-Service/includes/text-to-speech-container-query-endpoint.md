---
title: Consulta del punto de conexión del contenedor Conversión de texto a voz
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: include
ms.date: 04/01/2020
ms.author: aahi
ms.openlocfilehash: b2a621a23a81e4fb4f47e7c99d780211973e30a0
ms.sourcegitcommit: 8dc84e8b04390f39a3c11e9b0eaf3264861fcafc
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/13/2020
ms.locfileid: "81275084"
---
El contenedor proporciona [API de punto de conexión basadas en REST](../rest-text-to-speech.md). Hay muchos [proyectos de código fuente de ejemplo](https://github.com/Azure-Samples/Cognitive-Speech-TTS) disponibles para las variaciones de lenguaje, marco y plataforma.

Con el contenedor *Conversión de texto a voz estándar*, debe basarse en la configuración regional y en la voz de la etiqueta de imagen que descargó. Por ejemplo, si descargó la etiqueta `latest`, la configuración regional predeterminada es `en-US` y la voz `JessaRUS`. El argumento `{VOICE_NAME}` sería [`en-US-JessaRUS`](../language-support.md#standard-voices). Vea el SSML de ejemplo siguiente:

```xml
<speak version="1.0" xml:lang="en-US">
    <voice name="en-US-JessaRUS">
        This text will get converted into synthesized speech.
    </voice>
</speak>
```

Sin embargo, para *Conversión de texto a voz personalizada*, tendrá que obtener el valor de **Voice / model** desde el [portal de Voz personalizada](https://aka.ms/custom-voice-portal). El nombre del modelo personalizado es sinónimo del nombre de la voz. Vaya a la página de **entrenamiento** y copie el valor de **Voice / model** que se va a usar como el argumento `{VOICE_NAME}`.
<br><br>
:::image type="content" source="../media/custom-voice/custom-voice-model-voice-name.png" alt-text="Modelo de Voz personalizada: nombre de voz":::

Vea el SSML de ejemplo siguiente:

```xml
<speak version="1.0" xml:lang="en-US">
    <voice name="custom-voice-model">
        This text will get converted into synthesized speech.
    </voice>
</speak>
```

Vamos a crear una solicitud HTTP POST, proporcionando algunos encabezados y una carga de datos. Reemplace el marcador de posición `{VOICE_NAME}` por un valor propio.

```curl
curl -s -v -X POST http://localhost:5000/speech/synthesize/cognitiveservices/v1 \
 -H 'Accept: audio/*' \
 -H 'Content-Type: application/ssml+xml' \
 -H 'X-Microsoft-OutputFormat: riff-16khz-16bit-mono-pcm' \
 -d '<speak version="1.0" xml:lang="en-US"><voice name="{VOICE_NAME}">This is a test, only a test.</voice></speak>'
```

Este comando:

* Construye una solicitud HTTP POST para el punto de conexión `speech/synthesize/cognitiveservices/v1`.
* Especifica un encabezado `Accept` de `audio/*`
* Especifica un encabezado `Content-Type` de `application/ssml+xml`. Para más información, consulte el [cuerpo de la solicitud](../rest-text-to-speech.md#request-body).
* Especifica un encabezado `X-Microsoft-OutputFormat` de `riff-16khz-16bit-mono-pcm`. Para más opciones, consulte la [salida de audio](../rest-text-to-speech.md#audio-outputs).
* Envía la solicitud [Lenguaje de marcado de síntesis de voz (SSML)](../speech-synthesis-markup.md) dado el `{VOICE_NAME}` al punto de conexión.