---
title: 'Tutorial: Entidad de lista: LUIS'
description: Obtenga datos que coincidan con una lista de elementos predefinida. Cada elemento de la lista puede tener sinónimos que también coincidan exactamente.
ms.topic: tutorial
ms.date: 03/12/2020
ms.openlocfilehash: 9530719c43260751d64d7ccf446bc7941078d6e9
ms.sourcegitcommit: 34a6fa5fc66b1cfdfbf8178ef5cdb151c97c721c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/28/2020
ms.locfileid: "82101134"
---
# <a name="tutorial-get-exact-text-matched-data-from-an-utterance-with-list-entity"></a>Tutorial: Obtención de los datos coincidentes con el texto exacto de una expresión con una entidad de lista

En este tutorial, aprenderá cómo obtener datos que coincidan exactamente con una lista de elementos predefinida.

**En este tutorial, aprenderá a:**

<!-- green checkmark -->
> [!div class="checklist"]
> * Importar una aplicación y usar la intención existente
> * Incorporación de entidad de lista
> * Entrenar, publicar y consultar aplicaciones para obtener los datos extraídos

[!INCLUDE [LUIS Free account](../../../includes/cognitive-services-luis-free-key-short.md)]

## <a name="what-is-a-list-entity"></a>¿Qué es una entidad de lista?

Una entidad de la lista es una coincidencia de texto exacta con las palabras de la expresión. Cada elemento de la lista puede incluir una lista de sinónimos. Puede usar una entidad de lista cuando desee una coincidencia exacta.

Para esta aplicación de pizzas importada, cree una entidad de lista para los diferentes tipos de base de pizza.

Una entidad de lista es una buena opción para este tipo de datos cuando:

* Los valores de los datos son un conjunto conocido.
* El conjunto no excede los [límites](luis-limits.md) máximos de LUIS para este tipo de entidad.
* El texto de la expresión es una coincidencia, sin distinción entre mayúsculas y minúsculas, con un sinónimo o el nombre canónico. LUIS no usa la lista más allá de la coincidencia. La lematización, los plurales y otras variaciones no se resuelven solo con una entidad de lista. Para administrar las variaciones, considere el uso de un [patrón](reference-pattern-syntax.md#syntax-to-mark-optional-text-in-a-template-utterance) con la sintaxis de texto opcional.

> [!CAUTION]
> Si no está seguro de si desea una entidad de lista o una entidad con aprendizaje automático con una lista de frases como descriptor, el procedimiento mejor y más flexible es usar una entidad con aprendizaje automático con una lista de frases como descriptor. Este método permite a LUIS aprender y ampliar los valores de los datos que se van a extraer.

## <a name="import-example-json-and-add-utterances"></a>Importación del archivo .json de ejemplo y adición de expresiones

1.  Descargue y guarde el [archivo JSON de la aplicación](https://raw.githubusercontent.com/Azure-Samples/cognitive-services-language-understanding/master/documentation-samples/tutorials/machine-learned-entity/pizza-tutorial-with-entities.json).

    [!INCLUDE [Import app steps](includes/import-app-steps.md)]

1. La aplicación importada tiene una intención `OrderPizza`. Seleccione esa intención y agregue algunas expresiones con los nuevos tipos de base:

    |Nuevas expresiones|
    |--|--|
    |Pida una pizza de pepperoni pequeña con base normal|
    |3 pizzas hawaianas con base fina|
    |entregue 2 pizzas de base rellena con barritas de pan|
    |una pizza de base gruesa para llevar|
    |una pizza de pepperoni de plato hondo|

## <a name="crust-list-entity"></a>Entidad de lista de bases

Ahora que la intención **OrderPizza** tiene expresiones de ejemplo con los tipos de base, LUIS debe comprender qué palabras representan los tipos de base.

Algunos ejemplos de nombre principal y sinónimos son:

|Nombre canónico|Sinónimos|
|--|--|
|Plato hondo|Hondo<br>base de plato hondo<br>gruesa<br>base gruesa|
|Normal|normal<br>original<br>normal<br>base normal<br>base original<br>base normal|
|Rellena|base rellena|
|Fina|base fina<br>muy fina<br>base muy fina|

1. Seleccione **Entities** (Entidades) en el panel izquierdo.

1. Seleccione **+ Create** (+ Crear).

1. En el cuadro de diálogo emergente de la entidad, escriba `CrustList` para el nombre de la entidad y **List** (Lista) para el tipo de entidad. Seleccione **Next** (Siguiente).

    > [!div class="mx-imgBorder"]
    > ![Captura de pantalla del cuadro de diálogo emergente de creación de una nueva entidad](media/luis-quickstart-intent-and-list-entity/create-pizza-crust-list-entity.png)

1. En la página **Create a list entity** (Crear una entidad de lista), escriba los nombres canónicos y los sinónimos de cada nombre canónico y seleccione **Create** (Crear).

    > [!div class="mx-imgBorder"]
    > ![Captura de pantalla de la adición de elementos a la entidad de lista](media/luis-quickstart-intent-and-list-entity/add-pizza-crust-items-list-entity.png)

    Cuando se agrega una entidad de lista a una aplicación de LUIS, no es necesario [etiquetar](label-entity-example-utterance.md) el texto con la entidad de lista. Se aplica a todas las expresiones de todas las intenciones.

## <a name="train-the-app-before-testing-or-publishing"></a>Entrenamiento de la aplicación antes de las pruebas o la publicación

[!INCLUDE [LUIS How to Train steps](includes/howto-train.md)]

## <a name="publish-the-app-to-query-from-the-endpoint"></a>Publicación de la aplicación en la consulta desde el punto de conexión

[!INCLUDE [LUIS How to Publish steps](includes/howto-publish.md)]

## <a name="get-intent-and-entity-prediction-from-endpoint"></a>Obtención de intención y predicción de entidad desde el punto de conexión

1. [!INCLUDE [LUIS How to get endpoint first step](includes/howto-get-endpoint.md)]

2. Vaya al final de la dirección URL y escriba la siguiente expresión:

    `Deliver 2 deep dish hawaiian pizzas and a thin pepperoni`

    El último parámetro de la cadena de consulta es `query`, la expresión **query**.


    ```json
    {
        "query": "Deliver 2 deep dish hawaiian pizzas and a thin pepperoni",
        "prediction": {
            "topIntent": "OrderPizza",
            "intents": {
                "OrderPizza": {
                    "score": 0.9957229
                },
                "None": {
                    "score": 0.016832687
                },
                "Confirm": {
                    "score": 0.0015708931
                },
                "Greeting": {
                    "score": 0.00057060417
                }
            },
            "entities": {
                "number": [
                    2
                ],
                "CrustList": [
                    [
                        "Deep dish"
                    ],
                    [
                        "Thin"
                    ]
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
                    ],
                    "CrustList": [
                        {
                            "type": "CrustList",
                            "text": "deep dish",
                            "startIndex": 10,
                            "length": 9,
                            "modelTypeId": 5,
                            "modelType": "List Entity Extractor",
                            "recognitionSources": [
                                "model"
                            ]
                        },
                        {
                            "type": "CrustList",
                            "text": "thin",
                            "startIndex": 42,
                            "length": 4,
                            "modelTypeId": 5,
                            "modelType": "List Entity Extractor",
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

    Los tipos de base se encontraron como coincidencias exactas de texto y se devuelven en la respuesta JSON. La aplicación cliente utiliza esta información para procesar el pedido.

[!INCLUDE [LUIS How to clean up resources](includes/quickstart-tutorial-cleanup-resources.md)]

## <a name="related-information"></a>Información relacionada

* Información conceptual sobre [entidades de lista](luis-concept-entity-types.md#list-entity)
* [Cómo se realiza el entrenamiento](luis-how-to-train.md)
* [Publicación](luis-how-to-publish-app.md)
* [Prueba de la aplicación el portal de LUIS](luis-interactive-test.md)
* [Concepto: entidades](luis-concept-entity-types.md)
* [Referencia JSON de la entidad de expresión regular](reference-entity-regular-expression.md?tabs=V3)
* [Cómo agregar entidades para extraer datos](luis-how-to-add-entities.md)

## <a name="next-steps"></a>Pasos siguientes
En este tutorial se agregaron expresiones de ejemplo y luego se creó una entidad de lista para extraer las coincidencias exactas de texto de las expresiones. Después del aprendizaje y de la publicación de la aplicación, una consulta al punto de conexión identificó la intención y devolvió los datos extraídos.

> [!div class="nextstepaction"]
> [Incorporación de una entidad precompilada con un rol](tutorial-entity-roles.md)

