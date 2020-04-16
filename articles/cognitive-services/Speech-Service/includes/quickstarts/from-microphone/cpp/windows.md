---
author: trevorbye
ms.service: cognitive-services
ms.topic: include
ms.date: 04/03/2020
ms.author: trbye
ms.openlocfilehash: db87f31f4702d3487a11a6d833755105455d74c8
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/16/2020
ms.locfileid: "81400799"
---
## <a name="prerequisites"></a>Prerrequisitos

Antes de comenzar:

> [!div class="checklist"]
> * <a href="https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesSpeechServices" target="_blank">Creación de un recurso de voz de Azure <span class="docon docon-navigate-external x-hidden-focus"></span></a>
> * [Configuración del entorno de desarrollo y creación de un proyecto vacío](../../../../quickstarts/setup-platform.md?tabs=windows&pivots=programming-language-cpp)
> * Asegúrese de que tiene acceso a un micrófono para capturar el audio.

## <a name="source-code"></a>Código fuente

Cree un archivo de código fuente de C++ denominado *helloworld.cpp* y pegue el siguiente código en él.

[!code-cpp[Quickstart Code](~/samples-cognitive-services-speech-sdk/quickstart/cpp/windows/from-microphone/helloworld/helloworld.cpp#code)]

[!INCLUDE [replace key and region](../replace-key-and-region.md)]

## <a name="code-explanation"></a>Explicación del código

[!INCLUDE [code explanation](../code-explanation.md)]

## <a name="build-and-run-app"></a>Compilación y ejecución de una aplicación

1. En la barra de menús, seleccione **Compilar** > **Compilar solución** para compilar la aplicación. El código se debería compilar sin errores ahora.

1. Elija **Depurar** > **Iniciar depuración** o presione <kbd>F5</kbd> para iniciar la aplicación **HelloWorld**.

1. Diga una oración o frase en inglés. La aplicación transmite la voz al servicio de voz, que la transcribe en texto y lo envía de vuelta a la aplicación para mostrarlo.

   ![Salida de la consola después de un reconocimiento correcto](~/articles/cognitive-services/Speech-Service/media/sdk/qs-cpp-windows-08-console-output-release.png)

## <a name="next-steps"></a>Pasos siguientes

[!INCLUDE [Speech recognition basics](../../speech-to-text-next-steps.md)]