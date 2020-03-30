---
author: IEvangelist
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: include
ms.date: 01/31/2020
ms.author: dapine
ms.openlocfilehash: e9f02f95693552180a0eed1550cc59d8f975416b
ms.sourcegitcommit: 9ee0cbaf3a67f9c7442b79f5ae2e97a4dfc8227b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "76961387"
---
En este inicio rápido, usará el [SDK de Voz](~/articles/cognitive-services/speech-service/speech-sdk.md) para convertir texto en voz sintetizada. El servicio de conversión de texto a voz proporciona numerosas opciones para voces sintetizadas que puede encontrar en [compatibilidad con el lenguaje de conversión de texto a voz](../../../language-support.md#text-to-speech). Una vez que se cumplen los requisitos previos, para la representación de voz sintetizada en los altavoces predeterminados solo son necesarios cuatro pasos:
> [!div class="checklist"]
> * Cree un objeto `SpeechConfig` a partir de la clave y la región de suscripción.
> * Cree un objeto `SpeechSynthesizer` con el objeto `SpeechConfig` anterior.
> * Use el objeto `SpeechSynthesizer` para pronunciar el texto.
> * Compruebe el objeto `SpeechSynthesisResult` devuelto en caso de errores.
