---
author: trevorbye
ms.service: cognitive-services
ms.topic: include
ms.date: 03/09/2020
ms.author: trbye
ms.openlocfilehash: f0baac79bfbfc06b24d692caef87a20013c4654c
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/29/2020
ms.locfileid: "81421713"
---
Para hacer streaming en un formato de audio comprimido al servicio de voz, cree un elemento `SPXPullAudioInputStream` o `SPXPushAudioInputStream`.

En el siguiente fragmento de código se muestra cómo crear un elemento `SPXAudioConfiguration` a partir de una instancia de `SPXPushAudioInputStream`, especificando MP3 como formato de compresión de la transmisión.

[!code-objectivec[Set up the input stream](~/samples-cognitive-services-speech-sdk/samples/objective-c/ios/compressed-streams/CompressedStreamsSample/CompressedStreamsSample/ViewController.m?range=67-76&highlight=2-11)]

El siguiente fragmento de código muestra cómo se pueden leer los datos de audio comprimidos de un archivo y bombearlos en `SPXPushAudioInputStream`.

[!code-objectivec[Push compressed audio data into the stream](~/samples-cognitive-services-speech-sdk/samples/objective-c/ios/compressed-streams/CompressedStreamsSample/CompressedStreamsSample/ViewController.m?range=106-150&highlight=19-44)]
