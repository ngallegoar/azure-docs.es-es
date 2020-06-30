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
ms.openlocfilehash: ca6bff4c1e99bb8e63db212ca57693870afc30e7
ms.sourcegitcommit: 971a3a63cf7da95f19808964ea9a2ccb60990f64
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/19/2020
ms.locfileid: "85080645"
---
# <a name="long-audio-api-preview"></a>Long Audio API (versión preliminar)

Long Audio API está diseñada para la síntesis asincrónica de texto de formato largo a voz (por ejemplo: audiolibros, artículos de noticias y documentos). Esta API no devuelve audio sintetizado en tiempo real, sino que, en su lugar, se espera que se sondeen las respuestas y se consuman las salidas a medida que estén disponibles desde el servicio. A diferencia de Text To Speech API, que usa el SDK de Voz, Long Audio API puede crear audio sintetizado de más de diez minutos, lo que resulta idóneo para los editores y las plataformas de contenido de audio.

Ventajas adicionales de Long Audio API:

* La voz sintetizada devuelta por el servicio usa las mejores voces neuronales.
* No es necesario implementar un punto de conexión de voz ya que no sintetiza las voces en ningún modo por lotes en tiempo real.

> [!NOTE]
> Long Audio API admite ahora [voces neuronales públicas](https://docs.microsoft.com/azure/cognitive-services/speech-service/language-support#neural-voices) y [voces neuronales personalizadas](https://docs.microsoft.com/azure/cognitive-services/speech-service/how-to-custom-voice#custom-neural-voices).

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
> En el caso del chino (continental), chino (Hong Kong SAR), chino (Taiwán), japonés y coreano, una palabra se contará como dos caracteres. 

## <a name="submit-synthesis-requests"></a>Envío de solicitudes de síntesis

Después de preparar el contenido de entrada, siga el [inicio rápido de síntesis de audio de forma larga](https://aka.ms/long-audio-python) para enviar la solicitud. Si tiene más de un archivo de entrada, tendrá que enviar varias solicitudes. 

Los **códigos de estado HTTP** indican errores comunes.

| API | Código de estado HTTP | Descripción | Propuesta |
|-----|------------------|-------------|----------|
| Crear | 400 | La síntesis de voz no está habilitada en esta región. | Cambie la clave de suscripción de voz a una región admitida. |
|        | 400 | Solo es válida la suscripción **estándar** para esta región. | Cambie la clave de suscripción de voz al plan de tarifa "estándar". |
|        | 400 | Se superó el límite de solicitudes de 20 000 para la cuenta de Azure. Quite algunas solicitudes antes de enviar otras nuevas. | El servidor mantendrá hasta 20 000 solicitudes para cada cuenta de suscripción de Azure. Elimine algunas solicitudes antes de enviar otras nuevas. |
|        | 400 | Este modelo no se puede usar en la síntesis de voz: {modelID}. | Asegúrese de que el estado de {modelID} sea correcto. |
|        | 400 | La región de la solicitud no coincide con la región del modelo: {modelID}. | Asegúrese de que la región de {modelID} coincida con la región de la solicitud. |
|        | 400 | La síntesis de voz solo admite el archivo de texto en la codificación UTF-8 con el marcador de orden de bytes. | Asegúrese de que los archivos de entrada están en codificación UTF-8 con el marcador de orden de bytes. |
|        | 400 | Solo se permiten entradas SSML válidas en la solicitud de síntesis de voz. | Asegúrese de que las expresiones SSML de entrada sean correctas. |
|        | 400 | No se encuentra el nombre de voz {voiceName} en el archivo de entrada. | El nombre de voz SSML de entrada no está alineado con el id. de modelo. |
|        | 400 | El número de párrafos del archivo de entrada debe ser inferior a 10 000. | Asegúrese de que el número de párrafos del archivo sea inferior a 10 000. |
|        | 400 | El archivo de entrada debe tener más de 400 caracteres. | Asegúrese de que el archivo de entrada supere los 400 caracteres. |
|        | 404 | No se encuentra el modelo declarado en la definición de síntesis de voz: {modelID}. | Asegúrese de que {modelID} sea correcto. |
|        | 429 | Se superó el límite de síntesis de voz activa. Espere hasta que finalicen algunas solicitudes. | El servidor puede ejecutar y poner en cola hasta 120 solicitudes para cada cuenta de Azure. Espere y evite enviar nuevas solicitudes hasta que se completen algunas solicitudes. |
| All       | 429 | Hay demasiadas solicitudes. | El cliente puede enviar hasta 5 solicitudes al servidor por segundo para cada cuenta de Azure. Reduzca la cantidad de solicitudes por segundo. |
| Eliminar    | 400 | La tarea de síntesis de voz todavía está en uso. | Solo se pueden eliminar solicitudes **Completadas** o **Con errores**. |
| GetByID   | 404 | No se encuentra la entidad especificada. | Asegúrese de que el id. de síntesis sea correcto. |

## <a name="regions-and-endpoints"></a>Regiones y puntos de conexión

Long Audio API está disponible en varias regiones con puntos de conexión únicos.

| Region | Punto de conexión |
|--------|----------|
| Este de Australia | `https://australiaeast.customvoice.api.speech.microsoft.com` |
| Centro de Canadá | `https://canadacentral.customvoice.api.speech.microsoft.com` |
| Este de EE. UU. | `https://eastus.customvoice.api.speech.microsoft.com` |
| India central | `https://centralindia.customvoice.api.speech.microsoft.com` |
| Centro-sur de EE. UU. | `https://southcentralus.customvoice.api.speech.microsoft.com` |
| Sudeste de Asia | `https://southeastasia.customvoice.api.speech.microsoft.com` |
| Sur de Reino Unido | `https://uksouth.customvoice.api.speech.microsoft.com` |
| Oeste de Europa | `https://westeurope.customvoice.api.speech.microsoft.com` |
| Oeste de EE. UU. 2 | `https://westus2.customvoice.api.speech.microsoft.com` |

## <a name="audio-output-formats"></a>Formatos de salida de audio

Se admiten formatos de salida de audio flexibles. Puede generar salidas de audio por párrafo o concatenar las salidas de audio en una sola salida estableciendo el parámetro "concatenateResult". Long Audio API admite los siguientes formatos de salida de audio:

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
