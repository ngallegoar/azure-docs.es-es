---
title: ¿Qué es la transcripción por lotes? Servicio de Voz
titleSuffix: Azure Cognitive Services
description: La transcripción de lotes es ideal si desea transcribir una gran cantidad de audio en el almacenamiento, como los blobs de Azure. Mediante la API REST dedicada, puede apuntar a archivos de audio con un identificador URI de firma de acceso compartido (SAS) y recibir las transcripciones de forma asincrónica.
services: cognitive-services
author: wolfma61
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 03/18/2020
ms.author: wolfma
ms.openlocfilehash: 9804992aee318fdc34815bdbe4187144704cd667
ms.sourcegitcommit: 51718f41d36192b9722e278237617f01da1b9b4e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/19/2020
ms.locfileid: "85099777"
---
# <a name="what-is-batch-transcription"></a>¿Qué es la transcripción por lotes?

La transcripción por lotes es un conjunto de operaciones de API REST que permite transcribir una gran cantidad de audio en el almacenamiento. Puede apuntar a archivos de audio con un identificador URI de firma de acceso compartido (SAS) y recibir los resultados de las transcripciones de forma asincrónica. Con la nueva API v3.0, tiene la opción de transcribir uno o varios archivos de audio, o bien de procesar un contenedor de almacenamiento completo.

La transcripción de voz a texto asincrónica es solo una de las características. Puede usar las API REST de transcripción por lotes para llamar a los métodos siguientes:



|    Operación de transcripción por lotes                                             |    Método    |    Llamada a API REST                                   |
|------------------------------------------------------------------------------|--------------|----------------------------------------------------|
|    Crea una transcripción nueva.                                              |    POST      |    speechtotext/v3.0/transcriptions            |
|    Recupera una lista de transcripciones para la suscripción autenticada.    |    GET       |    speechtotext/v3.0/transcriptions            |
|    Obtiene una lista de configuraciones regionales admitidas para las transcripciones sin conexión.              |    GET       |    speechtotext/v3.0/transcriptions/locales    |
|    Actualiza los detalles mutables de la transcripción identificada por su id.    |    PATCH     |    speechtotext/v3.0/transcriptions/{id}       |
|    Elimina la tarea de transcripción especificada.                                 |    Delete    |    speechtotext/v3.0/transcriptions/{id}       |
|    Obtiene la transcripción identificada por el id. especificado.                        |    GET       |    speechtotext/v3.0/transcriptions/{id}       |
|    Obtiene los archivos de resultado de la transcripción identificada por el id. especificado.    |    GET       |    speechtotext/v3.0/transcriptions/{id}/files |




Puede revisar y probar la API detallada, que está disponible como un [documento de Swagger](https://westus.dev.cognitive.microsoft.com/docs/services/speech-to-text-api-v3-0).

Los trabajos de transcripción por lotes se programan de la mejor manera posible. Actualmente no hay ninguna estimación de cuándo un trabajo cambia al estado en ejecución. En la carga normal del sistema, se debe producir en cuestión de minutos. En este estado, la transcripción real se procesa más rápido que el audio en tiempo real.

Además de esta intuitiva API, no es necesario implementar puntos de conexión personalizados y no tiene que cumplir ningún requisito de simultaneidad.

## <a name="prerequisites"></a>Requisitos previos

### <a name="subscription-key"></a>Clave de suscripción

Como sucede con todas las características del servicio Voz, puede crear una clave de suscripción en [Azure Portal](https://portal.azure.com) siguiendo nuestra [guía de inicio](get-started.md).

>[!NOTE]
> Se requiere una suscripción estándar (S0) para el servicio de voz para usar la transcripción de lotes. Las claves de suscripción gratuita (F0) no funcionan. Para obtener más información, consulte los [precios y límites](https://azure.microsoft.com/pricing/details/cognitive-services/speech-services/).

### <a name="custom-models"></a>Modelos personalizados

Si tiene previsto personalizar los modelos, siga los pasos descritos en [Personalización acústica](how-to-customize-acoustic-models.md) y [Personalización de idioma](how-to-customize-language-model.md). Para usar los modelos creados en la transcripción por lotes, necesita sus ubicaciones. Puede recuperar la ubicación del modelo al inspeccionar los detalles del modelo (propiedad `self`). *No es necesario* un punto de conexión personalizado implementado para el servicio de transcripción por lotes.

## <a name="the-batch-transcription-api"></a>Transcription API de Azure Batch

### <a name="supported-formats"></a>Formatos compatibles

Transcription API de Batch admite los siguientes formatos:

| Formato | Códec | Bitrate | Velocidad de muestreo                     |
|--------|-------|---------|---------------------------------|
| WAV    | PCM   | 16 bits  | 8 kHz o 16 kHz, mono o estéreo |
| MP3    | PCM   | 16 bits  | 8 kHz o 16 kHz, mono o estéreo |
| OGG    | OPUS  | 16 bits  | 8 kHz o 16 kHz, mono o estéreo |

En el caso de las secuencias de audio estéreo, los canales izquierdo y derecho se dividen durante la transcripción. Para cada canal se crea un archivo de resultados JSON. Las marcas de tiempo por expresión generadas permiten al desarrollador crear una transcripción final ordenada.

### <a name="configuration"></a>Configuración

Los parámetros de configuración se proporcionan como JSON (uno o más archivos individuales):

```json
{
  "contentUrls": [
    "<URL to an audio file to transcribe>",
  ],
  "properties": {
    "wordLevelTimestampsEnabled": true
  },
  "locale": "en-US",
  "displayName": "Transcription of file using default model for en-US"
}
```

Los parámetros de configuración se proporcionan como JSON (se procesa un contenedor de almacenamiento completo):

```json
{
  "contentContainerUrl": "<SAS URL to the Azure blob container to transcribe>",
  "properties": {
    "wordLevelTimestampsEnabled": true
  },
  "locale": "en-US",
  "displayName": "Transcription of container using default model for en-US"
}
```

Para usar modelos entrenados personalizados en las transcripciones de lotes, se puede hacer referencia a ellos como se muestra a continuación:

```json
{
  "contentUrls": [
    "<URL to an audio file to transcribe>",
  ],
  "properties": {
    "wordLevelTimestampsEnabled": true
  },
  "locale": "en-US",
  "model": {
    "self": "https://westus.api.cognitive.microsoft.com/speechtotext/v3.0/models/{id}"
  },
  "displayName": "Transcription of file using default model for en-US"
}
```


### <a name="configuration-properties"></a>Propiedades de configuración

Utilice estas propiedades opcionales para configurar la transcripción:

:::row:::
   :::column span="1":::
      **Parámetro**
   :::column-end:::
   :::column span="2":::
      **Descripción**
:::row-end:::
:::row:::
   :::column span="1":::
      `profanityFilterMode`
   :::column-end:::
   :::column span="2":::
      Especifica cómo controlar las palabras soeces en los resultados del reconocimiento. Los valores aceptados son `None`, para deshabilitar el filtrado de palabras soeces; `Masked`, para reemplazar las palabras soeces con asteriscos; `Removed`, para quitar todas las palabras soeces del resultado; o `Tags`, para agregar etiquetas de "palabras soeces". El valor predeterminado es `Masked`.
:::row-end:::
:::row:::
   :::column span="1":::
      `punctuationMode`
   :::column-end:::
   :::column span="2":::
      Especifica cómo controlar la puntuación en los resultados del reconocimiento. Los valores aceptados son `None`, para deshabilitar la puntuación; `Dictated`, para implicar signos de puntuación explícitos (hablados); `Automatic`, para permitir que el descodificador trate con signos de puntuación o `DictatedAndAutomatic`, para usar la puntuación dictada y automática. El valor predeterminado es `DictatedAndAutomatic`.
:::row-end:::
:::row:::
   :::column span="1":::
      `wordLevelTimestampsEnabled`
   :::column-end:::
   :::column span="2":::
      Especifica si las marcas de tiempo de nivel de palabra se deben agregar a la salida. Los valores aceptados son `true`, para permitir las marcas de tiempo de nivel de palabra, y `false` (el valor predeterminado) para deshabilitarlas.
:::row-end:::
:::row:::
   :::column span="1":::
      `diarizationEnabled`
   :::column-end:::
   :::column span="2":::
      Especifica que el análisis de la diarización se debe llevar a cabo en la entrada que se espera sea un canal mono que contenga dos voces. Los valores aceptados son `true`, para permite la diarización, y `false` (el valor predeterminado), para deshabilitarla. También requiere que `wordLevelTimestampsEnabled` se establezca en true.
:::row-end:::
:::row:::
   :::column span="1":::
      `channels`
   :::column-end:::
   :::column span="2":::
      Matriz opcional de números de canal que se procesará. Aquí se puede especificar un subconjunto de los canales disponibles en el archivo de audio para procesarse (por ejemplo, solo `0`). Si no se especifica, los canales `0` y `1` se transcriben como valores predeterminados.
:::row-end:::
:::row:::
   :::column span="1":::
      `timeToLive`
   :::column-end:::
   :::column span="2":::
      Plazo opcional para eliminar automáticamente las transcripciones después de completar el proceso de transcripción. `timeToLive` resulta útil en el procesamiento en masa de transcripciones, para asegurarse de que se eliminarán (por ejemplo, `PT12H`). Si no se especifica o establece en `PT0H`, la transcripción no se eliminará automáticamente.
:::row-end:::
:::row:::
   :::column span="1":::
      `destinationContainerUrl`
   :::column-end:::
   :::column span="2":::
      URL opcional con [SAS de servicio](../../storage/common/storage-sas-overview.md) en un contenedor grabable de Azure. El resultado se almacena en este contenedor. Cuando no se especifica, Microsoft almacena los resultados en un contenedor de almacenamiento administrado por Microsoft. Cuando se elimina la transcripción mediante una llamada a [Delete transcription](https://westus.dev.cognitive.microsoft.com/docs/services/speech-to-text-api-v3-0/operations/DeleteTranscription), también se eliminan los datos del resultado.
:::row-end:::

### <a name="storage"></a>Storage

La transcripción de lotes admite [Azure Blob Storage](https://docs.microsoft.com/azure/storage/blobs/storage-blobs-overview) para leer el audio y escribir las transcripciones en el almacenamiento.

## <a name="the-batch-transcription-result"></a>El resultado de la transcripción por lotes

Para cada audio de entrada, se crea un archivo de resultado de transcripción. Puede obtener la lista de archivos de resultados si llama a [Get transcriptions files](https://westus.dev.cognitive.microsoft.com/docs/services/speech-to-text-api-v3-0/operations/GetTranscriptionFiles). Este método devuelve una lista de archivos de resultados correspondientes a esta transcripción. Para buscar el archivo de transcripción de un archivo de entrada específico, filtre todos los archivos devueltos con `kind` == `Transcription` y `name` == `{originalInputName.suffix}.json`.

Cada archivo de resultado de transcripción tiene el siguiente formato:

```json
{
  "source": "...",                                                 // the sas url of a given contentUrl or the path relative to the root of a given container
  "timestamp": "2020-06-16T09:30:21Z",                             // creation time of the transcription, ISO 8601 encoded timestamp, combined date and time
  "durationInTicks": 41200000,                                     // total audio duration in ticks (1 tick is 100 nanoseconds)
  "duration": "PT4.12S",                                           // total audio duration, ISO 8601 encoded duration
  "combinedRecognizedPhrases": [                                   // concatenated results for simple access in single string for each channel
    {
      "channel": 0,                                                // channel number of the concatenated results
      "lexical": "hello world",
      "itn": "hello world",
      "maskedITN": "hello world",
      "display": "Hello world."
    }
  ],
  "recognizedPhrases": [                                           // results for each phrase and each channel individually
    {
      "recognitionStatus": "Success",                              // recognition state, e.g. "Success", "Failure"
      "channel": 0,                                                // channel number of the result
      "offset": "PT0.07S",                                         // offset in audio of this phrase, ISO 8601 encoded duration 
      "duration": "PT1.59S",                                       // audio duration of this phrase, ISO 8601 encoded duration
      "offsetInTicks": 700000.0,                                   // offset in audio of this phrase in ticks (1 tick is 100 nanoseconds)
      "durationInTicks": 15900000.0,                               // audio duration of this phrase in ticks (1 tick is 100 nanoseconds)
      
      // possible transcriptions of the current phrase with confidences
      "nBest": [
        {
          "confidence": 0.898652852,                               // confidence value for the recognition of the whole phrase
          "speaker": 1,                                            // if `diarizationEnabled` is `true`, this is the identified speaker (1 or 2), otherwise this property is not present
          "lexical": "hello world",
          "itn": "hello world",
          "maskedITN": "hello world",
          "display": "Hello world.",
          
          // if wordLevelTimestampsEnabled is `true`, there will be a result for each word of the phrase, otherwise this property is not present
          "words": [
            {
              "word": "hello",
              "offset": "PT0.09S",
              "duration": "PT0.48S",
              "offsetInTicks": 900000.0,
              "durationInTicks": 4800000.0,
              "confidence": 0.987572
            },
            {
              "word": "world",
              "offset": "PT0.59S",
              "duration": "PT0.16S",
              "offsetInTicks": 5900000.0,
              "durationInTicks": 1600000.0,
              "confidence": 0.906032
            }
          ]
        }
      ]    
    }
  ]
}
```

El resultado contiene estas formas:

:::row:::
   :::column span="1":::
      **Forma**
   :::column-end:::
   :::column span="2":::
      **Contenido**
:::row-end:::
:::row:::
   :::column span="1":::
      `lexical`
   :::column-end:::
   :::column span="2":::
      Las palabras reales reconocidas.
:::row-end:::
:::row:::
   :::column span="1":::
      `itn`
   :::column-end:::
   :::column span="2":::
      El formato de normalización inversa de texto del texto reconocido. Se aplican las abreviaturas ("doctor Pérez" a "Dr Pérez"), los números de teléfono y otras transformaciones.
:::row-end:::
:::row:::
   :::column span="1":::
      `maskedITN`
   :::column-end:::
   :::column span="2":::
      El formato ITN con enmascaramiento de palabras soeces aplicado.
:::row-end:::
:::row:::
   :::column span="1":::
      `display`
   :::column-end:::
   :::column span="2":::
      El formato mostrado del texto reconocido. Se incluyen el uso de mayúsculas y minúsculas y la puntuación que se agrega.
:::row-end:::

## <a name="speaker-separation-diarization"></a>Separación de altavoces (diarización)

La diarización es el proceso de separación de los altavoces en una parte del audio. Nuestra canalización de Batch admite la diarización y es capaz de reconocer dos altavoces en las grabaciones de un canal mono. La característica no está disponible en las grabaciones estéreo.

La salida de la transcripción con la opción de diarización habilitada contiene una entrada `Speaker` para cada frase transcrita. Si no se usa la diarización, la propiedad `Speaker` no está presente en la salida JSON. Para la diarización, se admiten dos voces, por lo que los oradores se identifican como `1` o `2`.

Para solicitar la diarización, basta con agregar el parámetro correspondiente en la solicitud HTTP, como se muestra a continuación.

 ```json
{
  "contentUrls": [
    "<URL to an audio file to transcribe>",
  ],
  "properties": {
    "diarizationEnabled": true,
    "wordLevelTimestampsEnabled": true,
    "punctuationMode": "DictatedAndAutomatic",
    "profanityFilterMode": "Masked"
  },
  "locale": "en-US",
  "displayName": "Transcription of file using default model for en-US"
}
```

Las marcas de tiempo de nivel de palabra deben habilitarse, como indican los parámetros de la solicitud anterior.

## <a name="best-practices"></a>Procedimientos recomendados

El servicio transcripción puede controlar un gran número de transcripciones enviadas. Puede consultar el estado de las transcripciones a través de un elemento `GET` en [Get transcriptions](https://westus.dev.cognitive.microsoft.com/docs/services/speech-to-text-api-v3-0/operations/GetTranscriptions). Realice una llamada a [Delete transcription](https://westus.dev.cognitive.microsoft.com/docs/services/speech-to-text-api-v3-0/operations/DeleteTranscription) periódicamente desde el servicio cuando se hayan recuperado los resultados. También puede establecer la propiedad `timeToLive` en un valor razonable para garantizar que los resultados se eliminen en algún momento.

## <a name="sample-code"></a>Código de ejemplo

Hay ejemplos completos disponibles en el [repositorio de ejemplos de GitHub](https://aka.ms/csspeech/samples) dentro del subdirectorio `samples/batch`.

Si quiere usar un modelo personalizado, actualice el código de ejemplo con la información de suscripción, la región del servicio, el URI de SAS que apunta al archivo de audio que se va a transcribir y la ubicación del modelo.

[!code-csharp[Configuration variables for batch transcription](~/samples-cognitive-services-speech-sdk/samples/batch/csharp/program.cs#transcriptiondefinition)]

El código de ejemplo configura el cliente y envía la solicitud de transcripción. A continuación, sondea la información de estado e imprime los detalles sobre el progreso de la transcripción.

[!code-csharp[Code to check batch transcription status](~/samples-cognitive-services-speech-sdk/samples/batch/csharp/program.cs#transcriptionstatus)]

Para más información sobre las llamadas anteriores, consulte nuestro [documento de Swagger](https://westus.dev.cognitive.microsoft.com/docs/services/speech-to-text-api-v3-0). Para ver el ejemplo completo aquí mostrado, vaya a [GitHub](https://aka.ms/csspeech/samples) en el subdirectorio `samples/batch`.

Tome nota de la configuración asincrónica para publicar audio y recibir el estado de la transcripción. El cliente que crea es un cliente HTTP de .NET. Hay un método `PostTranscriptions` para enviar los detalles del archivo de audio y un método `GetTranscriptions` para recibir los estados. `PostTranscriptions` devuelve un identificador y `GetTranscriptions` lo usa para crear un identificador y obtener el estado de la transcripción.

El ejemplo de código actual no especifica un modelo personalizado. El servicio usa el modelo base de referencia para transcribir los archivos. Para especificar el modelo, puede pasar la referencia de modelo del modelo personalizado en el mismo método.

> [!NOTE]
> En el caso de las transcripciones de base de referencia, no es necesario declarar los identificadores del modelo de base de referencia.

## <a name="download-the-sample"></a>Descarga del ejemplo

Puede encontrar el ejemplo en el directorio `samples/batch` en el [repositorio de ejemplos de GitHub](https://aka.ms/csspeech/samples).

## <a name="next-steps"></a>Pasos siguientes

- [Obtenga su suscripción de prueba a Voz](https://azure.microsoft.com/try/cognitive-services/)
