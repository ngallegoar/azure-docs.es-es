---
title: 'Inicio rápido: Reconocimiento de la voz a través de un micrófono en Swift: servicio de voz'
titleSuffix: Azure Cognitive Services
description: Aprenda a reconocer la voz en Swift para iOS mediante el SDK de Voz
services: cognitive-services
author: chlandsi
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: quickstart
ms.date: 06/25/2020
ms.author: chlandsi
ms.openlocfilehash: 95c69aad24ff10b49bdc09411553e6ed43bfdccd
ms.sourcegitcommit: b56226271541e1393a4b85d23c07fd495a4f644d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/26/2020
ms.locfileid: "85391441"
---
# <a name="quickstart-recognize-speech-in-swift-on-ios-by-using-the-speech-sdk"></a>Inicio rápido: Reconocimiento de voz en Swift para iOS mediante el SDK de Voz

También hay inicios rápidos disponibles para la [síntesis de voz](~/articles/cognitive-services/Speech-Service/quickstarts/text-to-speech-langs/swift-ios.md).

En este artículo aprenderá a crear una aplicación iOS en Swift mediante el SDK de Voz de Azure Cognitive Services para transcribir a texto la voz grabada con un micrófono.

## <a name="prerequisites"></a>Requisitos previos

Antes de comenzar, necesitará lo siguiente:

* Una [clave de suscripción](~/articles/cognitive-services/Speech-Service/get-started.md) del servicio de voz.
* Una máquina macOS con [Xcode 9.4.1](https://geo.itunes.apple.com/us/app/xcode/id497799835?mt=12) o una versión posterior y [CocoaPods](https://cocoapods.org/) instalado.

## <a name="get-the-speech-sdk-for-ios"></a>Obtención del SDK de Voz para iOS

[!INCLUDE [License notice](~/includes/cognitive-services-speech-service-license-notice.md)]

Este tutorial no funcionará con ninguna versión del SDK anterior a la 1.6.0.

El SDK de Voz de Cognitive Services para iOS se distribuye como un paquete de marcos. Se puede usar en proyectos de Xcode como administrador de dependencias de [CocoaPods](https://cocoapods.org/) o se puede descargar desde https://aka.ms/csspeech/iosbinary y vincular manualmente. En este artículo se usa CocoaPods.

## <a name="create-an-xcode-project"></a>Creación de un proyecto de Xcode

Inicie Xcode y seleccione **Archivo** > **Nuevo** > **Proyecto** para iniciar un proyecto nuevo.
En el cuadro de diálogo de selección de plantilla, seleccione la plantilla **iOS Single View App** (Aplicación de vista única [iOS]).

En los cuadros de diálogo que aparecen después, realice las selecciones siguientes.

1. En **Opciones del proyecto**:
    1. Escriba un nombre para la aplicación de inicio rápido, por ejemplo, *helloworld*.
    1. Si ya tiene una cuenta de desarrollador de Apple, escriba un nombre y un identificador de organización adecuados. Para fines de prueba, use un nombre como *testorg*. Para firmar la aplicación, necesita un perfil de aprovisionamiento adecuado. Para más información, consulte el [centro para desarrolladores de Apple](https://developer.apple.com/).
    1. Asegúrese de elegir **Swift** como lenguaje del proyecto.
    1. Desactive las casillas para usar guiones gráficos y para crear una aplicación basada en documentos. La interfaz de usuario sencilla para la aplicación de ejemplo se crea mediante programación.
    1. Desactive todas las casillas de pruebas y datos principales.
1. Seleccione un directorio de proyecto:
    1. Elija un directorio en el que colocar el proyecto. En este paso se crea un directorio helloworld en el directorio elegido que contiene todos los archivos del proyecto de Xcode.
    1. Deshabilite la creación de un repositorio de Git para este proyecto de ejemplo.
1. La aplicación también debe declarar el uso del micrófono en el archivo `Info.plist`. Seleccione el archivo en la información general y agregue la clave **Privacidad: descripción de uso del micrófono**, con un valor como *Microphone is needed for speech recognition* (Se necesita el micrófono para el reconocimiento de voz).

    ![Configuración de Info.plist](~/articles/cognitive-services/Speech-Service/media/sdk/qs-swift-ios-info-plist.png)

1. Cierre el proyecto de Xcode. Después de configurar CocoaPods usará una instancia suya diferente.

## <a name="add-the-sample-code"></a>Incorporación del código de ejemplo

1. Coloque un nuevo archivo de encabezado con el nombre `MicrosoftCognitiveServicesSpeech-Bridging-Header.h` en el directorio helloworld dentro del proyecto helloworld. Pegue el siguiente código en él:

   [!code-objectivec[Quickstart code](~/samples-cognitive-services-speech-sdk/quickstart/swift/ios/from-microphone/helloworld/helloworld/MicrosoftCognitiveServicesSpeech-Bridging-Header.h#code)]

1. Agregue la ruta de acceso relativa `helloworld/MicrosoftCognitiveServicesSpeech-Bridging-Header.h` del encabezado puente a la configuración del proyecto de Swift para el destino helloworld en el campo **Objective-C Bridging Header** (Encabezado puente Objective-C).

   ![Propiedades de encabezado](~/articles/cognitive-services/Speech-Service/media/sdk/qs-swift-ios-bridging-header.png)

1. Reemplace el contenido del archivo `AppDelegate.swift` generado automáticamente por el código siguiente:

   [!code-swift[Quickstart code](~/samples-cognitive-services-speech-sdk/quickstart/swift/ios/from-microphone/helloworld/helloworld/AppDelegate.swift#code)]
1. Reemplace el contenido del archivo `ViewController.swift` generado automáticamente por el código siguiente:

   [!code-swift[Quickstart code](~/samples-cognitive-services-speech-sdk/quickstart/swift/ios/from-microphone/helloworld/helloworld/ViewController.swift#code)]
1. En `ViewController.swift`, reemplace la cadena `YourSubscriptionKey` por su clave de suscripción.
1. Reemplace la cadena `YourServiceRegion` por la [región](~/articles/cognitive-services/Speech-Service/regions.md) asociada a su suscripción. Por ejemplo, use `westus` para la suscripción de prueba gratuita.

## <a name="install-the-sdk-as-a-cocoapod"></a>Instalación del SDK como CocoaPod

1. Instale el administrador de dependencias de CocoaPod como se describe en sus [instrucciones de instalación](https://guides.cocoapods.org/using/getting-started.html).
1. Vaya al directorio de la aplicación de ejemplo, helloworld. Coloque un archivo de texto con el nombre *Podfile* y el siguiente contenido en ese directorio:

   [!code-ruby[Quickstart code](~/samples-cognitive-services-speech-sdk/quickstart/swift/ios/from-microphone/helloworld/Podfile)]
1. Vaya al directorio helloworld en un terminal y ejecute el comando `pod install`. Este comando genera un área de trabajo de Xcode `helloworld.xcworkspace` que contiene la aplicación de ejemplo y el SDK de Voz como dependencia. Este área de trabajo se usa en los pasos siguientes.

## <a name="build-and-run-the-sample"></a>Compilación y ejecución del ejemplo

1. Abra el área de trabajo `helloworld.xcworkspace` en Xcode.
1. Haga visible la salida de depuración; para ello, seleccione **View** > **Debug Area** > **Activate Console** (Ver > Área de depuración > Activar consola).
1. Elija el simulador de iOS o un dispositivo iOS conectado a la máquina de desarrollo como destino para la aplicación en la lista del menú **Producto** > **Destino**.
1. Compile y ejecute el código de ejemplo en el simulador de iOS; para ello, seleccione **Producto** > **Ejecutar** en el menú. También puede seleccionar el botón **Reproducir**.
1. Después de seleccionar el botón **Recognize** (Reconocer) de la aplicación y decir algunas palabras, verá el texto que ha dicho en la parte inferior de la pantalla.

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Exploración de ejemplos en GitHub](https://aka.ms/csspeech/samples)
