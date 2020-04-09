---
title: 'Audio Content Creation: servicio de voz'
titleSuffix: Azure Cognitive Services
description: Audio Content Creation es una herramienta en línea que le permite personalizar y ajustar la salida de texto a voz de Microsoft para sus aplicaciones y productos.
services: cognitive-services
author: IEvangelist
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 01/31/2020
ms.author: dapine
ms.openlocfilehash: 8184d30471b4d9171a23c8d03e17b2a54dca2ece
ms.sourcegitcommit: 632e7ed5449f85ca502ad216be8ec5dd7cd093cb
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/30/2020
ms.locfileid: "80397832"
---
# <a name="improve-synthesis-with-audio-content-creation"></a>Mejora de síntesis con Audio Content Creation

[Audio Content Creation](https://aka.ms/audiocontentcreation) es una herramienta en línea que le permite personalizar y ajustar la salida de texto a voz de Microsoft para sus aplicaciones y productos. Puede usar esta herramienta para ajustar las voces públicas y personalizadas a fin de lograr expresiones naturales más precisas y administrar el resultado en la nube.

La herramienta Audio Content Creation se basa en el [lenguaje de marcado de síntesis de voz (SSML)](speech-synthesis-markup.md). Para simplificar la personalización y ajuste, Audio Content Creation le permite inspeccionar visualmente las salidas de texto a voz en tiempo real.

## <a name="how-does-it-work"></a>¿Cómo funciona?

En este diagrama se muestran los pasos necesarios para optimizar y exportar salidas de voz a texto personalizadas. Use los siguientes vínculos para obtener más información sobre cada paso.

![](media/audio-content-creation/audio-content-creation-diagram.jpg)

1. El primer paso es [crear una cuenta de Azure, registrar un recurso de voz y obtener una clave de suscripción](#create-a-speech-resource). Después de tener una clave de suscripción, puede utilizarla para llamar al servicio de voz y para obtener acceso a [Audio Content Creation](https://aka.ms/audiocontentcreation).
2. [Cree un archivo de ajuste de audio](#create-an-audio-tuning-file) con texto sin formato o SSML.
3. Elija la voz y el idioma que quiere optimizar. Audio Content Creation incluye todas las [voces de texto a voz de Microsoft](language-support.md#text-to-speech). Puede usar voces estándar, neuronales o una propia personalizada.
   >[!NOTE]
   > El acceso controlado está disponible para las voces neuronales personalizadas, que le permiten crear voces de alta definición similares a una voz natural. Para obtener más información, consulte [Proceso de acceso controlado](https://aka.ms/ignite2019/speech/ethics).

4. Revise el resultado predeterminado. A continuación, use la herramienta de optimización para ajustar la pronunciación, el tono, la velocidad, la entonación, el estilo de voz, etc. Para obtener una lista completa de opciones, consulte [Lenguaje de marcado de síntesis de voz](speech-synthesis-markup.md).
5. Guarde y [exporte el audio optimizado](#export-tuned-audio). Cuando guarde la pista de optimización en el sistema, podrá seguir trabajando y recorrer en iteración la salida. Cuando esté satisfecho con el resultado, puede crear una tarea de creación de audio con la característica de exportación. Puede observar el estado de la tarea de exportación y descargar la salida para usarla con sus aplicaciones y productos.
6. El último paso es usar la voz personalizada optimizada en sus aplicaciones y productos.

## <a name="create-a-speech-resource"></a>Creación de un recurso de voz

Siga estos pasos para crear un recurso de voz y conectarlo con Speech Studio.

1. Siga estas instrucciones para [registrarse en una cuenta de Azure](get-started.md#new-resource) y [crear un recurso de Voz](https://docs.microsoft.com/azure/cognitive-services/speech-service/get-started#create-the-resource). Asegúrese de que el plan de tarifa está establecido en **S0**. Si usa una de las voces neuronales, asegúrese de crear el recurso en una [región compatible](regions.md#standard-and-neural-voices).
2. Inicie sesión en [Audio Content Creation](https://aka.ms/audiocontentcreation).
3. Seleccione un proyecto existente o haga clic en **Crear nuevo**.
4. Puede modificar su suscripción en cualquier momento con la opción **Configuración**, que se encuentra en la navegación superior.

## <a name="create-an-audio-tuning-file"></a>Creación de un archivo de ajuste de audio

Hay dos maneras de obtener el contenido de la herramienta Audio Content Creation.

**Opción 1:**

1. Después de iniciar sesión en [Audio Content Creation](https://aka.ms/audiocontentcreation), haga clic en **Audio Tuning** (ajuste de audio) para crear un nuevo archivo de ajuste de audio.
2. Cuando aparece la ventana de edición, puede introducir hasta 10 000 caracteres.
3. No olvide guardar.

**Opción 2:**

1. Después de iniciar sesión en [Audio Content Creation](https://aka.ms/audiocontentcreation), haga clic en **Cargar** para importar uno o varios archivos de texto. Se admiten el texto sin formato y SSML.
2. Al cargar los archivos de texto, asegúrese de que el contenido cumple estos requisitos.

   | Propiedad | Valor/notas |
   |----------|---------------|
   | Formato de archivo | Texto sin formato (.txt)<br/> Texto en SSML (.txt)<br/> No se admiten los archivos ZIP. |
   | Formato de codificación | UTF-8 |
   | Nombre de archivo | Cada archivo debe tener un nombre único. No se admiten los duplicados. |
   | Longitud del texto | Los archivos de texto no deben tener más de 10 000 caracteres. |
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

1. En la herramienta [Audio Content Creation](https://aka.ms/audiocontentcreation), haga clic en **Exportar** para crear una tarea de creación de audio.
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
