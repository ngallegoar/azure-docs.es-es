---
title: 'Inicio rápido: Reconocimiento de la voz a través de un micrófono en C++ (Linux): servicio de voz'
titleSuffix: Azure Cognitive Services
services: cognitive-services
author: wolfma61
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: include
ms.date: 12/17/2019
ms.author: wolfma
ms.openlocfilehash: 1ba1f2a61a293880e81dde0ce1f93460a555fa62
ms.sourcegitcommit: 9ee0cbaf3a67f9c7442b79f5ae2e97a4dfc8227b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "78925646"
---
## <a name="prerequisites"></a>Prerrequisitos

Antes de comenzar:

> [!div class="checklist"]
> * [Ha creado un recurso de Voz de Azure](../../../../get-started.md)
> * [Configuración del entorno de desarrollo y creación de un proyecto vacío](../../../../quickstarts/setup-platform.md?tabs=linux)
> * Asegúrese de que tiene acceso a un micrófono para capturar el audio.

## <a name="add-sample-code"></a>Incorporación de código de ejemplo

1. Cree un archivo de código fuente C++ denominado `helloworld.cpp` y pegue el siguiente código en él.

   [!code-cpp[Quickstart Code](~/samples-cognitive-services-speech-sdk/quickstart/cpp/linux/from-microphone/helloworld.cpp#code)]

1. En este nuevo archivo, reemplace la cadena `YourSubscriptionKey` por su clave de suscripción del servicio Voz.

1. Reemplace la cadena `YourServiceRegion` por el **identificador de región** de la [región](https://aka.ms/speech/sdkregion) asociada a su suscripción (por ejemplo, `westus` para la suscripción de evaluación gratuita).

> [!NOTE]
> El SDK de Voz se usará de forma predeterminada para reconocer el uso de en-us como idioma. Para más información sobre cómo elegir el idioma de origen, consulte [Especificación del idioma de origen para la conversión de voz a texto](../../../../how-to-specify-source-language.md).

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

[!INCLUDE [footer](./footer.md)]
