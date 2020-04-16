---
author: trevorbye
ms.service: cognitive-services
ms.topic: include
ms.date: 04/02/2020
ms.author: trbye
ms.openlocfilehash: 36243fd5db76a4706318f41b3e2cb3f557c17189
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/16/2020
ms.locfileid: "81400760"
---
En este inicio rápido usará el [SDK de voz](~/articles/cognitive-services/speech-service/speech-sdk.md) para reconocer de forma interactiva la voz de la entrada de micrófono y obtener la transcripción del texto del audio capturado. Es fácil integrar esta característica en las aplicaciones o dispositivos para tareas de reconocimiento comunes, como transcribir conversaciones. También se puede usar para integraciones más complejas, como el uso de Bot Framework con el SDK de Voz para crear asistentes de voz.

Una vez que se cumplen los requisitos previos, para realizar el reconocimiento de voz a través de un micrófono solo son necesarios cuatro pasos:

> [!div class="checklist"]
> * Cree un objeto `SpeechConfig` a partir de la clave y la región de suscripción.
> * Cree un objeto `SpeechRecognizer` con el objeto `SpeechConfig` anterior.
> * Con el objeto `SpeechRecognizer`, inicie el proceso de reconocimiento de una única expresión.
> * Inspeccione el objeto `SpeechRecognitionResult` devuelto.
