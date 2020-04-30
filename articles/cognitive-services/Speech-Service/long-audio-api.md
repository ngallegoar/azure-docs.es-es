---
title: 'Long Audio API (versión preliminar): servicio de voz'
titleSuffix: Azure Cognitive Services
description: Obtenga información sobre cómo se ha diseñado Long Audio API para la síntesis asincrónica de texto de formato largo a voz.
services: cognitive-services
author: trevorbye
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 01/30/2020
ms.author: trbye
ms.openlocfilehash: b7cca314ec59e46cf17751b1aec28b5c3ea029ed
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/29/2020
ms.locfileid: "81401074"
---
# <a name="long-audio-api-preview"></a>Long Audio API (versión preliminar)

Long Audio API está diseñada para la síntesis asincrónica de texto de formato largo a voz (por ejemplo: audiolibros). Esta API no devuelve audio sintetizado en tiempo real, sino que, en su lugar, se espera que se sondeen las respuestas y se consuman las salidas a medida que estén disponibles desde el servicio. A diferencia de Text To Speech API, que usa el SDK de Voz, Long Audio API puede crear audio sintetizado de más de diez minutos, lo que resulta idóneo para los editores y las plataformas de contenido de audio.

Ventajas adicionales de Long Audio API:

* La voz sintetizada devuelta por el servicio utiliza voces neuronales, lo que garantiza salidas de audio de alta fidelidad.
* Dado que no se admiten las respuestas en tiempo real, no es necesario implementar un punto de conexión de voz.

> [!NOTE]
> Long Audio API ahora solo admite [Voz neuronal personalizada](https://docs.microsoft.com/azure/cognitive-services/speech-service/how-to-custom-voice#custom-neural-voices).

## <a name="workflow"></a>Flujo de trabajo

Normalmente, cuando use Long Audio API, enviará un archivo de texto o archivos que se sintetizarán, sondeará el estado y, a continuación, si el estado es correcto, podrá descargar la salida de audio.

El diagrama a continuación proporciona una introducción general del flujo de trabajo.

![Diagrama de flujo de trabajo de Long Audio API](media/long-audio-api/long-audio-api-workflow.png)

## <a name="prepare-content-for-synthesis"></a>Preparación del contenido para la síntesis

Cuando prepare el archivo de texto, asegúrese de lo siguiente:

* Se trata de texto sin formato (.txt) o texto SSML (.txt).
* Está codificado como [UTF-8 con marca BOM](https://www.w3.org/International/questions/qa-utf8-bom.en#bom).
* Es un solo archivo, no un archivo ZIP.
* Contiene más de 400 caracteres para texto sin formato o 400 [caracteres facturables](https://docs.microsoft.com/azure/cognitive-services/speech-service/text-to-speech#pricing-note) para texto SSML y menos de 10 000 párrafos
  * En el caso de texto sin formato, cada párrafo se separa al presionar **Entrar**: consulte un [ejemplo de entrada de texto sin formato](https://github.com/Azure-Samples/Cognitive-Speech-TTS/blob/master/CustomVoice-API-Samples/Java/en-US.txt).
  * En el caso de texto SSML, cada fragmento de SSML se considera un párrafo. Los fragmentos de SSML se deben separar con párrafos distintos: consulte un [ejemplo de entrada de texto SSML](https://github.com/Azure-Samples/Cognitive-Speech-TTS/blob/master/CustomVoice-API-Samples/Java/SSMLTextInputSample.txt).
> [!NOTE]
> En el caso del chino (continental), chino (Hong Kong), chino (Taiwán), japonés y coreano, una palabra se contará como dos caracteres. 

## <a name="submit-synthesis-requests"></a>Envío de solicitudes de síntesis

Después de preparar el contenido de entrada, siga el [inicio rápido de síntesis de audio de forma larga](https://aka.ms/long-audio-python) para enviar la solicitud. Si tiene más de un archivo de entrada, tendrá que enviar varias solicitudes. Existen algunas limitaciones a tener en cuenta, por ejemplo: 
* El cliente puede enviar hasta 5 solicitudes al servidor por segundo para cada cuenta de suscripción de Azure. Si supera la limitación, el cliente obtendrá un código de error 429 (demasiadas solicitudes). Reduzca la cantidad de solicitudes por segundo.
* El servidor puede ejecutarse y poner en cola hasta 120 solicitudes para cada cuenta de suscripción de Azure. Si supera la limitación, el servidor devolverá un código de error 429 (demasiadas solicitudes). Espere y evite enviar una nueva solicitud hasta que se completen algunas solicitudes.
* El servidor mantendrá hasta 20 000 solicitudes para cada cuenta de suscripción de Azure. Si supera la limitación, elimine algunas solicitudes antes de enviar otras nuevas.

## <a name="audio-output-formats"></a>Formatos de salida de audio

Se admiten formatos de salida de audio flexibles. Puede generar salidas de audio por párrafo o concatenar los audios en una salida estableciendo el parámetro "concatenateResult". Long Audio API admite los siguientes formatos de salida de audio:

> [!NOTE]
> El formato de audio predeterminado es riff-16khz-16bit-mono-pcm.

* riff-8khz-16bit-mono-pcm
* riff-16khz-16bit-mono-pcm
* riff-24khz-16bit-mono-pcm
* riff-48khz-16bit-mono-pcm
* audio-16khz-32kbitrate-mono-mp3
* audio-16khz-64kbitrate-mono-mp3
* audio-16khz-128kbitrate-mono-mp3
* audio-24khz-48kbitrate-mono-mp3
* audio-24khz-96kbitrate-mono-mp3
* audio-24khz-160kbitrate-mono-mp3

## <a name="quickstarts"></a>Guías de inicio rápido

Se ofrecen guías de inicio rápido diseñadas para ayudarle a ejecutar Long Audio API correctamente. Esta tabla incluye una lista de inicios rápidos de Long Audio API en función del idioma.

* [Inicio rápido: Python](https://aka.ms/long-audio-python)

## <a name="sample-code"></a>Código de ejemplo
Hay un ejemplo de código para Long Audio API disponible en GitHub.

* [Código de ejemplo: Python](https://github.com/Azure-Samples/Cognitive-Speech-TTS/tree/master/CustomVoice-API-Samples/Python)
* [Código de ejemplo: C#](https://github.com/Azure-Samples/Cognitive-Speech-TTS/tree/master/CustomVoice-API-Samples/CSharp)
* [Código de ejemplo: Java](https://github.com/Azure-Samples/Cognitive-Speech-TTS/blob/master/CustomVoice-API-Samples/Java/)
