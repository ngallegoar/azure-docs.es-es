---
title: 'Inicio rápido: Síntesis de voz en Objective-C: servicio de voz'
titleSuffix: Azure Cognitive Services
description: Aprenda a sintetizar voz en Objective-C para iOS mediante el SDK de Voz
services: cognitive-services
author: yulin-li
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: quickstart
ms.date: 06/25/2020
ms.author: yulili
ms.openlocfilehash: 7a1cc2c45988e0c5a8c757e4d280b48dbbe56daa
ms.sourcegitcommit: b56226271541e1393a4b85d23c07fd495a4f644d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/26/2020
ms.locfileid: "85391390"
---
# <a name="quickstart-synthesize-speech-in-objective-c-on-ios-using-the-speech-sdk"></a>Inicio rápido: Síntesis de voz en Objective-C para iOS mediante el SDK de Voz

En este artículo, aprenderá a crear una aplicación iOS en Objective-C mediante el SDK de Voz de Cognitive Services para sintetizar voz a partir de texto.

## <a name="prerequisites"></a>Requisitos previos

Antes de comenzar, presentamos una lista de requisitos previos:

* Una [clave de suscripción](~/articles/cognitive-services/Speech-Service/get-started.md) para el servicio de voz.
* Una máquina macOS con [Xcode 9.4.1](https://geo.itunes.apple.com/us/app/xcode/id497799835?mt=12) o posterior
* El destino establecido en la versión de iOS 9.3 o posterior

## <a name="get-the-speech-sdk-for-ios"></a>Obtención del SDK de Voz para iOS

[!INCLUDE [License Notice](~/includes/cognitive-services-speech-service-license-notice.md)]

Tenga en cuenta que este tutorial no funcionará con ninguna versión del SDK anterior a la 1.7.0.

En la actualidad, el SDK de Voz de Cognitive Services para iOS se distribuye como marco de Cocoa.
Se puede usar en proyectos de Xcode como un administrador de dependencias [CocoaPod](https://cocoapods.org/), o se puede descargar desde https://aka.ms/csspeech/iosbinary y vincularse manualmente. En esta guía se usa CocoaPod.

## <a name="create-an-xcode-project"></a>Creación de un proyecto de Xcode

Inicie Xcode y haga clic en **Archivo** > **Nuevo** > **Proyecto** para iniciar un proyecto nuevo.
En el cuadro de diálogo de selección de plantilla, elija la plantilla "Aplicación de vista única (iOS)".

En los cuadros de diálogo que aparecen después, realice las selecciones siguientes:

1. Cuadro de diálogo Opciones del proyecto
    1. Escriba un nombre para la aplicación de inicio rápido, por ejemplo `helloworld`.
    1. Escriba un nombre e identificador de la organización adecuados, si ya tiene una cuenta de desarrollador de Apple. Con fines de prueba, puede elegir cualquier nombre, como `testorg`. Para firmar la aplicación, necesita un perfil de aprovisionamiento adecuado. Consulte el [sitio para desarrolladores de Apple](https://developer.apple.com/) para más información.
    1. Asegúrese de elegir Objective-C como lenguaje para el proyecto.
    1. Desactive todas las casillas de las pruebas y los datos principales.
    ![Configuración del proyecto](~/articles/cognitive-services/Speech-Service/media/sdk/qs-objectivec-project-settings.png)
1. Selección del directorio del proyecto
    1. Elija el directorio principal para incluir el proyecto. Se crea un directorio `helloworld` en el directorio principal que contiene todos los archivos del proyecto de Xcode.
    1. Deshabilite la creación de un repositorio de Git para este proyecto de ejemplo.

## <a name="install-the-sdk-as-a-cocoapod"></a>Instalación del SDK como CocoaPod

1. Instale el administrador de dependencias de CocoaPod como se describe en sus [instrucciones de instalación](https://guides.cocoapods.org/using/getting-started.html).
1. Vaya al directorio de la aplicación de ejemplo (`helloworld`). Coloque un archivo de texto con el nombre `Podfile` y el siguiente contenido en ese directorio:  
   [!code-ruby[Quickstart Code](~/samples-cognitive-services-speech-sdk/quickstart/objectivec/ios/text-to-speech/helloworld/Podfile)]
1. Vaya al directorio `helloworld` en un terminal y ejecute el comando `pod install`. Se genera un área de trabajo de Xcode `helloworld.xcworkspace` que contiene la aplicación de ejemplo y el SDK de Voz como dependencia. Este área de trabajo se usa en las secciones siguientes.

## <a name="add-the-sample-code"></a>Incorporación del código de ejemplo

1. Abra el área de trabajo `helloworld.xcworkspace` en Xcode.
1. Reemplace el contenido del archivo `AppDelegate.m` generado automáticamente por lo siguiente:  
   [!code-objectivec[Quickstart Code](~/samples-cognitive-services-speech-sdk/quickstart/objectivec/ios/text-to-speech/helloworld/helloworld/AppDelegate.m#code)]
1. Reemplace el contenido del archivo `ViewController.m` generado automáticamente por lo siguiente:  
   [!code-objectivec[Quickstart Code](~/samples-cognitive-services-speech-sdk/quickstart/objectivec/ios/text-to-speech/helloworld/helloworld/ViewController.m#code)]
1. Reemplace la cadena `YourSubscriptionKey` por la clave de suscripción.
1. Reemplace la cadena `YourServiceRegion` por la [región](~/articles/cognitive-services/Speech-Service/regions.md) asociada a sus suscripción (por ejemplo, `westus` para la suscripción de evaluación gratuita).

## <a name="build-and-run-the-sample"></a>Compilación y ejecución del ejemplo

1. Haga visible la salida de depuración (**Ver** > **Área de depuración** > **Activar consola**).
1. Elija el simulador de iOS o un dispositivo iOS conectado a la máquina de desarrollo como destino para la aplicación en la lista del menú **Producto** > **Destino**.
1. Para compilar y ejecutar el código de ejemplo en el simulador de iOS seleccione **Producto** > **Ejecutar** en el menú o haga clic en el botón **Reproducir**.

   ![Aplicación de iOS simulada](~/articles/cognitive-services/Speech-Service/media/sdk/qs-objectivec-simulated-app-tts.png)

1. Después de escribir texto y hacer clic en el botón de la aplicación, debería oír el audio sintetizado que se reproduce.

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Exploración de ejemplos de Objective-C en GitHub](https://aka.ms/csspeech/samples)

