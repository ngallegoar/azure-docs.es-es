---
title: 'Tutorial: Predicción de intenciones: LUIS'
description: Cree una aplicación personalizada que prediga la intención de un usuario en función de la expresión (texto) siguiendo este tutorial.
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: tutorial
ms.date: 05/05/2020
ms.openlocfilehash: c4a564834166686e8280f1117269742b489fc62c
ms.sourcegitcommit: 32c521a2ef396d121e71ba682e098092ac673b30
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/25/2020
ms.locfileid: "91334759"
---
# <a name="tutorial-build-a-luis-app-to-determine-user-intentions"></a>Tutorial: Compilación de una aplicación de LUIS para determinar las intenciones del usuario

En este tutorial creará una aplicación personalizada que predice la intención de un usuario en función de la expresión (texto).

**En este tutorial, aprenderá a:**

> [!div class="checklist"]
> * Creación de una nueva aplicación
> * Creación de intenciones
> * Incorporación de expresiones de ejemplo
> * Entrenamiento de la aplicación
> * Publicación de aplicación
> * Obtención de la predicción de la intención desde un punto de conexión

## <a name="user-intentions-as-intents"></a>Intenciones del usuario como intenciones

La finalidad de la aplicación es determinar la intención del texto de conversión en lenguaje natural:

`I'd like to order a veggie pizza with a salad on the side.`

Estas intenciones se categorizan en **Intenciones**.

|Intención|Propósito|
|--|--|
|`ModifyOrder`|Determinar el pedido de pizzas del usuario.|
|`Greeting`|Comenzar la conversación con el bot.|
|`ConfirmOrder`|Confirmar el pedido de pizzas.|
|`None`|Determinar si el usuario pregunta algo que la aplicación LUIS no está diseñada para responder. Esta intención se proporciona como parte de la creación de la aplicación y no se puede eliminar. |

## <a name="create-a-new-app"></a>Creación de una nueva aplicación

[!INCLUDE [Follow these steps to create a new LUIS app](includes/create-pizza-app.md)]

## <a name="create-a-new-intent"></a>Creación de una nueva intención

Las intenciones se usan para clasificar las expresiones de usuario en función del propósito del usuario, que se determina a partir del texto del lenguaje natural.

Para clasificar una expresión, la intención necesita ejemplos de expresiones del usuario que se deben clasificar con esta intención.

1. En la sección **Build** (Compilar) de la página **Intents** (Intenciones), seleccione **+ Create** (+ Crear) para crear una intención. Escriba el nombre de la nueva intención, `OrderPizza`, y después seleccione **Listo**.

    La intención `OrderPizza` se predice cuando un usuario desea pedir una pizza.

1. Agregue varias expresiones de ejemplo a esta intención que espera que un usuario pida:

    |Expresiones de ejemplo de `OrderPizza`|
    |--|
    |`can i get a pepperoni pizza and a can of coke please`|
    |`can i get a small pizza with onions peppers and olives`|
    |`delivery for a small pepperoni pizza`|
    |`pickup a cheddar cheese pizza large with extra anchovies`|
    |`i need 2 large cheese pizzas 6 large pepperoni pizzas and 1 large supreme pizza`|
    |`Order a pizza for me`|

    > [!div class="mx-imgBorder"]
    > ![Captura de pantalla del ejemplo de adición de expresiones en el portal de LUIS en la página de intención](media/tutorial-intents-only/add-example-utterances-for-pizza-order.png)

    Al proporcionar _expresiones de ejemplo_, está enseñando a LUIS qué tipos de expresiones deben predecirse para esta intención. Son ejemplos positivos. Las expresiones de todas las demás intenciones se tratan como ejemplos negativos de esta intención.

    [!INCLUDE [Do not use too few utterances](includes/do-not-use-too-few-utterances.md)]

## <a name="create-remaining-intents"></a>Crear intenciones restantes

1. Cree la intención `Greeting` y agregue las siguientes expresiones de ejemplo. Se trata de la intención para determinar si un usuario está empezando una nueva conversación para pedir pizzas.

    |Expresiones de ejemplo de `Greeting`|
    |--|
    |`Hi`|
    |`Hello`|
    |`Hey`|
    |`Start`|
    |`Begin`|

1. Cree la intención `Confirm` y agregue las siguientes expresiones de ejemplo. Se trata de la intención para determinar si un usuario ha terminado de pedir y acepta los detalles del pedido.

    |Expresiones de ejemplo de `Confirm`|
    |--|
    |`Go ahead`|
    |`ok`|
    |`Yes`|
    |`Sure`|


## <a name="none-intent-example-utterances"></a>Expresiones de ejemplo para la intención None

[!INCLUDE [Follow these steps to add the None intent to the app](includes/add-example-utterances-none-intent.md)]

## <a name="train-the-app"></a>Entrenamiento de la aplicación

[!INCLUDE [LUIS How to Train steps](includes/howto-train.md)]

## <a name="publish-the-app"></a>Publicación de la aplicación

[!INCLUDE [LUIS How to Publish steps](includes/howto-publish.md)]

## <a name="get-intent-prediction"></a>Obtener la predicción de la intención

1. [!INCLUDE [LUIS How to get endpoint first step](includes/howto-get-endpoint.md)]

1. Vaya al final de la dirección URL en la barra de direcciones y escriba:

    `get a medium vegetarian pizza for delivery`

  
    El parámetro de cadena de consulta es `query`, se pasa la **consulta** de expresión en el URI. Esta expresión no es la misma que cualquiera de las expresiones de ejemplo. Esto debería constituir una prueba adecuada para comprobar si LUIS aprende y predice la intención de `OrderPizza` como la de puntuación superior.

    ```JSON
    {
        "query": "get a medium vegetarian pizza for delivery",
        "prediction": {
            "topIntent": "OrderPizza",
            "intents": {
                "OrderPizza": {
                    "score": 0.6488959
                },
                "None": {
                    "score": 0.139966831
                },
                "Confirm": {
                    "score": 0.00736504374
                },
                "Greeting": {
                    "score": 0.003970454
                }
            },
            "entities": {}
        }
    }
    ```

    La matriz de entidades está vacía porque esta aplicación no tiene actualmente ninguna entidad (unidad de datos dentro de la expresión que se va a extraer).

    El resultado de JSON identifica la intención que tiene la puntuación más alta como propiedad **`prediction.topIntent`** . Todas las puntuaciones están comprendidas entre 1 y 0, y la mejor está más próxima a 1.

1. Cambie el parámetro de **consulta** de la dirección URL para que tenga como destino la intención **Greeting**:

    `Howdy`

    Esto no es exactamente lo mismo que una expresión de ejemplo, por lo que es una buena prueba para ver si LUIS puede aprender lo que debe predecirse con esta intención.

    ```json
    {
        "query": "howdy",
        "prediction": {
            "topIntent": "Greeting",
            "intents": {
                "Greeting": {
                    "score": 0.446016937
                },
                "Confirm": {
                    "score": 0.2390079
                },
                "None": {
                    "score": 0.09119555
                },
                "OrderPizza": {
                    "score": 0.00109590159
                }
            },
            "entities": {}
        }
    }
    ```

    Esta predicción tiene una puntuación de confianza del 44 %. Para aumentar la puntuación de confianza, agregue entre 15 y 30 expresiones de ejemplo.

## <a name="client-application-next-steps"></a>Pasos siguientes de la aplicación cliente

En este tutorial se creó una aplicación de LUIS, se crearon intenciones, se agregaron expresiones de ejemplo a cada intención, se agregaron expresiones de ejemplo a la intención None, se entrenó, se publicó y se probó en el punto de conexión. Estos son los pasos básicos de la creación de un modelo de LUIS.

Después de que LUIS devuelva la respuesta JSON, LUIS termina con esta solicitud. LUIS no proporciona respuestas a expresiones de usuario, solo identifica el tipo de información que se solicita en lenguaje natural. La aplicación cliente permite el seguimiento de la conversación, como una instancia de Azure Bot.


[!INCLUDE [LUIS How to clean up resources](includes/quickstart-tutorial-cleanup-resources.md)]

## <a name="related-information"></a>Información relacionada

* [Tipos de entidades](luis-concept-entity-types.md)
* [Cómo se realiza el entrenamiento](luis-how-to-train.md)
* [Publicación](luis-how-to-publish-app.md)
* [Prueba de la aplicación el portal de LUIS](luis-interactive-test.md)
* [Azure Bot](https://docs.microsoft.com/azure/bot-service/?view=azure-bot-service-4.0)


## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Adición de una entidad que se puede descomponer a esta aplicación](tutorial-machine-learned-entity.md)
