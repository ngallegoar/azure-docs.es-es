---
author: IEvangelist
ms.service: cognitive-services
ms.topic: include
ms.date: 03/09/2020
ms.author: dapine
ms.openlocfilehash: 788903d802ca47c763734e7cf6ddbbf3b0032203
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "78943847"
---
Para hacer streaming en un formato de audio comprimido al servicio de voz, cree un elemento `SPXPullAudioInputStream` o `SPXPushAudioInputStream`.

En el siguiente fragmento de código se muestra cómo crear un elemento `SPXAudioConfiguration` a partir de una instancia de `SPXPushAudioInputStream`, especificando MP3 como formato de compresión de la transmisión.

[!code-objectivec[Set up the input stream](~/samples-cognitive-services-speech-sdk/samples/objective-c/ios/compressed-streams/CompressedStreamsSample/CompressedStreamsSample/ViewController.m?range=67-76&highlight=2-11)]

El siguiente fragmento de código muestra cómo se pueden leer los datos de audio comprimidos de un archivo y bombearlos en `SPXPushAudioInputStream`.

[!code-objectivec[Push compressed audio data into the stream](~/samples-cognitive-services-speech-sdk/samples/objective-c/ios/compressed-streams/CompressedStreamsSample/CompressedStreamsSample/ViewController.m?range=106-150&highlight=19-44)]
