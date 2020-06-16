---
title: 'Inspección de la calidad de los datos de Habla personalizada: Servicio de voz'
titleSuffix: Azure Cognitive Services
description: Habla personalizada proporciona herramientas que le permiten inspeccionar visualmente la calidad del reconocimiento de un modelo mediante la comparación de los datos de audio con el resultado de reconocimiento correspondiente. Puede reproducir el audio cargado y determinar si el resultado de reconocimiento proporcionada es correcto.
services: cognitive-services
author: erhopf
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 12/02/2019
ms.author: erhopf
ms.openlocfilehash: e871d2c8e0fe00fa7db3144a787447163c82e62d
ms.sourcegitcommit: d7fba095266e2fb5ad8776bffe97921a57832e23
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/09/2020
ms.locfileid: "84629032"
---
# <a name="inspect-custom-speech-data"></a>Inspección de los datos de Habla personalizada

> [!NOTE]
> En esta página se supone que ha leído [Preparación de los datos de Habla personalizada](how-to-custom-speech-test-data.md) y que ha cargado un conjunto de datos para su inspección.

Habla personalizada proporciona herramientas que le permiten inspeccionar visualmente la calidad del reconocimiento de un modelo mediante la comparación de los datos de audio con el resultado de reconocimiento correspondiente. Desde el [portal de Habla personalizada](https://speech.microsoft.com/customspeech), puede reproducir el audio cargado y determinar si el resultado de reconocimiento proporcionada es correcto. Esta herramienta le ayuda a inspeccionar la calidad del modelo de conversión de voz a texto de línea de base de Microsoft, a inspeccionar un modelo personalizado entrenado o a comparar la transcripción con dos modelos.

En este documento, aprenderá a inspeccionar visualmente la calidad de un modelo con los datos de entrenamiento cargados previamente.

En esta página, aprenderá a inspeccionar visualmente la calidad del modelo de voz a texto de línea de base de Microsoft o de un modelo personalizado que haya entrenado. Usará los datos que cargó en la pestaña **Data** (Datos) para realizar pruebas.

## <a name="create-a-test"></a>Creación de una prueba

Para crear una prueba, siga estas instrucciones:

1. Inicie sesión en el [portal de Habla personalizada](https://speech.microsoft.com/customspeech).
2. Vaya a **Speech-to-text > Custom Speech > [nombre del proyecto] > Pruebas**.
3. Haga clic en **Add test** (Agregar prueba).
4. Seleccione **Inspect quality (Audio-only data)** (Inspeccionar la calidad [solo datos de audio]). Asigne un nombre y una descripción a la prueba y seleccione el conjunto de datos de audio.
5. Puede seleccionar hasta dos modelos para probar.
6. Haga clic en **Crear**.

Una vez que se ha creado correctamente una prueba, puede ver la forma en que un modelo transcribe el conjunto de datos de audio especificado o comparar los resultados de dos modelos en paralelo.

[!INCLUDE [service-pricing-advisory](includes/service-pricing-advisory.md)]

## <a name="side-by-side-model-comparisons"></a>Comparaciones de los modelos en paralelo

Cuando el estado de la prueba sea _Succeeded_ (Correcto), haga clic en el nombre del elemento de prueba para ver los detalles de la prueba. En esta página de detalles se enumeran todas los expresiones del conjunto de datos y se muestran los resultados de reconocimiento de los dos modelos que se comparan.

Con el fin de inspeccionar la comparación en paralelo, puede alternar los distintos tipos de error, como inserción, eliminación y sustitución. Al escuchar el audio y comparar los resultados del reconocimiento de cada columna (que muestra la transcripción con la etiqueta humana y los resultados de los dos modelos de conversión de voz a texto), puede decidir qué modelo satisface sus necesidades y dónde hacen falta mejoras.

Las pruebas de modelos en paralelo son útiles para validar qué modelo de reconocimiento de voz es el mejor para una aplicación. Si necesita una medida objetiva de precisión, que necesite la transcripción del audio, siga las instrucciones que encontrará en [Evaluación de la precisión](how-to-custom-speech-evaluate-data.md).

## <a name="next-steps"></a>Pasos siguientes

- [Evaluación de los datos](how-to-custom-speech-evaluate-data.md)
- [Entrenamiento del modelo](how-to-custom-speech-train-model.md)
- [Mejora del modelo](how-to-custom-speech-improve-accuracy.md)
- [Implementación del modelo](how-to-custom-speech-deploy-model.md)

## <a name="additional-resources"></a>Recursos adicionales

- [Preparación de los datos de prueba para Habla personalizada](how-to-custom-speech-test-data.md)
