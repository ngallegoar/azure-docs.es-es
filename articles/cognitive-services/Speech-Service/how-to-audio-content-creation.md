---
title: 'Audio Content Creation: servicio de voz'
titleSuffix: Azure Cognitive Services
description: Audio Content Creation es una herramienta en línea que le permite personalizar y ajustar la salida de texto a voz de Microsoft para sus aplicaciones y productos.
services: cognitive-services
author: trevorbye
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 01/31/2020
ms.author: trbye
ms.openlocfilehash: 083580435c467a7d4b6a4cede0a821a2c271962f
ms.sourcegitcommit: bb0afd0df5563cc53f76a642fd8fc709e366568b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/19/2020
ms.locfileid: "83589659"
---
# <a name="improve-synthesis-with-the-audio-content-creation-tool"></a>Mejora de la síntesis con la herramienta Audio Content Creation

[Audio Content Creation](https://aka.ms/audiocontentcreation) es una herramienta en línea que le permite personalizar y ajustar la salida de texto a voz de Microsoft para sus aplicaciones y productos. Puede usar esta herramienta para ajustar las voces públicas y personalizadas a fin de lograr expresiones naturales más precisas y administrar el resultado en la nube.

La herramienta Audio Content Creation se basa en el [lenguaje de marcado de síntesis de voz (SSML)](speech-synthesis-markup.md). Para simplificar la personalización y ajuste, Audio Content Creation le permite inspeccionar visualmente las salidas de texto a voz en tiempo real.

## <a name="how-does-it-work"></a>¿Cómo funciona?

En este diagrama se muestran los pasos necesarios para ajustar las salidas de texto a voz. Use los siguientes vínculos para obtener más información sobre cada paso.

![](media/audio-content-creation/audio-content-creation-diagram.jpg)

1. [Configuración de la cuenta de Azure y el recurso de voz](#set-up-your-azure-account-and-speech-resource) para comenzar.
2. [Creación de un archivo de ajuste de audio](#create-an-audio-tuning-file) con texto sin formato o scripts SSML.
3. Elija la voz y el lenguaje del contenido del script. Audio Content Creation incluye todas las [voces de texto a voz de Microsoft](language-support.md#text-to-speech). Puede usar voces estándar, neuronales o una propia personalizada.
   >[!NOTE]
   > El acceso controlado está disponible para las voces neuronales personalizadas, que le permiten crear voces de alta definición similares a una voz natural. Para obtener más información, consulte [Proceso de acceso controlado](https://aka.ms/ignite2019/speech/ethics).

4. Revise la salida de la síntesis predeterminada. A continuación, mejore la salida mediante el ajuste de la pronunciación, las interrupciones, el tono, la velocidad, la entonación, el estilo de voz, etc. Para obtener una lista completa de opciones, consulte [Lenguaje de marcado de síntesis de voz](speech-synthesis-markup.md). En este [vídeo](https://youtu.be/mUvf2NbfuYU) se muestra cómo ajustar la salida de voz con Audio Content Creation. 
5. Guarde y [exporte el audio optimizado](#export-tuned-audio). Cuando guarde la pista de ajuste en el sistema, podrá seguir trabajando e iterar en la salida. Cuando esté satisfecho con el resultado, puede crear una tarea de creación de audio con la característica de exportación. Puede observar el estado de la tarea de exportación y descargar la salida para usarla con sus aplicaciones y productos.

## <a name="set-up-your-azure-account-and-speech-resource"></a>Configuración de la cuenta de Azure y el recurso de voz

1. Para trabajar con Audio Content Creation, debe tener una cuenta de Azure. Puede crear una cuenta de Azure con su cuenta de Microsoft. Siga estas instrucciones para [configurar una cuenta de Azure](get-started.md#new-resource). 
2. [Cree un recurso de voz](https://docs.microsoft.com/azure/cognitive-services/speech-service/get-started#create-the-resource) para su cuenta de Azure. Asegúrese de que el plan de tarifa está establecido en **S0**. Si usa una de las voces neuronales, asegúrese de crear el recurso en una [región compatible](regions.md#standard-and-neural-voices).
2. Después de obtener la cuenta de Azure y el recurso de voz, puede usar los servicios de voz y acceder a [Audio Content Creation](https://aka.ms/audiocontentcreation).
3. Seleccione el recurso de voz en el que necesita trabajar. También puede crear un nuevo recurso de voz aquí. 
4. Puede modificar su recurso de voz en cualquier momento con la opción **Configuración**, que se encuentra en la navegación superior.

## <a name="create-an-audio-tuning-file"></a>Creación de un archivo de ajuste de audio

Hay dos maneras de obtener el contenido de la herramienta Audio Content Creation.

**Opción 1:**

1. Haga clic en **Nuevo archivo** para crear un nuevo archivo de ajuste de audio.
2. Escriba o pegue el contenido en la ventana de edición. En número máximo de caracteres de cada archivo es 20 000. Si el script tiene más de 20 000 caracteres, puede usar la opción 2 para dividir automáticamente el contenido en varios archivos. 
3. No olvide guardar.

**Opción 2:**

1. Haga clic en **Cargar** para importar uno o varios archivos de texto. Se admiten el texto sin formato y SSML.
2. Si el archivo de script tiene más de 20 000 caracteres, divida el archivo por párrafos, por caracteres o por expresiones regulares. 
3. Al cargar los archivos de texto, asegúrese de que el archivo cumple estos requisitos.

   | Propiedad | Valor/notas |
   |----------|---------------|
   | Formato de archivo | Texto sin formato (.txt)<br/> Texto en SSML (.txt)<br/> No se admiten los archivos ZIP. |
   | Formato de codificación | UTF-8 |
   | Nombre de archivo | Cada archivo debe tener un nombre único. No se admiten los duplicados. |
   | Longitud del texto | Los archivos de texto no deben tener más de 20 000 caracteres. |
   | Restricciones de SSML | Cada archivo SSML solo puede contener un único fragmento de SSML. |

### <a name="plain-text-example"></a>Ejemplo de texto sin formato

```txt
Welcome to use Audio Content Creation to customize audio output for your products.
```

### <a name="ssml-text-example"></a>Ejemplo de texto en SSML

```xml
<speak xmlns="http://www.w3.org/2001/10/synthesis" xmlns:mstts="http://www.w3.org/2001/mstts" version="1.0" xml:lang="en-US">
    <voice name="Microsoft Server Speech Text to Speech Voice (en-US, AriaNeural)">
    Welcome to use Audio Content Creation <break time="10ms" />to customize audio output for your products.
    </voice>
</speak>
```

## <a name="export-tuned-audio"></a>Exportación de audio ajustado

Una vez que haya revisado la salida de audio y esté satisfecho con la optimización y el ajuste, puede exportar el audio.

1. Haga clic en **Exportar** para crear una tarea de creación de audio. Se recomienda **Export to Audio Library (Exportar a biblioteca de audio)** , ya que admite la salida de audio larga y la experiencia de salida de audio completa. También puede descargar el audio directamente en el disco local, pero solo estarán disponibles los primeros 10 minutos. 
2. Elija el formato de salida para el audio ajustado. A continuación se ofrece una lista de formatos admitidos y frecuencias de muestreo.
3. Puede ver el estado de la tarea en la pestaña **Export task** (tarea de exportación). Si se produce un error en la tarea, consulte la página de información detallada para obtener un informe completo.
4. Una vez completada la tarea, el audio está disponible para su descarga en la pestaña **Audio Library** (biblioteca de audio).
5. Haga clic en **Descargar**. Ahora está listo para usar el audio ajustado personalizado en sus aplicaciones o productos.

### <a name="supported-audio-formats"></a>Formatos de audio compatibles

| Formato | Frecuencia de muestreo de 16 kHz | Frecuencia de muestreo de 24 kHz |
|--------|--------------------|--------------------|
| wav | riff-16khz-16bit-mono-pcm | riff-24khz-16bit-mono-pcm |
| mp3 | audio-16khz-128kbitrate-mono-mp3 | audio-24khz-160kbitrate-mono-mp3 |

## <a name="see-also"></a>Consulte también

* [Long Audio API](https://aka.ms/long-audio-api)

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Speech Studio](https://speech.microsoft.com)
