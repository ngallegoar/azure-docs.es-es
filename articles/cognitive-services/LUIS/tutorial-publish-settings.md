---
title: 'Tutorial: Configuración de publicación: LUIS'
titleSuffix: Azure Cognitive Services
description: En este tutorial, cambie la configuración de publicación para mejorar las predicciones.
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: tutorial
ms.date: 12/13/2019
ms.author: diberry
ms.openlocfilehash: 2df32c20bebf4243f383a0cccd8f037721533602
ms.sourcegitcommit: 9ee0cbaf3a67f9c7442b79f5ae2e97a4dfc8227b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "75890386"
---
# <a name="tutorial--add-sentiment-analysis-as-a-publishing-setting"></a>Tutorial:  Incorporación de análisis de opiniones como configuración de publicación

En este tutorial, modifique la configuración de publicación para extraer el análisis de opiniones y, a continuación, consulte el punto de conexión de LUIS para ver la opinión devuelta en la expresión de un usuario.

**En este tutorial, aprenderá a:**

<!-- green checkmark -->
> [!div class="checklist"]
> * Agregar análisis de opiniones como configuración de publicación
> * Obtener la opinión de una expresión desde el punto de conexión publicado

[!INCLUDE [LUIS Free account](../../../includes/cognitive-services-luis-free-key-short.md)]

## <a name="sentiment-analysis-is-a-publish-setting"></a>Análisis de sentimiento es una opción de publicación

Las expresiones siguientes muestran ejemplos de opiniones:

|Opinión|Score|Expresión|
|:--|:--|:--|
|negative|0,01 |La pizza era horrible.|
|positiva|0,97 % |La pizza de queso estaba buenísima.|

El análisis de opiniones es una configuración de publicación que se aplica a cada expresión. Una vez establecida, la aplicación devuelve la opinión de una expresión sin tener que etiquetar los datos.

Como es una configuración de publicación, no la verá etiquetada en las páginas de entidades o intenciones. Puede verlo en el panel de [prueba interactiva](luis-interactive-test.md#view-sentiment-results) o cuando se prueba en la dirección URL del punto de conexión.

## <a name="import-example-json-to-begin-app"></a>Importación de un archivo .json de ejemplo para comenzar la aplicación

1.  Descargue y guarde el [archivo JSON de la aplicación](https://raw.githubusercontent.com/Azure-Samples/cognitive-services-language-understanding/master/documentation-samples/tutorials/machine-learned-entity/pizza-tutorial-with-entities.json).

[!INCLUDE [Import app steps](includes/import-app-steps.md)]

## <a name="train-the-app"></a>Entrenamiento de la aplicación

[!INCLUDE [LUIS How to Train steps](includes/howto-train.md)]

## <a name="configure-app-to-include-sentiment-analysis"></a>Configuración de la aplicación para que incluya el análisis de opiniones

1. Seleccione **Publicar** (Publicar) en el menú superior. El análisis de opiniones es una configuración de publicación.

1. Seleccione **Espacio de producción** y seleccione **Cambiar configuración**.
1. Establezca el valor de Análisis de sentimiento en **Activado**.

    ![Activación del Análisis de sentimiento como configuración de publicación](./media/luis-quickstart-intent-and-sentiment-analysis/select-sentiment-publishing-setting.png)

## <a name="get-the-sentiment-of-an-utterance-from-the-endpoint"></a>Obtención de la opinión de una expresión desde el punto de conexión

1. [!INCLUDE [LUIS How to get endpoint first step](includes/howto-get-endpoint.md)]

1. Vaya al final de la dirección URL y escriba la siguiente expresión:

    `Deliver 2 of the best cheese pizzas ever!!!`

    El último parámetro de la cadena de consulta es `query`, la expresión **query**. Esta expresión no es la misma que cualquiera de las expresiones etiquetadas, por lo que es una buena prueba y debería devolver la intención `OrderPizza` con el análisis de sentimiento extraído.

    ```json
    {
        "query": "Deliver 2 of the best cheese pizzas ever!!!",
        "prediction": {
            "topIntent": "OrderPizza",
            "intents": {
                "OrderPizza": {
                    "score": 0.4659952
                },
                "None": {
                    "score": 0.16901511
                },
                "Confirm": {
                    "score": 0.00421415
                },
                "Greeting": {
                    "score": 0.00109914376
                }
            },
            "entities": {
                "number": [
                    2
                ],
                "$instance": {
                    "number": [
                        {
                            "type": "builtin.number",
                            "text": "2",
                            "startIndex": 8,
                            "length": 1,
                            "modelTypeId": 2,
                            "modelType": "Prebuilt Entity Extractor",
                            "recognitionSources": [
                                "model"
                            ]
                        }
                    ]
                }
            },
            "sentiment": {
                "label": "positive",
                "score": 0.861665964
            }
        }
    }
    ```

    El análisis de opiniones es positivo con una puntuación del 86 %.

## <a name="clean-up-resources"></a>Limpieza de recursos

[!INCLUDE [LUIS How to clean up resources](../../../includes/cognitive-services-luis-tutorial-how-to-clean-up-resources.md)]

## <a name="related-information"></a>Información relacionada

* [Text Analytics](../Text-Analytics/index.yml) de Cognitive Services ofrece Análisis de sentimiento. La característica está restringida a los [idiomas admitidos](luis-language-support.md#languages-supported) por Text Analytics.
* [Cómo se realiza el entrenamiento](luis-how-to-train.md)
* [Publicación](luis-how-to-publish-app.md)
* [Prueba de la aplicación el portal de LUIS](luis-interactive-test.md)


## <a name="next-steps"></a>Pasos siguientes
Este tutorial agrega el análisis de opiniones como una configuración de publicación para extraer los valores de las opiniones de la expresión en su conjunto.

> [!div class="nextstepaction"]
> [Revisión de las expresiones de punto de conexión en la aplicación de RR. HH.](luis-tutorial-review-endpoint-utterances.md)

