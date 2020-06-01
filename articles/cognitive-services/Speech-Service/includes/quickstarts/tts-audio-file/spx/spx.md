---
author: v-demjoh
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: include
ms.date: 05/18/2020
ms.author: v-demjoh
ms.openlocfilehash: bd48618a520f547c72bfba7ff04ae6a249d0a673
ms.sourcegitcommit: cf7caaf1e42f1420e1491e3616cc989d504f0902
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/22/2020
ms.locfileid: "83806464"
---
## <a name="run-the-speech-cli"></a>Ejecución de la CLI de Voz

Ahora está listo para ejecutar la CLI de Voz y sintetizar la voz de texto en un nuevo archivo de audio.

Desde la línea de comandos, cambie al directorio que contiene el archivo binario de la CLI de Voz y escriba:

```bash
spx synthesize --text "The speech synthesizer greets you!" --audio output greetings.wav
```

La CLI de Voz generará lenguaje natural en inglés en el archivo de audio `greetings.wav`.
En Windows, puede reproducir el archivo de audio escribiendo `start greetings.wav`.
