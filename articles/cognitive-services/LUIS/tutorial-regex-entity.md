---
title: 'Tutorial: Entidad de expresión regular: LUIS'
titleSuffix: Azure Cognitive Services
description: Extraiga datos con formato de forma coherente desde una expresión con la entidad de expresión regular.
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: tutorial
ms.date: 12/17/2019
ms.author: diberry
ms.openlocfilehash: 0ca6f2a67e01e4c604c2dcc8f8eaa9ffe8bad045
ms.sourcegitcommit: 9ee0cbaf3a67f9c7442b79f5ae2e97a4dfc8227b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "75381535"
---
# <a name="tutorial-get-well-formatted-data-from-the-utterance"></a>Tutorial: Obtención de datos con formato correcto a partir de la expresión
En este tutorial, va a crear una entidad de expresión regular para extraer datos con formato de forma coherente de una expresión.

**En este tutorial, aprenderá a:**

<!-- green checkmark -->
> [!div class="checklist"]
> * Importar una aplicación
> * Agregar intención
> * Incorporación de entidades de expresiones regulares
> * Entrenar, publicar y consultar aplicaciones para obtener los datos extraídos

[!INCLUDE [LUIS Free account](../../../includes/cognitive-services-luis-free-key-short.md)]

## <a name="regular-expression-entities"></a>Entidades de expresión regular

Use la entidad de expresión regular para extraer texto con formato correcto de una expresión. Aunque la intención de la expresión siempre se determina con el aprendizaje automático, este tipo específico de entidad no se aprende de manera automática. Un buen uso de la entidad de expresión regular es cualquier texto que se puede representar de forma coherente mediante una [expresión regular](https://docs.microsoft.com/dotnet/standard/base-types/regular-expression-language-quick-reference).

`Send pizza delivery time to x123432`

En este ejemplo se usa un _código corto_ para enviar mensajes de texto. Este código corto es un código numérico de 5 o 6 dígitos, con el prefijo x, que se puede describir con la expresión regular `x\d{5,6}`.

Cuando se agrega una entidad de expresión regular a una aplicación de LUIS, no es necesario [etiquetar](label-entity-example-utterance.md) el texto con la entidad de expresión regular. Se aplica a todas las expresiones de todas las intenciones.

## <a name="import-example-json-to-begin-app"></a>Importación de un archivo .json de ejemplo para comenzar la aplicación

1.  Descargue y guarde el [archivo JSON de la aplicación](https://raw.githubusercontent.com/Azure-Samples/cognitive-services-language-understanding/master/documentation-samples/tutorials/machine-learned-entity/pizza-tutorial-with-entities.json).

[!INCLUDE [Import app steps](includes/import-app-steps.md)]

## <a name="create-intent-for-sending-confirmation-text-messages"></a>Creación de una intención para enviar mensajes de texto de confirmación

1. Seleccione **+ Create** (+ Crear) para crear una nueva intención para clasificar la intención de una expresión para enviar un texto de confirmación.

1. Escriba `ConfirmationText` en el cuadro de diálogo emergente y seleccione **Done** (Listo).

1. Agregue expresiones de ejemplo a la intención.

    |Expresiones de ejemplo|
    |--|
    |Enviar hora de entrega de la pizza a x123432|
    |Escribir mensaje de texto a x234567 con la hora|
    |x23987 con el aviso|

    Para extraer entidades con aprendizaje automático, debe proporcionar ejemplos que incluyan la entidad en una variedad de expresiones, pero con esta entidad sin aprendizaje automático, la variación no es importante. Siempre que el texto coincida con la expresión regular, se extraerá.

## <a name="use-the-regular-expression-entity-for-well-formatted-data"></a>Uso de la entidad de expresión regular para datos con formato correcto
Cree una entidad de expresión regular para que coincida con el número de texto. Esta expresión regular coincide con el texto pero omite variantes de mayúsculas y minúsculas y la referencia cultural.

1. Seleccione **Entities** (Entidades) en el panel izquierdo.

1. Seleccione **+ Create** (+ Crear) en la página Entities list (Lista de entidades).

1. En el cuadro de diálogo emergente, escriba el nombre de la nueva entidad `ConfirmationTextRegEx`, seleccione **RegEx** como tipo de entidad y, a continuación, seleccione **Next** (Siguiente).

    > [!div class="mx-imgBorder"]
    > ![Pasos para iniciar la creación de entidades para la entidad de expresión regular](./media/luis-quickstart-intents-regex-entity/pizza-create-new-entity.png)

1. En **Create a regex entity** (Crear una entidad de expresión regular), escriba `x\d{5,6}` como valor de **Regex** y, a continuación, seleccione **Create** (Crear).

    > [!div class="mx-imgBorder"]
    > ![Escribir una expresión regular para extraer datos de la expresión de ejemplo](./media/luis-quickstart-intents-regex-entity/pizza-set-regular-expression-for-new-entity.png)

1. Seleccione **Intents** (Intenciones) en el menú de la izquierda y, a continuación, la intención **ConfirmationText** para ver la expresión regular etiquetada en las expresiones.

    > [!div class="mx-imgBorder"]
    > ![Ver expresión regular etiquetada en expresiones de ejemplo](./media/luis-quickstart-intents-regex-entity/pizza-reg-ex-entity-shown-example-utterances-intent.png)

    Dado que la entidad no es una entidad con aprendizaje automático, se aplica a las expresiones y se muestra en el portal de LUIS en cuanto se crea.

## <a name="train-the-app-before-testing-or-publishing"></a>Entrenamiento de la aplicación antes de las pruebas o la publicación

[!INCLUDE [LUIS How to Train steps](includes/howto-train.md)]

## <a name="publish-the-app-to-query-from-the-endpoint"></a>Publicación de la aplicación en la consulta desde el punto de conexión

[!INCLUDE [LUIS How to Publish steps](includes/howto-publish.md)]

## <a name="get-intent-and-entity-prediction-from-endpoint"></a>Obtención de intención y predicción de entidad desde el punto de conexión

1. [!INCLUDE [LUIS How to get endpoint first step](includes/howto-get-endpoint.md)]

2. Vaya al final de la dirección URL y escriba la siguiente expresión:

    `Text my pizza delivery to x23456 x234567 x12345`

    El último parámetro de la cadena de consulta es `query`, la expresión **query**.

    ```json
    {
        "query": "Text my pizza delivery to x23456 x234567 x12345",
        "prediction": {
            "topIntent": "ConfirmationText",
            "intents": {
                "ConfirmationText": {
                    "score": 0.7061845
                },
                "ModifyOrder": {
                    "score": 0.196021989
                },
                "None": {
                    "score": 0.02342912
                },
                "Test-Pizza": {
                    "score": 0.01213586
                },
                "CancelOrder": {
                    "score": 0.0063042324
                },
                "Confirmation": {
                    "score": 0.0058615827
                },
                "Greetings": {
                    "score": 0.00398947531
                }
            },
            "entities": {
                "ConfirmationTextRegEx": [
                    "x23456",
                    "x234567",
                    "x12345"
                ],
                "$instance": {
                    "ConfirmationTextRegEx": [
                        {
                            "type": "ConfirmationTextRegEx",
                            "text": "x23456",
                            "startIndex": 26,
                            "length": 6,
                            "modelTypeId": 8,
                            "modelType": "Regex Entity Extractor",
                            "recognitionSources": [
                                "model"
                            ]
                        },
                        {
                            "type": "ConfirmationTextRegEx",
                            "text": "x234567",
                            "startIndex": 33,
                            "length": 7,
                            "modelTypeId": 8,
                            "modelType": "Regex Entity Extractor",
                            "recognitionSources": [
                                "model"
                            ]
                        },
                        {
                            "type": "ConfirmationTextRegEx",
                            "text": "x12345",
                            "startIndex": 41,
                            "length": 6,
                            "modelTypeId": 8,
                            "modelType": "Regex Entity Extractor",
                            "recognitionSources": [
                                "model"
                            ]
                        }
                    ]
                }
            }
        }
    }
    ```

    Mediante el uso de una entidad de expresión regular, LUIS extrae datos con nombre, lo que es más útil desde el punto de vista de programación para la aplicación cliente que recibe la respuesta de JSON.


[!INCLUDE [LUIS How to clean up resources](includes/quickstart-tutorial-cleanup-resources.md)]

## <a name="related-information"></a>Información relacionada

* [Concepto: entidades](luis-concept-entity-types.md)
* [Referencia JSON de la entidad de expresión regular](reference-entity-regular-expression.md?tabs=V3)
* [Cómo agregar entidades para extraer datos](luis-how-to-add-entities.md)

## <a name="next-steps"></a>Pasos siguientes
Este tutorial se creó una nueva intención, se agregaron expresiones de ejemplo y luego se creó una entidad de expresión regular para extraer los datos con formato correcto en las expresiones. Después del aprendizaje y de la publicación de la aplicación, una consulta al punto de conexión identificó la intención y devolvió los datos extraídos.

> [!div class="nextstepaction"]
> [Obtenga información acerca de la entidad de la lista](tutorial-list-entity.md)

