---
title: 'Entrenamiento e implementación de un modelo de Custom Speech: Servicio de voz'
titleSuffix: Azure Cognitive Services
description: Aprenda a entrenar e implementar modelos de Custom Speech. El entrenamiento de un modelo de conversión de voz a texto puede mejorar la precisión del reconocimiento del modelo de línea base de Microsoft o de un modelo personalizado.
services: cognitive-services
author: trevorbye
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 11/11/2020
ms.author: trbye
ms.openlocfilehash: 130cd643856b38471eac6d6869cdc1ed8b0bcd2e
ms.sourcegitcommit: d60976768dec91724d94430fb6fc9498fdc1db37
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/02/2020
ms.locfileid: "96499159"
---
# <a name="train-and-deploy-a-custom-speech-model"></a>Entrenamiento e implementación de un modelo de Habla personalizada

En este artículo, aprenderá a entrenar e implementar modelos de Custom Speech. El entrenamiento de un modelo de conversión de voz a texto puede mejorar la precisión del reconocimiento del modelo de línea base de Microsoft. Para entrenar modelos se usan transcripciones con etiqueta humana y texto relacionado. Estos conjuntos de datos, junto con los datos de audio cargados anteriormente, se usan para refinar y entrenar el modelo de conversión de voz en texto.

## <a name="use-training-to-resolve-accuracy-problems"></a>Uso del entrenamiento para solucionar problemas de precisión

Si tiene problemas de reconocimiento con un modelo base, puede usar transcripciones con etiqueta humana y datos relacionados para entrenar un modelo personalizado que le ayude a mejorar la precisión. Use esta tabla para determinar qué conjunto de datos se debe usar para solucionar sus problemas:

| Caso de uso | Tipo de datos |
| -------- | --------- |
| Mejorar la precisión del reconocimiento en el vocabulario y la gramática específicos del sector, como la terminología médica o la jerga de TI. | Texto relacionado (frases o expresiones) |
| Definir tanto la fonética como la forma que se muestra de una palabra o término que tiene una pronunciación no estándar, como nombres de producto o acrónimos | Texto relacionado (pronunciación) |
| Mejorar la precisión del reconocimiento en estilos de habla, acentos o ruidos de fondo específicos | Transcripciones de audio con etiqueta humana |

## <a name="train-and-evaluate-a-model"></a>Entrenamiento y evaluación de un modelo

El primer paso para entrenar un modelo es cargar los datos de entrenamiento. En el artículo sobre [preparación y realización de pruebas en los datos](./how-to-custom-speech-test-and-train.md) encontrará instrucciones detalladas para preparar transcripciones etiquetada por usuarios y texto relacionado (expresiones y pronunciaciones). Después de cargar los datos de entrenamiento, siga estas instrucciones para empezar a entrenar el modelo:

1. Inicie sesión en el [portal de Habla personalizada](https://speech.microsoft.com/customspeech).
2. Vaya a **Speech-to-Text** > **Custom Speech** >  **[nombre del proyecto]**  > **Aprendizaje**.
3. Seleccione **Entrenar modelo**
4. Asigne un **nombre** y una **descripción** a su entrenamiento.
5. En la lista **Scenario and Baseline model** (Escenario y modelo de línea base), seleccione el escenario que más se ajuste a su dominio. Si no está seguro de qué escenario elegir, seleccione **General**. El modelo de línea de base es el punto de partida para el entrenamiento. El modelo más reciente suele ser la mejor opción.
6. En la página **Select training data** (Seleccionar datos de entrenamiento), elija uno o varios conjuntos de datos de audio + transcripción con la etiqueta humana que quiera usar para el entrenamiento.
7. Tras finalizar el entrenamiento, puede realizar pruebas de precisión en el modelo recién entrenado. Este paso es opcional.
8. Seleccione **Create** (Crear) para generar el modelo personalizado.

La tabla **Entrenamiento** muestra una nueva entrada que corresponde al modelo nuevo. En la tabla también se muestra el estado: **Procesando**, **Correcto** o **Error**.

Consulte el [procedimiento](how-to-custom-speech-evaluate-data.md) para evaluar y mejorar la precisión de los modelos de Habla personalizada. Si elige probar la precisión, es importante seleccionar un conjunto de datos acústico diferente del usado para la creación del modelo para obtener una idea realista del rendimiento del modelo.

## <a name="deploy-a-custom-model"></a>Implementación de un modelo personalizado

Después de haber cargado e inspeccionado los datos, haber evaluado la precisión y haber entrenado un modelo personalizado, puede implementar un punto de conexión personalizado para usarlo con sus productos, herramientas y aplicaciones. 

Para crear un punto de conexión personalizado, inicie sesión en el [portal de Custom Speech](https://speech.microsoft.com/customspeech). Seleccione **Implementación** en el menú **Custom Speech** de la parte superior de la página. Si se trata de la primera ejecución, observará que no aparece ningún punto de conexión en la tabla. Después de crear un punto de conexión, usará esta página para realizar el seguimiento de cada punto de conexión implementado.

A continuación, seleccione **Agregar punto de conexión** y escriba un **nombre** y una **descripción** para el punto de conexión personalizado. Luego, seleccione el modelo personalizado que desea asociar al punto de conexión.  Desde esta página también puede habilitar el registro. El registro le permite supervisar el tráfico del punto de conexión. Si está deshabilitado, el tráfico no se almacenará.

![Captura de pantalla que muestra la página Nuevo punto de conexión.](./media/custom-speech/custom-speech-deploy-model.png)

> [!NOTE]
> No olvide aceptar los términos de uso y los detalles de precios.

A continuación, seleccione **Crear**. Esta acción le devolverá a la página **Implementación**. Ahora, la tabla incluye una entrada que se corresponde con el punto de conexión personalizado. El estado del punto de conexión muestra su estado actual. Se puede tardar hasta 30 minutos en crear una instancia de un nuevo punto de conexión con los modelos personalizados. Cuando el estado de la implementación cambie a **Completado**, el punto de conexión estará listo para su uso.

Una vez implementado el punto de conexión, su nombre aparece como un vínculo. Seleccione el vínculo para ver información específica del punto de conexión, como su clave o su dirección URL, y código de ejemplo.

## <a name="view-logging-data"></a>Visualización de datos de registro

Los datos de registro se pueden descargar en **Punto de conexión** > **Detalles**.
> [!NOTE]
>Los datos de registro están disponibles durante 30 días en el almacenamiento que pertenece a Microsoft. Transcurrido ese periodo se eliminarán. Si una cuenta de almacenamiento que pertenezca a un cliente está vinculada a la suscripción a Cognitive Services, los datos de registro no se eliminarán automáticamente.

## <a name="next-steps"></a>Pasos siguientes

* [Aprenda a usar un modelo personalizado](how-to-specify-source-language.md).

## <a name="additional-resources"></a>Recursos adicionales

- [Preparación y prueba de los datos](./how-to-custom-speech-test-and-train.md)
- [Inspección de los datos](how-to-custom-speech-inspect-data.md)
- [Evaluación de los datos](how-to-custom-speech-evaluate-data.md)
