---
title: 'Tutorial: Configuración de publicación: LUIS'
description: En este tutorial, cambie la configuración de publicación para mejorar las predicciones.
ms.topic: tutorial
ms.date: 04/01/2020
ms.openlocfilehash: d976d06fbf20ea0e64fa01bd94658a27d680bc86
ms.sourcegitcommit: bb0afd0df5563cc53f76a642fd8fc709e366568b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/19/2020
ms.locfileid: "83588804"
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

## <a name="download-json-file-for-app"></a>Descarga de un archivo JSON para la aplicación

Descargue y guarde el [archivo JSON de la aplicación](https://raw.githubusercontent.com/Azure-Samples/cognitive-services-language-understanding/master/documentation-samples/tutorials/machine-learned-entity/pizza-tutorial-with-entities.json).

## <a name="import-json-file-for-app"></a>Importación de un archivo JSON para la aplicación

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

1. Vaya al final de la dirección URL de la barra de direcciones y reemplace _YOUR_QUERY_HERE_ por:

    `Deliver 2 of the best cheese pizzas ever!!!`

    Esta expresión no es la misma que cualquiera de las expresiones etiquetadas, por lo que es una buena prueba y debería devolver la intención `OrderPizza` con el análisis de sentimiento extraído.

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

[!INCLUDE [LUIS How to clean up resources](includes/quickstart-tutorial-cleanup-resources.md)]

## <a name="related-information"></a>Información relacionada

* [Text Analytics](../Text-Analytics/index.yml) de Cognitive Services ofrece Análisis de sentimiento. La característica está restringida a los [idiomas admitidos](luis-language-support.md#languages-supported) por Text Analytics.
* [Cómo se realiza el entrenamiento](luis-how-to-train.md)
* [Publicación](luis-how-to-publish-app.md)
* [Prueba de la aplicación el portal de LUIS](luis-interactive-test.md)


## <a name="next-steps"></a>Pasos siguientes
Este tutorial agrega el análisis de opiniones como una configuración de publicación para extraer los valores de las opiniones de la expresión en su conjunto.

> [!div class="nextstepaction"]
> [Revisión de las expresiones de punto de conexión en la aplicación de RR. HH.](luis-tutorial-review-endpoint-utterances.md)

