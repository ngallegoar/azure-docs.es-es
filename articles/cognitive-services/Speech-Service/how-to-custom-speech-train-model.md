---
title: 'Entrenamiento e implementación de un modelo de Habla personalizada: Servicio de voz'
titleSuffix: Azure Cognitive Services
description: En este artículo, aprenderá a entrenar e implementar modelos de Habla personalizada. El entrenamiento de un modelo de conversión de voz a texto puede mejorar la precisión del reconocimiento del modelo de base de referencia de Microsoft o de un modelo personalizado. Un modelo se entrena mediante transcripciones con etiqueta humana y el texto relacionado.
services: cognitive-services
author: trevorbye
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 11/11/2020
ms.author: trbye
ms.openlocfilehash: 9193da2f3841bb94aa395399c31d7fe826c395e0
ms.sourcegitcommit: 10d00006fec1f4b69289ce18fdd0452c3458eca5
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/21/2020
ms.locfileid: "95025606"
---
# <a name="train-and-deploy-a-custom-speech-model"></a>Entrenamiento e implementación de un modelo de Habla personalizada

En este artículo, aprenderá a entrenar e implementar modelos de Habla personalizada. El entrenamiento de un modelo de conversión de voz a texto puede mejorar la precisión del reconocimiento del modelo de base de referencia de Microsoft. Un modelo se entrena mediante transcripciones con etiqueta humana y el texto relacionado. Estos conjuntos de datos, junto con los datos de audio cargados anteriormente, se usan para refinar y entrenar el modelo de conversión de voz a texto.

## <a name="use-training-to-resolve-accuracy-issues"></a>Uso del entrenamiento para solucionar problemas de precisión

Si se está encontrando con problemas de reconocimiento con el modelo base, el uso de transcripciones con etiqueta humana y los datos relacionados para entrenar un modelo personalizado puede ayudar a mejorar la precisión. Use esta tabla para determinar qué conjunto de datos se debe usar para solucionar su problema:

| Caso de uso | Tipo de datos |
| -------- | --------- |
| Mejorar la precisión del reconocimiento en el vocabulario y la gramática específicos del sector, como la terminología médica o la jerga de TI. | Texto relacionado (frases o expresiones) |
| Definir el formato fonético y mostrado de una palabra o término que tenga una pronunciación no estándar, como nombres de producto o acrónimos | Texto relacionado (pronunciación) |
| Mejorar la precisión del reconocimiento en estilos de habla, acentos o ruidos de fondo específicos. | Transcripciones de audio con etiqueta humana |

## <a name="train-and-evaluate-a-model"></a>Entrenamiento y evaluación de un modelo

El primer paso para entrenar un modelo es cargar los datos de entrenamiento. Use [Preparación y prueba de los datos](./how-to-custom-speech-test-and-train.md) para obtener instrucciones paso a paso para preparar las transcripciones con etiqueta humana y el texto relacionado (expresiones y pronunciaciones). Después de cargar los datos de entrenamiento, siga estas instrucciones para empezar a entrenar el modelo:

1. Inicie sesión en el [portal de Habla personalizada](https://speech.microsoft.com/customspeech).
2. Vaya a **Speech-to-text > Custom Speech > [nombre del proyecto] > Aprendizaje**.
3. Haga clic en **Train model** (Entrenar modelo).
4. A continuación, asigne a su entrenamiento un **nombre** y una **descripción**.
5. En el menú desplegable **Scenario and Baseline model** (Escenario y modelo de línea de base), seleccione el escenario que mejor se adapte a su dominio. Si no está seguro de qué escenario elegir, seleccione **General**. El modelo de línea de base es el punto de partida para el entrenamiento. El modelo más reciente suele ser la mejor opción.
6. En la página **Select training data** (Seleccionar datos de entrenamiento), elija uno o varios conjuntos de datos de audio + transcripción con la etiqueta humana que quiera usar para el entrenamiento.
7. Una vez finalizado el entrenamiento, puede elegir realizar pruebas de precisión en el modelo recién entrenado. Este paso es opcional.
8. Seleccione **Create** (Crear) para generar el modelo personalizado.

En la tabla de entrenamiento se muestra una nueva entrada que corresponde a este modelo recién creado. En la tabla también se muestra el estado: Procesando, Correcto, Error.

Consulte el [procedimiento](how-to-custom-speech-evaluate-data.md) para evaluar y mejorar la precisión de los modelos de Habla personalizada. Si eligió probar la precisión, es importante seleccionar un conjunto de datos acústico diferente del usado para la creación del modelo para obtener una idea realista del rendimiento del modelo.

## <a name="deploy-a-custom-model"></a>Implementación de un modelo personalizado

Después de haber cargado e inspeccionar los datos y de haber evaluado y entrenado un modelo personalizado, puede implementar un punto de conexión personalizado para usar con sus productos, herramientas y aplicaciones. 

Para crear un punto de conexión personalizado, inicie sesión en el [portal de Habla personalizada](https://speech.microsoft.com/customspeech) y seleccione **Implementaciones** en el menú Habla personalizada de la parte superior de la página. Si se trata de la primera ejecución, observará que no aparece ningún punto de conexión en la tabla. Después de crear un punto de conexión, usará esta página para realizar el seguimiento de cada punto de conexión implementado.

A continuación, seleccione **Agregar punto de conexión** y escriba un **nombre** y una **descripción** para el punto de conexión personalizado. Luego, seleccione el modelo personalizado que le gustaría asociar a este punto de conexión. Desde esta página, también puede habilitar el registro. El registro le permite supervisar el tráfico del punto de conexión. Si está deshabilitado, no se almacenará el tráfico.

![Implementación de un modelo](./media/custom-speech/custom-speech-deploy-model.png)

> [!NOTE]
> No olvide aceptar los términos de uso y los detalles de precios.

A continuación, seleccione **Crear**. Esta acción le devolverá a la página **Implementación**. Ahora, la tabla incluye una entrada que se corresponde con el punto de conexión personalizado. El estado del punto de conexión muestra su estado actual. Se puede tardar hasta 30 minutos en crear una instancia de un nuevo punto de conexión con los modelos personalizados. Cuando el estado de la implementación cambie a **Completado**, el punto de conexión estará listo para su uso.

Una vez implementado el punto de conexión, su nombre aparece como un vínculo. Haga clic en el vínculo para mostrar información específica del punto de conexión, como la clave o la dirección URL, y código de ejemplo.

## <a name="view-logging-data"></a>Visualización de datos de registro

Los datos de registro están disponibles descargar en **Punto de conexión > Detalles**.
> [!NOTE]
>Los datos de registro están disponibles durante 30 días en el almacenamiento que pertenece a Microsoft y se quitarán después. En caso de que una cuenta de almacenamiento del cliente esté vinculada a la suscripción a Cognitive Services, los datos de registro no se eliminarán automáticamente.

## <a name="next-steps"></a>Pasos siguientes

* Aprenda a [usar el modelo personalizado](how-to-specify-source-language.md).

## <a name="additional-resources"></a>Recursos adicionales

- [Preparación y prueba de los datos](./how-to-custom-speech-test-and-train.md)
- [Inspección de los datos](how-to-custom-speech-inspect-data.md)
- [Evaluación de los datos](how-to-custom-speech-evaluate-data.md)
