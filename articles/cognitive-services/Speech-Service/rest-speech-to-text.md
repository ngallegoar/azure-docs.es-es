---
title: 'Referencia de Speech to Text API (REST): servicio de voz'
titleSuffix: Azure Cognitive Services
description: Aprenda a usar speech-to-text API REST. En este artículo, obtendrá más información sobre las opciones de autorización y de consulta y sobre cómo estructurar una solicitud y recibir una respuesta.
services: cognitive-services
author: IEvangelist
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 03/16/2020
ms.author: dapine
ms.openlocfilehash: 759ea697e4093da5bfc1c082c886c6dfda636f42
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "79474805"
---
# <a name="speech-to-text-rest-api"></a>Speech-to-text REST API

Como alternativa al [SDK de Voz](speech-sdk.md), el servicio de voz le permite convertir voz a texto mediante una API REST. Cada punto de conexión accesible se asocia con una región. La aplicación requiere una clave de suscripción para el punto de conexión que se va a usar. La API REST es muy limitada y solo se debe usar en aquellos casos en que no pueda utilizarse [Speech SDK](speech-sdk.md).

Antes de usar speech-to-text API REST debe comprender que:

* Las solicitudes que usan la API REST y transmiten audio directamente solo pueden contener hasta 60 segundos de audio.
* La API REST de voz a texto solo devuelve resultados finales. No se proporcionan resultados parciales.

Si el envío de un audio más grande es necesario para la aplicación, considere la posibilidad de usar el [SDK de Voz](speech-sdk.md) o una API REST basada en archivos, como la [transcripción por lotes](batch-transcription.md).

[!INCLUDE [](../../../includes/cognitive-services-speech-service-rest-auth.md)]

## <a name="regions-and-endpoints"></a>Regiones y puntos de conexión

El punto de conexión de la API REST tiene este formato:

```
https://<REGION_IDENTIFIER>.stt.speech.microsoft.com/speech/recognition/conversation/cognitiveservices/v1
```

Reemplace `<REGION_IDENTIFIER>` por el identificador que coincida con la región de la suscripción en la siguiente tabla:

[!INCLUDE [](../../../includes/cognitive-services-speech-service-region-identifier.md)]

> [!NOTE]
> El parámetro de idioma debe anexarse a la dirección URL para evitar la recepción de errores HTTP 4xx. Por ejemplo, el idioma definido a inglés de Estados Unidos con el punto de conexión del Oeste de EE. UU. es: `https://westus.stt.speech.microsoft.com/speech/recognition/conversation/cognitiveservices/v1?language=en-US`.

## <a name="query-parameters"></a>Parámetros de consulta

Estos parámetros podrían incluirse en la cadena de consulta de la solicitud de REST.

| Parámetro | Descripción | Obligatorio u opcional |
|-----------|-------------|---------------------|
| `language` | Identifica el idioma hablado que se está reconociendo. Vea [Idiomas admitidos](language-support.md#speech-to-text). | Obligatorio |
| `format` | Especifica el formato del resultado. Los valores aceptados son: `simple` y `detailed`. Los resultados simples incluyen `RecognitionStatus`, `DisplayText`, `Offset` y `Duration`. Las respuestas detalladas incluyen varios resultados con valores de confianza y cuatro representaciones diferentes. El valor predeterminado es `simple`. | Opcional |
| `profanity` | Especifica cómo controlar las palabras soeces en los resultados del reconocimiento. Los valores aceptados son `masked`, que reemplaza las palabras soeces con asteriscos, `removed`, que quita todas las palabras soeces del resultado o `raw` que incluye la palabra soez en el resultado. El valor predeterminado es `masked`. | Opcional |
| `cid` | Si se usa el [portal del Custom Speech](how-to-custom-speech.md) para crear modelos personalizados, puede usar modelos personalizados a través de su **identificador de punto de conexión**, que se encuentra en la página **Implementación**. Use el **identificador del punto de conexión** como argumento del parámetro de la cadena de consulta `cid`. | Opcional |

## <a name="request-headers"></a>Encabezados de solicitud

Esta tabla enumera los encabezados obligatorios y opcionales para las solicitudes de voz a texto.

|Encabezado| Descripción | Obligatorio u opcional |
|------|-------------|---------------------|
| `Ocp-Apim-Subscription-Key` | Clave de suscripción del servicio de voz. | Se necesita este encabezado, o bien `Authorization`. |
| `Authorization` | Un token de autorización precedido por la palabra `Bearer`. Para más información, consulte [Autenticación](#authentication). | Se necesita este encabezado, o bien `Ocp-Apim-Subscription-Key`. |
| `Content-type` | Describe el formato y el códec de los datos de audio proporcionados. Los valores aceptados son: `audio/wav; codecs=audio/pcm; samplerate=16000` y `audio/ogg; codecs=opus`. | Obligatorio |
| `Transfer-Encoding` | Especifica que se están enviando datos de audio fragmentados en lugar de un único archivo. Use este encabezado solo si hay fragmentación de los datos de audio. | Opcional |
| `Expect` | Si usa la transferencia fragmentada, envíe `Expect: 100-continue`. El servicio de voz confirma la solicitud inicial y espera datos adicionales.| Obligatorio si se envían datos de audio fragmentados. |
| `Accept` | Si se proporciona, debe ser `application/json`. El servicio de voz proporciona resultados en JSON. Algunos marcos de solicitud proporcionan un valor predeterminado no compatible. Se recomienda incluir siempre `Accept`. | Opcional pero recomendable. |

## <a name="audio-formats"></a>Formatos de audio

El audio se envía en el cuerpo de la solicitud HTTP `POST`. Debe estar en uno de los formatos de esta tabla:

| Formato | Códec | Bitrate | Velocidad de muestreo  |
|--------|-------|---------|--------------|
| WAV    | PCM   | 16 bits  | 16 kHz, mono |
| OGG    | OPUS  | 16 bits  | 16 kHz, mono |

>[!NOTE]
>Se admiten los formatos anteriores a través de la API REST y WebSocket en el servicio de voz. Actualmente, el [SDK de Voz](speech-sdk.md) admite el formato WAV con el códec PCM así como [otros formatos](how-to-use-codec-compressed-audio-input-streams.md).

## <a name="sample-request"></a>Solicitud de ejemplo

El ejemplo siguiente incluye el nombre de host y los encabezados necesarios. Es importante tener en cuenta que el servicio también espera datos de audio, que no están incluidos en este ejemplo. Como se ha mencionado anteriormente, la fragmentación es recomendable pero no obligatoria.

```HTTP
POST speech/recognition/conversation/cognitiveservices/v1?language=en-US&format=detailed HTTP/1.1
Accept: application/json;text/xml
Content-Type: audio/wav; codecs=audio/pcm; samplerate=16000
Ocp-Apim-Subscription-Key: YOUR_SUBSCRIPTION_KEY
Host: westus.stt.speech.microsoft.com
Transfer-Encoding: chunked
Expect: 100-continue
```

## <a name="http-status-codes"></a>Códigos de estado HTTP

El estado HTTP de cada respuesta indica estados de corrección o error comunes.

| Código de estado HTTP | Descripción | Posible motivo |
|------------------|-------------|-----------------|
| `100` | Continuar | Se ha aceptado la solicitud inicial. Continúe con el envío del resto de los datos. (Se usa con la transferencia fragmentada) |
| `200` | Aceptar | La solicitud es correcta; el cuerpo de la respuesta es un objeto JSON. |
| `400` | Solicitud incorrecta | Código de idioma no proporcionado, idioma no compatible; archivo de audio no válido, etc. |
| `401` | No autorizado | Clave de suscripción o token de autorización no válido en la región especificada, o punto de conexión no válido. |
| `403` | Prohibido | Falta la clave de suscripción o el token de autorización. |

## <a name="chunked-transfer"></a>Transferencia fragmentada

La transferencia fragmentada (`Transfer-Encoding: chunked`) puede ayudar a reducir la latencia de reconocimiento. Permite al servicio de voz empezar a procesar el archivo de audio mientras se transmite. La API de REST no proporciona resultados parciales ni provisionales.

Este ejemplo de código muestra cómo enviar audio en fragmentos. Solo el primer fragmento debe contener el encabezado del archivo de audio. `request` es un objeto `HttpWebRequest` conectado al punto de conexión de REST adecuado. `audioFile` es la ruta de acceso a un archivo de audio en disco.

```csharp
var request = (HttpWebRequest)HttpWebRequest.Create(requestUri);
request.SendChunked = true;
request.Accept = @"application/json;text/xml";
request.Method = "POST";
request.ProtocolVersion = HttpVersion.Version11;
request.Host = host;
request.ContentType = @"audio/wav; codecs=audio/pcm; samplerate=16000";
request.Headers["Ocp-Apim-Subscription-Key"] = "YOUR_SUBSCRIPTION_KEY";
request.AllowWriteStreamBuffering = false;

using (var fs = new FileStream(audioFile, FileMode.Open, FileAccess.Read))
{
    // Open a request stream and write 1024 byte chunks in the stream one at a time.
    byte[] buffer = null;
    int bytesRead = 0;
    using (var requestStream = request.GetRequestStream())
    {
        // Read 1024 raw bytes from the input audio file.
        buffer = new Byte[checked((uint)Math.Min(1024, (int)fs.Length))];
        while ((bytesRead = fs.Read(buffer, 0, buffer.Length)) != 0)
        {
            requestStream.Write(buffer, 0, bytesRead);
        }

        requestStream.Flush();
    }
}
```

## <a name="response-parameters"></a>Parámetros de respuesta

Los resultados se proporcionan como JSON. El formato `simple` incluye los siguientes campos de nivel superior.

| Parámetro | Descripción  |
|-----------|--------------|
|`RecognitionStatus`|Estado, como `Success`, para un reconocimiento correcto. Vea la tabla siguiente.|
|`DisplayText`|Texto reconocido tras mayúsculas, puntuación, normalización inversa de texto (conversión de texto hablado en formularios más cortos, como 200 para "doscientos" o "Dr. Smith" para "doctor smith") y enmascaramiento de palabras soeces. Solo se presenta en caso de corrección.|
|`Offset`|El tiempo (en unidades de 100 nanosegundos) en el que comienza la voz reconocida en la secuencia de audio.|
|`Duration`|La duración (en unidades de 100 nanosegundos) de la voz reconocida en la secuencia de audio.|

El campo `RecognitionStatus` puede contener estos valores:

| Status | Descripción |
|--------|-------------|
| `Success` | El reconocimiento es correcto y el campo `DisplayText` está presente. |
| `NoMatch` | Se detectó voz en la secuencia de audio, pero no se encontraron coincidencias de palabras en el idioma de destino. Normalmente significa que el idioma de reconocimiento es un idioma distinto al que habla el usuario. |
| `InitialSilenceTimeout` | El inicio de la secuencia de audio contiene solo silencio y el servicio ha agotado el tiempo de espera de la voz. |
| `BabbleTimeout` | El inicio de la secuencia de audio contiene solo ruido y el servicio ha agotado el tiempo de espera de la voz. |
| `Error` | El servicio de reconocimiento ha detectado un error interno y no ha podido continuar. Vuelva a intentarlo si es posible. |

> [!NOTE]
> Si el audio consta solo de palabras soeces y el parámetro de consulta `profanity` está establecido en `remove`, el servicio no devuelve ningún resultado de voz.

El formato `detailed` incluye los mismos datos que el formato `simple`, junto con `NBest`, una lista de interpretaciones alternativas del mismo resultado de reconocimiento. Estos resultados se clasifican de más probable a menos probable. La primera entrada es la misma que el resultado de reconocimiento principal.  Cuando se usa el formato `detailed`, se proporciona `DisplayText` como `Display` para cada resultado de la lista `NBest`.

Cada objeto de la lista `NBest` incluye:

| Parámetro | Descripción |
|-----------|-------------|
| `Confidence` | La puntuación de confianza de la entrada de 0,0 (ninguna confianza) a 1,0 (plena confianza) |
| `Lexical` | La forma léxica del texto reconocido: palabras reales reconocidas. |
| `ITN` | El formato de normalización inversa de texto ("canónica") del texto reconocido, con números de teléfono, números, abreviaturas ("doctor smith" a "dr smith") y otras transformaciones aplicadas. |
| `MaskedITN` | Formato ITN con enmascaramiento de palabras soeces aplicado, si se solicita. |
| `Display` | Formato de presentación del texto reconocido, con adición de signos de puntuación y mayúsculas. Este parámetro es el mismo que `DisplayText` que se proporcionó cuando el formato se estableció en `simple`. |

## <a name="sample-responses"></a>Respuestas de ejemplo

La siguiente es una respuesta típica de reconocimiento de `simple`:

```json
{
  "RecognitionStatus": "Success",
  "DisplayText": "Remind me to buy 5 pencils.",
  "Offset": "1236645672289",
  "Duration": "1236645672289"
}
```

La siguiente es una respuesta típica de reconocimiento de `detailed`:

```json
{
  "RecognitionStatus": "Success",
  "Offset": "1236645672289",
  "Duration": "1236645672289",
  "NBest": [
      {
        "Confidence" : "0.87",
        "Lexical" : "remind me to buy five pencils",
        "ITN" : "remind me to buy 5 pencils",
        "MaskedITN" : "remind me to buy 5 pencils",
        "Display" : "Remind me to buy 5 pencils.",
      },
      {
        "Confidence" : "0.54",
        "Lexical" : "rewind me to buy five pencils",
        "ITN" : "rewind me to buy 5 pencils",
        "MaskedITN" : "rewind me to buy 5 pencils",
        "Display" : "Rewind me to buy 5 pencils.",
      }
  ]
}
```

## <a name="next-steps"></a>Pasos siguientes

- [Obtenga su suscripción de prueba a Voz](https://azure.microsoft.com/try/cognitive-services/)
- [Personalización de modelos acústicos](how-to-customize-acoustic-models.md)
- [Personalización de modelos de lenguaje](how-to-customize-language-model.md)
