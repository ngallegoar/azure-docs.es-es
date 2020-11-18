---
title: Uso de un modelo ONNX con Windows ML (Custom Vision Service)
titleSuffix: Azure Cognitive Services
description: Obtenga información acerca de cómo crear una aplicación para UWP de Windows que utilice un modelo ONNX exportado desde Azure Cognitive Services.
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: custom-vision
ms.topic: conceptual
ms.date: 04/29/2020
ms.author: pafarley
ms.openlocfilehash: 58ced0c45d66223ac3e40112126e92a4539db32d
ms.sourcegitcommit: 9706bee6962f673f14c2dc9366fde59012549649
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/13/2020
ms.locfileid: "94616083"
---
# <a name="use-an-onnx-model-from-custom-vision-with-windows-ml-preview"></a>Uso de un modelo ONNX de Custom Vision con Windows ML (versión preliminar)

Obtenga información acerca de cómo usar un modelo ONNX exportado desde Custom Vision Service con Windows ML (versión preliminar).

En esta guía, aprenderá a usar un archivo ONNX exportado desde Custom Vision Service con Windows ML. Usará la aplicación de UWP de ejemplo con su propio clasificador de imágenes entrenado.

## <a name="prerequisites"></a>Prerrequisitos

* Windows 10 versión 1809 o posterior
* Windows SDK para la versión 17763 o posterior
* La versión 15.7 de Visual Studio 2017 o una posterior con la carga de trabajo __Desarrollo de la plataforma universal de Windows__ habilitada.
* Modo de desarrollador habilitado en su equipo. Para más información, consulte [Habilitar el dispositivo para el desarrollo](/windows/uwp/get-started/enable-your-device-for-development).

## <a name="about-the-example-app"></a>Acerca de la aplicación de ejemplo

La aplicación incluida es una aplicación para UWP genérica de Windows. Permite seleccionar una imagen del equipo, que después se procesa mediante un modelo de clasificación almacenado localmente. Las etiquetas y los resultados devueltos por el modelo se muestran junto a la imagen.

## <a name="get-the-example-code"></a>Obtención del código de ejemplo

La aplicación de ejemplo está disponible en el repositorio [Cognitive Services ONNX Custom Vision Sample](https://github.com/Azure-Samples/cognitive-services-onnx-customvision-sample) de GitHub. La puede clonar en la máquina local y abrir *SampleOnnxEvaluationApp.sln* en Visual Studio.

## <a name="test-the-application"></a>Prueba de la aplicación

1. Use la tecla `F5` para iniciar la aplicación desde Visual Studio. Es posible que se le solicite que habilite el modo de desarrollador.
1. Cuando se inicie la aplicación, use el botón para seleccionar una imagen para la puntuación. El modelo ONNX predeterminado está entrenado para clasificar distintos tipos de plancton.

## <a name="use-your-own-model"></a>Uso de su propio modelo

Para usar su propio modelo clasificador de imágenes, siga estos pasos:

1. Cree y entrene un clasificador con Custom Vision Service. Puede encontrar instrucciones sobre cómo hacerlo en [Creación y entrenamiento de un clasificador](./getting-started-build-a-classifier.md). Use uno de los dominios **compactos**, como **General (compacto)** . 
   * Si tiene un clasificador que usa un dominio diferente, puede convertirlo a **compacto** en la configuración del proyecto. Después, vuelva a entrenar el proyecto antes de continuar.
1. Exporte el modelo. Cambie a la pestaña Rendimiento y seleccione una iteración que se haya entrenado con un dominio **compacto**. Seleccione el botón **Exportar** que aparece. Luego, seleccione **ONNX** y, después, **Exportar**. Una vez que el archivo esté listo, seleccione el botón **Descargar**. Para más información sobre las opciones de exportación, consulte [Exportación del modelo](./export-your-model.md).
1. Abra el archivo *.zip* descargado y extraiga el archivo *model.onnx*. Este archivo contiene el modelo clasificador.
1. En el Explorador de soluciones de Visual Studio, haga clic con el botón derecho en la carpeta **Assets** y seleccione __Agregar elemento existente__. Seleccione el archivo ONNX.
1. En el Explorador de soluciones, haga clic con el botón derecho en el archivo ONNX y seleccione **Propiedades**. Cambie las siguientes propiedades del archivo:
   * __Acción de compilación__ -> __Contenido__
   * __Copiar en el directorio de salida__ -> __Copiar si es posterior__
1. Después, abra _MainPage.xaml.cs_ y cambie el valor de `_ourOnnxFileName` por el nombre del archivo ONNX.
1. Use `F5` para compilar y ejecutar el proyecto.
1. Haga clic en el botón para seleccionar la imagen que desea evaluar.

## <a name="next-steps"></a>Pasos siguientes

Para conocer otras maneras de exportar y usar un modelo de Custom Vision, consulte los documentos siguientes:

* [Exportación del modelo](./export-your-model.md)
* [Use exported Tensorflow model in an Android application](https://github.com/Azure-Samples/cognitive-services-android-customvision-sample) (Uso del modelo Tensorflow exportado en una aplicación de Android)
* [Use exported CoreML model in a Swift iOS application](https://go.microsoft.com/fwlink/?linkid=857726) (Uso del modelo CoreML exportado en una aplicación de iOS con Swift)
* [Use exported CoreML model in an iOS application with Xamarin](https://github.com/xamarin/ios-samples/tree/master/ios11/CoreMLAzureModel) (Uso del modelo CoreML exportado en una aplicación de iOS con Xamarin)

Para más información sobre el uso de modelos ONNX con Windows ML, consulte [Integración de un modelo en la aplicación con Windows ML](/windows/ai/windows-ml/integrate-model).