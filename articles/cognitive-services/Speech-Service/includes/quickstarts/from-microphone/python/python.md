---
author: trevorbye
ms.service: cognitive-services
ms.topic: include
ms.date: 04/03/2020
ms.author: trbye
ms.openlocfilehash: f26f0ab6da398dcdee307f89b27cca780d08af85
ms.sourcegitcommit: 374d1533ea2f2d9d3f8b6e6a8e65c6a5cd4aea47
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/01/2020
ms.locfileid: "85838805"
---
## <a name="prerequisites"></a>Requisitos previos

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

Ya está listo para probar el reconocimiento de voz con el Servicio de voz. 

Si ejecuta este proceso en macOS y es la primera aplicación Python que ha compilado y que usa un micrófono, es probable que tenga que proporcionar al terminal acceso al micrófono. Abra **Ajustes del sistema** y seleccione **Seguridad y privacidad**. A continuación, seleccione **Privacidad** y busque **Micrófono** en la lista. Por último, seleccione **Terminal** y guarde. 

1. **Inicie la aplicación**: en la línea de comandos, escriba:
    ```bash
    python quickstart.py
    ```
2. **Inicie el reconocimiento**: se le pedirá que diga una frase en inglés. La voz se envía al servicio Voz, se transcribe como texto y se representa en la consola.

## <a name="next-steps"></a>Pasos siguientes

[!INCLUDE [Speech recognition basics](../../speech-to-text-next-steps.md)]
