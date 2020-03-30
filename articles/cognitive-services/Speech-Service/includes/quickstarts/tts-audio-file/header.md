---
author: IEvangelist
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: include
ms.date: 01/31/2020
ms.author: dapine
ms.openlocfilehash: cd8da5eb9313685361ca56b56c024c2dfb37276e
ms.sourcegitcommit: 9ee0cbaf3a67f9c7442b79f5ae2e97a4dfc8227b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "76961469"
---
En este inicio rápido, usará el SDK de [Voz](~/articles/cognitive-services/speech-service/speech-sdk.md) para convertir texto en voz sintetizada en un archivo de audio. El servicio de conversión de texto a voz proporciona numerosas opciones para voces sintetizadas que puede encontrar en [compatibilidad con el lenguaje de conversión de texto a voz](../../../language-support.md#text-to-speech). Una vez que se cumplen los requisitos previos, la síntesis de voz en un archivo solo necesita realizar estos cinco pasos:
> [!div class="checklist"]
> * Cree un objeto `SpeechConfig` a partir de la clave y la región de suscripción.
> * Cree un objeto de configuración de audio que especifique el nombre del archivo .WAV.
> * Cree un objeto `SpeechSynthesizer` con los objetos de configuración anteriores.
> * Con el objeto `SpeechSynthesizer`, convierta el texto en voz sintetizada y guárdelo en el archivo de audio especificado.
> * Inspeccione el objeto `SpeechSynthesizer` devuelto en caso de errores.
