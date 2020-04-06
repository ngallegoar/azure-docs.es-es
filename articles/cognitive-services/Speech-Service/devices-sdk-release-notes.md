---
title: Documentación de Speech Devices SDK
titleSuffix: Azure Cognitive Services
description: Las notas de la versión proporcionan un registro de actualizaciones, mejoras, correcciones de errores y cambios en el SDK de dispositivos de voz. Este artículo se actualiza con cada versión del SDK de los dispositivos de voz.
services: cognitive-services
author: wsturman
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 02/12/2020
ms.author: wellsi
ms.openlocfilehash: 29dcb3c0e74482fd6670d1a0983e751043379d6e
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "80371616"
---
# <a name="release-notes-speech-devices-sdk"></a>Notas de la versión: SDK de dispositivos de voz

En las siguientes secciones se indican los cambios en las versiones más recientes.

## <a name="speech-devices-sdk-190"></a>SDK de dispositivos de voz 1.9.0:

- Se proporcionan archivos binarios iniciales para [Urbetter DDK](https://aka.ms/sdsdk-download-urbetter) (Linux ARM64).
- Ahora, Roobo v1 utiliza Maven para Speech SDK
- Se ha actualizado el componente [SDK de Voz](https://docs.microsoft.com/azure/cognitive-services/speech-service/speech-sdk-reference) a la versión 1.9.0. Para más información, consulte sus [notas de la versión](https://aka.ms/csspeech/whatsnew).

## <a name="speech-devices-sdk-170"></a>SDK de dispositivos de voz 1.7.0:

- Ahora se admite Linux ARM.
- Se proporcionan archivos binarios iniciales para [Roobo v2 DDK](https://aka.ms/sdsdk-download-roobov2) (Linux ARM64).
- Los usuarios de Windows pueden usar `AudioConfig.fromDefaultMicrophoneInput()` o `AudioConfig.fromMicrophoneInput(deviceName)` para especificar el micrófono que se utilizará.
- Se ha optimizado el tamaño de la biblioteca.
- Compatibilidad con el reconocimiento de múltiples turnos mediante el mismo objeto reconocedor de voz/intención.
- Se ha corregido el bloqueo ocasional que se produciría al detener el reconocimiento.
- Las aplicaciones de ejemplo ahora contienen un archivo participants.properties de ejemplo para demostrar el formato del archivo.
- Se ha actualizado el componente [SDK de Voz](https://docs.microsoft.com/azure/cognitive-services/speech-service/speech-sdk-reference) a la versión 1.7.0. Para más información, consulte sus [notas de la versión](https://aka.ms/csspeech/whatsnew).

## <a name="speech-devices-sdk-160"></a>SDK de dispositivos de voz 1.6.0:

- Compatibilidad con [Azure Kinect DK](https://azure.microsoft.com/services/kinect-dk/) en Windows y Linux con una [aplicación de ejemplo](https://aka.ms/sdsdk-download) común
- Se ha actualizado el componente [Speech SDK](https://docs.microsoft.com/azure/cognitive-services/speech-service/speech-sdk-reference) a la versión 1.6.0. Para más información, consulte sus [notas de la versión](https://aka.ms/csspeech/whatsnew).

## <a name="speech-devices-sdk-151"></a>SDK de dispositivos de voz 1.5.1:

- Incluya [transcripción de conversaciones](conversation-transcription-service.md) en la aplicación de ejemplo.
- Se ha actualizado el componente [Speech SDK](https://docs.microsoft.com/azure/cognitive-services/speech-service/speech-sdk-reference) a la versión 1.5.1. Para más información, consulte sus [notas de la versión](https://aka.ms/csspeech/whatsnew).

## <a name="speech-devices-sdk-150-2019-may-release"></a>SDK de dispositivos de voz 1.5.0: Versión de mayo de 2019

- SDK de dispositivos de voz es ahora disponibilidad general y ya no está en versión preliminar controlada.
- Se ha actualizado el componente [Speech SDK](https://docs.microsoft.com/azure/cognitive-services/speech-service/speech-sdk-reference) a la versión 1.5.0. Para más información, consulte sus [notas de la versión](https://aka.ms/csspeech/whatsnew).
- La nueva tecnología de palabra clave proporciona importantes mejoras de calidad; consulte Cambios importantes.
- Nueva canalización de procesamiento de audio para el reconocimiento mejorado de campo lejano.

**Cambios importantes**

- debido a la nueva tecnología de palabra clave, todas las palabras clave deben volver a crearse en nuestro portal mejorado de palabras clave. Para quitar completamente las palabras clave antiguas del dispositivo, desinstale la aplicación antigua.
  - adb uninstall com.microsoft.cognitiveservices.speech.samples.sdsdkstarterapp

## <a name="speech-devices-sdk-140-2019-apr-release"></a>SDK de dispositivos de voz 1.4.0: Versión de abril de 2019

- Se ha actualizado el componente [Speech SDK](https://docs.microsoft.com/azure/cognitive-services/speech-service/speech-sdk-reference) a la versión 1.4.0. Para más información, consulte sus [notas de la versión](https://aka.ms/csspeech/whatsnew).

## <a name="speech-devices-sdk-131-2019-mar-release"></a>SDK de dispositivos de voz 1.3.1: Versión de marzo de 2019

- Se ha actualizado el componente [Speech SDK](https://docs.microsoft.com/azure/cognitive-services/speech-service/speech-sdk-reference) a la versión 1.3.1. Para más información, consulte sus [notas de la versión](https://aka.ms/csspeech/whatsnew).
- Administración de palabras clave; consulte Cambios importantes.
- La aplicación de ejemplo agrega la opción de idioma para el reconocimiento y la traducción de voz.

**Cambios importantes**

- La [instalación de una palabra clave](https://docs.microsoft.com/azure/cognitive-services/speech-service/speech-devices-sdk-create-kws) se ha simplificado, y ahora es parte de la aplicación (no necesita una instalación independiente en el dispositivo).
- El reconocimiento de la palabra clave ha cambiado, y se admiten dos eventos.
  - `RecognizingKeyword,` indica que el resultado de voz contiene texto de palabras clave (sin verificar).
  - `RecognizedKeyword` indica que el reconocimiento de la palabra clave terminó de reconocer la palabra clave especificada.

## <a name="speech-devices-sdk-110-2018-nov-release"></a>SDK de dispositivos de voz 1.1.0: Versión de noviembre de 2018

- Se ha actualizado el componente [Speech SDK](https://docs.microsoft.com/azure/cognitive-services/speech-service/speech-sdk-reference) a la versión 1.1.0. Para más información, consulte sus [notas de la versión](https://aka.ms/csspeech/whatsnew).
- Ha aumentado la precisión del reconocimiento de voz a gran distancia gracias a nuestro algoritmo mejorado de procesamiento de audio.
- La aplicación de ejemplo ahora es compatible con el reconocimiento de voz en chino.

## <a name="speech-devices-sdk-101-2018-oct-release"></a>SDK de dispositivos de voz 1.0.1: versión de octubre de 2018

- Se ha actualizado el componente [Speech SDK](https://docs.microsoft.com/azure/cognitive-services/speech-service/speech-sdk-reference) a la versión 1.0.1. Para más información, consulte sus [notas de la versión](https://aka.ms/csspeech/whatsnew).
- La precisión del reconocimiento de voz se verá mejorada gracias a nuestro algoritmo mejorado de procesamiento de audio.
- Se ha corregido un error en la sesión de audio de reconocimiento continuo.

**Cambios importantes**

- Con esta versión se presentan una serie de cambios importantes. Consulte [esta página](https://aka.ms/csspeech/breakingchanges_1_0_0) para obtener detalles relacionados con las API.
- Los archivos de modelo de KWS no son compatibles con Speech Devices SDK 1.0.1. Los archivos existentes de palabras clave se eliminarán después de que los nuevos se escriban en el dispositivo.

## <a name="speech-devices-sdk-050-2018-aug-release"></a>SDK de dispositivos de voz 0.5.0: versión de agosto de 2018

- Se ha mejorado la precisión del reconocimiento de voz mediante la corrección de un error en el código de procesamiento de audio.
- Se ha actualizado el componente [Speech SDK](https://docs.microsoft.com/azure/cognitive-services/speech-service/speech-sdk-reference) a la versión 0.5.0. Para más información, consulte sus [notas de la versión](releasenotes.md#cognitive-services-speech-sdk-050-2018-july-release).

## <a name="speech-devices-sdk-0212733-2018-may-release"></a>SDK de dispositivos de voz 0.2.12733: Versión de mayo de 2018

La primera versión preliminar pública de Speech Devices SDK de Cognitive Services.
