---
title: 'Inicio rápido: Reconocimiento de la voz a través de un micrófono en Objective-C: servicio de voz'
titleSuffix: Azure Cognitive Services
description: Aprenda a reconocer la voz en Objective-C para macOS mediante el SDK de Voz
services: cognitive-services
author: chlandsi
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: quickstart
ms.date: 06/25/2020
ms.author: chlandsi
ms.openlocfilehash: 8f94f2ed810204d739dfd2e6d5e88ef0977ad9a4
ms.sourcegitcommit: b56226271541e1393a4b85d23c07fd495a4f644d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/26/2020
ms.locfileid: "85391526"
---
# <a name="quickstart-recognize-speech-in-objective-c-on-macos-by-using-the-speech-sdk"></a>Inicio rápido: Reconocimiento de voz en Objective-C para macOS mediante el SDK de Voz

También hay inicios rápidos disponibles para la [síntesis de voz](~/articles/cognitive-services/Speech-Service/quickstarts/text-to-speech-langs/objectivec-macos.md).

En este artículo aprenderá a crear una aplicación macOS en Objective-C mediante el SDK de Voz de Azure Cognitive Services para transcribir a texto la voz grabada con un micrófono.

## <a name="prerequisites"></a>Requisitos previos

Antes de comenzar, necesitará lo siguiente:

* Una [clave de suscripción](~/articles/cognitive-services/Speech-Service/get-started.md) del servicio de voz.
* Una máquina macOS con [Xcode 9.4.1](https://geo.itunes.apple.com/us/app/xcode/id497799835?mt=12) o una versión posterior y macOS 10.13 o posterior.

## <a name="get-the-speech-sdk-for-macos"></a>Obtención del SDK de Voz para macOS

[!INCLUDE [License notice](~/includes/cognitive-services-speech-service-license-notice.md)]

El SDK de Voz de Cognitive Services para Mac se distribuye como un paquete de marcos. Se puede usar en proyectos de Xcode como administrador de dependencias de [CocoaPods](https://cocoapods.org/) o se puede descargar desde https://aka.ms/csspeech/macosbinary y vincular manualmente. En este artículo se usa CocoaPods.

## <a name="create-an-xcode-project"></a>Creación de un proyecto de Xcode

Inicie Xcode y seleccione **Archivo** > **Nuevo** > **Proyecto** para iniciar un proyecto nuevo. En el cuadro de diálogo de selección de plantilla, seleccione la plantilla **Aplicación de Cocoa**.

En los cuadros de diálogo que aparecen después, realice las selecciones siguientes.

1. En **Opciones del proyecto**:
    1. Escriba un nombre para la aplicación de inicio rápido, por ejemplo, *helloworld*.
    1. Si ya tiene una cuenta de desarrollador de Apple, escriba un nombre y un identificador de organización adecuados. Para fines de prueba, use un nombre como *testorg*. Para firmar la aplicación, necesita un perfil de aprovisionamiento adecuado. Para más información, consulte el [centro para desarrolladores de Apple](https://developer.apple.com/).
    1. Asegúrese de seleccionar **Objective-C** como lenguaje para el proyecto.
    1. Desactive las casillas para usar guiones gráficos y para crear una aplicación basada en documentos. La interfaz de usuario sencilla para la aplicación de ejemplo se crea mediante programación.
    1. Desactive todas las casillas de pruebas y datos principales.

    ![Configuración del proyecto](~/articles/cognitive-services/Speech-Service/media/sdk/qs-objectivec-macos-project-settings.png)

1. Seleccione un directorio de proyecto:
    1. Elija un directorio en el que colocar el proyecto. En este paso se crea un directorio helloworld en el directorio principal que contiene todos los archivos del proyecto de Xcode.
    1. Deshabilite la creación de un repositorio de Git para este proyecto de ejemplo.
1. Establezca los derechos de acceso a la red y al micrófono. Seleccione el nombre de la aplicación en la primera línea de la información general a la izquierda para obtener la configuración de aplicación. Después, seleccione la pestaña **Funcionalidades**.
    1. Habilite la configuración **App Sandbox** para la aplicación.
    1. Habilite las casillas de acceso **Conexiones salientes** y **Micrófono**.

    ![Configuración del espacio aislado](~/articles/cognitive-services/Speech-Service/media/sdk/qs-objectivec-macos-sandbox.png)

1. La aplicación también debe declarar el uso del micrófono en el archivo `Info.plist`. Seleccione el archivo en la información general y agregue la clave **Privacidad: descripción de uso del micrófono**, con un valor como *Microphone is needed for speech recognition* (Se necesita el micrófono para el reconocimiento de voz).

    ![Configuración de Info.plist](~/articles/cognitive-services/Speech-Service/media/sdk/qs-objectivec-macos-info-plist.png)

1. Cierre el proyecto de Xcode. Después de configurar CocoaPods usará una instancia suya diferente.

## <a name="install-the-sdk-as-a-cocoapod"></a>Instalación del SDK como CocoaPod

1. Instale el administrador de dependencias de CocoaPod como se describe en sus [instrucciones de instalación](https://guides.cocoapods.org/using/getting-started.html).
1. Vaya al directorio de la aplicación de ejemplo, helloworld. Coloque un archivo de texto con el nombre *Podfile* y el siguiente contenido en ese directorio:

   [!code-ruby[Quickstart code](~/samples-cognitive-services-speech-sdk/quickstart/objectivec/macos/from-microphone/helloworld/Podfile)]
1. Vaya al directorio helloworld en un terminal y ejecute el comando `pod install`. Este comando genera un área de trabajo de Xcode `helloworld.xcworkspace` que contiene la aplicación de ejemplo y el SDK de Voz como dependencia. Este área de trabajo se usa en los pasos siguientes.

## <a name="add-the-sample-code"></a>Incorporación del código de ejemplo

1. Abra el área de trabajo `helloworld.xcworkspace` en Xcode.
1. Reemplace el contenido del archivo `AppDelegate.m` generado automáticamente por el código siguiente:

   [!code-objectivec[Quickstart code](~/samples-cognitive-services-speech-sdk/quickstart/objectivec/macos/from-microphone/helloworld/helloworld/AppDelegate.m#code)]
1. Reemplace la cadena `YourSubscriptionKey` por la clave de suscripción.
1. Reemplace la cadena `YourServiceRegion` por la [región](~/articles/cognitive-services/Speech-Service/regions.md) asociada a su suscripción. Por ejemplo, use `westus` para la suscripción de prueba gratuita.

## <a name="build-and-run-the-sample"></a>Compilación y ejecución del ejemplo

1. Haga visible la salida de depuración; para ello, seleccione **View** > **Debug Area** > **Activate Console** (Ver > Área de depuración > Activar consola).
1. Para compilar y ejecutar el código de ejemplo, seleccione **Producto** > **Ejecutar** en el menú. También puede seleccionar **Reproducir**.
1. Después de seleccionar el botón y decir algunas palabras, debería ver el texto que ha dicho en la parte inferior de la pantalla. Al ejecutar la aplicación por primera vez, se le solicitará que proporcione a la aplicación acceso al micrófono del equipo.

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Exploración de ejemplos de Objective-C en GitHub](https://aka.ms/csspeech/samples)
