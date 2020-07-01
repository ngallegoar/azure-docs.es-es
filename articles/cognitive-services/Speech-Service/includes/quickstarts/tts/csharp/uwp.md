---
title: 'Inicio rápido: Síntesis de voz en C# (UWP): servicio de voz'
titleSuffix: Azure Cognitive Services
description: En este artículo, creará una aplicación para Plataforma universal de Windows (UWP) de C# mediante el SDK de Voz de Cognitive Services. Puede sintetizar la voz a partir de texto en tiempo real en el altavoz del dispositivo. La aplicación se crea con el paquete NuGet del SDK de voz y Microsoft Visual Studio 2019.
services: cognitive-services
author: yinhew
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: include
ms.date: 04/04/2020
ms.author: yinhew
ms.openlocfilehash: 89fc5fddffb291942b8f3a4db3dfdf4ccd6cf46a
ms.sourcegitcommit: 34a6fa5fc66b1cfdfbf8178ef5cdb151c97c721c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/28/2020
ms.locfileid: "81275228"
---
> [!NOTE]
> La Plataforma universal de Windows permite desarrollar aplicaciones que se ejecutan en cualquier dispositivo que admite Windows 10, incluidos PC, Xbox, Surface Hub y otros dispositivos.

## <a name="prerequisites"></a>Prerrequisitos

Antes de comenzar, compruebe lo siguiente:

> [!div class="checklist"]
> * [Ha creado un recurso de Voz de Azure](../../../../get-started.md)
> * [Configuración del entorno de desarrollo y creación de un proyecto vacío](../../../../quickstarts/setup-platform.md?tabs=uwp&pivots=programming-language-csharp)

## <a name="add-sample-code"></a>Incorporación de código de ejemplo

Ahora, agregue el código XAML que define la interfaz de usuario de la aplicación y agregue la implementación de código C# subyacente.

1. En el **Explorador de soluciones**, abra `MainPage.xaml`.

1. En la vista XAML del diseñador, inserte el fragmento de código XAML siguiente en la etiqueta **Cuadrícula** (entre `<Grid>` y `</Grid>`):

   [!code-xml[UI elements](~/samples-cognitive-services-speech-sdk/quickstart/csharp/uwp/text-to-speech/helloworld/MainPage.xaml#StackPanel)]

1. En **Explorador de soluciones**, abra el archivo de código fuente subyacente `MainPage.xaml.cs`. (Se agrupa en `MainPage.xaml`).

1. Reemplace todo el código que contiene por el fragmento siguiente:

   [!code-csharp[Quickstart Code](~/samples-cognitive-services-speech-sdk/quickstart/csharp/uwp/text-to-speech/helloworld/MainPage.xaml.cs#code)]

1. En el controlador `Speak_ButtonClicked` del archivo de código fuente, busque la cadena `YourSubscriptionKey` y reemplácela por su clave de suscripción.

1. En el controlador `Speak_ButtonClicked`, busque la cadena `YourServiceRegion` y reemplácela por la [región](~/articles/cognitive-services/Speech-Service/regions.md) asociada a su suscripción. (Por ejemplo, use `westus` para la suscripción de prueba gratuita).

1. En la barra de menús, elija **Archivo** > **Guardar todo** para guardar los cambios.

## <a name="build-and-run-the-application"></a>Compilación y ejecución de la aplicación

Ahora está listo para compilar y probar la aplicación.

1. En la barra de menús, elija **Compilar** > **Compilar solución** para compilar la aplicación. El código se debería compilar sin errores ahora.

1. Elija **Depurar** > **Iniciar depuración** o presione **F5** para iniciar la aplicación. Aparece la ventana **HelloWorld**.

   ![Ejemplo de aplicación de síntesis de voz de UWP en C#: inicio rápido](~/articles/cognitive-services/Speech-Service/media/sdk/qs-text-to-speech-uwp-helloworld-window.png)

1. Escriba algún texto en el cuadro de texto y haga clic en **Hablar**. El texto se transmite al servicio de voz y se sintetiza en una voz que se reproduce en el altavoz.

    ![Interfaz de usuario de síntesis de voz](~/articles/cognitive-services/Speech-Service/media/sdk/qs-tts-csharp-uwp-ui-result.png)

## <a name="next-steps"></a>Pasos siguientes

[!INCLUDE [Speech synthesis basics](../../text-to-speech-next-steps.md)]

## <a name="see-also"></a>Consulte también

- [Creación de una voz personalizada](~/articles/cognitive-services/Speech-Service/how-to-custom-voice-create-voice.md)
- [Grabación de ejemplos de voz personalizada](~/articles/cognitive-services/Speech-Service/record-custom-voice-samples.md)
