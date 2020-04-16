---
author: trevorbye
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: include
ms.date: 01/31/2020
ms.author: trbye
ms.openlocfilehash: 005cf83508d25e8f44190e07336fbb4e444f8e6b
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/16/2020
ms.locfileid: "81400369"
---
En este inicio rápido, usará el [SDK de Voz](~/articles/cognitive-services/speech-service/speech-sdk.md) para convertir texto en voz sintetizada. El servicio de conversión de texto a voz proporciona numerosas opciones para voces sintetizadas que puede encontrar en [compatibilidad con el lenguaje de conversión de texto a voz](../../../language-support.md#text-to-speech). Una vez que se cumplen los requisitos previos, para la representación de voz sintetizada en los altavoces predeterminados solo son necesarios cuatro pasos:
> [!div class="checklist"]
> * Cree un objeto `SpeechConfig` a partir de la clave y la región de suscripción.
> * Cree un objeto `SpeechSynthesizer` con el objeto `SpeechConfig` anterior.
> * Use el objeto `SpeechSynthesizer` para pronunciar el texto.
> * Compruebe el objeto `SpeechSynthesisResult` devuelto en caso de errores.
