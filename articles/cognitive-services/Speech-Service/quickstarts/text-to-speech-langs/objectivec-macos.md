---
title: 'Inicio rápido: Síntesis de voz en Objective-C: servicio de voz'
titleSuffix: Azure Cognitive Services
description: Aprenda a sintetizar voz en Objective-C para macOS mediante el SDK de Voz
services: cognitive-services
author: yulin-li
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: quickstart
ms.date: 12/09/2019
ms.author: yulili
ms.openlocfilehash: 37eed03ed839411f1acf5d963d4118a3c6d2c379
ms.sourcegitcommit: 9ee0cbaf3a67f9c7442b79f5ae2e97a4dfc8227b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "74975931"
---
# <a name="quickstart-synthesize-speech-in-objective-c-on-macos-using-the-speech-sdk"></a>Inicio rápido: Síntesis de voz en Objective-C para macOS mediante el SDK de Voz

En este artículo, aprenderá a crear una aplicación macOS en Objective-C mediante el SDK de Voz de Cognitive Services para sintetizar voz a partir de texto y reproducirla con la salida de audio predeterminada.

## <a name="prerequisites"></a>Prerrequisitos

Antes de comenzar, presentamos una lista de requisitos previos:

* Una [clave de suscripción](~/articles/cognitive-services/Speech-Service/get-started.md) para el servicio de voz.
* Una máquina macOS con [Xcode 9.4.1](https://geo.itunes.apple.com/us/app/xcode/id497799835?mt=12) o posterior y macOS 10.13 o posterior

## <a name="get-the-speech-sdk-for-macos"></a>Obtención del SDK de Voz para macOS

[!INCLUDE [License Notice](~/includes/cognitive-services-speech-service-license-notice.md)]

Tenga en cuenta que este tutorial no funcionará con ninguna versión del SDK anterior a la 1.7.0.

El SDK de Voz de Cognitive Services para Mac se distribuye como un paquete de marcos.
Se puede usar en proyectos de Xcode como un administrador de dependencias [CocoaPod](https://cocoapods.org/), o se puede descargar desde https://aka.ms/csspeech/macosbinary y vincularse manualmente. En esta guía se usa CocoaPod.

## <a name="create-an-xcode-project"></a>Creación de un proyecto de Xcode

Inicie Xcode y haga clic en **Archivo** > **Nuevo** > **Proyecto** para iniciar un proyecto nuevo.
En el cuadro de diálogo de selección de plantilla, elija la plantilla "Aplicación de Cocoa".

En los cuadros de diálogo que aparecen después, realice las selecciones siguientes:

1. Cuadro de diálogo Opciones del proyecto
    1. Escriba un nombre para la aplicación de inicio rápido, por ejemplo `helloworld`.
    1. Si ya tiene una cuenta de desarrollador de Apple, escriba un nombre y un identificador de organización adecuados. Con fines de prueba, puede elegir cualquier nombre, como `testorg`. Para firmar la aplicación, necesita un perfil de aprovisionamiento adecuado. Consulte el [sitio para desarrolladores de Apple](https://developer.apple.com/) para más información.
    1. Asegúrese de elegir Objective-C como lenguaje para el proyecto.
    1. Desactive las casillas para usar guiones gráficos y para crear una aplicación basada en documentos. La interfaz de usuario sencilla para la aplicación de ejemplo se creará mediante programación.
    1. Desactive todas las casillas de las pruebas y los datos principales.
    ![Configuración del proyecto](~/articles/cognitive-services/Speech-Service/media/sdk/qs-objectivec-macos-project-settings.png)
1. Selección del directorio del proyecto
    1. Elija un directorio en el que colocar el proyecto. Se crea un directorio `helloworld` en el directorio principal que contiene todos los archivos del proyecto de Xcode.
    1. Deshabilite la creación de un repositorio de Git para este proyecto de ejemplo.
1. Establezca los derechos de acceso a la red. Haga clic en el nombre de la aplicación en la primera línea de la información general a la izquierda para obtener la configuración de aplicación y, a continuación, elija la pestaña "Funcionalidades".
    1. Habilite la opción "App Sandbox" para la aplicación.
    1. Habilite las casillas de acceso a "Conexiones salientes".
    ![Configuración de espacio aislado](~/articles/cognitive-services/Speech-Service/media/sdk/qs-objectivec-macos-sandbox-tts.png)
1. Cierre el proyecto de Xcode. Más tarde, usará una instancia diferente de él después de configurar CocoaPods.

## <a name="install-the-sdk-as-a-cocoapod"></a>Instalación del SDK como CocoaPod

1. Instale el administrador de dependencias de CocoaPod como se describe en sus [instrucciones de instalación](https://guides.cocoapods.org/using/getting-started.html).
1. Vaya al directorio de la aplicación de ejemplo (`helloworld`). Coloque un archivo de texto con el nombre `Podfile` y el siguiente contenido en ese directorio:  
   [!code-ruby[Quickstart Code](~/samples-cognitive-services-speech-sdk/quickstart/objectivec/macos/text-to-speech/helloworld/Podfile)]
1. Vaya al directorio `helloworld` en un terminal y ejecute el comando `pod install`. Se genera un área de trabajo de Xcode `helloworld.xcworkspace` que contiene la aplicación de ejemplo y el SDK de Voz como dependencia. Este área de trabajo se usa en las secciones siguientes.

## <a name="add-the-sample-code"></a>Incorporación del código de ejemplo

1. Abra el área de trabajo `helloworld.xcworkspace` en Xcode.
1. Reemplace el contenido del archivo `AppDelegate.m` generado automáticamente por lo siguiente:  
   [!code-objectivec[Quickstart Code](~/samples-cognitive-services-speech-sdk/quickstart/objectivec/macos/text-to-speech/helloworld/helloworld/AppDelegate.m#code)]
1. Reemplace la cadena `YourSubscriptionKey` por la clave de suscripción.
1. Reemplace la cadena `YourServiceRegion` por la [región](~/articles/cognitive-services/Speech-Service/regions.md) asociada a sus suscripción (por ejemplo, `westus` para la suscripción de evaluación gratuita).

## <a name="build-and-run-the-sample"></a>Compilación y ejecución del ejemplo

1. Haga visible la salida de depuración (**Ver** > **Área de depuración** > **Activar consola**).
1. Para compilar y ejecutar el código de ejemplo, seleccione **Producto** -> **Ejecutar** en el menú o haga clic en el botón **Reproducir**.
1. Después de escribir texto y hacer clic en el botón de la aplicación, debería oír el audio sintetizado que se reproduce.

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Exploración de ejemplos de Objective-C en GitHub](https://aka.ms/csspeech/samples)

