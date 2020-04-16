---
title: 'Tutorial: Uso de un modelo ONNX con Windows ML (Custom Vision Service)'
titleSuffix: Azure Cognitive Services
description: Obtenga información acerca de cómo crear una aplicación para UWP de Windows que utilice un modelo ONNX exportado desde Azure Cognitive Services.
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: custom-vision
ms.topic: tutorial
ms.date: 04/14/2020
ms.author: pafarley
ms.openlocfilehash: 0b59321bf04a8230342be706b88cd208c19d76ea
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/16/2020
ms.locfileid: "81404169"
---
# <a name="tutorial-use-an-onnx-model-from-custom-vision-with-windows-ml-preview"></a>Tutorial: Uso de un modelo ONNX de Custom Vision con Windows ML (versión preliminar)

Obtenga información acerca de cómo usar un modelo ONNX exportado desde Custom Vision Service con Windows ML (versión preliminar).

La información de este documento muestra cómo usar un archivo ONNX exportado desde Custom Vision Service con Windows ML. Se proporciona un ejemplo de una aplicación para UWP de Windows. En el ejemplo, se incluye un modelo entrenado que puede reconocer. También se proporcionan pasos acerca de cómo puede usar su propio modelo.

> [!div class="checklist"]
> * Acerca de la aplicación de ejemplo
> * Obtención del código de ejemplo
> * Ejecutar el ejemplo
> * Uso de su propio modelo

## <a name="prerequisites"></a>Prerrequisitos

* Windows 10 versión 1809 o posterior

* Windows SDK para la versión 17763 o posterior

* La versión 15.7 de Visual Studio 2017 o una posterior con la carga de trabajo __Desarrollo de la plataforma universal de Windows__ habilitada.

* Modo de programador habilitado. Para obtener más información, consulte el documento [Habilitar el dispositivo para el desarrollo](https://docs.microsoft.com/windows/uwp/get-started/enable-your-device-for-development).

## <a name="about-the-example-app"></a>Acerca de la aplicación de ejemplo

La aplicación es una aplicación para UWP genérica de Windows. Permite seleccionar una imagen de su equipo que, a continuación, se envía al modelo. Las etiquetas y los resultados devueltos por el modelo se muestran junto a la imagen.

## <a name="get-the-example-code"></a>Obtención del código de ejemplo

La aplicación de ejemplo está disponible en [https://github.com/Azure-Samples/cognitive-services-onnx-customvision-sample](https://github.com/Azure-Samples/cognitive-services-onnx-customvision-sample).

## <a name="run-the-example"></a>Ejecutar el ejemplo

1. Use la tecla `F5` para iniciar la aplicación desde Visual Studio. Es posible que se le solicite que habilite el modo de desarrollador. Para obtener más información, consulte el documento [Habilitar el dispositivo para el desarrollo](https://docs.microsoft.com/windows/uwp/get-started/enable-your-device-for-development).

1. Cuando se inicie la aplicación, use el botón para seleccionar una imagen para la puntuación.

## <a name="use-your-own-model"></a>Uso de su propio modelo

Para usar su propio modelo, siga estos pasos:

1. [Cree y entrene](https://docs.microsoft.com/azure/cognitive-services/custom-vision-service/getting-started-build-a-classifier) un clasificador con Custom Vision Service. Para exportar el modelo, seleccione un dominio __compacto__ como **General (compacto)** . Para exportar un clasificador existente, convierta el dominio en compacto seleccionando el icono de engranaje de la parte superior derecha. En __Configuración__, elija un modelo compacto, guárdelo y entrene el proyecto.  

1. Para [exportar el modelo](https://docs.microsoft.com/azure/cognitive-services/custom-vision-service/export-your-model) vaya a la pestaña Rendimiento. Seleccione una iteración entrenada con un dominio compacto y aparecerá un botón "Exportar". Seleccione *Exportar*, *ONNX* y *Exportar*. Una vez que el archivo esté listo, seleccione el botón *Descargar*.

1. Coloque el archivo ONNX en la carpeta de __recursos__ del proyecto. 

1. En el Explorador de soluciones, haga clic con el botón derecho en la carpeta de recursos y seleccione __Agregar elemento existente__. Seleccione el archivo ONNX.

1. En el Explorador de soluciones, seleccione el archivo ONNX de la carpeta de recursos. Cambie las siguientes propiedades del archivo:

    * __Acción de compilación__ -> __Contenido__
    * __Copiar en el directorio de salida__ -> __Copiar si es posterior__

1. Cambie la variable `_onnxFileNames` para que sea el nombre del archivo ONNX. Modifique también `ClassLabel` para que sea el número de etiquetas que contiene el modelo.

1. Compile y ejecute.

1. Haga clic en el botón para seleccionar la imagen que desea evaluar.

## <a name="next-steps"></a>Pasos siguientes

Para conocer otras maneras de exportar y usar un modelo de Custom Vision, consulte los documentos siguientes:

* [Exportación del modelo](https://docs.microsoft.com/azure/cognitive-services/custom-vision-service/export-your-model)
* [Use exported Tensorflow model in an Android application](https://github.com/Azure-Samples/cognitive-services-android-customvision-sample) (Uso del modelo Tensorflow exportado en una aplicación de Android)
* [Use exported CoreML model in a Swift iOS application](https://go.microsoft.com/fwlink/?linkid=857726) (Uso del modelo CoreML exportado en una aplicación de iOS con Swift)
* [Use exported CoreML model in an iOS application with Xamarin](https://github.com/xamarin/ios-samples/tree/master/ios11/CoreMLAzureModel) (Uso del modelo CoreML exportado en una aplicación de iOS con Xamarin)

Para obtener más información sobre el uso de modelos ONNX con Windows ML, consulte el documento [Integrar un modelo en la aplicación con Windows ML](/windows/ai/windows-ml/integrate-model).
