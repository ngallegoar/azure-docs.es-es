---
author: trevorbye
ms.service: cognitive-services
ms.topic: include
ms.date: 04/03/2020
ms.author: trbye
ms.openlocfilehash: 57c66cb3597d8811be3caef74fa36af193b93be7
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/16/2020
ms.locfileid: "81400723"
---
## <a name="prerequisites"></a>Prerrequisitos

Antes de comenzar:

> [!div class="checklist"]
> * <a href="https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesSpeechServices" target="_blank">Creación de un recurso de voz de Azure <span class="docon docon-navigate-external x-hidden-focus"></span></a>
> * [Configuración del entorno de desarrollo y creación de un proyecto vacío](../../../../quickstarts/setup-platform.md?tabs=linux&pivots=programming-language-cpp)
> * Asegúrese de que tiene acceso a un micrófono para capturar el audio.

## <a name="source-code"></a>Código fuente

Cree un archivo de código fuente de C++ denominado *helloworld.cpp* y pegue el siguiente código en él.

[!code-cpp[Quickstart Code](~/samples-cognitive-services-speech-sdk/quickstart/cpp/linux/from-microphone/helloworld.cpp#code)]

[!INCLUDE [replace key and region](../replace-key-and-region.md)]

## <a name="code-explanation"></a>Explicación del código

[!INCLUDE [code explanation](../code-explanation.md)]

## <a name="build-the-app"></a>Compilación de la aplicación

> [!NOTE]
> Asegúrese de introducir los siguientes comandos como una _única línea de comandos_. La forma más fácil de hacerlo es copiar el comando usando el botón **Copiar** junto a cada comando, y luego pegarlo en el símbolo del shell.

* En un sistema **x64** (64 bits), ejecute el siguiente comando para crear la aplicación.

  ```sh
  g++ helloworld.cpp -o helloworld -I "$SPEECHSDK_ROOT/include/cxx_api" -I "$SPEECHSDK_ROOT/include/c_api" --std=c++14 -lpthread -lMicrosoft.CognitiveServices.Speech.core -L "$SPEECHSDK_ROOT/lib/x64" -l:libasound.so.2
  ```

* En un sistema **x86** (32 bits), ejecute el siguiente comando para crear la aplicación.

  ```sh
  g++ helloworld.cpp -o helloworld -I "$SPEECHSDK_ROOT/include/cxx_api" -I "$SPEECHSDK_ROOT/include/c_api" --std=c++14 -lpthread -lMicrosoft.CognitiveServices.Speech.core -L "$SPEECHSDK_ROOT/lib/x86" -l:libasound.so.2
  ```

* En un sistema **ARM64** (64 bits), ejecute el siguiente comando para crear la aplicación.

  ```sh
  g++ helloworld.cpp -o helloworld -I "$SPEECHSDK_ROOT/include/cxx_api" -I "$SPEECHSDK_ROOT/include/c_api" --std=c++14 -lpthread -lMicrosoft.CognitiveServices.Speech.core -L "$SPEECHSDK_ROOT/lib/arm64" -l:libasound.so.2
  ```

## <a name="run-the-app"></a>Ejecución la aplicación

1. Configuración de la ruta de acceso de la biblioteca del cargador para que apunte a la biblioteca del SDK de Voz.

   * En un sistema **x64** (64 bits), escriba el siguiente comando.

     ```sh
     export LD_LIBRARY_PATH="$LD_LIBRARY_PATH:$SPEECHSDK_ROOT/lib/x64"
     ```

   * En un sistema **x86** (32 bits), escriba el siguiente comando.

     ```sh
     export LD_LIBRARY_PATH="$LD_LIBRARY_PATH:$SPEECHSDK_ROOT/lib/x86"
     ```

   * En un sistema **ARM64** (64 bits), escriba el siguiente comando.

     ```sh
     export LD_LIBRARY_PATH="$LD_LIBRARY_PATH:$SPEECHSDK_ROOT/lib/arm64"
     ```

1. Ejecute la aplicación.

   ```sh
   ./helloworld
   ```

1. En la ventana de consola, aparece un símbolo del sistema que solicita que se diga algo. Diga una oración o frase en inglés. Lo que diga se transmitirá al servicio de Voz y se transcribirá en texto, que aparece en la misma ventana.

   ```text
   Say something...
   We recognized: What's the weather like?
   ```

## <a name="next-steps"></a>Pasos siguientes

[!INCLUDE [Speech recognition basics](../../speech-to-text-next-steps.md)]

