---
title: 'Inicio rápido: Reconocimiento de la voz a través de un micrófono en Objective-C: servicio de voz'
titleSuffix: Azure Cognitive Services
description: Aprenda a reconocer la voz en Objective-C para iOS mediante el SDK de Voz.
services: cognitive-services
author: chlandsi
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: quickstart
ms.date: 12/23/2019
ms.author: chlandsi
ms.openlocfilehash: c1246b19670a18f8dadc0c5e1c64dd5af4c1b210
ms.sourcegitcommit: 9ee0cbaf3a67f9c7442b79f5ae2e97a4dfc8227b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "75380786"
---
# <a name="quickstart-recognize-speech-in-objective-c-on-ios-by-using-the-speech-sdk"></a>Inicio rápido: Reconocimiento de voz en Objective-C para iOS mediante el SDK de Voz

También hay inicios rápidos disponibles para la [síntesis de voz](~/articles/cognitive-services/Speech-Service/quickstarts/text-to-speech-langs/objectivec-ios.md).

En este artículo aprenderá a crear una aplicación iOS en Objective-C mediante el SDK de Voz de Azure Cognitive Services para transcribir voz a texto desde un micrófono o desde un archivo con audio grabado.

## <a name="prerequisites"></a>Prerrequisitos

Antes de comenzar, necesitará lo siguiente:

* Una [clave de suscripción](~/articles/cognitive-services/Speech-Service/get-started.md) del servicio de voz.
* Una máquina macOS con [Xcode 9.4.1](https://geo.itunes.apple.com/us/app/xcode/id497799835?mt=12) o una versión posterior.
* El destino establecido en la versión de iOS 9.3 o posterior.

## <a name="get-the-speech-sdk-for-ios"></a>Obtención del SDK de Voz para iOS

[!INCLUDE [License notice](~/includes/cognitive-services-speech-service-license-notice.md)]

En la actualidad, el SDK de Voz de Cognitive Services para iOS se distribuye como marco de Cocoa.
Se puede descargar de [este sitio web](https://aka.ms/csspeech/iosbinary). Descargue el archivo en el directorio principal.

## <a name="create-an-xcode-project"></a>Creación de un proyecto de Xcode

Inicie Xcode y seleccione **Archivo** > **Nuevo** > **Proyecto** para iniciar un proyecto nuevo.
En el cuadro de diálogo de selección de plantilla, seleccione la plantilla **iOS Single View App** (Aplicación de vista única [iOS]).

En los cuadros de diálogo que aparecen después, realice las selecciones siguientes.

1. En **Opciones del proyecto**:
    1. Escriba un nombre para la aplicación de inicio rápido, por ejemplo, *helloworld*.
    1. Escriba un nombre e identificador de la organización adecuados, si ya tiene una cuenta de desarrollador de Apple. Para fines de prueba, use un nombre como *testorg*. Para firmar la aplicación, necesita un perfil de aprovisionamiento adecuado. Para más información, consulte el [centro para desarrolladores de Apple](https://developer.apple.com/).
    1. Asegúrese de seleccionar **Objective-C** como lenguaje para el proyecto.
    1. Desactive todas las casillas de pruebas y datos principales.

    ![Configuración del proyecto](~/articles/cognitive-services/Speech-Service/media/sdk/qs-objectivec-project-settings.png)

1. Seleccione un directorio de proyecto:
   1. Elija el directorio principal para incluir el proyecto. En este paso se crea un directorio helloworld en el directorio principal que contiene todos los archivos del proyecto de Xcode.
   1. Deshabilite la creación de un repositorio de Git para este proyecto de ejemplo.
   1. Ajuste las rutas de acceso para el SDK en la pantalla de configuración del proyecto.
      1. En la pestaña **General** (General) del encabezado **Embedded Binaries** (Binarios insertados), agregue la biblioteca del SDK como marco: **Add embedded binaries** > **Add other** (Agregar binarios insertados > Agregar otro). Vaya a su directorio particular y seleccione el archivo `MicrosoftCognitiveServicesSpeech.framework`. Como resultado, se agrega automáticamente la biblioteca del SDK al encabezado **Linked Framework and Libraries** (Marcos y bibliotecas vinculados).
         ![Marco agregado](~/articles/cognitive-services/Speech-Service/media/sdk/qs-objectivec-framework.png)
      1. Vaya a la pestaña **Configuración de compilación** y seleccione la configuración **Todos**.
      1. Agregue el directorio $(SRCROOT)/.. a **Framework Search Paths** (Rutas de búsqueda de marco) bajo el encabezado **Rutas de búsqueda**.

      ![Configuración Framework Search Paths (Rutas de búsqueda de marco)](~/articles/cognitive-services/Speech-Service/media/sdk/qs-objectivec-framework-search-paths.png)

## <a name="set-up-the-ui"></a>Configuración de la interfaz de usuario

La aplicación de ejemplo tiene una interfaz de usuario muy simple, dos botones para iniciar el reconocimiento de voz desde un archivo o desde la entrada del micrófono y una etiqueta de texto para mostrar el resultado. La interfaz de usuario se configura en la parte `Main.storyboard` del proyecto. Para abrir la vista XML del guion gráfico, haga clic con el botón derecho en la entrada `Main.storyboard` del árbol del proyecto y seleccione **Abrir como** > **Código fuente**.

Reemplace el código XML generado automáticamente por este código:

[!code-xml[](~/samples-cognitive-services-speech-sdk/quickstart/objectivec/ios/from-microphone/helloworld/helloworld/Base.lproj/Main.storyboard)]

## <a name="add-the-sample-code"></a>Incorporación del código de ejemplo

1. Para descargar el [archivo wav de ejemplo](https://raw.githubusercontent.com/Azure-Samples/cognitive-services-speech-sdk/f9807b1079f3a85f07cbb6d762c6b5449d536027/samples/cpp/windows/console/samples/whatstheweatherlike.wav) haga clic con el botón derecho en el vínculo y seleccione **Guardar destino como**.
   Agregue el archivo wav al proyecto como un recurso arrastrándolo desde una ventana de Finder al nivel raíz de la vista del proyecto.
   Seleccione **Finalizar** en el siguiente cuadro de diálogo sin cambiar la configuración.
1. Reemplace el contenido del archivo `ViewController.m` generado automáticamente por el código siguiente:

   [!code-objectivec[Quickstart code](~/samples-cognitive-services-speech-sdk/quickstart/objectivec/ios/from-microphone/helloworld/helloworld/ViewController.m#code)]
1. Reemplace la cadena `YourSubscriptionKey` por la clave de suscripción.
1. Reemplace la cadena `YourServiceRegion` por la [región](~/articles/cognitive-services/Speech-Service/regions.md) asociada a su suscripción. Por ejemplo, use `westus` para la suscripción de prueba gratuita.
1. Agregue la solicitud de acceso de micrófono. Haga clic con el botón derecho en la entrada `Info.plist` del árbol del proyecto y seleccione **Abrir como** > **Código fuente**. Agregue las líneas siguientes en la sección `<dict>` y guarde el archivo.

    ```xml
    <key>NSMicrophoneUsageDescription</key>
    <string>Need microphone access for speech recognition from microphone.</string>
    ```

## <a name="build-and-run-the-sample"></a>Compilación y ejecución del ejemplo

1. Haga visible la salida de depuración; para ello, seleccione **View** > **Debug Area** > **Activate Console** (Ver > Área de depuración > Activar consola).
1. Elija el simulador de iOS o un dispositivo iOS conectado a la máquina de desarrollo como destino para la aplicación en la lista del menú **Producto** > **Destino**.
1. Compile y ejecute el código de ejemplo en el simulador de iOS; para ello, seleccione **Producto** > **Ejecutar** en el menú. También puede seleccionar el botón **Reproducir**.
1. Tras seleccionar el botón **Recognize (File)** (Reconocer [Archivo]) de la aplicación, debería ver el contenido del archivo de audio "What's the weather like?" (¿Qué tiempo hace?) en la parte inferior de la pantalla.

   ![Aplicación de iOS simulada](~/articles/cognitive-services/Speech-Service/media/sdk/qs-objectivec-simulated-app.png)

1. Después de seleccionar el botón **Recognize (Microphone)** (Reconocer [Micrófono]) de la aplicación y de decir algunas palabras, debería ver el texto que ha dicho en la parte inferior de la pantalla.

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Exploración de ejemplos de Objective-C en GitHub](https://aka.ms/csspeech/samples)
