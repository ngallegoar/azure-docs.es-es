---
title: 'Evaluación de la precisión de Habla personalizada: Servicio de voz'
titleSuffix: Azure Cognitive Services
description: En este documento, aprenderá a medir cuantitativamente la calidad del modelo de conversión de voz a texto o su modelo personalizado. Para probar la voz se requieren datos de transcripción de con la etiqueta audio + humano, y se deben proporcionar entre 30 minutos y 5 horas de audio representativo.
services: cognitive-services
author: erhopf
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 09/06/2019
ms.author: erhopf
ms.openlocfilehash: 27229d36dbe9592b6ad6fc2740f7714f99fd6857
ms.sourcegitcommit: d7fba095266e2fb5ad8776bffe97921a57832e23
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/09/2020
ms.locfileid: "84629058"
---
# <a name="evaluate-custom-speech-accuracy"></a>Evaluación de la precisión de Habla personalizada

En este documento, aprenderá a medir cuantitativamente la calidad del modelo de conversión de texto a voz de Microsoft o su modelo personalizado. Para probar la voz se requieren datos de transcripción de con la etiqueta audio + humano, y se deben proporcionar entre 30 minutos y 5 horas de audio representativo.

## <a name="what-is-word-error-rate-wer"></a>¿Qué es Word Error Rate (WER)?

Es el estándar del sector para medir la precisión del modelo. WER cuenta el número de palabras incorrectas identificadas durante el reconocimiento y luego las divide entre el número total de palabras proporcionadas en la transcripción con la etiqueta humano. Por último, ese número se multiplica por 100 % para calcular la tasa WER.

![Fórmula de WER](./media/custom-speech/custom-speech-wer-formula.png)

Las palabras identificadas incorrectamente pertenecen a tres categorías:

* Inserción (I): palabras que se agregan incorrectamente en la transcripción de hipótesis.
* Eliminación (D): palabras que no se detectan en la transcripción de hipótesis.
* Sustitución (S): palabras que se sustituyeron entre la referencia y la hipótesis.

Este es un ejemplo:

![Ejemplo de palabras identificadas incorrectamente](./media/custom-speech/custom-speech-dis-words.png)

## <a name="resolve-errors-and-improve-wer"></a>Resolución de errores y mejora de WER

Puede usar WER a partir de los resultados de reconocimiento automático para evaluar la calidad del modelo que usa con su aplicación, herramienta o producto. Una tasa WER de entre un 5 y un 10 % se considera buena calidad y está listo para usarse. Una tasa WER de 20 % es aceptable, pero quizás considere la posibilidad de entrenamiento adicional. Una tasa WER de 30 % o más señala una calidad deficiente y la necesidad de personalización y entrenamiento.

El modo en que se distribuyen los errores es importante. Si se encuentran muchos errores de eliminación, la causa suele ser la intensidad débil de señal de audio. Para resolver este problema, debe recopilar los datos de audio más cerca de la fuente. Los errores de inserción significan que el audio se grabó en un entorno ruidoso y pueden producirse interferencias, lo que ocasiona problemas de reconocimiento. Los errores de sustitución se encuentran a menudo cuando se ha proporcionado una muestra insuficiente de términos específicos del dominio como transcripciones con la etiqueta humano o texto relacionado.

Al analizar archivos individuales, puede determinar qué tipo de errores existen y qué errores son específicos de un determinado archivo. Comprender los problemas en el nivel de archivo le ayudará a identificar las mejoras.

## <a name="create-a-test"></a>Creación de una prueba

Si quiere probar la calidad del modelo de línea de base de texto a voz de Microsoft o un modelo personalizado que haya entrenado, puede comparar dos modelos en paralelo para evaluar la precisión. La comparación incluye los resultados de reconocimiento y WER. Normalmente, un modelo personalizado se compara con el modelo de línea de base de Microsoft.

Para evaluar los modelos en paralelo:

1. Inicie sesión en el [portal de Habla personalizada](https://speech.microsoft.com/customspeech).
2. Vaya a **Speech-to-text > Custom Speech > [nombre del proyecto] > Pruebas**.
3. Haga clic en **Add test** (Agregar prueba).
4. Seleccione **Evaluate accuracy** (Evaluar precisión). Proporcione a la prueba un nombre y una descripción, y seleccione el conjunto de datos de transcripción con la etiqueta audio + humano.
5. Puede seleccionar hasta dos modelos para probar.
6. Haga clic en **Crear**.

Después de que la prueba se ha creado correctamente, puede comparar los resultados en paralelo.

## <a name="side-by-side-comparison"></a>Comparación en paralelo

Una vez finalizada la prueba, que viene indicado por el cambio de estado a *Succeeded* (Correcto), encontrará un número de WER para ambos modelos incluidos en la prueba. Haga clic en el nombre de la prueba para ver la página de detalles de las pruebas. En esta página se muestran todas las expresiones del conjunto de datos y se indican los resultados del reconocimiento de los dos modelos al lado de la transcripción del conjunto de datos enviado. Con el fin de inspeccionar la comparación en paralelo, puede alternar los distintos tipos de error, como inserción, eliminación y sustitución. Al escuchar el audio y comparar los resultados del reconocimiento de cada columna, que muestra la transcripción con la etiqueta humano y los resultados de los dos modelos de voz a texto, puede decidir qué modelo satisface sus necesidades y dónde hacen falta entrenamiento y mejoras adicionales.

## <a name="next-steps"></a>Pasos siguientes

* [Entrenamiento del modelo](how-to-custom-speech-train-model.md)
* [Implementación del modelo](how-to-custom-speech-deploy-model.md)

## <a name="additional-resources"></a>Recursos adicionales

* [Preparación y prueba de los datos](how-to-custom-speech-test-data.md)
* [Inspección de los datos](how-to-custom-speech-inspect-data.md)
