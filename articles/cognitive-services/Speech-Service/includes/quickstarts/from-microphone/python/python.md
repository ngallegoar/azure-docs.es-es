---
author: trevorbye
ms.service: cognitive-services
ms.topic: include
ms.date: 04/03/2020
ms.author: trbye
ms.openlocfilehash: 3613d190ef079d0e477d42b426a224d8e4dda7e6
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/16/2020
ms.locfileid: "81400683"
---
## <a name="prerequisites"></a>Prerrequisitos

Antes de comenzar:

> [!div class="checklist"]
> * <a href="https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesSpeechServices" target="_blank">Creación de un recurso de voz de Azure <span class="docon docon-navigate-external x-hidden-focus"></span></a>
> * [Configuración del entorno de desarrollo y creación de un proyecto vacío](../../../../quickstarts/setup-platform.md?pivots=programming-language-python)
> * Asegúrese de que tiene acceso a un micrófono para capturar el audio.

## <a name="source-code"></a>Código fuente

Cree un archivo llamado *quickstart.py* y pegue en él el siguiente código de Python.

[!code-python[Quickstart Code](~/samples-cognitive-services-speech-sdk/quickstart/python/from-microphone/quickstart.py#code)]

[!INCLUDE [replace key and region](../replace-key-and-region.md)]

## <a name="code-explanation"></a>Explicación del código

[!INCLUDE [code explanation](../code-explanation.md)]

## <a name="build-and-run-app"></a>Compilación y ejecución de una aplicación

Ya está listo para probar la aplicación y comprobar la funcionalidad de reconocimiento de voz mediante el servicio de voz.

1. **Inicie la aplicación**: en la línea de comandos, escriba:
    ```bash
    python quickstart.py
    ```
2. **Inicie el reconocimiento**: se le pedirá que diga una frase en inglés. La voz se envía al servicio Voz, se transcribe como texto y se representa en la consola.

## <a name="next-steps"></a>Pasos siguientes

[!INCLUDE [Speech recognition basics](../../speech-to-text-next-steps.md)]

