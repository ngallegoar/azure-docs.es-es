---
author: trevorbye
ms.service: cognitive-services
ms.topic: include
ms.date: 04/02/2020
ms.author: trbye
ms.openlocfilehash: c2b844b3b1aa7a848e2668b3dca5a1cac5b5baaf
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/16/2020
ms.locfileid: "81400698"
---
## <a name="prerequisites"></a>Prerrequisitos

Antes de comenzar:

> [!div class="checklist"]
> * <a href="https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesSpeechServices" target="_blank">Creación de un recurso de voz de Azure <span class="docon docon-navigate-external x-hidden-focus"></span></a>
> * [Configuración del entorno de desarrollo y creación de un proyecto vacío](../../../../quickstarts/setup-platform.md?tabs=jre&pivots=programming-language-java)
> * Asegúrese de que tiene acceso a un micrófono para capturar el audio.

## <a name="source-code"></a>Código fuente

Para agregar una nueva clase vacía al proyecto de Java, seleccione **File (Archivo)**  > **New (Nuevo)**  > **Class (Clase)** . En la ventana **New Java Class** (Nueva clase de Java) escriba **speechsdk.quickstart** en el campo **Package** (Paquete) y **Main** en el campo **Name** (Nombre).

![Captura de pantalla de la ventana New Java Class (Nueva clase de Java)](~/articles/cognitive-services/Speech-Service/media/sdk/qs-java-jre-06-create-main-java.png)

Reemplace el contenido del archivo *Main.java* por el siguiente fragmento de código:

[!code-java[Quickstart Code](~/samples-cognitive-services-speech-sdk/quickstart/java/jre/from-microphone/src/speechsdk/quickstart/Main.java#code)]

[!INCLUDE [replace key and region](../replace-key-and-region.md)]

## <a name="code-explanation"></a>Explicación del código

[!INCLUDE [code explanation](../code-explanation.md)]

## <a name="build-and-run-app"></a>Compilación y ejecución de una aplicación

Presione <kbd>F11</kbd>, o seleccione **Ejecutar** > **Depurar**.
Los próximos 15 segundos de la entrada de voz del micrófono se reconocen y se registran en la ventana de consola.

![Captura de pantalla de la salida de la consola después de un reconocimiento correcto](~/articles/cognitive-services/Speech-Service/media/sdk/qs-java-jre-07-console-output.png)

## <a name="next-steps"></a>Pasos siguientes

[!INCLUDE [Speech recognition basics](../../speech-to-text-next-steps.md)]

